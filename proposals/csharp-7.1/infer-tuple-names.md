---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483685"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a><span data-ttu-id="186fd-101">Ableiten von tupelnamen (auch als bezeichnet).</span><span class="sxs-lookup"><span data-stu-id="186fd-101">Infer tuple names (aka.</span></span> <span data-ttu-id="186fd-102">tupelprojektionsinitialisierer)</span><span class="sxs-lookup"><span data-stu-id="186fd-102">tuple projection initializers)</span></span>

## <a name="summary"></a><span data-ttu-id="186fd-103">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="186fd-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="186fd-104">In einer Reihe gängiger Fälle ermöglicht dieses Feature, dass die tupelelementnamen ausgelassen und stattdessen abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="186fd-104">In a number of common cases, this feature allows the tuple element names to be omitted and instead be inferred.</span></span> <span data-ttu-id="186fd-105">Anstatt `(f1: x.f1, f2: x?.f2)`einzugeben, können die Elementnamen "F1" und "F2" beispielsweise aus `(x.f1, x?.f2)`abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="186fd-105">For instance, instead of typing `(f1: x.f1, f2: x?.f2)`, the element names "f1" and "f2" can be inferred from `(x.f1, x?.f2)`.</span></span>

<span data-ttu-id="186fd-106">Dies ist vergleichbar mit dem Verhalten anonymer Typen, die das Ableiten von Elementnamen während der Erstellung ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="186fd-106">This parallels the behavior of  anonymous types, which allow inferring member names during creation.</span></span> <span data-ttu-id="186fd-107">Beispielsweise deklariert `new { x.f1, y?.f2 }` die Member "F1" und "F2".</span><span class="sxs-lookup"><span data-stu-id="186fd-107">For instance, `new { x.f1, y?.f2 }` declares members "f1" and "f2".</span></span>

<span data-ttu-id="186fd-108">Dies ist besonders bei der Verwendung von Tupeln in LINQ nützlich:</span><span class="sxs-lookup"><span data-stu-id="186fd-108">This is particularly handy when using tuples in LINQ:</span></span>

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a><span data-ttu-id="186fd-109">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="186fd-109">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="186fd-110">Die Änderung besteht aus zwei Teilen:</span><span class="sxs-lookup"><span data-stu-id="186fd-110">There are two parts to the change:</span></span>

1.  <span data-ttu-id="186fd-111">Versuchen Sie, einen Kandidaten Namen für jedes Tupelelement abzuleiten, das keinen expliziten Namen hat:</span><span class="sxs-lookup"><span data-stu-id="186fd-111">Try to infer a candidate name for each tuple element which does not have an explicit name:</span></span>
    -   <span data-ttu-id="186fd-112">Die gleichen Regeln wie bei der namens Ableitung für anonyme Typen werden verwendet.</span><span class="sxs-lookup"><span data-stu-id="186fd-112">Using same rules as name inference for anonymous types.</span></span>
        - <span data-ttu-id="186fd-113">In C#sind dies drei Fälle möglich: `y` (Bezeichner), `x.y` (einfacher Member-Zugriff) und `x?.y` (bedingter Zugriff).</span><span class="sxs-lookup"><span data-stu-id="186fd-113">In C#, this allows three cases: `y` (identifier), `x.y` (simple member access) and `x?.y` (conditional access).</span></span>
        - <span data-ttu-id="186fd-114">In VB ermöglicht dies zusätzliche Fälle, wie z. b. `x.y()`.</span><span class="sxs-lookup"><span data-stu-id="186fd-114">In VB, this allows for additional cases, such as `x.y()`.</span></span>
    -   <span data-ttu-id="186fd-115">Ablehnen von reservierten tupelnamen (unter C#Scheidung nach Groß-/Kleinschreibung in VB), da Sie entweder unzulässig oder bereits implizit sind.</span><span class="sxs-lookup"><span data-stu-id="186fd-115">Rejecting reserved tuple names (case-sensitive in C#, case-insensitive in VB), as they are either forbidden or already implicit.</span></span> <span data-ttu-id="186fd-116">Beispielsweise z. b. `ItemN`, `Rest`und `ToString`.</span><span class="sxs-lookup"><span data-stu-id="186fd-116">For instance, such as `ItemN`, `Rest`, and `ToString`.</span></span>
    -   <span data-ttu-id="186fd-117">Wenn es sich bei allen Kandidaten Namen um Duplikate handelt ( C#Groß-/Kleinschreibung in VB ohne Beachtung der Groß-/Kleinschreibung) innerhalb des gesamten Tupels, löschen wir diese Kandidaten.</span><span class="sxs-lookup"><span data-stu-id="186fd-117">If any candidate names are duplicates (case-sensitive in C#, case-insensitive in VB) within the entire tuple, we drop those candidates,</span></span>
2.  <span data-ttu-id="186fd-118">Bei Konvertierungen (bei denen das Ablegen von Namen aus tupelliteralen überprüft und gewarnt wird) werden keine Warnungen ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="186fd-118">During conversions (which check and warn about dropping names from tuple literals), inferred names would not produce any warnings.</span></span> <span data-ttu-id="186fd-119">Dadurch wird das Abbrechen von vorhandenem tupelcode vermieden.</span><span class="sxs-lookup"><span data-stu-id="186fd-119">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="186fd-120">Beachten Sie, dass die Regel für die Verarbeitung von Duplikaten von der Regel für anonyme Typen abweicht.</span><span class="sxs-lookup"><span data-stu-id="186fd-120">Note that the rule for handling duplicates is different than that for anonymous types.</span></span> <span data-ttu-id="186fd-121">Beispielsweise erzeugt `new { x.f1, x.f1 }` einen Fehler, aber `(x.f1, x.f1)` wäre weiterhin zulässig (nur ohne herabherzufügende Namen).</span><span class="sxs-lookup"><span data-stu-id="186fd-121">For instance, `new { x.f1, x.f1 }` produces an error, but `(x.f1, x.f1)` would still be allowed (just without any inferred names).</span></span> <span data-ttu-id="186fd-122">Dadurch wird das Abbrechen von vorhandenem tupelcode vermieden.</span><span class="sxs-lookup"><span data-stu-id="186fd-122">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="186fd-123">Aus Konsistenz Gründen gilt das gleiche für Tupel, die durch Debug-Zuweisungen (in C#) erstellt werden:</span><span class="sxs-lookup"><span data-stu-id="186fd-123">For consistency, the same would apply to tuples produced by deconstruction-assignments (in C#):</span></span>

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

<span data-ttu-id="186fd-124">Dasselbe gilt auch für VB-Tupel, wobei die VB-spezifischen Regeln für das Ableiten von Namen aus Ausdrücken und die Groß-/Kleinschreibung von namens vergleichen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="186fd-124">The same would also apply to VB tuples, using the VB-specific rules for inferring name from expression and case-insensitive name comparisons.</span></span>

<span data-ttu-id="186fd-125">Wenn Sie den C# 7,1-Compiler (oder höher) mit der Sprachversion "7,0" verwenden, werden die Elementnamen abgeleitet (obwohl das Feature nicht verfügbar ist), es wird jedoch ein Fehler bei der Verwendung des-Standorts für den Zugriff darauf ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="186fd-125">When using the C# 7.1 compiler (or later) with language version "7.0", the element names will be inferred (despite the feature not being available), but there will be a use-site error for trying to access them.</span></span> <span data-ttu-id="186fd-126">Dadurch wird das Hinzufügen von neuem Code, der später dem Kompatibilitätsproblem ausgesetzt wäre, eingeschränkt (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="186fd-126">This will limit additions of new code that would later face the compatibility issue (described below).</span></span>

## <a name="drawbacks"></a><span data-ttu-id="186fd-127">Nachteile</span><span class="sxs-lookup"><span data-stu-id="186fd-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="186fd-128">Der Hauptnachteil besteht darin, dass dadurch eine Kompatibilitäts C# Pause von 7,0 eingeführt wird:</span><span class="sxs-lookup"><span data-stu-id="186fd-128">The main drawback is that this introduces a compatibility break from C# 7.0:</span></span>

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

<span data-ttu-id="186fd-129">Der Kompatibilitäts Schluss fand diese Unterbrechung als akzeptabel, da Sie begrenzt ist und das Zeitfenster seit dem Versand von C# Tupeln (in 7,0) kurz ist.</span><span class="sxs-lookup"><span data-stu-id="186fd-129">The compatibility council found this break acceptable, given that it is limited and the time window since tuples shipped (in C# 7.0) is short.</span></span>

## <a name="references"></a><span data-ttu-id="186fd-130">Verweise</span><span class="sxs-lookup"><span data-stu-id="186fd-130">References</span></span>
- [<span data-ttu-id="186fd-131">LDM, 4. April 2017</span><span class="sxs-lookup"><span data-stu-id="186fd-131">LDM April 4th 2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- <span data-ttu-id="186fd-132">[GitHub-Diskussion](https://github.com/dotnet/csharplang/issues/370) (Danke @alrz, um dieses Problem zu beheben)</span><span class="sxs-lookup"><span data-stu-id="186fd-132">[Github discussion](https://github.com/dotnet/csharplang/issues/370) (thanks @alrz for bringing this issue up)</span></span>
- [<span data-ttu-id="186fd-133">Design von Tupeln</span><span class="sxs-lookup"><span data-stu-id="186fd-133">Tuples design</span></span>](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
