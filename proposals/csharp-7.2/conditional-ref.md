---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483667"
---
# <a name="conditional-ref-expressions"></a><span data-ttu-id="9931e-101">Bedingte Verweis Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="9931e-101">Conditional ref expressions</span></span>

<span data-ttu-id="9931e-102">Das Muster, mit dem eine Verweis Variable an einen oder einen anderen Ausdruck bedingt gebunden wird, ist C#in derzeit nicht ausdrucksfähig.</span><span class="sxs-lookup"><span data-stu-id="9931e-102">The pattern of binding a ref variable to one or another expression conditionally is not currently expressible in C#.</span></span>

<span data-ttu-id="9931e-103">Die typische Problem Umgehung besteht darin, eine Methode wie die folgende einzuführen:</span><span class="sxs-lookup"><span data-stu-id="9931e-103">The typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="9931e-104">Beachten Sie, dass dies keine genaue Ersetzung eines ternären ist, da alle Argumente an der aufrufssite ausgewertet werden müssen.</span><span class="sxs-lookup"><span data-stu-id="9931e-104">Note that this is not an exact replacement of a ternary since all arguments must be evaluated at the call site.</span></span>

<span data-ttu-id="9931e-105">Folgendes funktioniert nicht wie erwartet:</span><span class="sxs-lookup"><span data-stu-id="9931e-105">The following will not work as expected:</span></span>

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

<span data-ttu-id="9931e-106">Die vorgeschlagene Syntax sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="9931e-106">The proposed syntax would look like:</span></span>

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

<span data-ttu-id="9931e-107">Der obige Versuch mit "Choice" kann mithilfe von "Ref ternäre" _Ordnungs_ gemäß geschrieben werden:</span><span class="sxs-lookup"><span data-stu-id="9931e-107">The above attempt with "Choice" can be _correctly_ written using ref ternary as:</span></span>

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="9931e-108">Der Unterschied besteht darin, dass die Folge und Alternative Ausdrücke _auf eine ordnungs_ gemäß bedingte Weise aufgerufen werden. Daher wird kein Absturz angezeigt, wenn ```arr == null```</span><span class="sxs-lookup"><span data-stu-id="9931e-108">The difference from Choice is that consequence and alternative expressions are accessed in a _truly_ conditional manner, so we do not see a crash if ```arr == null```</span></span>

<span data-ttu-id="9931e-109">Der ternäre Verweis ist nur ein ternärer Verweis, bei dem Alternative und Folge Refs sind.</span><span class="sxs-lookup"><span data-stu-id="9931e-109">The ternary ref is just a ternary where both alternative and consequence are refs.</span></span> <span data-ttu-id="9931e-110">Dies erfordert natürlich, dass die Folge/Alternative Operanden Lvalues sind.</span><span class="sxs-lookup"><span data-stu-id="9931e-110">It will naturally require that consequence/alternative operands are LValues.</span></span> <span data-ttu-id="9931e-111">Außerdem ist es erforderlich, dass diese Folge und Alternative über Typen verfügen, die in einander als Identitätswechsel konvertierbar sind.</span><span class="sxs-lookup"><span data-stu-id="9931e-111">It will also require that consequence and alternative have types that are identity convertible to each other.</span></span>

<span data-ttu-id="9931e-112">Der Typ des Ausdrucks wird ähnlich wie der für die reguläre ternäre berechnet.</span><span class="sxs-lookup"><span data-stu-id="9931e-112">The type of the expression will be computed similarly to the one for the regular ternary.</span></span> <span data-ttu-id="9931e-113">Das heißt,</span><span class="sxs-lookup"><span data-stu-id="9931e-113">I.E.</span></span> <span data-ttu-id="9931e-114">in einem Fall, wenn eine Folge und eine Alternative Identitäts konvertierbar, aber unterschiedliche Typen aufweisen, gelten die vorhandenen Regeln zum Zusammenführen von Typen.</span><span class="sxs-lookup"><span data-stu-id="9931e-114">in a case if consequence and alternative have identity convertible, but different types, the existing type-merging rules will apply.</span></span>

<span data-ttu-id="9931e-115">"Safe-to-return" wird von den bedingten Operanden konservativ angenommen.</span><span class="sxs-lookup"><span data-stu-id="9931e-115">Safe-to-return will be assumed conservatively from the conditional operands.</span></span> <span data-ttu-id="9931e-116">Wenn eine der beiden Optionen unsicher ist, ist die Rückgabe unsicher.</span><span class="sxs-lookup"><span data-stu-id="9931e-116">If either is unsafe to return the whole thing is unsafe to return.</span></span>

<span data-ttu-id="9931e-117">Ref ternäre ist ein Lvalue und kann daher als Verweis erfolgreich/zugewiesen/zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="9931e-117">Ref ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="9931e-118">Dabei kann es sich um einen lvalue-Wert handeln, der ebenfalls zugewiesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="9931e-118">Being an LValue, it can also be assigned to.</span></span> 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

<span data-ttu-id="9931e-119">Ref ternäre kann auch in einem regulären (nicht Verweis-) Kontext verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9931e-119">Ref ternary can be used in a regular (not ref) context as well.</span></span> <span data-ttu-id="9931e-120">Obwohl es nicht üblich wäre, sollten Sie nur ein reguläres Ternäres verwenden.</span><span class="sxs-lookup"><span data-stu-id="9931e-120">Although it would not be common since you could as well just use a regular ternary.</span></span>

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

<span data-ttu-id="9931e-121">Implementierungs Hinweise:</span><span class="sxs-lookup"><span data-stu-id="9931e-121">Implementation notes:</span></span> 

<span data-ttu-id="9931e-122">Die Komplexität der Implementierung wäre anscheinend die Größe einer mittelgroßen zu großen Fehlerbehebung.</span><span class="sxs-lookup"><span data-stu-id="9931e-122">The complexity of the implementation would seem to be the size of a moderate-to-large bug fix.</span></span> <span data-ttu-id="9931e-123">-I. E ist nicht sehr aufwendig.</span><span class="sxs-lookup"><span data-stu-id="9931e-123">- I.E not very expensive.</span></span>
<span data-ttu-id="9931e-124">Ich denke nicht, dass wir Änderungen an der Syntax oder der Verarbeitung benötigen.</span><span class="sxs-lookup"><span data-stu-id="9931e-124">I do not think we need any changes to the syntax or parsing.</span></span>
<span data-ttu-id="9931e-125">Es gibt keine Auswirkung auf Metadaten oder Interop.</span><span class="sxs-lookup"><span data-stu-id="9931e-125">There is no effect on metadata or interop.</span></span> <span data-ttu-id="9931e-126">Die Funktion ist vollständig Ausdrucks basiert.</span><span class="sxs-lookup"><span data-stu-id="9931e-126">The feature is completely expression based.</span></span>
<span data-ttu-id="9931e-127">Keine Auswirkung auf Debuggen/PDB</span><span class="sxs-lookup"><span data-stu-id="9931e-127">No effect on debugging/PDB either</span></span>
