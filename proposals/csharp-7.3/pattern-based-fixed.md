---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483631"
---
# <a name="pattern-based-fixed-statement"></a><span data-ttu-id="69c69-101">Musterbasierte `fixed`-Anweisung</span><span class="sxs-lookup"><span data-stu-id="69c69-101">Pattern-based `fixed` statement</span></span>

## <a name="summary"></a><span data-ttu-id="69c69-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="69c69-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="69c69-103">Stellen Sie ein Muster vor, das es Typen ermöglicht, an `fixed`-Anweisungen teilzunehmen.</span><span class="sxs-lookup"><span data-stu-id="69c69-103">Introduce a pattern that would allow types to participate in `fixed` statements.</span></span> 

## <a name="motivation"></a><span data-ttu-id="69c69-104">Motivation</span><span class="sxs-lookup"><span data-stu-id="69c69-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="69c69-105">Die Sprache bietet einen Mechanismus zum Fixieren von verwalteten Daten und zum Abrufen eines systemeigenen Zeigers auf den zugrunde liegenden Puffer.</span><span class="sxs-lookup"><span data-stu-id="69c69-105">The language provides a mechanism for pinning managed data and obtain a native pointer to the underlying buffer.</span></span>

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

<span data-ttu-id="69c69-106">Der Satz von Typen, die an `fixed` teilnehmen können, ist hart codiert und auf Arrays und `System.String`beschränkt.</span><span class="sxs-lookup"><span data-stu-id="69c69-106">The set of types that can participate in `fixed` is hardcoded and limited to arrays and `System.String`.</span></span> <span data-ttu-id="69c69-107">Das hart codieren von "speziellen" Typen wird nicht skaliert, wenn neue Primitive wie `ImmutableArray<T>`, `Span<T>``Utf8String` eingeführt werden.</span><span class="sxs-lookup"><span data-stu-id="69c69-107">Hardcoding "special" types does not scale when new primitives such as `ImmutableArray<T>`, `Span<T>`, `Utf8String` are introduced.</span></span> 

<span data-ttu-id="69c69-108">Darüber hinaus basiert die aktuelle Lösung für `System.String` auf einer recht starren API.</span><span class="sxs-lookup"><span data-stu-id="69c69-108">In addition, the current solution for `System.String` relies on a fairly rigid API.</span></span> <span data-ttu-id="69c69-109">Die Form der API impliziert, dass `System.String` ein zusammenhängendes Objekt ist, das UTF16 codierte Daten an einem festgelegten Offset aus dem Objekt Header einbettet.</span><span class="sxs-lookup"><span data-stu-id="69c69-109">The shape of the API implies that `System.String` is a contiguous object that embeds UTF16 encoded data at a fixed offset from the object header.</span></span> <span data-ttu-id="69c69-110">Diese Vorgehensweise wurde in mehreren Vorschlägen als problematisch eingestuft, die Änderungen am zugrunde liegenden Layout erfordern könnten.</span><span class="sxs-lookup"><span data-stu-id="69c69-110">Such approach has been found problematic in several proposals that could require changes to the underlying layout.</span></span> <span data-ttu-id="69c69-111">Es wäre wünschenswert, zu einem flexibleren wechseln zu können, das `System.String` Objekt von seiner internen Darstellung zum Zweck der nicht verwalteten Interop entkoppelt.</span><span class="sxs-lookup"><span data-stu-id="69c69-111">It would be desirable to be able to switch to something more flexible that decouples `System.String` object from its internal representation for the purpose of unmanaged interop.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="69c69-112">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="69c69-112">Detailed design</span></span>
[design]: #detailed-design

## <a name="pattern"></a><span data-ttu-id="69c69-113">*Muster*</span><span class="sxs-lookup"><span data-stu-id="69c69-113">*Pattern*</span></span> ##
<span data-ttu-id="69c69-114">Ein brauchbares, auf Mustern basierendes "Festes" muss Folgendes sein:</span><span class="sxs-lookup"><span data-stu-id="69c69-114">A viable pattern-based “fixed” need to:</span></span>
-   <span data-ttu-id="69c69-115">Stellen Sie die verwalteten Verweise bereit, um die Instanz anzuheften und den Zeiger zu initialisieren (vorzugsweise ist dies derselbe Verweis).</span><span class="sxs-lookup"><span data-stu-id="69c69-115">Provide the managed references to pin the instance and to initialize the pointer (preferably this is the same reference)</span></span>
-   <span data-ttu-id="69c69-116">Vermitteln Sie eindeutig den Typ des nicht verwalteten Elements (d. h. "char" für "String")</span><span class="sxs-lookup"><span data-stu-id="69c69-116">Convey unambiguously the type of the unmanaged element   (i.e. “char” for “string”)</span></span>
-   <span data-ttu-id="69c69-117">Weisen Sie das Verhalten im "leeren" Fall an, wenn keine Verweise vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="69c69-117">Prescribe the behavior in "empty" case when there is nothing to refer to.</span></span> 
-   <span data-ttu-id="69c69-118">Sollten API-Autoren nicht in Bezug auf Entwurfsentscheidungen pushen, die die Verwendung des Typs außerhalb `fixed`beeinträchtigen.</span><span class="sxs-lookup"><span data-stu-id="69c69-118">Should not push API authors toward design decisions that hurt the use of the type outside of `fixed`.</span></span>

<span data-ttu-id="69c69-119">Ich bin der Ansicht, dass die oben genannten Elemente durch das Erkennen eines speziell benannten Ref-zurück gebenden Members erfüllt werden können: `ref [readonly] T GetPinnableReference()`.</span><span class="sxs-lookup"><span data-stu-id="69c69-119">I think the above could be satisfied by recognizing a specially named ref-returning member: `ref [readonly] T GetPinnableReference()`.</span></span>

<span data-ttu-id="69c69-120">Die folgenden Bedingungen müssen erfüllt sein, damit Sie von der `fixed`-Anweisung verwendet werden können:</span><span class="sxs-lookup"><span data-stu-id="69c69-120">In order to be used by the `fixed` statement the following conditions must be met:</span></span>

1. <span data-ttu-id="69c69-121">Es ist nur ein solcher Member für einen Typ angegeben.</span><span class="sxs-lookup"><span data-stu-id="69c69-121">There is only one such member provided for a type.</span></span>
1. <span data-ttu-id="69c69-122">Wird von `ref` oder `ref readonly`zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="69c69-122">Returns by `ref` or `ref readonly`.</span></span> <span data-ttu-id="69c69-123">(`readonly` ist zulässig, damit Autoren von unveränderlichen/schreibgeschützten Typen das Muster implementieren können, ohne eine beschreibbare API hinzuzufügen, die in sicherem Code verwendet werden kann.)</span><span class="sxs-lookup"><span data-stu-id="69c69-123">(`readonly` is permitted so that authors of immutable/readonly types could implement the pattern without adding writeable API that could be used in safe code)</span></span>
1. <span data-ttu-id="69c69-124">T ist ein nicht verwalteter Typ.</span><span class="sxs-lookup"><span data-stu-id="69c69-124">T is an unmanaged type.</span></span>
<span data-ttu-id="69c69-125">(da `T*` der Zeigertyp wird.</span><span class="sxs-lookup"><span data-stu-id="69c69-125">(since `T*` becomes the pointer type.</span></span> <span data-ttu-id="69c69-126">Die Einschränkung wird auf natürliche Weise erweitert, wenn das Konzept "nicht verwaltet" erweitert wird.</span><span class="sxs-lookup"><span data-stu-id="69c69-126">The restriction will naturally expand if/when the notion of "unmanaged" is expanded)</span></span>
1. <span data-ttu-id="69c69-127">Gibt verwaltete `nullptr` zurück, wenn keine Daten zum Anheften vorhanden sind – wahrscheinlich die günstigste Methode zum vermitteln von leergaben.</span><span class="sxs-lookup"><span data-stu-id="69c69-127">Returns managed `nullptr` when there is no data to pin – probably the cheapest way to convey emptiness.</span></span>
<span data-ttu-id="69c69-128">(Beachten Sie, dass die Zeichenfolge "" einen Verweis auf "\ 0" zurückgibt, da Zeichen folgen auf Null enden).</span><span class="sxs-lookup"><span data-stu-id="69c69-128">(note that “” string returns a ref to '\0' since strings are null-terminated)</span></span>

<span data-ttu-id="69c69-129">Alternativ `#3` dazu können wir das Ergebnis in leeren Fällen als nicht definiert oder Implementierungs spezifisch zulassen.</span><span class="sxs-lookup"><span data-stu-id="69c69-129">Alternatively for the `#3` we can allow the result in empty cases be undefined or implementation-specific.</span></span> <span data-ttu-id="69c69-130">Dies kann jedoch die API gefährlicher machen und anfällig für Missbrauch und unbeabsichtigte Kompatibilitäts Belastungen werden.</span><span class="sxs-lookup"><span data-stu-id="69c69-130">That, however, may make the API more dangerous and prone to abuse and unintended compatibility burdens.</span></span> 

## <a name="translation"></a><span data-ttu-id="69c69-131">*Übersetzung*</span><span class="sxs-lookup"><span data-stu-id="69c69-131">*Translation*</span></span> ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

<span data-ttu-id="69c69-132">wird zum folgenden Pseudo Code (nicht alles Ausdrucks in C#)</span><span class="sxs-lookup"><span data-stu-id="69c69-132">becomes the following pseudocode (not all expressible in C#)</span></span>

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a><span data-ttu-id="69c69-133">Nachteile</span><span class="sxs-lookup"><span data-stu-id="69c69-133">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="69c69-134">Getpinnablereferenzierungsweise ist nur in `fixed`gedacht, aber nichts hindert die Verwendung in sicherem Code, weshalb dies von implemenor berücksichtigt werden muss.</span><span class="sxs-lookup"><span data-stu-id="69c69-134">GetPinnableReference is intended to be used only in `fixed`, but nothing prevents its use in safe code, so implementor must keep that in mind.</span></span>

## <a name="alternatives"></a><span data-ttu-id="69c69-135">Alternativen</span><span class="sxs-lookup"><span data-stu-id="69c69-135">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="69c69-136">Benutzer können getpinablereferenzierungsart oder ähnliches Mitglied einführen und als</span><span class="sxs-lookup"><span data-stu-id="69c69-136">Users can introduce GetPinnableReference or similar member and use it as</span></span>
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

<span data-ttu-id="69c69-137">Es gibt keine Lösung für `System.String`, wenn eine alternative Lösung gewünscht ist.</span><span class="sxs-lookup"><span data-stu-id="69c69-137">There is no solution for `System.String` if alternative solution is desired.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="69c69-138">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="69c69-138">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="69c69-139">[] Verhalten im "leeren" Zustand.</span><span class="sxs-lookup"><span data-stu-id="69c69-139">[ ] Behavior in "empty" state.</span></span><span data-ttu-id="69c69-140"> - `nullptr` oder `undefined`?</span><span class="sxs-lookup"><span data-stu-id="69c69-140"> - `nullptr` or `undefined` ?</span></span> 
- <span data-ttu-id="69c69-141">[] Sollten die Erweiterungs Methoden berücksichtigt werden?</span><span class="sxs-lookup"><span data-stu-id="69c69-141">[ ] Should the extension methods be considered ?</span></span> 
- <span data-ttu-id="69c69-142">[] Wenn ein Muster auf `System.String`erkannt wird, sollte es gewinnen?</span><span class="sxs-lookup"><span data-stu-id="69c69-142">[ ] If a pattern is detected on `System.String`, should it win over ?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="69c69-143">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="69c69-143">Design meetings</span></span>

<span data-ttu-id="69c69-144">Noch keine.</span><span class="sxs-lookup"><span data-stu-id="69c69-144">None yet.</span></span> 
