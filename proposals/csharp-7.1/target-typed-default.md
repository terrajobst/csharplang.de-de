---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "79483913"
---
# <a name="target-typed-default-literal"></a><span data-ttu-id="ea36b-101">"Standardliteralname" mit Zieltyp</span><span class="sxs-lookup"><span data-stu-id="ea36b-101">Target-typed "default" literal</span></span>

* <span data-ttu-id="ea36b-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="ea36b-102">[x] Proposed</span></span>
* <span data-ttu-id="ea36b-103">[x] Prototyp</span><span class="sxs-lookup"><span data-stu-id="ea36b-103">[x] Prototype</span></span>
* <span data-ttu-id="ea36b-104">[x]-Implementierung</span><span class="sxs-lookup"><span data-stu-id="ea36b-104">[x] Implementation</span></span>
* <span data-ttu-id="ea36b-105">[]-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="ea36b-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="ea36b-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="ea36b-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ea36b-107">Die `default` Funktion mit Zieltyp ist eine kürzere Formular Variation des `default(T)`-Operators, sodass der Typ ausgelassen werden kann.</span><span class="sxs-lookup"><span data-stu-id="ea36b-107">The target-typed `default` feature is a shorter form variation of the `default(T)` operator, which allows the type to be omitted.</span></span> <span data-ttu-id="ea36b-108">Der Typ wird stattdessen von der Ziel Typisierung abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="ea36b-108">Its type is inferred by target-typing instead.</span></span> <span data-ttu-id="ea36b-109">Abgesehen davon verhält sie sich wie `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="ea36b-109">Aside from that, it behaves like `default(T)`.</span></span>

## <a name="motivation"></a><span data-ttu-id="ea36b-110">Motivation</span><span class="sxs-lookup"><span data-stu-id="ea36b-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="ea36b-111">Die Hauptmotivation besteht darin, redundante Informationen zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="ea36b-111">The main motivation is to avoid typing redundant information.</span></span>

<span data-ttu-id="ea36b-112">Wenn beispielsweise `void Method(ImmutableArray<SomeType> array)`aufgerufen wird, lässt *default* das standardliteralzeichen `M(default)` anstelle von `M(default(ImmutableArray<SomeType>))`zu.</span><span class="sxs-lookup"><span data-stu-id="ea36b-112">For instance, when invoking `void Method(ImmutableArray<SomeType> array)`, the *default* literal allows `M(default)` in place of `M(default(ImmutableArray<SomeType>))`.</span></span>

<span data-ttu-id="ea36b-113">Dies gilt für eine Reihe von Szenarien, wie z. b.:</span><span class="sxs-lookup"><span data-stu-id="ea36b-113">This is applicable in a number of scenarios, such as:</span></span>

- <span data-ttu-id="ea36b-114">Deklarieren von lokalen Variablen (`ImmutableArray<SomeType> x = default;`)</span><span class="sxs-lookup"><span data-stu-id="ea36b-114">declaring locals (`ImmutableArray<SomeType> x = default;`)</span></span>
- <span data-ttu-id="ea36b-115">TERNÄRE Vorgänge (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span><span class="sxs-lookup"><span data-stu-id="ea36b-115">ternary operations (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span></span>
- <span data-ttu-id="ea36b-116">zurückgeben in Methoden und Lambdas (`return default;`)</span><span class="sxs-lookup"><span data-stu-id="ea36b-116">returning in methods and lambdas (`return default;`)</span></span>
- <span data-ttu-id="ea36b-117">Deklarieren von Standardwerten für optionale Parameter (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span><span class="sxs-lookup"><span data-stu-id="ea36b-117">declaring default values for optional parameters (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span></span>
- <span data-ttu-id="ea36b-118">einschließen von Standardwerten in Array Erstellungs Ausdrücken (`var x = new[] { default, ImmutableArray.Create(y) };`)</span><span class="sxs-lookup"><span data-stu-id="ea36b-118">including default values in array creation expressions (`var x = new[] { default, ImmutableArray.Create(y) };`)</span></span>


## <a name="detailed-design"></a><span data-ttu-id="ea36b-119">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="ea36b-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="ea36b-120">Ein neuer Ausdruck wird eingeführt, das *default* standardliteralzeichen.</span><span class="sxs-lookup"><span data-stu-id="ea36b-120">A new expression is introduced, the *default* literal.</span></span> <span data-ttu-id="ea36b-121">Ein Ausdruck mit dieser Klassifizierung kann durch eine *standardliteralkonvertierung*implizit in einen beliebigen Typ konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="ea36b-121">An expression with this classification can be implicitly converted to any type, by a *default literal conversion*.</span></span> 

<span data-ttu-id="ea36b-122">Das Ableiten des Typs für das standardliteralformat funktioniert genauso wie für das *null* -Literale, mit dem Unterschied, dass ein beliebiger Typ zulässig ist (nicht nur Verweis Typen). *default*</span><span class="sxs-lookup"><span data-stu-id="ea36b-122">The inference of the type for the *default* literal works the same as that for the *null* literal, except that any type is allowed (not just reference types).</span></span>

<span data-ttu-id="ea36b-123">Diese Konvertierung erzeugt den Standardwert des abhergenten Typs.</span><span class="sxs-lookup"><span data-stu-id="ea36b-123">This conversion produces the default value of the inferred type.</span></span>

<span data-ttu-id="ea36b-124">Das *standardliterale* kann abhängig vom abhergenten Typ einen konstanten Wert aufweisen.</span><span class="sxs-lookup"><span data-stu-id="ea36b-124">The *default* literal may have a constant value, depending on the inferred type.</span></span> <span data-ttu-id="ea36b-125">Daher ist `const int x = default;` Recht, aber `const int? y = default;` ist nicht.</span><span class="sxs-lookup"><span data-stu-id="ea36b-125">So `const int x = default;` is legal, but `const int? y = default;` is not.</span></span>

<span data-ttu-id="ea36b-126">Beim *default* standardliteralwert kann es sich um den Operanden von Gleichheits Operatoren handeln, solange der andere Operand einen Typ aufweist.</span><span class="sxs-lookup"><span data-stu-id="ea36b-126">The *default* literal can be the operand of equality operators, as long as the other operand has a type.</span></span> <span data-ttu-id="ea36b-127">Daher sind `default == x` und `x == default` gültige Ausdrücke, aber `default == default` ist unzulässig.</span><span class="sxs-lookup"><span data-stu-id="ea36b-127">So `default == x` and `x == default` are valid expressions, but `default == default` is illegal.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="ea36b-128">Nachteile</span><span class="sxs-lookup"><span data-stu-id="ea36b-128">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="ea36b-129">Ein geringfügiger Nachteil ist, dass in den meisten Kontexten anstelle von *null* -literalen *standardliterale* verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="ea36b-129">A minor drawback is that *default* literal can be used in place of *null* literal in most contexts.</span></span> <span data-ttu-id="ea36b-130">Zwei Ausnahmen sind `throw null;` und `null == null`, die für das *null* -Literalzeichen zulässig sind, aber nicht das standardliteralzeichen. *default*</span><span class="sxs-lookup"><span data-stu-id="ea36b-130">Two of the exceptions are `throw null;` and `null == null`, which are allowed for the *null* literal, but not the *default* literal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="ea36b-131">Alternativen</span><span class="sxs-lookup"><span data-stu-id="ea36b-131">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="ea36b-132">Es gibt einige Alternativen zu berücksichtigende Aspekte:</span><span class="sxs-lookup"><span data-stu-id="ea36b-132">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="ea36b-133">Der Status quo: das Feature ist nicht für seine eigenen Vorzüge gerechtfertigt, und Entwickler verwenden weiterhin den Standard Operator mit einem expliziten Typ.</span><span class="sxs-lookup"><span data-stu-id="ea36b-133">The status quo:  The feature is not justified on its own merits and developers continue to use the default operator with an explicit type.</span></span>
- <span data-ttu-id="ea36b-134">Erweitern des NULL-Literals: Dies ist der VB-Ansatz mit `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="ea36b-134">Extending the null literal: This is the VB approach with `Nothing`.</span></span> <span data-ttu-id="ea36b-135">Wir könnten `int x = null;`erlauben.</span><span class="sxs-lookup"><span data-stu-id="ea36b-135">We could allow `int x = null;`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="ea36b-136">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="ea36b-136">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="ea36b-137">[x] sollte *standardmäßig* als Operand des *is* -oder *As* -Operators zulässig sein?</span><span class="sxs-lookup"><span data-stu-id="ea36b-137">[x] Should *default* be allowed as the operand of the *is* or *as* operators?</span></span> <span data-ttu-id="ea36b-138">Antwort: nicht zulassen von `default is T`, zulassen von `x is default`, zulassen von `default as RefType` (bei Always-NULL-Warnung)</span><span class="sxs-lookup"><span data-stu-id="ea36b-138">Answer:  disallow `default is T`, allow `x is default`, allow `default as RefType` (with always-null warning)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="ea36b-139">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="ea36b-139">Design meetings</span></span>

- [<span data-ttu-id="ea36b-140">LDM 3/7/2017</span><span class="sxs-lookup"><span data-stu-id="ea36b-140">LDM 3/7/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [<span data-ttu-id="ea36b-141">LDM 3/28/2017</span><span class="sxs-lookup"><span data-stu-id="ea36b-141">LDM 3/28/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [<span data-ttu-id="ea36b-142">LDM 5/31/2017</span><span class="sxs-lookup"><span data-stu-id="ea36b-142">LDM 5/31/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
