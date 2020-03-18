---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "79484021"
---
# <a name="readonly-references"></a><span data-ttu-id="38223-101">Schreibgeschützte Verweise</span><span class="sxs-lookup"><span data-stu-id="38223-101">Readonly references</span></span>

* <span data-ttu-id="38223-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="38223-102">[x] Proposed</span></span>
* <span data-ttu-id="38223-103">[x] Prototyp</span><span class="sxs-lookup"><span data-stu-id="38223-103">[x] Prototype</span></span>
* <span data-ttu-id="38223-104">[x] Implementierung: gestartet</span><span class="sxs-lookup"><span data-stu-id="38223-104">[x] Implementation: Started</span></span>
* <span data-ttu-id="38223-105">[] Spezifikation: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="38223-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="38223-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="38223-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="38223-107">Die Funktion "schreibgeschützte Verweise" ist tatsächlich eine Gruppe von Features, die die Effizienz der Weitergabe von Variablen als Verweis nutzen, ohne die Daten für Änderungen verfügbar zu machen:</span><span class="sxs-lookup"><span data-stu-id="38223-107">The "readonly references" feature is actually a group of features that leverage the efficiency of passing variables by reference, but without exposing the data to modifications:</span></span>  
- <span data-ttu-id="38223-108">`in` Parameter</span><span class="sxs-lookup"><span data-stu-id="38223-108">`in` parameters</span></span>
- <span data-ttu-id="38223-109">`ref readonly`-Rückgaben</span><span class="sxs-lookup"><span data-stu-id="38223-109">`ref readonly` returns</span></span>
- <span data-ttu-id="38223-110">`readonly` Strukturen</span><span class="sxs-lookup"><span data-stu-id="38223-110">`readonly` structs</span></span>
- <span data-ttu-id="38223-111">`ref`/`in`-Erweiterungs Methoden</span><span class="sxs-lookup"><span data-stu-id="38223-111">`ref`/`in` extension methods</span></span>
- <span data-ttu-id="38223-112">lokale `ref readonly`</span><span class="sxs-lookup"><span data-stu-id="38223-112">`ref readonly` locals</span></span>
- <span data-ttu-id="38223-113">bedingte Ausdrücke `ref`</span><span class="sxs-lookup"><span data-stu-id="38223-113">`ref` conditional expressions</span></span>

## <a name="passing-arguments-as-readonly-references"></a><span data-ttu-id="38223-114">Übergeben von Argumenten als schreibgeschützte Verweise.</span><span class="sxs-lookup"><span data-stu-id="38223-114">Passing arguments as readonly references.</span></span>

<span data-ttu-id="38223-115">Es gibt ein vorhandenes Angebot, das dieses Thema https://github.com/dotnet/roslyn/issues/115 als Sonderfall von schreibgeschützten Parametern, ohne in viele Details zu gehen.</span><span class="sxs-lookup"><span data-stu-id="38223-115">There is an existing proposal that touches this topic https://github.com/dotnet/roslyn/issues/115 as a special case of readonly parameters without going into many details.</span></span>
<span data-ttu-id="38223-116">An dieser Stelle möchte ich nur bestätigen, dass die Idee allein nicht ganz neu ist.</span><span class="sxs-lookup"><span data-stu-id="38223-116">Here I just want to acknowledge that the idea by itself is not very new.</span></span>

### <a name="motivation"></a><span data-ttu-id="38223-117">Motivation</span><span class="sxs-lookup"><span data-stu-id="38223-117">Motivation</span></span>

<span data-ttu-id="38223-118">Vor diesem Feature C# verfügte nicht über eine effiziente Möglichkeit, Struktur Variablen in Methodenaufrufe für schreibgeschützte Zwecke zu übergeben, ohne zu ändern.</span><span class="sxs-lookup"><span data-stu-id="38223-118">Prior to this feature C# did not have an efficient way of expressing a desire to pass struct variables into method calls for readonly purposes with no intention of modifying.</span></span> <span data-ttu-id="38223-119">Die reguläre durch-Wert-Argument Übergabe impliziert das Kopieren, wodurch unnötige Kosten entstehen.</span><span class="sxs-lookup"><span data-stu-id="38223-119">Regular by-value argument passing implies copying, which adds unnecessary costs.</span></span>  <span data-ttu-id="38223-120">Dadurch werden Benutzer zur Verwendung der by-ref-Argument Übergabe und der Verwendung von Kommentaren/Dokumentationen aufgefordert, um anzugeben, dass die Daten nicht vom aufgerufenen mutiert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="38223-120">That drives users to use by-ref argument passing and rely on comments/documentation to indicate that the data is not supposed to be mutated by the callee.</span></span> <span data-ttu-id="38223-121">Es ist aus vielen Gründen keine gute Lösung.</span><span class="sxs-lookup"><span data-stu-id="38223-121">It is not a good solution for many reasons.</span></span>  
<span data-ttu-id="38223-122">Bei den Beispielen handelt es sich um zahlreiche Vektor-/matrixoperatoren in Grafik Bibliotheken, wie [XNA](https://msdn.microsoft.com/library/bb194944.aspx) bekanntermaßen nur aufgrund von Leistungs Überlegungen zu Referenz Operanden gehören.</span><span class="sxs-lookup"><span data-stu-id="38223-122">The examples are numerous - vector/matrix math operators in graphics libraries like [XNA](https://msdn.microsoft.com/library/bb194944.aspx) are known to have ref operands purely because of performance considerations.</span></span> <span data-ttu-id="38223-123">Der Roslyn-Compiler selbst enthält Code, der Strukturen verwendet, um Zuordnungen zu vermeiden, und Sie dann als Verweis übergibt, um das Kopieren von Kosten zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="38223-123">There is code in Roslyn compiler itself that uses structs to avoid allocations and then passes them by reference to avoid copying costs.</span></span>

### <a name="solution-in-parameters"></a><span data-ttu-id="38223-124">Lösung (`in` Parameter)</span><span class="sxs-lookup"><span data-stu-id="38223-124">Solution (`in` parameters)</span></span>

<span data-ttu-id="38223-125">Ebenso wie die `out` Parameter werden `in` Parameter als verwaltete Verweise mit zusätzlichen Garantien vom aufgerufenen übergeben.</span><span class="sxs-lookup"><span data-stu-id="38223-125">Similarly to the `out` parameters, `in` parameters are passed as managed references with additional guarantees from the callee.</span></span>  
<span data-ttu-id="38223-126">Im Gegensatz zu `out`-Parametern, die vom aufgerufenen vor einer anderen Verwendung zugewiesen werden _müssen_ , können `in` Parameter überhaupt nicht vom aufgerufenen zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="38223-126">Unlike `out` parameters which _must_ be assigned by the callee before any other use, `in` parameters cannot be assigned by the callee at all.</span></span>

<span data-ttu-id="38223-127">Daher können `in` Parameter die Effektivität der indirekten Argument Übergabe ermöglichen, ohne Argumente für Mutationen durch den aufgerufenen verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="38223-127">As a result `in` parameters allow for effectiveness of indirect argument passing without exposing arguments to mutations by the callee.</span></span>

### <a name="declaring-in-parameters"></a><span data-ttu-id="38223-128">Deklarieren eines `in`-Parameters</span><span class="sxs-lookup"><span data-stu-id="38223-128">Declaring `in` parameters</span></span>

<span data-ttu-id="38223-129">`in` Parameter werden mithilfe `in`-Schlüssel Worts als Modifizierer in der Parameter Signatur deklariert.</span><span class="sxs-lookup"><span data-stu-id="38223-129">`in` parameters are declared by using `in` keyword as a modifier in the parameter signature.</span></span>

<span data-ttu-id="38223-130">Für alle Zwecke wird der `in` Parameter als `readonly` Variable behandelt.</span><span class="sxs-lookup"><span data-stu-id="38223-130">For all purposes the `in` parameter is treated as a `readonly` variable.</span></span> <span data-ttu-id="38223-131">Die meisten Einschränkungen in Bezug auf die Verwendung von `in`-Parametern in der-Methode sind identisch mit denen `readonly` Felder.</span><span class="sxs-lookup"><span data-stu-id="38223-131">Most of the restrictions on the use of `in` parameters inside the method are the same as with `readonly` fields.</span></span>

> <span data-ttu-id="38223-132">Tatsächlich kann ein `in`-Parameter ein `readonly` Feld darstellen.</span><span class="sxs-lookup"><span data-stu-id="38223-132">Indeed an `in` parameter may represent a `readonly` field.</span></span> <span data-ttu-id="38223-133">Die Ähnlichkeit von Einschränkungen stellt keinen Zufall dar.</span><span class="sxs-lookup"><span data-stu-id="38223-133">Similarity of restrictions is not a coincidence.</span></span>

<span data-ttu-id="38223-134">Beispielsweise werden Felder eines `in`-Parameters mit einem Strukturtyp rekursiv als `readonly` Variablen klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="38223-134">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- <span data-ttu-id="38223-135">`in` Parameter sind überall zulässig, wo gewöhnliche ByVal-Parameter zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="38223-135">`in` parameters are allowed anywhere where ordinary byval parameters are allowed.</span></span> <span data-ttu-id="38223-136">Dies schließt Indexer, Operatoren (einschließlich Konvertierungen), Delegaten, Lambdas, lokale Funktionen ein.</span><span class="sxs-lookup"><span data-stu-id="38223-136">This includes indexers, operators (including conversions), delegates, lambdas, local functions.</span></span>

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- <span data-ttu-id="38223-137">`in` ist in Kombination mit `out` oder mit allem, was `out` nicht kombiniert, nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="38223-137">`in` is not allowed in combination with `out` or with anything that `out` does not combine with.</span></span>

- <span data-ttu-id="38223-138">Es ist nicht zulässig, `ref`/zu überlasten `out`/`in` Unterschiede.</span><span class="sxs-lookup"><span data-stu-id="38223-138">It is not permitted to overload on `ref`/`out`/`in` differences.</span></span>

- <span data-ttu-id="38223-139">Es ist zulässig, bei normalen ByVal-und `in` unterschieden überladen zu können.</span><span class="sxs-lookup"><span data-stu-id="38223-139">It is permitted to overload on ordinary byval and `in` differences.</span></span>

- <span data-ttu-id="38223-140">Für den Zweck von ohi (überladen, ausblenden, implementieren) verhält sich `in` ähnlich wie ein `out` Parameter.</span><span class="sxs-lookup"><span data-stu-id="38223-140">For the purpose of OHI (Overloading, Hiding, Implementing), `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="38223-141">Es gelten dieselben Regeln.</span><span class="sxs-lookup"><span data-stu-id="38223-141">All the same rules apply.</span></span>
<span data-ttu-id="38223-142">Beispielsweise muss die über schreibende Methode `in`-Parametern mit `in` Parametern eines Identitäts konvertierbaren Typs abgeglichen werden.</span><span class="sxs-lookup"><span data-stu-id="38223-142">For example the overriding method will have to match `in` parameters with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="38223-143">Für den Zweck der Konvertierung von Delegaten/Lambda-/Methodengruppen verhält sich `in` ähnlich wie ein `out` Parameter.</span><span class="sxs-lookup"><span data-stu-id="38223-143">For the purpose of delegate/lambda/method group conversions, `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="38223-144">Lambdas und entsprechende Methoden Gruppen Konvertierungs Kandidaten müssen `in` Parametern des Ziel Delegaten mit `in` Parametern eines Identitäts konvertierbaren Typs abgeglichen werden.</span><span class="sxs-lookup"><span data-stu-id="38223-144">Lambdas and applicable method group conversion candidates will have to match `in` parameters of the target delegate with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="38223-145">Zum Zweck der generischen Varianz sind `in` Parameter nicht Variant.</span><span class="sxs-lookup"><span data-stu-id="38223-145">For the purpose of generic variance, `in` parameters are nonvariant.</span></span>

> <span data-ttu-id="38223-146">Hinweis: Es gibt keine Warnungen für `in` Parameter, die Verweis-oder primitive Typen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="38223-146">NOTE: There are no warnings on `in` parameters that have reference or primitives types.</span></span>
<span data-ttu-id="38223-147">Im Allgemeinen ist es möglicherweise sinnlos, aber in einigen Fällen muss der Benutzer primitive als `in`übergeben.</span><span class="sxs-lookup"><span data-stu-id="38223-147">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="38223-148">Beispiele: Überschreiben einer generischen Methode, wie z. b. `Method(in T param)`, wenn `T` `int`ersetzt hat, oder wenn Methoden wie `Volatile.Read(in int location)`</span><span class="sxs-lookup"><span data-stu-id="38223-148">Examples - overriding a generic method like `Method(in T param)` when `T` was substituted to be `int`, or when having methods like `Volatile.Read(in int location)`</span></span>
>
> <span data-ttu-id="38223-149">Es ist denkbar, dass Sie über einen Analyzer verfügen, der im Fall einer ineffizienten Verwendung von `in`-Parametern gewarnt wird, aber die Regeln für diese Analyse wären zu unscharf, um Teil einer Sprachspezifikation zu sein.</span><span class="sxs-lookup"><span data-stu-id="38223-149">It is conceivable to have an analyzer that warns in cases of inefficient use of `in` parameters, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="use-of-in-at-call-sites-in-arguments"></a><span data-ttu-id="38223-150">Verwendung von `in` an Aufrufsites.</span><span class="sxs-lookup"><span data-stu-id="38223-150">Use of `in` at call sites.</span></span> <span data-ttu-id="38223-151">(`in` Argumente)</span><span class="sxs-lookup"><span data-stu-id="38223-151">(`in` arguments)</span></span>

<span data-ttu-id="38223-152">Es gibt zwei Möglichkeiten, Argumente an `in` Parameter zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="38223-152">There are two ways to pass arguments to `in` parameters.</span></span>

#### <a name="in-arguments-can-match-in-parameters"></a><span data-ttu-id="38223-153">`in` Argumente können `in` Parametern entsprechen:</span><span class="sxs-lookup"><span data-stu-id="38223-153">`in` arguments can match `in` parameters:</span></span>

<span data-ttu-id="38223-154">Ein Argument mit einem `in`-Modifizierer an der-aufrufssite kann `in` Parametern entsprechen.</span><span class="sxs-lookup"><span data-stu-id="38223-154">An argument with an `in` modifier at the call site can match `in` parameters.</span></span>

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- <span data-ttu-id="38223-155">`in` Argument muss ein _lesbarer_ lvalue (\*) sein.</span><span class="sxs-lookup"><span data-stu-id="38223-155">`in` argument must be a _readable_ LValue(\*).</span></span>
<span data-ttu-id="38223-156">Beispiel: `M1(in 42)` ist ungültig.</span><span class="sxs-lookup"><span data-stu-id="38223-156">Example: `M1(in 42)` is invalid</span></span>

> <span data-ttu-id="38223-157">(\*) Das Konzept von [Lvalue/Rvalue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) variiert je nach Sprache.</span><span class="sxs-lookup"><span data-stu-id="38223-157">(\*) The notion of [LValue/RValue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) vary between languages.</span></span>  
<span data-ttu-id="38223-158">Hier ist nach lvalue ein Ausdruck gemeint, der einen Speicherort darstellt, auf den direkt verwiesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="38223-158">Here, by LValue I mean an expression that represent a location that can be referred to directly.</span></span>
<span data-ttu-id="38223-159">Und Rvalue bedeutet einen Ausdruck, der ein temporäres Ergebnis ergibt, das nicht eigenständig persistent gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="38223-159">And RValue means an expression that yields a temporary result which does not persist on its own.</span></span>  

- <span data-ttu-id="38223-160">Insbesondere ist es zulässig, `readonly` Felder, `in` Parameter oder andere formal `readonly` Variablen als `in` Argumente zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="38223-160">In particular it is valid to pass `readonly` fields, `in` parameters or other formally `readonly` variables as `in` arguments.</span></span>
<span data-ttu-id="38223-161">Beispiel: `dictionary[in Guid.Empty]` ist zulässig.</span><span class="sxs-lookup"><span data-stu-id="38223-161">Example: `dictionary[in Guid.Empty]` is legal.</span></span> <span data-ttu-id="38223-162">`Guid.Empty` ist ein statisches Schreib geschütztes Feld.</span><span class="sxs-lookup"><span data-stu-id="38223-162">`Guid.Empty` is a static readonly field.</span></span>

- <span data-ttu-id="38223-163">`in` Argument muss eine _Typidentität_ aufweisen, die in den Typ des Parameters konvertiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="38223-163">`in` argument must have type _identity-convertible_ to the type of the parameter.</span></span>
<span data-ttu-id="38223-164">Beispiel: `M1<object>(in Guid.Empty)` ist ungültig.</span><span class="sxs-lookup"><span data-stu-id="38223-164">Example: `M1<object>(in Guid.Empty)` is invalid.</span></span> <span data-ttu-id="38223-165">`Guid.Empty` ist nicht _Identitäts_ Wechsel in `object`</span><span class="sxs-lookup"><span data-stu-id="38223-165">`Guid.Empty` is not _identity-convertible_ to `object`</span></span>

<span data-ttu-id="38223-166">Die Motivation für die oben genannten Regeln besteht darin, dass `in` Argumente das _Aliasing_ der Argument Variablen sicherstellen.</span><span class="sxs-lookup"><span data-stu-id="38223-166">The motivation for the above rules is that `in` arguments guarantee _aliasing_ of the argument variable.</span></span> <span data-ttu-id="38223-167">Der aufgerufene erhält immer einen direkten Verweis auf denselben Speicherort, der durch das-Argument dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="38223-167">The callee always receives a direct reference to the same location as represented by the argument.</span></span>

- <span data-ttu-id="38223-168">in seltenen Fällen, in denen `in` Argumente aufgrund von `await` Ausdrücken, die als Operanden desselben Aufrufes verwendet werden, Stapelüberlauf sein müssen, ist das Verhalten identisch mit dem `out` und `ref`-Argumenten. wenn die Variable nicht auf die Weise übertragen werden kann, wird ein Fehler gemeldet.</span><span class="sxs-lookup"><span data-stu-id="38223-168">in rare situations when `in` arguments must be stack-spilled due to `await` expressions used as operands of the same call, the behavior is the same as with `out` and `ref` arguments - if the variable cannot be spilled in referentially-transparent manner, an error is reported.</span></span>

<span data-ttu-id="38223-169">Beispiele:</span><span class="sxs-lookup"><span data-stu-id="38223-169">Examples:</span></span>
1. <span data-ttu-id="38223-170">`M1(in staticField, await SomethingAsync())` ist gültig.</span><span class="sxs-lookup"><span data-stu-id="38223-170">`M1(in staticField, await SomethingAsync())`  is valid.</span></span>
<span data-ttu-id="38223-171">`staticField` ist ein statisches Feld, auf das mehr als einmal ohne Observable-Nebeneffekte zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="38223-171">`staticField` is a static field which can be accessed more than once without observable side effects.</span></span> <span data-ttu-id="38223-172">Daher können die Reihenfolge von Nebeneffekten und Aliasing Anforderungen bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="38223-172">Therefore both the order of side effects and aliasing requirements can be provided.</span></span>
2. <span data-ttu-id="38223-173">`M1(in RefReturningMethod(), await SomethingAsync())` erzeugt einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="38223-173">`M1(in RefReturningMethod(), await SomethingAsync())`  will produce an error.</span></span>
<span data-ttu-id="38223-174">`RefReturningMethod()` ist eine `ref` Rückgabe Methode.</span><span class="sxs-lookup"><span data-stu-id="38223-174">`RefReturningMethod()` is a `ref` returning method.</span></span> <span data-ttu-id="38223-175">Ein Methodenaufruf kann beobachtbare Nebeneffekte haben und muss daher vor dem `SomethingAsync()`-Operanden ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="38223-175">A method call may have observable side effects, therefore it must be evaluated before the `SomethingAsync()` operand.</span></span> <span data-ttu-id="38223-176">Das Ergebnis des aufforderes ist jedoch ein Verweis, der nicht über den `await` Unterbrechungs Punkt beibehalten werden kann, der die direkte Verweis Anforderung unmöglich macht.</span><span class="sxs-lookup"><span data-stu-id="38223-176">However the result of the invocation is a reference that cannot be preserved across the `await` suspension point which make the direct reference requirement impossible.</span></span>

> <span data-ttu-id="38223-177">Hinweis: die Stapelüberlauf Fehler gelten als Implementierungs spezifische Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="38223-177">NOTE: the stack spilling errors are considered to be implementation-specific limitations.</span></span> <span data-ttu-id="38223-178">Daher haben Sie keinen Einfluss auf die Überladungs Auflösung oder den Lambda-Inference.</span><span class="sxs-lookup"><span data-stu-id="38223-178">Therefore they do not have effect on overload resolution or lambda inference.</span></span>

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a><span data-ttu-id="38223-179">Normale ByVal-Argumente können `in` Parametern entsprechen:</span><span class="sxs-lookup"><span data-stu-id="38223-179">Ordinary byval arguments can match `in` parameters:</span></span>

<span data-ttu-id="38223-180">Reguläre Argumente ohne modifiziererer können `in`-Parametern entsprechen.</span><span class="sxs-lookup"><span data-stu-id="38223-180">Regular arguments without modifiers can match `in` parameters.</span></span> <span data-ttu-id="38223-181">In diesem Fall verfügen die Argumente über die gleichen gelockerten Einschränkungen wie normale ByVal-Argumente.</span><span class="sxs-lookup"><span data-stu-id="38223-181">In such case the arguments have the same relaxed constraints as an ordinary byval arguments would have.</span></span>

<span data-ttu-id="38223-182">Die Motivation für dieses Szenario besteht darin, dass `in` Parameter in APIs zu Unannehmlichkeiten für den Benutzer führen können, wenn Argumente nicht als direkter Verweis (ex: Literale, berechnete oder `await`-bezogene Ergebnisse oder Argumente, die über spezifischere Typen verfügen,) weitergegeben werden können.</span><span class="sxs-lookup"><span data-stu-id="38223-182">The motivation for this scenario is that `in` parameters in APIs may result in inconveniences for the user when arguments cannot be passed as a direct reference - ex: literals, computed or `await`-ed results or arguments that happen to have more specific types.</span></span>  
<span data-ttu-id="38223-183">Alle diese Fälle haben eine triviale Lösung, den Argument Wert in einem temporären lokalen des entsprechenden Typs zu speichern und diesen lokal als `in` Argument zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="38223-183">All these cases have a trivial solution of storing the argument value in a temporary local of appropriate type and passing that local as an `in` argument.</span></span>  
<span data-ttu-id="38223-184">Wenn `in` Modifizierer nicht an der aufrufssite vorhanden ist, kann der Code Compiler die gleiche Transformation durchführen, um die Notwendigkeit zu verringern.</span><span class="sxs-lookup"><span data-stu-id="38223-184">To reduce the need for such boilerplate code compiler can perform the same transformation, if needed, when `in` modifier is not present at the call site.</span></span>  

<span data-ttu-id="38223-185">Außerdem gibt es in manchen Fällen, z. b. beim Aufrufen von Operatoren oder bei `in` Erweiterungs Methoden, keine syntaktische Methode, `in` überhaupt anzugeben.</span><span class="sxs-lookup"><span data-stu-id="38223-185">In addition, in some cases, such as invocation of operators, or `in` extension methods, there is no syntactical way to specify `in` at all.</span></span> <span data-ttu-id="38223-186">Das allein erfordert, dass Sie das Verhalten normaler ByVal-Argumente angeben, wenn Sie `in`-Parametern entsprechen.</span><span class="sxs-lookup"><span data-stu-id="38223-186">That alone requires specifying the behavior of ordinary byval arguments when they match `in` parameters.</span></span>

<span data-ttu-id="38223-187">Dies gilt insbesondere für:</span><span class="sxs-lookup"><span data-stu-id="38223-187">In particular:</span></span>

- <span data-ttu-id="38223-188">Es ist gültig, Rvalues zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="38223-188">it is valid to pass RValues.</span></span>
<span data-ttu-id="38223-189">Ein Verweis auf eine temporäre wird in diesem Fall übermittelt.</span><span class="sxs-lookup"><span data-stu-id="38223-189">A reference to a temporary is passed in such case.</span></span>
<span data-ttu-id="38223-190">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="38223-190">Example:</span></span>
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- <span data-ttu-id="38223-191">implizite Konvertierungen sind zulässig.</span><span class="sxs-lookup"><span data-stu-id="38223-191">implicit conversions are allowed.</span></span>

> <span data-ttu-id="38223-192">Dies ist ein Sonderfall, wenn ein rvalue-Wert übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="38223-192">This is actually a special case of passing an RValue</span></span>  

<span data-ttu-id="38223-193">In einem solchen Fall wird ein Verweis auf einen temporären, mit dem Wert konvertierten Wert übermittelt.</span><span class="sxs-lookup"><span data-stu-id="38223-193">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="38223-194">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="38223-194">Example:</span></span>
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- <span data-ttu-id="38223-195">bei einem Empfänger einer `in`-Erweiterungsmethode (im Gegensatz zu `ref` Erweiterungs Methoden) sind Rvalues oder implizite _this-Argument-Konvertierungen_ zulässig.</span><span class="sxs-lookup"><span data-stu-id="38223-195">in a case of a receiver of an `in` extension method (as opposed to `ref` extension methods), RValues or implicit _this-argument-conversions_ are allowed.</span></span>
<span data-ttu-id="38223-196">In einem solchen Fall wird ein Verweis auf einen temporären, mit dem Wert konvertierten Wert übermittelt.</span><span class="sxs-lookup"><span data-stu-id="38223-196">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="38223-197">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="38223-197">Example:</span></span>
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
<span data-ttu-id="38223-198">Weitere Informationen zu `ref`/`in` Erweiterungs Methoden finden Sie weiter unten in diesem Dokument.</span><span class="sxs-lookup"><span data-stu-id="38223-198">More information on `ref`/`in` extension methods is provided further in this document.</span></span>

- <span data-ttu-id="38223-199">ein Argument Überlauf aufgrund von `await` Operanden könnte ggf. "by-Value" überlaufen.</span><span class="sxs-lookup"><span data-stu-id="38223-199">argument spilling due to `await` operands could spill "by-value", if necessary.</span></span>
<span data-ttu-id="38223-200">In Szenarien, in denen die Bereitstellung eines direkten Verweises auf das-Argument aufgrund von dazwischen liegenden `await` nicht möglich ist, wird stattdessen eine Kopie des Argument Werts übertragen.</span><span class="sxs-lookup"><span data-stu-id="38223-200">In scenarios where providing a direct reference to the argument is not possible due to intervening `await` a copy of the argument's value is spilled instead.</span></span>  
<span data-ttu-id="38223-201">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="38223-201">Example:</span></span>
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
<span data-ttu-id="38223-202">Da es sich bei dem Ergebnis eines Nebeneffekten um einen Verweis handelt, der nicht über `await` Unterbrechung hinweg beibehalten werden kann, wird stattdessen ein temporäres mit dem tatsächlichen Wert beibehalten (wie es in einem normalen ByVal-Parameter Fall der Fall wäre).</span><span class="sxs-lookup"><span data-stu-id="38223-202">Since the result of a side-effecting invocation is a reference that cannot be preserved across `await` suspension, a temporary containing the actual value will be preserved instead (as it would in an ordinary byval parameter case).</span></span>

#### <a name="omitted-optional-arguments"></a><span data-ttu-id="38223-203">Nicht optionale Argumente ausgelassen</span><span class="sxs-lookup"><span data-stu-id="38223-203">Omitted optional arguments</span></span>

<span data-ttu-id="38223-204">Es ist zulässig, dass ein `in`-Parameter einen Standardwert angibt.</span><span class="sxs-lookup"><span data-stu-id="38223-204">It is permitted for an `in` parameter to specify a default value.</span></span> <span data-ttu-id="38223-205">Dadurch wird das entsprechende Argument optional.</span><span class="sxs-lookup"><span data-stu-id="38223-205">That makes the corresponding argument optional.</span></span>

<span data-ttu-id="38223-206">Wenn Sie das optionale Argument an der aufrufssite weglassen, wird der Standardwert über einen temporären übergeben.</span><span class="sxs-lookup"><span data-stu-id="38223-206">Omitting optional argument at the call site results in passing the default value via a temporary.</span></span>

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a><span data-ttu-id="38223-207">Aliasing-Verhalten im allgemeinen</span><span class="sxs-lookup"><span data-stu-id="38223-207">Aliasing behavior in general</span></span>

<span data-ttu-id="38223-208">Ebenso wie `ref` und `out` Variablen sind `in` Variablen Verweise/Aliase auf vorhandene Speicherorte.</span><span class="sxs-lookup"><span data-stu-id="38223-208">Just like `ref` and `out` variables, `in` variables are references/aliases to existing locations.</span></span>

<span data-ttu-id="38223-209">Das Schreiben eines `in`-Parameters ist zwar nicht zulässig, kann jedoch unterschiedliche Werte als Nebeneffekte anderer Auswertungen beobachten.</span><span class="sxs-lookup"><span data-stu-id="38223-209">While callee is not allowed to write into them, reading an `in` parameter can observe different values as a side effect of other evaluations.</span></span>

<span data-ttu-id="38223-210">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="38223-210">Example:</span></span>

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a><span data-ttu-id="38223-211">`in` Parameter und die Erfassung von lokalen Variablen.</span><span class="sxs-lookup"><span data-stu-id="38223-211">`in` parameters and capturing of local variables.</span></span>  
<span data-ttu-id="38223-212">Zum Zweck der Lambda-/Async-Erfassung `in` Parameter identisch mit den Parametern `out` und `ref`.</span><span class="sxs-lookup"><span data-stu-id="38223-212">For the purpose of lambda/async capturing `in` parameters behave the same as `out` and `ref` parameters.</span></span>

- <span data-ttu-id="38223-213">`in` Parameter können nicht in einem Abschluss aufgezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="38223-213">`in` parameters cannot be captured in a closure</span></span>
- <span data-ttu-id="38223-214">`in` Parameter sind in Iteratormethoden nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="38223-214">`in` parameters are not allowed in iterator methods</span></span>
- <span data-ttu-id="38223-215">`in` Parameter sind in Async-Methoden nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="38223-215">`in` parameters are not allowed in async methods</span></span>

### <a name="temporary-variables"></a><span data-ttu-id="38223-216">Temporäre Variablen.</span><span class="sxs-lookup"><span data-stu-id="38223-216">Temporary variables.</span></span>  
<span data-ttu-id="38223-217">Einige Verwendungsmöglichkeiten von `in` Parameter Übergabe erfordern möglicherweise die indirekte Verwendung einer temporären lokalen Variablen:</span><span class="sxs-lookup"><span data-stu-id="38223-217">Some uses of `in` parameter passing may require indirect use of a temporary local variable:</span></span>  
- <span data-ttu-id="38223-218">`in` Argumente werden immer als direkte Aliase weitergegeben, wenn die Aufrufsite `in`verwendet.</span><span class="sxs-lookup"><span data-stu-id="38223-218">`in` arguments are always passed as direct aliases when call-site uses `in`.</span></span> <span data-ttu-id="38223-219">Temporär wird in diesem Fall nie verwendet.</span><span class="sxs-lookup"><span data-stu-id="38223-219">Temporary is never used in such case.</span></span>
- <span data-ttu-id="38223-220">`in` Argumente müssen keine direkten Aliase sein, wenn die Aufrufsite `in`nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="38223-220">`in` arguments are not required to be direct aliases when call-site does not use `in`.</span></span> <span data-ttu-id="38223-221">Wenn das Argument kein lvalue ist, kann ein temporäres verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="38223-221">When argument is not an LValue, a temporary may be used.</span></span>
- <span data-ttu-id="38223-222">`in` Parameter kann einen Standardwert aufweisen.</span><span class="sxs-lookup"><span data-stu-id="38223-222">`in` parameter may have default value.</span></span> <span data-ttu-id="38223-223">Wenn das entsprechende Argument an der aufrufssite weggelassen wird, wird der Standardwert über einen temporären Wert übermittelt.</span><span class="sxs-lookup"><span data-stu-id="38223-223">When corresponding argument is omitted at the call site, the default value are passed via a temporary.</span></span>
- <span data-ttu-id="38223-224">`in` Argumente können implizite Konvertierungen aufweisen, einschließlich derjenigen, die die Identität nicht beibehalten.</span><span class="sxs-lookup"><span data-stu-id="38223-224">`in` arguments may have implicit conversions, including those that do not preserve identity.</span></span> <span data-ttu-id="38223-225">In diesen Fällen wird ein temporäres verwendet.</span><span class="sxs-lookup"><span data-stu-id="38223-225">A temporary is used in those cases.</span></span>
- <span data-ttu-id="38223-226">Empfänger von gewöhnlichen Struktur aufrufen sind möglicherweise keine beschreibbaren Lvalues (**vorhandener Fall!** ).</span><span class="sxs-lookup"><span data-stu-id="38223-226">receivers of ordinary struct calls may not be writeable LValues (**existing case!**).</span></span> <span data-ttu-id="38223-227">In diesen Fällen wird ein temporäres verwendet.</span><span class="sxs-lookup"><span data-stu-id="38223-227">A temporary is used in those cases.</span></span>

<span data-ttu-id="38223-228">Die Lebensdauer der Argument temporare stimmt mit dem nächstgelegenen Bereich der Aufruf Site überein.</span><span class="sxs-lookup"><span data-stu-id="38223-228">The life time of the argument temporaries matches the closest encompassing scope of the call-site.</span></span>

<span data-ttu-id="38223-229">Die formale Lebensdauer temporärer Variablen ist in Szenarien mit escapeanalysen von Variablen, die als Verweis zurückgegeben werden, semantisch signifikant.</span><span class="sxs-lookup"><span data-stu-id="38223-229">The formal life time of temporary variables is semantically significant in scenarios involving escape analysis of variables returned by reference.</span></span>

### <a name="metadata-representation-of-in-parameters"></a><span data-ttu-id="38223-230">Metadatendarstellung von `in`-Parametern.</span><span class="sxs-lookup"><span data-stu-id="38223-230">Metadata representation of `in` parameters.</span></span>
<span data-ttu-id="38223-231">Wenn `System.Runtime.CompilerServices.IsReadOnlyAttribute` auf einen ByRef-Parameter angewendet wird, bedeutet dies, dass der Parameter ein `in` Parameter ist.</span><span class="sxs-lookup"><span data-stu-id="38223-231">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a byref parameter, it means that the parameter is an `in` parameter.</span></span>

<span data-ttu-id="38223-232">Wenn die Methode *abstrakt* oder *virtuell*ist, muss die Signatur solcher Parameter (und nur solcher Parameter) über `modreq[System.Runtime.InteropServices.InAttribute]`verfügen.</span><span class="sxs-lookup"><span data-stu-id="38223-232">In addition, if the method is *abstract* or *virtual*, then the signature of such parameters (and only such parameters) must have `modreq[System.Runtime.InteropServices.InAttribute]`.</span></span>

<span data-ttu-id="38223-233">**Motivation**: Dies geschieht, um sicherzustellen, dass bei der Überschreibung/Implementierung der `in` Parameter Übereinstimmung vorliegt.</span><span class="sxs-lookup"><span data-stu-id="38223-233">**Motivation**: this is done to ensure that in a case of method overriding/implementing the `in` parameters match.</span></span>

<span data-ttu-id="38223-234">Die gleichen Anforderungen gelten für `Invoke` Methoden in Delegaten.</span><span class="sxs-lookup"><span data-stu-id="38223-234">Same requirements apply to `Invoke` methods in delegates.</span></span>

<span data-ttu-id="38223-235">**Motivation**: Dadurch wird sichergestellt, dass vorhandene Compiler beim Erstellen oder Zuweisen von Delegaten nicht einfach `readonly` ignorieren können.</span><span class="sxs-lookup"><span data-stu-id="38223-235">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when creating or assigning delegates.</span></span>

## <a name="returning-by-readonly-reference"></a><span data-ttu-id="38223-236">Rückgabe durch einen schreibgeschützten Verweis.</span><span class="sxs-lookup"><span data-stu-id="38223-236">Returning by readonly reference.</span></span>

### <a name="motivation"></a><span data-ttu-id="38223-237">Motivation</span><span class="sxs-lookup"><span data-stu-id="38223-237">Motivation</span></span>
<span data-ttu-id="38223-238">Die Motivation für diese Unterfunktion ist ungefähr symmetrisch zu den Gründen für die `in` Parameter-das Kopieren wird vermieden, aber auf der Rückgabe Seite.</span><span class="sxs-lookup"><span data-stu-id="38223-238">The motivation for this sub-feature is roughly symmetrical to the reasons for the `in` parameters - avoiding copying, but on the returning side.</span></span> <span data-ttu-id="38223-239">Vor dieser Funktion hatten eine Methode oder ein Indexer zwei Optionen: 1) Rückgabe als Verweis und verfügbar für mögliche Mutationen oder 2) Rückgabe nach Wert, der zum Kopieren führt.</span><span class="sxs-lookup"><span data-stu-id="38223-239">Prior to this feature, a method or an indexer had two options: 1) return by reference and be exposed to possible mutations or 2) return by value which results in copying.</span></span>

### <a name="solution-ref-readonly-returns"></a><span data-ttu-id="38223-240">Lösung (`ref readonly` Returns)</span><span class="sxs-lookup"><span data-stu-id="38223-240">Solution (`ref readonly` returns)</span></span>  
<span data-ttu-id="38223-241">Die Funktion ermöglicht es einem Member, Variablen als Verweis zurückzugeben, ohne Sie für Mutationen verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="38223-241">The feature allows a member to return variables by reference without exposing them to mutations.</span></span>

### <a name="declaring-ref-readonly-returning-members"></a><span data-ttu-id="38223-242">Deklarieren `ref readonly` zurück gebenden Membern</span><span class="sxs-lookup"><span data-stu-id="38223-242">Declaring `ref readonly` returning members</span></span>

<span data-ttu-id="38223-243">Eine Kombination von modifiziererelementen, die auf die Rückgabe Signatur `ref readonly`, wird verwendet, um anzugeben, dass der Member einen schreibgeschützten Verweis zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="38223-243">A combination of modifiers `ref readonly` on the return signature is used to to indicate that the member returns a readonly reference.</span></span>

<span data-ttu-id="38223-244">Für alle Zwecke wird ein `ref readonly` Member als `readonly` Variable behandelt, ähnlich wie `readonly` Feldern und `in`-Parametern.</span><span class="sxs-lookup"><span data-stu-id="38223-244">For all purposes a `ref readonly` member is treated as a `readonly` variable - similar to `readonly` fields and `in` parameters.</span></span>

<span data-ttu-id="38223-245">Beispielsweise werden Felder mit `ref readonly` Member mit einem Strukturtyp alle als `readonly` Variablen klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="38223-245">For example fields of `ref readonly` member which has a struct type are all recursively classified as `readonly` variables.</span></span> <span data-ttu-id="38223-246">-Es ist zulässig, Sie als `in` Argumente zu übergeben, aber nicht als `ref` oder `out` Argumente.</span><span class="sxs-lookup"><span data-stu-id="38223-246">- It is permitted to pass them as `in` arguments, but not as `ref` or `out` arguments.</span></span>

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- <span data-ttu-id="38223-247">`ref readonly` Rückgaben sind an denselben Stellen zulässig, wenn `ref` Rückgabe zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="38223-247">`ref readonly` returns are allowed in the same places were `ref` returns are allowed.</span></span>
<span data-ttu-id="38223-248">Dies schließt Indexer, Delegaten, Lambdas, lokale Funktionen ein.</span><span class="sxs-lookup"><span data-stu-id="38223-248">This includes indexers, delegates, lambdas, local functions.</span></span>

- <span data-ttu-id="38223-249">Es ist nicht zulässig, `ref`/`ref readonly`/Unterschiede zu überlasten.</span><span class="sxs-lookup"><span data-stu-id="38223-249">It is not permitted to overload on `ref`/`ref readonly` /  differences.</span></span>

- <span data-ttu-id="38223-250">Es ist zulässig, bei normalen ByVal-und `ref readonly` Rückschlag unterschieden zu überlasten.</span><span class="sxs-lookup"><span data-stu-id="38223-250">It is permitted to overload on ordinary byval and `ref readonly` return differences.</span></span>

- <span data-ttu-id="38223-251">Für ohi (überladen, ausblenden, implementieren) ist `ref readonly` ähnlich, unterscheidet sich jedoch von `ref`.</span><span class="sxs-lookup"><span data-stu-id="38223-251">For the purpose of OHI (Overloading, Hiding, Implementing), `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="38223-252">Beispielsweise muss eine Methode, die `ref readonly` eins überschreibt, selbst `ref readonly` sein und über einen Identitäts konvertierbaren Typ verfügen.</span><span class="sxs-lookup"><span data-stu-id="38223-252">For example the a method that overrides `ref readonly` one, must itself be `ref readonly` and have identity-convertible type.</span></span>

- <span data-ttu-id="38223-253">Für den Zweck der Konvertierung von Delegaten/Lambda-/Methodengruppen ist `ref readonly` ähnlich, unterscheidet sich jedoch von `ref`.</span><span class="sxs-lookup"><span data-stu-id="38223-253">For the purpose of delegate/lambda/method group conversions, `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="38223-254">Lambdas und entsprechende Methoden Gruppen Konvertierungs Kandidaten müssen `ref readonly` Rückgabe des Ziel Delegaten mit `ref readonly` Rückgabe des Typs, der Identitäts konvertierbar ist, abgleichen.</span><span class="sxs-lookup"><span data-stu-id="38223-254">Lambdas and applicable method group conversion candidates have to match `ref readonly` return of the target delegate with `ref readonly` return of the type that is identity-convertible.</span></span>

- <span data-ttu-id="38223-255">Zum Zweck der generischen Varianz sind `ref readonly`-Rückgaben nicht Variant.</span><span class="sxs-lookup"><span data-stu-id="38223-255">For the purpose of generic variance, `ref readonly` returns are nonvariant.</span></span>

> <span data-ttu-id="38223-256">Hinweis: Es gibt keine Warnungen für `ref readonly`-Rückgaben, die Verweis-oder primitive Typen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="38223-256">NOTE: There are no warnings on `ref readonly` returns that have reference or primitives types.</span></span>
<span data-ttu-id="38223-257">Im Allgemeinen ist es möglicherweise sinnlos, aber in einigen Fällen muss der Benutzer primitive als `in`übergeben.</span><span class="sxs-lookup"><span data-stu-id="38223-257">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="38223-258">Beispiele: Überschreiben einer generischen Methode wie `ref readonly T Method()`, wenn `T` als `int`ersetzt wurde.</span><span class="sxs-lookup"><span data-stu-id="38223-258">Examples - overriding a generic method like `ref readonly T Method()` when `T` was substituted to be `int`.</span></span>
>
><span data-ttu-id="38223-259">Es ist denkbar, eine Analyse zu verwenden, die im Falle einer ineffizienten Verwendung von `ref readonly`-Rückgabe gewarnt wird, aber die Regeln für diese Analyse wären zu unscharf, um Teil einer Sprachspezifikation zu sein.</span><span class="sxs-lookup"><span data-stu-id="38223-259">It is conceivable to have an analyzer that warns in cases of inefficient use of `ref readonly` returns, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="returning-from-ref-readonly-members"></a><span data-ttu-id="38223-260">Zurückgeben von `ref readonly` Membern</span><span class="sxs-lookup"><span data-stu-id="38223-260">Returning from `ref readonly` members</span></span>
<span data-ttu-id="38223-261">Innerhalb des Methoden Texts ist die Syntax identisch mit der regulären Ref-Rückgabe.</span><span class="sxs-lookup"><span data-stu-id="38223-261">Inside the method body the syntax is the same as with regular ref returns.</span></span> <span data-ttu-id="38223-262">Der `readonly` wird von der enthaltenden Methode abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="38223-262">The `readonly` will be inferred from the containing method.</span></span>

<span data-ttu-id="38223-263">Die Motivation besteht darin, dass `return ref readonly <expression>` unnötig lange ist und nur Konflikte im `readonly` Teil zulässt, die immer zu Fehlern führen.</span><span class="sxs-lookup"><span data-stu-id="38223-263">The motivation is that `return ref readonly <expression>` is unnecessary long and only allows for mismatches on the `readonly` part that would always result in errors.</span></span>
<span data-ttu-id="38223-264">Der `ref` ist jedoch aus Gründen der Konsistenz mit anderen Szenarien erforderlich, in denen etwas durch Strict Aliasing oder durch einen Wert übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="38223-264">The `ref` is, however, required for consistency with other scenarios where something is passed via strict aliasing vs. by value.</span></span>

> <span data-ttu-id="38223-265">Anders als bei `in` Parametern gibt `ref readonly` zurück, die nie über eine lokale Kopie zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="38223-265">Unlike the case with `in` parameters, `ref readonly` returns never return via a local copy.</span></span> <span data-ttu-id="38223-266">Wenn Sie in Erwägung ziehen, dass die Kopie sofort nach der Rückgabe einer solchen Übung nicht mehr vorhanden ist, wäre es sinnlos und gefährlich.</span><span class="sxs-lookup"><span data-stu-id="38223-266">Considering that the copy would cease to exist immediately upon returning such practice would be pointless and dangerous.</span></span> <span data-ttu-id="38223-267">Daher sind `ref readonly`-Rückgabe immer direkte Verweise.</span><span class="sxs-lookup"><span data-stu-id="38223-267">Therefore `ref readonly` returns are always direct references.</span></span>

<span data-ttu-id="38223-268">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="38223-268">Example:</span></span>

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- <span data-ttu-id="38223-269">Ein Argument von `return ref` muss ein Lvalue (**vorhandene Regel**) sein.</span><span class="sxs-lookup"><span data-stu-id="38223-269">An argument of `return ref` must be an LValue (**existing rule**)</span></span>
- <span data-ttu-id="38223-270">Ein Argument `return ref` muss "sicher zur Rückgabe" (**vorhandene Regel**) sein.</span><span class="sxs-lookup"><span data-stu-id="38223-270">An argument of `return ref` must be "safe to return" (**existing rule**)</span></span>
- <span data-ttu-id="38223-271">In einem `ref readonly` Member muss ein Argument `return ref` _nicht geschrieben werden können_ .</span><span class="sxs-lookup"><span data-stu-id="38223-271">In a `ref readonly` member an argument of `return ref` is _not required to be writeable_ .</span></span>
<span data-ttu-id="38223-272">Beispielsweise kann ein solcher Member Ref-ein Schreib geschütztes Feld oder einen seiner `in` Parameter zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="38223-272">For example such member can ref-return a readonly field or one of its `in` parameters.</span></span>

### <a name="safe-to-return-rules"></a><span data-ttu-id="38223-273">Sichere Rückgaberegeln.</span><span class="sxs-lookup"><span data-stu-id="38223-273">Safe to Return rules.</span></span>
<span data-ttu-id="38223-274">Normale sicher zum Zurückgeben von Regeln für Verweise gelten auch für schreibgeschützte Verweise.</span><span class="sxs-lookup"><span data-stu-id="38223-274">Normal safe to return rules for references will apply to readonly references as well.</span></span>

<span data-ttu-id="38223-275">Beachten Sie, dass ein `ref readonly` aus einem regulären `ref` local/Parameter/Return abgerufen werden kann, aber nicht umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="38223-275">Note that a `ref readonly` can be obtained from a regular `ref` local/parameter/return, but not the other way around.</span></span> <span data-ttu-id="38223-276">Andernfalls wird die Sicherheit der `ref readonly` Rückgabe auf die gleiche Weise wie bei regulären `ref` Rückgaben abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="38223-276">Otherwise the safety of `ref readonly` returns is inferred the same way as for regular `ref` returns.</span></span>

<span data-ttu-id="38223-277">Wenn Sie in Erwägung ziehen, dass Rvalues als `in` Parameter übergeben und als `ref readonly` zurückgegeben werden, benötigen wir eine weitere Regel- **Rvalues als Verweis nicht**als "sicher" zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="38223-277">Considering that RValues can be passed as `in` parameter and returned as `ref readonly` we need one more rule - **RValues are not safe-to-return by reference**.</span></span>

> <span data-ttu-id="38223-278">Beachten Sie die Situation, in der ein rvalue über eine Kopie an einen `in` Parameter übergeben und dann in Form einer `ref readonly`zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="38223-278">Consider the situation when an RValue is passed to an `in` parameter via a copy and then returned back in a form of a `ref readonly`.</span></span> <span data-ttu-id="38223-279">Im Zusammenhang mit dem Aufrufer ist das Ergebnis eines solchen Aufrufers ein Verweis auf lokale Daten, sodass die Rückgabe unsicher ist.</span><span class="sxs-lookup"><span data-stu-id="38223-279">In the context of the caller the result of such invocation is a reference to local data and as such is unsafe to return.</span></span>
> <span data-ttu-id="38223-280">Wenn die Rückgabe von Rvalues nicht sicher ist, wird dieser Fall bereits von der vorhandenen Regel `#6` behandelt.</span><span class="sxs-lookup"><span data-stu-id="38223-280">Once RValues are not safe to return, the existing rule `#6` already handles this case.</span></span>

<span data-ttu-id="38223-281">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="38223-281">Example:</span></span>
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

<span data-ttu-id="38223-282">Aktualisierte `safe to return` Regeln:</span><span class="sxs-lookup"><span data-stu-id="38223-282">Updated `safe to return` rules:</span></span>

1.  <span data-ttu-id="38223-283">**die Rückgabe von Refs auf Variablen im Heap ist sicher.**</span><span class="sxs-lookup"><span data-stu-id="38223-283">**refs to variables on the heap are safe to return**</span></span>
2.  <span data-ttu-id="38223-284">**ref/in-Parameter können sicher zurückgegeben werden,** 
`in` Parameter natürlich nur als schreibgeschützt zurückgegeben werden können.</span><span class="sxs-lookup"><span data-stu-id="38223-284">**ref/in parameters are safe to return**
`in` parameters naturally can only be returned as readonly.</span></span>
3.  <span data-ttu-id="38223-285">**out-Parameter können sicher zurückgegeben** werden (Sie müssen jedoch definitiv zugewiesen werden, wie dies bereits heute der Fall ist).</span><span class="sxs-lookup"><span data-stu-id="38223-285">**out parameters are safe to return** (but must be definitely assigned, as is already the case today)</span></span>
4.  <span data-ttu-id="38223-286">**instanzstrukturfelder können sicher zurückgegeben werden, solange der Empfänger sicher zurückgegeben werden kann.**</span><span class="sxs-lookup"><span data-stu-id="38223-286">**instance struct fields are safe to return as long as the receiver is safe to return**</span></span>
5.  <span data-ttu-id="38223-287">**"This" kann nicht sicher von Strukturmembern zurückgegeben werden.**</span><span class="sxs-lookup"><span data-stu-id="38223-287">**'this' is not safe to return from struct members**</span></span>
6.  <span data-ttu-id="38223-288">**ein Verweis, der von einer anderen Methode zurückgegeben wurde, kann sicher zurückgegeben werden, wenn alle an diese Methode weiter gegebenen Refs/out-Werte, wie Sie sicher zurückgegeben werden konnten**
*insbesondere ist es unerheblich, ob der Empfänger sicher zurückgegeben werden kann, unabhängig davon, ob der Empfänger eine Struktur oder Klasse ist oder als generischer Typparameter typisiert ist.*</span><span class="sxs-lookup"><span data-stu-id="38223-288">**a ref, returned from another method is safe to return if all refs/outs passed to that method as formal parameters were safe to return.**
*Specifically it is irrelevant if receiver is safe to return, regardless whether receiver is a struct, class or typed as a generic type parameter.*</span></span>
7.  <span data-ttu-id="38223-289">**Rvalues können nicht als Verweis zurückgegeben werden.** 
*insbesondere Rvalues sind sicher als in den Parametern zu übergeben.*</span><span class="sxs-lookup"><span data-stu-id="38223-289">**RValues are not safe to return by reference.**
*Specifically RValues are safe to pass as in parameters.*</span></span>

> <span data-ttu-id="38223-290">Hinweis: Es gibt weitere Regeln bezüglich der Sicherheit von zurückgegebenen Rückgaben, wenn Ref-like-Typen und ref-reassignments beteiligt sind.</span><span class="sxs-lookup"><span data-stu-id="38223-290">NOTE: There are additional rules regarding safety of returns that come into play when ref-like types and ref-reassignments are involved.</span></span>
> <span data-ttu-id="38223-291">Die Regeln gelten gleichermaßen für `ref`-und `ref readonly`-Member und werden daher hier nicht erwähnt.</span><span class="sxs-lookup"><span data-stu-id="38223-291">The rules equally apply to `ref` and `ref readonly` members and therefore are not mentioned here.</span></span>

### <a name="aliasing-behavior"></a><span data-ttu-id="38223-292">Aliasing Verhalten.</span><span class="sxs-lookup"><span data-stu-id="38223-292">Aliasing behavior.</span></span>
<span data-ttu-id="38223-293">`ref readonly` Member bieten das gleiche Aliasing Verhalten wie normale `ref` Member (mit Ausnahme von "schreibgeschützt").</span><span class="sxs-lookup"><span data-stu-id="38223-293">`ref readonly` members provide the same aliasing behavior as ordinary `ref` members (except for being readonly).</span></span>
<span data-ttu-id="38223-294">Aus diesem Grund dienen die Erfassung in Lambdas, async, Iteratoren, Stapelüberlauf usw... Es gelten die gleichen Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="38223-294">Therefore for the purpose of capturing in lambdas, async, iterators, stack spilling etc... the same restrictions apply.</span></span> <span data-ttu-id="38223-295">d.</span><span class="sxs-lookup"><span data-stu-id="38223-295">- I.E.</span></span> <span data-ttu-id="38223-296">aufgrund der Tatsache, dass die eigentlichen Verweise nicht erfasst werden können, sind solche Szenarios aufgrund der Nebenwirkungen der Mitglieder Auswertung nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="38223-296">due to inability to capture the actual references and due to side-effecting nature of member evaluation such scenarios are disallowed.</span></span>

> <span data-ttu-id="38223-297">Es ist zulässig und erforderlich, eine Kopie zu erstellen, wenn `ref readonly` Rückgabe ein Empfänger regulärer Struktur Methoden ist, die `this` als einen normalen beschreibbaren Verweis annehmen.</span><span class="sxs-lookup"><span data-stu-id="38223-297">It is permitted and required to make a copy when `ref readonly` return is a receiver of regular struct methods, which take `this` as an ordinary writeable reference.</span></span> <span data-ttu-id="38223-298">In der Vergangenheit wird in allen Fällen, in denen solche Aufrufe auf die schreibgeschützte Variable angewendet werden, eine lokale Kopie erstellt.</span><span class="sxs-lookup"><span data-stu-id="38223-298">Historically in all cases where such invocations are applied to readonly variable a local copy is made.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="38223-299">Metadatendarstellung.</span><span class="sxs-lookup"><span data-stu-id="38223-299">Metadata representation.</span></span>
<span data-ttu-id="38223-300">Wenn `System.Runtime.CompilerServices.IsReadOnlyAttribute` auf die Rückgabe einer ByRef-Rückgabe Methode angewendet wird, bedeutet dies, dass die Methode einen schreibgeschützten Verweis zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="38223-300">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to the return of a byref returning method, it means that the method returns a readonly reference.</span></span>

<span data-ttu-id="38223-301">Außerdem muss die Ergebnis Signatur solcher Methoden (und nur dieser Methoden) über `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`verfügen.</span><span class="sxs-lookup"><span data-stu-id="38223-301">In addition, the result signature of such methods (and only those methods) must have `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.</span></span>

<span data-ttu-id="38223-302">**Motivation**: Dadurch wird sichergestellt, dass vorhandene Compiler beim Aufrufen von Methoden mit `ref readonly`-Rückgabe nicht einfach `readonly` ignorieren können.</span><span class="sxs-lookup"><span data-stu-id="38223-302">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when invoking methods with `ref readonly` returns</span></span>

## <a name="readonly-structs"></a><span data-ttu-id="38223-303">Schreibgeschützte Strukturen</span><span class="sxs-lookup"><span data-stu-id="38223-303">Readonly structs</span></span>
<span data-ttu-id="38223-304">Kurz gesagt: eine Funktion, die `this`-Parameter aller Instanzmember einer Struktur, mit Ausnahme von Konstruktoren, als `in` Parameter definiert.</span><span class="sxs-lookup"><span data-stu-id="38223-304">In short - a feature that makes `this` parameter of all instance members of a struct, except for constructors, an `in` parameter.</span></span>

### <a name="motivation"></a><span data-ttu-id="38223-305">Motivation</span><span class="sxs-lookup"><span data-stu-id="38223-305">Motivation</span></span>
<span data-ttu-id="38223-306">Der Compiler muss davon ausgehen, dass die Instanz durch einen beliebigen Methoden aufrufin einer Struktur Instanz geändert werden kann.</span><span class="sxs-lookup"><span data-stu-id="38223-306">Compiler must assume that any method call on a struct instance may modify the instance.</span></span> <span data-ttu-id="38223-307">Tatsächlich wird ein Beschreib barer Verweis als `this`-Parameter an die Methode übergeben und dieses Verhalten vollständig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="38223-307">Indeed a writeable reference is passed to the method as `this` parameter and fully enables this behavior.</span></span> <span data-ttu-id="38223-308">Um solche Aufrufe für `readonly` Variablen zuzulassen, werden die Aufrufe auf temporäre Kopien angewendet.</span><span class="sxs-lookup"><span data-stu-id="38223-308">To allow such invocations on `readonly` variables, the invocations are applied to temp copies.</span></span> <span data-ttu-id="38223-309">Das kann nicht intuitiv sein und manchmal dazu gezwungen, `readonly` aus Leistungsgründen zu verwerfen.</span><span class="sxs-lookup"><span data-stu-id="38223-309">That could be unintuitive and sometimes forces people to abandon `readonly` for performance reasons.</span></span>  
<span data-ttu-id="38223-310">Beispiel: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span><span class="sxs-lookup"><span data-stu-id="38223-310">Example: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span></span>

<span data-ttu-id="38223-311">Nach dem Hinzufügen der Unterstützung für `in` Parameter und `ref readonly` wird das Problem des defensiven Kopierens zurückgegeben, da schreibgeschützte Variablen häufiger verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="38223-311">After adding support for `in` parameters and `ref readonly` returns the problem of defensive copying will get worse since readonly variables will become more common.</span></span>

### <a name="solution"></a><span data-ttu-id="38223-312">Lösung</span><span class="sxs-lookup"><span data-stu-id="38223-312">Solution</span></span>
<span data-ttu-id="38223-313">Lässt `readonly` Modifizierer für Struktur Deklarationen zu, was dazu führen würde, dass `this` als `in` Parameter für alle strukturinstanzmethoden mit Ausnahme von Konstruktoren behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="38223-313">Allow `readonly` modifier on struct declarations which would result in `this` being treated as `in` parameter on all struct instance methods except for constructors.</span></span>

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a><span data-ttu-id="38223-314">Einschränkungen für Mitglieder der schreibgeschützten Struktur</span><span class="sxs-lookup"><span data-stu-id="38223-314">Restrictions on members of readonly struct</span></span>
- <span data-ttu-id="38223-315">Instanzfelder einer schreibgeschützten Struktur müssen schreibgeschützt sein.</span><span class="sxs-lookup"><span data-stu-id="38223-315">Instance fields of a readonly struct must be readonly.</span></span>  
<span data-ttu-id="38223-316">**Motivation:** kann nur in extern, aber nicht über Member geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="38223-316">**Motivation:** can only be written to externally, but not through members.</span></span>
- <span data-ttu-id="38223-317">Instanzeigenschaften einer schreibgeschützten Struktur müssen nur abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="38223-317">Instance autoproperties of a readonly struct must be get-only.</span></span>  
<span data-ttu-id="38223-318">**Motivation:** die Folge der Einschränkung für Instanzfelder.</span><span class="sxs-lookup"><span data-stu-id="38223-318">**Motivation:** consequence of restriction on instance fields.</span></span>
- <span data-ttu-id="38223-319">Die schreibgeschützte Struktur darf keine Feld ähnlichen Ereignisse deklarieren.</span><span class="sxs-lookup"><span data-stu-id="38223-319">Readonly struct may not declare field-like events.</span></span>  
<span data-ttu-id="38223-320">**Motivation:** die Folge der Einschränkung für Instanzfelder.</span><span class="sxs-lookup"><span data-stu-id="38223-320">**Motivation:** consequence of restriction on instance fields.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="38223-321">Metadatendarstellung.</span><span class="sxs-lookup"><span data-stu-id="38223-321">Metadata representation.</span></span>
<span data-ttu-id="38223-322">Wenn `System.Runtime.CompilerServices.IsReadOnlyAttribute` auf einen Werttyp angewendet wird, bedeutet dies, dass der Typ ein `readonly struct`ist.</span><span class="sxs-lookup"><span data-stu-id="38223-322">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a value type, it means that the type is a `readonly struct`.</span></span>

<span data-ttu-id="38223-323">Dies gilt insbesondere für:</span><span class="sxs-lookup"><span data-stu-id="38223-323">In particular:</span></span>
-  <span data-ttu-id="38223-324">Die Identität des `IsReadOnlyAttribute` Typs ist unwichtig.</span><span class="sxs-lookup"><span data-stu-id="38223-324">The identity of the `IsReadOnlyAttribute` type is unimportant.</span></span> <span data-ttu-id="38223-325">Sie kann bei Bedarf von dem Compiler in die enthaltende Assembly eingebettet werden.</span><span class="sxs-lookup"><span data-stu-id="38223-325">In fact it can be embedded by the compiler in the containing assembly if needed.</span></span>

## <a name="refin-extension-methods"></a><span data-ttu-id="38223-326">`ref`/`in`-Erweiterungs Methoden</span><span class="sxs-lookup"><span data-stu-id="38223-326">`ref`/`in` extension methods</span></span>
<span data-ttu-id="38223-327">Es gibt tatsächlich einen vorhandenen Vorschlag (https://github.com/dotnet/roslyn/issues/165) und den entsprechenden Prototype-PR (https://github.com/dotnet/roslyn/pull/15650).</span><span class="sxs-lookup"><span data-stu-id="38223-327">There is actually an existing proposal (https://github.com/dotnet/roslyn/issues/165) and corresponding prototype PR (https://github.com/dotnet/roslyn/pull/15650).</span></span>
<span data-ttu-id="38223-328">Ich möchte nur bestätigen, dass diese Idee nicht völlig neu ist.</span><span class="sxs-lookup"><span data-stu-id="38223-328">I just want to acknowledge that this idea is not entirely new.</span></span> <span data-ttu-id="38223-329">Dies ist jedoch hier von Bedeutung, da `ref readonly` das am häufigsten strittige Problem zu diesen Methoden entfernt, was mit Rvalue-Empfängern zu tun ist.</span><span class="sxs-lookup"><span data-stu-id="38223-329">It is, however, relevant here since `ref readonly` elegantly removes the most contentious issue about such methods - what to do with RValue receivers.</span></span>

<span data-ttu-id="38223-330">Die allgemeine Idee besteht darin, Erweiterungs Methoden zuzulassen, den `this` Parameter als Verweis zu verwenden, solange der Typ bekannt ist, dass es sich um einen Strukturtyp handelt.</span><span class="sxs-lookup"><span data-stu-id="38223-330">The general idea is allowing extension methods to take the `this` parameter by reference, as long as the type is known to be a struct type.</span></span>

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

<span data-ttu-id="38223-331">Die Gründe für das Schreiben solcher Erweiterungs Methoden sind hauptsächlich:</span><span class="sxs-lookup"><span data-stu-id="38223-331">The reasons for writing such extension methods are primarily:</span></span>  
1.  <span data-ttu-id="38223-332">Kopiervorgang vermeiden, wenn der Empfänger eine große Struktur ist</span><span class="sxs-lookup"><span data-stu-id="38223-332">Avoid copying when receiver is a large struct</span></span>
2.  <span data-ttu-id="38223-333">Verändernde Erweiterungs Methoden für Strukturen zulassen</span><span class="sxs-lookup"><span data-stu-id="38223-333">Allow mutating extension methods on structs</span></span>

<span data-ttu-id="38223-334">Die Gründe, warum wir dies nicht für Klassen zulassen möchten.</span><span class="sxs-lookup"><span data-stu-id="38223-334">The reasons why we do not want to allow this on classes</span></span>  
1.  <span data-ttu-id="38223-335">Dies wäre ein sehr eingeschränkter Zweck.</span><span class="sxs-lookup"><span data-stu-id="38223-335">It would be of very limited purpose.</span></span>
2.  <span data-ttu-id="38223-336">Es würde eine lange bestehende invariante unterbrechen, dass ein Methodenaufruf einen nicht`null` Empfänger nicht umwandeln kann, um nach dem Aufruf `null` werden zu können.</span><span class="sxs-lookup"><span data-stu-id="38223-336">It would break long standing invariant that a method call cannot turn non-`null` receiver to become `null` after invocation.</span></span>
> <span data-ttu-id="38223-337">Tatsächlich kann eine nicht`null` Variable nicht `null` werden, es sei denn, Sie wird _explizit_ von `ref` oder `out`zugewiesen oder übermittelt.</span><span class="sxs-lookup"><span data-stu-id="38223-337">In fact, currently a non-`null` variable cannot become `null` unless _explicitly_ assigned or passed by `ref` or `out`.</span></span>
> <span data-ttu-id="38223-338">Dadurch wird die Lesbarkeit oder andere Formen von "kann dies eine NULL hier-Analyse sein" erheblich unterstützt.</span><span class="sxs-lookup"><span data-stu-id="38223-338">That greatly aids readability or other forms of "can this be a null here" analysis.</span></span>
3.  <span data-ttu-id="38223-339">Es wäre schwierig, mit der Semantik "einmal auswerten" der Semantik von NULL-bedingten Zugriffen zu stimmen.</span><span class="sxs-lookup"><span data-stu-id="38223-339">It would be hard to reconcile with "evaluate once" semantics of null-conditional accesses.</span></span>
<span data-ttu-id="38223-340">Beispiel: `obj.stringField?.RefExtension(...)` müssen eine Kopie von `stringField` erfassen, um die NULL-Überprüfung aussagekräftig zu machen, aber dann werden Zuweisungen zum `this` in refextension nicht wieder in das Feld übernommen.</span><span class="sxs-lookup"><span data-stu-id="38223-340">Example: `obj.stringField?.RefExtension(...)` - need to capture a copy of `stringField` to make the null check meaningful, but then assignments to `this` inside RefExtension would not be reflected back to the field.</span></span>

<span data-ttu-id="38223-341">Eine Möglichkeit zum Deklarieren von Erweiterungs Methoden für **Strukturen** , die das erste Argument als Verweis akzeptieren, war eine langfristige Anforderung.</span><span class="sxs-lookup"><span data-stu-id="38223-341">An ability to declare extension methods on **structs** that take the first argument by reference was a long-standing request.</span></span> <span data-ttu-id="38223-342">Eine der blockierenden Aspekte lautete: "Was geschieht, wenn der Empfänger kein lvalue ist?".</span><span class="sxs-lookup"><span data-stu-id="38223-342">One of the blocking consideration was "what happens if receiver is not an LValue?".</span></span>

- <span data-ttu-id="38223-343">Es gibt einen Präzedenzfall, dass jede Erweiterungsmethode auch als statische Methode aufgerufen werden kann (manchmal ist Sie die einzige Möglichkeit, die Mehrdeutigkeit aufzulösen).</span><span class="sxs-lookup"><span data-stu-id="38223-343">There is a precedent that any extension method could also be called as a static method (sometimes it is the only way to resolve ambiguity).</span></span> <span data-ttu-id="38223-344">Es würde vorschreiben, dass Rvalue-Empfänger nicht zulässig sein sollten.</span><span class="sxs-lookup"><span data-stu-id="38223-344">It would dictate that RValue receivers should be disallowed.</span></span>
- <span data-ttu-id="38223-345">Andererseits ist es ratsam, in ähnlichen Situationen, in denen strukturinstanzmethoden beteiligt sind, einen Aufruf auf eine Kopie vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="38223-345">On the other hand there is a practice of making invocation on a copy in similar situations when struct instance methods are involved.</span></span>

<span data-ttu-id="38223-346">Der Grund, warum das implizite kopieren vorhanden ist, besteht darin, dass die Mehrzahl der Struktur Methoden nicht die Struktur tatsächlich ändert und nicht in der Lage ist, dies anzugeben.</span><span class="sxs-lookup"><span data-stu-id="38223-346">The reason why the "implicit copying" exists is because the majority of struct methods do not actually modify the struct while not being able to indicate that.</span></span> <span data-ttu-id="38223-347">Die praktischste Lösung bestand daher darin, den Aufruf auf eine Kopie zu übernehmen, aber diese Vorgehensweise ist bekannt, um die Leistung zu beeinträchtigen und Fehler zu verursachen.</span><span class="sxs-lookup"><span data-stu-id="38223-347">Therefore the most practical solution was to just make the invocation on a copy, but this practice is known for harming performance and causing bugs.</span></span>

<span data-ttu-id="38223-348">Mit der Verfügbarkeit von `in`-Parametern kann eine Erweiterung nun der Absicht signalisieren.</span><span class="sxs-lookup"><span data-stu-id="38223-348">Now, with availability of `in` parameters, it is possible for an extension to signal the intent.</span></span> <span data-ttu-id="38223-349">Daher kann das Problem gelöst werden, indem `ref` Erweiterungen mit beschreibbaren Empfängern aufgerufen werden müssen, während `in` Erweiterungen implizites kopieren bei Bedarf zulassen.</span><span class="sxs-lookup"><span data-stu-id="38223-349">Therefore the conundrum can be resolved by requiring `ref` extensions to be called with writeable receivers while `in` extensions permit implicit copying if necessary.</span></span>

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a><span data-ttu-id="38223-350">`in` Erweiterungen und Generika.</span><span class="sxs-lookup"><span data-stu-id="38223-350">`in` extensions and generics.</span></span>
<span data-ttu-id="38223-351">Der Zweck `ref` Erweiterungs Methoden besteht darin, den Empfänger direkt oder durch Aufrufen von mutierenden Membern zu mutieren.</span><span class="sxs-lookup"><span data-stu-id="38223-351">The purpose of `ref` extension methods is to mutate the receiver directly or by invoking mutating members.</span></span> <span data-ttu-id="38223-352">Daher sind `ref this T` Erweiterungen zulässig, solange `T` auf eine Struktur beschränkt ist.</span><span class="sxs-lookup"><span data-stu-id="38223-352">Therefore `ref this T` extensions are allowed as long as `T` is constrained to be a struct.</span></span>

<span data-ttu-id="38223-353">Auf der anderen Seite `in` Erweiterungs Methoden speziell vorhanden sind, um implizites kopieren zu verringern.</span><span class="sxs-lookup"><span data-stu-id="38223-353">On the other hand `in` extension methods exist specifically to reduce implicit copying.</span></span> <span data-ttu-id="38223-354">Allerdings muss die Verwendung eines `in T`-Parameters über einen Schnittstellenmember erfolgen.</span><span class="sxs-lookup"><span data-stu-id="38223-354">However any use of an `in T` parameter will have to be done through an interface member.</span></span> <span data-ttu-id="38223-355">Da alle Schnittstellenmember als muating angesehen werden, ist für jede solche Verwendung eine Kopie erforderlich.</span><span class="sxs-lookup"><span data-stu-id="38223-355">Since all interface members are considered mutating, any such use would require a copy.</span></span> <span data-ttu-id="38223-356">-Anstatt das Kopieren zu verringern, wäre der Effekt das Gegenteil.</span><span class="sxs-lookup"><span data-stu-id="38223-356">- Instead of reducing copying, the effect would be the opposite.</span></span> <span data-ttu-id="38223-357">Daher ist `in this T` nicht zulässig, wenn `T` ein generischer Typparameter ist, unabhängig von Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="38223-357">Therefore `in this T` is not allowed when `T` is a generic type parameter regardless of constraints.</span></span>

### <a name="valid-kinds-of-extension-methods-recap"></a><span data-ttu-id="38223-358">Gültige Arten von Erweiterungs Methoden (Recap):</span><span class="sxs-lookup"><span data-stu-id="38223-358">Valid kinds of extension methods (recap):</span></span>
<span data-ttu-id="38223-359">Die folgenden Formen der `this` Deklaration in einer Erweiterungsmethode sind jetzt zulässig:</span><span class="sxs-lookup"><span data-stu-id="38223-359">The following forms of `this` declaration in an extension method are now allowed:</span></span>
1) <span data-ttu-id="38223-360">`this T arg` reguläre ByVal-Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="38223-360">`this T arg` - regular byval extension.</span></span> <span data-ttu-id="38223-361">(**vorhandener Fall**)</span><span class="sxs-lookup"><span data-stu-id="38223-361">(**existing case**)</span></span>
- <span data-ttu-id="38223-362">"T" kann ein beliebiger Typ sein, einschließlich Verweis Typen oder Typparametern.</span><span class="sxs-lookup"><span data-stu-id="38223-362">T can be any type, including reference types or type parameters.</span></span>
<span data-ttu-id="38223-363">Die Instanz ist nach dem-Befehl dieselbe Variable.</span><span class="sxs-lookup"><span data-stu-id="38223-363">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="38223-364">Lässt implizite Konvertierungen _dieser Art von Argument Konvertierungen_ zu.</span><span class="sxs-lookup"><span data-stu-id="38223-364">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="38223-365">Kann für Rvalues aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="38223-365">Can be called on RValues.</span></span>

- <span data-ttu-id="38223-366">`in this T self` - `in` Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="38223-366">`in this T self` - `in` extension.</span></span>
<span data-ttu-id="38223-367">"T" muss ein tatsächlicher Strukturtyp sein.</span><span class="sxs-lookup"><span data-stu-id="38223-367">T must be an actual struct type.</span></span>
<span data-ttu-id="38223-368">Die Instanz ist nach dem-Befehl dieselbe Variable.</span><span class="sxs-lookup"><span data-stu-id="38223-368">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="38223-369">Lässt implizite Konvertierungen _dieser Art von Argument Konvertierungen_ zu.</span><span class="sxs-lookup"><span data-stu-id="38223-369">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="38223-370">Kann für Rvalues aufgerufen werden (kann bei Bedarf für eine Temp aufgerufen werden).</span><span class="sxs-lookup"><span data-stu-id="38223-370">Can be called on RValues (may be invoked on a temp if needed).</span></span>

- <span data-ttu-id="38223-371">`ref this T self` - `ref` Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="38223-371">`ref this T self` - `ref` extension.</span></span>
<span data-ttu-id="38223-372">T muss ein Strukturtyp oder ein generischer Typparameter sein, der als Struktur eingeschränkt ist.</span><span class="sxs-lookup"><span data-stu-id="38223-372">T must be a struct type or a generic type parameter constrained to be a struct.</span></span>
<span data-ttu-id="38223-373">Die-Instanz kann durch den Aufruf von geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="38223-373">Instance may be written to by the invocation.</span></span>
<span data-ttu-id="38223-374">Lässt nur Identitäts Konvertierungen zu.</span><span class="sxs-lookup"><span data-stu-id="38223-374">Allows only identity conversions.</span></span>
<span data-ttu-id="38223-375">Muss für beschreibbaren lvalue aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="38223-375">Must be called on writeable LValue.</span></span> <span data-ttu-id="38223-376">(nie über eine Temp aufgerufen).</span><span class="sxs-lookup"><span data-stu-id="38223-376">(never invoked via a temp).</span></span>

## <a name="readonly-ref-locals"></a><span data-ttu-id="38223-377">Schreibgeschützte lokale Ref-Variablen.</span><span class="sxs-lookup"><span data-stu-id="38223-377">Readonly ref locals.</span></span>

### <a name="motivation"></a><span data-ttu-id="38223-378">Ationen.</span><span class="sxs-lookup"><span data-stu-id="38223-378">Motivation.</span></span>
<span data-ttu-id="38223-379">Nachdem `ref readonly` Mitglieder eingeführt wurden, war es klar, dass Sie mit der passenden Art von lokalem paar kombiniert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="38223-379">Once `ref readonly` members were introduced, it was clear from the use that they need to be paired with appropriate kind of local.</span></span> <span data-ttu-id="38223-380">Durch die Auswertung eines Members können Nebeneffekte erzeugt oder beobachtet werden. wenn das Ergebnis mehrmals verwendet werden muss, muss es daher gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="38223-380">Evaluation of a member may produce or observe side effects, therefore if the result must be used more than once, it needs to be stored.</span></span> <span data-ttu-id="38223-381">Normale `ref` lokale unterstützen hier nicht, da Ihnen keine `readonly` Referenz zugewiesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="38223-381">Ordinary `ref` locals do not help here since they cannot be assigned a `readonly` reference.</span></span>   

### <a name="solution"></a><span data-ttu-id="38223-382">Not.</span><span class="sxs-lookup"><span data-stu-id="38223-382">Solution.</span></span>
<span data-ttu-id="38223-383">Ermöglicht das Deklarieren von `ref readonly` lokalen</span><span class="sxs-lookup"><span data-stu-id="38223-383">Allow declaring `ref readonly` locals.</span></span> <span data-ttu-id="38223-384">Dabei handelt es sich um eine neue Art von `ref` lokalen Variablen, die nicht geschrieben werden können.</span><span class="sxs-lookup"><span data-stu-id="38223-384">This is a new kind of `ref` locals that is not writeable.</span></span> <span data-ttu-id="38223-385">Daher können `ref readonly` lokale Verweise auf schreibgeschützte Variablen akzeptieren, ohne diese Variablen für Schreibvorgänge verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="38223-385">As a result `ref readonly` locals can accept references to readonly variables without exposing these variables to writes.</span></span>

### <a name="declaring-and-using-ref-readonly-locals"></a><span data-ttu-id="38223-386">Deklarieren und Verwenden von `ref readonly` lokalen Variablen.</span><span class="sxs-lookup"><span data-stu-id="38223-386">Declaring and using `ref readonly` locals.</span></span>

<span data-ttu-id="38223-387">Die Syntax dieser lokalen Variablen verwendet `ref readonly` Modifizierer an der Deklarations Site (in dieser bestimmten Reihenfolge).</span><span class="sxs-lookup"><span data-stu-id="38223-387">The syntax of such locals uses `ref readonly` modifiers at declaration site (in that specific order).</span></span> <span data-ttu-id="38223-388">Ähnlich wie bei den normalen `ref` lokalen Variablen müssen `ref readonly` lokalen Variablen bei der Deklaration als ref-initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="38223-388">Similarly to ordinary `ref` locals, `ref readonly` locals must be ref-initialized at declaration.</span></span> <span data-ttu-id="38223-389">Im Gegensatz zu regulären `ref` lokalen Variablen können `ref readonly` lokal auf `readonly` Lvalues verweisen, wie `in` Parameter, `readonly` Felder und `ref readonly` Methoden.</span><span class="sxs-lookup"><span data-stu-id="38223-389">Unlike regular `ref` locals, `ref readonly` locals can refer to `readonly` LValues like `in` parameters, `readonly` fields, `ref readonly` methods.</span></span>

<span data-ttu-id="38223-390">Für alle Zwecke wird ein `ref readonly` local als `readonly` Variable behandelt.</span><span class="sxs-lookup"><span data-stu-id="38223-390">For all purposes a `ref readonly` local is treated as a `readonly` variable.</span></span> <span data-ttu-id="38223-391">Die meisten Einschränkungen in Bezug auf die Verwendung sind identisch mit der Verwendung von `readonly` Feldern oder `in` Parametern.</span><span class="sxs-lookup"><span data-stu-id="38223-391">Most of the restrictions on the use are the same as with `readonly` fields or `in` parameters.</span></span>

<span data-ttu-id="38223-392">Beispielsweise werden Felder eines `in`-Parameters mit einem Strukturtyp rekursiv als `readonly` Variablen klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="38223-392">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a><span data-ttu-id="38223-393">Einschränkungen bei der Verwendung von `ref readonly` lokalen Variablen</span><span class="sxs-lookup"><span data-stu-id="38223-393">Restrictions on use of `ref readonly` locals</span></span>
<span data-ttu-id="38223-394">Mit Ausnahme der `readonly` Natur Verhalten sich `ref readonly` lokal wie normale `ref` lokale und unterliegen exakt denselben Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="38223-394">Except for their `readonly` nature, `ref readonly` locals behave like ordinary `ref` locals and are subject to exactly same restrictions.</span></span>  
<span data-ttu-id="38223-395">Beispielsweise gelten Einschränkungen im Zusammenhang mit der Erfassung von Abschlüssen, das Deklarieren in `async` Methoden oder die `safe-to-return` Analyse gleichermaßen für `ref readonly` lokale.</span><span class="sxs-lookup"><span data-stu-id="38223-395">For example restrictions related to capturing in closures, declaring in `async` methods or the `safe-to-return` analysis equally applies to `ref readonly` locals.</span></span>

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a><span data-ttu-id="38223-396">TERNÄRE `ref` Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="38223-396">Ternary `ref` expressions.</span></span> <span data-ttu-id="38223-397">(Alias "Conditional Lvalues")</span><span class="sxs-lookup"><span data-stu-id="38223-397">(aka "Conditional LValues")</span></span>

### <a name="motivation"></a><span data-ttu-id="38223-398">Motivation</span><span class="sxs-lookup"><span data-stu-id="38223-398">Motivation</span></span>
<span data-ttu-id="38223-399">Die Verwendung von `ref` und `ref readonly` lokalen Variablen gab an, dass solche lokalen Variablen auf der Grundlage einer Bedingung mit einer oder einer anderen Zielvariablen ref-initialisiert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="38223-399">Use of `ref` and `ref readonly` locals exposed a need to ref-initialize such locals with one or another target variable based on a condition.</span></span>

<span data-ttu-id="38223-400">Eine typische Problem Umgehung besteht darin, eine Methode wie die folgende einzuführen:</span><span class="sxs-lookup"><span data-stu-id="38223-400">A typical workaround is to introduce a method like:</span></span>

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

<span data-ttu-id="38223-401">Beachten Sie, dass `Choice` keine genaue Ersetzung eines ternären ist, da _alle_ Argumente an der aufrufssite ausgewertet werden müssen, was zu nicht intuitivem Verhalten und Fehlern geführt hat.</span><span class="sxs-lookup"><span data-stu-id="38223-401">Note that `Choice` is not an exact replacement of a ternary since _all_ arguments must be evaluated at the call site, which was leading to unintuitive behavior and bugs.</span></span>

<span data-ttu-id="38223-402">Folgendes funktioniert nicht wie erwartet:</span><span class="sxs-lookup"><span data-stu-id="38223-402">The following will not work as expected:</span></span>

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a><span data-ttu-id="38223-403">Lösung</span><span class="sxs-lookup"><span data-stu-id="38223-403">Solution</span></span>
<span data-ttu-id="38223-404">Hiermit wird eine spezielle Art von bedingtem Ausdruck zugelassen, die basierend auf einer Bedingung zu einem Verweis auf ein Lvalue-Argument ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="38223-404">Allow special kind of conditional expression that evaluates to a reference to one of LValue argument based on a condition.</span></span>

### <a name="using-ref-ternary-expression"></a><span data-ttu-id="38223-405">Verwenden `ref` ternären Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="38223-405">Using `ref` ternary expression.</span></span>

<span data-ttu-id="38223-406">Die Syntax für die `ref`-Konfiguration eines bedingten Ausdrucks ist `<condition> ? ref <consequence> : ref <alternative>;`</span><span class="sxs-lookup"><span data-stu-id="38223-406">The syntax for the `ref` flavor of a conditional expression is `<condition> ? ref <consequence> : ref <alternative>;`</span></span>

<span data-ttu-id="38223-407">Genau wie bei dem normalen bedingten Ausdruck nur `<consequence>` oder `<alternative>` wird in Abhängigkeit vom Ergebnis des Ausdrucks der booleschen Bedingung ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="38223-407">Just like with the ordinary conditional expression only `<consequence>` or `<alternative>` is evaluated depending on result of the boolean condition expression.</span></span>

<span data-ttu-id="38223-408">Im Gegensatz zum normalen bedingten Ausdruck `ref` Bedingungs Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="38223-408">Unlike ordinary conditional expression, `ref` conditional expression:</span></span>
- <span data-ttu-id="38223-409">erfordert, dass `<consequence>` und `<alternative>` Lvalues sind.</span><span class="sxs-lookup"><span data-stu-id="38223-409">requires that `<consequence>` and `<alternative>` are LValues.</span></span>
- <span data-ttu-id="38223-410">`ref` bedingte Ausdruck selbst ist ein Lvalue und</span><span class="sxs-lookup"><span data-stu-id="38223-410">`ref` conditional expression itself is an LValue and</span></span>
- <span data-ttu-id="38223-411">`ref` bedingte Ausdruck ist beschreibbar, wenn sowohl `<consequence>` als auch `<alternative>` beschreibbare Lvalues sind.</span><span class="sxs-lookup"><span data-stu-id="38223-411">`ref` conditional expression is writeable if both `<consequence>` and `<alternative>` are writeable LValues</span></span>

<span data-ttu-id="38223-412">Beispiele:</span><span class="sxs-lookup"><span data-stu-id="38223-412">Examples:</span></span>  
<span data-ttu-id="38223-413">`ref` ternäre ist ein Lvalue und kann daher übermittelt/zugewiesen/als Verweis zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="38223-413">`ref` ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="38223-414">Dabei kann es sich um einen lvalue-Wert handeln, der ebenfalls zugewiesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="38223-414">Being an LValue, it can also be assigned to.</span></span>
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

<span data-ttu-id="38223-415">Kann als Empfänger eines Methoden Aufrufes verwendet werden und das Kopieren bei Bedarf überspringen.</span><span class="sxs-lookup"><span data-stu-id="38223-415">Can be used as a receiver of a method call and skip copying if necessary.</span></span>
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

<span data-ttu-id="38223-416">`ref` ternäre kann auch in einem regulären (not Ref) Kontext verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="38223-416">`ref` ternary can be used in a regular (not ref) context as well.</span></span>
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a><span data-ttu-id="38223-417">Nachteile</span><span class="sxs-lookup"><span data-stu-id="38223-417">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="38223-418">Ich kann zwei wichtige Argumente für erweiterte Unterstützung für Verweise und schreibgeschützte Verweise sehen:</span><span class="sxs-lookup"><span data-stu-id="38223-418">I can see two major arguments against enhanced support for references and readonly references:</span></span>

1) <span data-ttu-id="38223-419">Die hier gelösten Probleme sind sehr alt.</span><span class="sxs-lookup"><span data-stu-id="38223-419">The problems that are solved here are very old.</span></span> <span data-ttu-id="38223-420">Warum lösen Sie diese jetzt plötzlich, besonders weil Sie vorhandenen Code nicht unterstützen würde?</span><span class="sxs-lookup"><span data-stu-id="38223-420">Why suddenly solve them now, especially since it would not help existing code?</span></span>

<span data-ttu-id="38223-421">Wie wir sehen C# , und .net in neuen Domänen verwendet wird, werden einige Probleme deutlicher.</span><span class="sxs-lookup"><span data-stu-id="38223-421">As we find C# and .Net used in new domains, some problems become more prominent.</span></span>  
<span data-ttu-id="38223-422">Als Beispiele für Umgebungen, die kritischer als der Durchschnitt bei Berechnungs überschreitungs Werten sind, kann ich</span><span class="sxs-lookup"><span data-stu-id="38223-422">As examples of environments that are more critical than average about computation overheads, I can list</span></span>

* <span data-ttu-id="38223-423">Cloud-/Datacenter-Szenarios, in denen die Berechnung abgerechnet wird und die Reaktionsfähigkeit einen Wettbewerbsvorteil ist.</span><span class="sxs-lookup"><span data-stu-id="38223-423">cloud/datacenter scenarios where computation is billed for and responsiveness is a competitive advantage.</span></span>
* <span data-ttu-id="38223-424">Spiele/VR/AR mit Soft-Echtzeitanforderungen bei Latenzen</span><span class="sxs-lookup"><span data-stu-id="38223-424">Games/VR/AR with soft-realtime requirements on latencies</span></span>     

<span data-ttu-id="38223-425">Mit diesem Feature werden keine der vorhandenen stärken, wie z. b. Typsicherheit, geopfert, während einige gängige Szenarien einen geringeren Aufwand ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="38223-425">This feature does not sacrifice any of the existing strengths such as type-safety, while allowing to lower overheads in some common scenarios.</span></span>

2) <span data-ttu-id="38223-426">Können wir in angemessener Weise sicherstellen, dass der aufgerufene die Regeln wieder gibt, wenn er sich für `readonly` Verträge entscheidet?</span><span class="sxs-lookup"><span data-stu-id="38223-426">Can we reasonably guarantee that the callee will play by the rules when it opts into `readonly` contracts?</span></span>

<span data-ttu-id="38223-427">Bei der Verwendung `out`haben wir eine vergleichbare Vertrauensstellung.</span><span class="sxs-lookup"><span data-stu-id="38223-427">We have similar trust when using `out`.</span></span> <span data-ttu-id="38223-428">Eine falsche Implementierung von `out` kann ein nicht bestimmtes Verhalten verursachen, aber in der Realität kommt es selten vor.</span><span class="sxs-lookup"><span data-stu-id="38223-428">Incorrect implementation of `out` can cause unspecified behavior, but in reality it rarely happens.</span></span>  

<span data-ttu-id="38223-429">Wenn die formalen Überprüfungs Regeln, die mit `ref readonly` vertraut sind, das Vertrauensstellungs Problem weiter verringern.</span><span class="sxs-lookup"><span data-stu-id="38223-429">Making the formal verification rules familiar with `ref readonly` would further mitigate the trust issue.</span></span>

### <a name="alternatives"></a><span data-ttu-id="38223-430">Alternativen</span><span class="sxs-lookup"><span data-stu-id="38223-430">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="38223-431">Der wichtigste konkurrierende Entwurf ist wirklich "Do Nothing".</span><span class="sxs-lookup"><span data-stu-id="38223-431">The main competing design is really "do nothing".</span></span>

### <a name="unresolved-questions"></a><span data-ttu-id="38223-432">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="38223-432">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a><span data-ttu-id="38223-433">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="38223-433">Design meetings</span></span>

<span data-ttu-id="38223-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span><span class="sxs-lookup"><span data-stu-id="38223-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span></span>
