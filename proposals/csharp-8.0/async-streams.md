---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79484147"
---
# <a name="async-streams"></a><span data-ttu-id="ea7c9-101">Async-Streams</span><span class="sxs-lookup"><span data-stu-id="ea7c9-101">Async Streams</span></span>

* <span data-ttu-id="ea7c9-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="ea7c9-102">[x] Proposed</span></span>
* <span data-ttu-id="ea7c9-103">[x] Prototyp</span><span class="sxs-lookup"><span data-stu-id="ea7c9-103">[x] Prototype</span></span>
* <span data-ttu-id="ea7c9-104">[]-Implementierung</span><span class="sxs-lookup"><span data-stu-id="ea7c9-104">[ ] Implementation</span></span>
* <span data-ttu-id="ea7c9-105">[]-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="ea7c9-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="ea7c9-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="ea7c9-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ea7c9-107">C#bietet Unterstützung für Iteratormethoden und asynchrone Methoden, aber keine Unterstützung für eine Methode, bei der es sich um einen Iterator und eine Async-Methode handelt.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-107">C# has support for iterator methods and async methods, but no support for a method that is both an iterator and an async method.</span></span>  <span data-ttu-id="ea7c9-108">Dies sollten Sie beheben, indem Sie zulassen, dass `await` in einer neuen Form `async` Iterators verwendet wird, der eine `IAsyncEnumerable<T>` oder `IAsyncEnumerator<T>` anstelle eines `IEnumerable<T>` oder `IEnumerator<T>`zurückgibt, wobei `IAsyncEnumerable<T>` in einem neuen `await foreach`verwendbar ist.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-108">We should rectify this by allowing for `await` to be used in a new form of `async` iterator, one that returns an `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` rather than an `IEnumerable<T>` or `IEnumerator<T>`, with `IAsyncEnumerable<T>` consumable in a new `await foreach`.</span></span>  <span data-ttu-id="ea7c9-109">Eine `IAsyncDisposable`-Schnittstelle wird auch verwendet, um das asynchrone Cleanup zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-109">An `IAsyncDisposable` interface is also used to enable asynchronous cleanup.</span></span>

## <a name="related-discussion"></a><span data-ttu-id="ea7c9-110">Verwandte Diskussion</span><span class="sxs-lookup"><span data-stu-id="ea7c9-110">Related discussion</span></span>
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a><span data-ttu-id="ea7c9-111">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="ea7c9-111">Detailed design</span></span>
[design]: #detailed-design

## <a name="interfaces"></a><span data-ttu-id="ea7c9-112">Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="ea7c9-112">Interfaces</span></span>

### <a name="iasyncdisposable"></a><span data-ttu-id="ea7c9-113">Iasyncverwerfbare</span><span class="sxs-lookup"><span data-stu-id="ea7c9-113">IAsyncDisposable</span></span>

<span data-ttu-id="ea7c9-114">Es gab viele Diskussionen über `IAsyncDisposable` (z. b. https://github.com/dotnet/roslyn/issues/114) und ob es sich um eine gute Idee handelt.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-114">There has been much discussion of `IAsyncDisposable` (e.g. https://github.com/dotnet/roslyn/issues/114) and whether it's a good idea.</span></span>  <span data-ttu-id="ea7c9-115">Es ist jedoch ein erforderliches Konzept, das für die Unterstützung von asynchronen Iteratoren hinzugefügt werden muss.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-115">However, it's a required concept to add in support of async iterators.</span></span>  <span data-ttu-id="ea7c9-116">Da `finally` Blöcke `await`s enthalten können, und da `finally` Blöcke im Rahmen der Freigabe von Iteratoren ausgeführt werden müssen, ist eine asynchrone Entfernung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-116">Since `finally` blocks may contain `await`s, and since `finally` blocks need to be run as part of disposing of iterators, we need async disposal.</span></span>  <span data-ttu-id="ea7c9-117">Es ist auch in der Regel hilfreich, wenn Ressourcen bereinigt werden können, z. b. das Schließen von Dateien (d. h. Leerungen), das Aufheben von Rückrufen und das Bereitstellen einer Möglichkeit, zu wissen, wann die Registrierung abgeschlossen ist usw.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-117">It's also just generally useful any time cleaning up of resources might take any period of time, e.g. closing files (requiring flushes), deregistering callbacks and providing a way to know when deregistration has completed, etc.</span></span>

<span data-ttu-id="ea7c9-118">Die folgende Schnittstelle wird den .net-Kernbibliotheken (z. b. System. private. Corelib/System. Runtime) hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-118">The following interface is added to the core .NET libraries (e.g. System.Private.CoreLib / System.Runtime):</span></span>
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
<span data-ttu-id="ea7c9-119">Wie bei `Dispose`ist das Aufrufen von `DisposeAsync` mehrmals akzeptabel, und nachfolgende Aufrufe nach dem ersten müssen als nops behandelt werden. dabei wird eine synchron abgeschlossene Aufgabe zurückgegeben (`DisposeAsync` die nicht Thread sicher sein müssen, aber gleichzeitige Aufrufe nicht unterstützen müssen).</span><span class="sxs-lookup"><span data-stu-id="ea7c9-119">As with `Dispose`, invoking `DisposeAsync` multiple times is acceptable, and subsequent invocations after the first should be treated as nops, returning a synchronously completed successful task (`DisposeAsync` need not be thread-safe, though, and need not support concurrent invocation).</span></span>  <span data-ttu-id="ea7c9-120">Darüber hinaus können Typen sowohl `IDisposable` als auch `IAsyncDisposable`implementieren. wenn dies der Fall ist, ist es ebenso akzeptabel, `Dispose` aufzurufen und dann `DisposeAsync` oder umgekehrt, aber nur der erste sollte sinnvoll sein, und nachfolgende Aufrufe von beiden sollten ein NOP sein.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-120">Further, types may implement both `IDisposable` and `IAsyncDisposable`, and if they do, it's similarly acceptable to invoke `Dispose` and then `DisposeAsync` or vice versa, but only the first should be meaningful and subsequent invocations of either should be a nop.</span></span>  <span data-ttu-id="ea7c9-121">Wenn ein Typ beide implementiert, wird empfohlen, nur einmal und nur einmal die relevantere Methode aufzurufen, die auf dem Kontext basiert, `Dispose` in synchronen Kontexten und `DisposeAsync` in asynchronen Kontexten.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-121">As such, if a type does implement both, consumers are encouraged to call once and only once the more relevant method based on the context, `Dispose` in synchronous contexts and `DisposeAsync` in asynchronous ones.</span></span>

<span data-ttu-id="ea7c9-122">(Ich werde die Diskussion über die Interaktion von `IAsyncDisposable` mit `using` in einer separaten Diskussion belassen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-122">(I'm leaving discussion of how `IAsyncDisposable` interacts with `using` to a separate discussion.</span></span>  <span data-ttu-id="ea7c9-123">Und die Abdeckungs Weise, wie Sie mit `foreach` interagiert, wird später in diesem Vorschlag behandelt.)</span><span class="sxs-lookup"><span data-stu-id="ea7c9-123">And coverage of how it interacts with `foreach` is handled later in this proposal.)</span></span>

<span data-ttu-id="ea7c9-124">Berücksichtigte Alternativen:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-124">Alternatives considered:</span></span>
- <span data-ttu-id="ea7c9-125">_`DisposeAsync` akzeptieren einer `CancellationToken`_ : Obwohl es theoretisch Sinn macht, dass alle asynchronen Vorgänge abgebrochen werden können, sind die Bereinigung, das Schließen von Dingen, das Einfrieren von Ressourcen usw., was in der Regel nicht abgebrochen werden sollte. die Bereinigung ist für abgebrochene Arbeit weiterhin wichtig.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-125">_`DisposeAsync` accepting a `CancellationToken`_: while in theory it makes sense that anything async can be canceled, disposal is about cleanup, closing things out, free'ing resources, etc., which is generally not something that should be canceled; cleanup is still important for work that's canceled.</span></span>  <span data-ttu-id="ea7c9-126">Dieselbe `CancellationToken`, die bewirkt hat, dass die tatsächliche Arbeit abgebrochen wurde, ist in der Regel das gleiche Token, das an `DisposeAsync`übergebenen wird, was `DisposeAsync` wertlos ist, weil der Abbruch der Arbeit `DisposeAsync` zu einem NOP führen würde.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-126">The same `CancellationToken` that caused the actual work to be canceled would typically be the same token passed to `DisposeAsync`, making `DisposeAsync` worthless because cancellation of the work would cause `DisposeAsync` to be a nop.</span></span>  <span data-ttu-id="ea7c9-127">Wenn Sie verhindern möchten, dass Sie blockiert werden, warten Sie, bis Sie auf die resultierende `ValueTask`warten, oder warten Sie nur für einen bestimmten Zeitraum.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-127">If someone wants to avoid being blocked waiting for disposal, they can avoid waiting on the resulting `ValueTask`, or wait on it only for some period of time.</span></span>
- <span data-ttu-id="ea7c9-128">_`DisposeAsync` zurückgeben eines `Task`_ : Nachdem eine nicht generische `ValueTask` vorhanden ist, die aus einer `IValueTaskSource`erstellt werden kann, kann durch das Zurückgeben von `ValueTask` von `DisposeAsync` ein vorhandenes Objekt als Zusage wieder verwendet werden, die den asynchronen Abschluss des `DisposeAsync`darstellt, wobei eine `Task` Zuordnung in dem Fall gespeichert wird, in dem `DisposeAsync` asynchron abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-128">_`DisposeAsync` returning a `Task`_: Now that a non-generic `ValueTask` exists and can be constructed from an `IValueTaskSource`, returning `ValueTask` from `DisposeAsync` allows an existing object to be reused as the promise representing the eventual async completion of `DisposeAsync`, saving a `Task` allocation in the case where `DisposeAsync` completes asynchronously.</span></span>
- <span data-ttu-id="ea7c9-129">_Konfigurieren von `DisposeAsync` mit einer `bool continueOnCapturedContext` (`ConfigureAwait`)_ : Es gibt zwar Probleme im Zusammenhang mit der Offenlegung eines solchen Konzepts für `using`, `foreach`und andere Sprachkonstrukte, die diese nutzen. aus Schnittstellen Sicht führt es jedoch keine `await`", und es ist nichts zu konfigurieren... Consumer der `ValueTask` können Sie dennoch nutzen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-129">_Configuring `DisposeAsync` with a `bool continueOnCapturedContext` (`ConfigureAwait`)_: While there may be issues related to how such a concept is exposed to `using`, `foreach`, and other language constructs that consume this, from an interface perspective it's not actually doing any `await`'ing and there's nothing to configure... consumers of the `ValueTask` can consume it however they wish.</span></span>
- <span data-ttu-id="ea7c9-130">_`IAsyncDisposable`, die `IDisposable`erbt_ : da nur eine oder die andere verwendet werden sollte, ist es nicht sinnvoll, die Implementierung beider Typen zu erzwingen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-130">_`IAsyncDisposable` inheriting `IDisposable`_:  Since only one or the other should be used, it doesn't make sense to force types to implement both.</span></span>
- <span data-ttu-id="ea7c9-131">_`IDisposableAsync` statt `IAsyncDisposable`_ : Wir haben die Benennung der "Async"-Methode verfolgt, während Vorgänge "Done Async" lauten, sodass die Typen "Async" als Präfix aufweisen und dass die Methoden "Async" als Suffix aufweisen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-131">_`IDisposableAsync` instead of `IAsyncDisposable`_: We've been following the naming that things/types are an "async something" whereas operations are "done async", so types have "Async" as a prefix and methods have "Async" as a suffix.</span></span>

### <a name="iasyncenumerable--iasyncenumerator"></a><span data-ttu-id="ea7c9-132">Iasyncengerable/iasyncenumschlag</span><span class="sxs-lookup"><span data-stu-id="ea7c9-132">IAsyncEnumerable / IAsyncEnumerator</span></span>

<span data-ttu-id="ea7c9-133">Den .net-Kernbibliotheken werden zwei Schnittstellen hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-133">Two interfaces are added to the core .NET libraries:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> MoveNextAsync();
        T Current { get; }
    }
}
```

<span data-ttu-id="ea7c9-134">Der typische Verbrauch (ohne zusätzliche sprach Features) sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-134">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
        Use(enumerator.Current);
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="ea7c9-135">Verworfene Optionen:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-135">Discarded options considered:</span></span>
- <span data-ttu-id="ea7c9-136">_`Task<bool> MoveNextAsync(); T current { get; }`_ : die Verwendung von `Task<bool>` unterstützt die Verwendung eines zwischengespeicherten Task Objekts zur Darstellung synchroner, erfolgreicher `MoveNextAsync` Aufrufe, aber eine Zuordnung wäre weiterhin für den asynchronen Abschluss erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-136">_`Task<bool> MoveNextAsync(); T current { get; }`_: Using `Task<bool>` would support using a cached task object to represent synchronous, successful `MoveNextAsync` calls, but an allocation would still be required for asynchronous completion.</span></span>  <span data-ttu-id="ea7c9-137">Durch die Rückgabe von `ValueTask<bool>`aktivieren wir das Enumeratorobjekt, damit es `IValueTaskSource<bool>` implementiert und als Unterstützung für die von `MoveNextAsync`zurückgegebenen `ValueTask<bool>` verwendet werden kann, was wiederum einen erheblich reduzierten Overheads ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-137">By returning `ValueTask<bool>`, we enable the enumerator object to itself implement `IValueTaskSource<bool>` and be used as the backing for the `ValueTask<bool>` returned from `MoveNextAsync`, which in turn allows for significantly reduced overheads.</span></span>
- <span data-ttu-id="ea7c9-138">_`ValueTask<(bool, T)> MoveNextAsync();`_ : Es ist nicht nur schwerer zu verwenden, sondern es bedeutet, dass `T` nicht mehr kovariant sein kann.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-138">_`ValueTask<(bool, T)> MoveNextAsync();`_: It's not only harder to consume, but it means that `T` can no longer be covariant.</span></span>
- <span data-ttu-id="ea7c9-139">_`ValueTask<T?> TryMoveNextAsync();`_ : nicht kovariant.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-139">_`ValueTask<T?> TryMoveNextAsync();`_: Not covariant.</span></span>
- <span data-ttu-id="ea7c9-140">_`Task<T?> TryMoveNextAsync();`_ : nicht kovariant, Belegungen bei jedem-Rückruf usw.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-140">_`Task<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="ea7c9-141">_`ITask<T?> TryMoveNextAsync();`_ : nicht kovariant, Belegungen bei jedem-Rückruf usw.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-141">_`ITask<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="ea7c9-142">_`ITask<(bool,T)> TryMoveNextAsync();`_ : nicht kovariant, Belegungen bei jedem-Rückruf usw.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-142">_`ITask<(bool,T)> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="ea7c9-143">_`Task<bool> TryMoveNextAsync(out T result);`_ : das `out` Ergebnis muss festgelegt werden, wenn der Vorgang synchron zurückgegeben wird, und nicht, wenn die Aufgabe asynchron abgeschlossen wird, wenn die Aufgabe möglicherweise irgendwann in der Zukunft abgeschlossen ist. an diesem Punkt gibt es keine Möglichkeit, das Ergebnis zu kommunizieren.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-143">_`Task<bool> TryMoveNextAsync(out T result);`_: The `out` result would need to be set when the operation returns synchronously, not when it asynchronously completes the task potentially sometime long in the future, at which point there'd be no way to communicate the result.</span></span>
- <span data-ttu-id="ea7c9-144">_`IAsyncEnumerator<T>` `IAsyncDisposable`nicht implementiert_ : Wir könnten diese Optionen trennen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-144">_`IAsyncEnumerator<T>` not implementing `IAsyncDisposable`_: We could choose to separate these.</span></span>  <span data-ttu-id="ea7c9-145">Dies erschwert jedoch bestimmte andere Bereiche des Angebots, da Code dann in der Lage sein muss, mit der Möglichkeit zu umgehen, dass ein Enumerator keine Entsorgung bereitstellt. dadurch ist es schwierig, Musterbasierte Hilfsprogramme zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-145">However, doing so complicates certain other areas of the proposal, as code must then be able to deal with the possibility that an enumerator doesn't provide disposal, which makes it difficult to write pattern-based helpers.</span></span>  <span data-ttu-id="ea7c9-146">Außerdem ist es für Enumeratoren üblich, dass Sie zur Verfügung stehen (z. b. einen C# beliebigen Async-Iterator, der über einen letzten Block verfügt, in dem die meisten Elemente von einer Netzwerkverbindung aufgelistet sind usw.), und wenn dies nicht der Fall ist, ist es einfach, die Methode ausschließlich als `public ValueTask DisposeAsync() => default(ValueTask);` mit minimalem zusätzlichen Aufwand zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-146">Further, it will be common for enumerators to have a need for disposal (e.g. any C# async iterator that has a finally block, most things enumerating data from a network connection, etc.), and if one doesn't, it is simple to implement the method purely as `public ValueTask DisposeAsync() => default(ValueTask);` with minimal additional overhead.</span></span>
- <span data-ttu-id="ea7c9-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: kein Abbruch Token-Parameter.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: No cancellation token parameter.</span></span>

#### <a name="viable-alternative"></a><span data-ttu-id="ea7c9-148">Mögliche Alternative:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-148">Viable alternative:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator();
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> WaitForNextAsync();
        T TryGetNext(out bool success);
    }
}
```

<span data-ttu-id="ea7c9-149">`TryGetNext` wird in einer inneren Schleife verwendet, um Elemente mit einem einzelnen Schnittstellen Aufrufwert zu verarbeiten, solange Sie synchron verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-149">`TryGetNext` is used in an inner loop to consume items with a single interface call as long as they're available synchronously.</span></span>  <span data-ttu-id="ea7c9-150">Wenn das nächste Element nicht synchron abgerufen werden kann, wird false zurückgegeben, und jedes Mal, wenn es false zurückgibt, muss ein Aufrufer anschließend `WaitForNextAsync` aufrufen, um entweder darauf zu warten, dass das nächste Element verfügbar ist, oder um festzustellen, dass es nie ein anderes Element gibt.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-150">When the next item can't be retrieved synchronously, it returns false, and any time it returns false, a caller must subsequently invoke `WaitForNextAsync` to either wait for the next item to be available or to determine that there will never be another item.</span></span> <span data-ttu-id="ea7c9-151">Der typische Verbrauch (ohne zusätzliche sprach Features) sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-151">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.WaitForNextAsync())
    {
        while (true)
        {
            int item = enumerator.TryGetNext(out bool success);
            if (!success) break;
            Use(item);
        }
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="ea7c9-152">Der Vorteil besteht darin, dass zwei fache, ein kleiner und ein hauptsächlich:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-152">The advantage of this is two-fold, one minor and one major:</span></span>
- <span data-ttu-id="ea7c9-153">_Neben Version: ermöglicht einem Enumerator, mehrere Consumer zu unterstützen_.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-153">_Minor: Allows for an enumerator to support multiple consumers_.</span></span> <span data-ttu-id="ea7c9-154">Möglicherweise gibt es Szenarien, in denen es für einen Enumerator wichtig ist, mehrere gleichzeitige Consumer zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-154">There may be scenarios where it's valuable for an enumerator to support multiple concurrent consumers.</span></span>  <span data-ttu-id="ea7c9-155">Dies kann nicht erreicht werden, wenn `MoveNextAsync` und `Current` voneinander getrennt sind, sodass eine Implementierung ihre Verwendung nicht atomarisch machen kann.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-155">That can't be achieved when `MoveNextAsync` and `Current` are separate such that an implementation can't make their usage atomic.</span></span>  <span data-ttu-id="ea7c9-156">Im Gegensatz dazu bietet dieser Ansatz eine einzelne Methode `TryGetNext`, die das Übertragen des Enumerators und das nächste Element unterstützt. Daher kann der Enumerator bei Bedarf Atomizität aktivieren.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-156">In contrast, this approach provides a single method `TryGetNext` that supports pushing the enumerator forward and getting the next item, so the enumerator can enable atomicity if desired.</span></span>  <span data-ttu-id="ea7c9-157">Es ist jedoch wahrscheinlich, dass solche Szenarios auch aktiviert werden können, indem jeder Consumer seinen eigenen Enumerator aus einem freigegebenen Aufzähl baren Element bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-157">However, it's likely that such scenarios could also be enabled by giving each consumer its own enumerator from a shared enumerable.</span></span>  <span data-ttu-id="ea7c9-158">Außerdem möchten wir nicht erzwingen, dass jeder Enumerator die gleichzeitige Verwendung unterstützt, da dadurch nicht triviale Aufwand zum Mehrheits Fall hinzugefügt werden, der nicht erforderlich ist. Dies bedeutet, dass sich ein Consumer der Schnittstelle im Allgemeinen nicht auf diese Weise verlassen kann.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-158">Further, we don't want to enforce that every enumerator support concurrent usage, as that would add non-trivial overheads to the majority case that doesn't require it, which means a consumer of the interface generally couldn't rely on this any way.</span></span>
- <span data-ttu-id="ea7c9-159">_Hauptversion: Leistung_.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-159">_Major: Performance_.</span></span> <span data-ttu-id="ea7c9-160">Der `MoveNextAsync`/`Current` Ansatz erfordert zwei Schnittstellen Aufrufe pro Vorgang, während der beste Fall für `WaitForNextAsync`/`TryGetNext` ist, dass die meisten Iterationen synchron ausgeführt werden, was eine enge innere Schleife mit `TryGetNext`ermöglicht, sodass nur ein Schnittstellen Aufruf pro Vorgang durchgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-160">The `MoveNextAsync`/`Current` approach requires two interface calls per operation, whereas the best case for `WaitForNextAsync`/`TryGetNext` is that most iterations complete synchronously, enabling a tight inner loop with `TryGetNext`, such that we only have one interface call per operation.</span></span>  <span data-ttu-id="ea7c9-161">Dies kann in Situationen, in denen die Schnittstellen Aufrufe die Berechnung dominieren, messbare Auswirkungen haben.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-161">This can have a measurable impact in situations where the interface calls dominate the computation.</span></span>

<span data-ttu-id="ea7c9-162">Es gibt jedoch nicht triviale Nachteile, einschließlich einer erheblich größeren Komplexität, wenn diese manuell genutzt werden, und eine größere Chance, bei deren Verwendung Fehler zu verursachen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-162">However, there are non-trivial downsides, including significantly increased complexity when consuming these manually, and an increased chance of introducing bugs when using them.</span></span>  <span data-ttu-id="ea7c9-163">Und während die Leistungsvorteile in den Mikrobenchmarks angezeigt werden, glauben wir nicht, dass Sie sich auf den größten Teil der realen Verwendung auswirken werden.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-163">And while the performance benefits show up in microbenchmarks, we don't believe they'll be impactful in the vast majority of real usage.</span></span>  <span data-ttu-id="ea7c9-164">Wenn sich dies herausstellt, können wir einen zweiten Satz von Schnittstellen in einer hellen Weise einführen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-164">If it turns out they are, we can introduce a second set of interfaces in a light-up fashion.</span></span>

<span data-ttu-id="ea7c9-165">Verworfene Optionen:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-165">Discarded options considered:</span></span>
- <span data-ttu-id="ea7c9-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` Parameter können nicht kovariant sein.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` parameters can't be covariant.</span></span>  <span data-ttu-id="ea7c9-167">Hier gibt es auch eine kleine Auswirkung (ein Problem mit dem try-Muster im allgemeinen), dass dies wahrscheinlich eine Lauf Zeit Schreib Barriere für Verweistyp Ergebnisse verursacht.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-167">There's also a small impact here (an issue with the try pattern in general) that this likely incurs a runtime write barrier for reference type results.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="ea7c9-168">Abbruch</span><span class="sxs-lookup"><span data-stu-id="ea7c9-168">Cancellation</span></span>

<span data-ttu-id="ea7c9-169">Es gibt mehrere mögliche Ansätze zur Unterstützung von Abbruch:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-169">There are several possible approaches to supporting cancellation:</span></span>
1. <span data-ttu-id="ea7c9-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` sind Abbruch agnostisch: `CancellationToken` nicht an einer beliebigen Stelle angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` are cancellation-agnostic: `CancellationToken` doesn't appear anywhere.</span></span>  <span data-ttu-id="ea7c9-171">Der Abbruch wird erreicht, indem die `CancellationToken` logisch in den Aufzähl Bare-und/oder-Enumerator gebackt wird. Dies ist z. b. der Fall, wenn ein Iterator aufgerufen wird, der `CancellationToken` als Argument an die Iteratormethode übergeben und im Text des Iterators verwendet wird, wie es bei einem anderen Parameter der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-171">Cancellation is achieved by logically baking the `CancellationToken` into the enumerable and/or enumerator in whatever manner is appropriate, e.g. when calling an iterator, passing the `CancellationToken` as an argument to the iterator method and using it in the body of the iterator, as is done with any other parameter.</span></span>
2. <span data-ttu-id="ea7c9-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: Sie übergeben eine `CancellationToken` an `GetAsyncEnumerator`, und nachfolgende `MoveNextAsync` Vorgänge berücksichtigen dies, dies kann jedoch der Fall sein.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: You pass a `CancellationToken` to `GetAsyncEnumerator`, and subsequent `MoveNextAsync` operations respect it however it can.</span></span>
3. <span data-ttu-id="ea7c9-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: übergeben Sie eine `CancellationToken` an jeden einzelnen `MoveNextAsync`-Befehl.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: You pass a `CancellationToken` to each individual `MoveNextAsync` call.</span></span>
4. <span data-ttu-id="ea7c9-174">1 & & 2: Sie Betten `CancellationToken`s in ihren Enumerable/Enumerator ein und übergeben `CancellationToken`s an `GetAsyncEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-174">1 && 2: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `GetAsyncEnumerator`.</span></span>
5. <span data-ttu-id="ea7c9-175">1 & & 3: Sie Betten `CancellationToken`s in ihren Enumerable/Enumerator ein und übergeben `CancellationToken`s an `MoveNextAsync`.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-175">1 && 3: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `MoveNextAsync`.</span></span>

<span data-ttu-id="ea7c9-176">Aus rein theoretischen Sicht ist (5) das stabilste, da (a) `MoveNextAsync` das akzeptieren einer `CancellationToken` die präzisere Kontrolle über das abgebrochene ermöglicht, und (b) `CancellationToken` ist nur ein beliebiger anderer Typ, der als Argument an Iteratoren übergeben werden kann, die in beliebige Typen eingebettet sind, usw.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-176">From a purely theoretical perspective, (5) is the most robust, in that (a) `MoveNextAsync` accepting a `CancellationToken` enables the most fine-grained control over what's canceled, and (b) `CancellationToken` is just any other type that can passed as an argument into iterators, embedded in arbitrary types, etc.</span></span>

<span data-ttu-id="ea7c9-177">Bei diesem Ansatz gibt es jedoch mehrere Probleme:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-177">However, there are multiple problems with that approach:</span></span>
- <span data-ttu-id="ea7c9-178">Wie wird ein `CancellationToken` an `GetAsyncEnumerator` an den Text des Iterators geleitet?</span><span class="sxs-lookup"><span data-stu-id="ea7c9-178">How does a `CancellationToken` passed to `GetAsyncEnumerator` make it into the body of the iterator?</span></span>  <span data-ttu-id="ea7c9-179">Wir könnten ein neues `iterator`-Schlüsselwort verfügbar machen, von dem Sie einen Punkt abrufen können, um Zugriff auf die an `GetEnumerator``CancellationToken` zu erhalten. a) das ist aber eine Menge zusätzlicher Maschinen, b) wir machen es zu einem erstklassigen Bürger und c) der Fall von 99% scheint derselbe Code zu sein, der einen Iterator aufruft und `GetAsyncEnumerator` darauf aufruft. in diesem Fall kann er einfach die `CancellationToken` als Argument an die-Methode übergeben.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-179">We could expose a new `iterator` keyword that you could dot off of to get access to the `CancellationToken` passed to `GetEnumerator`, but a) that's a lot of additional machinery, b) we're making it a very first-class citizen, and c) the 99% case would seem to be the same code both calling an iterator and calling `GetAsyncEnumerator` on it, in which case it can just pass the `CancellationToken` as an argument into the method.</span></span>
- <span data-ttu-id="ea7c9-180">Wie wird ein `CancellationToken` an `MoveNextAsync` an den Text der Methode geleitet?</span><span class="sxs-lookup"><span data-stu-id="ea7c9-180">How does a `CancellationToken` passed to `MoveNextAsync` get into the body of the method?</span></span>  <span data-ttu-id="ea7c9-181">Dies ist noch schlimmer, denn wenn Sie von einem `iterator` lokalen Objekt verfügbar gemacht wird, kann sich der Wert über erwartungsgemäß ändern. Dies bedeutet, dass jeder Code, der mit dem Token registriert ist, die Registrierung bei ihm vor den Vorgängen aufheben und anschließend erneut registrieren muss. Es ist auch möglich, dass die Registrierung und die Aufhebung der Registrierung in jedem `MoveNextAsync`-Befehl durchgeführt werden müssen, unabhängig davon, ob der Compiler manuell in einem Iterator oder einem Entwickler implementiert ist.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-181">This is even worse, as if it's exposed off of an `iterator` local object, its value could change across awaits, which means any code that registered with the token would need to unregister from it prior to awaits and then re-register after; it's also potentially quite expensive to need to do such registering and unregistering in every `MoveNextAsync` call, regardless of whether implemented by the compiler in an iterator or by a developer manually.</span></span>
- <span data-ttu-id="ea7c9-182">Wie wird ein Entwickler eine `foreach` Schleife Abbrechen?</span><span class="sxs-lookup"><span data-stu-id="ea7c9-182">How does a developer cancel a `foreach` loop?</span></span>  <span data-ttu-id="ea7c9-183">Wenn dies geschieht, indem ein `CancellationToken` an einen Aufzähl Bare/Enumerator übergeben wird, dann eine), müssen wir `foreach`"over Enumeratoren unterstützen, die Sie als erstklassige Bürger auslösen. und jetzt müssen Sie sich über ein Ökosystem Gedanken machen, das auf Enumeratoren aufbaut (z. b. LINQ-Methoden) oder b) Wir müssen die `CancellationToken` trotzdem in das Aufzähl Bare Element einbetten, indem wir einige `WithCancellation` Erweiterungsmethode außerhalb von `IAsyncEnumerable<T>` haben, in der das bereitgestellte Token gespeichert wird. `GetAsyncEnumerator` `GetAsyncEnumerator` wird aufgerufen (dieses Token wird ignoriert).</span><span class="sxs-lookup"><span data-stu-id="ea7c9-183">If it's done by giving a `CancellationToken` to an enumerable/enumerator, then either a) we need to support `foreach`'ing over enumerators, which raises them to being first-class citizens, and now you need to start thinking about an ecosystem built up around enumerators (e.g. LINQ methods) or b) we need to embed the `CancellationToken` in the enumerable anyway by having some `WithCancellation` extension method off of `IAsyncEnumerable<T>` that would store the provided token and then pass it into  the wrapped enumerable's `GetAsyncEnumerator` when the `GetAsyncEnumerator` on the returned struct is invoked (ignoring that token).</span></span>  <span data-ttu-id="ea7c9-184">Oder Sie können einfach die `CancellationToken` verwenden, die Sie im Text von foreach haben.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-184">Or, you can just use the `CancellationToken` you have in the body of the foreach.</span></span>
- <span data-ttu-id="ea7c9-185">Wenn/wenn Abfrage Ausdrücke unterstützt werden, wie soll die `CancellationToken` an `GetEnumerator` oder `MoveNextAsync` an jede Klausel übergeben werden?</span><span class="sxs-lookup"><span data-stu-id="ea7c9-185">If/when query comprehensions are supported, how would the `CancellationToken` supplied to `GetEnumerator` or `MoveNextAsync` be passed into each clause?</span></span>  <span data-ttu-id="ea7c9-186">Am einfachsten wäre es, wenn die-Klausel die-Klausel aufzeichnen soll. an dieser Stelle wird das Token an `GetAsyncEnumerator`/`MoveNextAsync` ignoriert.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-186">The easiest way would simply be for the clause to capture it, at which point whatever token is passed to `GetAsyncEnumerator`/`MoveNextAsync` is ignored.</span></span>

<span data-ttu-id="ea7c9-187">Eine frühere Version dieses Dokuments wurde empfohlen (1), wir haben jedoch zu (4) gewechselt.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-187">An earlier version of this document recommended (1), but we since switched to (4).</span></span>

<span data-ttu-id="ea7c9-188">Die zwei Hauptprobleme bei (1):</span><span class="sxs-lookup"><span data-stu-id="ea7c9-188">The two main problems with (1):</span></span>
- <span data-ttu-id="ea7c9-189">Producer von abbrechbaren Enumerationen müssen einige Bausteine implementieren und können nur die Unterstützung des Compilers für Async-Iteratoren nutzen, um eine `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` Methode zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-189">producers of cancellable enumerables have to implement some boilerplate, and can only leverage the compiler's support for async-iterators to implement a `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` method.</span></span>
- <span data-ttu-id="ea7c9-190">Es ist wahrscheinlich, dass viele Producer einfach einen `CancellationToken` Parameter zu ihrer asynchronen Enumerable-Signatur hinzufügen würden, wodurch verhindert wird, dass Consumer das gewünschte Abbruch Token übergeben, wenn Sie einen `IAsyncEnumerable` Typ erhalten.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-190">it is likely that many producers would be tempted to just add a `CancellationToken` parameter to their async-enumerable signature instead, which will prevent consumers from passing the cancellation token they want when they are given an `IAsyncEnumerable` type.</span></span>

<span data-ttu-id="ea7c9-191">Es gibt zwei Haupt Verwendungs Szenarien:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-191">There are two main consumption scenarios:</span></span>
1. <span data-ttu-id="ea7c9-192">`await foreach (var i in GetData(token)) ...`, in dem der Consumer die Async-Iterator-Methode aufruft,</span><span class="sxs-lookup"><span data-stu-id="ea7c9-192">`await foreach (var i in GetData(token)) ...` where the consumer calls the async-iterator method,</span></span>
2. <span data-ttu-id="ea7c9-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...`, in dem der Consumer eine bestimmte `IAsyncEnumerable` Instanz behandelt.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` where the consumer deals with a given `IAsyncEnumerable` instance.</span></span>

<span data-ttu-id="ea7c9-194">Wir stellen fest, dass ein angemessener Kompromiss zur Unterstützung beider Szenarien auf eine Weise, die sowohl für Producer als auch für Consumer von asynchronen Streams geeignet ist, die Verwendung eines speziell mit Anmerkungen versehene Parameters in der Async-Iterator-Methode ist.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-194">We find that a reasonable compromise to support both scenarios in a way that is convenient for both producers and consumers of async-streams is to use a specially annotated parameter in the async-iterator method.</span></span> <span data-ttu-id="ea7c9-195">Zu diesem Zweck wird das `[EnumeratorCancellation]`-Attribut verwendet.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-195">The `[EnumeratorCancellation]` attribute is used for this purpose.</span></span> <span data-ttu-id="ea7c9-196">Das Platzieren dieses Attributs für einen Parameter weist den Compiler an, dass, wenn ein Token an die `GetAsyncEnumerator` Methode übergeben wird, dieses Token anstelle des ursprünglich für den-Parameter übergebenen Werts verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-196">Placing this attribute on a parameter tells the compiler that if a token is passed to the `GetAsyncEnumerator` method, that token should be used instead of the value originally passed for the parameter.</span></span>

<span data-ttu-id="ea7c9-197">Gehen Sie von `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)` aus.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-197">Consider `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`.</span></span> <span data-ttu-id="ea7c9-198">Der Implementierer dieser Methode kann einfach den-Parameter im Methoden Text verwenden.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-198">The implementer of this method can simply use the parameter in the method body.</span></span> <span data-ttu-id="ea7c9-199">Der Consumer kann entweder die folgenden Verbrauchsmuster verwenden:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-199">The consumer can use either consumption patterns above:</span></span>
1. <span data-ttu-id="ea7c9-200">Wenn Sie `GetData(token)`verwenden, wird das Token in "Async-Enumerable" gespeichert und in der Iterations Gruppe verwendet.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-200">if you use `GetData(token)`, then the token is saved into the async-enumerable and will be used in iteration,</span></span>
2. <span data-ttu-id="ea7c9-201">Wenn Sie `givenIAsyncEnumerable.WithCancellation(token)`verwenden, ersetzt das an `GetAsyncEnumerator` übergebenen Token alle Token, die im Async-Enumerable-Element gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-201">if you use `givenIAsyncEnumerable.WithCancellation(token)`, then the token passed to `GetAsyncEnumerator` will supersede any token saved in the async-enumerable.</span></span>

## <a name="foreach"></a><span data-ttu-id="ea7c9-202">foreach</span><span class="sxs-lookup"><span data-stu-id="ea7c9-202">foreach</span></span>

<span data-ttu-id="ea7c9-203">`foreach` wird erweitert, um `IAsyncEnumerable<T>` zusätzlich zu seiner vorhandenen Unterstützung für `IEnumerable<T>`zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-203">`foreach` will be augmented to support `IAsyncEnumerable<T>` in addition to its existing support for `IEnumerable<T>`.</span></span>  <span data-ttu-id="ea7c9-204">Außerdem wird die Entsprechung von `IAsyncEnumerable<T>` als Muster unterstützt, wenn die relevanten Member öffentlich verfügbar gemacht werden, wobei die Schnittstelle direkt verwendet wird, wenn dies nicht der Fall ist, um auf struct basierende Erweiterungen zu aktivieren, die die Zuordnung und die Verwendung alternativer "awaitables" als Rückgabetyp von `MoveNextAsync` und `DisposeAsync`vermeiden.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-204">And it will support the equivalent of `IAsyncEnumerable<T>` as a pattern if the relevant members are exposed publicly, falling back to using the interface directly if not, in order to enable struct-based extensions that avoid allocating as well as using alternative awaitables as the return type of `MoveNextAsync` and `DisposeAsync`.</span></span>

### <a name="syntax"></a><span data-ttu-id="ea7c9-205">Syntax</span><span class="sxs-lookup"><span data-stu-id="ea7c9-205">Syntax</span></span>

<span data-ttu-id="ea7c9-206">Verwenden Sie die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-206">Using the syntax:</span></span>

```csharp
foreach (var i in enumerable)
```

<span data-ttu-id="ea7c9-207">C#behandelt `enumerable` weiterhin als synchrones Aufzähl bares Element, sodass selbst dann, wenn die relevanten APIs für asynchrone Enumerables verfügbar gemacht werden (das Muster verfügbar gemacht oder die Schnittstelle implementiert wird), nur die synchronen APIs berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-207">C# will continue to treat `enumerable` as a synchronous enumerable, such that even if it exposes the relevant APIs for async enumerables (exposing the pattern or implementing the interface), it will only consider the synchronous APIs.</span></span>

<span data-ttu-id="ea7c9-208">Um `foreach` zu erzwingen, dass Sie stattdessen nur die asynchronen APIs berücksichtigt, wird `await` wie folgt eingefügt:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-208">To force `foreach` to instead only consider the asynchronous APIs, `await` is inserted as follows:</span></span>

```csharp
await foreach (var i in enumerable)
```

<span data-ttu-id="ea7c9-209">Es wurde keine Syntax bereitgestellt, die die Verwendung der Async-oder Sync-APIs unterstützt. der Entwickler muss basierend auf der verwendeten Syntax auswählen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-209">No syntax would be provided that would support using either the async or the sync APIs; the developer must choose based on the syntax used.</span></span>

<span data-ttu-id="ea7c9-210">Verworfene Optionen:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-210">Discarded options considered:</span></span>
- <span data-ttu-id="ea7c9-211">_`foreach (var i in await enumerable)`_ : Dies ist bereits eine gültige Syntax, und das Ändern der Bedeutung wäre eine Breaking Change.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-211">_`foreach (var i in await enumerable)`_: This is already valid syntax, and changing its meaning would be a breaking change.</span></span>  <span data-ttu-id="ea7c9-212">Dies bedeutet, dass Sie die `enumerable``await`, eine synchrone Iterable von der Datei erhalten und diese dann synchron durchlaufen können.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-212">This means to `await` the `enumerable`, get back something synchronously iterable from it, and then synchronously iterate through that.</span></span>
- <span data-ttu-id="ea7c9-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)``foreach (await var i in enumerable)`_ : Dies deutet darauf hin, dass wir auf das nächste Element warten, aber es gibt noch weitere Warteschlangen, die sich an foreach beteiligen, insbesondere, wenn das Aufzähl Bare Element ein `IAsyncDisposable`ist, wird die asynchrone Entfernung `await`.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)`, `foreach (await var i in enumerable)`_: These all suggest that we're awaiting the next item, but there are other awaits involved in foreach, in particular if the enumerable is an `IAsyncDisposable`, we will be `await`'ing its async disposal.</span></span>  <span data-ttu-id="ea7c9-214">Der erwartungsgemäß ist der Bereich von foreach und nicht für jedes einzelne Element, weshalb das `await`-Schlüsselwort auf `foreach` Ebene liegen muss.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-214">That await is as the scope of the foreach rather than for each individual element, and thus the `await` keyword deserves to be at the `foreach` level.</span></span>  <span data-ttu-id="ea7c9-215">Außerdem bietet uns eine Möglichkeit, die `foreach` mit einem anderen Begriff zu beschreiben, wenn Sie mit dem `foreach` verknüpft ist, z. b. "warten auf foreach".</span><span class="sxs-lookup"><span data-stu-id="ea7c9-215">Further, having it associated with the `foreach` gives us a way to describe the `foreach` with a different term, e.g. a "await foreach".</span></span>  <span data-ttu-id="ea7c9-216">Noch wichtiger ist jedoch, `foreach` Syntax zur gleichen Zeit wie `using` Syntax zu berücksichtigen, sodass Sie konsistent zueinander bleiben und `using (await ...)` bereits gültige Syntax ist.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-216">But more importantly, there's value in considering `foreach` syntax at the same time as `using` syntax, so that they remain consistent with each other, and `using (await ...)` is already valid syntax.</span></span>
- _`foreach await (var i in enumerable)`_

<span data-ttu-id="ea7c9-217">Beachten Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-217">Still to consider:</span></span>
- <span data-ttu-id="ea7c9-218">`foreach` heute unterstützt keine Iteration durch einen Enumerator.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-218">`foreach` today does not support iterating through an enumerator.</span></span>  <span data-ttu-id="ea7c9-219">Wir gehen davon aus, dass es häufiger `IAsyncEnumerator<T>`s gibt, und daher ist es verlockend, `await foreach` mit `IAsyncEnumerable<T>` und `IAsyncEnumerator<T>`zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-219">We expect it will be more common to have `IAsyncEnumerator<T>`s handed around, and thus it's tempting to support `await foreach` with both `IAsyncEnumerable<T>` and `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="ea7c9-220">Nachdem wir diese Unterstützung hinzugefügt haben, wird die Frage gestellt, ob `IAsyncEnumerator<T>` ein erstklassiger Bürger ist, und ob Sie über über Ladungen von combinatoren verfügen müssen, die auf Enumeratoren neben Enumerationen angewendet werden?</span><span class="sxs-lookup"><span data-stu-id="ea7c9-220">But once we add such support, it introduces the question of whether `IAsyncEnumerator<T>` is a first-class citizen, and whether we need to have overloads of combinators that operate on enumerators in addition to enumerables?</span></span>    <span data-ttu-id="ea7c9-221">Möchten wir Methoden zum Zurückgeben von Enumeratoren anstelle von Enumerables ermutigen?</span><span class="sxs-lookup"><span data-stu-id="ea7c9-221">Do we want to encourage methods to return enumerators rather than enumerables?</span></span> <span data-ttu-id="ea7c9-222">Wir sollten dies weiterhin besprechen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-222">We should continue to discuss this.</span></span>  <span data-ttu-id="ea7c9-223">Wenn wir entscheiden, dass wir Sie nicht unterstützen möchten, können wir eine Erweiterungsmethode `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` einführen, mit der ein Enumerator weiterhin `foreach`d werden kann.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-223">If we decide we don't want to support it, we might want to introduce an extension method `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` that would allow an enumerator to still be `foreach`'d.</span></span>  <span data-ttu-id="ea7c9-224">Wenn wir uns entscheiden, dies zu unterstützen, müssen wir auch entscheiden, ob die `await foreach` für den Aufruf von `DisposeAsync` auf dem Enumerator zuständig wäre, und die Antwort ist wahrscheinlich "Nein, die Kontrolle über die Freigabe sollte von jedem Benutzer behandelt werden, der `GetEnumerator`aufgerufen wurde."</span><span class="sxs-lookup"><span data-stu-id="ea7c9-224">If we decide we do want to support it, we'll need to also decide on whether the `await foreach` would be responsible for calling `DisposeAsync` on the enumerator, and the answer is likely "no, control over disposal should be handled by whoever called `GetEnumerator`."</span></span>

### <a name="pattern-based-compilation"></a><span data-ttu-id="ea7c9-225">Musterbasierte Kompilierung</span><span class="sxs-lookup"><span data-stu-id="ea7c9-225">Pattern-based Compilation</span></span>

<span data-ttu-id="ea7c9-226">Der Compiler bindet eine Bindung an die musterbasierten APIs, wenn Sie vorhanden sind, und bevorzugt diese für die Verwendung der-Schnittstelle (das Muster ist möglicherweise mit Instanzmethoden oder Erweiterungs Methoden erfüllt).</span><span class="sxs-lookup"><span data-stu-id="ea7c9-226">The compiler will bind to the pattern-based APIs if they exist, preferring those over using the interface (the pattern may be satisfied with instance methods or extension methods).</span></span>  <span data-ttu-id="ea7c9-227">Folgende Anforderungen gelten für das Muster:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-227">The requirements for the pattern are:</span></span>
- <span data-ttu-id="ea7c9-228">Das Aufzähl Bare Element muss eine `GetAsyncEnumerator` Methode verfügbar machen, die ohne Argumente aufgerufen werden kann und einen Enumerator zurückgibt, der das relevante Muster erfüllt.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-228">The enumerable must expose a `GetAsyncEnumerator` method that may be called with no arguments and that returns an enumerator that meets the relevant pattern.</span></span>
- <span data-ttu-id="ea7c9-229">Der Enumerator muss eine `MoveNextAsync` Methode verfügbar machen, die ohne Argumente aufgerufen werden kann und einen Wert zurückgibt, der möglicherweise `await`Ed ist und dessen `GetResult()` eine `bool`zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-229">The enumerator must expose a `MoveNextAsync` method that may be called with no arguments and that returns something which may be `await`ed and whose `GetResult()` returns a `bool`.</span></span>
- <span data-ttu-id="ea7c9-230">Der Enumerator muss auch `Current`-Eigenschaft verfügbar machen, deren Getter eine `T` zurückgibt, die die Art der aufzuzählenden Daten darstellt.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-230">The enumerator must also expose `Current` property whose getter returns a `T` representing the kind of data being enumerated.</span></span>
- <span data-ttu-id="ea7c9-231">Der Enumerator kann optional eine `DisposeAsync` Methode verfügbar machen, die ohne Argumente aufgerufen werden kann, und die etwas zurückgibt, das `await`Ed und dessen `GetResult()` `void`zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-231">The enumerator may optionally expose a `DisposeAsync` method that may be invoked with no arguments and that returns something that can be `await`ed and whose `GetResult()` returns `void`.</span></span>

<span data-ttu-id="ea7c9-232">Dieser Code:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-232">This code:</span></span>

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

<span data-ttu-id="ea7c9-233">wird in die-Entsprechung von übersetzt:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-233">is translated to the equivalent of:</span></span>

```csharp
var enumerable = ...;
var enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
       T item = enumerator.Current;
       ...
    }
}
finally
{
    await enumerator.DisposeAsync(); // omitted, along with the try/finally, if the enumerator doesn't expose DisposeAsync
}
```

<span data-ttu-id="ea7c9-234">Wenn der Iterierte Typ das richtige Muster nicht verfügbar macht, werden die Schnittstellen verwendet.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-234">If the iterated type doesn't expose the right pattern, the interfaces will be used.</span></span>

### <a name="configureawait"></a><span data-ttu-id="ea7c9-235">"Configureawait"</span><span class="sxs-lookup"><span data-stu-id="ea7c9-235">ConfigureAwait</span></span>

<span data-ttu-id="ea7c9-236">Diese Musterbasierte Kompilierung ermöglicht die Verwendung von `ConfigureAwait` für alle über eine `ConfigureAwait` Erweiterungsmethode:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-236">This pattern-based compilation will allow `ConfigureAwait` to be used on all of the awaits, via a `ConfigureAwait` extension method:</span></span>

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

<span data-ttu-id="ea7c9-237">Dies basiert auf den Typen, die wir .NET hinzufügen, wahrscheinlich auch System. Threading. Tasks. Extensions. dll:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-237">This will be based on types we'll add to .NET as well, likely to System.Threading.Tasks.Extensions.dll:</span></span>

```csharp
// Approximate implementation, omitting arg validation and the like
namespace System.Threading.Tasks
{
    public static class AsyncEnumerableExtensions
    {
        public static ConfiguredAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext) =>
            new ConfiguredAsyncEnumerable<T>(enumerable, continueOnCapturedContext);

        public struct ConfiguredAsyncEnumerable<T>
        {
            private readonly IAsyncEnumerable<T> _enumerable;
            private readonly bool _continueOnCapturedContext;

            internal ConfiguredAsyncEnumerable(IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext)
            {
                _enumerable = enumerable;
                _continueOnCapturedContext = continueOnCapturedContext;
            }

            public ConfiguredAsyncEnumerator<T> GetAsyncEnumerator() =>
                new ConfiguredAsyncEnumerator<T>(_enumerable.GetAsyncEnumerator(), _continueOnCapturedContext);

            public struct Enumerator
            {
                private readonly IAsyncEnumerator<T> _enumerator;
                private readonly bool _continueOnCapturedContext;

                internal Enumerator(IAsyncEnumerator<T> enumerator, bool continueOnCapturedContext)
                {
                    _enumerator = enumerator;
                    _continueOnCapturedContext = continueOnCapturedContext;
                }

                public ConfiguredValueTaskAwaitable<bool> MoveNextAsync() =>
                    _enumerator.MoveNextAsync().ConfigureAwait(_continueOnCapturedContext);

                public T Current => _enumerator.Current;

                public ConfiguredValueTaskAwaitable DisposeAsync() =>
                    _enumerator.DisposeAsync().ConfigureAwait(_continueOnCapturedContext);
            }
        }
    }
}
```

<span data-ttu-id="ea7c9-238">Beachten Sie, dass dieser Ansatz nicht die Verwendung von `ConfigureAwait` mit musterbasierten Enumerables ermöglicht. aber auch hier ist es bereits der Fall, dass der `ConfigureAwait` nur als Erweiterung auf `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` verfügbar gemacht wird und nicht auf willkürliche, nicht aufnutzbare Dinge angewendet werden kann, da er nur bei der Anwendung auf Tasks sinnvoll ist (er steuert ein Verhalten, das in der Fortsetzungs Unterstützung der Aufgabe implementiert ist). Daher ist es nicht sinnvoll, wenn ein Muster verwendet wird</span><span class="sxs-lookup"><span data-stu-id="ea7c9-238">Note that this approach will not enable `ConfigureAwait` to be used with pattern-based enumerables, but then again it's already the case that the `ConfigureAwait` is only exposed as an extension on `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` and can't be applied to arbitrary awaitable things, as it only makes sense when applied to Tasks (it controls a behavior implemented in Task's continuation support), and thus doesn't make sense when using a pattern where the awaitable things may not be tasks.</span></span>  <span data-ttu-id="ea7c9-239">Jede Person, die überwachte Dinge zurückgibt, kann Ihr eigenes benutzerdefiniertes Verhalten in solchen erweiterten Szenarien bereitstellen</span><span class="sxs-lookup"><span data-stu-id="ea7c9-239">Anyone returning awaitable things can provide their own custom behavior in such advanced scenarios.</span></span>

<span data-ttu-id="ea7c9-240">(Wenn es eine Möglichkeit gibt, eine `ConfigureAwait` Lösung auf Bereichs-oder Assemblyebene zu unterstützen, ist dies nicht erforderlich.)</span><span class="sxs-lookup"><span data-stu-id="ea7c9-240">(If we can come up with some way to support a scope- or assembly-level `ConfigureAwait` solution, then this won't be necessary.)</span></span>

## <a name="async-iterators"></a><span data-ttu-id="ea7c9-241">Async-Iteratoren</span><span class="sxs-lookup"><span data-stu-id="ea7c9-241">Async Iterators</span></span>

<span data-ttu-id="ea7c9-242">Die Sprache/der Compiler unterstützt die Erstellung von `IAsyncEnumerable<T>`s und `IAsyncEnumerator<T>`s zusätzlich zu deren Nutzung.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-242">The language / compiler will support producing `IAsyncEnumerable<T>`s and `IAsyncEnumerator<T>`s in addition to consuming them.</span></span> <span data-ttu-id="ea7c9-243">Heute unterstützt die Sprache das Schreiben eines Iterators wie:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-243">Today the language supports writing an iterator like:</span></span>

```csharp
static IEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            Thread.Sleep(1000);
            yield return i;
        }
    }
    finally
    {
        Thread.Sleep(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="ea7c9-244">`await` kann jedoch nicht im Text dieser Iteratoren verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-244">but `await` can't be used in the body of these iterators.</span></span>  <span data-ttu-id="ea7c9-245">Wir werden diese Unterstützung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-245">We will add that support.</span></span>

### <a name="syntax"></a><span data-ttu-id="ea7c9-246">Syntax</span><span class="sxs-lookup"><span data-stu-id="ea7c9-246">Syntax</span></span>

<span data-ttu-id="ea7c9-247">Die vorhandene Sprachunterstützung für Iteratoren leitet die iteratorart der Methode basierend darauf ab, ob Sie `yield`s enthält.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-247">The existing language support for iterators infers the iterator nature of the method based on whether it contains any `yield`s.</span></span>  <span data-ttu-id="ea7c9-248">Das gleiche gilt für Async-Iteratoren.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-248">The same will be true for async iterators.</span></span>  <span data-ttu-id="ea7c9-249">Solche Async-Iteratoren werden durch das Hinzufügen von `async` zur Signatur abgegrenzt und unterscheiden sich von synchronen Iteratoren. Außerdem muss entweder `IAsyncEnumerable<T>` oder `IAsyncEnumerator<T>` als Rückgabetyp verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-249">Such async iterators will be demarcated and differentiated from synchronous iterators via adding `async` to the signature, and must then also have either `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` as its return type.</span></span>  <span data-ttu-id="ea7c9-250">Das obige Beispiel könnte z. b. wie folgt als Async-Iterator geschrieben werden:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-250">For example, the above example could be written as an async iterator as follows:</span></span>

```csharp
static async IAsyncEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            await Task.Delay(1000);
            yield return i;
        }
    }
    finally
    {
        await Task.Delay(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="ea7c9-251">Berücksichtigte Alternativen:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-251">Alternatives considered:</span></span>
- <span data-ttu-id="ea7c9-252">Die Verwendung von _`async` in der Signatur wird nicht_verwendet: die Verwendung `async` ist für den Compiler wahrscheinlich technisch erforderlich, da er verwendet wird, um zu bestimmen, ob `await` in diesem Kontext gültig ist.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-252">_Not using `async` in the signature_: Using `async` is likely technically required by the compiler, as it uses it to determine whether `await` is valid in that context.</span></span>  <span data-ttu-id="ea7c9-253">Aber auch wenn dies nicht erforderlich ist, haben wir festgestellt, dass `await` nur in Methoden verwendet werden können, die als `async`gekennzeichnet sind, und es ist wichtig, die Konsistenz zu wahren.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-253">But even if it's not required, we've established that `await` may only be used in methods marked as `async`, and it seems important to keep the consistency.</span></span>
- <span data-ttu-id="ea7c9-254">_Aktivieren benutzerdefinierter Generatoren für `IAsyncEnumerable<T>`_ : das ist etwas, das wir uns für die Zukunft ansehen konnten, aber die Technik ist kompliziert, und wir unterstützen das nicht für die synchronen Gegenstücke.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-254">_Enabling custom builders for `IAsyncEnumerable<T>`_:  That's something we could look at for the future, but the machinery is complicated and we don't support that for the synchronous counterparts.</span></span>
- <span data-ttu-id="ea7c9-255">_Ein `iterator`-Schlüsselwort in der Signatur_: Async-Iteratoren verwenden `async iterator` in der Signatur, und `yield` können nur in `async` Methoden verwendet werden, die `iterator`enthalten. `iterator` dann in synchronen Iteratoren optional gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-255">_Having an `iterator` keyword in the signature_: Async iterators would use `async iterator` in the signature, and `yield` could only be used in `async` methods that included `iterator`; `iterator` would then be made optional on synchronous iterators.</span></span>  <span data-ttu-id="ea7c9-256">Abhängig von ihrer Perspektive hat dies den Vorteil, dass es durch die Signatur der-Methode ganz klar ist, ob `yield` zulässig ist und ob die Methode tatsächlich zum Zurückgeben von Instanzen des Typs `IAsyncEnumerable<T>` gedacht ist, anstatt die compilerfertigung, je nachdem, ob der Code `yield` verwendet oder nicht.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-256">Depending on your perspective, this has the benefit of making it very clear by the signature of the method whether `yield` is allowed and whether the method is actually meant to return instances of type `IAsyncEnumerable<T>` rather than the compiler manufacturing one based on whether the code uses `yield` or not.</span></span>  <span data-ttu-id="ea7c9-257">Sie unterscheidet sich jedoch von synchronen Iteratoren, die nicht für eine Anforderung erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-257">But it is different from synchronous iterators, which don't and can't be made to require one.</span></span>  <span data-ttu-id="ea7c9-258">Außerdem sind einige Entwickler nicht der zusätzlichen Syntax gefallen.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-258">Plus some developers don't like the extra syntax.</span></span>  <span data-ttu-id="ea7c9-259">Wenn wir Sie von Grund auf neu entwerfen, wäre dies wahrscheinlich erforderlich, aber an diesem Punkt gibt es noch viel mehr Wert, wenn Sie asynchrone Iteratoren in der Nähe der Synchronisierungs Iteratoren halten.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-259">If we were designing it from scratch, we'd probably make this required, but at this point there's much more value in keeping async iterators close to sync iterators.</span></span>

## <a name="linq"></a><span data-ttu-id="ea7c9-260">LINQ</span><span class="sxs-lookup"><span data-stu-id="ea7c9-260">LINQ</span></span>

<span data-ttu-id="ea7c9-261">Es gibt über ~ 200 über Ladungen von Methoden für die `System.Linq.Enumerable`-Klasse, die alle im Hinblick auf `IEnumerable<T>`funktionieren. Einige davon akzeptieren `IEnumerable<T>`, von denen einige `IEnumerable<T>`werden, und viele beide.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-261">There are over ~200 overloads of methods on the `System.Linq.Enumerable` class, all of which work in terms of `IEnumerable<T>`; some of these accept `IEnumerable<T>`, some of them produce `IEnumerable<T>`, and many do both.</span></span>  <span data-ttu-id="ea7c9-262">Das Hinzufügen von LINQ-Unterstützung für `IAsyncEnumerable<T>` würde wahrscheinlich dazu führen, dass alle diese über Ladungen für das Element duplizieren, für ein anderes ~ 200.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-262">Adding LINQ support for `IAsyncEnumerable<T>` would likely entail duplicating all of these overloads for it, for another ~200.</span></span>  <span data-ttu-id="ea7c9-263">Da `IAsyncEnumerator<T>` wahrscheinlich häufiger als eigenständige Entität in der asynchronen Welt als `IEnumerator<T>` in der synchronen Welt ist, benötigen wir möglicherweise weitere ~ 200-über Ladungen, die mit `IAsyncEnumerator<T>`arbeiten.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-263">And since `IAsyncEnumerator<T>` is likely to be more common as a standalone entity in the asynchronous world than `IEnumerator<T>` is in the synchronous world, we could potentially need another ~200 overloads that work with `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="ea7c9-264">Außerdem ist eine große Anzahl von über Ladungen mit Prädikaten (z. b. `Where`, die eine `Func<T, bool>`) behandeln, und es kann wünschenswert sein, `IAsyncEnumerable<T>`-basierte über Ladungen zu haben, die sowohl synchrone als auch asynchrone Prädikate (z. b. `Func<T, ValueTask<bool>>` zusätzlich zu `Func<T, bool>`) behandeln.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-264">Plus, a large number of the overloads deal with predicates (e.g. `Where` that takes a `Func<T, bool>`), and it may be desirable to have `IAsyncEnumerable<T>`-based overloads that deal with both synchronous and asynchronous predicates (e.g. `Func<T, ValueTask<bool>>` in addition to `Func<T, bool>`).</span></span>  <span data-ttu-id="ea7c9-265">Dies gilt zwar nicht für alle jetzt ~ 400 neuen über Ladungen, aber eine grobe Berechnung ist, dass Sie auf die Hälfte anwendbar ist, was eine weitere ~ 200-Überladung bedeutet, um insgesamt ~ 600 neue Methoden zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-265">While this isn't applicable to all of the now ~400 new overloads, a rough calculation is that it'd be applicable to half, which means another ~200 overloads, for a total of ~600 new methods.</span></span>

<span data-ttu-id="ea7c9-266">Das ist eine unglaubliche Anzahl von APIs, mit der Möglichkeit, noch mehr zu tun, wenn Erweiterungsbibliotheken wie interaktive Erweiterungen (IX) berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-266">That is a staggering number of APIs, with the potential for even more when extension libraries like Interactive Extensions (Ix) are considered.</span></span>  <span data-ttu-id="ea7c9-267">IX verfügt aber bereits über eine Implementierung vieler dieser Vorgänge, und es scheint keinen großen Grund dafür zu geben, die Arbeit zu duplizieren. Wir sollten die Community stattdessen dabei unterstützen, IX zu verbessern und für den Fall zu empfehlen, dass Entwickler LINQ mit `IAsyncEnumerable<T>`verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-267">But Ix already has an implementation of many of these, and there doesn't seem to be a great reason to duplicate that work; we should instead help the community improve Ix and recommend it for when developers want to use LINQ with `IAsyncEnumerable<T>`.</span></span>

<span data-ttu-id="ea7c9-268">Es gibt auch das Problem der Abfrage Verständnis Syntax.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-268">There is also the issue of query comprehension syntax.</span></span>  <span data-ttu-id="ea7c9-269">Aufgrund der musterbasierten Natur von Abfrage Vorgängen können Sie mit einigen Operatoren "einfach arbeiten", z. b. wenn IX die folgenden Methoden bereitstellt:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-269">The pattern-based nature of query comprehensions would allow them to "just work" with some operators, e.g. if Ix provides the following methods:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

<span data-ttu-id="ea7c9-270">Anschließend wird C# dieser Code "just work":</span><span class="sxs-lookup"><span data-stu-id="ea7c9-270">then this C# code will "just work":</span></span>

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

<span data-ttu-id="ea7c9-271">Es gibt jedoch keine Abfrage Verständnis Syntax, die die Verwendung von `await` in den-Klauseln unterstützt, d. h., wenn IX hinzugefügt wurde, z.b.:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-271">However, there is no query comprehension syntax that supports using `await` in the clauses, so if Ix added, for example:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

<span data-ttu-id="ea7c9-272">dann würde dies "just work" lauten:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-272">then this would "just work":</span></span>

```csharp
IAsyncEnumerable<string> result = from url in urls
                                  where item % 2 == 0
                                  select SomeAsyncMethod(item);

async ValueTask<int> SomeAsyncMethod(int item)
{
    await Task.Yield();
    return item * 2;
}
```

<span data-ttu-id="ea7c9-273">Es gibt jedoch keine Möglichkeit, Sie mit dem `await` Inline in der `select`-Klausel zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-273">but there'd be no way to write it with the `await` inline in the `select` clause.</span></span>  <span data-ttu-id="ea7c9-274">Als separater Aufwand könnten wir uns mit dem Hinzufügen von `async { ... }` Ausdrücken zur Sprache befassen. zu diesem Zeitpunkt könnten wir die Verwendung in Abfrage Begriffen erlauben, und die oben genannten könnte stattdessen wie folgt geschrieben werden:</span><span class="sxs-lookup"><span data-stu-id="ea7c9-274">As a separate effort, we could look into adding `async { ... }` expressions to the language, at which point we could allow them to be used in query comprehensions and the above could instead be written as:</span></span>

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

<span data-ttu-id="ea7c9-275">oder, um `await` direkt in Ausdrücken zu verwenden, z. b. durch Unterstützung von `async from`.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-275">or to enabling `await` to be used directly in expressions, such as by supporting `async from`.</span></span>  <span data-ttu-id="ea7c9-276">Es ist jedoch unwahrscheinlich, dass sich hier der Rest der featuremenge auf eine oder andere Weise auswirkt, und dies ist keine besonders hohe Bedeutung, um sofort zu investieren. der Vorschlag ist hier, hier nichts weiter zu tun.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-276">However, it's unlikely a design here would impact the rest of the feature set one way or the other, and this isn't a particularly high-value thing to invest in right now, so the proposal is to do nothing additional here right now.</span></span>

## <a name="integration-with-other-asynchronous-frameworks"></a><span data-ttu-id="ea7c9-277">Integration in andere asynchrone Frameworks</span><span class="sxs-lookup"><span data-stu-id="ea7c9-277">Integration with other asynchronous frameworks</span></span>

<span data-ttu-id="ea7c9-278">Die Integration in `IObservable<T>` und andere asynchrone Frameworks (z. b. reaktive Streams) erfolgt auf Bibliotheks Ebene und nicht auf Sprachebene.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-278">Integration with `IObservable<T>` and other asynchronous frameworks (e.g. reactive streams) would be done at the library level rather than at the language level.</span></span>  <span data-ttu-id="ea7c9-279">Beispielsweise können alle Daten aus einer `IAsyncEnumerator<T>` in einer `IObserver<T>` veröffentlicht werden, indem Sie einfach den Enumerator `await foreach`und `OnNext`die Daten an den Beobachter übermitteln, sodass eine `AsObservable<T>` Erweiterungsmethode möglich ist.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-279">For example, all of the data from an `IAsyncEnumerator<T>` can be published to an `IObserver<T>` simply by `await foreach`'ing over the enumerator and `OnNext`'ing the data to the observer, so an `AsObservable<T>` extension method is possible.</span></span>  <span data-ttu-id="ea7c9-280">Wenn ein `IObservable<T>` in einem `await foreach` verarbeitet wird, müssen die Daten gepuffert werden (für den Fall, dass ein anderes Element per Push übertragen wird, während das vorherige Element noch verarbeitet wird). ein solcher Push-Pull-Adapter kann jedoch problemlos implementiert werden, um zu ermöglichen, dass eine `IObservable<T>` von mit einer `IAsyncEnumerator<T>`</span><span class="sxs-lookup"><span data-stu-id="ea7c9-280">Consuming an `IObservable<T>` in a `await foreach` requires buffering the data (in case another item is pushed while the previous item is still being processing), but such a push-pull adapter can easily be implemented to enable an `IObservable<T>` to be pulled from with an `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="ea7c9-281">Etc.  RX/IX stellt bereits Prototypen solcher Implementierungen bereit, und Bibliotheken wie https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels stellen verschiedene Arten von Pufferung von Datenstrukturen bereit.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-281">Etc.  Rx/Ix already provide prototypes of such implementations, and libraries like https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels provide various kinds of buffering data structures.</span></span>  <span data-ttu-id="ea7c9-282">Die Sprache muss an dieser Phase nicht beteiligt sein.</span><span class="sxs-lookup"><span data-stu-id="ea7c9-282">The language need not be involved at this stage.</span></span>
