---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483601"
---
# <a name="unmanaged-type-constraint"></a><span data-ttu-id="4de67-101">Nicht verwaltete Typeinschränkung</span><span class="sxs-lookup"><span data-stu-id="4de67-101">Unmanaged type constraint</span></span>

## <a name="summary"></a><span data-ttu-id="4de67-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="4de67-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="4de67-103">Die nicht verwaltete Einschränkungs Funktion bietet eine sprach Erzwingung für die Klasse von Typen, die in der C# Sprachspezifikation als "nicht verwaltete Typen" bezeichnet werden.  Dies wird im Abschnitt 18,2 als Typ definiert, der kein Referenztyp ist und keine Verweistyp Felder auf einer beliebigen Schachtelungs Ebene enthält.</span><span class="sxs-lookup"><span data-stu-id="4de67-103">The unmanaged constraint feature will give language enforcement to the class of types known as "unmanaged types" in the C# language spec.  This is defined in section 18.2 as a type which is not a reference type and doesn't contain reference type fields at any level of nesting.</span></span>  

## <a name="motivation"></a><span data-ttu-id="4de67-104">Motivation</span><span class="sxs-lookup"><span data-stu-id="4de67-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="4de67-105">Die Hauptmotivation besteht darin, das Erstellen von Interop-Code auf niedriger Ebene C#in zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="4de67-105">The primary motivation is to make it easier to author low level interop code in C#.</span></span> <span data-ttu-id="4de67-106">Nicht verwaltete Typen sind einer der Kern Bausteine für Interop-Code, aber aufgrund der fehlenden Unterstützung in Generika ist es unmöglich, wiederverwendbare Routinen für alle nicht verwalteten Typen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4de67-106">Unmanaged types are one of the core building blocks for interop code, yet the lack of support in generics makes it impossible to create re-usable routines across all unmanaged types.</span></span> <span data-ttu-id="4de67-107">Stattdessen sind Entwickler gezwungen, den gleichen Code für die kesselplatte für jeden nicht verwalteten Typ in der Bibliothek zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="4de67-107">Instead developers are forced to author the same boiler plate code for every unmanaged type in their library:</span></span>

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

<span data-ttu-id="4de67-108">Um diese Art von Szenario zu ermöglichen, wird in der Sprache eine neue Einschränkung eingeführt: nicht verwaltet:</span><span class="sxs-lookup"><span data-stu-id="4de67-108">To enable this type of scenario the language will be introducing a new constraint: unmanaged:</span></span>

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

<span data-ttu-id="4de67-109">Diese Einschränkung kann nur von Typen erfüllt werden, die in die nicht verwaltete Typdefinition in der C# Sprachspezifikation passen. Eine andere Möglichkeit der Betrachtung besteht darin, dass ein Typ das nicht verwaltete Einschränkungs-IFF erfüllt, das auch als Zeiger verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="4de67-109">This constraint can only be met by types which fit into the unmanaged type definition in the C# language spec. Another way of looking at it is that a type satisfies the unmanaged constraint iff it can also be used as a pointer.</span></span> 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

<span data-ttu-id="4de67-110">Typparameter mit der nicht verwalteten Einschränkung können alle Features verwenden, die für nicht verwaltete Typen verfügbar sind: Zeiger, korrigiert usw...</span><span class="sxs-lookup"><span data-stu-id="4de67-110">Type parameters with the unmanaged constraint can use all the features available to unmanaged types: pointers, fixed, etc ...</span></span> 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

<span data-ttu-id="4de67-111">Diese Einschränkung ermöglicht außerdem die effiziente Konvertierung zwischen strukturierten Daten und Streams von Bytes.</span><span class="sxs-lookup"><span data-stu-id="4de67-111">This constraint will also make it possible to have efficient conversions between structured data and streams of bytes.</span></span> <span data-ttu-id="4de67-112">Dies ist ein Vorgang, der in Netzwerk Stapeln und serialisierungsschichten üblich ist:</span><span class="sxs-lookup"><span data-stu-id="4de67-112">This is an operation that is common in networking stacks and serialization layers:</span></span>

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

<span data-ttu-id="4de67-113">Solche Routinen sind von Vorteil, da Sie zur Kompilierzeit und Zuordnungs Kosten sicher sind.</span><span class="sxs-lookup"><span data-stu-id="4de67-113">Such routines are advantageous because they are provably safe at compile time and allocation free.</span></span>  <span data-ttu-id="4de67-114">Interop-Autoren können dies nicht tun (auch wenn es sich um eine Ebene handelt, bei der Leistungs wichtig ist).</span><span class="sxs-lookup"><span data-stu-id="4de67-114">Interop authors today can not do this (even though it's at a layer where perf is critical).</span></span>  <span data-ttu-id="4de67-115">Stattdessen müssen Sie Routinen verwenden, die über teure Laufzeitüberprüfungen verfügen, um sicherzustellen, dass die Werte ordnungsgemäß nicht verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="4de67-115">Instead they need to rely on allocating routines that have expensive runtime checks to verify values are correctly unmanaged.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="4de67-116">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="4de67-116">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="4de67-117">In der Sprache wird eine neue Einschränkung mit dem Namen `unmanaged`eingeführt.</span><span class="sxs-lookup"><span data-stu-id="4de67-117">The language will introduce a new constraint named `unmanaged`.</span></span> <span data-ttu-id="4de67-118">Um diese Einschränkung zu erfüllen, muss ein Typ eine Struktur sein, und alle Felder des Typs müssen in eine der folgenden Kategorien fallen:</span><span class="sxs-lookup"><span data-stu-id="4de67-118">In order to satisfy this constraint a type must be a struct and all the fields of the type must fall into one of the following categories:</span></span>

- <span data-ttu-id="4de67-119">Sie haben den Typ `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` oder `UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="4de67-119">Have the type `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` or `UIntPtr`.</span></span>
- <span data-ttu-id="4de67-120">Jeder `enum` Typ.</span><span class="sxs-lookup"><span data-stu-id="4de67-120">Be any `enum` type.</span></span>
- <span data-ttu-id="4de67-121">Ist ein Zeigertyp.</span><span class="sxs-lookup"><span data-stu-id="4de67-121">Be a pointer type.</span></span>
- <span data-ttu-id="4de67-122">Dabei handelt es sich um eine benutzerdefinierte Struktur, die die `unmanaged` Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="4de67-122">Be a user defined struct that satsifies the `unmanaged` constraint.</span></span>

<span data-ttu-id="4de67-123">Vom Compiler generierte Instanzfelder, wie z. b. solche, die automatisch implementierte Eigenschaften unterstützen, müssen diese Einschränkungen ebenfalls erfüllen.</span><span class="sxs-lookup"><span data-stu-id="4de67-123">Compiler generated instance fields, such as those backing auto-implemented properties, must also meet these constraints.</span></span> 

<span data-ttu-id="4de67-124">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4de67-124">For example:</span></span>

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

<span data-ttu-id="4de67-125">Die `unmanaged`-Einschränkung kann nicht mit `struct`, `class` oder `new()`kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="4de67-125">The `unmanaged` constraint cannot be combined with `struct`, `class` or `new()`.</span></span> <span data-ttu-id="4de67-126">Diese Einschränkung leitet sich von der Tatsache ab, dass `unmanaged` `struct` folglich sind die anderen Einschränkungen nicht sinnvoll.</span><span class="sxs-lookup"><span data-stu-id="4de67-126">This restriction derives from the fact that `unmanaged` implies `struct` hence the other constraints do not make sense.</span></span>

<span data-ttu-id="4de67-127">Die `unmanaged` Einschränkung wird nicht von CLR erzwungen, sondern nur von der Sprache.</span><span class="sxs-lookup"><span data-stu-id="4de67-127">The `unmanaged` constraint is not enforced by CLR, only by the language.</span></span> <span data-ttu-id="4de67-128">Um die fehl Verwendung durch andere Sprachen zu verhindern, werden Methoden mit dieser Einschränkung durch eine mod-req geschützt. Dadurch wird verhindert, dass andere Sprachen Typargumente verwenden, bei denen es sich nicht um verwaltete Typen handelt.</span><span class="sxs-lookup"><span data-stu-id="4de67-128">To prevent mis-use by other languages, methods which have this constraint will be protected by a mod-req. This will prevent other languages from using type arguments which are not unmanaged types.</span></span>

<span data-ttu-id="4de67-129">Das in der Einschränkung `unmanaged` Token ist weder ein Schlüsselwort noch ein Kontext Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="4de67-129">The token `unmanaged` in the constraint is not a keyword, nor a contextual keyword.</span></span> <span data-ttu-id="4de67-130">Stattdessen ist es wie `var`, dass es an diesem Speicherort ausgewertet wird und eine der folgenden Aktionen durchführt:</span><span class="sxs-lookup"><span data-stu-id="4de67-130">Instead it is like `var` in that it is evaluated at that location and will either:</span></span>

- <span data-ttu-id="4de67-131">An benutzerdefinierten oder referenzierten Typ mit dem Namen "`unmanaged`" binden: Dies wird genauso behandelt, wie jede andere benannte Typeinschränkung behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="4de67-131">Bind to user defined or referenced type named `unmanaged`: This will be treated just as any other named type constraint is treated.</span></span> 
- <span data-ttu-id="4de67-132">Binden an keinen Typ: Dies wird als `unmanaged` Einschränkung interpretiert.</span><span class="sxs-lookup"><span data-stu-id="4de67-132">Bind to no type: This will be interpreted as the `unmanaged` constraint.</span></span>

<span data-ttu-id="4de67-133">Wenn ein Typ mit dem Namen `unmanaged` vorhanden ist, der ohne Qualifizierung im aktuellen Kontext verfügbar ist, gibt es keine Möglichkeit, die `unmanaged`-Einschränkung zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="4de67-133">In the case there is a type named `unmanaged` and it is available without qualification in the current context, then there will be no way to use the `unmanaged` constraint.</span></span> <span data-ttu-id="4de67-134">Dies entspricht den Regeln, die die Funktions `var` und benutzerdefinierten Typen desselben Namens betreffen.</span><span class="sxs-lookup"><span data-stu-id="4de67-134">This parallels the rules surrounding the feature `var` and user defined types of the same name.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="4de67-135">Nachteile</span><span class="sxs-lookup"><span data-stu-id="4de67-135">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="4de67-136">Der Hauptnachteil dieses Features besteht darin, dass es eine kleine Anzahl von Entwicklern bietet: in der Regel auf der Basis von Bibliotheks Autoren oder-Frameworks.</span><span class="sxs-lookup"><span data-stu-id="4de67-136">The primary drawback of this feature is that it serves a small number of developers: typically low level library authors or frameworks.</span></span>  <span data-ttu-id="4de67-137">Daher wird für eine kleine Anzahl von Entwicklern eine kostbare sprach Zeit aufgewendet.</span><span class="sxs-lookup"><span data-stu-id="4de67-137">Hence it's spending precious language time for a small number of developers.</span></span> 

<span data-ttu-id="4de67-138">Diese Frameworks sind aber häufig die Grundlage für die Mehrzahl der .NET-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="4de67-138">Yet these frameworks are often the basis for the majority of .NET applications out there.</span></span>  <span data-ttu-id="4de67-139">Daher kann die Leistung/Richtigkeit auf dieser Ebene einen Ripple-Effekt auf das .NET-Ökosystem haben.</span><span class="sxs-lookup"><span data-stu-id="4de67-139">Hence performance / correctness wins at this level can have a ripple effect on the .NET ecosystem.</span></span>  <span data-ttu-id="4de67-140">Dadurch wird das Feature auch mit eingeschränkter Zielgruppe in Erwägung gezogen.</span><span class="sxs-lookup"><span data-stu-id="4de67-140">This makes the feature worth considering even with the limited audience.</span></span>

## <a name="alternatives"></a><span data-ttu-id="4de67-141">Alternativen</span><span class="sxs-lookup"><span data-stu-id="4de67-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="4de67-142">Es gibt einige Alternativen zu berücksichtigende Aspekte:</span><span class="sxs-lookup"><span data-stu-id="4de67-142">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="4de67-143">Der Status quo: das Feature ist nicht für seine eigenen Vorzüge gerechtfertigt, und Entwickler verwenden weiterhin das implizite Opt-in-Verhalten.</span><span class="sxs-lookup"><span data-stu-id="4de67-143">The status quo:  The feature is not justified on its own merits and developers continue to use the implicit opt in behavior.</span></span>

## <a name="questions"></a><span data-ttu-id="4de67-144">Fragen</span><span class="sxs-lookup"><span data-stu-id="4de67-144">Questions</span></span>
[quesions]: #questions

### <a name="metadata-representation"></a><span data-ttu-id="4de67-145">Metadatendarstellung</span><span class="sxs-lookup"><span data-stu-id="4de67-145">Metadata Representation</span></span>

<span data-ttu-id="4de67-146">Die F# -Sprache codiert die Einschränkung in der Signatur Datei. C# Dies bedeutet, dass ihre Darstellung nicht wieder verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="4de67-146">The F# language encodes the constraint in the signature file which means C# cannot re-use their representation.</span></span> <span data-ttu-id="4de67-147">Für diese Einschränkung muss ein neues Attribut ausgewählt werden.</span><span class="sxs-lookup"><span data-stu-id="4de67-147">A new attribute will need to be chosen for this constraint.</span></span> <span data-ttu-id="4de67-148">Darüber hinaus muss eine Methode, die diese Einschränkung hat, durch einen mod-req geschützt werden.</span><span class="sxs-lookup"><span data-stu-id="4de67-148">Additionally a method which has this constraint must be protected by a mod-req.</span></span>

### <a name="blittable-vs-unmanaged"></a><span data-ttu-id="4de67-149">Blitfähige und nicht verwaltete</span><span class="sxs-lookup"><span data-stu-id="4de67-149">Blittable vs. Unmanaged</span></span>
<span data-ttu-id="4de67-150">Die F# Sprache verfügt über ein sehr [ähnliches Feature](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) , das das Schlüsselwort "nicht verwaltet" verwendet.</span><span class="sxs-lookup"><span data-stu-id="4de67-150">The F# language has a very [similar feature](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) which uses the keyword unmanaged.</span></span> <span data-ttu-id="4de67-151">Der blitfähige Name stammt aus der Verwendung in Midori.</span><span class="sxs-lookup"><span data-stu-id="4de67-151">The blittable name comes from the use in Midori.</span></span>  <span data-ttu-id="4de67-152">Sie sollten sich hier als Rangfolge ansehen und nicht verwaltete verwenden.</span><span class="sxs-lookup"><span data-stu-id="4de67-152">May want to look to precedence here and use unmanaged instead.</span></span> 

<span data-ttu-id="4de67-153">**Lösung** Die Sprache entscheidet sich für die Verwendung nicht verwalteter</span><span class="sxs-lookup"><span data-stu-id="4de67-153">**Resolution** The language decide to use unmanaged</span></span> 

### <a name="verifier"></a><span data-ttu-id="4de67-154">Verifier</span><span class="sxs-lookup"><span data-stu-id="4de67-154">Verifier</span></span>

<span data-ttu-id="4de67-155">Muss der Verifier/die Laufzeit aktualisiert werden, um die Verwendung von Zeigern auf generische Typparameter zu verstehen?</span><span class="sxs-lookup"><span data-stu-id="4de67-155">Does the verifier / runtime need to be updated to understand the use of pointers to generic type parameters?</span></span>  <span data-ttu-id="4de67-156">Oder kann es einfach unverändert funktionieren, ohne dass Änderungen vorgenommen werden?</span><span class="sxs-lookup"><span data-stu-id="4de67-156">Or can it simply work as is without changes?</span></span>

<span data-ttu-id="4de67-157">**Lösung** Es sind keine Änderungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="4de67-157">**Resolution** No changes needed.</span></span> <span data-ttu-id="4de67-158">Alle Zeiger Typen sind einfach nicht verifizierbar.</span><span class="sxs-lookup"><span data-stu-id="4de67-158">All pointer types are simply unverifiable.</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="4de67-159">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="4de67-159">Design meetings</span></span>

<span data-ttu-id="4de67-160">–</span><span class="sxs-lookup"><span data-stu-id="4de67-160">n/a</span></span>
