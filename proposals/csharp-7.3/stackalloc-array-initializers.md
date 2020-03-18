---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483589"
---
# <a name="stackalloc-array-initializers"></a><span data-ttu-id="802ae-101">Stackalloc-Arrayinitialisierer</span><span class="sxs-lookup"><span data-stu-id="802ae-101">Stackalloc array initializers</span></span>

## <a name="summary"></a><span data-ttu-id="802ae-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="802ae-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="802ae-103">Ermöglicht die Verwendung der arrayinitialisierersyntax mit `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="802ae-103">Allow array initializer syntax to be used with `stackalloc`.</span></span>

## <a name="motivation"></a><span data-ttu-id="802ae-104">Motivation</span><span class="sxs-lookup"><span data-stu-id="802ae-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="802ae-105">Bei normalen Arrays können ihre Elemente zum Zeitpunkt der Erstellung initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="802ae-105">Ordinary arrays can have their elements initialized at creation time.</span></span> <span data-ttu-id="802ae-106">Es scheint sinnvoll, dies in `stackalloc` Fall zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="802ae-106">It seems reasonable to allow that in `stackalloc` case.</span></span>

<span data-ttu-id="802ae-107">Die Frage, warum eine solche Syntax mit `stackalloc` nicht zulässig ist, wird recht häufig angezeigt.</span><span class="sxs-lookup"><span data-stu-id="802ae-107">The question of why such syntax is not allowed with `stackalloc` arises fairly frequently.</span></span>  
<span data-ttu-id="802ae-108">Siehe z. b. [#1112](https://github.com/dotnet/csharplang/issues/1112)</span><span class="sxs-lookup"><span data-stu-id="802ae-108">See, for example, [#1112](https://github.com/dotnet/csharplang/issues/1112)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="802ae-109">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="802ae-109">Detailed design</span></span>

<span data-ttu-id="802ae-110">Normale Arrays können mithilfe der folgenden Syntax erstellt werden:</span><span class="sxs-lookup"><span data-stu-id="802ae-110">Ordinary arrays can be created through the following syntax:</span></span>

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

<span data-ttu-id="802ae-111">Die Erstellung von Stapel zugeordneten Arrays sollte durch folgende Aktionen zugelassen werden:</span><span class="sxs-lookup"><span data-stu-id="802ae-111">We should allow stack allocated arrays be created through:</span></span>  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

<span data-ttu-id="802ae-112">Die Semantik aller Fälle ist ungefähr mit Arrays identisch.</span><span class="sxs-lookup"><span data-stu-id="802ae-112">The semantics of all cases is roughly the same as with arrays.</span></span>  
<span data-ttu-id="802ae-113">Beispiel: im letzten Fall wird der Elementtyp vom Initialisierer abgeleitet und muss ein nicht verwalteter Typ sein.</span><span class="sxs-lookup"><span data-stu-id="802ae-113">For example: in the last case the element type is inferred from the initializer and must be an "unmanaged" type.</span></span>

<span data-ttu-id="802ae-114">Hinweis: das Feature ist nicht davon abhängig, dass es sich bei dem Ziel um eine `Span<T>`handelt.</span><span class="sxs-lookup"><span data-stu-id="802ae-114">NOTE: the feature is not dependent on the target being a `Span<T>`.</span></span> <span data-ttu-id="802ae-115">Dies gilt auch für `T*` Fall, sodass es nicht sinnvoll erscheint, es in `Span<T>` Fall zu kennzeichnen.</span><span class="sxs-lookup"><span data-stu-id="802ae-115">It is just as applicable in `T*` case, so it does not seem reasonable to predicate it on `Span<T>` case.</span></span>  

## <a name="translation"></a><span data-ttu-id="802ae-116">Übersetzung</span><span class="sxs-lookup"><span data-stu-id="802ae-116">Translation</span></span>

<span data-ttu-id="802ae-117">Die naive Implementierung könnte das Array einfach direkt nach der Erstellung durch eine Reihe von Element weisen Zuweisungen initialisieren.</span><span class="sxs-lookup"><span data-stu-id="802ae-117">The naive implementation could just initialize the array right after creation through a series of element-wise assignments.</span></span>  

<span data-ttu-id="802ae-118">Ähnlich wie bei Arrays kann es möglich und wünschenswert sein, Fälle zu erkennen, in denen alle oder die meisten Elemente blitfähige Typen sind, und effizientere Techniken zu verwenden, indem Sie den vorab erstellten Zustand aller Konstanten Elemente kopieren.</span><span class="sxs-lookup"><span data-stu-id="802ae-118">Similarly to the case with arrays, it might be possible and desirable to detect cases where all or most of the elements are blittable types and use more efficient techniques by copying over the pre-created state of all the constant elements.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="802ae-119">Nachteile</span><span class="sxs-lookup"><span data-stu-id="802ae-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

## <a name="alternatives"></a><span data-ttu-id="802ae-120">Alternativen</span><span class="sxs-lookup"><span data-stu-id="802ae-120">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="802ae-121">Dies ist ein praktisches Feature.</span><span class="sxs-lookup"><span data-stu-id="802ae-121">This is a convenience feature.</span></span> <span data-ttu-id="802ae-122">Es ist möglich, nichts zu tun.</span><span class="sxs-lookup"><span data-stu-id="802ae-122">It is possible to just do nothing.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="802ae-123">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="802ae-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="802ae-124">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="802ae-124">Design meetings</span></span>

<span data-ttu-id="802ae-125">Noch keine.</span><span class="sxs-lookup"><span data-stu-id="802ae-125">None yet.</span></span> 
