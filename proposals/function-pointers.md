---
ms.openlocfilehash: 8bf3a18dc42e225e64bd3ccda2106aed89b421ed
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108388"
---
# <a name="function-pointers"></a><span data-ttu-id="e8a7b-101">Funktionszeiger</span><span class="sxs-lookup"><span data-stu-id="e8a7b-101">Function Pointers</span></span>

## <a name="summary"></a><span data-ttu-id="e8a7b-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="e8a7b-102">Summary</span></span>

<span data-ttu-id="e8a7b-103">Dieser Vorschlag stellt Sprachkonstrukte bereit, die IL-Opcodes verfügbar machen, auf die zurzeit nicht effizient C# zugegriffen werden kann, oder überhaupt in: `ldftn` und `calli`.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-103">This proposal provides language constructs that expose IL opcodes that cannot currently be accessed efficiently, or at all, in C# today: `ldftn` and `calli`.</span></span> <span data-ttu-id="e8a7b-104">Diese IL-Opcodes können bei Hochleistungs Code wichtig sein, und Entwickler benötigen eine effiziente Zugriffsmethode.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-104">These IL opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="e8a7b-105">Motivation</span><span class="sxs-lookup"><span data-stu-id="e8a7b-105">Motivation</span></span>

<span data-ttu-id="e8a7b-106">Die Gründe und der Hintergrund für dieses Feature werden im folgenden Problem beschrieben (wie eine mögliche Implementierung des Features):</span><span class="sxs-lookup"><span data-stu-id="e8a7b-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span>

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="e8a7b-107">Dies ist ein alternativer Entwurfsvorschlag für systeminterne [Compilerfunktionen](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md) .</span><span class="sxs-lookup"><span data-stu-id="e8a7b-107">This is an alternate design proposal to [compiler intrinsics](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="e8a7b-108">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="e8a7b-108">Detailed Design</span></span>

### <a name="function-pointers"></a><span data-ttu-id="e8a7b-109">Funktionszeiger</span><span class="sxs-lookup"><span data-stu-id="e8a7b-109">Function pointers</span></span>

<span data-ttu-id="e8a7b-110">Die Sprache ermöglicht die Deklaration von Funktions Zeigern mit der `delegate*`-Syntax.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-110">The language will allow for the declaration of function pointers using the `delegate*` syntax.</span></span> <span data-ttu-id="e8a7b-111">Die vollständige Syntax wird im nächsten Abschnitt ausführlich beschrieben, aber Sie soll der Syntax ähneln, die von `Func` und `Action` Typdeklarationen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-111">The full syntax is described in detail in the next section but it is meant to resemble the syntax used by `Func` and `Action` type declarations.</span></span>

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

<span data-ttu-id="e8a7b-112">Diese Typen werden mithilfe des Funktionszeiger Typs dargestellt, wie in ECMA-335 beschrieben.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-112">These types are represented using the function pointer type as outlined in ECMA-335.</span></span> <span data-ttu-id="e8a7b-113">Dies bedeutet, dass für den Aufruf einer `delegate*` `calli` verwendet wird, bei dem der Aufruf eines `delegate` `callvirt` für die `Invoke`-Methode verwendet.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-113">This means invocation of a `delegate*` will use `calli` where invocation of a `delegate` will use `callvirt` on the `Invoke` method.</span></span>
<span data-ttu-id="e8a7b-114">Obwohl der Aufruf für beide Konstrukte identisch ist, ist der Aufruf syntaktisch.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-114">Syntactically though invocation is identical for both constructs.</span></span>

<span data-ttu-id="e8a7b-115">Die ECMA-335-Definition von Methoden Zeigern enthält die Aufruf Konvention als Teil der Typsignatur (Abschnitt 7,1).</span><span class="sxs-lookup"><span data-stu-id="e8a7b-115">The ECMA-335 definition of method pointers includes the calling convention as part of the type signature (section 7.1).</span></span>
<span data-ttu-id="e8a7b-116">Die Standard Aufruf Konvention wird `managed`.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-116">The default calling convention will be `managed`.</span></span> <span data-ttu-id="e8a7b-117">Alternative Formen können angegeben werden, indem der entsprechende-Modifizierer nach der `delegate*`-Syntax hinzugefügt wird: `managed`, `cdecl`, `stdcall`, `thiscall`oder `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-117">Alternate forms can be specified by adding the appropriate modifier after the `delegate*` syntax: `managed`, `cdecl`, `stdcall`, `thiscall`, or `unmanaged`.</span></span> <span data-ttu-id="e8a7b-118">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-118">Example:</span></span>

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

<span data-ttu-id="e8a7b-119">Konvertierungen zwischen `delegate*` Typen werden basierend auf Ihrer Signatur einschließlich der Aufruf Konvention ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-119">Conversions between `delegate*` types is done based on their signature including the calling convention.</span></span>

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

<span data-ttu-id="e8a7b-120">Ein `delegate*` Typ ist ein Zeigertyp. Dies bedeutet, dass er über alle Funktionen und Einschränkungen eines Standard Zeiger Typs verfügt:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-120">A `delegate*` type is a pointer type which means it has all of the capabilities and restrictions of a standard pointer type:</span></span>

- <span data-ttu-id="e8a7b-121">Nur in einem `unsafe` Kontext gültig.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-121">Only valid in an `unsafe` context.</span></span>
- <span data-ttu-id="e8a7b-122">Methoden, die einen `delegate*` Parameter oder einen Rückgabetyp enthalten, können nur von einem `unsafe` Kontext aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-122">Methods which contain a `delegate*` parameter or return type can only be called from an `unsafe` context.</span></span>
- <span data-ttu-id="e8a7b-123">Kann nicht in `object`konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-123">Cannot be converted to `object`.</span></span>
- <span data-ttu-id="e8a7b-124">Kann nicht als generisches Argument verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-124">Cannot be used as a generic argument.</span></span>
- <span data-ttu-id="e8a7b-125">Kann `delegate*` implizit in `void*`konvertieren.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-125">Can implicitly convert `delegate*` to `void*`.</span></span>
- <span data-ttu-id="e8a7b-126">Kann explizit von `void*` in `delegate*`konvertieren.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-126">Can explicitly convert from `void*` to `delegate*`.</span></span>

<span data-ttu-id="e8a7b-127">Begrenzungen</span><span class="sxs-lookup"><span data-stu-id="e8a7b-127">Restrictions:</span></span>

- <span data-ttu-id="e8a7b-128">Benutzerdefinierte Attribute können nicht auf eine `delegate*` oder eines ihrer Elemente angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-128">Custom attributes cannot be applied to a `delegate*` or any of its elements.</span></span>
- <span data-ttu-id="e8a7b-129">Ein `delegate*` Parameter kann nicht als `params` gekennzeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-129">A `delegate*` parameter cannot be marked as `params`</span></span>
- <span data-ttu-id="e8a7b-130">Ein `delegate*` Typ weist alle Einschränkungen eines normalen Zeiger Typs auf.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-130">A `delegate*` type has all of the restrictions of a normal pointer type.</span></span>

### <a name="function-pointer-syntax"></a><span data-ttu-id="e8a7b-131">Funktionszeiger Syntax</span><span class="sxs-lookup"><span data-stu-id="e8a7b-131">Function pointer syntax</span></span>

<span data-ttu-id="e8a7b-132">Die vollständige Funktionszeiger Syntax wird durch die folgende Grammatik dargestellt:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-132">The full function pointer syntax is represented by the following grammar:</span></span>

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

<span data-ttu-id="e8a7b-133">Die `unmanaged`-Aufruf Konvention stellt die Standard Aufruf Konvention für nativen Code auf der aktuellen Plattform dar und wird als WINAPI codiert.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-133">The `unmanaged` calling convention represents the default calling convention for native code on the current platform, and is encoded as winapi.</span></span>
<span data-ttu-id="e8a7b-134">Alle `calling_convention`s sind kontextabhängige Schlüsselwörter, wenn eine `delegate*`vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-134">All `calling_convention`s are contextual keywords when preceded by a `delegate*`.</span></span>

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a><span data-ttu-id="e8a7b-135">Funktionszeiger Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="e8a7b-135">Function pointer conversions</span></span>

<span data-ttu-id="e8a7b-136">In einem unsicheren Kontext wird der Satz der verfügbaren impliziten Konvertierungen (implizite Konvertierungen) um die folgenden impliziten Zeiger Konvertierungen erweitert:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-136">In an unsafe context, the set of available implicit conversions (Implicit conversions) is extended to include the following implicit pointer conversions:</span></span>
- [<span data-ttu-id="e8a7b-137">_Vorhandene Konvertierungen_</span><span class="sxs-lookup"><span data-stu-id="e8a7b-137">_Existing conversions_</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- <span data-ttu-id="e8a7b-138">Von _funcptr\_Typ_ `F0` zu einem anderen _funkptr\_Typ_ `F1`, wenn alle folgenden Punkte zutreffen:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-138">From _funcptr\_type_ `F0` to another _funcptr\_type_ `F1`, provided all of the following are true:</span></span>
    - <span data-ttu-id="e8a7b-139">`F0` und `F1` haben die gleiche Anzahl von Parametern, und jeder Parameter `D0n` in `F0` hat dieselben `ref`-, `out`-oder `in` Modifizierer wie der entsprechende Parameter `D1n` `F1`.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-139">`F0` and `F1` have the same number of parameters, and each parameter `D0n` in `F0` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter `D1n` in `F1`.</span></span>
    - <span data-ttu-id="e8a7b-140">Für jeden value-Parameter (ein Parameter ohne `ref`, `out`oder `in` Modifizierer), ist eine Identitäts Konvertierung, eine implizite Verweis Konvertierung oder eine implizite Zeiger Konvertierung aus dem Parametertyp in `F0` in den entsprechenden Parametertyp in `F1`vorhanden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-140">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `F0` to the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="e8a7b-141">Für jeden `ref`-, `out`-oder `in`-Parameter ist der Parametertyp in `F0` mit dem entsprechenden Parametertyp in `F1`identisch.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-141">For each `ref`, `out`, or `in` parameter, the parameter type in `F0` is the same as the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="e8a7b-142">Wenn der Rückgabetyp nach Wert (keine `ref` oder `ref readonly`) ist, ist eine Identität, ein impliziter Verweis oder eine implizite Zeiger Konvertierung vom Rückgabetyp `F1` zum Rückgabetyp `F0`vorhanden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-142">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F1` to the return type of `F0`.</span></span>
    - <span data-ttu-id="e8a7b-143">Wenn der Rückgabetyp als Verweis (`ref` oder `ref readonly`) erfolgt, sind der Rückgabetyp und die `ref` modifizierermodifiziererer `F1` identisch mit dem Rückgabetyp und `ref` modifizierermodifizierer `F0`</span><span class="sxs-lookup"><span data-stu-id="e8a7b-143">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F1` are the same as the return type and `ref` modifiers of `F0`.</span></span>
    - <span data-ttu-id="e8a7b-144">Die Aufruf Konvention von `F0` ist mit der Aufruf Konvention `F1`identisch.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-144">The calling convention of `F0` is the same as the calling convention of `F1`.</span></span>

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="e8a7b-145">Address-of-to-target-Methoden zulassen</span><span class="sxs-lookup"><span data-stu-id="e8a7b-145">Allow address-of to target methods</span></span>

<span data-ttu-id="e8a7b-146">Methoden Gruppen werden nun als Argumente für einen Address-of-Ausdruck zugelassen.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-146">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="e8a7b-147">Der Typ eines solchen Ausdrucks ist eine `delegate*`, die über die entsprechende Signatur der Ziel Methode und eine verwaltete Aufruf Konvention verfügt:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-147">The type of such an expression will be a `delegate*` which has the equivalent signature of the target method and a managed calling convention:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

<span data-ttu-id="e8a7b-148">In einem unsicheren Kontext ist eine Methode `M` mit einem Funktions Zeigertyp kompatibel `F` wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-148">In an unsafe context, a method `M` is compatible with a function pointer type `F` if all of the following are true:</span></span>
- <span data-ttu-id="e8a7b-149">`M` und `F` haben die gleiche Anzahl von Parametern, und jeder Parameter in `D` hat dieselben `ref`, `out`oder `in` Modifizierer wie der entsprechende Parameter in `F`.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-149">`M` and `F` have the same number of parameters, and each parameter in `D` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter in `F`.</span></span>
- <span data-ttu-id="e8a7b-150">Für jeden value-Parameter (ein Parameter ohne `ref`, `out`oder `in` Modifizierer), ist eine Identitäts Konvertierung, eine implizite Verweis Konvertierung oder eine implizite Zeiger Konvertierung aus dem Parametertyp in `M` in den entsprechenden Parametertyp in `F`vorhanden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-150">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `M` to the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="e8a7b-151">Für jeden `ref`-, `out`-oder `in`-Parameter ist der Parametertyp in `M` mit dem entsprechenden Parametertyp in `F`identisch.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-151">For each `ref`, `out`, or `in` parameter, the parameter type in `M` is the same as the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="e8a7b-152">Wenn der Rückgabetyp nach Wert (keine `ref` oder `ref readonly`) ist, ist eine Identität, ein impliziter Verweis oder eine implizite Zeiger Konvertierung vom Rückgabetyp `F` zum Rückgabetyp `M`vorhanden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-152">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F` to the return type of `M`.</span></span>
- <span data-ttu-id="e8a7b-153">Wenn der Rückgabetyp als Verweis (`ref` oder `ref readonly`) erfolgt, sind der Rückgabetyp und die `ref` modifizierermodifiziererer `F` identisch mit dem Rückgabetyp und `ref` modifizierermodifizierer `M`</span><span class="sxs-lookup"><span data-stu-id="e8a7b-153">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F` are the same as the return type and `ref` modifiers of `M`.</span></span>
- <span data-ttu-id="e8a7b-154">Die Aufruf Konvention von `M` ist mit der Aufruf Konvention `F`identisch.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-154">The calling convention of `M` is the same as the calling convention of `F`.</span></span>
- <span data-ttu-id="e8a7b-155">`M` ist eine statische Methode.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-155">`M` is a static method.</span></span>

<span data-ttu-id="e8a7b-156">In einem unsicheren Kontext gibt es eine implizite Konvertierung von einem address-of-Ausdruck, dessen Ziel eine Methoden Gruppe ist `E` zu einem kompatiblen Funktions Zeigertyp `F`, wenn `E` mindestens eine Methode enthält, die in der normalen Form auf eine Argumentliste anwendbar ist, die durch die Verwendung der Parametertypen und Modifizierer von `F`erstellt wurde, wie im folgenden beschrieben.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-156">In an unsafe context, an implicit conversion exists from an address-of expression whose target is a method group `E` to a compatible function pointer type `F` if `E` contains at least one method that is applicable in its normal form to an argument list constructed by use of the parameter types and modifiers of `F`, as described in the following.</span></span>
- <span data-ttu-id="e8a7b-157">Es wird eine einzelne Methode `M` ausgewählt, die einem Methodenaufruf des Formulars entspricht, `E(A)` mit den folgenden Änderungen:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-157">A single method `M` is selected corresponding to a method invocation of the form `E(A)` with the following modifications:</span></span>
   - <span data-ttu-id="e8a7b-158">Bei der Argumentliste `A` handelt es sich um eine Liste von Ausdrücken, die jeweils als Variable und Typ und Modifizierer (`ref`, `out`oder `in`) des entsprechenden _formalen\_-Parameters\_Liste_ der `D`klassifiziert sind.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-158">The arguments list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref`, `out`, or `in`) of the corresponding _formal\_parameter\_list_ of `D`.</span></span>
   - <span data-ttu-id="e8a7b-159">Bei den Kandidaten Methoden handelt es sich nur um die Methoden, die in ihrer normalen Form anwendbar sind, nicht um die Methoden, die im erweiterten Formular anwendbar sind.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-159">The candidate methods are only those methods that are applicable in their normal form, not those applicable in their expanded form.</span></span>
   - <span data-ttu-id="e8a7b-160">Bei den Kandidaten Methoden handelt es sich nur um statische Methoden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-160">The candidate methods are only those methods that are static.</span></span>
- <span data-ttu-id="e8a7b-161">Wenn der Algorithmus von Methoden aufrufen einen Fehler erzeugt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-161">If the algorithm of Method invocations produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="e8a7b-162">Andernfalls erzeugt der Algorithmus eine einzige beste Methode `M` die gleiche Anzahl von Parametern wie `F` haben, und die Konvertierung wird als vorhanden betrachtet.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-162">Otherwise, the algorithm produces a single best method `M` having the same number of parameters as `F` and the conversion is considered to exist.</span></span>
- <span data-ttu-id="e8a7b-163">Die ausgewählte Methode `M` muss (wie oben definiert) mit dem Funktions Zeigertyp `F`kompatibel sein.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-163">The selected method `M` must be compatible (as defined above) with the function pointer type `F`.</span></span> <span data-ttu-id="e8a7b-164">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-164">Otherwise, a compile-time error occurs.</span></span>
- <span data-ttu-id="e8a7b-165">Das Ergebnis der Konvertierung ist ein Funktionszeiger vom Typ `F`.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-165">The result of the conversion is a function pointer of type `F`.</span></span>

<span data-ttu-id="e8a7b-166">Eine implizite Konvertierung ist von einem address-of-Ausdruck vorhanden, dessen Ziel eine Methoden Gruppe `E` ist, `void*`, wenn in `E`nur eine statische Methode `M` ist.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-166">An implicit conversion exists from an address-of expression whose target is a method group `E` to `void*` if there is only one static method `M` in `E`.</span></span>
<span data-ttu-id="e8a7b-167">Wenn eine statische Methode vorhanden ist, wird die einzige beste Methode aus `E` `M`.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-167">If there is one static method, then the single best method from `E` is `M`.</span></span>
<span data-ttu-id="e8a7b-168">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-168">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="e8a7b-169">Dies bedeutet, dass Entwickler von Regeln zur Überladungs Auflösung abhängig sein können, damit Sie in Verbindung mit dem address-of-Operator funktionieren:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-169">This means developers can depend on overload resolution rules to work in conjunction with the address-of operator:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

<span data-ttu-id="e8a7b-170">Der Address-of-Operator wird mithilfe der `ldftn` Anweisung implementiert.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-170">The address-of operator will be implemented using the `ldftn` instruction.</span></span>

<span data-ttu-id="e8a7b-171">Einschränkungen dieses Features:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-171">Restrictions of this feature:</span></span>

- <span data-ttu-id="e8a7b-172">Gilt nur für Methoden, die als `static`gekennzeichnet sind.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-172">Only applies to methods marked as `static`.</span></span>
- <span data-ttu-id="e8a7b-173">Nicht`static` lokale Funktionen können nicht in `&`verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-173">Non-`static` local functions cannot be used in `&`.</span></span> <span data-ttu-id="e8a7b-174">Die Implementierungsdetails dieser Methoden werden absichtlich nicht in der Sprache angegeben.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-174">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="e8a7b-175">Dies umfasst auch, ob es sich um statische oder Instanz handelt oder genau, mit welcher Signatur Sie ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-175">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="better-function-member"></a><span data-ttu-id="e8a7b-176">Besseres Funktionsmember</span><span class="sxs-lookup"><span data-stu-id="e8a7b-176">Better function member</span></span>

<span data-ttu-id="e8a7b-177">Die bessere Funktionsmember-Spezifikation wird so geändert, dass Sie die folgende Zeile enthält:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-177">The better function member specification will be changed to include the following line:</span></span>

> <span data-ttu-id="e8a7b-178">Eine `delegate*` ist spezifischer als `void*`</span><span class="sxs-lookup"><span data-stu-id="e8a7b-178">A `delegate*` is more specific than `void*`</span></span>

<span data-ttu-id="e8a7b-179">Dies bedeutet, dass es möglich ist, `void*` und eine `delegate*` zu überlasten und den Address-of-Operator weiterhin vernünftig zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-179">This means that it is possible to overload on `void*` and a `delegate*` and still sensibly use the address-of operator.</span></span>

## <a name="open-issues"></a><span data-ttu-id="e8a7b-180">Offene Probleme</span><span class="sxs-lookup"><span data-stu-id="e8a7b-180">Open Issues</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="e8a7b-181">Nativecallableattribute</span><span class="sxs-lookup"><span data-stu-id="e8a7b-181">NativeCallableAttribute</span></span>

<span data-ttu-id="e8a7b-182">Dabei handelt es sich um ein Attribut, das von der CLR verwendet wird, um das verwaltete zu systemeigene Prolog zu vermeiden, wenn</span><span class="sxs-lookup"><span data-stu-id="e8a7b-182">This is an attribute used by the CLR to avoid the managed to native prologue when invoking.</span></span> <span data-ttu-id="e8a7b-183">Von diesem Attribut gekennzeichnete Methoden können nur aus nativem Code aufgerufen werden, nicht verwaltet (Methoden nicht aufrufen, Delegaten erstellen usw.).</span><span class="sxs-lookup"><span data-stu-id="e8a7b-183">Methods marked by this attribute are only callable from native code, not managed (can’t call methods, create a delegate, etc …).</span></span> <span data-ttu-id="e8a7b-184">Das-Attribut ist für mscorlib nicht spezifisch. die Laufzeit behandelt alle Attribute mit diesem Namen mit derselben Semantik.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-184">The attribute is not special to mscorlib; the runtime will treat any attribute with this name with the same semantics.</span></span>

<span data-ttu-id="e8a7b-185">Es ist möglich, dass die Laufzeit und die Sprache zusammenarbeiten, um dies vollständig zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-185">It's possible for the runtime and language to work together to fully support this.</span></span> <span data-ttu-id="e8a7b-186">Die Sprache kann die Address-of-`static` Member mit einem `NativeCallable`-Attribut als `delegate*` mit der angegebenen Aufruf Konvention behandeln.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-186">The language could choose to treat address-of `static` members with a `NativeCallable` attribute as a `delegate*` with the specified calling convention.</span></span>

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

<span data-ttu-id="e8a7b-187">Außerdem sollte die Sprache wahrscheinlich auch folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-187">Additionally the language would likely also want to:</span></span>

- <span data-ttu-id="e8a7b-188">Kennzeichnen Sie alle verwalteten Aufrufe einer Methode, die mit `NativeCallable` gekennzeichnet ist, als Fehler.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-188">Flag any managed calls to a method tagged with `NativeCallable` as an error.</span></span> <span data-ttu-id="e8a7b-189">Da die Funktion nicht aus verwaltetem Code aufgerufen werden kann, sollte der Compiler verhindern, dass Entwickler einen solchen Aufruf durchgeführt haben.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-189">Given the function can't be invoked from managed code the compiler should prevent developers from attempting such an invocation.</span></span>
- <span data-ttu-id="e8a7b-190">Verhindern, dass Methoden Gruppen Konvertierungen `delegate` werden, wenn die-Methode mit `NativeCallable`gekennzeichnet ist.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-190">Prevent method group conversions to `delegate` when the method is tagged with `NativeCallable`.</span></span>

<span data-ttu-id="e8a7b-191">Dies ist für die Unterstützung von `NativeCallable` jedoch nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-191">This is not necessary to support `NativeCallable` though.</span></span> <span data-ttu-id="e8a7b-192">Der Compiler kann das `NativeCallable`-Attribut unterstützen, wie es die vorhandene Syntax verwendet.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-192">The compiler can support the `NativeCallable` attribute as is using the existing syntax.</span></span> <span data-ttu-id="e8a7b-193">Das Programm muss lediglich in `void*` umgewandelt werden, bevor es in die richtige `delegate*` Signatur umgewandelt wird.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-193">The program would simply need to cast to `void*` before casting to the correct `delegate*` signature.</span></span> <span data-ttu-id="e8a7b-194">Das wäre nicht schlechter als die Unterstützung heute.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-194">That would be no worse than the support today.</span></span>

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a><span data-ttu-id="e8a7b-195">Erweiterbarer Satz nicht verwalteter Aufruf Konventionen</span><span class="sxs-lookup"><span data-stu-id="e8a7b-195">Extensible set of unmanaged calling conventions</span></span>

<span data-ttu-id="e8a7b-196">Der Satz der nicht verwalteten Aufruf Konventionen, die von den aktuellen ECMA-335-Codierungen unterstützt werden, ist veraltet.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-196">The set of unmanaged calling conventions supported by the current ECMA-335 encodings is outdated.</span></span> <span data-ttu-id="e8a7b-197">Wir haben Anforderungen zum Hinzufügen von Unterstützung für mehr nicht verwaltete Aufruf Konventionen gesehen, z. b.:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-197">We have seen requests to add support for more unmanaged calling conventions, for example:</span></span>

- <span data-ttu-id="e8a7b-198">[vectorcallcenter](https://docs.microsoft.com/cpp/cpp/vectorcall) - https://github.com/dotnet/coreclr/issues/12120</span><span class="sxs-lookup"><span data-stu-id="e8a7b-198">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span></span>
- <span data-ttu-id="e8a7b-199">Stdcallmit expliziten diesem https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span><span class="sxs-lookup"><span data-stu-id="e8a7b-199">StdCall with explicit this https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span></span>

<span data-ttu-id="e8a7b-200">Das Design dieses Features sollte die Erweiterung der nicht verwalteten Aufruf Konventionen nach Bedarf in Zukunft ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-200">The design of this feature should allow extending the set of unmanaged calling conventions as needed in future.</span></span> <span data-ttu-id="e8a7b-201">Die Probleme umfassen den begrenzten Speicherplatz für das Codieren von Aufruf Konventionen (12 von 16 Werten werden in `IMAGE_CEE_CS_CALLCONV_MASK`) und die Anzahl von stellen, die berührt werden müssen, um eine neue Aufruf Konvention hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-201">The problems include limited space for encoding calling conventions (12 out of 16 values are taken in `IMAGE_CEE_CS_CALLCONV_MASK`) and number of places that need to be touched in order to add a new calling convention.</span></span> <span data-ttu-id="e8a7b-202">Eine mögliche Lösung besteht darin, eine neue Codierung einzuführen, die die Aufruf Konvention mithilfe [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) -Enum repräsentiert.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-202">A potential solution is to introduce a new encoding that represents the calling convention using [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enum.</span></span>

<span data-ttu-id="e8a7b-203">https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h enthält die Liste der Aufruf Konventionen, die von llvm unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-203">For reference, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h has the list of calling conventions supported by LLVM.</span></span> <span data-ttu-id="e8a7b-204">Obwohl es unwahrscheinlich ist, dass .net immer alle unterstützen muss, wird veranschaulicht, dass der Raum der Aufruf Konventionen sehr umfangreich ist.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-204">While it is unlikely that .NET will ever need to support all of them, it demonstrates that the space of calling conventions is very rich.</span></span>

## <a name="considerations"></a><span data-ttu-id="e8a7b-205">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="e8a7b-205">Considerations</span></span>

### <a name="allow-instance-methods"></a><span data-ttu-id="e8a7b-206">Instanzmethoden zulassen</span><span class="sxs-lookup"><span data-stu-id="e8a7b-206">Allow instance methods</span></span>

<span data-ttu-id="e8a7b-207">Der Vorschlag könnte so erweitert werden, dass Instanzmethoden unterstützt werden, indem die `EXPLICITTHIS` CLI-Aufruf Konvention C# (mit dem Namen `instance` im Code) genutzt wird.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-207">The proposal could be extended to support instance methods by taking advantage of the `EXPLICITTHIS` CLI calling convention (named `instance` in C# code).</span></span> <span data-ttu-id="e8a7b-208">Diese Form von CLI-Funktions Zeigern legt den `this`-Parameter als expliziten ersten Parameter der Funktionszeiger Syntax ab.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-208">This form of CLI function pointers puts the `this` parameter as an explicit first parameter of the function pointer syntax.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

<span data-ttu-id="e8a7b-209">Das ist zwar vernünftig, aber fügt dem Vorschlag eine gewisse Komplikation hinzu.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-209">This is sound but adds some complication to the proposal.</span></span> <span data-ttu-id="e8a7b-210">Insbesondere weil Funktionszeiger, die sich durch die Aufruf Konvention unterscheiden, `instance` und `managed` nicht kompatibel sind, auch wenn beide Fälle verwendet werden, um C# verwaltete Methoden mit derselben Signatur aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-210">Particularly because function pointers which differed by the calling convention `instance` and `managed` would be incompatible even though both cases are used to invoke managed methods with the same C# signature.</span></span> <span data-ttu-id="e8a7b-211">In jedem Fall ist es sinnvoll, eine einfache Problem Umgehung zu finden: Verwenden Sie eine `static` lokale Funktion.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-211">Also in every case considered where this would be valuable to have there was a simple work around: use a `static` local function.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a><span data-ttu-id="e8a7b-212">Unsichere Deklaration nicht erforderlich</span><span class="sxs-lookup"><span data-stu-id="e8a7b-212">Don't require unsafe at declaration</span></span>

<span data-ttu-id="e8a7b-213">Anstatt `unsafe` bei jeder Verwendung eines `delegate*`zu benötigen, benötigen Sie nur an dem Punkt, an dem eine Methoden Gruppe in einen `delegate*`konvertiert wird.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-213">Instead of requiring `unsafe` at every use of a `delegate*`, only require it at the point where a method group is converted to a `delegate*`.</span></span> <span data-ttu-id="e8a7b-214">Hier kommen die wichtigsten Sicherheitsprobleme ins Spiel (wenn Sie wissen, dass die enthaltende Assembly nicht entladen werden kann, während der Wert aktiv ist).</span><span class="sxs-lookup"><span data-stu-id="e8a7b-214">This is where the core safety issues come into play (knowing that the containing assembly cannot be unloaded while the value is alive).</span></span> <span data-ttu-id="e8a7b-215">Das verlangen von `unsafe` an anderen Speicherorten kann als übertrieben angesehen werden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-215">Requiring `unsafe` on the other locations can be seen as excessive.</span></span>

<span data-ttu-id="e8a7b-216">Auf diese Weise wurde der Entwurf ursprünglich beabsichtigt.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-216">This is how the design was originally intended.</span></span> <span data-ttu-id="e8a7b-217">Die sich ergebenden Sprachregeln waren jedoch sehr umständlich.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-217">But the resulting language rules felt very awkward.</span></span> <span data-ttu-id="e8a7b-218">Es ist nicht möglich, die Tatsache auszublenden, dass es sich um einen Zeiger Wert handelt, und auch ohne das Schlüsselwort "`unsafe`".</span><span class="sxs-lookup"><span data-stu-id="e8a7b-218">It's impossible to hide the fact that this is a pointer value and it kept peeking through even without the `unsafe` keyword.</span></span> <span data-ttu-id="e8a7b-219">Beispielsweise kann die Konvertierung in `object` nicht zulässig sein, Sie kann nicht Mitglied einer `class`sein usw... Der C# Entwurf besteht darin, `unsafe` für alle Zeiger Verwendungen anzufordern, weshalb dieses Design darauf folgt.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-219">For example the conversion to `object` can't be allowed, it can't be a member of a `class`, etc ... The C# design is to require `unsafe` for all pointer uses and hence this design follows that.</span></span>

<span data-ttu-id="e8a7b-220">Entwickler können weiterhin einen _sicheren_ Wrapper zusätzlich zu `delegate*` Werten auf die gleiche Weise wie für normale Zeiger Typen darstellen.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-220">Developers will still be capable of presenting a _safe_ wrapper on top of `delegate*` values the same way that they do for normal pointer types today.</span></span> <span data-ttu-id="e8a7b-221">Berücksichtigen Sie dabei Folgendes:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-221">Consider:</span></span>

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a><span data-ttu-id="e8a7b-222">Verwenden von Delegaten</span><span class="sxs-lookup"><span data-stu-id="e8a7b-222">Using delegates</span></span>

<span data-ttu-id="e8a7b-223">Verwenden Sie anstelle eines neuen Syntax Elements `delegate*`einfach vorhandene `delegate` Typen mit einem `*`, das auf den-Typ folgt:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-223">Instead of using a new syntax element, `delegate*`, simply use existing `delegate` types with a `*` following the type:</span></span>

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

<span data-ttu-id="e8a7b-224">Um die Aufruf Konvention zu verarbeiten, können Sie die `delegate` Typen mit einem Attribut versehen, das einen `CallingConvention` Wert angibt.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-224">Handling calling convention can be done by annotating the `delegate` types with an attribute that specifies a `CallingConvention` value.</span></span> <span data-ttu-id="e8a7b-225">Das Fehlen eines Attributs würde die verwaltete Aufruf Konvention bedeuten.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-225">The lack of an attribute would signify the managed calling convention.</span></span>

<span data-ttu-id="e8a7b-226">Das Codieren in Il ist problematisch.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-226">Encoding this in IL is problematic.</span></span> <span data-ttu-id="e8a7b-227">Der zugrunde liegende Wert muss als Zeiger dargestellt werden, aber er muss auch Folgendes aufweisen:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-227">The underlying value needs to be represented as a pointer yet it also must:</span></span>

1. <span data-ttu-id="e8a7b-228">Sie haben einen eindeutigen Typ, der über Ladungen mit unterschiedlichen Funktionszeiger Typen zulässt.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-228">Have a unique type to allow for overloads with different function pointer types.</span></span>
1. <span data-ttu-id="e8a7b-229">Für ohi-Zwecke über assemblygrenzen hinweg gleichwertig sein.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-229">Be equivalent for OHI purposes across assembly boundaries.</span></span>

<span data-ttu-id="e8a7b-230">Der letzte Punkt ist besonders problematisch.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-230">The last point is particularly problematic.</span></span> <span data-ttu-id="e8a7b-231">Dies bedeutet, dass jede Assembly, die `Func<int>*` verwendet, einen entsprechenden Typ in Metadaten codieren muss, obwohl `Func<int>*` in einer Assembly definiert ist, die jedoch nicht von gesteuert wird.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-231">This mean that every assembly which uses `Func<int>*` must encode an equivalent type in metadata even though `Func<int>*` is defined in an assembly though don't control.</span></span>
<span data-ttu-id="e8a7b-232">Außerdem muss jeder andere Typ, der mit dem Namen `System.Func<T>` in einer Assembly definiert ist, die nicht mscorlib ist, sich von der in mscorlib definierten Version unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-232">Additionally any other type which is defined with the name `System.Func<T>` in an assembly that is not mscorlib must be different than the version defined in mscorlib.</span></span>

<span data-ttu-id="e8a7b-233">Eine Option, die untersucht wurde, hat einen derartigen Zeiger als `mod_req(Func<int>) void*`ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-233">One option that was explored was emitting such a pointer as `mod_req(Func<int>) void*`.</span></span> <span data-ttu-id="e8a7b-234">Dies funktioniert jedoch nicht, da eine `mod_req` nicht an eine `TypeSpec` gebunden werden kann und daher keine generischen Instanziierungen als Ziel haben kann.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-234">This doesn't work though as a `mod_req` cannot bind to a `TypeSpec` and hence cannot target generic instantiations.</span></span>

### <a name="named-function-pointers"></a><span data-ttu-id="e8a7b-235">Benannte Funktionszeiger</span><span class="sxs-lookup"><span data-stu-id="e8a7b-235">Named function pointers</span></span>

<span data-ttu-id="e8a7b-236">Die Funktionszeiger Syntax kann mühsam sein, insbesondere in komplexen Fällen wie z. b. für die Zeiger auf die Zeichen.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-236">The function pointer syntax can be cumbersome, particularly in complex cases like nested function pointers.</span></span> <span data-ttu-id="e8a7b-237">Anstatt den Entwicklern die Möglichkeit zu geben, die Signatur jedes Mal einzugeben, wenn die Sprache benannte Deklarationen von Funktions Zeigern zulässt, wie es bei `delegate`der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-237">Rather than have developers type out the signature every time the language could allow for named declarations of function pointers as is done with `delegate`.</span></span>

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

<span data-ttu-id="e8a7b-238">Ein Teil des Problems hier ist, dass der zugrunde liegende CLI-primitiv keine Namen hat, C# daher wäre dies eine rein Erfindung und erfordert ein wenig zu aktivierende metadatenarbeit.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-238">Part of the problem here is the underlying CLI primitive doesn't have names hence this would be purely a C# invention and require a bit of metadata work to enable.</span></span> <span data-ttu-id="e8a7b-239">Das ist zwar möglich, ist aber ein bedeutender Bezug zur Arbeit.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-239">That is doable but is a significant about of work.</span></span> <span data-ttu-id="e8a7b-240">Es ist im C# wesentlichen erforderlich, dass ein Companion für die Typdef-Tabelle ausschließlich für diese Namen vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-240">It essentially requires C# to have a companion to the type def table purely for these names.</span></span>

<span data-ttu-id="e8a7b-241">Auch wenn die Argumente für benannte Funktionszeiger untersucht wurden, haben wir festgestellt, dass Sie auch auf eine Reihe anderer Szenarios gleichermaßen gut angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-241">Also when the arguments for named function pointers were examined we found they could apply equally well to a number of other scenarios.</span></span> <span data-ttu-id="e8a7b-242">Beispielsweise wäre es genauso praktisch, benannte Tupel zu deklarieren, um die Notwendigkeit zu verringern, die vollständige Signatur in allen Fällen einzugeben.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-242">For example it would be just as convenient to declare named tuples to reduce the need to type out the full signature in all cases.</span></span>

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

<span data-ttu-id="e8a7b-243">Nach der Erörterung haben wir uns entschieden, die benannte Deklaration von `delegate*` Typen nicht zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-243">After discussion we decided to not allow named declaration of `delegate*` types.</span></span> <span data-ttu-id="e8a7b-244">Wenn Sie feststellen, dass es aufgrund des Feedbacks der Kunden eine beträchtliche Notwendigkeit gibt, werden wir eine namens Lösung untersuchen, die für Funktionszeiger, Tupel, Generika usw. funktioniert. Dies ist in der Regel ähnlich wie die vollständige `typedef` Unterstützung in der Sprache.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-244">If we find there is significant need for this based on customer usage feedback then we will investigate a naming solution that works for function pointers, tuples, generics, etc ... This is likely to be similar in form to other suggestions like full `typedef` support in the language.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="e8a7b-245">Überlegungen für die Zukunft</span><span class="sxs-lookup"><span data-stu-id="e8a7b-245">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="e8a7b-246">statische lokale Funktionen</span><span class="sxs-lookup"><span data-stu-id="e8a7b-246">static local functions</span></span>

<span data-ttu-id="e8a7b-247">Dies bezieht sich auf [den Vorschlag](https://github.com/dotnet/csharplang/issues/1565) , den `static` Modifizierer für lokale Funktionen zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-247">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="e8a7b-248">Eine solche Funktion wird garantiert als `static` und mit der exakten Signatur ausgegeben, die im Quellcode angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-248">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="e8a7b-249">Eine solche Funktion sollte ein gültiges Argument für `&` sein, da Sie keines der Probleme aufweist, die lokale Funktionen heute aufweisen.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-249">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today</span></span>

### <a name="static-delegates"></a><span data-ttu-id="e8a7b-250">statische Delegaten</span><span class="sxs-lookup"><span data-stu-id="e8a7b-250">static delegates</span></span>

<span data-ttu-id="e8a7b-251">Dies bezieht sich auf [den Vorschlag](https://github.com/dotnet/csharplang/issues/302) , um die Deklaration von `delegate` Typen zu ermöglichen, die nur auf `static` Member verweisen können.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-251">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/302) to allow for the declaration of `delegate` types which can only refer to `static` members.</span></span> <span data-ttu-id="e8a7b-252">Der Vorteil, dass solche `delegate` Instanzen in Leistungs sensiblen Szenarios kostenfrei und besser sind.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-252">The advantage being that such `delegate` instances can be allocation free and better in performance sensitive scenarios.</span></span>

<span data-ttu-id="e8a7b-253">Wenn die Funktionszeiger Funktion implementiert ist, wird der `static delegate` Vorschlag wahrscheinlich geschlossen. Der vorgeschlagene Vorteil dieses Features ist die Art der Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-253">If the function pointer feature is implemented the `static delegate` proposal will likely be closed out. The proposed advantage of that feature is the allocation free nature.</span></span> <span data-ttu-id="e8a7b-254">Es wurden jedoch letzte Untersuchungen festgestellt, die aufgrund der Entladung der Assembly nicht möglich sind.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-254">However recent investigations have found that is not possible to achieve due to assembly unloading.</span></span> <span data-ttu-id="e8a7b-255">Es muss ein sicheres Handle von der `static delegate` zu der Methode vorhanden sein, auf die Sie verweist, damit die Assembly nicht aus ihr entladen wird.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-255">There must be a strong handle from the `static delegate` to the method it refers to in order to keep the assembly from being unloaded out from under it.</span></span>

<span data-ttu-id="e8a7b-256">Um alle `static delegate`-Instanz beizubehalten, wäre es erforderlich, ein neues Handle zuzuweisen, das den Zielen des Angebots entgegensteht.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-256">To maintain every `static delegate` instance would be required to allocate a new handle which runs counter to the goals of the proposal.</span></span> <span data-ttu-id="e8a7b-257">Es gab einige Entwürfe, bei denen die Zuordnung zu einer einzelnen Zuordnung pro aufrufssite amortisiert werden konnte, aber das war ein wenig komplexer und sah nicht den Kompromiss aus.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-257">There were some designs where the allocation could be amortized to a single allocation per call-site but that was a bit complex and didn't seem worth the trade off.</span></span>

<span data-ttu-id="e8a7b-258">Dies bedeutet, dass Entwickler sich grundsätzlich zwischen den folgenden Kompromisse entscheiden müssen:</span><span class="sxs-lookup"><span data-stu-id="e8a7b-258">That means developers essentially have to decide between the following trade offs:</span></span>

1. <span data-ttu-id="e8a7b-259">Sicherheit beim Entladen der Assembly: Dies erfordert Zuordnungen, sodass `delegate` bereits eine ausreichende Option ist.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-259">Safety in the face of assembly unloading: this requires allocations and hence `delegate` is already a sufficient option.</span></span>
1. <span data-ttu-id="e8a7b-260">Keine Sicherheit beim Entladen der Assembly: Verwenden Sie einen `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-260">No safety in face of assembly unloading: use a `delegate*`.</span></span> <span data-ttu-id="e8a7b-261">Dies kann in einem `struct` umschließt werden, um die Verwendung außerhalb eines `unsafe` Kontexts im restlichen Code zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="e8a7b-261">This can be wrapped in a `struct` to allow usage outside an `unsafe` context in the rest of the code.</span></span>
