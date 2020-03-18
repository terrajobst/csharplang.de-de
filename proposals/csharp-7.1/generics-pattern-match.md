---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483673"
---
# <a name="pattern-matching-with-generics"></a><span data-ttu-id="09f70-101">Muster Vergleich mit Generika</span><span class="sxs-lookup"><span data-stu-id="09f70-101">pattern-matching with generics</span></span>

* <span data-ttu-id="09f70-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="09f70-102">[x] Proposed</span></span>
* <span data-ttu-id="09f70-103">[] Prototyp:</span><span class="sxs-lookup"><span data-stu-id="09f70-103">[ ] Prototype:</span></span>
* <span data-ttu-id="09f70-104">[]-Implementierung:</span><span class="sxs-lookup"><span data-stu-id="09f70-104">[ ] Implementation:</span></span>
* <span data-ttu-id="09f70-105">[]-Spezifikation:</span><span class="sxs-lookup"><span data-stu-id="09f70-105">[ ] Specification:</span></span>

## <a name="summary"></a><span data-ttu-id="09f70-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="09f70-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="09f70-107">Die Spezifikation für den [vorhandenen C# as-Operator](../../spec/expressions.md#the-as-operator) ermöglicht es, keine Konvertierung zwischen dem Typ des Operanden und dem angegebenen Typ durchzusetzen, wenn ein offener Typ ist.</span><span class="sxs-lookup"><span data-stu-id="09f70-107">The specification for the [existing C# as operator](../../spec/expressions.md#the-as-operator) permits there to be no conversion between the type of the operand and the specified type when either is an open type.</span></span> <span data-ttu-id="09f70-108">In C# 7 erfordert das `Type identifier` Muster jedoch eine Konvertierung zwischen dem Typ der Eingabe und dem angegebenen Typ.</span><span class="sxs-lookup"><span data-stu-id="09f70-108">However, in C# 7 the `Type identifier` pattern requires there be a conversion between the type of the input and the given type.</span></span>

<span data-ttu-id="09f70-109">Es wird vorgeschlagen, diesen Vorgang zu lockern und `expression is Type identifier`zu ändern, zusätzlich zu den Bedingungen, die in C# 7 zulässig sind, auch zulässig, wenn `expression as Type` zulässig wäre.</span><span class="sxs-lookup"><span data-stu-id="09f70-109">We propose to relax this and change `expression is Type identifier`, in addition to being permitted in the conditions when it is permitted in C# 7, to also be permitted when `expression as Type` would be allowed.</span></span> <span data-ttu-id="09f70-110">Insbesondere handelt es sich bei den neuen Fällen um Fälle, in denen der Typ des Ausdrucks oder der angegebene Typ ein offener Typ ist.</span><span class="sxs-lookup"><span data-stu-id="09f70-110">Specifically, the new cases are cases where the type of the expression or the specified type is an open type.</span></span> 

## <a name="motivation"></a><span data-ttu-id="09f70-111">Motivation</span><span class="sxs-lookup"><span data-stu-id="09f70-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="09f70-112">Fälle, in denen Muster Vergleiche "offensichtlich" zugelassen werden, können derzeit nicht kompiliert werden.</span><span class="sxs-lookup"><span data-stu-id="09f70-112">Cases where pattern-matching should "obviously" be permitted currently fail to compile.</span></span> <span data-ttu-id="09f70-113">Siehe z. b. https://github.com/dotnet/roslyn/issues/16195.</span><span class="sxs-lookup"><span data-stu-id="09f70-113">See, for example, https://github.com/dotnet/roslyn/issues/16195.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="09f70-114">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="09f70-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="09f70-115">Wir ändern den Absatz in der Spezifikation für die Muster Übereinstimmung (die vorgeschlagene Addition ist fett dargestellt):</span><span class="sxs-lookup"><span data-stu-id="09f70-115">We change the paragraph in the pattern-matching specification (the proposed addition is shown in bold):</span></span>

> <span data-ttu-id="09f70-116">Bestimmte Kombinationen von statischem Typ der linken Seite und des angegebenen Typs werden als nicht kompatibel betrachtet und führen zu einem Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="09f70-116">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="09f70-117">Ein Wert vom Typ "statischer Typ `E`" ist mit dem Typ " *Muster kompatibel* " `T`, wenn eine Identitäts Konvertierung, eine implizite Verweis Konvertierung, eine Boxing-Konvertierung, eine explizite Verweis Konvertierung oder eine Unboxing-Konvertierung von `E` in `T`vorhanden **ist, oder wenn entweder `E` oder `T` ein offener Typ ist**.</span><span class="sxs-lookup"><span data-stu-id="09f70-117">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`**, or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="09f70-118">Es handelt sich um einen Kompilierzeitfehler, wenn ein Ausdruck vom Typ `E` nicht mit dem Typ in einem Typmuster kompatibel ist, mit dem er übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="09f70-118">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="09f70-119">Nachteile</span><span class="sxs-lookup"><span data-stu-id="09f70-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="09f70-120">None.</span><span class="sxs-lookup"><span data-stu-id="09f70-120">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="09f70-121">Alternativen</span><span class="sxs-lookup"><span data-stu-id="09f70-121">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="09f70-122">None.</span><span class="sxs-lookup"><span data-stu-id="09f70-122">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="09f70-123">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="09f70-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="09f70-124">None.</span><span class="sxs-lookup"><span data-stu-id="09f70-124">None.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="09f70-125">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="09f70-125">Design meetings</span></span>

<span data-ttu-id="09f70-126">LDM hat diese Frage in Erwägung gezogen und gespürt, dass es sich um eine Fehler Behebungs Ebene handelt.</span><span class="sxs-lookup"><span data-stu-id="09f70-126">LDM considered this question and felt it was a bug-fix level change.</span></span> <span data-ttu-id="09f70-127">Wir behandeln Sie als separate Sprachfunktion, da die Änderung nach der Veröffentlichung der Sprache eine vorwärts Inkompatibilität mit sich bringen würde.</span><span class="sxs-lookup"><span data-stu-id="09f70-127">We are treating it as a separate language feature because just making the change after the language has been released would introduce a forward incompatibility.</span></span> <span data-ttu-id="09f70-128">Für die Verwendung der vorgeschlagenen Änderung muss der Programmierer Sprachversion 7,1 angeben.</span><span class="sxs-lookup"><span data-stu-id="09f70-128">Using the proposed change requires that the programmer specify language version 7.1.</span></span>
