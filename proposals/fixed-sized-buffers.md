---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483571"
---
# <a name="fixed-sized-buffers"></a>Puffer fester Größe

* [x] vorgeschlagen
* [] Prototyp: nicht gestartet
* [] Implementierung: nicht gestartet
* [] Spezifikation: nicht gestartet

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Stellen Sie einen allgemeinen und sicheren Mechanismus zum Deklarieren von Puffer fester Größe in C# der Sprache bereit.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Heute haben Benutzer die Möglichkeit, Puffer fester Größe in einem unsicheren Kontext zu erstellen. Dies erfordert jedoch, dass der Benutzer mit Zeigern umgeht, Begrenzungen Überprüfungen manuell durchführt und nur einen begrenzten Satz von Typen (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`und `double`) unterstützt.

Die häufigste Beschwerde ist, dass Puffer fester Größe nicht in sicherem Code indiziert werden können. Die zweite Möglichkeit, mehr Typen zu verwenden, ist die zweite.

Mit einigen geringfügigen Anpassungen können wir allgemeine Puffer mit fester Größe bereitstellen, die jeden Typ unterstützen, in einem sicheren Kontext verwendet werden können und automatische Begrenzungen überprüft werden.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Ein Puffer mit fester Größe wird wie folgt deklariert:

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

Die Deklaration würde vom Compiler in eine interne Darstellung übersetzt werden, die der folgenden ähnelt.

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

Da solche Puffer keine `fixed`mehr benötigen, ist es sinnvoll, einen beliebigen Elementtyp zuzulassen.  

> Hinweis: `fixed` wird weiterhin unterstützt, aber nur, wenn der Elementtyp `blittable`

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

* Es könnten einige Probleme mit der Abwärtskompatibilität bestehen, da die vorhandenen Puffer mit fester Größe jedoch nur mit einer Auswahl primitiver Typen funktionieren, sollte der Compiler den "Just-Working"-Vorgang fortsetzen können, wenn der Benutzer den Fixed-Buffer als Zeichner.
* Nicht kompatible Konstrukte müssen möglicherweise eine etwas andere `v2` Codierung verwenden, um die Felder des alten Compilers auszublenden.
* Das Packen ist in Il-Spezifikationen für generische Typen nicht gut definiert. Während der Ansatz funktionieren sollte, werden wir auf nicht dokumentiertes Verhalten angrenzen. Wir sollten dies dokumentieren und sicherstellen, dass andere JITs wie Mono das gleiche Verhalten aufweisen.
* Die Angabe eines separaten Typs für jede Länge (eine möglicherweise eine andere für `readonly` Felder, falls unterstützt) wirkt sich auf Metadaten aus. Sie wird von der Anzahl der Arrays unterschiedlicher Größen in der angegebenen app gebunden.
* `ref` Math ist nicht formal überprüfbar (da Sie unsicher ist). Wir müssen eine Möglichkeit finden, die Überprüfungs Regeln zu aktualisieren, um zu wissen, dass die Verwendung in Ordnung ist.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Deklarieren Sie Ihre Strukturen manuell, und verwenden Sie unsicheren Code zum Erstellen von Indexer.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

- sollten wir `readonly`zulassen?  (mit Schreib geschütztem Indexer)
- sollten Arrayinitialisierer zugelassen werden?
- ist `fixed` Schlüsselwort erforderlich?
- `foreach`?
- nur Instanzfelder in Strukturen?

## <a name="design-meetings"></a>Treffen von Besprechungen

Verknüpfen Sie die Entwurfs Hinweise, die sich auf diesen Vorschlag auswirken, und beschreiben Sie in einem Satz für jede Änderungen, zu denen Sie geführt haben.