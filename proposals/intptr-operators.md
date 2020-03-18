---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483547"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a><span data-ttu-id="3acbd-101">Operatoren sollten für `System.IntPtr` und `System.UIntPtr` verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="3acbd-101">Operators should be exposed for `System.IntPtr` and `System.UIntPtr`</span></span>

* <span data-ttu-id="3acbd-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="3acbd-102">[x] Proposed</span></span>
* <span data-ttu-id="3acbd-103">[] Prototyp: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="3acbd-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="3acbd-104">[] Implementierung: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="3acbd-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="3acbd-105">[] Spezifikation: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="3acbd-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="3acbd-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="3acbd-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3acbd-107">Die CLR unterstützt eine Reihe von Operatoren für die `System.IntPtr`-und `System.UIntPtr` Typen (`native int`).</span><span class="sxs-lookup"><span data-stu-id="3acbd-107">The CLR supports a set of operators for the `System.IntPtr` and `System.UIntPtr` types (`native int`).</span></span> <span data-ttu-id="3acbd-108">Diese Operatoren sind in `III.1.5` der Common Language Infrastructure Spezifikation (`ECMA-335`) zu sehen.</span><span class="sxs-lookup"><span data-stu-id="3acbd-108">These operators can be seen in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="3acbd-109">Diese Operatoren werden von C#jedoch nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="3acbd-109">However, these operators are not supported by C#.</span></span>

<span data-ttu-id="3acbd-110">Die Sprachunterstützung sollte für den gesamten Satz von Operatoren bereitgestellt werden, die von `System.IntPtr` und `System.UIntPtr`unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="3acbd-110">Language support should be provided for the full set of operators supported by `System.IntPtr` and `System.UIntPtr`.</span></span> <span data-ttu-id="3acbd-111">Diese Operatoren lauten wie folgt: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span><span class="sxs-lookup"><span data-stu-id="3acbd-111">These operators are: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span></span>

## <a name="motivation"></a><span data-ttu-id="3acbd-112">Motivation</span><span class="sxs-lookup"><span data-stu-id="3acbd-112">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="3acbd-113">Heute können Benutzer problemlos Anwendungen für C# mehrere Plattformen schreiben, indem Sie verschiedene Tools und Frameworks verwenden, wie z. b.: `Xamarin`, `.NET Core`, `Mono`usw...</span><span class="sxs-lookup"><span data-stu-id="3acbd-113">Today, users can easily write C# applications targeting multiple platforms using various tools and frameworks, such as: `Xamarin`, `.NET Core`, `Mono`, etc...</span></span>

<span data-ttu-id="3acbd-114">Beim Schreiben von Platt Form übergreifendem Code ist es häufig erforderlich, Interop-Code zu schreiben, der mit einer bestimmten Zielplattform auf eine bestimmte Weise interagiert.</span><span class="sxs-lookup"><span data-stu-id="3acbd-114">When writing cross-platform code, it is often necessary to write interop code that interacts with a particular target platform in a specific manner.</span></span> <span data-ttu-id="3acbd-115">Dies kann das Schreiben von Grafik Code, das Aufrufen einer System-API oder das interagieren mit einer vorhandenen nativen Bibliothek einschließen.</span><span class="sxs-lookup"><span data-stu-id="3acbd-115">This could include writing graphics code, calling some System API, or interacting with an existing native library.</span></span>

<span data-ttu-id="3acbd-116">Dieser Interop-Code muss häufig Handles, nicht verwalteten Speicher oder sogar plattformspezifische Ganzzahlen verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="3acbd-116">This interop code often has to deal with handles, unmanaged memory, or even just platform-specific sized integers.</span></span>

<span data-ttu-id="3acbd-117">Die Common Language Runtime unterstützt dies durch Definieren einer Reihe von Operatoren, die für die primitiven Typen `native int` (`System.IntPtr`) und `native unsigned int` (`System.UIntPtr`) verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="3acbd-117">The runtime provides support for this by defining a set of operators that can be used on the `native int` (`System.IntPtr`) and `native unsigned int` (`System.UIntPtr`) primitive types.</span></span>

<span data-ttu-id="3acbd-118">C#hat diese Operatoren nie unterstützt, sodass Benutzer das Problem umgehen müssen.</span><span class="sxs-lookup"><span data-stu-id="3acbd-118">C# has never supported these operators and so users have to work around the issue.</span></span> <span data-ttu-id="3acbd-119">Dadurch wird die Codekomplexität häufig erhöht und die Code Wartbarkeit verringert.</span><span class="sxs-lookup"><span data-stu-id="3acbd-119">This often increases code complexity and lowers code maintainability.</span></span>

<span data-ttu-id="3acbd-120">Daher sollte die Sprache damit beginnen, diese Operatoren zu unterstützen, um die Sprache zu unterstützen, um diese Anforderungen besser zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="3acbd-120">As such, the language should begin to support these operators to help advance the language to better support these requirements.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="3acbd-121">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="3acbd-121">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="3acbd-122">Der vollständige Satz der unterstützten Operatoren wird in `III.1.5` der Common Language Infrastructure Spezifikation (`ECMA-335`) definiert.</span><span class="sxs-lookup"><span data-stu-id="3acbd-122">The full set of operators supported are defined in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="3acbd-123">Die Spezifikation ist hier verfügbar: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span><span class="sxs-lookup"><span data-stu-id="3acbd-123">The specification is available here: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span></span>

* <span data-ttu-id="3acbd-124">Im folgenden finden Sie eine Zusammenfassung der Operatoren zur einfacheren Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="3acbd-124">A summary of the operators is provided below for convenience.</span></span>
* <span data-ttu-id="3acbd-125">Die nicht überprüfbaren Operatoren, die von der CLI-Spezifikation definiert werden, werden nicht aufgelistet und sind zurzeit nicht Teil dieses Vorschlags (auch wenn dies auch in Erwägung gezogen werden sollte).</span><span class="sxs-lookup"><span data-stu-id="3acbd-125">The unverifiable operators defined by the CLI spec are not listed and are not currently part of this proposal (although it may be worth considering these as well).</span></span>
* <span data-ttu-id="3acbd-126">Das Bereitstellen eines Schlüssel Worts (z. b. `nint` und `nuint`) und das Bereitstellen einer Möglichkeit, Literale für `System.IntPtr` und `System.UIntPtr` (z. b. 0n) zu deklarieren, ist nicht Teil dieses Angebots (auch wenn dies auch in Erwägung gezogen werden sollte).</span><span class="sxs-lookup"><span data-stu-id="3acbd-126">Providing a keyword (such as `nint` and `nuint`) nor providing a way to for literals to be declared for `System.IntPtr` and `System.UIntPtr` (such as 0n) is not part of this proposal (although it may be worth considering these as well).</span></span>

### <a name="unary-plus-operator"></a><span data-ttu-id="3acbd-127">Unärer Plus-Operator</span><span class="sxs-lookup"><span data-stu-id="3acbd-127">Unary Plus Operator</span></span>

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a><span data-ttu-id="3acbd-128">Unärer Minus-Operator</span><span class="sxs-lookup"><span data-stu-id="3acbd-128">Unary Minus Operator</span></span>

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a><span data-ttu-id="3acbd-129">Bitweiser Komplement Operator</span><span class="sxs-lookup"><span data-stu-id="3acbd-129">Bitwise Complement Operator</span></span>

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a><span data-ttu-id="3acbd-130">Umwandlungsoperatoren</span><span class="sxs-lookup"><span data-stu-id="3acbd-130">Cast Operators</span></span>

```csharp
explicit operator sbyte(System.IntPtr)               // Truncate
explicit operator short(System.IntPtr)               // Truncate
explicit operator int(System.IntPtr)                 // Truncate
explicit operator long(System.IntPtr)                // Sign Extend

explicit operator byte(System.IntPtr)                // Truncate
explicit operator ushort(System.IntPtr)              // Truncate
explicit operator uint(System.IntPtr)                // Truncate
explicit operator ulong(System.IntPtr)               // Zero Extend

explicit operator System.IntPtr(int)                 // Sign Extend
explicit operator System.IntPtr(long)                // Truncate

explicit operator System.IntPtr(uint)                // Sign Extend
explicit operator System.IntPtr(ulong)               // Truncate

explicit operator System.IntPtr(System.IntPtr)
explicit operator System.IntPtr(System.UIntPtr)
```

```csharp
explicit operator sbyte(System.UIntPtr)               // Truncate
explicit operator short(System.UIntPtr)               // Truncate
explicit operator int(System.UIntPtr)                 // Truncate
explicit operator long(System.UIntPtr)                // Sign Extend

explicit operator byte(System.UIntPtr)                // Truncate
explicit operator ushort(System.UIntPtr)              // Truncate
explicit operator uint(System.UIntPtr)                // Truncate
explicit operator ulong(System.UIntPtr)               // Zero Extend

explicit operator System.UIntPtr(int)                 // Zero Extend
explicit operator System.UIntPtr(long)                // Truncate

explicit operator System.UIntPtr(uint)                // Zero Extend
explicit operator System.UIntPtr(ulong)               // Truncate

explicit operator System.UIntPtr(System.IntPtr)
explicit operator System.UIntPtr(System.UIntPtr)
```

### <a name="multiplication-operator"></a><span data-ttu-id="3acbd-131">Multiplikations Operator</span><span class="sxs-lookup"><span data-stu-id="3acbd-131">Multiplication Operator</span></span>

```csharp
System.IntPtr operator *(int, System.IntPtr)
System.IntPtr operator *(System.IntPtr, int)
System.IntPtr operator *(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator *(uint, System.UIntPtr)
System.UIntPtr operator *(System.UIntPtr, uint)
System.UIntPtr operator *(System.UIntPtr, System.UIntPtr)
```

### <a name="division-operator"></a><span data-ttu-id="3acbd-132">Divisions Operator</span><span class="sxs-lookup"><span data-stu-id="3acbd-132">Division Operator</span></span>

```csharp
System.IntPtr operator /(int, System.IntPtr)
System.IntPtr operator /(System.IntPtr, int)
System.IntPtr operator /(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator /(uint, System.UIntPtr)
System.UIntPtr operator /(System.UIntPtr, uint)
System.UIntPtr operator /(System.UIntPtr, System.UIntPtr)
```

### <a name="remainder-operator"></a><span data-ttu-id="3acbd-133">Rest-Operator</span><span class="sxs-lookup"><span data-stu-id="3acbd-133">Remainder Operator</span></span>

```csharp
System.IntPtr operator %(int, System.IntPtr)
System.IntPtr operator %(System.IntPtr, int)
System.IntPtr operator %(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator %(uint, System.UIntPtr)
System.UIntPtr operator %(System.UIntPtr, uint)
System.UIntPtr operator %(System.UIntPtr, System.UIntPtr)
```

### <a name="addition-operator"></a><span data-ttu-id="3acbd-134">Additions Operator</span><span class="sxs-lookup"><span data-stu-id="3acbd-134">Addition Operator</span></span>

```csharp
System.IntPtr operator +(int, System.IntPtr)
System.IntPtr operator +(System.IntPtr, int)
System.IntPtr operator +(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator +(uint, System.UIntPtr)
System.UIntPtr operator +(System.UIntPtr, uint)
System.UIntPtr operator +(System.UIntPtr, System.UIntPtr)
```

### <a name="subtraction-operator"></a><span data-ttu-id="3acbd-135">Subtraktions Operator</span><span class="sxs-lookup"><span data-stu-id="3acbd-135">Subtraction Operator</span></span>

```csharp
System.IntPtr operator -(int, System.IntPtr)
System.IntPtr operator -(System.IntPtr, int)
System.IntPtr operator -(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator -(uint, System.UIntPtr)
System.UIntPtr operator -(System.UIntPtr, uint)
System.UIntPtr operator -(System.UIntPtr, System.UIntPtr)
```

### <a name="shift-operators"></a><span data-ttu-id="3acbd-136">Schiebeoperatoren</span><span class="sxs-lookup"><span data-stu-id="3acbd-136">Shift Operators</span></span>

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a><span data-ttu-id="3acbd-137">Integer-Vergleichs Operatoren</span><span class="sxs-lookup"><span data-stu-id="3acbd-137">Integer Comparison Operators</span></span>

```csharp
bool operator ==(int, System.IntPtr)
bool operator ==(System.IntPtr, int)
bool operator ==(System.IntPtr, System.IntPtr)

bool operator !=(int, System.IntPtr)
bool operator !=(System.IntPtr, int)
bool operator !=(System.IntPtr, System.IntPtr)

bool operator  <(int, System.IntPtr)
bool operator  <(System.IntPtr, int)
bool operator  <(System.IntPtr, System.IntPtr)

bool operator  >(int, System.IntPtr)
bool operator  >(System.IntPtr, int)
bool operator  >(System.IntPtr, System.IntPtr)

bool operator <=(int, System.IntPtr)
bool operator <=(System.IntPtr, int)
bool operator <=(System.IntPtr, System.IntPtr)

bool operator >=(int, System.IntPtr)
bool operator >=(System.IntPtr, int)
bool operator >=(System.IntPtr, System.IntPtr)
```

```csharp
bool operator ==(uint, System.UIntPtr)
bool operator ==(System.UIntPtr, uint)
bool operator ==(System.UIntPtr, System.UIntPtr)

bool operator !=(uint, System.UIntPtr)
bool operator !=(System.UIntPtr, uint)
bool operator !=(System.UIntPtr, System.UIntPtr)

bool operator  <(uint, System.UIntPtr)
bool operator  <(System.UIntPtr, uint)
bool operator  <(System.UIntPtr, System.UIntPtr)

bool operator  >(uint, System.UIntPtr)
bool operator  >(System.UIntPtr, uint)
bool operator  >(System.UIntPtr, System.UIntPtr)

bool operator <=(uint, System.UIntPtr)
bool operator <=(System.UIntPtr, uint)
bool operator <=(System.UIntPtr, System.UIntPtr)

bool operator >=(uint, System.UIntPtr)
bool operator >=(System.UIntPtr, uint)
bool operator >=(System.UIntPtr, System.UIntPtr)
```

### <a name="integer-logical-operators"></a><span data-ttu-id="3acbd-138">Ganz Zahl logische Operatoren</span><span class="sxs-lookup"><span data-stu-id="3acbd-138">Integer Logical Operators</span></span>

```csharp
System.IntPtr operator &(int, System.IntPtr)
System.IntPtr operator &(System.IntPtr, int)
System.IntPtr operator &(System.IntPtr, System.IntPtr)

System.IntPtr operator |(int, System.IntPtr)
System.IntPtr operator |(System.IntPtr, int)
System.IntPtr operator |(System.IntPtr, System.IntPtr)

System.IntPtr operator ^(int, System.IntPtr)
System.IntPtr operator ^(System.IntPtr, int)
System.IntPtr operator ^(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator &(uint, System.UIntPtr)
System.UIntPtr operator &(System.UIntPtr, uint)
System.UIntPtr operator &(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator |(uint, System.UIntPtr)
System.UIntPtr operator |(System.UIntPtr, uint)
System.UIntPtr operator |(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator ^(uint, System.UIntPtr)
System.UIntPtr operator ^(System.UIntPtr, uint)
System.UIntPtr operator ^(System.UIntPtr, System.UIntPtr)
```

## <a name="drawbacks"></a><span data-ttu-id="3acbd-139">Nachteile</span><span class="sxs-lookup"><span data-stu-id="3acbd-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="3acbd-140">Die tatsächliche Verwendung dieser Operatoren ist möglicherweise klein und auf Endbenutzer beschränkt, die Bibliotheken auf niedrigerer Ebene oder Interop-Code schreiben.</span><span class="sxs-lookup"><span data-stu-id="3acbd-140">The actual use of these operators may be small and limited to end-users who are writing lower level libraries or interop code.</span></span> <span data-ttu-id="3acbd-141">Die meisten Endbenutzer nutzen wahrscheinlich diese Bibliotheken der untersten Ebene selbst, die die nativen Ganzzahlen, Handles und Interop-Code abstrahieren würden.</span><span class="sxs-lookup"><span data-stu-id="3acbd-141">Most end-users would likely be consuming these lower level libraries themselves which would have the native sized integers, handles, and interop code abstracted away.</span></span> <span data-ttu-id="3acbd-142">Daher müssten Sie die Operatoren selbst nicht benötigen.</span><span class="sxs-lookup"><span data-stu-id="3acbd-142">As such, they would not have need of the operators themselves.</span></span>

## <a name="alternatives"></a><span data-ttu-id="3acbd-143">Alternativen</span><span class="sxs-lookup"><span data-stu-id="3acbd-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="3acbd-144">Lassen Sie das Framework die erforderlichen Operatoren implementieren, indem Sie Sie direkt in Il schreiben.</span><span class="sxs-lookup"><span data-stu-id="3acbd-144">Have the framework implement the required operators by writing them directly in IL.</span></span> <span data-ttu-id="3acbd-145">Darüber hinaus kann die Laufzeit eine intrinsische Unterstützung für die vom Framework definierten Operatoren bereitstellen, um die Leistung zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="3acbd-145">Additionally, the runtime could provide intrinsic support for the operators defined by the framework, so as to better optimize the end performance.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="3acbd-146">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="3acbd-146">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="3acbd-147">Welche Teile des Entwurfs sind noch TBD?</span><span class="sxs-lookup"><span data-stu-id="3acbd-147">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="3acbd-148">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="3acbd-148">Design meetings</span></span>

<span data-ttu-id="3acbd-149">Verknüpfen Sie die Entwurfs Hinweise, die sich auf diesen Vorschlag auswirken, und beschreiben Sie in einem Satz für jede Änderungen, zu denen Sie geführt haben.</span><span class="sxs-lookup"><span data-stu-id="3acbd-149">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>