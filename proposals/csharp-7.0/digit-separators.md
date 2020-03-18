---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483499"
---
# <a name="digit-separators"></a><span data-ttu-id="f0110-101">Zahlentrennzeichen</span><span class="sxs-lookup"><span data-stu-id="f0110-101">Digit separators</span></span>

<span data-ttu-id="f0110-102">Die Möglichkeit, Ziffern in großen numerischen Literalen zu gruppieren, hätte eine gute Lesbarkeit und keinen signifikanten Nachteil.</span><span class="sxs-lookup"><span data-stu-id="f0110-102">Being able to group digits in large numeric literals would have great readability impact and no significant downside.</span></span> 

<span data-ttu-id="f0110-103">Durch das Hinzufügen von binären Literalen (#215) wird die Wahrscheinlichkeit erhöht, dass numerische Literale lang sind, sodass sich die beiden Features gegenseitig verbessern.</span><span class="sxs-lookup"><span data-stu-id="f0110-103">Adding binary literals (#215) would increase the likelihood of numeric literals being long, so the two features enhance each other.</span></span> 

<span data-ttu-id="f0110-104">Wir folgen Java und anderen und verwenden einen Unterstrich `_` als Ziffern Trennzeichen.</span><span class="sxs-lookup"><span data-stu-id="f0110-104">We would follow Java and others, and use an underscore `_` as a digit separator.</span></span> <span data-ttu-id="f0110-105">Sie wäre in der Lage, überall in einem numerischen Literalformat (mit Ausnahme des ersten und des letzten Zeichens) zu vorkommen, da unterschiedliche Gruppierungen in verschiedenen Szenarios und insbesondere in verschiedenen numerischen Basen sinnvoll sein können:</span><span class="sxs-lookup"><span data-stu-id="f0110-105">It would be able to occur everywhere in a numeric literal (except as the first and last character), since different groupings may make sense in different scenarios and especially for different numeric bases:</span></span>

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

<span data-ttu-id="f0110-106">Jede Sequenz von Ziffern kann durch Unterstriche getrennt werden, möglicherweise mehr als ein Unterstrich zwischen zwei aufeinander folgenden Ziffern.</span><span class="sxs-lookup"><span data-stu-id="f0110-106">Any sequence of digits may be separated by underscores, possibly more than one underscore between two consecutive digits.</span></span> <span data-ttu-id="f0110-107">Sie sind in Dezimalzahlen und Exponenten zulässig, aber nach der vorherigen Regel werden Sie möglicherweise nicht neben dem Dezimaltrennzeichen (`10_.0`), neben dem Exponentenzeichen (`1.1e_1`) oder neben dem Typspezifizierer (`10_f`) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f0110-107">They are allowed in decimals as well as exponents, but following the previous rule, they may not appear next to the decimal (`10_.0`), next to the exponent character (`1.1e_1`), or next to the type specifier (`10_f`).</span></span> <span data-ttu-id="f0110-108">Wenn Sie in binären und hexadezimalen Literalen verwendet werden, werden Sie möglicherweise nicht unmittelbar nach dem `0x` oder `0b`angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f0110-108">When used in binary and hexadecimal literals, they may not appear immediately following the `0x` or `0b`.</span></span>

<span data-ttu-id="f0110-109">Die Syntax ist einfach, und die Trennzeichen haben keine semantischen Auswirkungen. Sie werden einfach ignoriert.</span><span class="sxs-lookup"><span data-stu-id="f0110-109">The syntax is straightforward, and the separators have no semantic impact - they are simply ignored.</span></span>

<span data-ttu-id="f0110-110">Dies hat einen Breitenwert und lässt sich leicht implementieren.</span><span class="sxs-lookup"><span data-stu-id="f0110-110">This has broad value and is easy to implement.</span></span>
