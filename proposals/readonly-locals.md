---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483481"
---
# <a name="readonly-locals-and-parameters"></a>schreibgeschützte lokale Variablen und Parameter

* [x] vorgeschlagen
* [] Prototyp
* []-Implementierung
* []-Spezifikation

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Zulassen, dass lokale und Parameter als schreibgeschützt kommentiert werden, um eine flache Mutation dieser lokalen Variablen und Parameter zu verhindern.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Heute kann das `readonly`-Schlüsselwort auf Felder angewendet werden. Dadurch wird sichergestellt, dass ein Feld nur während der Erstellung geschrieben werden kann (statische Konstruktion bei einem statischen Feld oder Instanzerstellung im Fall eines Instanzfelds). Dadurch können Entwickler Fehler vermeiden, indem Sie versehentlich einen Zustand überschreiben, der nicht geändert werden sollte. Aber Felder sind nicht die einzigen Orte, die Entwickler sicherstellen möchten, dass die Werte nicht mutiert werden. Insbesondere ist es üblich, eine lokale Variable zu erstellen, um den temporären Zustand zu speichern. das versehentliche aktualisieren dieses temporären Zustands kann zu fehlerhaften Berechnungen und anderen Fehlern führen, insbesondere wenn solche "Locals" in Lambdas aufgezeichnet werden. an diesem Punkt werden Sie zu Feldern entfernt. es gibt jedoch keine Möglichkeit, solche gehoben Felder als `readonly`zu markieren.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Lokale Variablen können auch `readonly` auch als festgelegt werden, und der Compiler stellt sicher, dass Sie nur zum Zeitpunkt der Deklaration festgelegt sind C# (bestimmte lokale in sind bereits implizit schreibgeschützt, wie z. b. die Iterations Variable in einer foreach-Schleife oder die verwendete Variable in einem Using-Block), aber derzeit kann ein Entwickler andere lokale lokal nicht als `readonly`markieren. Solche `readonly` lokalen Variablen müssen einen Initialisierer aufweisen:

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

Und als Kurzform für `readonly var`kann das vorhandene kontextabhängige Schlüsselwort `let` verwendet werden, z. b.

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

Es gibt keine besonderen Einschränkungen für den Initialisierer und kann als Initialisierer für lokale Elemente gelten, z. b.

```csharp
readonly T data = arg1 ?? arg2;
```

`readonly` on Locals ist besonders wertvoll, wenn Sie mit Lambdas und Abschlüssen arbeiten. Wenn eine anonyme Methode oder ein Lambda auf den lokalen Zustand aus dem einschließenden Bereich zugreift, wird dieser Zustand von dem Compiler als Closure aufgezeichnet, der durch eine "Anzeige Klasse" dargestellt wird.  Jedes "local", das erfasst wird, ist ein Feld in dieser Klasse, aber da der Compiler dieses Feld in Ihrem Namen erstellt, haben Sie keine Möglichkeit, es als `readonly` zu kommentieren, um zu verhindern, dass der Lambda-Ausdruck fälschlicherweise in das "lokale" schreibt (in Anführungszeichen, weil es sich nicht um eine lokale Datei handelt, zumindest nicht in der resultierenden MSIL). Bei `readonly` lokalen Variablen kann der Compiler verhindern, dass der Lambda-Ausdruck in den lokalen Schreibvorgang geschrieben wird. Dies ist besonders nützlich in Szenarios mit Multithreading, bei denen ein fehlerhafter Schreibvorgang zu einem gefährlichen, aber seltenen und schwer zu suchenden Parallelitäts Fehler führen könnte.

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

Als spezielle Form von local können Parameter auch als `readonly`angezeigt werden. Dies hat keine Auswirkung darauf, was der Aufrufer der Methode an den-Parameter übergeben kann (so wie es keine Einschränkung gibt, welche Werte in einem `readonly` Feld gespeichert werden können), aber wie bei allen `readonly` lokalen kann der Compiler verhindern, dass Code nach der Deklaration in den Parameter schreibt, was bedeutet, dass der Text der Methode nicht in den-Parameter geschrieben werden kann.

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

`readonly` Parameter wirken sich nicht auf die Signatur/Metadaten aus, die vom Compiler für diese Methode ausgegeben werden, und wirken sich einfach darauf aus, wie der Compiler die Kompilierung des Methoden Texts behandelt. So kann beispielsweise eine virtuelle Basis Methode einen `readonly`-Parameter haben, und dieser Parameter kann in einer außer Kraft Setzung beschreibbar sein.

Wie bei Feldern sind `readonly` für lokale und Parameter flach, was sich auf den Speicherort auswirkt, aber nicht transitiv auf das Objekt Diagramm wirkt. Ebenso wie bei Feldern wird durch das Aufrufen einer Methode für eine `readonly` local/Parameter-Struktur tatsächlich eine Kopie der Struktur erstellt und die-Methode für die Kopie aufgerufen, um eine interne Mutation `this`zu vermeiden.

`readonly` lokale und Parameter können nicht als `ref` oder `out` Argumente übermittelt werden, es sei denn,/bis `ref readonly` wird ebenfalls unterstützt.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

- `val` können als Alternative Kurzform zum `let`verwendet werden.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

- `readonly ref` / `ref readonly` / `readonly ref readonly`: Ich habe die Frage gestellt, wie `ref readonly` getrennt von diesem Vorschlag behandelt werden soll.
- Dieser Vorschlag behandelt nicht schreibgeschützte Strukturen/unveränderliche Typen. Dies bleibt für einen separaten Vorschlag.

## <a name="design-meetings"></a>Treffen von Besprechungen

- Kurz erläutert am 21. Januar 2015 (<https://github.com/dotnet/roslyn/issues/98>)
