---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483571"
---
# <a name="fixed-sized-buffers"></a><span data-ttu-id="e7d7c-101">Puffer fester Größe</span><span class="sxs-lookup"><span data-stu-id="e7d7c-101">Fixed Sized Buffers</span></span>

* <span data-ttu-id="e7d7c-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="e7d7c-102">[x] Proposed</span></span>
* <span data-ttu-id="e7d7c-103">[] Prototyp: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="e7d7c-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="e7d7c-104">[] Implementierung: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="e7d7c-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="e7d7c-105">[] Spezifikation: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="e7d7c-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="e7d7c-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="e7d7c-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="e7d7c-107">Stellen Sie einen allgemeinen und sicheren Mechanismus zum Deklarieren von Puffer fester Größe in C# der Sprache bereit.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-107">Provide a general-purpose and safe mechanism for declaring fixed sized buffers to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="e7d7c-108">Motivation</span><span class="sxs-lookup"><span data-stu-id="e7d7c-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="e7d7c-109">Heute haben Benutzer die Möglichkeit, Puffer fester Größe in einem unsicheren Kontext zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-109">Today, users have the ability to create fixed-sized buffers in an unsafe-context.</span></span> <span data-ttu-id="e7d7c-110">Dies erfordert jedoch, dass der Benutzer mit Zeigern umgeht, Begrenzungen Überprüfungen manuell durchführt und nur einen begrenzten Satz von Typen (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`und `double`) unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-110">However, this requires the user to deal with pointers, manually perform bounds checks, and only supports a limited set of types (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`, and `double`).</span></span>

<span data-ttu-id="e7d7c-111">Die häufigste Beschwerde ist, dass Puffer fester Größe nicht in sicherem Code indiziert werden können.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-111">The most common complaint is that fixed-size buffers cannot be indexed in safe code.</span></span> <span data-ttu-id="e7d7c-112">Die zweite Möglichkeit, mehr Typen zu verwenden, ist die zweite.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-112">Inability to use more types is the second.</span></span>

<span data-ttu-id="e7d7c-113">Mit einigen geringfügigen Anpassungen können wir allgemeine Puffer mit fester Größe bereitstellen, die jeden Typ unterstützen, in einem sicheren Kontext verwendet werden können und automatische Begrenzungen überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-113">With a few minor tweaks, we could provide general-purpose fixed-sized buffers which support any type, can be used in a safe context, and have automatic bounds checking performed.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="e7d7c-114">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="e7d7c-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="e7d7c-115">Ein Puffer mit fester Größe wird wie folgt deklariert:</span><span class="sxs-lookup"><span data-stu-id="e7d7c-115">One would declare a safe fixed-sized buffer via the following:</span></span>

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

<span data-ttu-id="e7d7c-116">Die Deklaration würde vom Compiler in eine interne Darstellung übersetzt werden, die der folgenden ähnelt.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-116">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

<span data-ttu-id="e7d7c-117">Da solche Puffer keine `fixed`mehr benötigen, ist es sinnvoll, einen beliebigen Elementtyp zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-117">Since such fixed-sized buffers no longer require use of `fixed`, it makes sense to allow any element type.</span></span>  

> <span data-ttu-id="e7d7c-118">Hinweis: `fixed` wird weiterhin unterstützt, aber nur, wenn der Elementtyp `blittable`</span><span class="sxs-lookup"><span data-stu-id="e7d7c-118">NOTE: `fixed` will still be supported, but only if the element type is `blittable`</span></span>

## <a name="drawbacks"></a><span data-ttu-id="e7d7c-119">Nachteile</span><span class="sxs-lookup"><span data-stu-id="e7d7c-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

* <span data-ttu-id="e7d7c-120">Es könnten einige Probleme mit der Abwärtskompatibilität bestehen, da die vorhandenen Puffer mit fester Größe jedoch nur mit einer Auswahl primitiver Typen funktionieren, sollte der Compiler den "Just-Working"-Vorgang fortsetzen können, wenn der Benutzer den Fixed-Buffer als Zeichner.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-120">There could be some challenges with backwards compatibility, but given that the existing fixed-sized buffers only work with a selection of primitive types, it should be possible for the compiler to continue "just-working" if the user treats the fixed-buffer as a pointer.</span></span>
* <span data-ttu-id="e7d7c-121">Nicht kompatible Konstrukte müssen möglicherweise eine etwas andere `v2` Codierung verwenden, um die Felder des alten Compilers auszublenden.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-121">Incompatible constructs may need to use slightly different `v2` encoding to hide the fields from old compiler.</span></span>
* <span data-ttu-id="e7d7c-122">Das Packen ist in Il-Spezifikationen für generische Typen nicht gut definiert.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-122">Packing is not well defined in IL spec for generic types.</span></span> <span data-ttu-id="e7d7c-123">Während der Ansatz funktionieren sollte, werden wir auf nicht dokumentiertes Verhalten angrenzen.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-123">While the approach should work, we will be bordering on undocumented behavior.</span></span> <span data-ttu-id="e7d7c-124">Wir sollten dies dokumentieren und sicherstellen, dass andere JITs wie Mono das gleiche Verhalten aufweisen.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-124">We should make that documented and make sure other JITs like Mono have the same behavior.</span></span>
* <span data-ttu-id="e7d7c-125">Die Angabe eines separaten Typs für jede Länge (eine möglicherweise eine andere für `readonly` Felder, falls unterstützt) wirkt sich auf Metadaten aus.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-125">Specifying a separate type for every length (an possibly another for `readonly` fields, if supported) will have impact on metadata.</span></span> <span data-ttu-id="e7d7c-126">Sie wird von der Anzahl der Arrays unterschiedlicher Größen in der angegebenen app gebunden.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-126">It will be bound by the number of arrays of different sizes in the given app.</span></span>
* <span data-ttu-id="e7d7c-127">`ref` Math ist nicht formal überprüfbar (da Sie unsicher ist).</span><span class="sxs-lookup"><span data-stu-id="e7d7c-127">`ref` math is not formally verifiable (since it is unsafe).</span></span> <span data-ttu-id="e7d7c-128">Wir müssen eine Möglichkeit finden, die Überprüfungs Regeln zu aktualisieren, um zu wissen, dass die Verwendung in Ordnung ist.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-128">We will need to find a way to update verification rules to know that our use is ok.</span></span>

## <a name="alternatives"></a><span data-ttu-id="e7d7c-129">Alternativen</span><span class="sxs-lookup"><span data-stu-id="e7d7c-129">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="e7d7c-130">Deklarieren Sie Ihre Strukturen manuell, und verwenden Sie unsicheren Code zum Erstellen von Indexer.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-130">Manually declare your structures and use unsafe code to construct indexers.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="e7d7c-131">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="e7d7c-131">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="e7d7c-132">sollten wir `readonly`zulassen?</span><span class="sxs-lookup"><span data-stu-id="e7d7c-132">should we allow `readonly`?</span></span>  <span data-ttu-id="e7d7c-133">(mit Schreib geschütztem Indexer)</span><span class="sxs-lookup"><span data-stu-id="e7d7c-133">(with readonly indexer)</span></span>
- <span data-ttu-id="e7d7c-134">sollten Arrayinitialisierer zugelassen werden?</span><span class="sxs-lookup"><span data-stu-id="e7d7c-134">should we allow array initializers?</span></span>
- <span data-ttu-id="e7d7c-135">ist `fixed` Schlüsselwort erforderlich?</span><span class="sxs-lookup"><span data-stu-id="e7d7c-135">is `fixed` keyword necessary?</span></span>
- <span data-ttu-id="e7d7c-136">`foreach`?</span><span class="sxs-lookup"><span data-stu-id="e7d7c-136">`foreach`?</span></span>
- <span data-ttu-id="e7d7c-137">nur Instanzfelder in Strukturen?</span><span class="sxs-lookup"><span data-stu-id="e7d7c-137">only instance fields in structs?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="e7d7c-138">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="e7d7c-138">Design meetings</span></span>

<span data-ttu-id="e7d7c-139">Verknüpfen Sie die Entwurfs Hinweise, die sich auf diesen Vorschlag auswirken, und beschreiben Sie in einem Satz für jede Änderungen, zu denen Sie geführt haben.</span><span class="sxs-lookup"><span data-stu-id="e7d7c-139">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>