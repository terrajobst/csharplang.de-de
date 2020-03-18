---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "79483961"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a><span data-ttu-id="fa579-101">Erzwingung der Sicherheit für Verweis ähnliche Typen in der Kompilierzeit</span><span class="sxs-lookup"><span data-stu-id="fa579-101">Compile time enforcement of safety for ref-like types</span></span>

## <a name="introduction"></a><span data-ttu-id="fa579-102">Einführung</span><span class="sxs-lookup"><span data-stu-id="fa579-102">Introduction</span></span>

<span data-ttu-id="fa579-103">Der Hauptgrund für die zusätzlichen Sicherheitsregeln beim Umgang mit Typen wie `Span<T>` und `ReadOnlySpan<T>` besteht darin, dass solche Typen auf den Ausführungs Stapel beschränkt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="fa579-103">The main reason for the additional safety rules when dealing with types like `Span<T>` and `ReadOnlySpan<T>` is that such types must be confined to the execution stack.</span></span>
 
<span data-ttu-id="fa579-104">Es gibt zwei Gründe, warum `Span<T>` und ähnliche Typen nur Stapel-Typen sein müssen.</span><span class="sxs-lookup"><span data-stu-id="fa579-104">There are two reasons why `Span<T>` and similar types must be a stack-only types.</span></span>

1. <span data-ttu-id="fa579-105">`Span<T>` ist semantisch eine Struktur, die einen Verweis und einen Bereichs `(ref T data, int length)`enthält.</span><span class="sxs-lookup"><span data-stu-id="fa579-105">`Span<T>` is semantically a struct containing a reference and a range - `(ref T data, int length)`.</span></span> <span data-ttu-id="fa579-106">Unabhängig von der eigentlichen Implementierung wäre das Schreiben in eine solche Struktur nicht atomarisch.</span><span class="sxs-lookup"><span data-stu-id="fa579-106">Regardless of actual implementation, writes to such struct would not be atomic.</span></span> <span data-ttu-id="fa579-107">Das gleichzeitige "zerreißen" einer solchen Struktur würde dazu führen, dass `length` nicht mit der `data`übereinstimmt, was zu Zugriffs enden Zugriffen und typsicherheitsverstößen führt, was letztendlich zu einer GC-Heap Beschädigung im scheinbar "sicheren" Code führen könnte.</span><span class="sxs-lookup"><span data-stu-id="fa579-107">Concurrent "tearing" of such struct would lead to the possibility of `length` not matching the `data`, causing out-of-range accesses and type-safety violations, which ultimately could result in GC heap corruption in seemingly "safe" code.</span></span>
2. <span data-ttu-id="fa579-108">Einige Implementierungen von `Span<T>` in einem ihrer Felder buchstäblich einen verwalteten Zeiger enthalten.</span><span class="sxs-lookup"><span data-stu-id="fa579-108">Some implementations of `Span<T>` literally contain a managed pointer in one of its fields.</span></span> <span data-ttu-id="fa579-109">Verwaltete Zeiger werden nicht als Felder von Heap Objekten unterstützt, und Code, der einen verwalteten Zeiger auf den GC-Heap speichert, stürzt normalerweise bei der JIT-Zeit ab.</span><span class="sxs-lookup"><span data-stu-id="fa579-109">Managed pointers are not supported as fields of heap objects and code that manages to put a managed pointer on the GC heap typically crashes at JIT time.</span></span>

<span data-ttu-id="fa579-110">Alle oben genannten Probleme würden verringert werden, wenn Instanzen von `Span<T>` nur auf dem Ausführungs Stapel vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="fa579-110">All the above problems would be alleviated if instances of `Span<T>` are constrained to exist only on the execution stack.</span></span> 

<span data-ttu-id="fa579-111">Aufgrund der Komposition entsteht ein zusätzliches Problem.</span><span class="sxs-lookup"><span data-stu-id="fa579-111">An additional problem arises due to composition.</span></span> <span data-ttu-id="fa579-112">Im allgemeinen wäre es wünschenswert, komplexere Datentypen zu erstellen, die `Span<T>`-und `ReadOnlySpan<T>`-Instanzen einbetten würden.</span><span class="sxs-lookup"><span data-stu-id="fa579-112">It would be generally desirable to build more complex data types that would embed `Span<T>` and `ReadOnlySpan<T>` instances.</span></span> <span data-ttu-id="fa579-113">Solche zusammengesetzten Typen müssten Strukturen sein und alle Risiken und Anforderungen der `Span<T>`teilen.</span><span class="sxs-lookup"><span data-stu-id="fa579-113">Such composite types would have to be structs and would share all the hazards and requirements of `Span<T>`.</span></span> <span data-ttu-id="fa579-114">Daher sollten die hier beschriebenen Sicherheitsregeln für den gesamten Bereich von **_ref-ähnlichen Typen_** angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="fa579-114">As a result the safety rules described here should be viewed as applicable to the whole range of **_ref-like types_**.</span></span>

<span data-ttu-id="fa579-115">Die [Spezifikation für die Entwurfs Sprache](#draft-language-specification) soll sicherstellen, dass die Werte eines Verweis ähnlichen Typs nur im Stapel vorkommen.</span><span class="sxs-lookup"><span data-stu-id="fa579-115">The [draft language specification](#draft-language-specification) is intended to ensure that values of a ref-like type occurs only on the stack.</span></span>

## <a name="generalized-ref-like-types-in-source-code"></a><span data-ttu-id="fa579-116">Verallgemeinerte `ref-like` Typen im Quellcode</span><span class="sxs-lookup"><span data-stu-id="fa579-116">Generalized `ref-like` types in source code</span></span>

<span data-ttu-id="fa579-117">`ref-like` Strukturen werden mithilfe `ref` Modifizierers explizit im Quellcode gekennzeichnet:</span><span class="sxs-lookup"><span data-stu-id="fa579-117">`ref-like` structs are explicitly marked in the source code using `ref` modifier:</span></span>

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

<span data-ttu-id="fa579-118">Wenn eine Struktur als ref-like festgelegt wird, können in der Struktur Verweis ähnliche Instanzfelder verwendet werden, und es werden auch alle Anforderungen von Verweis ähnlichen Typen auf die Struktur angewendet.</span><span class="sxs-lookup"><span data-stu-id="fa579-118">Designating a struct as ref-like will allow the struct to have ref-like instance fields and will also make all the requirements of ref-like types applicable to the struct.</span></span> 

## <a name="metadata-representation-or-ref-like-structs"></a><span data-ttu-id="fa579-119">Metadatendarstellung oder ref-like-Strukturen</span><span class="sxs-lookup"><span data-stu-id="fa579-119">Metadata representation or ref-like structs</span></span>

<span data-ttu-id="fa579-120">Ref-like-Strukturen werden mit dem **System. Runtime. CompilerServices. isreflikeattribute** -Attribut markiert.</span><span class="sxs-lookup"><span data-stu-id="fa579-120">Ref-like structs will be marked with **System.Runtime.CompilerServices.IsRefLikeAttribute** attribute.</span></span>

<span data-ttu-id="fa579-121">Das-Attribut wird zu allgemeinen Basis Bibliotheken hinzugefügt, z. b. `mscorlib`.</span><span class="sxs-lookup"><span data-stu-id="fa579-121">The attribute will be added to common base libraries such as `mscorlib`.</span></span> <span data-ttu-id="fa579-122">Wenn das-Attribut nicht verfügbar ist, generiert der Compiler eine interne, ähnlich wie andere eingebettete Attribute, wie z. b. `IsReadOnlyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="fa579-122">In a case if the attribute is not available, compiler will generate an internal one similarly to other embedded-on-demand attributes such as `IsReadOnlyAttribute`.</span></span>

<span data-ttu-id="fa579-123">Es wird ein zusätzliches Measure verwendet, um die Verwendung von Verweis ähnlichen Strukturen in Compilern zu verhindern, die mit den Sicherheitsregeln nicht vertraut sind C# (Dies schließt Compiler ein, die vor dem implementiert sind, in dem diese Funktion implementiert ist).</span><span class="sxs-lookup"><span data-stu-id="fa579-123">An additional measure will be taken to prevent the use of ref-like structs in compilers not familiar with the safety rules (this includes C# compilers prior to the one in which this feature is implemented).</span></span> 

<span data-ttu-id="fa579-124">Wenn keine anderen guten Alternativen vorhanden sind, die in alten Compilern ohne Wartung funktionieren, wird einem `Obsolete` Attribut mit einer bekannten Zeichenfolge alle Ref-like-Strukturen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="fa579-124">Having no other good alternatives that work in old compilers without servicing, an `Obsolete` attribute with a known string will be added to all ref-like structs.</span></span> <span data-ttu-id="fa579-125">Compiler, die wissen, wie ref-like-Typen verwendet werden, ignorieren diese spezielle Form von `Obsolete`.</span><span class="sxs-lookup"><span data-stu-id="fa579-125">Compilers that know how to use ref-like types will ignore this particular form of `Obsolete`.</span></span>

<span data-ttu-id="fa579-126">Eine typische Metadatendarstellung:</span><span class="sxs-lookup"><span data-stu-id="fa579-126">A typical metadata representation:</span></span>

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

<span data-ttu-id="fa579-127">Hinweis: Es ist nicht das Ziel, es so zu machen, dass die Verwendung von Verweis ähnlichen Typen für alte Compiler 100% fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="fa579-127">NOTE: it is not the goal to make it so that any use of ref-like types on old compilers fails 100%.</span></span> <span data-ttu-id="fa579-128">Das ist schwer zu erreichen und ist nicht unbedingt erforderlich.</span><span class="sxs-lookup"><span data-stu-id="fa579-128">That is hard to achieve and is not strictly necessary.</span></span> <span data-ttu-id="fa579-129">Beispielsweise ist es immer möglich, die `Obsolete` mithilfe von dynamischem Code zu umgehen oder beispielsweise ein Array von Verweis ähnlichen Typen durch Reflektion zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa579-129">For example there would always be a way to get around the `Obsolete` using dynamic code or, for example, creating an array of ref-like types through reflection.</span></span>

<span data-ttu-id="fa579-130">Insbesondere, wenn der Benutzer eine `Obsolete` oder ein `Deprecated` Attribut in einem ref-like-Typ platzieren möchte, haben wir keine andere Wahl als die nicht vordefinierte, da `Obsolete` Attribut nicht mehrmals angewendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="fa579-130">In particular, if user wants to actually put an `Obsolete` or `Deprecated` attribute on a ref-like type, we will have no choice other than not emitting the predefined one since `Obsolete` attribute cannot be applied more than once..</span></span>  

## <a name="examples"></a><span data-ttu-id="fa579-131">Beispiele:</span><span class="sxs-lookup"><span data-stu-id="fa579-131">Examples:</span></span>

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a><span data-ttu-id="fa579-132">Entwurfs Sprachen Spezifikation</span><span class="sxs-lookup"><span data-stu-id="fa579-132">Draft language specification</span></span>

<span data-ttu-id="fa579-133">Im folgenden wird ein Satz von Sicherheitsregeln für Verweis ähnliche Typen (`ref struct`s) beschrieben, um sicherzustellen, dass die Werte dieser Typen nur im Stapel vorkommen.</span><span class="sxs-lookup"><span data-stu-id="fa579-133">Below we describe a set of safety rules for ref-like types (`ref struct`s) to ensure that values of these types occur only on the stack.</span></span> <span data-ttu-id="fa579-134">Ein anderer, einfacherer Satz von Sicherheitsregeln wäre möglich, wenn die lokalen Variablen nicht als Verweis übermittelt werden können.</span><span class="sxs-lookup"><span data-stu-id="fa579-134">A different, simpler set of safety rules would be possible if locals cannot be passed by reference.</span></span> <span data-ttu-id="fa579-135">Diese Spezifikation ermöglicht außerdem die sichere Neuzuweisung von lokalen Ref-Variablen.</span><span class="sxs-lookup"><span data-stu-id="fa579-135">This specification would also permit the safe reassignment of ref locals.</span></span>

### <a name="overview"></a><span data-ttu-id="fa579-136">Übersicht</span><span class="sxs-lookup"><span data-stu-id="fa579-136">Overview</span></span>

<span data-ttu-id="fa579-137">Wir ordnen jedem Ausdruck zum Zeitpunkt der Kompilierung das Konzept des Bereichs zu, mit dem dieser Ausdruck als Escapezeichen versehen werden darf.</span><span class="sxs-lookup"><span data-stu-id="fa579-137">We associate with each expression at compile-time the concept of what scope that expression is permitted to escape to, "safe-to-escape".</span></span> <span data-ttu-id="fa579-138">Ebenso behalten wir für jeden lvalue ein Konzept, mit dem ein Verweis auf das Escapezeichen "Ref-Safe-to-Escape" versehen werden kann.</span><span class="sxs-lookup"><span data-stu-id="fa579-138">Similarly, for each lvalue we maintain a concept of what scope a reference to it is permitted to escape to, "ref-safe-to-escape".</span></span> <span data-ttu-id="fa579-139">Bei einem bestimmten lvalue-Ausdruck können sich diese unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="fa579-139">For a given lvalue expression, these may be different.</span></span>

<span data-ttu-id="fa579-140">Diese entsprechen der "sicheren Rückgabe" des Features "Ref Locals", aber Sie ist präziser.</span><span class="sxs-lookup"><span data-stu-id="fa579-140">These are analogous to the "safe to return" of the ref locals feature, but it is more fine-grained.</span></span> <span data-ttu-id="fa579-141">Wenn der "Safe-to-return"-Ausdruck eines Ausdrucks nur dann aufgezeichnet wird, ob er die einschließende Methode als Ganzes mit einem Escapezeichen versehen soll (oder nicht), werden die Datensätze mit sicherer Escapezeichen aufgezeichnet.</span><span class="sxs-lookup"><span data-stu-id="fa579-141">Where the "safe-to-return" of an expression records only whether (or not) it may escape the enclosing method as a whole, the safe-to-escape records which scope it may escape to (which scope it may not escape beyond).</span></span> <span data-ttu-id="fa579-142">Der grundlegende Sicherheitsmechanismus wird wie folgt erzwungen.</span><span class="sxs-lookup"><span data-stu-id="fa579-142">The basic safety mechanism is enforced as follows.</span></span> <span data-ttu-id="fa579-143">Bei einer Zuweisung von einem Ausdruck E1 mit einem abgesicherten Bereich S1 zu einem (lvalue)-Ausdruck E2 mit einem Sicherheits-zu-escapebereich S2 ist ein Fehler, wenn S2 ein größerer Bereich als S1 ist.</span><span class="sxs-lookup"><span data-stu-id="fa579-143">Given an assignment from an expression E1 with a safe-to-escape scope S1, to an (lvalue) expression E2 with safe-to-escape scope S2, it is an error if S2 is a wider scope than S1.</span></span> <span data-ttu-id="fa579-144">Bei der Erstellung befinden sich die beiden Bereiche S1 und S2 in einer Schachtelungs Beziehung, da ein gültiger Ausdruck immer sicher von einem Bereich, der den Ausdruck einschließt, zurückgegeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="fa579-144">By construction, the two scopes S1 and S2 are in a nesting relationship, because a legal expression is always safe-to-return from some scope enclosing the expression.</span></span>

<span data-ttu-id="fa579-145">Die Zeit reicht für die Analyse aus, um nur zwei Bereiche zu unterstützen, die sich außerhalb der-Methode befinden, sowie den Bereich der obersten Ebene der-Methode.</span><span class="sxs-lookup"><span data-stu-id="fa579-145">For the time being it is sufficient, for the purpose of the analysis, to support just two scopes - external to the method, and top-level scope of the method.</span></span> <span data-ttu-id="fa579-146">Dies liegt daran, dass Ref-like-Werte mit inneren Bereichen nicht erstellt werden können und lokale Ref-Variablen keine erneute Zuweisung unterstützen.</span><span class="sxs-lookup"><span data-stu-id="fa579-146">That is because ref-like values with inner scopes cannot be created and ref locals do not support re-assignment.</span></span> <span data-ttu-id="fa579-147">Die Regeln können jedoch mehr als zwei Bereichs Ebenen unterstützen.</span><span class="sxs-lookup"><span data-stu-id="fa579-147">The rules, however, can support more than two scope levels.</span></span>

<span data-ttu-id="fa579-148">Die genauen Regeln zum Berechnen des Status von " *sicher an Rückgabe* " eines Ausdrucks und der Regeln für die Rechtmäßigkeit von Ausdrücken folgen.</span><span class="sxs-lookup"><span data-stu-id="fa579-148">The precise rules for computing the *safe-to-return* status of an expression, and the rules governing the legality of expressions, follow.</span></span>

### <a name="ref-safe-to-escape"></a><span data-ttu-id="fa579-149">Ref-Safe-to-Escape</span><span class="sxs-lookup"><span data-stu-id="fa579-149">ref-safe-to-escape</span></span>

<span data-ttu-id="fa579-150">Das *ref-Safe-to-Escape* -Zeichen ist ein Bereich, der einen lvalue-Ausdruck einschließt, in den es sicher ist, dass ein Verweis auf den lvalue-Wert mit Escapezeichen versehen wird.</span><span class="sxs-lookup"><span data-stu-id="fa579-150">The *ref-safe-to-escape* is a scope, enclosing an lvalue expression, to which it is safe for a ref to the lvalue to escape to.</span></span> <span data-ttu-id="fa579-151">Wenn dieser Bereich die gesamte Methode ist, sagen wir, dass ein Verweis auf den lvalue-Wert sicher von der-Methode *zurückgegeben* werden kann.</span><span class="sxs-lookup"><span data-stu-id="fa579-151">If that scope is the entire method, we say that a ref to the lvalue is *safe to return* from the method.</span></span>

### <a name="safe-to-escape"></a><span data-ttu-id="fa579-152">sichere Escapezeichen</span><span class="sxs-lookup"><span data-stu-id="fa579-152">safe-to-escape</span></span>

<span data-ttu-id="fa579-153">Der *Safe-to-Escape* -Vorgang ist ein Bereich, der einen Ausdruck einschließt, in den der Wert für den Escapezeichen sicher ist.</span><span class="sxs-lookup"><span data-stu-id="fa579-153">The *safe-to-escape* is a scope, enclosing an expression, to which it is safe for the value to escape to.</span></span> <span data-ttu-id="fa579-154">Wenn der Gültigkeitsbereich die gesamte Methode ist, sagen wir, dass der Wert sicher von der-Methode *zurückgegeben* werden kann.</span><span class="sxs-lookup"><span data-stu-id="fa579-154">If that scope is the entire method, we say that a the value is *safe to return* from the method.</span></span>

<span data-ttu-id="fa579-155">Ein Ausdruck, dessen Typ kein `ref struct` Typ ist, kann von der gesamten *einschließenden* Methode abgehandelt werden.</span><span class="sxs-lookup"><span data-stu-id="fa579-155">An expression whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span> <span data-ttu-id="fa579-156">Andernfalls verweisen wir auf die unten aufgeführten Regeln.</span><span class="sxs-lookup"><span data-stu-id="fa579-156">Otherwise we refer to the rules below.</span></span>

#### <a name="parameters"></a><span data-ttu-id="fa579-157">Parameter</span><span class="sxs-lookup"><span data-stu-id="fa579-157">Parameters</span></span>

<span data-ttu-id="fa579-158">Ein Lvalue, der einen formalen Parameter festlegt *, ist Ref-Safe-to-Escape* (als Verweis) wie folgt:</span><span class="sxs-lookup"><span data-stu-id="fa579-158">An lvalue designating a formal parameter is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="fa579-159">Wenn es sich bei dem Parameter um einen `ref`-, `out`-oder `in`-Parameter handelt, ist der Verweis von der gesamten Methode (z. b. durch eine `return ref`-Anweisung) *ref-Safe-* Escapezeichen. sonst</span><span class="sxs-lookup"><span data-stu-id="fa579-159">If the parameter is a `ref`, `out`, or `in` parameter, it is *ref-safe-to-escape* from the entire method (e.g. by a `return ref` statement); otherwise</span></span>
- <span data-ttu-id="fa579-160">Wenn es sich bei dem Parameter um den `this`-Parameter eines Strukturtyps handelt, ist dieser Verweis *sicher* in den Bereich der obersten Ebene der Methode (aber nicht in der gesamten Methode selbst). [Beispiel](#struct-this-escape)</span><span class="sxs-lookup"><span data-stu-id="fa579-160">If the parameter is the `this` parameter of a struct type, it is *ref-safe-to-escape* to the top-level scope of the method (but not from the entire method itself); [Sample](#struct-this-escape)</span></span>
- <span data-ttu-id="fa579-161">Andernfalls handelt es sich bei dem Parameter um einen value-Parameter, der *ref-Safe-to-Escape-* Vorgang mit dem Bereich der obersten Ebene der Methode (aber nicht von der Methode selbst) ist.</span><span class="sxs-lookup"><span data-stu-id="fa579-161">Otherwise the parameter is a value parameter, and it is *ref-safe-to-escape* to the top-level scope of the method (but not from the method itself).</span></span>

<span data-ttu-id="fa579-162">Ein Ausdruck, bei dem es sich um einen Rvalue-Wert handelt, der angibt, dass ein formaler Parameter verwendet wird, ist der *sichere* Escapezeichen (durch Wert) der gesamten Methode (z. b. durch eine `return`-Anweisung).</span><span class="sxs-lookup"><span data-stu-id="fa579-162">An expression that is an rvalue designating the use of a formal parameter is *safe-to-escape* (by value) from the entire method (e.g. by a `return` statement).</span></span> <span data-ttu-id="fa579-163">Dies gilt auch für den `this`-Parameter.</span><span class="sxs-lookup"><span data-stu-id="fa579-163">This applies to the `this` parameter as well.</span></span>

#### <a name="locals"></a><span data-ttu-id="fa579-164">Lokal</span><span class="sxs-lookup"><span data-stu-id="fa579-164">Locals</span></span>

<span data-ttu-id="fa579-165">Ein Lvalue, der eine lokale Variable festlegt, ist wie folgt *ref-Safe-to-Escape* (als Verweis):</span><span class="sxs-lookup"><span data-stu-id="fa579-165">An lvalue designating a local variable is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="fa579-166">Wenn die Variable eine `ref` Variable ist, wird der *ref-Safe-to-Escape* -Wert von der *ref-Safe-to-Escape* -Aktion des Initialisierungs Ausdrucks übernommen. sonst</span><span class="sxs-lookup"><span data-stu-id="fa579-166">If the variable is a `ref` variable, then its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of its initializing expression; otherwise</span></span>
- <span data-ttu-id="fa579-167">Die Variable ist ein *ref-Safe-to-Escape* -Zeichenbereich, in dem Sie deklariert wurde.</span><span class="sxs-lookup"><span data-stu-id="fa579-167">The variable is *ref-safe-to-escape* the scope in which it was declared.</span></span>

<span data-ttu-id="fa579-168">Ein Ausdruck, bei dem es sich um einen Rvalue-Wert handelt, der die Verwendung einer lokalen Variablen angibt, kann wie folgt als *sichere* Escapezeichen (durch Wert) verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="fa579-168">An expression that is an rvalue designating the use of a local variable is *safe-to-escape* (by value) as follows:</span></span>
- <span data-ttu-id="fa579-169">Die oben genannte allgemeine Regel, bei der es sich jedoch nicht um einen `ref struct` Typ handelt, ist die *sichere Rückgabe* von der gesamten einschließenden Methode.</span><span class="sxs-lookup"><span data-stu-id="fa579-169">But the general rule above, a local whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="fa579-170">Wenn die Variable eine Iterations Variable einer `foreach` Schleife ist, ist der *Safe-to-Escape* -Bereich der Variablen identisch mit dem *sicheren* Escapezeichen des Ausdrucks der `foreach` Schleife.</span><span class="sxs-lookup"><span data-stu-id="fa579-170">If the variable is an iteration variable of a `foreach` loop, then the variable's *safe-to-escape* scope is the same as the *safe-to-escape* of the `foreach` loop's expression.</span></span>
- <span data-ttu-id="fa579-171">Eine lokale `ref struct` Typs, die zum Zeitpunkt der Deklaration nicht initialisiert wurde, kann von der gesamten einschließenden Methode *sicher zurückgegeben* werden.</span><span class="sxs-lookup"><span data-stu-id="fa579-171">A local of `ref struct` type and uninitialized at the point of declaration is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="fa579-172">Andernfalls ist der Typ der Variable ein `ref struct` Typ, und die Deklaration der Variablen erfordert einen Initialisierer.</span><span class="sxs-lookup"><span data-stu-id="fa579-172">Otherwise the variable's type is a `ref struct` type, and the variable's declaration requires an initializer.</span></span> <span data-ttu-id="fa579-173">Der *Safe-to-Escape* -Bereich der Variablen ist mit dem " *Safe-to-Escape" des Initialisierers* identisch.</span><span class="sxs-lookup"><span data-stu-id="fa579-173">The variable's *safe-to-escape* scope is the same as the *safe-to-escape* of its initializer.</span></span>

#### <a name="field-reference"></a><span data-ttu-id="fa579-174">Feldverweis</span><span class="sxs-lookup"><span data-stu-id="fa579-174">Field reference</span></span>

<span data-ttu-id="fa579-175">Ein Lvalue, der einen Verweis auf ein Feld (`e.F`) festlegt, ist wie folgt *ref-Safe-to-Escape* (als Verweis):</span><span class="sxs-lookup"><span data-stu-id="fa579-175">An lvalue designating a reference to a field, `e.F`, is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="fa579-176">Wenn `e` einen Verweistyp hat, ist es *ref-Safe-to-Escape* von der gesamten Methode. sonst</span><span class="sxs-lookup"><span data-stu-id="fa579-176">If `e` is of a reference type, it is *ref-safe-to-escape* from the entire method; otherwise</span></span>
- <span data-ttu-id="fa579-177">Wenn `e` von einem Werttyp ist, wird das *ref-Safe-to-* Escape-Zeichen von der *ref-Safe-to-Escape* -`e`verwendet.</span><span class="sxs-lookup"><span data-stu-id="fa579-177">If `e` is of a value type, its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of `e`.</span></span>

<span data-ttu-id="fa579-178">Ein rvalue-Wert, der einen Verweis auf ein Feld (`e.F`) festlegt, verfügt über einen Bereich mit *sicherer* Escapezeichen, der mit dem *sicheren* Escapezeichen des `e`identisch ist.</span><span class="sxs-lookup"><span data-stu-id="fa579-178">An rvalue designating a reference to a field, `e.F`, has a *safe-to-escape* scope that is the same as the *safe-to-escape* of `e`.</span></span>

#### <a name="operators-including-"></a><span data-ttu-id="fa579-179">Operatoren einschließlich `?:`</span><span class="sxs-lookup"><span data-stu-id="fa579-179">Operators including `?:`</span></span>

<span data-ttu-id="fa579-180">Die Anwendung eines benutzerdefinierten Operators wird als Methodenaufruf behandelt.</span><span class="sxs-lookup"><span data-stu-id="fa579-180">The application of a user-defined operator is treated as a method invocation.</span></span>

<span data-ttu-id="fa579-181">Bei einem Operator, der einen Rvalue ergibt, wie z. b. `e1 + e2` oder `c ? e1 : e2`, ist das " *Safe* "-Escapezeichen für das Ergebnis der engste Bereich zwischen dem *Safe-to-Escape-* Operator der Operanden des Operators.</span><span class="sxs-lookup"><span data-stu-id="fa579-181">For an operator that yields an rvalue, such as `e1 + e2` or `c ? e1 : e2`, the *safe-to-escape* of the result is the narrowest scope among the *safe-to-escape* of the operands of the operator.</span></span>  <span data-ttu-id="fa579-182">Folglich ist für einen unären Operator, der einen Rvalue ergibt, wie z. b. `+e`, *der Safe* *-to-* Escapezeichen des-Operanden.</span><span class="sxs-lookup"><span data-stu-id="fa579-182">As a consequence, for a unary operator that yields an rvalue, such as `+e`, the *safe-to-escape* of the result is the *safe-to-escape* of the operand.</span></span>

<span data-ttu-id="fa579-183">Bei einem Operator, der einen lvalue ergibt, z. b. `c ? ref e1 : ref e2`</span><span class="sxs-lookup"><span data-stu-id="fa579-183">For an operator that yields an lvalue, such as `c ? ref e1 : ref e2`</span></span>
- <span data-ttu-id="fa579-184">der *ref-Safe-to-Escape* -Wert des Ergebnisses ist der engste Bereich zwischen der *ref-Safe-to-Escape-* Operation der Operanden des Operators.</span><span class="sxs-lookup"><span data-stu-id="fa579-184">the *ref-safe-to-escape* of the result is the narrowest scope among the *ref-safe-to-escape* of the operands of the operator.</span></span>
- <span data-ttu-id="fa579-185">der *Safe-to-Escape-* Operator muss zustimmen, und das ist der *sichere* Escapezeichen des resultierenden lvalue.</span><span class="sxs-lookup"><span data-stu-id="fa579-185">the *safe-to-escape* of the operands must agree, and that is the *safe-to-escape* of the resulting lvalue.</span></span>

#### <a name="method-invocation"></a><span data-ttu-id="fa579-186">Methodenaufruf</span><span class="sxs-lookup"><span data-stu-id="fa579-186">Method invocation</span></span>

<span data-ttu-id="fa579-187">Ein Lvalue-Wert, der sich aus einem Methodenaufruf `e1.M(e2, ...)`, der Ref zurückgibt, ist ein *ref-Safe-* Escapezeichen für den kleinsten der folgenden Bereiche:</span><span class="sxs-lookup"><span data-stu-id="fa579-187">An lvalue resulting from a ref-returning method invocation `e1.M(e2, ...)` is *ref-safe-to-escape* the smallest of the following scopes:</span></span>
- <span data-ttu-id="fa579-188">Die gesamte einschließende Methode</span><span class="sxs-lookup"><span data-stu-id="fa579-188">The entire enclosing method</span></span>
- <span data-ttu-id="fa579-189">der *ref-Safe-to-Escape-* Ausdruck aller `ref`-und `out` Argument Ausdrücke (ausgenommen des Empfängers)</span><span class="sxs-lookup"><span data-stu-id="fa579-189">the *ref-safe-to-escape* of all `ref` and `out` argument expressions (excluding the receiver)</span></span>
- <span data-ttu-id="fa579-190">Für jeden `in` Parameter der-Methode, wenn es einen entsprechenden Ausdruck mit einem lvalue-Wert gibt, der *ref-Safe-to-Escape*-Wert ist, andernfalls der nächste einschließende Bereich.</span><span class="sxs-lookup"><span data-stu-id="fa579-190">For each `in` parameter of the method, if there is a corresponding expression that is an lvalue, its *ref-safe-to-escape*, otherwise the nearest enclosing scope</span></span>
- <span data-ttu-id="fa579-191">der *Safe-to-Escape-* Ausdruck aller Argument Ausdrücke (einschließlich des Empfängers)</span><span class="sxs-lookup"><span data-stu-id="fa579-191">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

> <span data-ttu-id="fa579-192">Hinweis: das letzte Aufzählungs Zeichen ist erforderlich, um Code wie</span><span class="sxs-lookup"><span data-stu-id="fa579-192">Note: the last bullet is necessary to handle code such as</span></span>
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> <span data-ttu-id="fa579-193">oder</span><span class="sxs-lookup"><span data-stu-id="fa579-193">or</span></span>
> ```csharp
> return ref M(sp, 0);
> ```

<span data-ttu-id="fa579-194">Ein rvalue, der sich aus einem Methodenaufruf `e1.M(e2, ...)` ergibt *,* kann aus den kleinsten der folgenden Bereiche abgegrenzt werden:</span><span class="sxs-lookup"><span data-stu-id="fa579-194">An rvalue resulting from a method invocation `e1.M(e2, ...)` is *safe-to-escape* from the smallest of the following scopes:</span></span>
- <span data-ttu-id="fa579-195">Die gesamte einschließende Methode</span><span class="sxs-lookup"><span data-stu-id="fa579-195">The entire enclosing method</span></span>
- <span data-ttu-id="fa579-196">der *Safe-to-Escape-* Ausdruck aller Argument Ausdrücke (einschließlich des Empfängers)</span><span class="sxs-lookup"><span data-stu-id="fa579-196">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

#### <a name="an-rvalue"></a><span data-ttu-id="fa579-197">Ein rvalue</span><span class="sxs-lookup"><span data-stu-id="fa579-197">An Rvalue</span></span>
<span data-ttu-id="fa579-198">Ein rvalue-Wert ist vom nächsten *einschließenden Bereich Ref-Safe-to-Escape* .</span><span class="sxs-lookup"><span data-stu-id="fa579-198">An rvalue is *ref-safe-to-escape* from the nearest enclosing scope.</span></span> <span data-ttu-id="fa579-199">Dies tritt beispielsweise bei einem Aufruf auf, z. b. `M(ref d.Length)`, bei dem `d` vom Typ `dynamic`ist.</span><span class="sxs-lookup"><span data-stu-id="fa579-199">This occurs for example in an invocation such as `M(ref d.Length)` where `d` is of type `dynamic`.</span></span> <span data-ttu-id="fa579-200">Sie ist auch mit der Behandlung von Argumenten konsistent, die `in` Parametern entsprechen. \*</span><span class="sxs-lookup"><span data-stu-id="fa579-200">It is also consistent with (and perhaps subsumes) our handling of arguments corresponding to `in` parameters.\*</span></span>

#### <a name="property-invocations"></a><span data-ttu-id="fa579-201">Eigenschafts Aufrufe</span><span class="sxs-lookup"><span data-stu-id="fa579-201">Property invocations</span></span>

<span data-ttu-id="fa579-202">Ein Eigenschafts Aufruf (entweder `get` oder `set`), der mit den oben genannten Regeln als Methodenaufruf der zugrunde liegenden Methode behandelt wurde.</span><span class="sxs-lookup"><span data-stu-id="fa579-202">A property invocation (either `get` or `set`) it treated as a method invocation of the underlying method by the above rules.</span></span>

#### `stackalloc`

<span data-ttu-id="fa579-203">Bei einem stackzuweisung-Ausdruck handelt es sich um einen Rvalue-Wert, der *sicher* mit dem Bereich der obersten Ebene der Methode (aber nicht aus der gesamten Methode selbst) entfernt werden kann.</span><span class="sxs-lookup"><span data-stu-id="fa579-203">A stackalloc expression is an rvalue that is *safe-to-escape* to the top-level scope of the method (but not from the entire method itself).</span></span>

#### <a name="constructor-invocations"></a><span data-ttu-id="fa579-204">Konstruktoraufrufe</span><span class="sxs-lookup"><span data-stu-id="fa579-204">Constructor invocations</span></span>

<span data-ttu-id="fa579-205">Ein `new` Ausdruck, der einen Konstruktor aufruft, befolgt dieselben Regeln wie ein Methodenaufruf, der als Rückgabe des erstellten Typs angesehen wird.</span><span class="sxs-lookup"><span data-stu-id="fa579-205">A `new` expression that invokes a constructor obeys the same rules as a method invocation that is considered to return the type being constructed.</span></span>

<span data-ttu-id="fa579-206">Außerdem ist " *Safe-to-Escape* " nicht breiter als das kleinste der " *Safe* "-Escapezeichen aller Argumente/Operanden der objektinitialisiererausdrücke, rekursiv, wenn der Initialisierer vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="fa579-206">In addition *safe-to-escape* is no wider than the smallest of the *safe-to-escape* of all arguments/operands of the object initializer expressions, recursively, if initializer is present.</span></span> 

#### <a name="span-constructor"></a><span data-ttu-id="fa579-207">Span-Konstruktor</span><span class="sxs-lookup"><span data-stu-id="fa579-207">Span constructor</span></span>
<span data-ttu-id="fa579-208">Die Sprache basiert `Span<T>` keinen Konstruktor der folgenden Form:</span><span class="sxs-lookup"><span data-stu-id="fa579-208">The language relies on `Span<T>` not having a constructor of the following form:</span></span>

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

<span data-ttu-id="fa579-209">Ein solcher Konstruktor stellt `Span<T>` dar, die als Felder verwendet werden, die nicht von einem `ref` Feld unterschieden werden können.</span><span class="sxs-lookup"><span data-stu-id="fa579-209">Such a constructor makes `Span<T>` which are used as fields indistinguishable from a `ref` field.</span></span> <span data-ttu-id="fa579-210">Die in diesem Dokument beschriebenen Sicherheitsregeln hängen von `ref` Felder ab, die in C#oder .net nicht als gültiges Konstrukt enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="fa579-210">The safety rules described in this document depend on `ref` fields not being a valid construct in C#, or .NET.</span></span>

#### <a name="default-expressions"></a><span data-ttu-id="fa579-211">`default`-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="fa579-211">`default` expressions</span></span>

<span data-ttu-id="fa579-212">Ein `default` Ausdruck kann *von der* gesamten einschließenden Methode abgehandelt werden.</span><span class="sxs-lookup"><span data-stu-id="fa579-212">A `default` expression is *safe-to-escape* from the entire enclosing method.</span></span>

## <a name="language-constraints"></a><span data-ttu-id="fa579-213">Spracheinschränkungen</span><span class="sxs-lookup"><span data-stu-id="fa579-213">Language Constraints</span></span>

<span data-ttu-id="fa579-214">Wir möchten sicherstellen, dass keine `ref` lokale Variable und keine Variable `ref struct` Typs auf den Stapel Speicher oder die nicht mehr aktiven Variablen verweist.</span><span class="sxs-lookup"><span data-stu-id="fa579-214">We wish to ensure that no `ref` local variable, and no variable of `ref struct` type, refers to stack memory or variables that are no longer alive.</span></span> <span data-ttu-id="fa579-215">Daher haben wir die folgenden Spracheinschränkungen:</span><span class="sxs-lookup"><span data-stu-id="fa579-215">We therefore have the following language constraints:</span></span>

- <span data-ttu-id="fa579-216">Weder ein ref-Parameter noch ein lokaler ref-Parameter oder ein-Parameter oder ein lokaler eines `ref struct` Typs können in eine Lambda-oder lokale Funktion angehoben werden.</span><span class="sxs-lookup"><span data-stu-id="fa579-216">Neither a ref parameter, nor a ref local, nor a parameter or local of a `ref struct` type can be lifted into a lambda or local function.</span></span>

- <span data-ttu-id="fa579-217">Weder ein ref-Parameter noch ein Parameter eines `ref struct` Typs kann ein Argument für eine Iteratormethode oder eine `async` Methode sein.</span><span class="sxs-lookup"><span data-stu-id="fa579-217">Neither a ref parameter nor a parameter of a `ref struct` type may be an argument on an iterator method or an `async` method.</span></span>

- <span data-ttu-id="fa579-218">Weder eine lokale ref-Anweisung noch eine lokale eines `ref struct` Typs sind im Gültigkeitsbereich zum Zeitpunkt einer `yield return`-Anweisung oder eines `await`-Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="fa579-218">Neither a ref local, nor a local of a `ref struct` type may be in scope at the point of a `yield return` statement or an `await` expression.</span></span>

- <span data-ttu-id="fa579-219">Ein `ref struct` Typ darf nicht als Typargument oder als Elementtyp in einem tupeltyp verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="fa579-219">A `ref struct` type may not be used as a type argument, or as an element type in a tuple type.</span></span>

- <span data-ttu-id="fa579-220">Ein `ref struct` Typ ist möglicherweise nicht der deklarierte Typ eines Felds, mit dem Unterschied, dass es sich um den deklarierten Typ eines Instanzfelds eines anderen `ref struct`handelt.</span><span class="sxs-lookup"><span data-stu-id="fa579-220">A `ref struct` type may not be the declared type of a field, except that it may be the declared type of an instance field of another `ref struct`.</span></span>

- <span data-ttu-id="fa579-221">Ein `ref struct` Typ darf nicht der Elementtyp eines Arrays sein.</span><span class="sxs-lookup"><span data-stu-id="fa579-221">A `ref struct` type may not be the element type of an array.</span></span>

- <span data-ttu-id="fa579-222">Der Wert eines `ref struct` Typs darf nicht gekapselt werden:</span><span class="sxs-lookup"><span data-stu-id="fa579-222">A value of a `ref struct` type may not be boxed:</span></span>
  - <span data-ttu-id="fa579-223">Es gibt keine Konvertierung von einem `ref struct`-Typ in den-Typ `object` oder den-Typ `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="fa579-223">There is no conversion from a `ref struct` type to the type `object` or the type `System.ValueType`.</span></span>
  - <span data-ttu-id="fa579-224">Ein `ref struct` Typ kann nicht für die Implementierung einer Schnittstelle deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="fa579-224">A `ref struct` type may not be declared to implement any interface</span></span>
  - <span data-ttu-id="fa579-225">Keine Instanzmethode, die in `object` oder `System.ValueType` deklariert, aber nicht in einem `ref struct` Typ überschrieben wurde, kann mit einem Empfänger dieses `ref struct` Typs aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="fa579-225">No instance method declared in `object` or in `System.ValueType` but not overridden in a `ref struct` type may be called with a receiver of that `ref struct` type.</span></span>
  - <span data-ttu-id="fa579-226">Es kann keine Instanzmethode eines `ref struct` Typs durch die Methoden Konvertierung in einen Delegattyp aufgezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="fa579-226">No instance method of a `ref struct` type may be captured by method conversion to a delegate type.</span></span>

- <span data-ttu-id="fa579-227">Bei einer `ref e1 = ref e2`Ref-Neuzuweisung muss die *ref-sicher-zu-* Escapezeichen von `e2` mindestens so breit sein, wie der *ref-Safe-to-Escape* -`e1`ist.</span><span class="sxs-lookup"><span data-stu-id="fa579-227">For a ref reassignment `ref e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="fa579-228">Für eine Ref Return-Anweisung `return ref e1`muss der *ref-Safe-to-Escape* -`e1` von der gesamten Methode *ref-Safe-to-Escape* -Zeichen sein.</span><span class="sxs-lookup"><span data-stu-id="fa579-228">For a ref return statement `return ref e1`, the *ref-safe-to-escape* of `e1` must be *ref-safe-to-escape* from the entire method.</span></span> <span data-ttu-id="fa579-229">(TODO: Wir benötigen auch eine Regel, dass `e1` von der gesamten Methode *sicher oder über* flüssig sein muss?)</span><span class="sxs-lookup"><span data-stu-id="fa579-229">(TODO: Do we also need a rule that `e1` must be *safe-to-escape* from the entire method, or is that redundant?)</span></span>

- <span data-ttu-id="fa579-230">Bei einer Return *-* Anweisung `return e1`muss die `e1` mit sicherer Escapezeichen von der gesamten Methode *sicher* sein.</span><span class="sxs-lookup"><span data-stu-id="fa579-230">For a return statement `return e1`, the *safe-to-escape* of `e1` must be *safe-to-escape* from the entire method.</span></span>

- <span data-ttu-id="fa579-231">Bei einem Zuweisungs `e1 = e2`muss der Typ der `e1` ein `ref struct` Typ sein, wenn der Typ der ein Typ ist, dann muss der *Safe-to-Escape* -`e2` mindestens so breit wie der *sichere bis* Escapezeichen des `e1`sein.</span><span class="sxs-lookup"><span data-stu-id="fa579-231">For an assignment `e1 = e2`, if the type of `e1` is a `ref struct` type, then the *safe-to-escape* of `e2` must be at least as wide a scope as the *safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="fa579-232">Bei einem Methodenaufruf, bei dem ein `ref` oder `out` Argument eines `ref struct` Typs (einschließlich des Empfängers) mit " *Safe-to-Escape* E1" vorhanden ist, kann kein Argument (einschließlich des Empfängers) einen engeren *Sicherheits-zu-* Escapezeichen als E1 aufweisen.</span><span class="sxs-lookup"><span data-stu-id="fa579-232">For a method invocation if there is a `ref` or `out` argument of a `ref struct` type (including the receiver), with *safe-to-escape* E1, then no argument (including the receiver) may have a narrower *safe-to-escape* than E1.</span></span> [<span data-ttu-id="fa579-233">Beispiel</span><span class="sxs-lookup"><span data-stu-id="fa579-233">Sample</span></span>](#method-arguments-must-match)

- <span data-ttu-id="fa579-234">Eine lokale Funktion oder eine anonyme Funktion verweist möglicherweise nicht auf einen lokalen oder-Parameter `ref struct` Typs, der in einem einschließenden Bereich deklariert wurde.</span><span class="sxs-lookup"><span data-stu-id="fa579-234">A local function or anonymous function may not refer to a local or parameter of `ref struct` type declared in an enclosing scope.</span></span>

> <span data-ttu-id="fa579-235">***Problem öffnen:*** Wir benötigen eine Regel, mit der wir einen Fehler verursachen können, wenn ein Stapel Wert eines `ref struct` Typs auf einen Erwartungs Ausdruck überlaufen werden muss, z. b. im Code</span><span class="sxs-lookup"><span data-stu-id="fa579-235">***Open Issue:*** We need some rule that permits us to produce an error when needing to spill a stack value of a `ref struct` type at an await expression, for example in the code</span></span>
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a><span data-ttu-id="fa579-236">Erklären</span><span class="sxs-lookup"><span data-stu-id="fa579-236">Explanations</span></span>
<span data-ttu-id="fa579-237">In diesen Erläuterungen und Beispielen wird erläutert, warum viele der oben aufgeführten Sicherheitsregeln vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="fa579-237">These explanations and samples help explain why many of the safety rules above exist</span></span>

### <a name="method-arguments-must-match"></a><span data-ttu-id="fa579-238">Methodenargumente müssen entsprechen</span><span class="sxs-lookup"><span data-stu-id="fa579-238">Method Arguments Must Match</span></span>
<span data-ttu-id="fa579-239">Wenn eine Methode aufgerufen wird, bei der eine `out`ist, `ref` Parameter, bei dem es sich um einen `ref struct` einschließlich des Empfängers handelt, müssen alle `ref struct` dieselbe Lebensdauer haben.</span><span class="sxs-lookup"><span data-stu-id="fa579-239">When invoking a method where there is an `out`, `ref` parameter that is a `ref struct` including the receiver then all of the `ref struct` need to have the same lifetime.</span></span> <span data-ttu-id="fa579-240">Dies ist erforderlich, C# da alle Entscheidungen hinsichtlich der Lebensdauer Sicherheit auf der Grundlage der in der Signatur der Methode verfügbaren Informationen und der Lebensdauer der Werte an der aufrufssite treffen muss.</span><span class="sxs-lookup"><span data-stu-id="fa579-240">This is necessary because C# must make all of it's decisions around lifetime safety based on the information available in the signature of the method and the lifetime of the values at the call site.</span></span> 

<span data-ttu-id="fa579-241">Wenn `ref` Parameter vorhanden sind, die `ref struct` werden, gibt es die möglichen möglichen Inhalte der Inhalte.</span><span class="sxs-lookup"><span data-stu-id="fa579-241">When there are `ref` parameters that are `ref struct` then there is the possiblity they could swap around their contents.</span></span> <span data-ttu-id="fa579-242">Daher müssen wir an der aufrufssite sicherstellen, dass alle diese **möglichen** Austausch Vorgängen kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="fa579-242">Hence at the call site we must ensure all of these **potential** swaps are compatible.</span></span> <span data-ttu-id="fa579-243">Wenn die Sprache diese nicht erzwingt, wird ein fehlerhafter Code wie der folgende zugelassen.</span><span class="sxs-lookup"><span data-stu-id="fa579-243">If the language didn't enforce that then it will allow for bad code like the following.</span></span>

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

<span data-ttu-id="fa579-244">Die Einschränkung für den Empfänger ist erforderlich, da kein Inhalt von Ref-Safe-to-Escape-Vorgang bereitgestellte Werte speichern kann.</span><span class="sxs-lookup"><span data-stu-id="fa579-244">The restriction on the receiver is necessary because while none of its contents are ref-safe-to-escape it can store the provided values.</span></span> <span data-ttu-id="fa579-245">Dies bedeutet, dass Sie mit einer nicht übereinstimmenden Lebensdauer wie folgt eine typsicherheitslücke erstellen können:</span><span class="sxs-lookup"><span data-stu-id="fa579-245">This means with mismatched lifetimes you could create a type safety hole in the following way:</span></span>

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a><span data-ttu-id="fa579-246">Struct this Escape</span><span class="sxs-lookup"><span data-stu-id="fa579-246">Struct This Escape</span></span>
<span data-ttu-id="fa579-247">Bei spannen Sicherheitsregeln wird der `this` Wert in einem Instanzmember als Parameter für den Member modelliert.</span><span class="sxs-lookup"><span data-stu-id="fa579-247">When it comes to span safety rules the `this` value in an instance member is modeled as a parameter to the member.</span></span> <span data-ttu-id="fa579-248">Bei einer `struct` der `this` tatsächlich `ref S`, wo Sie in einer `class` ist Sie einfach `S` (für Mitglieder einer `class / struct` mit dem Namen s).</span><span class="sxs-lookup"><span data-stu-id="fa579-248">Now for a `struct` the type of `this` is actually `ref S` where in a `class` it's simply `S` (for members of a `class / struct` named S).</span></span> 

<span data-ttu-id="fa579-249">`this` hat jedoch andere Escaperegeln als andere `ref` Parameter.</span><span class="sxs-lookup"><span data-stu-id="fa579-249">Yet `this` has different escaping rules than other `ref` parameters.</span></span> <span data-ttu-id="fa579-250">Insbesondere ist es nicht Ref-Safe-to-Escape, während andere Parameter lauten:</span><span class="sxs-lookup"><span data-stu-id="fa579-250">Specifically it is not ref-safe-to-escape while other parameters are:</span></span>

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

<span data-ttu-id="fa579-251">Der Grund für diese Einschränkung hat nur wenig zu tun, wenn `struct` Member aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="fa579-251">The reason for this restriction actually has little to do with `struct` member invocation.</span></span> <span data-ttu-id="fa579-252">Es gibt einige Regeln, die in Bezug auf den Element Aufruf auf `struct` Membern, bei denen der Empfänger ein rvalue ist, ausgearbeitet werden müssen.</span><span class="sxs-lookup"><span data-stu-id="fa579-252">There are some rules that need to be worked out with respect to member invocation on `struct` members where the receiver is an rvalue.</span></span> <span data-ttu-id="fa579-253">Das ist jedoch sehr genehmigbar.</span><span class="sxs-lookup"><span data-stu-id="fa579-253">But that is very approachable.</span></span> 

<span data-ttu-id="fa579-254">Der Grund für diese Einschränkung ist tatsächlich der Schnittstellen Aufruf.</span><span class="sxs-lookup"><span data-stu-id="fa579-254">The reason for this restriction is actually about interface invocation.</span></span> <span data-ttu-id="fa579-255">Insbesondere wird darauf zurückgegriffen, ob das folgende Beispiel nicht kompiliert werden soll.</span><span class="sxs-lookup"><span data-stu-id="fa579-255">Specifically it comes down to whether or not the following sample should or should not compile;</span></span>

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

<span data-ttu-id="fa579-256">Beachten Sie den Fall, in dem `T` als `struct`instanziiert wird.</span><span class="sxs-lookup"><span data-stu-id="fa579-256">Consider the case where `T` is instantiated as a `struct`.</span></span> <span data-ttu-id="fa579-257">Wenn der `this`-Parameter ref-Safe-to-Escape ist, kann die Rückgabe von `p.Get` auf den Stapel zeigen (insbesondere ein Feld innerhalb des instanziierten Typs `T`).</span><span class="sxs-lookup"><span data-stu-id="fa579-257">If the `this` parameter is ref-safe-to-escape then the return of `p.Get` could point to the stack (specifically it could be a field inside of the instantiated type of `T`).</span></span> <span data-ttu-id="fa579-258">Dies bedeutet, dass die Sprache diese Stichprobe nicht kompilieren kann, da Sie eine `ref` an einen Stapel Speicherort zurückgeben könnte.</span><span class="sxs-lookup"><span data-stu-id="fa579-258">That means the language could not allow this sample to compile as it could be returning a `ref` to a stack location.</span></span> <span data-ttu-id="fa579-259">Wenn `this` hingegen nicht Ref-Safe-to-Escape ist, kann `p.Get` nicht auf den Stapel verweisen und kann daher sicher zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="fa579-259">On the other hand if `this` is not ref-safe-to-escape then `p.Get` cannot refer to the stack and hence it's safe to return.</span></span> 

<span data-ttu-id="fa579-260">Aus diesem Grund ist die Möglichkeit der `this` in einer `struct` wirklich alles über Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="fa579-260">This is why the escapability of `this` in a `struct` is really all about interfaces.</span></span> <span data-ttu-id="fa579-261">Die Anwendung kann für Sie geeignet sein, aber Sie hat einen Kompromiss.</span><span class="sxs-lookup"><span data-stu-id="fa579-261">It can absolutely be made to work but it has a trade off.</span></span> <span data-ttu-id="fa579-262">Der Entwurf wurde schließlich zugunsten von Schnittstellen flexibler.</span><span class="sxs-lookup"><span data-stu-id="fa579-262">The design eventually came down in favor of making interfaces more flexible.</span></span> 

<span data-ttu-id="fa579-263">Es besteht die Möglichkeit, dies in Zukunft zu lockern.</span><span class="sxs-lookup"><span data-stu-id="fa579-263">There is potential for us to relax this in the future though.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="fa579-264">Überlegungen für die Zukunft</span><span class="sxs-lookup"><span data-stu-id="fa579-264">Future Considerations</span></span>

### <a name="length-one-spant-over-ref-values"></a><span data-ttu-id="fa579-265">Die Länge eines Bereichs\<t > über Ref-Werte.</span><span class="sxs-lookup"><span data-stu-id="fa579-265">Length one Span\<T> over ref values</span></span>
<span data-ttu-id="fa579-266">Obwohl es heute nicht gesetzlich zulässig ist, gibt es Fälle, in denen das Erstellen einer `Span<T>` Instanz über einen Wert in Bezug auf einen Wert vorteilhaft ist</span><span class="sxs-lookup"><span data-stu-id="fa579-266">Though not legal today there are cases where creating a length one `Span<T>` instance over a value would be beneficial:</span></span>

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

<span data-ttu-id="fa579-267">Diese Funktion wird immer wichtiger, wenn wir die Einschränkungen für [Puffer mit fester Größe](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) anheben, da dies `Span<T>` Instanzen mit einer noch größeren Länge ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="fa579-267">This feature gets more compelling if we lift the restrictions on [fixed sized buffers](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) as it would allow for `Span<T>` instances of even greater length.</span></span> 

<span data-ttu-id="fa579-268">Wenn es jemals erforderlich ist, diesen Pfad zu verlassen, kann die Sprache dies ermöglichen, indem sichergestellt wird, dass `Span<T>` Instanzen nur abwärts ausgerichtet wurden.</span><span class="sxs-lookup"><span data-stu-id="fa579-268">If there is ever a need to go down this path then the language could accommodate this by ensuring such `Span<T>` instances were downward facing only.</span></span> <span data-ttu-id="fa579-269">Das heißt, dass Sie nur einmal *sicher* in den Bereich, in dem Sie erstellt wurden, wieder hergestellt werden konnten.</span><span class="sxs-lookup"><span data-stu-id="fa579-269">That is they were only ever *safe-to-escape* to the scope in which they were created.</span></span> <span data-ttu-id="fa579-270">Dadurch wird sichergestellt, dass die Sprache nie einen `ref` Wert mit dem Escapezeichen einer Methode über eine `ref struct` Rückgabe oder ein `ref struct`Feld in Erwägung zieht.</span><span class="sxs-lookup"><span data-stu-id="fa579-270">This ensure the language never had to consider a `ref` value escaping a method via a `ref struct` return or field of `ref struct`.</span></span> <span data-ttu-id="fa579-271">Dies würde wahrscheinlich auch weitere Änderungen erfordern, damit solche Konstruktoren erkannt werden, wie z. b. das Erfassen eines `ref`-Parameters.</span><span class="sxs-lookup"><span data-stu-id="fa579-271">This would likely also require further changes to recognize such constructors as capturing a `ref` parameter in this way though.</span></span>
