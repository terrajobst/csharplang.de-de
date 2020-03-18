---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483493"
---
# <a name="null-conditional-await"></a><span data-ttu-id="ba7c2-101">NULL bedingter warte Wert</span><span class="sxs-lookup"><span data-stu-id="ba7c2-101">null-conditional await</span></span>

* <span data-ttu-id="ba7c2-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="ba7c2-102">[x] Proposed</span></span>
* <span data-ttu-id="ba7c2-103">[] Prototyp: keine</span><span class="sxs-lookup"><span data-stu-id="ba7c2-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="ba7c2-104">[] Implementierung: keine</span><span class="sxs-lookup"><span data-stu-id="ba7c2-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="ba7c2-105">[] Spezifikation: gestartet, unten</span><span class="sxs-lookup"><span data-stu-id="ba7c2-105">[ ] Specification: Started, below</span></span>

## <a name="summary"></a><span data-ttu-id="ba7c2-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="ba7c2-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ba7c2-107">Unterstützen Sie einen Ausdruck der Form `await? e`, der `e` erwartet, wenn er nicht NULL ist, andernfalls führt er zu `null`.</span><span class="sxs-lookup"><span data-stu-id="ba7c2-107">Support an expression of the form `await? e`, which awaits `e` if it is non-null, otherwise it results in `null`.</span></span>

## <a name="motivation"></a><span data-ttu-id="ba7c2-108">Motivation</span><span class="sxs-lookup"><span data-stu-id="ba7c2-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="ba7c2-109">Dabei handelt es sich um ein gängiges Codierungs Muster, und dieses Feature hätte eine schöne Zusammenarbeit mit den vorhandenen NULL-propagierenden und NULL-Sammel Operatoren.</span><span class="sxs-lookup"><span data-stu-id="ba7c2-109">This is a common coding pattern, and this feature would have nice synergy with the existing null-propagating and null-coalescing operators.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="ba7c2-110">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="ba7c2-110">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="ba7c2-111">Wir fügen eine neue Form der *await_expression*hinzu:</span><span class="sxs-lookup"><span data-stu-id="ba7c2-111">We add a new form of the *await_expression*:</span></span>

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

<span data-ttu-id="ba7c2-112">Der NULL bedingte `await` Operator erwartet nur dann seinen Operanden, wenn dieser Operand nicht NULL ist.</span><span class="sxs-lookup"><span data-stu-id="ba7c2-112">The null-conditional `await` operator awaits its operand only if that operand is non-null.</span></span> <span data-ttu-id="ba7c2-113">Andernfalls ist das Ergebnis der Anwendung des Operators NULL.</span><span class="sxs-lookup"><span data-stu-id="ba7c2-113">Otherwise the result of applying the operator is null.</span></span>

<span data-ttu-id="ba7c2-114">Der Ergebnistyp wird mithilfe der [Regeln für den NULL bedingten Operator](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator)berechnet.</span><span class="sxs-lookup"><span data-stu-id="ba7c2-114">The type of the result is computed using the [rules for the null-conditional operator](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).</span></span>

> <span data-ttu-id="ba7c2-115">**Hinweis:** Wenn `e` vom Typ `Task`ist, würde `await? e;` nichts tun, wenn `e` `null`ist, und `e`, wenn es nicht `null`ist.</span><span class="sxs-lookup"><span data-stu-id="ba7c2-115">**NOTE:** If `e` is of type `Task`, then `await? e;` would do nothing if `e` is `null`, and await `e` if it is not `null`.</span></span>
>
> <span data-ttu-id="ba7c2-116">Wenn `e` vom Typ `Task<K>` ist, bei dem `K` ein Werttyp ist, würde `await? e` einen Wert vom Typ `K?`ergeben.</span><span class="sxs-lookup"><span data-stu-id="ba7c2-116">If `e` is of type `Task<K>` where `K` is a value type, then `await? e` would yield a value of type `K?`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="ba7c2-117">Nachteile</span><span class="sxs-lookup"><span data-stu-id="ba7c2-117">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="ba7c2-118">Wie bei allen Sprach Features müssen wir Fragen, ob die zusätzliche Komplexität der Sprache in der zusätzlichen Klarheit für den Text der C# Programme, die von der Funktion profitieren würden, zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="ba7c2-118">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="ba7c2-119">Alternativen</span><span class="sxs-lookup"><span data-stu-id="ba7c2-119">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="ba7c2-120">Obwohl es Code Bausteine erfordert, kann die Verwendung dieses Operators häufig durch einen Ausdruck wie `(e == null) ? null : await e` oder eine Anweisung wie `if (e != null) await e`ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="ba7c2-120">Although it requires some boilerplate code, uses of this operator can often be replaced by an expression something like `(e == null) ? null : await e` or a statement like `if (e != null) await e`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="ba7c2-121">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="ba7c2-121">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="ba7c2-122">[] Erfordert eine LDM-Überprüfung</span><span class="sxs-lookup"><span data-stu-id="ba7c2-122">[ ] Requires LDM review</span></span>

## <a name="design-meetings"></a><span data-ttu-id="ba7c2-123">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="ba7c2-123">Design meetings</span></span>

<span data-ttu-id="ba7c2-124">None.</span><span class="sxs-lookup"><span data-stu-id="ba7c2-124">None.</span></span>
