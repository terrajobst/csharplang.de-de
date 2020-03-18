---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2019
ms.locfileid: "79483889"
---
# <a name="null-coalescing-assignment"></a><span data-ttu-id="44b3b-101">Koaleszierende NULL-Zuweisung</span><span class="sxs-lookup"><span data-stu-id="44b3b-101">null coalescing assignment</span></span>

* <span data-ttu-id="44b3b-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="44b3b-102">[x] Proposed</span></span>
* <span data-ttu-id="44b3b-103">[] Prototyp: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="44b3b-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="44b3b-104">[] Implementierung: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="44b3b-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="44b3b-105">[] Spezifikation: unten</span><span class="sxs-lookup"><span data-stu-id="44b3b-105">[ ] Specification: Below</span></span>

## <a name="summary"></a><span data-ttu-id="44b3b-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="44b3b-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="44b3b-107">Vereinfacht ein gängiges Codierungs Muster, bei dem einer Variablen ein Wert zugewiesen wird, wenn Sie NULL ist.</span><span class="sxs-lookup"><span data-stu-id="44b3b-107">Simplifies a common coding pattern where a variable is assigned a value if it is null.</span></span>

<span data-ttu-id="44b3b-108">Im Rahmen dieses Angebots werden auch die typanforderungen auf `??` gelockert, damit ein Ausdruck, dessen Typ ein uneingeschränkter Typparameter ist, auf der linken Seite verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="44b3b-108">As part of this proposal, we will also loosen the type requirements on `??` to allow an expression whose type is an unconstrained type parameter to be used on the left-hand side.</span></span>

## <a name="motivation"></a><span data-ttu-id="44b3b-109">Motivation</span><span class="sxs-lookup"><span data-stu-id="44b3b-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="44b3b-110">Es kommt häufig vor, dass Code des Formulars angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="44b3b-110">It is common to see code of the form</span></span>

```csharp
if (variable == null)
{
    variable = expression;
}
```

<span data-ttu-id="44b3b-111">In diesem Vorschlag wird der Sprache, die diese Funktion ausführt, ein nicht über ladbarer binärer Operator hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="44b3b-111">This proposal adds a non-overloadable binary operator to the language that performs this function.</span></span>

<span data-ttu-id="44b3b-112">Für dieses Feature gab es mindestens acht communityanforderungen.</span><span class="sxs-lookup"><span data-stu-id="44b3b-112">There have been at least eight separate community requests for this feature.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="44b3b-113">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="44b3b-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="44b3b-114">Wir fügen eine neue Form von Zuweisungs Operatoren hinzu.</span><span class="sxs-lookup"><span data-stu-id="44b3b-114">We add a new form of assignment operator</span></span>

``` antlr
assignment_operator
    : '??='
    ;
```

<span data-ttu-id="44b3b-115">Der den [vorhandenen Semantik Regeln für Verbund Zuweisungs Operatoren](../../spec/expressions.md#compound-assignment)folgt, mit dem Unterschied, dass die Zuweisung entfernt wird, wenn die linke Seite nicht NULL ist.</span><span class="sxs-lookup"><span data-stu-id="44b3b-115">Which follows the [existing semantic rules for compound assignment operators](../../spec/expressions.md#compound-assignment), except that we elide the assignment if the left-hand side is non-null.</span></span> <span data-ttu-id="44b3b-116">Die Regeln für dieses Feature lauten wie folgt.</span><span class="sxs-lookup"><span data-stu-id="44b3b-116">The rules for this feature are as follows.</span></span>

<span data-ttu-id="44b3b-117">Wenn `A` der `a``a ??= b`ist, ist `B` der Typ des `b`, und `A0` ist der zugrunde liegende Typ von `A`, wenn `A` ein Werttyp ist, der NULL-Werte zulässt:</span><span class="sxs-lookup"><span data-stu-id="44b3b-117">Given `a ??= b`, where `A` is the type of `a`, `B` is the type of `b`, and `A0` is the underlying type of `A` if `A` is a nullable value type:</span></span>

1. <span data-ttu-id="44b3b-118">Wenn `A` nicht vorhanden ist oder ein nicht auf NULL festleg barer Werttyp ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="44b3b-118">If `A` does not exist or is a non-nullable value type, a compile-time error occurs.</span></span>
2. <span data-ttu-id="44b3b-119">Wenn `B` nicht implizit in `A` oder `A0` konvertiert werden kann (wenn `A0` vorhanden ist), tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="44b3b-119">If `B` is not implicitly convertible to `A` or `A0` (if `A0` exists), a compile-time error occurs.</span></span>
3. <span data-ttu-id="44b3b-120">Wenn `A0` vorhanden ist und `B` implizit in `A0`konvertierbar ist und `B` nicht dynamisch ist, wird der Typ des `a ??= b` `A0`.</span><span class="sxs-lookup"><span data-stu-id="44b3b-120">If `A0` exists and `B` is implicitly convertible to `A0`, and `B` is not dynamic, then the type of `a ??= b` is `A0`.</span></span> <span data-ttu-id="44b3b-121">`a ??= b` wird zur Laufzeit wie folgt ausgewertet:</span><span class="sxs-lookup"><span data-stu-id="44b3b-121">`a ??= b` is evaluated at runtime as:</span></span>
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   <span data-ttu-id="44b3b-122">Mit der Ausnahme, dass `a` nur einmal ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="44b3b-122">Except that `a` is only evaluated once.</span></span>
4. <span data-ttu-id="44b3b-123">Andernfalls wird der Typ des `a ??= b` `A`.</span><span class="sxs-lookup"><span data-stu-id="44b3b-123">Otherwise, the type of `a ??= b` is `A`.</span></span> <span data-ttu-id="44b3b-124">`a ??= b` wird zur Laufzeit als `a ?? (a = b)`ausgewertet, mit dem Unterschied, dass `a` nur einmal ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="44b3b-124">`a ??= b` is evaluated at runtime as `a ?? (a = b)`, except that `a` is only evaluated once.</span></span>


<span data-ttu-id="44b3b-125">Um die typanforderungen `??`zu erfüllen, aktualisieren wir die Spezifikation, in der Sie derzeit angibt, dass `a ?? b`, wobei `A` der `a`ist:</span><span class="sxs-lookup"><span data-stu-id="44b3b-125">For the relaxation of the type requirements of `??`, we update the spec where it currently states that, given `a ?? b`, where `A` is the type of `a`:</span></span>

> 1. <span data-ttu-id="44b3b-126">Wenn ein vorhanden ist und kein Werte zulässt-Typ oder Verweistyp ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="44b3b-126">If A exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>

<span data-ttu-id="44b3b-127">Wir lockern diese Anforderung für Folgendes:</span><span class="sxs-lookup"><span data-stu-id="44b3b-127">We relax this requirement to:</span></span>

1. <span data-ttu-id="44b3b-128">Wenn eine vorhanden ist und ein Werttyp ist, der keine NULL-Werte zulässt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="44b3b-128">If A exists and is a non-nullable value type, a compile-time error occurs.</span></span>

<span data-ttu-id="44b3b-129">Dadurch kann der NULL-Sammel Operator an nicht eingeschränkten Typparametern arbeiten, da der nicht eingeschränkte Typparameter T vorhanden ist, kein Werte zulässt-Typ ist und kein Referenztyp ist.</span><span class="sxs-lookup"><span data-stu-id="44b3b-129">This allows the null coalescing operator to work on unconstrained type parameters, as the unconstrained type parameter T exists, is not a nullable type, and is not a reference type.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="44b3b-130">Nachteile</span><span class="sxs-lookup"><span data-stu-id="44b3b-130">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="44b3b-131">Wie bei allen Sprach Features müssen wir Fragen, ob die zusätzliche Komplexität der Sprache in der zusätzlichen Klarheit für den Text der C# Programme, die von der Funktion profitieren würden, zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="44b3b-131">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="44b3b-132">Alternativen</span><span class="sxs-lookup"><span data-stu-id="44b3b-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="44b3b-133">Der Programmierer kann `(x = x ?? y)`, `if (x == null) x = y;`oder `x ?? (x = y)` per Hand schreiben.</span><span class="sxs-lookup"><span data-stu-id="44b3b-133">The programmer can write `(x = x ?? y)`, `if (x == null) x = y;`, or `x ?? (x = y)` by hand.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="44b3b-134">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="44b3b-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="44b3b-135">[] Erfordert eine LDM-Überprüfung</span><span class="sxs-lookup"><span data-stu-id="44b3b-135">[ ] Requires LDM review</span></span>
- <span data-ttu-id="44b3b-136">[] Sollten auch `&&=`-und `||=`-Operatoren unterstützt werden?</span><span class="sxs-lookup"><span data-stu-id="44b3b-136">[ ] Should we also support `&&=` and `||=` operators?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="44b3b-137">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="44b3b-137">Design meetings</span></span>

<span data-ttu-id="44b3b-138">None.</span><span class="sxs-lookup"><span data-stu-id="44b3b-138">None.</span></span>
