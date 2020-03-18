---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79484147"
---
# <a name="async-streams"></a>Async-Streams

* [x] vorgeschlagen
* [x] Prototyp
* []-Implementierung
* []-Spezifikation

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

C#bietet Unterstützung für Iteratormethoden und asynchrone Methoden, aber keine Unterstützung für eine Methode, bei der es sich um einen Iterator und eine Async-Methode handelt.  Dies sollten Sie beheben, indem Sie zulassen, dass `await` in einer neuen Form `async` Iterators verwendet wird, der eine `IAsyncEnumerable<T>` oder `IAsyncEnumerator<T>` anstelle eines `IEnumerable<T>` oder `IEnumerator<T>`zurückgibt, wobei `IAsyncEnumerable<T>` in einem neuen `await foreach`verwendbar ist.  Eine `IAsyncDisposable`-Schnittstelle wird auch verwendet, um das asynchrone Cleanup zu aktivieren.

## <a name="related-discussion"></a>Verwandte Diskussion
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

## <a name="interfaces"></a>Schnittstellen

### <a name="iasyncdisposable"></a>Iasyncverwerfbare

Es gab viele Diskussionen über `IAsyncDisposable` (z. b. https://github.com/dotnet/roslyn/issues/114) und ob es sich um eine gute Idee handelt.  Es ist jedoch ein erforderliches Konzept, das für die Unterstützung von asynchronen Iteratoren hinzugefügt werden muss.  Da `finally` Blöcke `await`s enthalten können, und da `finally` Blöcke im Rahmen der Freigabe von Iteratoren ausgeführt werden müssen, ist eine asynchrone Entfernung erforderlich.  Es ist auch in der Regel hilfreich, wenn Ressourcen bereinigt werden können, z. b. das Schließen von Dateien (d. h. Leerungen), das Aufheben von Rückrufen und das Bereitstellen einer Möglichkeit, zu wissen, wann die Registrierung abgeschlossen ist usw.

Die folgende Schnittstelle wird den .net-Kernbibliotheken (z. b. System. private. Corelib/System. Runtime) hinzugefügt:
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
Wie bei `Dispose`ist das Aufrufen von `DisposeAsync` mehrmals akzeptabel, und nachfolgende Aufrufe nach dem ersten müssen als nops behandelt werden. dabei wird eine synchron abgeschlossene Aufgabe zurückgegeben (`DisposeAsync` die nicht Thread sicher sein müssen, aber gleichzeitige Aufrufe nicht unterstützen müssen).  Darüber hinaus können Typen sowohl `IDisposable` als auch `IAsyncDisposable`implementieren. wenn dies der Fall ist, ist es ebenso akzeptabel, `Dispose` aufzurufen und dann `DisposeAsync` oder umgekehrt, aber nur der erste sollte sinnvoll sein, und nachfolgende Aufrufe von beiden sollten ein NOP sein.  Wenn ein Typ beide implementiert, wird empfohlen, nur einmal und nur einmal die relevantere Methode aufzurufen, die auf dem Kontext basiert, `Dispose` in synchronen Kontexten und `DisposeAsync` in asynchronen Kontexten.

(Ich werde die Diskussion über die Interaktion von `IAsyncDisposable` mit `using` in einer separaten Diskussion belassen.  Und die Abdeckungs Weise, wie Sie mit `foreach` interagiert, wird später in diesem Vorschlag behandelt.)

Berücksichtigte Alternativen:
- _`DisposeAsync` akzeptieren einer `CancellationToken`_ : Obwohl es theoretisch Sinn macht, dass alle asynchronen Vorgänge abgebrochen werden können, sind die Bereinigung, das Schließen von Dingen, das Einfrieren von Ressourcen usw., was in der Regel nicht abgebrochen werden sollte. die Bereinigung ist für abgebrochene Arbeit weiterhin wichtig.  Dieselbe `CancellationToken`, die bewirkt hat, dass die tatsächliche Arbeit abgebrochen wurde, ist in der Regel das gleiche Token, das an `DisposeAsync`übergebenen wird, was `DisposeAsync` wertlos ist, weil der Abbruch der Arbeit `DisposeAsync` zu einem NOP führen würde.  Wenn Sie verhindern möchten, dass Sie blockiert werden, warten Sie, bis Sie auf die resultierende `ValueTask`warten, oder warten Sie nur für einen bestimmten Zeitraum.
- _`DisposeAsync` zurückgeben eines `Task`_ : Nachdem eine nicht generische `ValueTask` vorhanden ist, die aus einer `IValueTaskSource`erstellt werden kann, kann durch das Zurückgeben von `ValueTask` von `DisposeAsync` ein vorhandenes Objekt als Zusage wieder verwendet werden, die den asynchronen Abschluss des `DisposeAsync`darstellt, wobei eine `Task` Zuordnung in dem Fall gespeichert wird, in dem `DisposeAsync` asynchron abgeschlossen wird.
- _Konfigurieren von `DisposeAsync` mit einer `bool continueOnCapturedContext` (`ConfigureAwait`)_ : Es gibt zwar Probleme im Zusammenhang mit der Offenlegung eines solchen Konzepts für `using`, `foreach`und andere Sprachkonstrukte, die diese nutzen. aus Schnittstellen Sicht führt es jedoch keine `await`", und es ist nichts zu konfigurieren... Consumer der `ValueTask` können Sie dennoch nutzen.
- _`IAsyncDisposable`, die `IDisposable`erbt_ : da nur eine oder die andere verwendet werden sollte, ist es nicht sinnvoll, die Implementierung beider Typen zu erzwingen.
- _`IDisposableAsync` statt `IAsyncDisposable`_ : Wir haben die Benennung der "Async"-Methode verfolgt, während Vorgänge "Done Async" lauten, sodass die Typen "Async" als Präfix aufweisen und dass die Methoden "Async" als Suffix aufweisen.

### <a name="iasyncenumerable--iasyncenumerator"></a>Iasyncengerable/iasyncenumschlag

Den .net-Kernbibliotheken werden zwei Schnittstellen hinzugefügt:

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

Der typische Verbrauch (ohne zusätzliche sprach Features) sieht wie folgt aus:

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

Verworfene Optionen:
- _`Task<bool> MoveNextAsync(); T current { get; }`_ : die Verwendung von `Task<bool>` unterstützt die Verwendung eines zwischengespeicherten Task Objekts zur Darstellung synchroner, erfolgreicher `MoveNextAsync` Aufrufe, aber eine Zuordnung wäre weiterhin für den asynchronen Abschluss erforderlich.  Durch die Rückgabe von `ValueTask<bool>`aktivieren wir das Enumeratorobjekt, damit es `IValueTaskSource<bool>` implementiert und als Unterstützung für die von `MoveNextAsync`zurückgegebenen `ValueTask<bool>` verwendet werden kann, was wiederum einen erheblich reduzierten Overheads ermöglicht.
- _`ValueTask<(bool, T)> MoveNextAsync();`_ : Es ist nicht nur schwerer zu verwenden, sondern es bedeutet, dass `T` nicht mehr kovariant sein kann.
- _`ValueTask<T?> TryMoveNextAsync();`_ : nicht kovariant.
- _`Task<T?> TryMoveNextAsync();`_ : nicht kovariant, Belegungen bei jedem-Rückruf usw.
- _`ITask<T?> TryMoveNextAsync();`_ : nicht kovariant, Belegungen bei jedem-Rückruf usw.
- _`ITask<(bool,T)> TryMoveNextAsync();`_ : nicht kovariant, Belegungen bei jedem-Rückruf usw.
- _`Task<bool> TryMoveNextAsync(out T result);`_ : das `out` Ergebnis muss festgelegt werden, wenn der Vorgang synchron zurückgegeben wird, und nicht, wenn die Aufgabe asynchron abgeschlossen wird, wenn die Aufgabe möglicherweise irgendwann in der Zukunft abgeschlossen ist. an diesem Punkt gibt es keine Möglichkeit, das Ergebnis zu kommunizieren.
- _`IAsyncEnumerator<T>` `IAsyncDisposable`nicht implementiert_ : Wir könnten diese Optionen trennen.  Dies erschwert jedoch bestimmte andere Bereiche des Angebots, da Code dann in der Lage sein muss, mit der Möglichkeit zu umgehen, dass ein Enumerator keine Entsorgung bereitstellt. dadurch ist es schwierig, Musterbasierte Hilfsprogramme zu schreiben.  Außerdem ist es für Enumeratoren üblich, dass Sie zur Verfügung stehen (z. b. einen C# beliebigen Async-Iterator, der über einen letzten Block verfügt, in dem die meisten Elemente von einer Netzwerkverbindung aufgelistet sind usw.), und wenn dies nicht der Fall ist, ist es einfach, die Methode ausschließlich als `public ValueTask DisposeAsync() => default(ValueTask);` mit minimalem zusätzlichen Aufwand zu implementieren.
- _ `IAsyncEnumerator<T> GetAsyncEnumerator()`: kein Abbruch Token-Parameter.

#### <a name="viable-alternative"></a>Mögliche Alternative:

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

`TryGetNext` wird in einer inneren Schleife verwendet, um Elemente mit einem einzelnen Schnittstellen Aufrufwert zu verarbeiten, solange Sie synchron verfügbar sind.  Wenn das nächste Element nicht synchron abgerufen werden kann, wird false zurückgegeben, und jedes Mal, wenn es false zurückgibt, muss ein Aufrufer anschließend `WaitForNextAsync` aufrufen, um entweder darauf zu warten, dass das nächste Element verfügbar ist, oder um festzustellen, dass es nie ein anderes Element gibt. Der typische Verbrauch (ohne zusätzliche sprach Features) sieht wie folgt aus:

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

Der Vorteil besteht darin, dass zwei fache, ein kleiner und ein hauptsächlich:
- _Neben Version: ermöglicht einem Enumerator, mehrere Consumer zu unterstützen_. Möglicherweise gibt es Szenarien, in denen es für einen Enumerator wichtig ist, mehrere gleichzeitige Consumer zu unterstützen.  Dies kann nicht erreicht werden, wenn `MoveNextAsync` und `Current` voneinander getrennt sind, sodass eine Implementierung ihre Verwendung nicht atomarisch machen kann.  Im Gegensatz dazu bietet dieser Ansatz eine einzelne Methode `TryGetNext`, die das Übertragen des Enumerators und das nächste Element unterstützt. Daher kann der Enumerator bei Bedarf Atomizität aktivieren.  Es ist jedoch wahrscheinlich, dass solche Szenarios auch aktiviert werden können, indem jeder Consumer seinen eigenen Enumerator aus einem freigegebenen Aufzähl baren Element bereitstellt.  Außerdem möchten wir nicht erzwingen, dass jeder Enumerator die gleichzeitige Verwendung unterstützt, da dadurch nicht triviale Aufwand zum Mehrheits Fall hinzugefügt werden, der nicht erforderlich ist. Dies bedeutet, dass sich ein Consumer der Schnittstelle im Allgemeinen nicht auf diese Weise verlassen kann.
- _Hauptversion: Leistung_. Der `MoveNextAsync`/`Current` Ansatz erfordert zwei Schnittstellen Aufrufe pro Vorgang, während der beste Fall für `WaitForNextAsync`/`TryGetNext` ist, dass die meisten Iterationen synchron ausgeführt werden, was eine enge innere Schleife mit `TryGetNext`ermöglicht, sodass nur ein Schnittstellen Aufruf pro Vorgang durchgeführt wird.  Dies kann in Situationen, in denen die Schnittstellen Aufrufe die Berechnung dominieren, messbare Auswirkungen haben.

Es gibt jedoch nicht triviale Nachteile, einschließlich einer erheblich größeren Komplexität, wenn diese manuell genutzt werden, und eine größere Chance, bei deren Verwendung Fehler zu verursachen.  Und während die Leistungsvorteile in den Mikrobenchmarks angezeigt werden, glauben wir nicht, dass Sie sich auf den größten Teil der realen Verwendung auswirken werden.  Wenn sich dies herausstellt, können wir einen zweiten Satz von Schnittstellen in einer hellen Weise einführen.

Verworfene Optionen:
- `ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` Parameter können nicht kovariant sein.  Hier gibt es auch eine kleine Auswirkung (ein Problem mit dem try-Muster im allgemeinen), dass dies wahrscheinlich eine Lauf Zeit Schreib Barriere für Verweistyp Ergebnisse verursacht.

#### <a name="cancellation"></a>Abbruch

Es gibt mehrere mögliche Ansätze zur Unterstützung von Abbruch:
1. `IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` sind Abbruch agnostisch: `CancellationToken` nicht an einer beliebigen Stelle angezeigt.  Der Abbruch wird erreicht, indem die `CancellationToken` logisch in den Aufzähl Bare-und/oder-Enumerator gebackt wird. Dies ist z. b. der Fall, wenn ein Iterator aufgerufen wird, der `CancellationToken` als Argument an die Iteratormethode übergeben und im Text des Iterators verwendet wird, wie es bei einem anderen Parameter der Fall ist.
2. `IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: Sie übergeben eine `CancellationToken` an `GetAsyncEnumerator`, und nachfolgende `MoveNextAsync` Vorgänge berücksichtigen dies, dies kann jedoch der Fall sein.
3. `IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: übergeben Sie eine `CancellationToken` an jeden einzelnen `MoveNextAsync`-Befehl.
4. 1 & & 2: Sie Betten `CancellationToken`s in ihren Enumerable/Enumerator ein und übergeben `CancellationToken`s an `GetAsyncEnumerator`.
5. 1 & & 3: Sie Betten `CancellationToken`s in ihren Enumerable/Enumerator ein und übergeben `CancellationToken`s an `MoveNextAsync`.

Aus rein theoretischen Sicht ist (5) das stabilste, da (a) `MoveNextAsync` das akzeptieren einer `CancellationToken` die präzisere Kontrolle über das abgebrochene ermöglicht, und (b) `CancellationToken` ist nur ein beliebiger anderer Typ, der als Argument an Iteratoren übergeben werden kann, die in beliebige Typen eingebettet sind, usw.

Bei diesem Ansatz gibt es jedoch mehrere Probleme:
- Wie wird ein `CancellationToken` an `GetAsyncEnumerator` an den Text des Iterators geleitet?  Wir könnten ein neues `iterator`-Schlüsselwort verfügbar machen, von dem Sie einen Punkt abrufen können, um Zugriff auf die an `GetEnumerator``CancellationToken` zu erhalten. a) das ist aber eine Menge zusätzlicher Maschinen, b) wir machen es zu einem erstklassigen Bürger und c) der Fall von 99% scheint derselbe Code zu sein, der einen Iterator aufruft und `GetAsyncEnumerator` darauf aufruft. in diesem Fall kann er einfach die `CancellationToken` als Argument an die-Methode übergeben.
- Wie wird ein `CancellationToken` an `MoveNextAsync` an den Text der Methode geleitet?  Dies ist noch schlimmer, denn wenn Sie von einem `iterator` lokalen Objekt verfügbar gemacht wird, kann sich der Wert über erwartungsgemäß ändern. Dies bedeutet, dass jeder Code, der mit dem Token registriert ist, die Registrierung bei ihm vor den Vorgängen aufheben und anschließend erneut registrieren muss. Es ist auch möglich, dass die Registrierung und die Aufhebung der Registrierung in jedem `MoveNextAsync`-Befehl durchgeführt werden müssen, unabhängig davon, ob der Compiler manuell in einem Iterator oder einem Entwickler implementiert ist.
- Wie wird ein Entwickler eine `foreach` Schleife Abbrechen?  Wenn dies geschieht, indem ein `CancellationToken` an einen Aufzähl Bare/Enumerator übergeben wird, dann eine), müssen wir `foreach`"over Enumeratoren unterstützen, die Sie als erstklassige Bürger auslösen. und jetzt müssen Sie sich über ein Ökosystem Gedanken machen, das auf Enumeratoren aufbaut (z. b. LINQ-Methoden) oder b) Wir müssen die `CancellationToken` trotzdem in das Aufzähl Bare Element einbetten, indem wir einige `WithCancellation` Erweiterungsmethode außerhalb von `IAsyncEnumerable<T>` haben, in der das bereitgestellte Token gespeichert wird. `GetAsyncEnumerator` `GetAsyncEnumerator` wird aufgerufen (dieses Token wird ignoriert).  Oder Sie können einfach die `CancellationToken` verwenden, die Sie im Text von foreach haben.
- Wenn/wenn Abfrage Ausdrücke unterstützt werden, wie soll die `CancellationToken` an `GetEnumerator` oder `MoveNextAsync` an jede Klausel übergeben werden?  Am einfachsten wäre es, wenn die-Klausel die-Klausel aufzeichnen soll. an dieser Stelle wird das Token an `GetAsyncEnumerator`/`MoveNextAsync` ignoriert.

Eine frühere Version dieses Dokuments wurde empfohlen (1), wir haben jedoch zu (4) gewechselt.

Die zwei Hauptprobleme bei (1):
- Producer von abbrechbaren Enumerationen müssen einige Bausteine implementieren und können nur die Unterstützung des Compilers für Async-Iteratoren nutzen, um eine `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` Methode zu implementieren.
- Es ist wahrscheinlich, dass viele Producer einfach einen `CancellationToken` Parameter zu ihrer asynchronen Enumerable-Signatur hinzufügen würden, wodurch verhindert wird, dass Consumer das gewünschte Abbruch Token übergeben, wenn Sie einen `IAsyncEnumerable` Typ erhalten.

Es gibt zwei Haupt Verwendungs Szenarien:
1. `await foreach (var i in GetData(token)) ...`, in dem der Consumer die Async-Iterator-Methode aufruft,
2. `await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...`, in dem der Consumer eine bestimmte `IAsyncEnumerable` Instanz behandelt.

Wir stellen fest, dass ein angemessener Kompromiss zur Unterstützung beider Szenarien auf eine Weise, die sowohl für Producer als auch für Consumer von asynchronen Streams geeignet ist, die Verwendung eines speziell mit Anmerkungen versehene Parameters in der Async-Iterator-Methode ist. Zu diesem Zweck wird das `[EnumeratorCancellation]`-Attribut verwendet. Das Platzieren dieses Attributs für einen Parameter weist den Compiler an, dass, wenn ein Token an die `GetAsyncEnumerator` Methode übergeben wird, dieses Token anstelle des ursprünglich für den-Parameter übergebenen Werts verwendet werden soll.

Gehen Sie von `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)` aus. Der Implementierer dieser Methode kann einfach den-Parameter im Methoden Text verwenden. Der Consumer kann entweder die folgenden Verbrauchsmuster verwenden:
1. Wenn Sie `GetData(token)`verwenden, wird das Token in "Async-Enumerable" gespeichert und in der Iterations Gruppe verwendet.
2. Wenn Sie `givenIAsyncEnumerable.WithCancellation(token)`verwenden, ersetzt das an `GetAsyncEnumerator` übergebenen Token alle Token, die im Async-Enumerable-Element gespeichert werden.

## <a name="foreach"></a>foreach

`foreach` wird erweitert, um `IAsyncEnumerable<T>` zusätzlich zu seiner vorhandenen Unterstützung für `IEnumerable<T>`zu unterstützen.  Außerdem wird die Entsprechung von `IAsyncEnumerable<T>` als Muster unterstützt, wenn die relevanten Member öffentlich verfügbar gemacht werden, wobei die Schnittstelle direkt verwendet wird, wenn dies nicht der Fall ist, um auf struct basierende Erweiterungen zu aktivieren, die die Zuordnung und die Verwendung alternativer "awaitables" als Rückgabetyp von `MoveNextAsync` und `DisposeAsync`vermeiden.

### <a name="syntax"></a>Syntax

Verwenden Sie die folgende Syntax:

```csharp
foreach (var i in enumerable)
```

C#behandelt `enumerable` weiterhin als synchrones Aufzähl bares Element, sodass selbst dann, wenn die relevanten APIs für asynchrone Enumerables verfügbar gemacht werden (das Muster verfügbar gemacht oder die Schnittstelle implementiert wird), nur die synchronen APIs berücksichtigt werden.

Um `foreach` zu erzwingen, dass Sie stattdessen nur die asynchronen APIs berücksichtigt, wird `await` wie folgt eingefügt:

```csharp
await foreach (var i in enumerable)
```

Es wurde keine Syntax bereitgestellt, die die Verwendung der Async-oder Sync-APIs unterstützt. der Entwickler muss basierend auf der verwendeten Syntax auswählen.

Verworfene Optionen:
- _`foreach (var i in await enumerable)`_ : Dies ist bereits eine gültige Syntax, und das Ändern der Bedeutung wäre eine Breaking Change.  Dies bedeutet, dass Sie die `enumerable``await`, eine synchrone Iterable von der Datei erhalten und diese dann synchron durchlaufen können.
- _`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)``foreach (await var i in enumerable)`_ : Dies deutet darauf hin, dass wir auf das nächste Element warten, aber es gibt noch weitere Warteschlangen, die sich an foreach beteiligen, insbesondere, wenn das Aufzähl Bare Element ein `IAsyncDisposable`ist, wird die asynchrone Entfernung `await`.  Der erwartungsgemäß ist der Bereich von foreach und nicht für jedes einzelne Element, weshalb das `await`-Schlüsselwort auf `foreach` Ebene liegen muss.  Außerdem bietet uns eine Möglichkeit, die `foreach` mit einem anderen Begriff zu beschreiben, wenn Sie mit dem `foreach` verknüpft ist, z. b. "warten auf foreach".  Noch wichtiger ist jedoch, `foreach` Syntax zur gleichen Zeit wie `using` Syntax zu berücksichtigen, sodass Sie konsistent zueinander bleiben und `using (await ...)` bereits gültige Syntax ist.
- _`foreach await (var i in enumerable)`_

Beachten Sie Folgendes:
- `foreach` heute unterstützt keine Iteration durch einen Enumerator.  Wir gehen davon aus, dass es häufiger `IAsyncEnumerator<T>`s gibt, und daher ist es verlockend, `await foreach` mit `IAsyncEnumerable<T>` und `IAsyncEnumerator<T>`zu unterstützen.  Nachdem wir diese Unterstützung hinzugefügt haben, wird die Frage gestellt, ob `IAsyncEnumerator<T>` ein erstklassiger Bürger ist, und ob Sie über über Ladungen von combinatoren verfügen müssen, die auf Enumeratoren neben Enumerationen angewendet werden?    Möchten wir Methoden zum Zurückgeben von Enumeratoren anstelle von Enumerables ermutigen? Wir sollten dies weiterhin besprechen.  Wenn wir entscheiden, dass wir Sie nicht unterstützen möchten, können wir eine Erweiterungsmethode `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` einführen, mit der ein Enumerator weiterhin `foreach`d werden kann.  Wenn wir uns entscheiden, dies zu unterstützen, müssen wir auch entscheiden, ob die `await foreach` für den Aufruf von `DisposeAsync` auf dem Enumerator zuständig wäre, und die Antwort ist wahrscheinlich "Nein, die Kontrolle über die Freigabe sollte von jedem Benutzer behandelt werden, der `GetEnumerator`aufgerufen wurde."

### <a name="pattern-based-compilation"></a>Musterbasierte Kompilierung

Der Compiler bindet eine Bindung an die musterbasierten APIs, wenn Sie vorhanden sind, und bevorzugt diese für die Verwendung der-Schnittstelle (das Muster ist möglicherweise mit Instanzmethoden oder Erweiterungs Methoden erfüllt).  Folgende Anforderungen gelten für das Muster:
- Das Aufzähl Bare Element muss eine `GetAsyncEnumerator` Methode verfügbar machen, die ohne Argumente aufgerufen werden kann und einen Enumerator zurückgibt, der das relevante Muster erfüllt.
- Der Enumerator muss eine `MoveNextAsync` Methode verfügbar machen, die ohne Argumente aufgerufen werden kann und einen Wert zurückgibt, der möglicherweise `await`Ed ist und dessen `GetResult()` eine `bool`zurückgibt.
- Der Enumerator muss auch `Current`-Eigenschaft verfügbar machen, deren Getter eine `T` zurückgibt, die die Art der aufzuzählenden Daten darstellt.
- Der Enumerator kann optional eine `DisposeAsync` Methode verfügbar machen, die ohne Argumente aufgerufen werden kann, und die etwas zurückgibt, das `await`Ed und dessen `GetResult()` `void`zurückgibt.

Dieser Code:

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

wird in die-Entsprechung von übersetzt:

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

Wenn der Iterierte Typ das richtige Muster nicht verfügbar macht, werden die Schnittstellen verwendet.

### <a name="configureawait"></a>"Configureawait"

Diese Musterbasierte Kompilierung ermöglicht die Verwendung von `ConfigureAwait` für alle über eine `ConfigureAwait` Erweiterungsmethode:

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

Dies basiert auf den Typen, die wir .NET hinzufügen, wahrscheinlich auch System. Threading. Tasks. Extensions. dll:

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

Beachten Sie, dass dieser Ansatz nicht die Verwendung von `ConfigureAwait` mit musterbasierten Enumerables ermöglicht. aber auch hier ist es bereits der Fall, dass der `ConfigureAwait` nur als Erweiterung auf `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` verfügbar gemacht wird und nicht auf willkürliche, nicht aufnutzbare Dinge angewendet werden kann, da er nur bei der Anwendung auf Tasks sinnvoll ist (er steuert ein Verhalten, das in der Fortsetzungs Unterstützung der Aufgabe implementiert ist). Daher ist es nicht sinnvoll, wenn ein Muster verwendet wird  Jede Person, die überwachte Dinge zurückgibt, kann Ihr eigenes benutzerdefiniertes Verhalten in solchen erweiterten Szenarien bereitstellen

(Wenn es eine Möglichkeit gibt, eine `ConfigureAwait` Lösung auf Bereichs-oder Assemblyebene zu unterstützen, ist dies nicht erforderlich.)

## <a name="async-iterators"></a>Async-Iteratoren

Die Sprache/der Compiler unterstützt die Erstellung von `IAsyncEnumerable<T>`s und `IAsyncEnumerator<T>`s zusätzlich zu deren Nutzung. Heute unterstützt die Sprache das Schreiben eines Iterators wie:

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

`await` kann jedoch nicht im Text dieser Iteratoren verwendet werden.  Wir werden diese Unterstützung hinzufügen.

### <a name="syntax"></a>Syntax

Die vorhandene Sprachunterstützung für Iteratoren leitet die iteratorart der Methode basierend darauf ab, ob Sie `yield`s enthält.  Das gleiche gilt für Async-Iteratoren.  Solche Async-Iteratoren werden durch das Hinzufügen von `async` zur Signatur abgegrenzt und unterscheiden sich von synchronen Iteratoren. Außerdem muss entweder `IAsyncEnumerable<T>` oder `IAsyncEnumerator<T>` als Rückgabetyp verwendet werden.  Das obige Beispiel könnte z. b. wie folgt als Async-Iterator geschrieben werden:

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

Berücksichtigte Alternativen:
- Die Verwendung von _`async` in der Signatur wird nicht_verwendet: die Verwendung `async` ist für den Compiler wahrscheinlich technisch erforderlich, da er verwendet wird, um zu bestimmen, ob `await` in diesem Kontext gültig ist.  Aber auch wenn dies nicht erforderlich ist, haben wir festgestellt, dass `await` nur in Methoden verwendet werden können, die als `async`gekennzeichnet sind, und es ist wichtig, die Konsistenz zu wahren.
- _Aktivieren benutzerdefinierter Generatoren für `IAsyncEnumerable<T>`_ : das ist etwas, das wir uns für die Zukunft ansehen konnten, aber die Technik ist kompliziert, und wir unterstützen das nicht für die synchronen Gegenstücke.
- _Ein `iterator`-Schlüsselwort in der Signatur_: Async-Iteratoren verwenden `async iterator` in der Signatur, und `yield` können nur in `async` Methoden verwendet werden, die `iterator`enthalten. `iterator` dann in synchronen Iteratoren optional gemacht werden.  Abhängig von ihrer Perspektive hat dies den Vorteil, dass es durch die Signatur der-Methode ganz klar ist, ob `yield` zulässig ist und ob die Methode tatsächlich zum Zurückgeben von Instanzen des Typs `IAsyncEnumerable<T>` gedacht ist, anstatt die compilerfertigung, je nachdem, ob der Code `yield` verwendet oder nicht.  Sie unterscheidet sich jedoch von synchronen Iteratoren, die nicht für eine Anforderung erforderlich sind.  Außerdem sind einige Entwickler nicht der zusätzlichen Syntax gefallen.  Wenn wir Sie von Grund auf neu entwerfen, wäre dies wahrscheinlich erforderlich, aber an diesem Punkt gibt es noch viel mehr Wert, wenn Sie asynchrone Iteratoren in der Nähe der Synchronisierungs Iteratoren halten.

## <a name="linq"></a>LINQ

Es gibt über ~ 200 über Ladungen von Methoden für die `System.Linq.Enumerable`-Klasse, die alle im Hinblick auf `IEnumerable<T>`funktionieren. Einige davon akzeptieren `IEnumerable<T>`, von denen einige `IEnumerable<T>`werden, und viele beide.  Das Hinzufügen von LINQ-Unterstützung für `IAsyncEnumerable<T>` würde wahrscheinlich dazu führen, dass alle diese über Ladungen für das Element duplizieren, für ein anderes ~ 200.  Da `IAsyncEnumerator<T>` wahrscheinlich häufiger als eigenständige Entität in der asynchronen Welt als `IEnumerator<T>` in der synchronen Welt ist, benötigen wir möglicherweise weitere ~ 200-über Ladungen, die mit `IAsyncEnumerator<T>`arbeiten.  Außerdem ist eine große Anzahl von über Ladungen mit Prädikaten (z. b. `Where`, die eine `Func<T, bool>`) behandeln, und es kann wünschenswert sein, `IAsyncEnumerable<T>`-basierte über Ladungen zu haben, die sowohl synchrone als auch asynchrone Prädikate (z. b. `Func<T, ValueTask<bool>>` zusätzlich zu `Func<T, bool>`) behandeln.  Dies gilt zwar nicht für alle jetzt ~ 400 neuen über Ladungen, aber eine grobe Berechnung ist, dass Sie auf die Hälfte anwendbar ist, was eine weitere ~ 200-Überladung bedeutet, um insgesamt ~ 600 neue Methoden zu erhalten.

Das ist eine unglaubliche Anzahl von APIs, mit der Möglichkeit, noch mehr zu tun, wenn Erweiterungsbibliotheken wie interaktive Erweiterungen (IX) berücksichtigt werden.  IX verfügt aber bereits über eine Implementierung vieler dieser Vorgänge, und es scheint keinen großen Grund dafür zu geben, die Arbeit zu duplizieren. Wir sollten die Community stattdessen dabei unterstützen, IX zu verbessern und für den Fall zu empfehlen, dass Entwickler LINQ mit `IAsyncEnumerable<T>`verwenden möchten.

Es gibt auch das Problem der Abfrage Verständnis Syntax.  Aufgrund der musterbasierten Natur von Abfrage Vorgängen können Sie mit einigen Operatoren "einfach arbeiten", z. b. wenn IX die folgenden Methoden bereitstellt:

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

Anschließend wird C# dieser Code "just work":

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

Es gibt jedoch keine Abfrage Verständnis Syntax, die die Verwendung von `await` in den-Klauseln unterstützt, d. h., wenn IX hinzugefügt wurde, z.b.:

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

dann würde dies "just work" lauten:

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

Es gibt jedoch keine Möglichkeit, Sie mit dem `await` Inline in der `select`-Klausel zu schreiben.  Als separater Aufwand könnten wir uns mit dem Hinzufügen von `async { ... }` Ausdrücken zur Sprache befassen. zu diesem Zeitpunkt könnten wir die Verwendung in Abfrage Begriffen erlauben, und die oben genannten könnte stattdessen wie folgt geschrieben werden:

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

oder, um `await` direkt in Ausdrücken zu verwenden, z. b. durch Unterstützung von `async from`.  Es ist jedoch unwahrscheinlich, dass sich hier der Rest der featuremenge auf eine oder andere Weise auswirkt, und dies ist keine besonders hohe Bedeutung, um sofort zu investieren. der Vorschlag ist hier, hier nichts weiter zu tun.

## <a name="integration-with-other-asynchronous-frameworks"></a>Integration in andere asynchrone Frameworks

Die Integration in `IObservable<T>` und andere asynchrone Frameworks (z. b. reaktive Streams) erfolgt auf Bibliotheks Ebene und nicht auf Sprachebene.  Beispielsweise können alle Daten aus einer `IAsyncEnumerator<T>` in einer `IObserver<T>` veröffentlicht werden, indem Sie einfach den Enumerator `await foreach`und `OnNext`die Daten an den Beobachter übermitteln, sodass eine `AsObservable<T>` Erweiterungsmethode möglich ist.  Wenn ein `IObservable<T>` in einem `await foreach` verarbeitet wird, müssen die Daten gepuffert werden (für den Fall, dass ein anderes Element per Push übertragen wird, während das vorherige Element noch verarbeitet wird). ein solcher Push-Pull-Adapter kann jedoch problemlos implementiert werden, um zu ermöglichen, dass eine `IObservable<T>` von mit einer `IAsyncEnumerator<T>`  Etc.  RX/IX stellt bereits Prototypen solcher Implementierungen bereit, und Bibliotheken wie https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels stellen verschiedene Arten von Pufferung von Datenstrukturen bereit.  Die Sprache muss an dieser Phase nicht beteiligt sein.
