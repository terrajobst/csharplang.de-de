---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "79483961"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a>Erzwingung der Sicherheit für Verweis ähnliche Typen in der Kompilierzeit

## <a name="introduction"></a>Einführung

Der Hauptgrund für die zusätzlichen Sicherheitsregeln beim Umgang mit Typen wie `Span<T>` und `ReadOnlySpan<T>` besteht darin, dass solche Typen auf den Ausführungs Stapel beschränkt werden müssen.
 
Es gibt zwei Gründe, warum `Span<T>` und ähnliche Typen nur Stapel-Typen sein müssen.

1. `Span<T>` ist semantisch eine Struktur, die einen Verweis und einen Bereichs `(ref T data, int length)`enthält. Unabhängig von der eigentlichen Implementierung wäre das Schreiben in eine solche Struktur nicht atomarisch. Das gleichzeitige "zerreißen" einer solchen Struktur würde dazu führen, dass `length` nicht mit der `data`übereinstimmt, was zu Zugriffs enden Zugriffen und typsicherheitsverstößen führt, was letztendlich zu einer GC-Heap Beschädigung im scheinbar "sicheren" Code führen könnte.
2. Einige Implementierungen von `Span<T>` in einem ihrer Felder buchstäblich einen verwalteten Zeiger enthalten. Verwaltete Zeiger werden nicht als Felder von Heap Objekten unterstützt, und Code, der einen verwalteten Zeiger auf den GC-Heap speichert, stürzt normalerweise bei der JIT-Zeit ab.

Alle oben genannten Probleme würden verringert werden, wenn Instanzen von `Span<T>` nur auf dem Ausführungs Stapel vorhanden sind. 

Aufgrund der Komposition entsteht ein zusätzliches Problem. Im allgemeinen wäre es wünschenswert, komplexere Datentypen zu erstellen, die `Span<T>`-und `ReadOnlySpan<T>`-Instanzen einbetten würden. Solche zusammengesetzten Typen müssten Strukturen sein und alle Risiken und Anforderungen der `Span<T>`teilen. Daher sollten die hier beschriebenen Sicherheitsregeln für den gesamten Bereich von **_ref-ähnlichen Typen_** angezeigt werden.

Die [Spezifikation für die Entwurfs Sprache](#draft-language-specification) soll sicherstellen, dass die Werte eines Verweis ähnlichen Typs nur im Stapel vorkommen.

## <a name="generalized-ref-like-types-in-source-code"></a>Verallgemeinerte `ref-like` Typen im Quellcode

`ref-like` Strukturen werden mithilfe `ref` Modifizierers explizit im Quellcode gekennzeichnet:

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

Wenn eine Struktur als ref-like festgelegt wird, können in der Struktur Verweis ähnliche Instanzfelder verwendet werden, und es werden auch alle Anforderungen von Verweis ähnlichen Typen auf die Struktur angewendet. 

## <a name="metadata-representation-or-ref-like-structs"></a>Metadatendarstellung oder ref-like-Strukturen

Ref-like-Strukturen werden mit dem **System. Runtime. CompilerServices. isreflikeattribute** -Attribut markiert.

Das-Attribut wird zu allgemeinen Basis Bibliotheken hinzugefügt, z. b. `mscorlib`. Wenn das-Attribut nicht verfügbar ist, generiert der Compiler eine interne, ähnlich wie andere eingebettete Attribute, wie z. b. `IsReadOnlyAttribute`.

Es wird ein zusätzliches Measure verwendet, um die Verwendung von Verweis ähnlichen Strukturen in Compilern zu verhindern, die mit den Sicherheitsregeln nicht vertraut sind C# (Dies schließt Compiler ein, die vor dem implementiert sind, in dem diese Funktion implementiert ist). 

Wenn keine anderen guten Alternativen vorhanden sind, die in alten Compilern ohne Wartung funktionieren, wird einem `Obsolete` Attribut mit einer bekannten Zeichenfolge alle Ref-like-Strukturen hinzugefügt. Compiler, die wissen, wie ref-like-Typen verwendet werden, ignorieren diese spezielle Form von `Obsolete`.

Eine typische Metadatendarstellung:

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

Hinweis: Es ist nicht das Ziel, es so zu machen, dass die Verwendung von Verweis ähnlichen Typen für alte Compiler 100% fehlschlägt. Das ist schwer zu erreichen und ist nicht unbedingt erforderlich. Beispielsweise ist es immer möglich, die `Obsolete` mithilfe von dynamischem Code zu umgehen oder beispielsweise ein Array von Verweis ähnlichen Typen durch Reflektion zu erstellen.

Insbesondere, wenn der Benutzer eine `Obsolete` oder ein `Deprecated` Attribut in einem ref-like-Typ platzieren möchte, haben wir keine andere Wahl als die nicht vordefinierte, da `Obsolete` Attribut nicht mehrmals angewendet werden kann.  

## <a name="examples"></a>Beispiele:

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a>Entwurfs Sprachen Spezifikation

Im folgenden wird ein Satz von Sicherheitsregeln für Verweis ähnliche Typen (`ref struct`s) beschrieben, um sicherzustellen, dass die Werte dieser Typen nur im Stapel vorkommen. Ein anderer, einfacherer Satz von Sicherheitsregeln wäre möglich, wenn die lokalen Variablen nicht als Verweis übermittelt werden können. Diese Spezifikation ermöglicht außerdem die sichere Neuzuweisung von lokalen Ref-Variablen.

### <a name="overview"></a>Übersicht

Wir ordnen jedem Ausdruck zum Zeitpunkt der Kompilierung das Konzept des Bereichs zu, mit dem dieser Ausdruck als Escapezeichen versehen werden darf. Ebenso behalten wir für jeden lvalue ein Konzept, mit dem ein Verweis auf das Escapezeichen "Ref-Safe-to-Escape" versehen werden kann. Bei einem bestimmten lvalue-Ausdruck können sich diese unterscheiden.

Diese entsprechen der "sicheren Rückgabe" des Features "Ref Locals", aber Sie ist präziser. Wenn der "Safe-to-return"-Ausdruck eines Ausdrucks nur dann aufgezeichnet wird, ob er die einschließende Methode als Ganzes mit einem Escapezeichen versehen soll (oder nicht), werden die Datensätze mit sicherer Escapezeichen aufgezeichnet. Der grundlegende Sicherheitsmechanismus wird wie folgt erzwungen. Bei einer Zuweisung von einem Ausdruck E1 mit einem abgesicherten Bereich S1 zu einem (lvalue)-Ausdruck E2 mit einem Sicherheits-zu-escapebereich S2 ist ein Fehler, wenn S2 ein größerer Bereich als S1 ist. Bei der Erstellung befinden sich die beiden Bereiche S1 und S2 in einer Schachtelungs Beziehung, da ein gültiger Ausdruck immer sicher von einem Bereich, der den Ausdruck einschließt, zurückgegeben werden kann.

Die Zeit reicht für die Analyse aus, um nur zwei Bereiche zu unterstützen, die sich außerhalb der-Methode befinden, sowie den Bereich der obersten Ebene der-Methode. Dies liegt daran, dass Ref-like-Werte mit inneren Bereichen nicht erstellt werden können und lokale Ref-Variablen keine erneute Zuweisung unterstützen. Die Regeln können jedoch mehr als zwei Bereichs Ebenen unterstützen.

Die genauen Regeln zum Berechnen des Status von " *sicher an Rückgabe* " eines Ausdrucks und der Regeln für die Rechtmäßigkeit von Ausdrücken folgen.

### <a name="ref-safe-to-escape"></a>Ref-Safe-to-Escape

Das *ref-Safe-to-Escape* -Zeichen ist ein Bereich, der einen lvalue-Ausdruck einschließt, in den es sicher ist, dass ein Verweis auf den lvalue-Wert mit Escapezeichen versehen wird. Wenn dieser Bereich die gesamte Methode ist, sagen wir, dass ein Verweis auf den lvalue-Wert sicher von der-Methode *zurückgegeben* werden kann.

### <a name="safe-to-escape"></a>sichere Escapezeichen

Der *Safe-to-Escape* -Vorgang ist ein Bereich, der einen Ausdruck einschließt, in den der Wert für den Escapezeichen sicher ist. Wenn der Gültigkeitsbereich die gesamte Methode ist, sagen wir, dass der Wert sicher von der-Methode *zurückgegeben* werden kann.

Ein Ausdruck, dessen Typ kein `ref struct` Typ ist, kann von der gesamten *einschließenden* Methode abgehandelt werden. Andernfalls verweisen wir auf die unten aufgeführten Regeln.

#### <a name="parameters"></a>Parameter

Ein Lvalue, der einen formalen Parameter festlegt *, ist Ref-Safe-to-Escape* (als Verweis) wie folgt:
- Wenn es sich bei dem Parameter um einen `ref`-, `out`-oder `in`-Parameter handelt, ist der Verweis von der gesamten Methode (z. b. durch eine `return ref`-Anweisung) *ref-Safe-* Escapezeichen. sonst
- Wenn es sich bei dem Parameter um den `this`-Parameter eines Strukturtyps handelt, ist dieser Verweis *sicher* in den Bereich der obersten Ebene der Methode (aber nicht in der gesamten Methode selbst). [Beispiel](#struct-this-escape)
- Andernfalls handelt es sich bei dem Parameter um einen value-Parameter, der *ref-Safe-to-Escape-* Vorgang mit dem Bereich der obersten Ebene der Methode (aber nicht von der Methode selbst) ist.

Ein Ausdruck, bei dem es sich um einen Rvalue-Wert handelt, der angibt, dass ein formaler Parameter verwendet wird, ist der *sichere* Escapezeichen (durch Wert) der gesamten Methode (z. b. durch eine `return`-Anweisung). Dies gilt auch für den `this`-Parameter.

#### <a name="locals"></a>Lokal

Ein Lvalue, der eine lokale Variable festlegt, ist wie folgt *ref-Safe-to-Escape* (als Verweis):
- Wenn die Variable eine `ref` Variable ist, wird der *ref-Safe-to-Escape* -Wert von der *ref-Safe-to-Escape* -Aktion des Initialisierungs Ausdrucks übernommen. sonst
- Die Variable ist ein *ref-Safe-to-Escape* -Zeichenbereich, in dem Sie deklariert wurde.

Ein Ausdruck, bei dem es sich um einen Rvalue-Wert handelt, der die Verwendung einer lokalen Variablen angibt, kann wie folgt als *sichere* Escapezeichen (durch Wert) verwendet werden:
- Die oben genannte allgemeine Regel, bei der es sich jedoch nicht um einen `ref struct` Typ handelt, ist die *sichere Rückgabe* von der gesamten einschließenden Methode.
- Wenn die Variable eine Iterations Variable einer `foreach` Schleife ist, ist der *Safe-to-Escape* -Bereich der Variablen identisch mit dem *sicheren* Escapezeichen des Ausdrucks der `foreach` Schleife.
- Eine lokale `ref struct` Typs, die zum Zeitpunkt der Deklaration nicht initialisiert wurde, kann von der gesamten einschließenden Methode *sicher zurückgegeben* werden.
- Andernfalls ist der Typ der Variable ein `ref struct` Typ, und die Deklaration der Variablen erfordert einen Initialisierer. Der *Safe-to-Escape* -Bereich der Variablen ist mit dem " *Safe-to-Escape" des Initialisierers* identisch.

#### <a name="field-reference"></a>Feldverweis

Ein Lvalue, der einen Verweis auf ein Feld (`e.F`) festlegt, ist wie folgt *ref-Safe-to-Escape* (als Verweis):
- Wenn `e` einen Verweistyp hat, ist es *ref-Safe-to-Escape* von der gesamten Methode. sonst
- Wenn `e` von einem Werttyp ist, wird das *ref-Safe-to-* Escape-Zeichen von der *ref-Safe-to-Escape* -`e`verwendet.

Ein rvalue-Wert, der einen Verweis auf ein Feld (`e.F`) festlegt, verfügt über einen Bereich mit *sicherer* Escapezeichen, der mit dem *sicheren* Escapezeichen des `e`identisch ist.

#### <a name="operators-including-"></a>Operatoren einschließlich `?:`

Die Anwendung eines benutzerdefinierten Operators wird als Methodenaufruf behandelt.

Bei einem Operator, der einen Rvalue ergibt, wie z. b. `e1 + e2` oder `c ? e1 : e2`, ist das " *Safe* "-Escapezeichen für das Ergebnis der engste Bereich zwischen dem *Safe-to-Escape-* Operator der Operanden des Operators.  Folglich ist für einen unären Operator, der einen Rvalue ergibt, wie z. b. `+e`, *der Safe* *-to-* Escapezeichen des-Operanden.

Bei einem Operator, der einen lvalue ergibt, z. b. `c ? ref e1 : ref e2`
- der *ref-Safe-to-Escape* -Wert des Ergebnisses ist der engste Bereich zwischen der *ref-Safe-to-Escape-* Operation der Operanden des Operators.
- der *Safe-to-Escape-* Operator muss zustimmen, und das ist der *sichere* Escapezeichen des resultierenden lvalue.

#### <a name="method-invocation"></a>Methodenaufruf

Ein Lvalue-Wert, der sich aus einem Methodenaufruf `e1.M(e2, ...)`, der Ref zurückgibt, ist ein *ref-Safe-* Escapezeichen für den kleinsten der folgenden Bereiche:
- Die gesamte einschließende Methode
- der *ref-Safe-to-Escape-* Ausdruck aller `ref`-und `out` Argument Ausdrücke (ausgenommen des Empfängers)
- Für jeden `in` Parameter der-Methode, wenn es einen entsprechenden Ausdruck mit einem lvalue-Wert gibt, der *ref-Safe-to-Escape*-Wert ist, andernfalls der nächste einschließende Bereich.
- der *Safe-to-Escape-* Ausdruck aller Argument Ausdrücke (einschließlich des Empfängers)

> Hinweis: das letzte Aufzählungs Zeichen ist erforderlich, um Code wie
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> oder
> ```csharp
> return ref M(sp, 0);
> ```

Ein rvalue, der sich aus einem Methodenaufruf `e1.M(e2, ...)` ergibt *,* kann aus den kleinsten der folgenden Bereiche abgegrenzt werden:
- Die gesamte einschließende Methode
- der *Safe-to-Escape-* Ausdruck aller Argument Ausdrücke (einschließlich des Empfängers)

#### <a name="an-rvalue"></a>Ein rvalue
Ein rvalue-Wert ist vom nächsten *einschließenden Bereich Ref-Safe-to-Escape* . Dies tritt beispielsweise bei einem Aufruf auf, z. b. `M(ref d.Length)`, bei dem `d` vom Typ `dynamic`ist. Sie ist auch mit der Behandlung von Argumenten konsistent, die `in` Parametern entsprechen. *

#### <a name="property-invocations"></a>Eigenschafts Aufrufe

Ein Eigenschafts Aufruf (entweder `get` oder `set`), der mit den oben genannten Regeln als Methodenaufruf der zugrunde liegenden Methode behandelt wurde.

#### `stackalloc`

Bei einem stackzuweisung-Ausdruck handelt es sich um einen Rvalue-Wert, der *sicher* mit dem Bereich der obersten Ebene der Methode (aber nicht aus der gesamten Methode selbst) entfernt werden kann.

#### <a name="constructor-invocations"></a>Konstruktoraufrufe

Ein `new` Ausdruck, der einen Konstruktor aufruft, befolgt dieselben Regeln wie ein Methodenaufruf, der als Rückgabe des erstellten Typs angesehen wird.

Außerdem ist " *Safe-to-Escape* " nicht breiter als das kleinste der " *Safe* "-Escapezeichen aller Argumente/Operanden der objektinitialisiererausdrücke, rekursiv, wenn der Initialisierer vorhanden ist. 

#### <a name="span-constructor"></a>Span-Konstruktor
Die Sprache basiert `Span<T>` keinen Konstruktor der folgenden Form:

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

Ein solcher Konstruktor stellt `Span<T>` dar, die als Felder verwendet werden, die nicht von einem `ref` Feld unterschieden werden können. Die in diesem Dokument beschriebenen Sicherheitsregeln hängen von `ref` Felder ab, die in C#oder .net nicht als gültiges Konstrukt enthalten sind.

#### <a name="default-expressions"></a>`default`-Ausdrücke

Ein `default` Ausdruck kann *von der* gesamten einschließenden Methode abgehandelt werden.

## <a name="language-constraints"></a>Spracheinschränkungen

Wir möchten sicherstellen, dass keine `ref` lokale Variable und keine Variable `ref struct` Typs auf den Stapel Speicher oder die nicht mehr aktiven Variablen verweist. Daher haben wir die folgenden Spracheinschränkungen:

- Weder ein ref-Parameter noch ein lokaler ref-Parameter oder ein-Parameter oder ein lokaler eines `ref struct` Typs können in eine Lambda-oder lokale Funktion angehoben werden.

- Weder ein ref-Parameter noch ein Parameter eines `ref struct` Typs kann ein Argument für eine Iteratormethode oder eine `async` Methode sein.

- Weder eine lokale ref-Anweisung noch eine lokale eines `ref struct` Typs sind im Gültigkeitsbereich zum Zeitpunkt einer `yield return`-Anweisung oder eines `await`-Ausdrucks.

- Ein `ref struct` Typ darf nicht als Typargument oder als Elementtyp in einem tupeltyp verwendet werden.

- Ein `ref struct` Typ ist möglicherweise nicht der deklarierte Typ eines Felds, mit dem Unterschied, dass es sich um den deklarierten Typ eines Instanzfelds eines anderen `ref struct`handelt.

- Ein `ref struct` Typ darf nicht der Elementtyp eines Arrays sein.

- Der Wert eines `ref struct` Typs darf nicht gekapselt werden:
  - Es gibt keine Konvertierung von einem `ref struct`-Typ in den-Typ `object` oder den-Typ `System.ValueType`.
  - Ein `ref struct` Typ kann nicht für die Implementierung einer Schnittstelle deklariert werden.
  - Keine Instanzmethode, die in `object` oder `System.ValueType` deklariert, aber nicht in einem `ref struct` Typ überschrieben wurde, kann mit einem Empfänger dieses `ref struct` Typs aufgerufen werden.
  - Es kann keine Instanzmethode eines `ref struct` Typs durch die Methoden Konvertierung in einen Delegattyp aufgezeichnet werden.

- Bei einer `ref e1 = ref e2`Ref-Neuzuweisung muss die *ref-sicher-zu-* Escapezeichen von `e2` mindestens so breit sein, wie der *ref-Safe-to-Escape* -`e1`ist.

- Für eine Ref Return-Anweisung `return ref e1`muss der *ref-Safe-to-Escape* -`e1` von der gesamten Methode *ref-Safe-to-Escape* -Zeichen sein. (TODO: Wir benötigen auch eine Regel, dass `e1` von der gesamten Methode *sicher oder über* flüssig sein muss?)

- Bei einer Return *-* Anweisung `return e1`muss die `e1` mit sicherer Escapezeichen von der gesamten Methode *sicher* sein.

- Bei einem Zuweisungs `e1 = e2`muss der Typ der `e1` ein `ref struct` Typ sein, wenn der Typ der ein Typ ist, dann muss der *Safe-to-Escape* -`e2` mindestens so breit wie der *sichere bis* Escapezeichen des `e1`sein.

- Bei einem Methodenaufruf, bei dem ein `ref` oder `out` Argument eines `ref struct` Typs (einschließlich des Empfängers) mit " *Safe-to-Escape* E1" vorhanden ist, kann kein Argument (einschließlich des Empfängers) einen engeren *Sicherheits-zu-* Escapezeichen als E1 aufweisen. [Beispiel](#method-arguments-must-match)

- Eine lokale Funktion oder eine anonyme Funktion verweist möglicherweise nicht auf einen lokalen oder-Parameter `ref struct` Typs, der in einem einschließenden Bereich deklariert wurde.

> ***Problem öffnen:*** Wir benötigen eine Regel, mit der wir einen Fehler verursachen können, wenn ein Stapel Wert eines `ref struct` Typs auf einen Erwartungs Ausdruck überlaufen werden muss, z. b. im Code
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a>Erklären
In diesen Erläuterungen und Beispielen wird erläutert, warum viele der oben aufgeführten Sicherheitsregeln vorhanden sind.

### <a name="method-arguments-must-match"></a>Methodenargumente müssen entsprechen
Wenn eine Methode aufgerufen wird, bei der eine `out`ist, `ref` Parameter, bei dem es sich um einen `ref struct` einschließlich des Empfängers handelt, müssen alle `ref struct` dieselbe Lebensdauer haben. Dies ist erforderlich, C# da alle Entscheidungen hinsichtlich der Lebensdauer Sicherheit auf der Grundlage der in der Signatur der Methode verfügbaren Informationen und der Lebensdauer der Werte an der aufrufssite treffen muss. 

Wenn `ref` Parameter vorhanden sind, die `ref struct` werden, gibt es die möglichen möglichen Inhalte der Inhalte. Daher müssen wir an der aufrufssite sicherstellen, dass alle diese **möglichen** Austausch Vorgängen kompatibel sind. Wenn die Sprache diese nicht erzwingt, wird ein fehlerhafter Code wie der folgende zugelassen.

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

Die Einschränkung für den Empfänger ist erforderlich, da kein Inhalt von Ref-Safe-to-Escape-Vorgang bereitgestellte Werte speichern kann. Dies bedeutet, dass Sie mit einer nicht übereinstimmenden Lebensdauer wie folgt eine typsicherheitslücke erstellen können:

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a>Struct this Escape
Bei spannen Sicherheitsregeln wird der `this` Wert in einem Instanzmember als Parameter für den Member modelliert. Bei einer `struct` der `this` tatsächlich `ref S`, wo Sie in einer `class` ist Sie einfach `S` (für Mitglieder einer `class / struct` mit dem Namen s). 

`this` hat jedoch andere Escaperegeln als andere `ref` Parameter. Insbesondere ist es nicht Ref-Safe-to-Escape, während andere Parameter lauten:

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

Der Grund für diese Einschränkung hat nur wenig zu tun, wenn `struct` Member aufgerufen wird. Es gibt einige Regeln, die in Bezug auf den Element Aufruf auf `struct` Membern, bei denen der Empfänger ein rvalue ist, ausgearbeitet werden müssen. Das ist jedoch sehr genehmigbar. 

Der Grund für diese Einschränkung ist tatsächlich der Schnittstellen Aufruf. Insbesondere wird darauf zurückgegriffen, ob das folgende Beispiel nicht kompiliert werden soll.

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

Beachten Sie den Fall, in dem `T` als `struct`instanziiert wird. Wenn der `this`-Parameter ref-Safe-to-Escape ist, kann die Rückgabe von `p.Get` auf den Stapel zeigen (insbesondere ein Feld innerhalb des instanziierten Typs `T`). Dies bedeutet, dass die Sprache diese Stichprobe nicht kompilieren kann, da Sie eine `ref` an einen Stapel Speicherort zurückgeben könnte. Wenn `this` hingegen nicht Ref-Safe-to-Escape ist, kann `p.Get` nicht auf den Stapel verweisen und kann daher sicher zurückgegeben werden. 

Aus diesem Grund ist die Möglichkeit der `this` in einer `struct` wirklich alles über Schnittstellen. Die Anwendung kann für Sie geeignet sein, aber Sie hat einen Kompromiss. Der Entwurf wurde schließlich zugunsten von Schnittstellen flexibler. 

Es besteht die Möglichkeit, dies in Zukunft zu lockern. 

## <a name="future-considerations"></a>Überlegungen für die Zukunft

### <a name="length-one-spant-over-ref-values"></a>Die Länge eines Bereichs\<t > über Ref-Werte.
Obwohl es heute nicht gesetzlich zulässig ist, gibt es Fälle, in denen das Erstellen einer `Span<T>` Instanz über einen Wert in Bezug auf einen Wert vorteilhaft ist

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

Diese Funktion wird immer wichtiger, wenn wir die Einschränkungen für [Puffer mit fester Größe](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) anheben, da dies `Span<T>` Instanzen mit einer noch größeren Länge ermöglicht. 

Wenn es jemals erforderlich ist, diesen Pfad zu verlassen, kann die Sprache dies ermöglichen, indem sichergestellt wird, dass `Span<T>` Instanzen nur abwärts ausgerichtet wurden. Das heißt, dass Sie nur einmal *sicher* in den Bereich, in dem Sie erstellt wurden, wieder hergestellt werden konnten. Dadurch wird sichergestellt, dass die Sprache nie einen `ref` Wert mit dem Escapezeichen einer Methode über eine `ref struct` Rückgabe oder ein `ref struct`Feld in Erwägung zieht. Dies würde wahrscheinlich auch weitere Änderungen erfordern, damit solche Konstruktoren erkannt werden, wie z. b. das Erfassen eines `ref`-Parameters.
