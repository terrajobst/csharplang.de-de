---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483523"
---
# <a name="nullable-enhanced-common-type"></a><span data-ttu-id="54d39-101">Nullable-Enhanced Common Type</span><span class="sxs-lookup"><span data-stu-id="54d39-101">Nullable-Enhanced Common Type</span></span>

* <span data-ttu-id="54d39-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="54d39-102">[x] Proposed</span></span>
* <span data-ttu-id="54d39-103">[] Prototyp: keine</span><span class="sxs-lookup"><span data-stu-id="54d39-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="54d39-104">[] Implementierung: keine</span><span class="sxs-lookup"><span data-stu-id="54d39-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="54d39-105">[] Spezifikation: siehe unten</span><span class="sxs-lookup"><span data-stu-id="54d39-105">[ ] Specification: See below</span></span>

## <a name="summary"></a><span data-ttu-id="54d39-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="54d39-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="54d39-107">Es gibt eine Situation, in der die aktuellen Algorithmus-Ergebnisse des allgemeinen Typs kontraintuitiv sind, und führt dazu, dass der Programmierer eine redundante Umwandlung zum Code hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="54d39-107">There is a situation in which the current common-type algorithm results are counter-intuitive, and results in the programmer adding what feels like a redundant cast to the code.</span></span> <span data-ttu-id="54d39-108">Bei dieser Änderung würde ein Ausdruck wie `condition ? 1 : null` einen Wert vom Typ "`int?`" ergeben, und ein Ausdruck wie `condition ? x : 1.0`, bei dem `x` vom Typ "`int?`" einen Wert vom Typ "`double?`" ergibt.</span><span class="sxs-lookup"><span data-stu-id="54d39-108">With this change, an expression such as `condition ? 1 : null` would result in a value of type `int?`, and an expression such as `condition ? x : 1.0` where `x` is of type `int?` would result in a value of type `double?`.</span></span>

## <a name="motivation"></a><span data-ttu-id="54d39-109">Motivation</span><span class="sxs-lookup"><span data-stu-id="54d39-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="54d39-110">Dies ist eine häufige Ursache dafür, wie sich der Programmierer wie der Code für unnötige Bausteine eignet.</span><span class="sxs-lookup"><span data-stu-id="54d39-110">This is a common cause of what feels to the programmer like needless boilerplate code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="54d39-111">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="54d39-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="54d39-112">Wir ändern die Spezifikation, um [den besten allgemeinen Typ eines Satzes von Ausdrücken zu ermitteln](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) , der sich auf die folgenden Situationen auswirkt:</span><span class="sxs-lookup"><span data-stu-id="54d39-112">We modify the specification for [finding the best common type of a set of expressions](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) to affect the following situations:</span></span>

- <span data-ttu-id="54d39-113">Wenn ein Ausdruck aus einem Werttyp, der nicht auf NULL festgelegt werden kann `T` und der andere ein NULL-Literalwert ist, ist das Ergebnis vom Typ `T?`.</span><span class="sxs-lookup"><span data-stu-id="54d39-113">If one expression is of a non-nullable value type `T` and the other is a null literal, the result is of type `T?`.</span></span>
- <span data-ttu-id="54d39-114">Wenn ein Ausdruck einen Werttyp hat, der NULL-Werte zulässt `T?` und der andere einen Werttyp `U`und eine implizite Konvertierung von `T` in `U`vorliegt, ist das Ergebnis vom Typ `U?`.</span><span class="sxs-lookup"><span data-stu-id="54d39-114">If one expression is of a nullable value type `T?` and the other is of a value type `U`, and there is an implicit conversion from `T` to `U`, then the result is of type `U?`.</span></span>

<span data-ttu-id="54d39-115">Dies wirkt sich auf die folgenden Aspekte der Sprache aus:</span><span class="sxs-lookup"><span data-stu-id="54d39-115">This is expected to affect the following aspects of the language:</span></span>

- <span data-ttu-id="54d39-116">der [ternäre Ausdruck](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span><span class="sxs-lookup"><span data-stu-id="54d39-116">the [ternary expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span></span>
- <span data-ttu-id="54d39-117">implizit typisierter [Array Erstellungs Ausdruck](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span><span class="sxs-lookup"><span data-stu-id="54d39-117">implicitly typed [array creation expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span></span>
- <span data-ttu-id="54d39-118">Ableiten des [Rückgabe Typs eines Lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) -Ausdrucks für den Typrückschluss</span><span class="sxs-lookup"><span data-stu-id="54d39-118">inferring the [return type of a lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) for type inference</span></span>
- <span data-ttu-id="54d39-119">Fälle mit Generika, z. b. das Aufrufen von `M<T>(T a, T b)` als `M(1, null)`.</span><span class="sxs-lookup"><span data-stu-id="54d39-119">cases involving generics, such as invoking `M<T>(T a, T b)` as `M(1, null)`.</span></span>

<span data-ttu-id="54d39-120">Genauer ist, dass wir die folgenden Abschnitte der Spezifikation ändern (Einfügungen in Fett Formatierung, Löschungen in durch Strichen):</span><span class="sxs-lookup"><span data-stu-id="54d39-120">More precisely, we change the following sections of the specification (insertions in bold, deletions in strikethrough):</span></span>

> #### <a name="output-type-inferences"></a><span data-ttu-id="54d39-121">Ausschlüsse auf Ausgabetyp</span><span class="sxs-lookup"><span data-stu-id="54d39-121">Output type inferences</span></span>
> 
> <span data-ttu-id="54d39-122">Ein *ausgabetyprückschluss* erfolgt auf folgende Weise *von* einem Ausdruck `E` *auf* einen Typ `T`:</span><span class="sxs-lookup"><span data-stu-id="54d39-122">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>
> 
> *  <span data-ttu-id="54d39-123">Wenn `E` eine anonyme Funktion mit dem abzurufenden Rückgabetyp `U` (abzurufender[Rückgabetyp](expressions.md#inferred-return-type)) ist und `T` ein Delegattyp oder ein Ausdrucks Strukturtyp mit dem Rückgabetyp `Tb`ist, wird ein untergeordneter Daten *Rückschluss* ([niedrigerer gebundener Schlussfolgerung](expressions.md#lower-bound-inferences)) *von* `U` *bis* `Tb`vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="54d39-123">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="54d39-124">Andernfalls, wenn `E` eine Methoden Gruppe ist und `T` ein Delegattyp oder ein Ausdrucks Strukturtyp mit Parametertypen `T1...Tk` und der Rückgabetyp `Tb`ist und die Überladungs Auflösung von `E` mit den Typen `T1...Tk` eine einzelne Methode mit dem Rückgabetyp `U`ergibt, wird ein *niedrigerer gebundener Rückschluss* *von* `U` *bis* `Tb`ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="54d39-124">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="54d39-125">\* \* Andernfalls, wenn `E` ein Ausdruck mit einem Werttyp, der NULL-Werte zulässt `U?`ist, wird ein Rückschluss *von* `U` *zum* `T` hergestellt, und `T`wird eine NULL *-* *Grenze* hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="54d39-125">\*\*Otherwise, if `E` is an expression with nullable value type `U?`, then a *lower-bound inference* is made *from* `U` *to* `T` and a *null bound* is added to `T`.</span></span> **
> *  <span data-ttu-id="54d39-126">Andernfalls, wenn `E` ein Ausdruck mit dem Typ `U`ist, wird ein Rück *Schluss* *von* `U` *zum* `T`gemacht.</span><span class="sxs-lookup"><span data-stu-id="54d39-126">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
> *  <span data-ttu-id="54d39-127">**Andernfalls, wenn `E` ein konstanter Ausdruck mit einem Wert `null`ist, wird eine *null-Grenze* zu `T`**</span><span class="sxs-lookup"><span data-stu-id="54d39-127">**Otherwise, if `E` is a constant expression with value `null`, then a *null bound* is added to `T`**</span></span> 
> *  <span data-ttu-id="54d39-128">Andernfalls werden keine Rückschlüsse gemacht.</span><span class="sxs-lookup"><span data-stu-id="54d39-128">Otherwise, no inferences are made.</span></span>

> #### <a name="fixing"></a><span data-ttu-id="54d39-129">Festnahme</span><span class="sxs-lookup"><span data-stu-id="54d39-129">Fixing</span></span>
> 
> <span data-ttu-id="54d39-130">Eine Variable vom Typ " *unfixed* " `Xi` mit einem Satz von Begrenzungen ist wie folgt *festgelegt* :</span><span class="sxs-lookup"><span data-stu-id="54d39-130">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>
> 
> *  <span data-ttu-id="54d39-131">Der Satz von *Kandidaten Typen* `Uj` beginnt als Satz aller Typen im Satz von Begrenzungen für `Xi`.</span><span class="sxs-lookup"><span data-stu-id="54d39-131">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
> *  <span data-ttu-id="54d39-132">Anschließend untersuchen wir die einzelnen gebundenen `Xi` wiederum: für jede exakte gebundene `U` `Xi` alle Typen `Uj` die nicht mit `U` identisch sind, werden aus dem Kandidaten Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="54d39-132">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="54d39-133">Für jede Untergrenze `U` `Xi` alle Typen `Uj` die *keine* implizite Konvertierung von `U` aus dem Kandidaten Satz entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="54d39-133">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="54d39-134">Für jede obere Grenze `U` `Xi` alle Typen `Uj` aus denen *keine* implizite Konvertierung in `U` aus dem Kandidaten Satz entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="54d39-134">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
> *  <span data-ttu-id="54d39-135">Wenn zwischen den verbleibenden Kandidaten Typen `Uj` ein eindeutiger Typ `V` der eine implizite Konvertierung in alle anderen Kandidaten Typen enthält, ~~wird`Xi` auf `V`korrigiert.~~</span><span class="sxs-lookup"><span data-stu-id="54d39-135">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then ~~`Xi` is fixed to `V`.~~</span></span>
>     -  <span data-ttu-id="54d39-136">**Wenn `V` ein Werttyp ist und ein NULL-Wert an `Xi`*gebunden\* ist, wird `Xi` auf `V?`*\*</span><span class="sxs-lookup"><span data-stu-id="54d39-136">**If `V` is a value type and there is a *null bound* for `Xi`, then `Xi` is fixed to `V?`**</span></span>
>     -  <span data-ttu-id="54d39-137">**Andernfalls wird `Xi` korrigiert `V`**</span><span class="sxs-lookup"><span data-stu-id="54d39-137">**Otherwise   `Xi` is fixed to `V`**</span></span>
> *  <span data-ttu-id="54d39-138">Andernfalls schlägt der Typrückschluss fehl.</span><span class="sxs-lookup"><span data-stu-id="54d39-138">Otherwise, type inference fails.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="54d39-139">Nachteile</span><span class="sxs-lookup"><span data-stu-id="54d39-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="54d39-140">Es gibt möglicherweise einige Inkompatibilitäten, die von diesem Vorschlag eingeführt werden.</span><span class="sxs-lookup"><span data-stu-id="54d39-140">There may be some incompatibilities introduced by this proposal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="54d39-141">Alternativen</span><span class="sxs-lookup"><span data-stu-id="54d39-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="54d39-142">None.</span><span class="sxs-lookup"><span data-stu-id="54d39-142">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="54d39-143">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="54d39-143">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="54d39-144">[] Was ist der Schweregrad der Inkompatibilität, der von diesem Vorschlag eingeführt wurde, und wie kann er moderiert werden?</span><span class="sxs-lookup"><span data-stu-id="54d39-144">[ ] What is the severity of incompatibility introduced by this proposal, if any, and how can it be moderated?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="54d39-145">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="54d39-145">Design meetings</span></span>

<span data-ttu-id="54d39-146">None.</span><span class="sxs-lookup"><span data-stu-id="54d39-146">None.</span></span>
