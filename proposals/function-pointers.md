---
ms.openlocfilehash: 8bf3a18dc42e225e64bd3ccda2106aed89b421ed
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108388"
---
# <a name="function-pointers"></a>Funktionszeiger

## <a name="summary"></a>Zusammenfassung

Dieser Vorschlag stellt Sprachkonstrukte bereit, die IL-Opcodes verfügbar machen, auf die zurzeit nicht effizient C# zugegriffen werden kann, oder überhaupt in: `ldftn` und `calli`. Diese IL-Opcodes können bei Hochleistungs Code wichtig sein, und Entwickler benötigen eine effiziente Zugriffsmethode.

## <a name="motivation"></a>Motivation

Die Gründe und der Hintergrund für dieses Feature werden im folgenden Problem beschrieben (wie eine mögliche Implementierung des Features):

https://github.com/dotnet/csharplang/issues/191

Dies ist ein alternativer Entwurfsvorschlag für systeminterne [Compilerfunktionen](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md) .

## <a name="detailed-design"></a>Detaillierter Entwurf

### <a name="function-pointers"></a>Funktionszeiger

Die Sprache ermöglicht die Deklaration von Funktions Zeigern mit der `delegate*`-Syntax. Die vollständige Syntax wird im nächsten Abschnitt ausführlich beschrieben, aber Sie soll der Syntax ähneln, die von `Func` und `Action` Typdeklarationen verwendet wird.

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

Diese Typen werden mithilfe des Funktionszeiger Typs dargestellt, wie in ECMA-335 beschrieben. Dies bedeutet, dass für den Aufruf einer `delegate*` `calli` verwendet wird, bei dem der Aufruf eines `delegate` `callvirt` für die `Invoke`-Methode verwendet.
Obwohl der Aufruf für beide Konstrukte identisch ist, ist der Aufruf syntaktisch.

Die ECMA-335-Definition von Methoden Zeigern enthält die Aufruf Konvention als Teil der Typsignatur (Abschnitt 7,1).
Die Standard Aufruf Konvention wird `managed`. Alternative Formen können angegeben werden, indem der entsprechende-Modifizierer nach der `delegate*`-Syntax hinzugefügt wird: `managed`, `cdecl`, `stdcall`, `thiscall`oder `unmanaged`. Beispiel:

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

Konvertierungen zwischen `delegate*` Typen werden basierend auf Ihrer Signatur einschließlich der Aufruf Konvention ausgeführt.

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

Ein `delegate*` Typ ist ein Zeigertyp. Dies bedeutet, dass er über alle Funktionen und Einschränkungen eines Standard Zeiger Typs verfügt:

- Nur in einem `unsafe` Kontext gültig.
- Methoden, die einen `delegate*` Parameter oder einen Rückgabetyp enthalten, können nur von einem `unsafe` Kontext aufgerufen werden.
- Kann nicht in `object`konvertiert werden.
- Kann nicht als generisches Argument verwendet werden.
- Kann `delegate*` implizit in `void*`konvertieren.
- Kann explizit von `void*` in `delegate*`konvertieren.

Begrenzungen

- Benutzerdefinierte Attribute können nicht auf eine `delegate*` oder eines ihrer Elemente angewendet werden.
- Ein `delegate*` Parameter kann nicht als `params` gekennzeichnet werden.
- Ein `delegate*` Typ weist alle Einschränkungen eines normalen Zeiger Typs auf.

### <a name="function-pointer-syntax"></a>Funktionszeiger Syntax

Die vollständige Funktionszeiger Syntax wird durch die folgende Grammatik dargestellt:

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

Die `unmanaged`-Aufruf Konvention stellt die Standard Aufruf Konvention für nativen Code auf der aktuellen Plattform dar und wird als WINAPI codiert.
Alle `calling_convention`s sind kontextabhängige Schlüsselwörter, wenn eine `delegate*`vorangestellt ist.

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a>Funktionszeiger Konvertierungen

In einem unsicheren Kontext wird der Satz der verfügbaren impliziten Konvertierungen (implizite Konvertierungen) um die folgenden impliziten Zeiger Konvertierungen erweitert:
- [_Vorhandene Konvertierungen_](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- Von _funcptr\_Typ_ `F0` zu einem anderen _funkptr\_Typ_ `F1`, wenn alle folgenden Punkte zutreffen:
    - `F0` und `F1` haben die gleiche Anzahl von Parametern, und jeder Parameter `D0n` in `F0` hat dieselben `ref`-, `out`-oder `in` Modifizierer wie der entsprechende Parameter `D1n` `F1`.
    - Für jeden value-Parameter (ein Parameter ohne `ref`, `out`oder `in` Modifizierer), ist eine Identitäts Konvertierung, eine implizite Verweis Konvertierung oder eine implizite Zeiger Konvertierung aus dem Parametertyp in `F0` in den entsprechenden Parametertyp in `F1`vorhanden.
    - Für jeden `ref`-, `out`-oder `in`-Parameter ist der Parametertyp in `F0` mit dem entsprechenden Parametertyp in `F1`identisch.
    - Wenn der Rückgabetyp nach Wert (keine `ref` oder `ref readonly`) ist, ist eine Identität, ein impliziter Verweis oder eine implizite Zeiger Konvertierung vom Rückgabetyp `F1` zum Rückgabetyp `F0`vorhanden.
    - Wenn der Rückgabetyp als Verweis (`ref` oder `ref readonly`) erfolgt, sind der Rückgabetyp und die `ref` modifizierermodifiziererer `F1` identisch mit dem Rückgabetyp und `ref` modifizierermodifizierer `F0`
    - Die Aufruf Konvention von `F0` ist mit der Aufruf Konvention `F1`identisch.

### <a name="allow-address-of-to-target-methods"></a>Address-of-to-target-Methoden zulassen

Methoden Gruppen werden nun als Argumente für einen Address-of-Ausdruck zugelassen. Der Typ eines solchen Ausdrucks ist eine `delegate*`, die über die entsprechende Signatur der Ziel Methode und eine verwaltete Aufruf Konvention verfügt:

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

In einem unsicheren Kontext ist eine Methode `M` mit einem Funktions Zeigertyp kompatibel `F` wenn Folgendes zutrifft:
- `M` und `F` haben die gleiche Anzahl von Parametern, und jeder Parameter in `D` hat dieselben `ref`, `out`oder `in` Modifizierer wie der entsprechende Parameter in `F`.
- Für jeden value-Parameter (ein Parameter ohne `ref`, `out`oder `in` Modifizierer), ist eine Identitäts Konvertierung, eine implizite Verweis Konvertierung oder eine implizite Zeiger Konvertierung aus dem Parametertyp in `M` in den entsprechenden Parametertyp in `F`vorhanden.
- Für jeden `ref`-, `out`-oder `in`-Parameter ist der Parametertyp in `M` mit dem entsprechenden Parametertyp in `F`identisch.
- Wenn der Rückgabetyp nach Wert (keine `ref` oder `ref readonly`) ist, ist eine Identität, ein impliziter Verweis oder eine implizite Zeiger Konvertierung vom Rückgabetyp `F` zum Rückgabetyp `M`vorhanden.
- Wenn der Rückgabetyp als Verweis (`ref` oder `ref readonly`) erfolgt, sind der Rückgabetyp und die `ref` modifizierermodifiziererer `F` identisch mit dem Rückgabetyp und `ref` modifizierermodifizierer `M`
- Die Aufruf Konvention von `M` ist mit der Aufruf Konvention `F`identisch.
- `M` ist eine statische Methode.

In einem unsicheren Kontext gibt es eine implizite Konvertierung von einem address-of-Ausdruck, dessen Ziel eine Methoden Gruppe ist `E` zu einem kompatiblen Funktions Zeigertyp `F`, wenn `E` mindestens eine Methode enthält, die in der normalen Form auf eine Argumentliste anwendbar ist, die durch die Verwendung der Parametertypen und Modifizierer von `F`erstellt wurde, wie im folgenden beschrieben.
- Es wird eine einzelne Methode `M` ausgewählt, die einem Methodenaufruf des Formulars entspricht, `E(A)` mit den folgenden Änderungen:
   - Bei der Argumentliste `A` handelt es sich um eine Liste von Ausdrücken, die jeweils als Variable und Typ und Modifizierer (`ref`, `out`oder `in`) des entsprechenden _formalen\_-Parameters\_Liste_ der `D`klassifiziert sind.
   - Bei den Kandidaten Methoden handelt es sich nur um die Methoden, die in ihrer normalen Form anwendbar sind, nicht um die Methoden, die im erweiterten Formular anwendbar sind.
   - Bei den Kandidaten Methoden handelt es sich nur um statische Methoden.
- Wenn der Algorithmus von Methoden aufrufen einen Fehler erzeugt, tritt ein Kompilierzeitfehler auf. Andernfalls erzeugt der Algorithmus eine einzige beste Methode `M` die gleiche Anzahl von Parametern wie `F` haben, und die Konvertierung wird als vorhanden betrachtet.
- Die ausgewählte Methode `M` muss (wie oben definiert) mit dem Funktions Zeigertyp `F`kompatibel sein. Andernfalls tritt ein Kompilierungsfehler auf.
- Das Ergebnis der Konvertierung ist ein Funktionszeiger vom Typ `F`.

Eine implizite Konvertierung ist von einem address-of-Ausdruck vorhanden, dessen Ziel eine Methoden Gruppe `E` ist, `void*`, wenn in `E`nur eine statische Methode `M` ist.
Wenn eine statische Methode vorhanden ist, wird die einzige beste Methode aus `E` `M`.
Andernfalls tritt ein Kompilierungsfehler auf.

Dies bedeutet, dass Entwickler von Regeln zur Überladungs Auflösung abhängig sein können, damit Sie in Verbindung mit dem address-of-Operator funktionieren:

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

Der Address-of-Operator wird mithilfe der `ldftn` Anweisung implementiert.

Einschränkungen dieses Features:

- Gilt nur für Methoden, die als `static`gekennzeichnet sind.
- Nicht`static` lokale Funktionen können nicht in `&`verwendet werden. Die Implementierungsdetails dieser Methoden werden absichtlich nicht in der Sprache angegeben. Dies umfasst auch, ob es sich um statische oder Instanz handelt oder genau, mit welcher Signatur Sie ausgegeben werden.

### <a name="better-function-member"></a>Besseres Funktionsmember

Die bessere Funktionsmember-Spezifikation wird so geändert, dass Sie die folgende Zeile enthält:

> Eine `delegate*` ist spezifischer als `void*`

Dies bedeutet, dass es möglich ist, `void*` und eine `delegate*` zu überlasten und den Address-of-Operator weiterhin vernünftig zu verwenden.

## <a name="open-issues"></a>Offene Probleme

### <a name="nativecallableattribute"></a>Nativecallableattribute

Dabei handelt es sich um ein Attribut, das von der CLR verwendet wird, um das verwaltete zu systemeigene Prolog zu vermeiden, wenn Von diesem Attribut gekennzeichnete Methoden können nur aus nativem Code aufgerufen werden, nicht verwaltet (Methoden nicht aufrufen, Delegaten erstellen usw.). Das-Attribut ist für mscorlib nicht spezifisch. die Laufzeit behandelt alle Attribute mit diesem Namen mit derselben Semantik.

Es ist möglich, dass die Laufzeit und die Sprache zusammenarbeiten, um dies vollständig zu unterstützen. Die Sprache kann die Address-of-`static` Member mit einem `NativeCallable`-Attribut als `delegate*` mit der angegebenen Aufruf Konvention behandeln.

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

Außerdem sollte die Sprache wahrscheinlich auch folgende Aktionen ausführen:

- Kennzeichnen Sie alle verwalteten Aufrufe einer Methode, die mit `NativeCallable` gekennzeichnet ist, als Fehler. Da die Funktion nicht aus verwaltetem Code aufgerufen werden kann, sollte der Compiler verhindern, dass Entwickler einen solchen Aufruf durchgeführt haben.
- Verhindern, dass Methoden Gruppen Konvertierungen `delegate` werden, wenn die-Methode mit `NativeCallable`gekennzeichnet ist.

Dies ist für die Unterstützung von `NativeCallable` jedoch nicht erforderlich. Der Compiler kann das `NativeCallable`-Attribut unterstützen, wie es die vorhandene Syntax verwendet. Das Programm muss lediglich in `void*` umgewandelt werden, bevor es in die richtige `delegate*` Signatur umgewandelt wird. Das wäre nicht schlechter als die Unterstützung heute.

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a>Erweiterbarer Satz nicht verwalteter Aufruf Konventionen

Der Satz der nicht verwalteten Aufruf Konventionen, die von den aktuellen ECMA-335-Codierungen unterstützt werden, ist veraltet. Wir haben Anforderungen zum Hinzufügen von Unterstützung für mehr nicht verwaltete Aufruf Konventionen gesehen, z. b.:

- [vectorcallcenter](https://docs.microsoft.com/cpp/cpp/vectorcall) - https://github.com/dotnet/coreclr/issues/12120
- Stdcallmit expliziten diesem https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750

Das Design dieses Features sollte die Erweiterung der nicht verwalteten Aufruf Konventionen nach Bedarf in Zukunft ermöglichen. Die Probleme umfassen den begrenzten Speicherplatz für das Codieren von Aufruf Konventionen (12 von 16 Werten werden in `IMAGE_CEE_CS_CALLCONV_MASK`) und die Anzahl von stellen, die berührt werden müssen, um eine neue Aufruf Konvention hinzuzufügen. Eine mögliche Lösung besteht darin, eine neue Codierung einzuführen, die die Aufruf Konvention mithilfe [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) -Enum repräsentiert.

https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h enthält die Liste der Aufruf Konventionen, die von llvm unterstützt werden. Obwohl es unwahrscheinlich ist, dass .net immer alle unterstützen muss, wird veranschaulicht, dass der Raum der Aufruf Konventionen sehr umfangreich ist.

## <a name="considerations"></a>Überlegungen

### <a name="allow-instance-methods"></a>Instanzmethoden zulassen

Der Vorschlag könnte so erweitert werden, dass Instanzmethoden unterstützt werden, indem die `EXPLICITTHIS` CLI-Aufruf Konvention C# (mit dem Namen `instance` im Code) genutzt wird. Diese Form von CLI-Funktions Zeigern legt den `this`-Parameter als expliziten ersten Parameter der Funktionszeiger Syntax ab.

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

Das ist zwar vernünftig, aber fügt dem Vorschlag eine gewisse Komplikation hinzu. Insbesondere weil Funktionszeiger, die sich durch die Aufruf Konvention unterscheiden, `instance` und `managed` nicht kompatibel sind, auch wenn beide Fälle verwendet werden, um C# verwaltete Methoden mit derselben Signatur aufzurufen. In jedem Fall ist es sinnvoll, eine einfache Problem Umgehung zu finden: Verwenden Sie eine `static` lokale Funktion.

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a>Unsichere Deklaration nicht erforderlich

Anstatt `unsafe` bei jeder Verwendung eines `delegate*`zu benötigen, benötigen Sie nur an dem Punkt, an dem eine Methoden Gruppe in einen `delegate*`konvertiert wird. Hier kommen die wichtigsten Sicherheitsprobleme ins Spiel (wenn Sie wissen, dass die enthaltende Assembly nicht entladen werden kann, während der Wert aktiv ist). Das verlangen von `unsafe` an anderen Speicherorten kann als übertrieben angesehen werden.

Auf diese Weise wurde der Entwurf ursprünglich beabsichtigt. Die sich ergebenden Sprachregeln waren jedoch sehr umständlich. Es ist nicht möglich, die Tatsache auszublenden, dass es sich um einen Zeiger Wert handelt, und auch ohne das Schlüsselwort "`unsafe`". Beispielsweise kann die Konvertierung in `object` nicht zulässig sein, Sie kann nicht Mitglied einer `class`sein usw... Der C# Entwurf besteht darin, `unsafe` für alle Zeiger Verwendungen anzufordern, weshalb dieses Design darauf folgt.

Entwickler können weiterhin einen _sicheren_ Wrapper zusätzlich zu `delegate*` Werten auf die gleiche Weise wie für normale Zeiger Typen darstellen. Berücksichtigen Sie dabei Folgendes:

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a>Verwenden von Delegaten

Verwenden Sie anstelle eines neuen Syntax Elements `delegate*`einfach vorhandene `delegate` Typen mit einem `*`, das auf den-Typ folgt:

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

Um die Aufruf Konvention zu verarbeiten, können Sie die `delegate` Typen mit einem Attribut versehen, das einen `CallingConvention` Wert angibt. Das Fehlen eines Attributs würde die verwaltete Aufruf Konvention bedeuten.

Das Codieren in Il ist problematisch. Der zugrunde liegende Wert muss als Zeiger dargestellt werden, aber er muss auch Folgendes aufweisen:

1. Sie haben einen eindeutigen Typ, der über Ladungen mit unterschiedlichen Funktionszeiger Typen zulässt.
1. Für ohi-Zwecke über assemblygrenzen hinweg gleichwertig sein.

Der letzte Punkt ist besonders problematisch. Dies bedeutet, dass jede Assembly, die `Func<int>*` verwendet, einen entsprechenden Typ in Metadaten codieren muss, obwohl `Func<int>*` in einer Assembly definiert ist, die jedoch nicht von gesteuert wird.
Außerdem muss jeder andere Typ, der mit dem Namen `System.Func<T>` in einer Assembly definiert ist, die nicht mscorlib ist, sich von der in mscorlib definierten Version unterscheiden.

Eine Option, die untersucht wurde, hat einen derartigen Zeiger als `mod_req(Func<int>) void*`ausgegeben. Dies funktioniert jedoch nicht, da eine `mod_req` nicht an eine `TypeSpec` gebunden werden kann und daher keine generischen Instanziierungen als Ziel haben kann.

### <a name="named-function-pointers"></a>Benannte Funktionszeiger

Die Funktionszeiger Syntax kann mühsam sein, insbesondere in komplexen Fällen wie z. b. für die Zeiger auf die Zeichen. Anstatt den Entwicklern die Möglichkeit zu geben, die Signatur jedes Mal einzugeben, wenn die Sprache benannte Deklarationen von Funktions Zeigern zulässt, wie es bei `delegate`der Fall ist.

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

Ein Teil des Problems hier ist, dass der zugrunde liegende CLI-primitiv keine Namen hat, C# daher wäre dies eine rein Erfindung und erfordert ein wenig zu aktivierende metadatenarbeit. Das ist zwar möglich, ist aber ein bedeutender Bezug zur Arbeit. Es ist im C# wesentlichen erforderlich, dass ein Companion für die Typdef-Tabelle ausschließlich für diese Namen vorhanden ist.

Auch wenn die Argumente für benannte Funktionszeiger untersucht wurden, haben wir festgestellt, dass Sie auch auf eine Reihe anderer Szenarios gleichermaßen gut angewendet werden können. Beispielsweise wäre es genauso praktisch, benannte Tupel zu deklarieren, um die Notwendigkeit zu verringern, die vollständige Signatur in allen Fällen einzugeben.

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

Nach der Erörterung haben wir uns entschieden, die benannte Deklaration von `delegate*` Typen nicht zuzulassen. Wenn Sie feststellen, dass es aufgrund des Feedbacks der Kunden eine beträchtliche Notwendigkeit gibt, werden wir eine namens Lösung untersuchen, die für Funktionszeiger, Tupel, Generika usw. funktioniert. Dies ist in der Regel ähnlich wie die vollständige `typedef` Unterstützung in der Sprache.

## <a name="future-considerations"></a>Überlegungen für die Zukunft

### <a name="static-local-functions"></a>statische lokale Funktionen

Dies bezieht sich auf [den Vorschlag](https://github.com/dotnet/csharplang/issues/1565) , den `static` Modifizierer für lokale Funktionen zuzulassen. Eine solche Funktion wird garantiert als `static` und mit der exakten Signatur ausgegeben, die im Quellcode angegeben ist. Eine solche Funktion sollte ein gültiges Argument für `&` sein, da Sie keines der Probleme aufweist, die lokale Funktionen heute aufweisen.

### <a name="static-delegates"></a>statische Delegaten

Dies bezieht sich auf [den Vorschlag](https://github.com/dotnet/csharplang/issues/302) , um die Deklaration von `delegate` Typen zu ermöglichen, die nur auf `static` Member verweisen können. Der Vorteil, dass solche `delegate` Instanzen in Leistungs sensiblen Szenarios kostenfrei und besser sind.

Wenn die Funktionszeiger Funktion implementiert ist, wird der `static delegate` Vorschlag wahrscheinlich geschlossen. Der vorgeschlagene Vorteil dieses Features ist die Art der Zuordnung. Es wurden jedoch letzte Untersuchungen festgestellt, die aufgrund der Entladung der Assembly nicht möglich sind. Es muss ein sicheres Handle von der `static delegate` zu der Methode vorhanden sein, auf die Sie verweist, damit die Assembly nicht aus ihr entladen wird.

Um alle `static delegate`-Instanz beizubehalten, wäre es erforderlich, ein neues Handle zuzuweisen, das den Zielen des Angebots entgegensteht. Es gab einige Entwürfe, bei denen die Zuordnung zu einer einzelnen Zuordnung pro aufrufssite amortisiert werden konnte, aber das war ein wenig komplexer und sah nicht den Kompromiss aus.

Dies bedeutet, dass Entwickler sich grundsätzlich zwischen den folgenden Kompromisse entscheiden müssen:

1. Sicherheit beim Entladen der Assembly: Dies erfordert Zuordnungen, sodass `delegate` bereits eine ausreichende Option ist.
1. Keine Sicherheit beim Entladen der Assembly: Verwenden Sie einen `delegate*`. Dies kann in einem `struct` umschließt werden, um die Verwendung außerhalb eines `unsafe` Kontexts im restlichen Code zuzulassen.
