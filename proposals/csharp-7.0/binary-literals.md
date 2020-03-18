---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483433"
---
# <a name="binary-literals"></a><span data-ttu-id="7817c-101">Binäre Literale</span><span class="sxs-lookup"><span data-stu-id="7817c-101">Binary literals</span></span>

<span data-ttu-id="7817c-102">Es gibt eine relativ häufige Anforderung, binäre Literale zu C# und VB hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="7817c-102">There’s a relatively common request to add binary literals to C# and VB.</span></span> <span data-ttu-id="7817c-103">Für Bitmasken (z. b. Flag-Aufstände) scheint dies wirklich nützlich zu sein, aber es wäre auch für Schulungszwecke gut geeignet.</span><span class="sxs-lookup"><span data-stu-id="7817c-103">For bitmasks (e.g. flag enums) this seems genuinely useful, but it would also be great just for educational purposes.</span></span>

<span data-ttu-id="7817c-104">Binäre Literale würden wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="7817c-104">Binary literals would look like this:</span></span>

```csharp
int nineteen = 0b10011;
```

<span data-ttu-id="7817c-105">Syntaktisch und semantisch sind Sie mit hexadezimalen Literalen identisch, mit Ausnahme der Verwendung `b`/`B` anstelle `x`/`X`, wobei nur Ziffern `0` und `1` und in Basis 2 anstelle von 16 interpretiert werden.</span><span class="sxs-lookup"><span data-stu-id="7817c-105">Syntactically and semantically they are identical to hexadecimal literals, except for using `b`/`B` instead of `x`/`X`, having only digits `0` and `1` and being interpreted in base 2 instead of 16.</span></span>

<span data-ttu-id="7817c-106">Es gibt wenig Kosten für die Implementierung dieser und wenig konzeptionellen Aufwand für Benutzer der Sprache.</span><span class="sxs-lookup"><span data-stu-id="7817c-106">There’s little cost to implementing these, and little conceptual overhead to users of the language.</span></span>

## <a name="syntax"></a><span data-ttu-id="7817c-107">Syntax</span><span class="sxs-lookup"><span data-stu-id="7817c-107">Syntax</span></span>

<span data-ttu-id="7817c-108">Die Grammatik sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="7817c-108">The grammar would be as follows:</span></span>

```antlr
integer-literal:
    : ...
    | binary-integer-literal
    ;
binary-integer-literal:
    : `0b` binary-digits integer-type-suffix-opt
    | `0B` binary-digits integer-type-suffix-opt
    ;
binary-digits:
    : binary-digit
    | binary-digits binary-digit
    ;
binary-digit:
    : `0`
    | `1`
    ;
```
