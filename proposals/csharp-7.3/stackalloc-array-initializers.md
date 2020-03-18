---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483589"
---
# <a name="stackalloc-array-initializers"></a>Stackalloc-Arrayinitialisierer

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Ermöglicht die Verwendung der arrayinitialisierersyntax mit `stackalloc`.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Bei normalen Arrays können ihre Elemente zum Zeitpunkt der Erstellung initialisiert werden. Es scheint sinnvoll, dies in `stackalloc` Fall zuzulassen.

Die Frage, warum eine solche Syntax mit `stackalloc` nicht zulässig ist, wird recht häufig angezeigt.  
Siehe z. b. [#1112](https://github.com/dotnet/csharplang/issues/1112)

## <a name="detailed-design"></a>Detaillierter Entwurf

Normale Arrays können mithilfe der folgenden Syntax erstellt werden:

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

Die Erstellung von Stapel zugeordneten Arrays sollte durch folgende Aktionen zugelassen werden:  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

Die Semantik aller Fälle ist ungefähr mit Arrays identisch.  
Beispiel: im letzten Fall wird der Elementtyp vom Initialisierer abgeleitet und muss ein nicht verwalteter Typ sein.

Hinweis: das Feature ist nicht davon abhängig, dass es sich bei dem Ziel um eine `Span<T>`handelt. Dies gilt auch für `T*` Fall, sodass es nicht sinnvoll erscheint, es in `Span<T>` Fall zu kennzeichnen.  

## <a name="translation"></a>Übersetzung

Die naive Implementierung könnte das Array einfach direkt nach der Erstellung durch eine Reihe von Element weisen Zuweisungen initialisieren.  

Ähnlich wie bei Arrays kann es möglich und wünschenswert sein, Fälle zu erkennen, in denen alle oder die meisten Elemente blitfähige Typen sind, und effizientere Techniken zu verwenden, indem Sie den vorab erstellten Zustand aller Konstanten Elemente kopieren. 

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Dies ist ein praktisches Feature. Es ist möglich, nichts zu tun.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Treffen von Besprechungen

Noch keine. 
