---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "79483937"
---
# <a name="efficient-params-and-string-formatting"></a><span data-ttu-id="d1d65-101">Effiziente Parameter und Zeichen folgen Formatierung</span><span class="sxs-lookup"><span data-stu-id="d1d65-101">Efficient Params and String Formatting</span></span>

## <a name="summary"></a><span data-ttu-id="d1d65-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="d1d65-102">Summary</span></span>
<span data-ttu-id="d1d65-103">Diese Kombination von Features erhöht die Effizienz der Formatierung `string` Werte und übergibt `params` Stil Argumente.</span><span class="sxs-lookup"><span data-stu-id="d1d65-103">This combination of features will increase the efficiency of formatting `string` values and passing of `params` style arguments.</span></span>

## <a name="motivation"></a><span data-ttu-id="d1d65-104">Motivation</span><span class="sxs-lookup"><span data-stu-id="d1d65-104">Motivation</span></span>
<span data-ttu-id="d1d65-105">Der Zuordnungs Aufwand für das Formatieren `string` Werte kann die Leistung vieler textbasierter Anwendungen dominieren: von der Boxing-Einbuße `struct` Typen, der `object[]` Zuordnung für `params` und der zwischen `string` Zuordnungen während `string.Format` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-105">The allocation overhead of formatting `string` values can dominate the performance of many text based applications: from the boxing penalty of `struct` types, the `object[]` allocation for `params` and the intermediate `string` allocations during `string.Format` calls.</span></span> <span data-ttu-id="d1d65-106">Um die Effizienz aufrechtzuerhalten, müssen solche Anwendungen häufig Produktivitäts Features wie `params` und `string` Interpolations-und Wechsel zu nicht standardmäßigen Hand codierten Lösungen verwerfen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-106">In order to maintain efficiency such applications often need to abandon productivity features such as `params` and `string` interpolation and move to non-standard, hand coded solutions.</span></span> 

<span data-ttu-id="d1d65-107">Nehmen Sie als Beispiel MSBuild an.</span><span class="sxs-lookup"><span data-stu-id="d1d65-107">Consider MSBuild as an example.</span></span> <span data-ttu-id="d1d65-108">Dies wird mit vielen modernen C# Features von Entwicklern geschrieben, die die Leistung bewusst sind.</span><span class="sxs-lookup"><span data-stu-id="d1d65-108">This is written using a lot of modern C# features by developers who are conscious of performance.</span></span> <span data-ttu-id="d1d65-109">In einem Beispiel für einen repräsentativen Build generiert MSBuild jedoch mit minimaler Ausführlichkeit eine `string` Zuordnung mit einer Größe von 262mb.</span><span class="sxs-lookup"><span data-stu-id="d1d65-109">Yet in one representative build sample MSBuild will generate 262MB of `string` allocation using minimal verbosity.</span></span> <span data-ttu-id="d1d65-110">Von dieser 1/2 der Zuordnungen sind kurzlebige Zuordnungen innerhalb `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="d1d65-110">Of that 1/2 of the allocations are short lived allocations inside `string.Format`.</span></span> <span data-ttu-id="d1d65-111">Diese Features würden einen Großteil davon auf .net-Desktop entfernen und in .net Core auf fast null senken, da die Verfügbarkeit von `Span<T>`</span><span class="sxs-lookup"><span data-stu-id="d1d65-111">These features would remove much of that on .NET Desktop and get it down to nearly zero on .NET Core due to the availability of `Span<T>`</span></span>

<span data-ttu-id="d1d65-112">Die hier beschriebenen sprach Features ermöglichen es Anwendungen, diese Features weiterhin zu verwenden, und zwar mit nur wenigen oder gar keinen Änderungen an ihrer Anwendungs Codebasis, während gleichzeitig der unbeabsichtigte Zuweisungs Aufwand in den meisten Fällen entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="d1d65-112">The set of language features described here will enable applications to continue using these features, with very little or no churn to their application code base, while removing the unintended allocation overhead in the majority of cases.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d1d65-113">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="d1d65-113">Detailed Design</span></span> 
<span data-ttu-id="d1d65-114">Es gibt eine Reihe von Features, die hier verwendet werden, um diese Ergebnisse zu erzielen:</span><span class="sxs-lookup"><span data-stu-id="d1d65-114">There are a set of features that will be used here to achieve these results:</span></span>

- <span data-ttu-id="d1d65-115">Erweitern von `params`, um einen umfassenderen Satz von Sammlungs Typen zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-115">Expanding `params` to support a broader set of collection types.</span></span>
- <span data-ttu-id="d1d65-116">Ermöglicht Entwicklern das Anpassen der Art `string` Interpolationen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-116">Allowing for developers to customize how `string` interpolation is achieved.</span></span> 
- <span data-ttu-id="d1d65-117">Ermöglicht das Binden von interpoliert `string` an effizientere `string.Format` Überladungen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-117">Allowing for interpolated `string` to bind to more efficient `string.Format` overloads.</span></span>

### <a name="extending-params"></a><span data-ttu-id="d1d65-118">Erweitern von Parametern</span><span class="sxs-lookup"><span data-stu-id="d1d65-118">Extending params</span></span>
<span data-ttu-id="d1d65-119">Die Sprache ermöglicht, dass `params` in einer Methoden Signatur die Typen `Span<T>`, `ReadOnlySpan<T>` und `IEnumerable<T>`aufweisen kann.</span><span class="sxs-lookup"><span data-stu-id="d1d65-119">The language will allow for `params` in a method signature to have the types `Span<T>`, `ReadOnlySpan<T>` and `IEnumerable<T>`.</span></span> <span data-ttu-id="d1d65-120">Die gleichen Regeln für den Aufruf gelten für die folgenden neuen Typen, die für `params T[]`gelten:</span><span class="sxs-lookup"><span data-stu-id="d1d65-120">The same rules for invocation will apply to these new types that apply to `params T[]`:</span></span>

- <span data-ttu-id="d1d65-121">Kann nicht überladen, da der einzige Unterschied ein `params`-Schlüsselwort ist</span><span class="sxs-lookup"><span data-stu-id="d1d65-121">Can't overload where the only difference is a `params` keyword.</span></span>
- <span data-ttu-id="d1d65-122">Kann aufrufen, indem eine Reihe von Argumenten übergeben wird, die implizit in `T` oder eine einzelne `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` Argument konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="d1d65-122">Can invoke by passing a series of arguments that are implicitly convertible to `T` or a single `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` argument.</span></span>
- <span data-ttu-id="d1d65-123">Muss der letzte Parameter in einer Methoden Signatur sein.</span><span class="sxs-lookup"><span data-stu-id="d1d65-123">Must be the last parameter in a method signature.</span></span>
- <span data-ttu-id="d1d65-124">Usw....</span><span class="sxs-lookup"><span data-stu-id="d1d65-124">Etc ...</span></span> 

<span data-ttu-id="d1d65-125">Die `Span<T>`-und `ReadOnlySpan<T>` Varianten werden aus Gründen der Einfachheit als `Span<T>` unten bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="d1d65-125">The `Span<T>` and `ReadOnlySpan<T>` variants will be referred to as `Span<T>` below for simplicity.</span></span> <span data-ttu-id="d1d65-126">In Fällen, in denen sich das Verhalten von `ReadOnlySpan<T>` unterscheidet, wird es explizit als bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="d1d65-126">In cases where the behavior of `ReadOnlySpan<T>` differs it will be explicitly called out.</span></span> 

<span data-ttu-id="d1d65-127">Der Vorteil der `Span<T>` Varianten von `params` ist, dass der Compiler großartige Flexibilität bei der Zuordnung des Sicherungs Speichers für den `Span<T>` Wert bietet.</span><span class="sxs-lookup"><span data-stu-id="d1d65-127">The advantage the `Span<T>` variants of `params` provides is it gives the compiler great flexibility in how it allocates the backing storage for the `Span<T>` value.</span></span> <span data-ttu-id="d1d65-128">Mit einem-`params T[]` muss der Compiler für jeden Aufruf einer `params`-Methode eine neue `T[]` zuordnen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-128">With a `params T[]` the compiler must allocate a new `T[]` for every invocation of a `params` method.</span></span> <span data-ttu-id="d1d65-129">Die Wiederverwendung ist nicht möglich, da Sie davon ausgehen muss, dass der aufgerufene den-Parameter speichert und wieder verwendet.</span><span class="sxs-lookup"><span data-stu-id="d1d65-129">Re-use is not possible because it must assume the callee stored and reused the parameter.</span></span> <span data-ttu-id="d1d65-130">Dies kann zu einer großen Ineffizienz bei Methoden mit vielen `params` aufrufen führen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-130">This can lead to a large inefficiency in methods with lots of `params` invocations.</span></span>

<span data-ttu-id="d1d65-131">Angegebene `Span<T>` Varianten sind `ref struct` der aufgerufene das Argument nicht speichern kann.</span><span class="sxs-lookup"><span data-stu-id="d1d65-131">Given `Span<T>` variants are `ref struct` the callee cannot store the argument.</span></span> <span data-ttu-id="d1d65-132">Daher kann der Compiler die CallSites optimieren, indem er Aktionen wie das erneute Verwenden des Arguments vornimmt.</span><span class="sxs-lookup"><span data-stu-id="d1d65-132">Hence the compiler can optimize the call sites by taking actions like re-using the argument.</span></span> <span data-ttu-id="d1d65-133">Dies kann dazu führen, dass wiederholte Aufrufe im Vergleich zu `T[]`sehr effizient sind.</span><span class="sxs-lookup"><span data-stu-id="d1d65-133">This can make repeated invocations very efficient as compared to `T[]`.</span></span> <span data-ttu-id="d1d65-134">In der Sprache werden jedoch keine besonderen Garantien hinsichtlich der Optimierung solcher Aufruf Sites gegeben.</span><span class="sxs-lookup"><span data-stu-id="d1d65-134">The language though will make no specific guarantees about how such callsites are optimized.</span></span> <span data-ttu-id="d1d65-135">Beachten Sie, dass der Compiler beim Aufrufen einer `params Span<T>` Methode andere Werte als `T[]` verwenden kann.</span><span class="sxs-lookup"><span data-stu-id="d1d65-135">Only note that the compiler is free to use values other than `T[]` when invoking a `params Span<T>` method.</span></span> 

<span data-ttu-id="d1d65-136">Eine mögliche Implementierung ist die folgende:</span><span class="sxs-lookup"><span data-stu-id="d1d65-136">One such potential implementation is the following.</span></span> <span data-ttu-id="d1d65-137">Beachten Sie, dass alle `params` Aufrufe in einem Methoden Text berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="d1d65-137">Consider all `params` invocation in a method body.</span></span> <span data-ttu-id="d1d65-138">Der Compiler kann ein Array zuordnen, das eine Größe gleich dem größten `params` Aufruf hat, und dieses für alle Aufrufe verwenden, indem es `Span<T>` Instanzen mit entsprechender Größe über das Array erstellt.</span><span class="sxs-lookup"><span data-stu-id="d1d65-138">The compiler could allocate an array which has a size equal to the largest `params` invocation and use that for all of the invocations by creating appropriately sized `Span<T>` instances over the array.</span></span> <span data-ttu-id="d1d65-139">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d1d65-139">For example:</span></span>

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

<span data-ttu-id="d1d65-140">Der Compiler könnte den Text der `Go` wie folgt ausgeben:</span><span class="sxs-lookup"><span data-stu-id="d1d65-140">The compiler could choose to emit the body of `Go` as follows:</span></span>

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

<span data-ttu-id="d1d65-141">Dadurch kann die Anzahl der in einer Anwendung zugewiesenen Arrays erheblich reduziert werden.</span><span class="sxs-lookup"><span data-stu-id="d1d65-141">This can significantly reduce the number of arrays allocated in an application.</span></span> <span data-ttu-id="d1d65-142">Zuweisungen können sogar noch weiter reduziert werden, wenn die Laufzeit Hilfsprogramme für eine intelligentere Stapel Zuordnung von Arrays bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="d1d65-142">Allocations can be even further reduced if the runtime provides utilities for smarter stack allocation of arrays.</span></span>

<span data-ttu-id="d1d65-143">Diese Optimierung kann jedoch nicht immer angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="d1d65-143">This optimization cannot always be applied though.</span></span> <span data-ttu-id="d1d65-144">Obwohl der aufgerufene das `params` Argument nicht erfassen kann, kann es dennoch im Aufrufer aufgezeichnet werden, wenn ein `ref` oder ein `out / ref` Parameter vorhanden ist, der selbst ein `ref struct`-Typ ist.</span><span class="sxs-lookup"><span data-stu-id="d1d65-144">Even though the callee cannot capture the `params` argument it can still be captured in the caller when there is a `ref` or a `out / ref` parameter that is itself a `ref struct` type.</span></span> 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

<span data-ttu-id="d1d65-145">Diese Fälle sind jedoch statisch erkennbar.</span><span class="sxs-lookup"><span data-stu-id="d1d65-145">These cases are statically detectable though.</span></span> <span data-ttu-id="d1d65-146">Sie tritt möglicherweise auf, wenn eine `ref` Rückgabe oder ein `ref struct` Parameter von `out` oder `ref`übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="d1d65-146">It potentially occurs whenever there is a `ref` return or a `ref struct` parameter passed by `out` or `ref`.</span></span> <span data-ttu-id="d1d65-147">In einem solchen Fall muss der Compiler für jeden Aufruf eine neue `T[]` zuordnen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-147">In such a case the compiler must allocate a fresh `T[]` for every invocation.</span></span> 

<span data-ttu-id="d1d65-148">Am Ende dieses Dokuments werden einige andere mögliche Optimierungsstrategien erläutert.</span><span class="sxs-lookup"><span data-stu-id="d1d65-148">Several other potential optimization strategies are discussed at the end of this document.</span></span>

<span data-ttu-id="d1d65-149">Der `IEnumerable<T>` Variant ist eine einfache Überladung.</span><span class="sxs-lookup"><span data-stu-id="d1d65-149">The `IEnumerable<T>` variant is a merely a convenience overload.</span></span> <span data-ttu-id="d1d65-150">Dies ist in Szenarios nützlich, in denen häufig `IEnumerable<T>` verwendet werden, aber auch viele `params` Verwendung.</span><span class="sxs-lookup"><span data-stu-id="d1d65-150">It's useful in scenarios which have frequent uses of `IEnumerable<T>` but also have lots of `params` usage.</span></span> <span data-ttu-id="d1d65-151">Wenn Sie in `T` Argument Form aufgerufen wird, wird der Sicherungs Speicher als `T[]` wie `params T[]` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="d1d65-151">When invoked in `T` argument form the backing storage will be allocated as a `T[]` just as `params T[]` is done today.</span></span>

### <a name="params-overload-resolution-changes"></a><span data-ttu-id="d1d65-152">Änderungen der Überladungs Auflösung von Parametern</span><span class="sxs-lookup"><span data-stu-id="d1d65-152">params overload resolution changes</span></span>
<span data-ttu-id="d1d65-153">Dieses Angebot bedeutet, dass die Sprache nun vier Varianten von `params` enthält, in denen Sie sich vor einem solchen Dienst befand.</span><span class="sxs-lookup"><span data-stu-id="d1d65-153">This proposal means the language now has four variants of `params` where before it had one.</span></span> <span data-ttu-id="d1d65-154">Es ist sinnvoll, dass Methoden über Ladungen von Methoden definieren, die sich nur vom Typ einer `params` Deklarationen unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="d1d65-154">It is sensible for methods to define overloads of methods that differ only on the type of a `params` declarations.</span></span> 

<span data-ttu-id="d1d65-155">Beachten Sie, dass `StringBuilder.AppendFormat` zusätzlich zum `params object[]`eine `params ReadOnlySpan<object>` Überladung hinzufügen würde.</span><span class="sxs-lookup"><span data-stu-id="d1d65-155">Consider that `StringBuilder.AppendFormat` would certainly add a `params ReadOnlySpan<object>` overload in addition to the `params object[]`.</span></span> <span data-ttu-id="d1d65-156">Dadurch kann die Leistung erheblich verbessert werden, indem Sammlungs Zuordnungen reduziert werden, ohne dass Änderungen am aufrufenden Code erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="d1d65-156">This would allow it to substantially improve performance by reducing collection allocations without requiring any changes to the calling code.</span></span> 

<span data-ttu-id="d1d65-157">Um dies zu vereinfachen, führt die Sprache die folgende Regel zur Trennung der Überladungs Auflösung ein.</span><span class="sxs-lookup"><span data-stu-id="d1d65-157">To facilitate this the language will introduce the following overload resolution tie breaking rule.</span></span> <span data-ttu-id="d1d65-158">Wenn sich die Kandidaten Methoden nur durch den `params`-Parameter unterscheiden, werden die Kandidaten in der folgenden Reihenfolge bevorzugt:</span><span class="sxs-lookup"><span data-stu-id="d1d65-158">When the candidate methods differ only by the `params` parameter then the candidates will be preferred in the following order:</span></span>

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

<span data-ttu-id="d1d65-159">Diese Reihenfolge ist der am wenigsten effiziente für den allgemeinen Fall.</span><span class="sxs-lookup"><span data-stu-id="d1d65-159">This order is the most to the least efficient for the general case.</span></span>

### <a name="variant"></a><span data-ttu-id="d1d65-160">Variant</span><span class="sxs-lookup"><span data-stu-id="d1d65-160">Variant</span></span>
<span data-ttu-id="d1d65-161">Corefx ist ein Prototyp eines neuen verwalteten Typs namens [Variant](https://github.com/dotnet/corefxlab/pull/2595).</span><span class="sxs-lookup"><span data-stu-id="d1d65-161">CoreFX is prototyping a new managed type named [Variant](https://github.com/dotnet/corefxlab/pull/2595).</span></span> <span data-ttu-id="d1d65-162">Dieser Typ ist für die Verwendung in APIs vorgesehen, die heterogene Werte erwarten, aber nicht den mehr Aufwand nutzen möchten, indem `object` als Parameter verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="d1d65-162">This type is meant to be used in APIs which expect heterogeneous values but don't want the overhead brought on by using `object` as the parameter.</span></span> <span data-ttu-id="d1d65-163">Der `Variant` Typ stellt universellen Speicher bereit, vermeidet aber die Boxing-Zuordnung für die am häufigsten verwendeten Typen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-163">The `Variant` type provides universal storage but avoids the boxing allocation for the most commonly used types.</span></span> <span data-ttu-id="d1d65-164">Durch die Verwendung dieses Typs in APIs wie `string.Format` kann der boxingaufwand in den meisten Fällen vermieden werden.</span><span class="sxs-lookup"><span data-stu-id="d1d65-164">Using this type in APIs like `string.Format` can eliminate the boxing overhead in the majority of cases.</span></span>

<span data-ttu-id="d1d65-165">Dieser Typ selbst ist nicht notwendigerweise speziell für die Sprache.</span><span class="sxs-lookup"><span data-stu-id="d1d65-165">This type itself is not necessarily special to the language.</span></span> <span data-ttu-id="d1d65-166">Sie wird in diesem Dokument separat in dieses Dokument eingefügt, da es zu einem Implementierungsdetail anderer Teile des Angebots wird.</span><span class="sxs-lookup"><span data-stu-id="d1d65-166">It is being introduced in this document separately though as it becomes an implementation detail of other parts of the proposal.</span></span> 

### <a name="efficient-interpolated-strings"></a><span data-ttu-id="d1d65-167">Effiziente interinterpolierte Zeichen folgen</span><span class="sxs-lookup"><span data-stu-id="d1d65-167">Efficient interpolated strings</span></span>
<span data-ttu-id="d1d65-168">Interinterpolierte Zeichen folgen sind eine beliebte, aber C#ineffiziente Funktion in.</span><span class="sxs-lookup"><span data-stu-id="d1d65-168">Interpolated strings are a popular yet inefficient feature in C#.</span></span> <span data-ttu-id="d1d65-169">Die häufigste Syntax, die eine interinterpolierte `string` als `string`verwendet, wird in einen `string.Format(string, params object[])`-Befehl übersetzt.</span><span class="sxs-lookup"><span data-stu-id="d1d65-169">The most common syntax, using an interpolated `string` as a `string`, translates into a `string.Format(string, params object[])` call.</span></span> <span data-ttu-id="d1d65-170">Dies verursacht boxingzuordnungen für alle Werttypen, zwischen `string` Zuordnungen, da die Implementierung größtenteils `object.ToString` für die Formatierung und die Zuordnung von Arrays verwendet, wenn die Anzahl der Argumente die Anzahl der Parameter für die "schnellen" über Ladungen von `string.Format`überschreitet.</span><span class="sxs-lookup"><span data-stu-id="d1d65-170">That will incur boxing allocations for all value types, intermediate `string` allocations as the implementation largely uses `object.ToString` for formatting as well as array allocations once the number of arguments exceeds the amount of parameters on the "fast" overloads of `string.Format`.</span></span> 

<span data-ttu-id="d1d65-171">Die Sprache ändert die Interpolations Herabsetzung, um alternative über Ladungen von `string.Format`in Erwägung zu gezogen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-171">The language will change its interpolation lowering to consider alternate overloads of `string.Format`.</span></span> <span data-ttu-id="d1d65-172">Dabei werden alle Formen von `string.Format(string, params)` berücksichtigt und die "beste" Überladung ausgewählt, die den Argument Typen entspricht.</span><span class="sxs-lookup"><span data-stu-id="d1d65-172">It will consider all forms of `string.Format(string, params)` and pick the "best" overload which satisfies the argument types.</span></span>
<span data-ttu-id="d1d65-173">Die "beste" `params` Überladung wird durch die oben beschriebenen Regeln bestimmt.</span><span class="sxs-lookup"><span data-stu-id="d1d65-173">The "best" `params` overload will be determined by the rules discussed above.</span></span> <span data-ttu-id="d1d65-174">Dies bedeutet, dass interpoliert `string` jetzt an sehr effiziente über Ladungen wie `string.Format(string format, params ReadOnlySpan<Variant> args)`gebunden werden kann.</span><span class="sxs-lookup"><span data-stu-id="d1d65-174">This means interpolated `string` can now bind to very efficient overloads like `string.Format(string format, params ReadOnlySpan<Variant> args)`.</span></span> <span data-ttu-id="d1d65-175">In vielen Fällen werden dadurch alle zwischen Zuordnungen entfernt.</span><span class="sxs-lookup"><span data-stu-id="d1d65-175">In many cases this will remove all intermediate allocations.</span></span>

### <a name="customizable-interpolated-strings"></a><span data-ttu-id="d1d65-176">Anpassbare interinterpolierte Zeichen folgen</span><span class="sxs-lookup"><span data-stu-id="d1d65-176">Customizable interpolated strings</span></span>
<span data-ttu-id="d1d65-177">Entwickler können das Verhalten interpoliert Zeichen folgen mit `FormattableString`anpassen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-177">Developers are able to customize the behavior of interpolated strings with `FormattableString`.</span></span> <span data-ttu-id="d1d65-178">Diese Datei enthält die Daten, die in eine interinterpolierte Zeichenfolge fließen: das Format `string` und die Argumente als Array.</span><span class="sxs-lookup"><span data-stu-id="d1d65-178">This contains the data which goes into an interpolated string: the format `string` and the arguments as an array.</span></span> <span data-ttu-id="d1d65-179">Dies hat jedoch immer noch die Zuordnung von Boxing-und Argument Arrays sowie die Zuordnung für `FormattableString` (`abstract class`).</span><span class="sxs-lookup"><span data-stu-id="d1d65-179">This though still has the boxing and argument array allocation as well as the allocation for `FormattableString` (it's an `abstract class`).</span></span> <span data-ttu-id="d1d65-180">Daher ist es von geringem Verwendungs Aufwand für Anwendungen, die die `string` Formatierung stark belegen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-180">Hence it's of little use to applications which are allocation heavy in `string` formatting.</span></span>

<span data-ttu-id="d1d65-181">Um das Formatieren von interpoliert Zeichen folgen effizient zu gestalten, erkennt die Sprache einen neuen Typ: `System.ValueFormattableString`.</span><span class="sxs-lookup"><span data-stu-id="d1d65-181">To make interpolated string formatting efficient the language will recognize a new type: `System.ValueFormattableString`.</span></span> <span data-ttu-id="d1d65-182">Alle interpoliert-Zeichen folgen weisen eine Zieltyp Konvertierung in diesen Typ auf.</span><span class="sxs-lookup"><span data-stu-id="d1d65-182">All interpolated strings will have a target type conversion to this type.</span></span> <span data-ttu-id="d1d65-183">Dies wird implementiert, indem die interinterpolierte Zeichenfolge in den-Rückruf übersetzt wird `ValueFormattableString.Create` genau wie für `FormattableString.Create` heute.</span><span class="sxs-lookup"><span data-stu-id="d1d65-183">This will be implemented by translating the interpolated string into the call `ValueFormattableString.Create` exactly as is done for `FormattableString.Create` today.</span></span> <span data-ttu-id="d1d65-184">Die Sprache unterstützt alle `params` Optionen, die in diesem Dokument beschrieben werden, wenn Sie nach der am besten geeigneten `ValueFormattableString.Create` Methode suchen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-184">The language will support all `params` options described in this document when looking for the most suitable `ValueFormattableString.Create` method.</span></span> 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

<span data-ttu-id="d1d65-185">Regeln zur Überladungs Auflösung werden geändert, um `ValueFormattableString` über `string` vorzuziehen, wenn das Argument eine interpoliert Zeichenfolge ist.</span><span class="sxs-lookup"><span data-stu-id="d1d65-185">Overload resolution rules will be changed to prefer `ValueFormattableString` over `string` when the argument is an interpolated string.</span></span> <span data-ttu-id="d1d65-186">Dies bedeutet, dass es für über Ladungen nützlich sein wird, die sich nur bei `string` und `ValueFormattableString`unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="d1d65-186">This means it will be valuable to have overloads which differ only on `string` and `ValueFormattableString`.</span></span> <span data-ttu-id="d1d65-187">Eine solche Überladung ist heute mit `FormattableString` nicht wertvoll, da der Compiler immer die `string` Version bevorzugen wird (es sei denn, der Entwickler verwendet eine explizite Umwandlung).</span><span class="sxs-lookup"><span data-stu-id="d1d65-187">Such an overload today with `FormattableString` is not valuable as the compiler will always prefer the `string` version (unless the developer uses an explicit cast).</span></span> 

## <a name="open-issues"></a><span data-ttu-id="d1d65-188">Offene Probleme</span><span class="sxs-lookup"><span data-stu-id="d1d65-188">Open Issues</span></span>

### <a name="valueformattablestring-breaking-change"></a><span data-ttu-id="d1d65-189">Valueformattablestring-Breaking Change</span><span class="sxs-lookup"><span data-stu-id="d1d65-189">ValueFormattableString breaking change</span></span>
<span data-ttu-id="d1d65-190">Die Änderung, die bei der Überladungs Auflösung bei `string` `ValueFormattableString` bevorzugen, ist eine Breaking Change.</span><span class="sxs-lookup"><span data-stu-id="d1d65-190">The change to prefer `ValueFormattableString` during overload resolution over `string` is a breaking change.</span></span> <span data-ttu-id="d1d65-191">Es ist möglich, dass ein Entwickler heute einen Typ namens "`ValueFormattableString`" definiert und ihn in Methoden Überladungen mit `string`verwendet.</span><span class="sxs-lookup"><span data-stu-id="d1d65-191">It is possible for a developer to have defined a type called `ValueFormattableString` today and use it in method overloads with `string`.</span></span> <span data-ttu-id="d1d65-192">Diese vorgeschlagene Änderung würde bewirken, dass der Compiler eine andere Überladung ausgewählt hat, nachdem dieser Satz von Features implementiert wurde.</span><span class="sxs-lookup"><span data-stu-id="d1d65-192">This proposed change would cause the compiler to pick a different overload once this set of features was implemented.</span></span> 

<span data-ttu-id="d1d65-193">Die Möglichkeit, dies zu tun, ist relativ gering.</span><span class="sxs-lookup"><span data-stu-id="d1d65-193">The possibility of this seems reasonably low.</span></span> <span data-ttu-id="d1d65-194">Der-Typ benötigt den vollständigen Namen `System.ValueFormattableString` und muss `static` Methoden mit dem Namen `Create`haben.</span><span class="sxs-lookup"><span data-stu-id="d1d65-194">The type would need the full name `System.ValueFormattableString` and it would need to have `static` methods named `Create`.</span></span> <span data-ttu-id="d1d65-195">Da Entwickler dringend davon ausgehen, dass Sie keinen Typ im `System` Namespace definieren, scheint diese Unterbrechung eine sinnvolle Gefährdung zu sein.</span><span class="sxs-lookup"><span data-stu-id="d1d65-195">Given that developers are strongly discouraged from defining any type in the `System` namespace this break seems like a reasonable compromise.</span></span>

### <a name="expanding-to-more-types"></a><span data-ttu-id="d1d65-196">Erweitern auf weitere Typen</span><span class="sxs-lookup"><span data-stu-id="d1d65-196">Expanding to more types</span></span>
<span data-ttu-id="d1d65-197">Da wir uns in diesem Bereich befinden, sollten wir `IList<T>`, `ICollection<T>` und `IReadOnlyList<T>` der Sammlung von Sammlungen hinzufügen, für die `params` unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="d1d65-197">Given we're in the area we should consider adding `IList<T>`, `ICollection<T>` and `IReadOnlyList<T>` to the set of collections for which `params` is supported.</span></span> <span data-ttu-id="d1d65-198">Im Hinblick auf die Implementierung kostet die andere Arbeit hier eine kleine Menge.</span><span class="sxs-lookup"><span data-stu-id="d1d65-198">In terms of implementation it will cost a small amount over the other work here.</span></span>

<span data-ttu-id="d1d65-199">LDM muss entscheiden, ob die Komplikation für die Sprache den Wert hat.</span><span class="sxs-lookup"><span data-stu-id="d1d65-199">LDM needs to decide if the complication to the language is worth it though.</span></span> <span data-ttu-id="d1d65-200">Durch das Hinzufügen von `IEnumerable<T>` wird ein sehr spezifischer Reibungspunkt entfernt.</span><span class="sxs-lookup"><span data-stu-id="d1d65-200">The addition of `IEnumerable<T>` removes a very specific friction point.</span></span> <span data-ttu-id="d1d65-201">Wenn diese `params` Lösung fehlt, waren viele Kunden gezwungen, `T[]` aus einem `IEnumerable<T>` zuzuweisen, wenn eine `params`-Methode aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="d1d65-201">Lacking this `params` solution many customers were forced to allocate `T[]` from an `IEnumerable<T>` when calling a `params` method.</span></span> <span data-ttu-id="d1d65-202">Durch das Hinzufügen von `IEnumerable<T>` wird dies behoben.</span><span class="sxs-lookup"><span data-stu-id="d1d65-202">The addition of `IEnumerable<T>` fixes this though.</span></span> <span data-ttu-id="d1d65-203">Es gibt keinen bestimmten Reibungspunkt, der von den anderen Schnittstellen behoben wird.</span><span class="sxs-lookup"><span data-stu-id="d1d65-203">There is no specific friction point that the other interfaces fix here.</span></span> 

## <a name="considerations"></a><span data-ttu-id="d1d65-204">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="d1d65-204">Considerations</span></span>

### <a name="variant2-and-variant3"></a><span data-ttu-id="d1d65-205">Variant2 und Variant3</span><span class="sxs-lookup"><span data-stu-id="d1d65-205">Variant2 and Variant3</span></span>
<span data-ttu-id="d1d65-206">Das corefx-Team verfügt auch über einen nicht Zuweisungs Satz von Speichertypen für bis zu drei `Variant` Argumente.</span><span class="sxs-lookup"><span data-stu-id="d1d65-206">The CoreFX team also has a non-allocating set of storage types for up to three `Variant` arguments.</span></span> <span data-ttu-id="d1d65-207">Dabei handelt es sich um eine einzelne `Variant`, `Variant2` und `Variant3`.</span><span class="sxs-lookup"><span data-stu-id="d1d65-207">These are a single `Variant`, `Variant2` and `Variant3`.</span></span> <span data-ttu-id="d1d65-208">Alle verfügen über ein paar Methoden, um eine Zuordnung frei `Span<Variant>` von Ihnen zu erhalten: `CreateSpan` und `KeepAlive`.</span><span class="sxs-lookup"><span data-stu-id="d1d65-208">All have a pair of methods for getting an allocation free `Span<Variant>` off of them: `CreateSpan` and `KeepAlive`.</span></span> <span data-ttu-id="d1d65-209">Dies bedeutet, dass für eine `params Span<Variant>` von bis zu drei Argumenten die Website für die Website vollständig freigegeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="d1d65-209">This means for a `params Span<Variant>` of up to three arguments the call site can be entirely allocation free.</span></span>

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

<span data-ttu-id="d1d65-210">Die `Go`-Methode kann auf Folgendes herabgesetzt werden:</span><span class="sxs-lookup"><span data-stu-id="d1d65-210">The `Go` method can be lowered to the following:</span></span>

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

<span data-ttu-id="d1d65-211">Dies erfordert sehr wenig Aufwand, um `T[]` zwischen `params Span<T>` aufrufen wiederzuverwenden.</span><span class="sxs-lookup"><span data-stu-id="d1d65-211">This requires very little work on top of the proposal to re-use `T[]` between `params Span<T>` calls.</span></span> <span data-ttu-id="d1d65-212">Der Compiler muss bereits einen temporären pro-Rückruf verwalten und eine Bereinigungs Arbeit nach ausführen (auch wenn er in einem Fall nur eine interne Temp als frei markiert).</span><span class="sxs-lookup"><span data-stu-id="d1d65-212">The compiler already needs to manage a temporary per call and do clean up work after (even if in one case it's just marking an internal temp as free).</span></span> 

<span data-ttu-id="d1d65-213">Hinweis: die `KeepAlive`-Funktion ist nur auf dem Desktop erforderlich.</span><span class="sxs-lookup"><span data-stu-id="d1d65-213">Note: the `KeepAlive` function is only necessary on desktop.</span></span> <span data-ttu-id="d1d65-214">In .net Core ist die Methode nicht verfügbar, daher gibt der Compiler keinen aufzurufenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="d1d65-214">On .NET Core the method will not be available and hence the compiler won't emit a call to it.</span></span>

### <a name="clr-stack-allocation-helpers"></a><span data-ttu-id="d1d65-215">Hilfe zur CLR-Stapel Zuordnung</span><span class="sxs-lookup"><span data-stu-id="d1d65-215">CLR stack allocation helpers</span></span>
<span data-ttu-id="d1d65-216">Die CLR bietet nur [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) für die Stapel Zuordnung von zusammenhängenden Arbeitsspeicher.</span><span class="sxs-lookup"><span data-stu-id="d1d65-216">The CLR only provides only [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) for stack allocation of contiguous memory.</span></span> <span data-ttu-id="d1d65-217">Diese Anweisung ist darauf beschränkt, dass Sie nur für `unmanaged` Typen funktioniert.</span><span class="sxs-lookup"><span data-stu-id="d1d65-217">This instruction is limited in that it only works for `unmanaged` types.</span></span> <span data-ttu-id="d1d65-218">Dies bedeutet, dass Sie nicht als universelle Lösung für die effiziente Zuordnung des Sicherungs Speichers für `params 
 Span<T>`verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="d1d65-218">This means it can't be used as a universal solution for efficiently allocating the backing storage for `params 
 Span<T>`.</span></span> 

<span data-ttu-id="d1d65-219">Diese Einschränkung ist jedoch nicht eine grundlegende Einschränkung, sondern vielmehr ein Element des Verlaufs.</span><span class="sxs-lookup"><span data-stu-id="d1d65-219">This limitation is not some fundamental restriction though but instead more an artifact of history.</span></span> <span data-ttu-id="d1d65-220">Die CLR könnte neue op-Codes/intrinsie-Funktionen hinzufügen, die eine universelle Stapel Zuordnung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-220">The CLR could choose to add new op codes / intrinsics which provide universal stack allocation.</span></span> <span data-ttu-id="d1d65-221">Diese können dann verwendet werden, um den Sicherungs Speicher für die meisten `params Span<T>` Aufrufe zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-221">These could then be used to allocate the backing storage for most `params Span<T>` calls.</span></span>

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

<span data-ttu-id="d1d65-222">Die `Go`-Methode kann auf Folgendes herabgesetzt werden:</span><span class="sxs-lookup"><span data-stu-id="d1d65-222">The `Go` method can be lowered to the following:</span></span>

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

<span data-ttu-id="d1d65-223">Obwohl dieser Ansatz sehr Heap effizient ist, führt dies zu einer zusätzlichen Stapel Auslastung.</span><span class="sxs-lookup"><span data-stu-id="d1d65-223">While this approach is very heap efficient it does cause extra stack usage.</span></span> <span data-ttu-id="d1d65-224">In einem Algorithmus, der über einen tiefen Stapel und viele `params` Verwendung verfügt, kann dies dazu führen, dass eine `StackOverflowException` generiert wird, wenn eine einfache `T[]` Zuordnung erfolgreich ist.</span><span class="sxs-lookup"><span data-stu-id="d1d65-224">In an algorithm which has a deep stack and lots of `params` usage it's possible this could cause a `StackOverflowException` to be generated where a simple `T[]` allocation would succeed.</span></span> 

<span data-ttu-id="d1d65-225">Leider C# ist nicht für den Typ der Analyse übergreifende Analyse eingerichtet, bei der es eine fundierte Entscheidung treffen könnte, ob die Stapel-oder Heap Zuordnung von `params`verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="d1d65-225">Unfortunately C# is not set up for the type of inter-method analysis where it could make an educated determination of whether or not call should use stack or heap allocation of `params`.</span></span> <span data-ttu-id="d1d65-226">Die einzelnen Aufrufe können nur für Sie selbst in Erwägung gezogen werden.</span><span class="sxs-lookup"><span data-stu-id="d1d65-226">It can only really consider each call on its own.</span></span>

<span data-ttu-id="d1d65-227">Die CLR ist die beste Einrichtung, um diese Art von Bestimmung zur Laufzeit zu treffen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-227">The CLR is best setup for making this type of determination at runtime.</span></span> <span data-ttu-id="d1d65-228">Daher ist es wahrscheinlich, dass die Laufzeit zwei Methoden für die universelle Stapel Zuweisung bereitstellt:</span><span class="sxs-lookup"><span data-stu-id="d1d65-228">Hence we'd likely have the runtime provide two methods for universal stack allocation:</span></span>

1. <span data-ttu-id="d1d65-229">`Span<T> StackAlloc<T>(int length)`: Dies hat das gleiche Verhalten und die gleichen Einschränkungen wie `localloc`, außer dass es für beliebige Typen `T`funktionieren kann.</span><span class="sxs-lookup"><span data-stu-id="d1d65-229">`Span<T> StackAlloc<T>(int length)`: this has the same behaviors and limitations of `localloc` except it can work on any type `T`.</span></span> 
1. <span data-ttu-id="d1d65-230">`Span<T> MaybeStackAlloc<T>(int length)`: Diese Laufzeit kann diese Option implementieren, indem Sie eine Stapel-oder Heap Zuordnung durchführen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-230">`Span<T> MaybeStackAlloc<T>(int length)`: this runtime can choose to implement this by doing a stack or heap allocation.</span></span> <span data-ttu-id="d1d65-231">Die Laufzeit kann dann den Ausführungs Kontext verwenden, in dem Sie aufgerufen wird, um zu bestimmen, wie die `Span<T>` zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="d1d65-231">The runtime can then use the execution context in which it's called to determine how the `Span<T>` is allocated.</span></span> <span data-ttu-id="d1d65-232">Der Aufrufer behandelt ihn jedoch immer so, als ob er Stapel zugeordnet wäre.</span><span class="sxs-lookup"><span data-stu-id="d1d65-232">The caller though will always treat it as if it were stack allocated.</span></span>

<span data-ttu-id="d1d65-233">Für sehr einfache Fälle, wie z. b. ein bis C# zwei Argumente, könnte der Compiler immer `StackAlloc<T>` Variant verwenden.</span><span class="sxs-lookup"><span data-stu-id="d1d65-233">For very simple cases, like one to two arguments, the C# compiler could always use `StackAlloc<T>` variant.</span></span> <span data-ttu-id="d1d65-234">In den meisten Fällen ist es unwahrscheinlich, dass die Stapel Auslastung maßgeblich ist.</span><span class="sxs-lookup"><span data-stu-id="d1d65-234">This is unlikely to significantly contribute to stack exhaustion in most cases.</span></span> <span data-ttu-id="d1d65-235">In anderen Fällen könnte der Compiler entscheiden, `MaybeStackAlloc<T>` stattdessen zu verwenden und die Laufzeit den-Befehl zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-235">For other cases the compiler could choose to use `MaybeStackAlloc<T>` instead and let the runtime make the call.</span></span>

<span data-ttu-id="d1d65-236">Wie wir uns entscheiden, erfordert wahrscheinlich eine tiefere Untersuchung und Untersuchung von realen apps.</span><span class="sxs-lookup"><span data-stu-id="d1d65-236">How we choose will likely require a deeper investigation and examination of real world apps.</span></span> <span data-ttu-id="d1d65-237">Wenn diese neuen systeminternen Funktionen verfügbar sind, erhalten wir diese Art von Flexibilität.</span><span class="sxs-lookup"><span data-stu-id="d1d65-237">But if these new intrinsics are available then it will give us this type of flexibility.</span></span>

### <a name="why-not-varargs"></a><span data-ttu-id="d1d65-238">Warum nicht varargs?</span><span class="sxs-lookup"><span data-stu-id="d1d65-238">Why not varargs?</span></span> 
<span data-ttu-id="d1d65-239">Die vorhandene [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) -Funktion wurde hier als mögliche Lösung betrachtet.</span><span class="sxs-lookup"><span data-stu-id="d1d65-239">The existing [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) feature was considered here as a possible solution.</span></span> <span data-ttu-id="d1d65-240">Diese Funktion ist jedoch in erster Linie C++für/CLI-Szenarien gedacht und verfügt über bekannte Lücken für andere Szenarien.</span><span class="sxs-lookup"><span data-stu-id="d1d65-240">This feature though is meant primarily for C++/CLI scenarios and has known holes for other scenarios.</span></span> <span data-ttu-id="d1d65-241">Außerdem entstehen beträchtliche Kosten bei der Portierung auf UNIX.</span><span class="sxs-lookup"><span data-stu-id="d1d65-241">Additionally there is significant cost in porting this to Unix.</span></span> <span data-ttu-id="d1d65-242">Daher wurde sie nicht als eine funktionierende Lösung angesehen.</span><span class="sxs-lookup"><span data-stu-id="d1d65-242">Hence it wasn't seen as a viable solution.</span></span>

## <a name="related-issues"></a><span data-ttu-id="d1d65-243">Verwandte Probleme</span><span class="sxs-lookup"><span data-stu-id="d1d65-243">Related Issues</span></span>
<span data-ttu-id="d1d65-244">Diese Spezifikation bezieht sich auf die folgenden Probleme:</span><span class="sxs-lookup"><span data-stu-id="d1d65-244">This spec is related to the following issues:</span></span> 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

