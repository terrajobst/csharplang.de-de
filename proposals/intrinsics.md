---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483553"
---
# <a name="compiler-intrinsics"></a>Intrinsische Compilerfunktionen

## <a name="summary"></a>Zusammenfassung

Dieser Vorschlag bietet Sprachkonstrukte, die IL-Opcodes auf niedriger Ebene verfügbar machen, auf die zurzeit nicht effizient zugegriffen werden kann, oder überhaupt: `ldftn`, `ldvirtftn``ldtoken` und `calli`. Diese Opcodes auf niedriger Ebene können bei Hochleistungs Code wichtig sein, und Entwickler benötigen eine effiziente Zugriffsmethode.

## <a name="motivation"></a>Motivation

Die Gründe und der Hintergrund für dieses Feature werden im folgenden Problem beschrieben (wie eine mögliche Implementierung des Features): 

https://github.com/dotnet/csharplang/issues/191

Dieser Alternative Entwurfsvorschlag wird nach der Überprüfung der Prototypimplementierung des ursprünglichen Angebots durch @msjabby und der Verwendung in einer bedeutenden Codebasis angezeigt. Dieser Entwurf wurde mit erheblichen Eingaben aus @mjsabby, @tmat und @jkotasdurchgeführt.

## <a name="detailed-design"></a>Detaillierter Entwurf 

### <a name="allow-address-of-to-target-methods"></a>Address-Methode für Ziel Methoden zulassen

Methoden Gruppen werden nun als Argumente für einen Address-of-Ausdruck zugelassen. Der Typ eines solchen Ausdrucks wird `void*`. 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

Da hier keine Delegatkonvertierung vorhanden ist, ist der einzige Mechanismus zum Filtern von Elementen in der Methoden Gruppe der statische/instanzzugriff. Wenn die Elemente nicht unterschieden werden können, tritt ein Kompilierzeitfehler auf.

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

Der AddressOf-Ausdruck in diesem Kontext wird folgendermaßen implementiert:

- ldftn:, wenn die Methode nicht virtuell ist.
- ldvirtftn: Wenn die Methode virtuell ist.

Einschränkungen dieses Features:

- Instanzmethoden können nur angegeben werden, wenn ein Aufruf Ausdruck für einen Wert verwendet wird.
- Lokale Funktionen können nicht in `&`verwendet werden. Die Implementierungsdetails dieser Methoden werden absichtlich nicht in der Sprache angegeben. Dies umfasst auch, ob es sich um statische oder Instanz handelt oder genau, mit welcher Signatur Sie ausgegeben werden.

### <a name="handleof"></a>Lenker

Das kontextabhängige Schlüsselwort `handleof` übersetzt ein Feld, einen Member oder einen Typ mithilfe der `ldtoken`-Anweisung in den entsprechenden `RuntimeHandle` Typ. Der genaue Typ des Ausdrucks hängt von der Art des Namens in `handleof`ab:

- Feld: `RuntimeFieldHandle`
- Typ: `RuntimeTypeHandle`
- Methode: `RuntimeMethodHandle`

Die Argumente für die `handleof` sind identisch mit `nameof`. Dabei muss es sich um einen einfachen Namen, einen qualifizierten Namen, einen Element Zugriff, den Basis Zugriff mit einem angegebenen Member oder um diesen Zugriff mit einem angegebenen Element handeln. Der Argumentausdruck identifiziert eine Codedefinition, wird jedoch niemals ausgewertet.

Der `handleof` Ausdruck wird zur Laufzeit ausgewertet und hat den Rückgabetyp `RuntimeHandle`. Dies kann in sicherem Code und unsicher ausgeführt werden. 

``` 
RuntimeHandle stringHandle = handleof(string);
```

Einschränkungen dieses Features:

- Eigenschaften können nicht in einem `handleof` Ausdruck verwendet werden.
- Der `handleof` Ausdruck kann nicht verwendet werden, wenn ein vorhandener `handleof` Name im Gültigkeitsbereich vorhanden ist. Z. b. Typ, Namespace usw...

### <a name="calli"></a>Calli

Der Compiler fügt Unterstützung für einen neuen Typ von `extern` Funktion hinzu, der effizient in eine `.calli` Anweisung übersetzt wird. Das extern-Attribut wird mit einem Attribut der folgenden Form gekennzeichnet:

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

Dies ermöglicht Entwicklern das Definieren von Methoden in der folgenden Form:

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

Einschränkungen für die Methode, auf die das `CallIndirect`-Attribut angewendet wurde:

- Ein `DllImport`-Attribut ist nicht möglich.
- Kann nicht generisch sein.

## <a name="open-issues"></a>Offene Probleme

### <a name="callingconvention"></a>CallingConvention

Die `CallIndirectAttribute` wie entworfen verwendet die `CallingConvention` Enumeration, die einen Eintrag für verwaltete Aufruf Konventionen fehlt. Die Aufzählung muss entweder so erweitert werden, dass diese Aufruf Konvention enthalten ist, oder das Attribut muss einen anderen Ansatz annehmen.

## <a name="considerations"></a>Überlegungen

### <a name="disambiguating-method-groups"></a>Disambiguating-Methoden Gruppen

Es gab einige Erörterung von Features, die es einfacher machen, Methoden Gruppen zu unterscheiden, die an einen Address-of-Ausdruck übergeben wurden. Beispielsweise können der Syntax möglicherweise Signatur Elemente hinzugefügt werden:

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

Dies wurde zurückgewiesen, weil ein Großbuchstabe nicht durchgeführt werden konnte und hier keine einfache Syntax gefunden werden konnte. Außerdem gibt es eine recht geradlinige Aufgabe: einfach definieren Sie eine andere Methode, die eindeutig ist C# , und verwenden Sie Code, um die gewünschte Funktion aufzurufen. 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

Dies wird sogar noch einfacher, wenn `static` lokalen Funktionen die Sprache eingeben. Anschließend könnte die Problem Umgehung in der gleichen Funktion definiert werden, die den mehrdeutigen address-of-Vorgang verwendet hat:

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a>LoadTypeTokenInt32

Der ursprüngliche Vorschlag, der zum Laden von Metadatentoken als `int` Werte zur Kompilierzeit zulässig ist. Verfügen im Wesentlichen über `tokenof`, die die gleichen Argumente wie `handleof` haben, aber zur Kompilierzeit auf eine `int` Konstante ausgewertet wird. 

Dies wurde abgelehnt, da dadurch ein erhebliches Problem bei Il-neuschreib Vorgängen verursacht wird (von denen .net viele umfasst). Solche ReWriter manipulieren die Metadatentabellen oft so, dass diese Werte ungültig werden könnten. Es gibt keine sinnvolle Möglichkeit für solche ReWriter, diese Werte zu aktualisieren, wenn Sie als einfache `int` Werte gespeichert werden.

Die zugrundeliegende Idee, ein undurchsichtiges Handle für Metadateneinträge zu haben, wird weiterhin vom Lauf Zeit Team untersucht. 

## <a name="future-considerations"></a>Überlegungen für die Zukunft

### <a name="static-local-functions"></a>statische lokale Funktionen

Dies bezieht sich auf [den Vorschlag](https://github.com/dotnet/csharplang/issues/1565) , den `static` Modifizierer für lokale Funktionen zuzulassen. Eine solche Funktion wird garantiert als `static` und mit der exakten Signatur ausgegeben, die im Quellcode angegeben ist. Eine solche Funktion sollte ein gültiges Argument für `&` sein, da Sie keines der Probleme aufweist, die lokale Funktionen heute aufweisen.

### <a name="nativecallableattribute"></a>Nativecallableattribute

Die CLR verfügt über eine Funktion, mit der verwaltete Methoden so ausgegeben werden können, dass Sie direkt aus nativem Code aufgerufen werden können. Dies erfolgt durch Hinzufügen des `NativeCallableAttribute` zu Methoden. Eine solche Methode kann nur aus nativem Code aufgerufen werden und muss daher nur blitfähige Typen in der Signatur enthalten. Der Aufruf von verwaltetem Code führt zu einem Laufzeitfehler. 

Diese Funktion ist mit diesem Vorschlag gut wie möglich:

- Übergeben einer in verwaltetem Code definierten Funktion an systemeigenen Code als Funktionszeiger (über Address-of) ohne mehr Aufwand in verwaltetem oder nativem Code. 
- Runtime kann Verwendungs Site Fehler für solche Funktionen in verwaltetem Code einführen, um zu verhindern, dass Sie zur Kompilierzeit aufgerufen werden.




