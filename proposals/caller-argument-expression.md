---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483517"
---
# <a name="callerargumentexpression"></a>Callerargumumtexpression

* [x] vorgeschlagen
* [] Prototyp: nicht gestartet
* [] Implementierung: nicht gestartet
* [] Spezifikation: nicht gestartet

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Ermöglicht Entwicklern das Erfassen der an eine Methode übergebenen Ausdrücke, um bessere Fehlermeldungen in Diagnose-/Test-APIs zu ermöglichen und Tastatureingaben zu verringern.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Wenn eine Assert-oder Argument Validierung fehlschlägt, möchte der Entwickler so viel wie möglich wissen, wo und warum er fehlgeschlagen ist. Die Diagnose-APIs von heute vereinfachen dies jedoch nicht vollständig. Sehen Sie sich die folgende Methode an:

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

Wenn einer der Bestätigungen fehlschlägt, werden nur der Dateiname, die Zeilennummer und der Methodenname in der Stapel Überwachung bereitgestellt. Der Entwickler kann nicht feststellen, welche Assert-Anweisung diese Informationen nicht bestanden hat. er muss die Datei öffnen und zu der angegebenen Zeilennummer navigieren, um zu sehen, was schief gelaufen ist.

Dies ist auch der Grund, warum das Testen von Frameworks eine Vielzahl von Assert-Methoden bereitstellen muss. Mit xUnit werden `Assert.True` und `Assert.False` nicht häufig verwendet, da Sie nicht genug Kontext zum fehlgeschlagenen Kontext bereitstellen.

Obwohl die Situation für die Argument Validierung etwas besser ist, da die Namen der ungültigen Argumente dem Entwickler angezeigt werden, muss der Entwickler diese Namen manuell an Ausnahmen übergeben. Wenn das obige Beispiel so umgeschrieben wurde, dass anstelle der `Debug.Assert`herkömmliche Argument Validierung verwendet wird, sieht es wie folgt aus:

```csharp
T Single<T>(this T[] array)
{
    if (array == null)
    {
        throw new ArgumentNullException(nameof(array));
    }

    if (array.Length != 1)
    {
        throw new ArgumentException("Array must contain a single element.", nameof(array));
    }

    return array[0];
}
```

Beachten Sie, dass `nameof(array)` an jede Ausnahme übermittelt werden muss, obwohl Sie bereits aus dem Kontext eindeutig ist, welches Argument ungültig ist.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

In den obigen Beispielen kann der Entwickler mit der Zeichenfolge `"array != null"` oder `"array.Length == 1"` in der Assert-Nachricht ermitteln, was fehlgeschlagen ist. Geben Sie `CallerArgumentExpression`ein: Es ist ein Attribut, mit dem das Framework die Zeichenfolge abrufen kann, die einem bestimmten Methoden Argument zugeordnet ist. Wir würden es `Debug.Assert` wie folgt hinzufügen.

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

Der Quellcode im obigen Beispiel würde unverändert bleiben. Der Code, den der Compiler tatsächlich ausgibt, entspricht jedoch

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

Der Compiler erkennt das-Attribut speziell in `Debug.Assert`. Sie übergibt die Zeichenfolge, die dem Argument zugeordnet ist, auf das im Konstruktor des Attributs (in diesem Fall `condition`) in der aufrufssite verwiesen wird. Wenn eine der Assert-Fehler fehlschlägt, wird dem Entwickler die Bedingung angezeigt, die falsch war, und weiß, welcher Fehler aufgetreten ist.

Bei der Argument Validierung kann das Attribut nicht direkt verwendet werden, kann jedoch über eine Hilfsklasse verwendet werden:

```csharp
public static class Verify
{
    public static void Argument(bool condition, string message, [CallerArgumentExpression("condition")] string conditionExpression = null)
    {
        if (!condition) throw new ArgumentException(message: message, paramName: conditionExpression);
    }

    public static void InRange(int argument, int low, int high,
        [CallerArgumentExpression("argument")] string argumentExpression = null,
        [CallerArgumentExpression("low")] string lowExpression = null,
        [CallerArgumentExpression("high")] string highExpression = null)
    {
        if (argument < low)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be less than {lowExpression} ({low}).");
        }

        if (argument > high)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be greater than {highExpression} ({high}).");
        }
    }

    public static void NotNull<T>(T argument, [CallerArgumentExpression("argument")] string argumentExpression = null)
        where T : class
    {
        if (argument == null) throw new ArgumentNullException(paramName: argumentExpression);
    }
}

T Single<T>(this T[] array)
{
    Verify.NotNull(array); // paramName: "array"
    Verify.Argument(array.Length == 1, "Array must contain a single element."); // paramName: "array.Length == 1"

    return array[0];
}

T ElementAt(this T[] array, int index)
{
    Verify.NotNull(array); // paramName: "array"
    // paramName: "index"
    // message: "index (-1) cannot be less than 0 (0).", or
    //          "index (6) cannot be greater than array.Length - 1 (5)."
    Verify.InRange(index, 0, array.Length - 1);

    return array[index];
}
```

Ein Vorschlag zum Hinzufügen einer solchen Hilfsklasse zum Framework wird bei https://github.com/dotnet/corefx/issues/17068ausgeführt. Wenn diese Sprachfunktion implementiert wurde, könnte der Vorschlag aktualisiert werden, um dieses Feature zu nutzen.

### <a name="extension-methods"></a>Erweiterungsmethoden

Auf den `this`-Parameter in einer Erweiterungsmethode kann von `CallerArgumentExpression`verwiesen werden. Beispiel:

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

`thisExpression` empfängt den Ausdruck, der dem Objekt vor dem Punkt entspricht. Wenn Sie mit statischer Methoden Syntax aufgerufen wird, z. b. `Ext.ShouldBe(contestant.Points, 1337)`, verhält sie sich so, als ob der erste Parameter nicht als `this`gekennzeichnet wäre.

Es sollte immer ein Ausdruck vorhanden sein, der dem `this`-Parameter entspricht. Auch wenn eine Instanz einer Klasse eine Erweiterungsmethode auf sich selbst aufruft, z. b. `this.Single()` aus einem Sammlungstyp, wird der `this` vom Compiler vorgeschrieben, damit `"this"` weitergeleitet wird. Wenn diese Regel in Zukunft geändert wird, können Sie die Übergabe von `null` oder der leeren Zeichenfolge in Erwägung gezogen.

### <a name="extra-details"></a>Weitere Details

- Wie bei den anderen `Caller*` Attributen (z. b. `CallerMemberName`) kann dieses Attribut nur für Parameter mit Standardwerten verwendet werden.
- Mehrere Parameter, die mit `CallerArgumentExpression` gekennzeichnet sind, sind zulässig, wie oben gezeigt.
- Der Namespace des Attributs wird `System.Runtime.CompilerServices`.
- Wenn `null` oder eine Zeichenfolge, die kein Parameter Name ist (z. b. `"notAParameterName"`), angegeben wird, übergibt der Compiler eine leere Zeichenfolge.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

- Personen, die wissen, wie Decompiler verwendet werden, können einen Teil des Quellcodes an Aufruf Sites für Methoden sehen, die mit diesem Attribut gekennzeichnet sind. Dies kann für geschlossene Quellsoftware nicht erwünscht bzw. unerwartet sein.

- Dies ist zwar kein Fehler in der Funktion selbst, aber eine mögliche Ursache ist, dass heute eine `Debug.Assert`-API vorhanden ist, die nur eine `bool`benötigt. Selbst wenn die Überladung, die eine Nachricht annimmt, den zweiten Parameter hat, der mit diesem Attribut gekennzeichnet und optional gemacht wurde, wählt der Compiler weiterhin die nichtnachricht in der Überladungs Auflösung aus. Daher müsste die Überladung No-Message entfernt werden, um dieses Feature zu nutzen. Dies wäre eine binäre (obwohl keine Quell-) Breaking Change.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

- Wenn in der Lage ist, Quellcode für Methoden, die dieses Attribut verwenden, auf Aufruf Websites zu erkennen, kann ein Problem auftreten. Entwickler werden Sie über ein assemblyweites `[assembly: EnableCallerArgumentExpression]` Attribut aktivieren, das Sie in `AssemblyInfo.cs`ablegen.
  - Wenn die Auswirkungen des-Attributs nicht aktiviert sind, ist das Aufrufen von Methoden, die mit dem-Attribut markiert sind, kein Fehler, damit vorhandene Methoden das-Attribut verwenden und die Quell Kompatibilität beibehalten können. Das-Attribut wird jedoch ignoriert, und die-Methode wird mit dem angegebenen Standardwert aufgerufen.

```csharp
// Assembly1

void Foo(string bar); // V1
void Foo(string bar, string barExpression = "not provided"); // V2
void Foo(string bar, [CallerArgumentExpression("bar")] string barExpression = "not provided"); // V3

// Assembly2

Foo(a); // V1: Compiles to Foo(a), V2, V3: Compiles to Foo(a, "not provided")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")

// Assembly3

[assembly: EnableCallerArgumentExpression]

Foo(a); // V1: Compiles to Foo(a), V2: Compiles to Foo(a, "not provided"), V3: Compiles to Foo(a, "a")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")
```

- Um zu verhindern, dass das [binäre Kompatibilitätsproblem] [ drawbacks] jedes Mal auftritt, wenn Sie `Debug.Assert`neue Aufruferinformationen hinzufügen möchten, besteht eine alternative Lösung darin, dem Framework eine `CallerInfo` Struktur hinzuzufügen, die alle erforderlichen Informationen über den Aufrufer enthält.

```csharp
struct CallerInfo
{
    public string MemberName { get; set; }
    public string TypeName { get; set; }
    public string Namespace { get; set; }
    public string FullTypeName { get; set; }
    public string FilePath { get; set; }
    public int LineNumber { get; set; }
    public int ColumnNumber { get; set; }
    public Type Type { get; set; }
    public MethodBase Method { get; set; }
    public string[] ArgumentExpressions { get; set; }
}

[Flags]
enum CallerInfoOptions
{
    MemberName = 1, TypeName = 2, ...
}

public static class Debug
{
    public static void Assert(bool condition,
        // If a flag is not set here, the corresponding CallerInfo member is not populated by the caller, so it's
        // pay-for-play friendly.
        [CallerInfo(CallerInfoOptions.FilePath | CallerInfoOptions.Method | CallerInfoOptions.ArgumentExpressions)] CallerInfo callerInfo = default(CallerInfo))
    {
        string filePath = callerInfo.FilePath;
        MethodBase method = callerInfo.Method;
        string conditionExpression = callerInfo.ArgumentExpressions[0];
        ...
    }
}

class Bar
{
    void Foo()
    {
        Debug.Assert(false);

        // Translates to:

        var callerInfo = new CallerInfo();
        callerInfo.FilePath = @"C:\Bar.cs";
        callerInfo.Method = MethodBase.GetCurrentMethod();
        callerInfo.ArgumentExpressions = new string[] { "false" };
        Debug.Assert(false, callerInfo);
    }
}
```

Dies wurde ursprünglich bei https://github.com/dotnet/csharplang/issues/87vorgeschlagen.

Diese Vorgehensweise hat einige Nachteile:

- Obwohl es Ihnen ermöglicht, benutzerfreundlich zu machen, indem Sie festlegen können, welche Eigenschaften Sie benötigen, kann die Leistung durch die Zuordnung eines Arrays für die Ausdrücke bzw. das Aufrufen von `MethodBase.GetCurrentMethod` selbst beim Durchlaufen der Bestätigung erheblich beeinträchtigt werden.

- Wenn Sie ein neues Flag an das `CallerInfo` Breaking Change Attribut übergeben, ist es außerdem nicht garantiert, dass `Debug.Assert` tatsächlich den neuen Parameter von den aufrufen, die mit einer alten Version der Methode kompiliert wurden, empfangen.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

TBD

## <a name="design-meetings"></a>Treffen von Besprechungen

N/V
