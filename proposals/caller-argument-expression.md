---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483517"
---
# <a name="callerargumentexpression"></a><span data-ttu-id="25b4e-101">Callerargumumtexpression</span><span class="sxs-lookup"><span data-stu-id="25b4e-101">CallerArgumentExpression</span></span>

* <span data-ttu-id="25b4e-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="25b4e-102">[x] Proposed</span></span>
* <span data-ttu-id="25b4e-103">[] Prototyp: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="25b4e-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="25b4e-104">[] Implementierung: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="25b4e-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="25b4e-105">[] Spezifikation: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="25b4e-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="25b4e-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="25b4e-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="25b4e-107">Ermöglicht Entwicklern das Erfassen der an eine Methode übergebenen Ausdrücke, um bessere Fehlermeldungen in Diagnose-/Test-APIs zu ermöglichen und Tastatureingaben zu verringern.</span><span class="sxs-lookup"><span data-stu-id="25b4e-107">Allow developers to capture the expressions passed to a method, to enable better error messages in diagnostic/testing APIs and reduce keystrokes.</span></span>

## <a name="motivation"></a><span data-ttu-id="25b4e-108">Motivation</span><span class="sxs-lookup"><span data-stu-id="25b4e-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="25b4e-109">Wenn eine Assert-oder Argument Validierung fehlschlägt, möchte der Entwickler so viel wie möglich wissen, wo und warum er fehlgeschlagen ist.</span><span class="sxs-lookup"><span data-stu-id="25b4e-109">When an assertion or argument validation fails, the developer wants to know as much as possible about where and why it failed.</span></span> <span data-ttu-id="25b4e-110">Die Diagnose-APIs von heute vereinfachen dies jedoch nicht vollständig.</span><span class="sxs-lookup"><span data-stu-id="25b4e-110">However, today's diagnostic APIs do not fully facilitate this.</span></span> <span data-ttu-id="25b4e-111">Sehen Sie sich die folgende Methode an:</span><span class="sxs-lookup"><span data-stu-id="25b4e-111">Consider the following method:</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

<span data-ttu-id="25b4e-112">Wenn einer der Bestätigungen fehlschlägt, werden nur der Dateiname, die Zeilennummer und der Methodenname in der Stapel Überwachung bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="25b4e-112">When one of the asserts fail, only the filename, line number, and method name will be provided in the stack trace.</span></span> <span data-ttu-id="25b4e-113">Der Entwickler kann nicht feststellen, welche Assert-Anweisung diese Informationen nicht bestanden hat. er muss die Datei öffnen und zu der angegebenen Zeilennummer navigieren, um zu sehen, was schief gelaufen ist.</span><span class="sxs-lookup"><span data-stu-id="25b4e-113">The developer will not be able to tell which assert failed from this information-- (s)he will have to open the file and navigate to the provided line number to see what went wrong.</span></span>

<span data-ttu-id="25b4e-114">Dies ist auch der Grund, warum das Testen von Frameworks eine Vielzahl von Assert-Methoden bereitstellen muss.</span><span class="sxs-lookup"><span data-stu-id="25b4e-114">This is also the reason testing frameworks have to provide a variety of assert methods.</span></span> <span data-ttu-id="25b4e-115">Mit xUnit werden `Assert.True` und `Assert.False` nicht häufig verwendet, da Sie nicht genug Kontext zum fehlgeschlagenen Kontext bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="25b4e-115">With xUnit, `Assert.True` and `Assert.False` are not frequently used because they do not provide enough context about what failed.</span></span>

<span data-ttu-id="25b4e-116">Obwohl die Situation für die Argument Validierung etwas besser ist, da die Namen der ungültigen Argumente dem Entwickler angezeigt werden, muss der Entwickler diese Namen manuell an Ausnahmen übergeben.</span><span class="sxs-lookup"><span data-stu-id="25b4e-116">While the situation is a bit better for argument validation because the names of invalid arguments are shown to the developer, the developer must pass these names to exceptions manually.</span></span> <span data-ttu-id="25b4e-117">Wenn das obige Beispiel so umgeschrieben wurde, dass anstelle der `Debug.Assert`herkömmliche Argument Validierung verwendet wird, sieht es wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="25b4e-117">If the above example were rewritten to use traditional argument validation instead of `Debug.Assert`, it would look like</span></span>

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

<span data-ttu-id="25b4e-118">Beachten Sie, dass `nameof(array)` an jede Ausnahme übermittelt werden muss, obwohl Sie bereits aus dem Kontext eindeutig ist, welches Argument ungültig ist.</span><span class="sxs-lookup"><span data-stu-id="25b4e-118">Notice that `nameof(array)` must be passed to each exception, although it's already clear from context which argument is invalid.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="25b4e-119">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="25b4e-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="25b4e-120">In den obigen Beispielen kann der Entwickler mit der Zeichenfolge `"array != null"` oder `"array.Length == 1"` in der Assert-Nachricht ermitteln, was fehlgeschlagen ist.</span><span class="sxs-lookup"><span data-stu-id="25b4e-120">In the above examples, including the string `"array != null"` or `"array.Length == 1"` in the assert message would help the developer determine what failed.</span></span> <span data-ttu-id="25b4e-121">Geben Sie `CallerArgumentExpression`ein: Es ist ein Attribut, mit dem das Framework die Zeichenfolge abrufen kann, die einem bestimmten Methoden Argument zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="25b4e-121">Enter `CallerArgumentExpression`: it's an attribute the framework can use to obtain the string associated with a particular method argument.</span></span> <span data-ttu-id="25b4e-122">Wir würden es `Debug.Assert` wie folgt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="25b4e-122">We would add it to `Debug.Assert` like so</span></span>

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

<span data-ttu-id="25b4e-123">Der Quellcode im obigen Beispiel würde unverändert bleiben.</span><span class="sxs-lookup"><span data-stu-id="25b4e-123">The source code in the above example would stay the same.</span></span> <span data-ttu-id="25b4e-124">Der Code, den der Compiler tatsächlich ausgibt, entspricht jedoch</span><span class="sxs-lookup"><span data-stu-id="25b4e-124">However, the code the compiler actually emits would correspond to</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

<span data-ttu-id="25b4e-125">Der Compiler erkennt das-Attribut speziell in `Debug.Assert`.</span><span class="sxs-lookup"><span data-stu-id="25b4e-125">The compiler specially recognizes the attribute on `Debug.Assert`.</span></span> <span data-ttu-id="25b4e-126">Sie übergibt die Zeichenfolge, die dem Argument zugeordnet ist, auf das im Konstruktor des Attributs (in diesem Fall `condition`) in der aufrufssite verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="25b4e-126">It passes the string associated with the argument referred to in the attribute's constructor (in this case, `condition`) at the call site.</span></span> <span data-ttu-id="25b4e-127">Wenn eine der Assert-Fehler fehlschlägt, wird dem Entwickler die Bedingung angezeigt, die falsch war, und weiß, welcher Fehler aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="25b4e-127">When either assert fails, the developer will be shown the condition that was false and will know which one failed.</span></span>

<span data-ttu-id="25b4e-128">Bei der Argument Validierung kann das Attribut nicht direkt verwendet werden, kann jedoch über eine Hilfsklasse verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="25b4e-128">For argument validation, the attribute cannot be used directly, but can be made use of through a helper class:</span></span>

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

<span data-ttu-id="25b4e-129">Ein Vorschlag zum Hinzufügen einer solchen Hilfsklasse zum Framework wird bei https://github.com/dotnet/corefx/issues/17068ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="25b4e-129">A proposal to add such a helper class to the framework is underway at https://github.com/dotnet/corefx/issues/17068.</span></span> <span data-ttu-id="25b4e-130">Wenn diese Sprachfunktion implementiert wurde, könnte der Vorschlag aktualisiert werden, um dieses Feature zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="25b4e-130">If this language feature was implemented, the proposal could be updated to take advantage of this feature.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="25b4e-131">Erweiterungsmethoden</span><span class="sxs-lookup"><span data-stu-id="25b4e-131">Extension methods</span></span>

<span data-ttu-id="25b4e-132">Auf den `this`-Parameter in einer Erweiterungsmethode kann von `CallerArgumentExpression`verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="25b4e-132">The `this` parameter in an extension method may be referenced by `CallerArgumentExpression`.</span></span> <span data-ttu-id="25b4e-133">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="25b4e-133">For example:</span></span>

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

<span data-ttu-id="25b4e-134">`thisExpression` empfängt den Ausdruck, der dem Objekt vor dem Punkt entspricht.</span><span class="sxs-lookup"><span data-stu-id="25b4e-134">`thisExpression` will receive the expression corresponding to the object before the dot.</span></span> <span data-ttu-id="25b4e-135">Wenn Sie mit statischer Methoden Syntax aufgerufen wird, z. b. `Ext.ShouldBe(contestant.Points, 1337)`, verhält sie sich so, als ob der erste Parameter nicht als `this`gekennzeichnet wäre.</span><span class="sxs-lookup"><span data-stu-id="25b4e-135">If it's called with static method syntax, e.g. `Ext.ShouldBe(contestant.Points, 1337)`, it will behave as if first parameter wasn't marked `this`.</span></span>

<span data-ttu-id="25b4e-136">Es sollte immer ein Ausdruck vorhanden sein, der dem `this`-Parameter entspricht.</span><span class="sxs-lookup"><span data-stu-id="25b4e-136">There should always be an expression corresponding to the `this` parameter.</span></span> <span data-ttu-id="25b4e-137">Auch wenn eine Instanz einer Klasse eine Erweiterungsmethode auf sich selbst aufruft, z. b. `this.Single()` aus einem Sammlungstyp, wird der `this` vom Compiler vorgeschrieben, damit `"this"` weitergeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="25b4e-137">Even if an instance of a class calls an extension method on itself, e.g. `this.Single()` from inside a collection type, the `this` is mandated by the compiler so `"this"` will get passed.</span></span> <span data-ttu-id="25b4e-138">Wenn diese Regel in Zukunft geändert wird, können Sie die Übergabe von `null` oder der leeren Zeichenfolge in Erwägung gezogen.</span><span class="sxs-lookup"><span data-stu-id="25b4e-138">If this rule is changed in the future, we can consider passing `null` or the empty string.</span></span>

### <a name="extra-details"></a><span data-ttu-id="25b4e-139">Weitere Details</span><span class="sxs-lookup"><span data-stu-id="25b4e-139">Extra details</span></span>

- <span data-ttu-id="25b4e-140">Wie bei den anderen `Caller*` Attributen (z. b. `CallerMemberName`) kann dieses Attribut nur für Parameter mit Standardwerten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="25b4e-140">Like the other `Caller*` attributes, such as `CallerMemberName`, this attribute may only be used on parameters with default values.</span></span>
- <span data-ttu-id="25b4e-141">Mehrere Parameter, die mit `CallerArgumentExpression` gekennzeichnet sind, sind zulässig, wie oben gezeigt.</span><span class="sxs-lookup"><span data-stu-id="25b4e-141">Multiple parameters marked with `CallerArgumentExpression` are permitted, as shown above.</span></span>
- <span data-ttu-id="25b4e-142">Der Namespace des Attributs wird `System.Runtime.CompilerServices`.</span><span class="sxs-lookup"><span data-stu-id="25b4e-142">The attribute's namespace will be `System.Runtime.CompilerServices`.</span></span>
- <span data-ttu-id="25b4e-143">Wenn `null` oder eine Zeichenfolge, die kein Parameter Name ist (z. b. `"notAParameterName"`), angegeben wird, übergibt der Compiler eine leere Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="25b4e-143">If `null` or a string that is not a parameter name (e.g. `"notAParameterName"`) is provided, the compiler will pass in an empty string.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="25b4e-144">Nachteile</span><span class="sxs-lookup"><span data-stu-id="25b4e-144">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="25b4e-145">Personen, die wissen, wie Decompiler verwendet werden, können einen Teil des Quellcodes an Aufruf Sites für Methoden sehen, die mit diesem Attribut gekennzeichnet sind.</span><span class="sxs-lookup"><span data-stu-id="25b4e-145">People who know how to use decompilers will be able to see some of the source code at call sites for methods marked with this attribute.</span></span> <span data-ttu-id="25b4e-146">Dies kann für geschlossene Quellsoftware nicht erwünscht bzw. unerwartet sein.</span><span class="sxs-lookup"><span data-stu-id="25b4e-146">This may be undesirable/unexpected for closed-source software.</span></span>

- <span data-ttu-id="25b4e-147">Dies ist zwar kein Fehler in der Funktion selbst, aber eine mögliche Ursache ist, dass heute eine `Debug.Assert`-API vorhanden ist, die nur eine `bool`benötigt.</span><span class="sxs-lookup"><span data-stu-id="25b4e-147">Although this is not a flaw in the feature itself, a source of concern may be that there exists a `Debug.Assert` API today that only takes a `bool`.</span></span> <span data-ttu-id="25b4e-148">Selbst wenn die Überladung, die eine Nachricht annimmt, den zweiten Parameter hat, der mit diesem Attribut gekennzeichnet und optional gemacht wurde, wählt der Compiler weiterhin die nichtnachricht in der Überladungs Auflösung aus.</span><span class="sxs-lookup"><span data-stu-id="25b4e-148">Even if the overload taking a message had its second parameter marked with this attribute and made optional, the compiler would still pick the no-message one in overload resolution.</span></span> <span data-ttu-id="25b4e-149">Daher müsste die Überladung No-Message entfernt werden, um dieses Feature zu nutzen. Dies wäre eine binäre (obwohl keine Quell-) Breaking Change.</span><span class="sxs-lookup"><span data-stu-id="25b4e-149">Therefore, the no-message overload would have to be removed to take advantage of this feature, which would be a binary (although not source) breaking change.</span></span>

## <a name="alternatives"></a><span data-ttu-id="25b4e-150">Alternativen</span><span class="sxs-lookup"><span data-stu-id="25b4e-150">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="25b4e-151">Wenn in der Lage ist, Quellcode für Methoden, die dieses Attribut verwenden, auf Aufruf Websites zu erkennen, kann ein Problem auftreten.</span><span class="sxs-lookup"><span data-stu-id="25b4e-151">If being able to see source code at call sites for methods that use this attribute proves to be a problem, we can make the attribute's effects opt-in.</span></span> <span data-ttu-id="25b4e-152">Entwickler werden Sie über ein assemblyweites `[assembly: EnableCallerArgumentExpression]` Attribut aktivieren, das Sie in `AssemblyInfo.cs`ablegen.</span><span class="sxs-lookup"><span data-stu-id="25b4e-152">Developers will enable it through an assembly-wide `[assembly: EnableCallerArgumentExpression]` attribute they put in `AssemblyInfo.cs`.</span></span>
  - <span data-ttu-id="25b4e-153">Wenn die Auswirkungen des-Attributs nicht aktiviert sind, ist das Aufrufen von Methoden, die mit dem-Attribut markiert sind, kein Fehler, damit vorhandene Methoden das-Attribut verwenden und die Quell Kompatibilität beibehalten können.</span><span class="sxs-lookup"><span data-stu-id="25b4e-153">In the case the attribute's effects are not enabled, calling methods marked with the attribute would not be an error, to allow existing methods to use the attribute and maintain source compatibility.</span></span> <span data-ttu-id="25b4e-154">Das-Attribut wird jedoch ignoriert, und die-Methode wird mit dem angegebenen Standardwert aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="25b4e-154">However, the attribute would be ignored and the method would be called with whatever default value was provided.</span></span>

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

- <span data-ttu-id="25b4e-155">Um zu verhindern, dass das [binäre Kompatibilitätsproblem] [ drawbacks] jedes Mal auftritt, wenn Sie `Debug.Assert`neue Aufruferinformationen hinzufügen möchten, besteht eine alternative Lösung darin, dem Framework eine `CallerInfo` Struktur hinzuzufügen, die alle erforderlichen Informationen über den Aufrufer enthält.</span><span class="sxs-lookup"><span data-stu-id="25b4e-155">To prevent the [binary compatibility problem][drawbacks] from occurring every time we want to add new caller info to `Debug.Assert`, an alternative solution would be to add a `CallerInfo` struct to the framework that contains all the necessary information about the caller.</span></span>

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

<span data-ttu-id="25b4e-156">Dies wurde ursprünglich bei https://github.com/dotnet/csharplang/issues/87vorgeschlagen.</span><span class="sxs-lookup"><span data-stu-id="25b4e-156">This was originally proposed at https://github.com/dotnet/csharplang/issues/87.</span></span>

<span data-ttu-id="25b4e-157">Diese Vorgehensweise hat einige Nachteile:</span><span class="sxs-lookup"><span data-stu-id="25b4e-157">There are a few disadvantages of this approach:</span></span>

- <span data-ttu-id="25b4e-158">Obwohl es Ihnen ermöglicht, benutzerfreundlich zu machen, indem Sie festlegen können, welche Eigenschaften Sie benötigen, kann die Leistung durch die Zuordnung eines Arrays für die Ausdrücke bzw. das Aufrufen von `MethodBase.GetCurrentMethod` selbst beim Durchlaufen der Bestätigung erheblich beeinträchtigt werden.</span><span class="sxs-lookup"><span data-stu-id="25b4e-158">Despite being pay-for-play friendly by allowing you to specify which properties you need, it could still hurt perf significantly by allocating an array for the expressions/calling `MethodBase.GetCurrentMethod` even when the assert passes.</span></span>

- <span data-ttu-id="25b4e-159">Wenn Sie ein neues Flag an das `CallerInfo` Breaking Change Attribut übergeben, ist es außerdem nicht garantiert, dass `Debug.Assert` tatsächlich den neuen Parameter von den aufrufen, die mit einer alten Version der Methode kompiliert wurden, empfangen.</span><span class="sxs-lookup"><span data-stu-id="25b4e-159">Additionally, while passing a new flag to the `CallerInfo` attribute won't be a breaking change, `Debug.Assert` won't be guaranteed to actually receive that new parameter from call sites that compiled against an old version of the method.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="25b4e-160">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="25b4e-160">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="25b4e-161">TBD</span><span class="sxs-lookup"><span data-stu-id="25b4e-161">TBD</span></span>

## <a name="design-meetings"></a><span data-ttu-id="25b4e-162">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="25b4e-162">Design meetings</span></span>

<span data-ttu-id="25b4e-163">N/V</span><span class="sxs-lookup"><span data-stu-id="25b4e-163">N/A</span></span>
