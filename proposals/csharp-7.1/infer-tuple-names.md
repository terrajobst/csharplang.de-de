---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483685"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a>Ableiten von tupelnamen (auch als bezeichnet). tupelprojektionsinitialisierer)

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

In einer Reihe gängiger Fälle ermöglicht dieses Feature, dass die tupelelementnamen ausgelassen und stattdessen abgeleitet werden. Anstatt `(f1: x.f1, f2: x?.f2)`einzugeben, können die Elementnamen "F1" und "F2" beispielsweise aus `(x.f1, x?.f2)`abgeleitet werden.

Dies ist vergleichbar mit dem Verhalten anonymer Typen, die das Ableiten von Elementnamen während der Erstellung ermöglichen. Beispielsweise deklariert `new { x.f1, y?.f2 }` die Member "F1" und "F2".

Dies ist besonders bei der Verwendung von Tupeln in LINQ nützlich:

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Die Änderung besteht aus zwei Teilen:

1.  Versuchen Sie, einen Kandidaten Namen für jedes Tupelelement abzuleiten, das keinen expliziten Namen hat:
    -   Die gleichen Regeln wie bei der namens Ableitung für anonyme Typen werden verwendet.
        - In C#sind dies drei Fälle möglich: `y` (Bezeichner), `x.y` (einfacher Member-Zugriff) und `x?.y` (bedingter Zugriff).
        - In VB ermöglicht dies zusätzliche Fälle, wie z. b. `x.y()`.
    -   Ablehnen von reservierten tupelnamen (unter C#Scheidung nach Groß-/Kleinschreibung in VB), da Sie entweder unzulässig oder bereits implizit sind. Beispielsweise z. b. `ItemN`, `Rest`und `ToString`.
    -   Wenn es sich bei allen Kandidaten Namen um Duplikate handelt ( C#Groß-/Kleinschreibung in VB ohne Beachtung der Groß-/Kleinschreibung) innerhalb des gesamten Tupels, löschen wir diese Kandidaten.
2.  Bei Konvertierungen (bei denen das Ablegen von Namen aus tupelliteralen überprüft und gewarnt wird) werden keine Warnungen ausgegeben. Dadurch wird das Abbrechen von vorhandenem tupelcode vermieden.

Beachten Sie, dass die Regel für die Verarbeitung von Duplikaten von der Regel für anonyme Typen abweicht. Beispielsweise erzeugt `new { x.f1, x.f1 }` einen Fehler, aber `(x.f1, x.f1)` wäre weiterhin zulässig (nur ohne herabherzufügende Namen). Dadurch wird das Abbrechen von vorhandenem tupelcode vermieden.

Aus Konsistenz Gründen gilt das gleiche für Tupel, die durch Debug-Zuweisungen (in C#) erstellt werden:

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

Dasselbe gilt auch für VB-Tupel, wobei die VB-spezifischen Regeln für das Ableiten von Namen aus Ausdrücken und die Groß-/Kleinschreibung von namens vergleichen verwendet werden.

Wenn Sie den C# 7,1-Compiler (oder höher) mit der Sprachversion "7,0" verwenden, werden die Elementnamen abgeleitet (obwohl das Feature nicht verfügbar ist), es wird jedoch ein Fehler bei der Verwendung des-Standorts für den Zugriff darauf ausgegeben. Dadurch wird das Hinzufügen von neuem Code, der später dem Kompatibilitätsproblem ausgesetzt wäre, eingeschränkt (siehe unten).

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Der Hauptnachteil besteht darin, dass dadurch eine Kompatibilitäts C# Pause von 7,0 eingeführt wird:

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

Der Kompatibilitäts Schluss fand diese Unterbrechung als akzeptabel, da Sie begrenzt ist und das Zeitfenster seit dem Versand von C# Tupeln (in 7,0) kurz ist.

## <a name="references"></a>Verweise
- [LDM, 4. April 2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- [GitHub-Diskussion](https://github.com/dotnet/csharplang/issues/370) (Danke @alrz, um dieses Problem zu beheben)
- [Design von Tupeln](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
