---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483457"
---
# <a name="covariant-return-types"></a><span data-ttu-id="460ee-101">kovariante Rückgabe Typen</span><span class="sxs-lookup"><span data-stu-id="460ee-101">covariant return types</span></span>

* <span data-ttu-id="460ee-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="460ee-102">[x] Proposed</span></span>
* <span data-ttu-id="460ee-103">[] Prototyp: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="460ee-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="460ee-104">[] Implementierung: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="460ee-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="460ee-105">[] Spezifikation: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="460ee-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="460ee-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="460ee-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="460ee-107">Unterstützung _kovariant-Rückgabe Typen_.</span><span class="sxs-lookup"><span data-stu-id="460ee-107">Support _covariant return types_.</span></span> <span data-ttu-id="460ee-108">Insbesondere ermöglichen Sie einer über schreibenden Methode einen stärker abgeleiteten Verweistyp als die Methode, die Sie überschreibt.</span><span class="sxs-lookup"><span data-stu-id="460ee-108">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span>

## <a name="motivation"></a><span data-ttu-id="460ee-109">Motivation</span><span class="sxs-lookup"><span data-stu-id="460ee-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="460ee-110">Es ist ein gängiges Muster im Code, dass verschiedene Methodennamen erfunden werden müssen, um die sprach Einschränkung zu umgehen, dass über schreibungen denselben Typ zurückgeben müssen wie die überschriebene Methode.</span><span class="sxs-lookup"><span data-stu-id="460ee-110">It is a common pattern in code that different method names have to be invented to work around the language constraint that overrides must return the same type as the overridden method.</span></span> <span data-ttu-id="460ee-111">Im folgenden finden Sie ein Beispiel aus der Roslyn-Codebasis.</span><span class="sxs-lookup"><span data-stu-id="460ee-111">See below for an example from the Roslyn code base.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="460ee-112">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="460ee-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="460ee-113">Unterstützung _kovariant-Rückgabe Typen_.</span><span class="sxs-lookup"><span data-stu-id="460ee-113">Support _covariant return types_.</span></span> <span data-ttu-id="460ee-114">Insbesondere ermöglichen Sie einer über schreibenden Methode einen stärker abgeleiteten Verweistyp als die Methode, die Sie überschreibt.</span><span class="sxs-lookup"><span data-stu-id="460ee-114">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span> <span data-ttu-id="460ee-115">Dies gilt für Methoden und Eigenschaften und wird in Klassen und Schnittstellen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="460ee-115">This would apply to methods and properties, and be supported in classes and interfaces.</span></span>

<span data-ttu-id="460ee-116">Dies wäre für das Factorymuster nützlich.</span><span class="sxs-lookup"><span data-stu-id="460ee-116">This would be useful in the factory pattern.</span></span> <span data-ttu-id="460ee-117">In der Roslyn-Codebasis haben wir beispielsweise Folgendes:</span><span class="sxs-lookup"><span data-stu-id="460ee-117">For example, in the Roslyn code base we would have</span></span>

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

<span data-ttu-id="460ee-118">Die Implementierung dieser Methode besteht darin, dass der Compiler die über schreibende Methode als "neue" virtuelle Methode ausgibt, die die Basisklassen Methode blendet, zusammen mit einer _Bridge Methode_ , die die Basisklassen Methode mit einem aufzurufenden Befehl an die Methode der abgeleiteten Klasse implementiert.</span><span class="sxs-lookup"><span data-stu-id="460ee-118">The implementation of this would be for the compiler to emit the overriding method as a "new" virtual method that hides the base class method, along with a _bridge method_ that implements the base class method with a call to the derived class method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="460ee-119">Nachteile</span><span class="sxs-lookup"><span data-stu-id="460ee-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="460ee-120">[] Jede sprach Änderung muss für sich selbst bezahlen.</span><span class="sxs-lookup"><span data-stu-id="460ee-120">[ ] Every language change must pay for itself.</span></span>
- <span data-ttu-id="460ee-121">[] Wir sollten sicherstellen, dass die Leistung angemessen ist, auch im Fall von tiefen Vererbungs Hierarchien.</span><span class="sxs-lookup"><span data-stu-id="460ee-121">[ ] We should ensure that the performance is reasonable, even in the case of deep inheritance hierarchies</span></span>
- <span data-ttu-id="460ee-122">[] Wir sollten sicherstellen, dass sich Artefakte der Übersetzungsstrategie nicht auf die sprach Semantik auswirken, auch wenn Sie eine neue Il aus alten Compilern nutzen.</span><span class="sxs-lookup"><span data-stu-id="460ee-122">[ ] We should ensure that artifacts of the translation strategy do not affect language semantics, even when consuming new IL from old compilers.</span></span>

## <a name="alternatives"></a><span data-ttu-id="460ee-123">Alternativen</span><span class="sxs-lookup"><span data-stu-id="460ee-123">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="460ee-124">Wir könnten die Sprachregeln so lockern, dass Sie in der Quelle</span><span class="sxs-lookup"><span data-stu-id="460ee-124">We could relax the language rules slightly to allow, in source,</span></span>

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a><span data-ttu-id="460ee-125">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="460ee-125">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="460ee-126">[] Wie funktionieren APIs, die zur Verwendung dieses Features kompiliert wurden, in älteren Versionen der Sprache?</span><span class="sxs-lookup"><span data-stu-id="460ee-126">[ ] How will APIs that have been compiled to use this feature work in older versions of the language?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="460ee-127">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="460ee-127">Design meetings</span></span>

<span data-ttu-id="460ee-128">Noch keine.</span><span class="sxs-lookup"><span data-stu-id="460ee-128">None yet.</span></span> <span data-ttu-id="460ee-129">Bei <https://github.com/dotnet/roslyn/issues/357>wurden einige Diskussionen erläutert.</span><span class="sxs-lookup"><span data-stu-id="460ee-129">There has been some discussion at <https://github.com/dotnet/roslyn/issues/357>.</span></span>