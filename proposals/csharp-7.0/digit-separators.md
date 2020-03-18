---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483499"
---
# <a name="digit-separators"></a>Zahlentrennzeichen

Die Möglichkeit, Ziffern in großen numerischen Literalen zu gruppieren, hätte eine gute Lesbarkeit und keinen signifikanten Nachteil. 

Durch das Hinzufügen von binären Literalen (#215) wird die Wahrscheinlichkeit erhöht, dass numerische Literale lang sind, sodass sich die beiden Features gegenseitig verbessern. 

Wir folgen Java und anderen und verwenden einen Unterstrich `_` als Ziffern Trennzeichen. Sie wäre in der Lage, überall in einem numerischen Literalformat (mit Ausnahme des ersten und des letzten Zeichens) zu vorkommen, da unterschiedliche Gruppierungen in verschiedenen Szenarios und insbesondere in verschiedenen numerischen Basen sinnvoll sein können:

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

Jede Sequenz von Ziffern kann durch Unterstriche getrennt werden, möglicherweise mehr als ein Unterstrich zwischen zwei aufeinander folgenden Ziffern. Sie sind in Dezimalzahlen und Exponenten zulässig, aber nach der vorherigen Regel werden Sie möglicherweise nicht neben dem Dezimaltrennzeichen (`10_.0`), neben dem Exponentenzeichen (`1.1e_1`) oder neben dem Typspezifizierer (`10_f`) angezeigt. Wenn Sie in binären und hexadezimalen Literalen verwendet werden, werden Sie möglicherweise nicht unmittelbar nach dem `0x` oder `0b`angezeigt.

Die Syntax ist einfach, und die Trennzeichen haben keine semantischen Auswirkungen. Sie werden einfach ignoriert.

Dies hat einen Breitenwert und lässt sich leicht implementieren.
