---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483475"
---
# <a name="static-delegates"></a><span data-ttu-id="b126c-101">Statische Delegaten</span><span class="sxs-lookup"><span data-stu-id="b126c-101">Static Delegates</span></span>

* <span data-ttu-id="b126c-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="b126c-102">[x] Proposed</span></span>
* <span data-ttu-id="b126c-103">[] Prototyp: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="b126c-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="b126c-104">[] Implementierung: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="b126c-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="b126c-105">[] Spezifikation: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="b126c-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="b126c-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="b126c-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b126c-107">Stellen Sie eine allgemeine, leichte Rückruffunktion für die C# Sprache bereit.</span><span class="sxs-lookup"><span data-stu-id="b126c-107">Provide a general-purpose, lightweight callback capability to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="b126c-108">Motivation</span><span class="sxs-lookup"><span data-stu-id="b126c-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b126c-109">Heute haben Benutzer die Möglichkeit, Rückrufe mithilfe des `System.Delegate` Typs zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b126c-109">Today, users have the ability to create callbacks using the `System.Delegate` type.</span></span> <span data-ttu-id="b126c-110">Diese sind jedoch recht schwer zu handhaben (z. b. Wenn eine Heap Zuordnung erforderlich ist und Sie immer mit dem Verketten von Rückrufe arbeiten).</span><span class="sxs-lookup"><span data-stu-id="b126c-110">However, these are fairly heavyweight (such as requiring a heap allocation and always having handling for chaining callbacks together).</span></span>

<span data-ttu-id="b126c-111">Außerdem bietet `System.Delegate` keinen optimalen Interop mit nicht verwalteten Funktions Zeigern, d. h. aufgrund nicht blitfähiger und erforderlicher Marshalling, wenn die verwaltete/nicht verwaltete Grenze überschritten wird.</span><span class="sxs-lookup"><span data-stu-id="b126c-111">Additionally, `System.Delegate` does not provide the best interop with unmanaged function pointers, namely due being non-blittable and requiring marshalling anytime it crosses the managed/unmanaged boundary.</span></span>

<span data-ttu-id="b126c-112">Mit einigen geringfügigen Anpassungen könnten wir einen neuen Typ von Delegaten bereitstellen, der einfach, universell und gut mit System eigenem Code ist.</span><span class="sxs-lookup"><span data-stu-id="b126c-112">With a few minor tweaks, we could provide a new type of delegate that is lightweight, general-purpose, and interops well with native code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b126c-113">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="b126c-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="b126c-114">Zum einen deklarieren Sie einen statischen Delegaten über Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b126c-114">One would declare a static delegate via the following:</span></span>

```C#
static delegate int Func()
```

<span data-ttu-id="b126c-115">Sie können der Deklaration außerdem ein Attribut ähnlich `System.Runtime.InteropServices.UnmanagedFunctionPointer` zuweisen, sodass die Aufruf Konvention, das Marshalling von Zeichen folgen und das Festlegen des letzten Fehler Verhaltens gesteuert werden kann.</span><span class="sxs-lookup"><span data-stu-id="b126c-115">One could additionally attribute the declaration with something similar to `System.Runtime.InteropServices.UnmanagedFunctionPointer` so that the calling convention, string marshalling, and set last error behavior can be controlled.</span></span> <span data-ttu-id="b126c-116">Hinweis: die Verwendung von `System.Runtime.InteropServices.UnmanagedFunctionPointer` selbst funktioniert nicht, da Sie nur für Delegaten verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b126c-116">NOTE: Using `System.Runtime.InteropServices.UnmanagedFunctionPointer` itself will not work, as it is only usable on Delegates.</span></span>

<span data-ttu-id="b126c-117">Die Deklaration würde vom Compiler in eine interne Darstellung übersetzt werden, die der folgenden ähnelt.</span><span class="sxs-lookup"><span data-stu-id="b126c-117">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

<span data-ttu-id="b126c-118">Das heißt, Sie wird intern durch eine Struktur dargestellt, die über einen einzelnen Member vom Typ "`IntPtr`" verfügt (eine solche Struktur ist blitfähig und verursacht keine Heap Zuweisungen):</span><span class="sxs-lookup"><span data-stu-id="b126c-118">That is to say, it is internally represented by a struct that has a single member of type `IntPtr` (such a struct is blittable and does not incur any heap allocations):</span></span>
* <span data-ttu-id="b126c-119">Der Member enthält die Adresse der Funktion, die als Rückruf fungieren soll.</span><span class="sxs-lookup"><span data-stu-id="b126c-119">The member contains the address of the function that is to be the callback.</span></span>
* <span data-ttu-id="b126c-120">Der-Typ deklariert eine Methode, die mit der Methoden Signatur des Rückrufs übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="b126c-120">The type declares a method matching the method signature of the callback.</span></span>
* <span data-ttu-id="b126c-121">Der Name der Struktur sollte nicht vom Benutzer konstruiert werden können (wie bei anderen intern generierten Unterstützungsstrukturen).</span><span class="sxs-lookup"><span data-stu-id="b126c-121">The name of the struct should not be user-constructible (as we do with other internally generated backing structures).</span></span>
 * <span data-ttu-id="b126c-122">Beispiel: Puffer fester Größe generieren eine Struktur mit einem Namen im Format `<name>e__FixedBuffer` (`<` und `>` sind Teil des Bezeichners und sorgen dafür, dass der Bezeichner nicht in C#konstruktierbar ist, aber trotzdem in Il verwendbar ist).</span><span class="sxs-lookup"><span data-stu-id="b126c-122">For example: fixed size buffers generate a struct with a name in the format of `<name>e__FixedBuffer` (`<` and `>` are part of the identifier and make the identifier not constructible in C#, but still useable in IL).</span></span>
* <span data-ttu-id="b126c-123">Der Name der Methoden Deklaration sollte ein bekannter Name sein, der für alle statischen Delegattypen verwendet wird (Dies ermöglicht es dem Compiler, den Namen zu ermitteln, der beim Bestimmen der Signatur gesucht werden soll).</span><span class="sxs-lookup"><span data-stu-id="b126c-123">The name of the method declaration should be a well known name used across all static delegate types (this allows the compiler to know the name to look for when determining the signature).</span></span>

<span data-ttu-id="b126c-124">Der Wert des statischen Delegaten kann nur an eine statische Methode gebunden werden, die mit der Signatur des Rückrufs übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="b126c-124">The value of the static delegate can only be bound to a static method that matches the signature of the callback.</span></span>

<span data-ttu-id="b126c-125">Das Verketten von Rückrufe wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b126c-125">Chaining callbacks together is not supported.</span></span>

<span data-ttu-id="b126c-126">Der Aufruf des Rückrufs würde von der `calli` Anweisung implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="b126c-126">Invocation of the callback would be implemented by the `calli` instruction.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="b126c-127">Nachteile</span><span class="sxs-lookup"><span data-stu-id="b126c-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="b126c-128">Statische Delegaten können nicht mit vorhandenen APIs verwendet werden, die reguläre Delegaten verwenden (eine muss den genannten statischen Delegaten in einem regulären Delegaten derselben Signatur einschließen).</span><span class="sxs-lookup"><span data-stu-id="b126c-128">Static Delegates would not work with existing APIs that use regular delegates (one would need to wrap said static delegate in a regular delegate of the same signature).</span></span>
* <span data-ttu-id="b126c-129">Da `System.Delegate` intern als Satz von `object`-und `IntPtr` Feldern (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs)) dargestellt wird, ist es möglich, die implizite Konvertierung eines statischen Delegaten in einen `System.Delegate` zuzulassen, der über eine übereinstimmende Methoden Signatur verfügt.</span><span class="sxs-lookup"><span data-stu-id="b126c-129">Given that `System.Delegate` is represented internally as a set of `object` and `IntPtr` fields (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), it would be possible to allow implicit conversion of a static delegate to a `System.Delegate` that has a matching method signature.</span></span> <span data-ttu-id="b126c-130">Es wäre auch möglich, eine explizite Konvertierung in umgekehrter Richtung bereitzustellen, sofern die `System.Delegate` für alle Anforderungen eines statischen Delegaten konform ist.</span><span class="sxs-lookup"><span data-stu-id="b126c-130">It would also be possible to provide an explicit conversion in the opposite direction, provided the `System.Delegate` conformed to all the requirements of being a static delegate.</span></span>

<span data-ttu-id="b126c-131">Es ist zusätzliche Arbeit erforderlich, um den statischen Delegaten im Kern Framework problemlos verwendbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="b126c-131">Additional work would be needed to make Static Delegate readily usable in the core framework.</span></span>

## <a name="alternatives"></a><span data-ttu-id="b126c-132">Alternativen</span><span class="sxs-lookup"><span data-stu-id="b126c-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="b126c-133">TBD</span><span class="sxs-lookup"><span data-stu-id="b126c-133">TBD</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="b126c-134">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="b126c-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="b126c-135">Welche Teile des Entwurfs sind noch TBD?</span><span class="sxs-lookup"><span data-stu-id="b126c-135">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="b126c-136">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="b126c-136">Design meetings</span></span>

<span data-ttu-id="b126c-137">Verknüpfen Sie die Entwurfs Hinweise, die sich auf diesen Vorschlag auswirken, und beschreiben Sie in einem Satz für jede Änderungen, zu denen Sie geführt haben.</span><span class="sxs-lookup"><span data-stu-id="b126c-137">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>


