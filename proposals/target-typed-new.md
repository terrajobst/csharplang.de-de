---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483529"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="36e72-101">`new` Ausdrücke mit Ziel Typisierung</span><span class="sxs-lookup"><span data-stu-id="36e72-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="36e72-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="36e72-102">[x] Proposed</span></span>
* <span data-ttu-id="36e72-103">[x] [Prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="36e72-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="36e72-104">[]-Implementierung</span><span class="sxs-lookup"><span data-stu-id="36e72-104">[ ] Implementation</span></span>
* <span data-ttu-id="36e72-105">[]-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="36e72-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="36e72-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="36e72-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="36e72-107">Sie benötigen keine Typspezifikation für Konstruktoren, wenn der Typ bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="36e72-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="36e72-108">Motivation</span><span class="sxs-lookup"><span data-stu-id="36e72-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="36e72-109">Ermöglicht die Feld Initialisierung ohne Duplizieren des Typs.</span><span class="sxs-lookup"><span data-stu-id="36e72-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
<span data-ttu-id="36e72-110">Lassen Sie den Typ weglassen aus, wenn er von der Verwendung abgeleitet werden kann.</span><span class="sxs-lookup"><span data-stu-id="36e72-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
<span data-ttu-id="36e72-111">Instanziieren Sie ein-Objekt, ohne den Typ zu benennen.</span><span class="sxs-lookup"><span data-stu-id="36e72-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a><span data-ttu-id="36e72-112">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="36e72-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="36e72-113">Die *object_creation_expression* Syntax wird so geändert, dass der *Typ* optional ist, wenn Klammern vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="36e72-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="36e72-114">Dies ist erforderlich, um die Mehrdeutigkeit mit *anonymous_object_creation_expression*zu beheben.</span><span class="sxs-lookup"><span data-stu-id="36e72-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
<span data-ttu-id="36e72-115">Ein mit dem Ziel typisiertes `new` kann in einen beliebigen Typ konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="36e72-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="36e72-116">Dies hat zur Folge, dass Sie nicht zur Überladungs Auflösung beiträgt.</span><span class="sxs-lookup"><span data-stu-id="36e72-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="36e72-117">Dies dient vor allem dazu, unvorhersehbare wichtige Änderungen zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="36e72-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="36e72-118">Die Argumentliste und die initialisiererausdrücke werden gebunden, nachdem der Typ bestimmt wurde.</span><span class="sxs-lookup"><span data-stu-id="36e72-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="36e72-119">Der Typ des Ausdrucks wird vom Zieltyp abgeleitet, der einen der folgenden Werte benötigen würde:</span><span class="sxs-lookup"><span data-stu-id="36e72-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="36e72-120">**Beliebiger Strukturtyp**</span><span class="sxs-lookup"><span data-stu-id="36e72-120">**Any struct type**</span></span>
- <span data-ttu-id="36e72-121">**Beliebiger Verweistyp**</span><span class="sxs-lookup"><span data-stu-id="36e72-121">**Any reference type**</span></span>
- <span data-ttu-id="36e72-122">Ein **beliebiger Typparameter** mit einem Konstruktor oder einer `struct`-Einschränkung</span><span class="sxs-lookup"><span data-stu-id="36e72-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="36e72-123">mit den folgenden Ausnahmen:</span><span class="sxs-lookup"><span data-stu-id="36e72-123">with the following exceptions:</span></span>

- <span data-ttu-id="36e72-124">**Enumerationstypen:** nicht alle Enumerationstypen enthalten die Konstante NULL. Daher sollte es wünschenswert sein, den expliziten Enumerationsmember zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="36e72-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="36e72-125">**Schnittstellentypen:** Dies ist eine Nische Funktion, und es sollte besser sein, den Typ explizit zu erwähnen.</span><span class="sxs-lookup"><span data-stu-id="36e72-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="36e72-126">**Array Typen:** Arrays benötigen eine spezielle Syntax, um die Länge bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="36e72-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="36e72-127">**Strukturstandardkonstruktor**: Hierdurch werden alle primitiven Typen und die meisten Werttypen Regeln.</span><span class="sxs-lookup"><span data-stu-id="36e72-127">**Struct default constructor**: this rules out all primitive types and most value types.</span></span> <span data-ttu-id="36e72-128">Wenn Sie den Standardwert dieser Typen verwenden möchten, können Sie stattdessen `default` schreiben.</span><span class="sxs-lookup"><span data-stu-id="36e72-128">If you wanted to use the default value of such types you could write `default` instead.</span></span>

<span data-ttu-id="36e72-129">Alle anderen Typen, die in der *object_creation_expression* nicht zulässig sind, werden ebenfalls ausgeschlossen, z.b. Zeiger Typen.</span><span class="sxs-lookup"><span data-stu-id="36e72-129">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

> <span data-ttu-id="36e72-130">**Open Issue:** sollten wir Delegaten und Tupel als Zieltyp zulassen?</span><span class="sxs-lookup"><span data-stu-id="36e72-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="36e72-131">Die oben genannten Regeln enthalten Delegaten (einen Verweistyp) und Tupel (einen Strukturtyp).</span><span class="sxs-lookup"><span data-stu-id="36e72-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="36e72-132">Obwohl beide Typen konstruiert werden können, kann die Verwendung einer anonymen Funktion oder eines tupelliterals bereits verwendet werden, wenn der Typ abgeleitet werden kann.</span><span class="sxs-lookup"><span data-stu-id="36e72-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> <span data-ttu-id="36e72-133">**Open Issue:** sollten wir `throw new()` mit `Exception` als Zieltyp zulassen?</span><span class="sxs-lookup"><span data-stu-id="36e72-133">**Open Issue:** should we allow `throw new()` with `Exception` as the target-type?</span></span>

<span data-ttu-id="36e72-134">Wir haben heute `throw null`, aber nicht `throw default` (obwohl dies die gleiche Wirkung haben würde).</span><span class="sxs-lookup"><span data-stu-id="36e72-134">We have `throw null` today, but not `throw default` (though it would have the same effect).</span></span> <span data-ttu-id="36e72-135">Auf der anderen Seite könnten `throw new()` als Kurzform für `throw new Exception(...)`nützlich sein.</span><span class="sxs-lookup"><span data-stu-id="36e72-135">On the other hand, `throw new()` could be actually useful as a shorthand for `throw new Exception(...)`.</span></span> <span data-ttu-id="36e72-136">Beachten Sie, dass Sie bereits von der aktuellen Spezifikation zugelassen wird.</span><span class="sxs-lookup"><span data-stu-id="36e72-136">Note that it is already allowed by the current specification.</span></span> <span data-ttu-id="36e72-137">`Exception` ist ein Referenztyp, und die Spezifikation für die throw-Anweisung besagt, dass der Ausdruck in `Exception`konvertiert wird.</span><span class="sxs-lookup"><span data-stu-id="36e72-137">`Exception` is a reference type, and the specification for the throw statement says that the expression is converted to `Exception`.</span></span>

> <span data-ttu-id="36e72-138">**Open Issue:** sollten wir die Verwendung einer Ziel typisierten `new` mit benutzerdefinierten Vergleichs-und arithmetischen Operatoren zulassen?</span><span class="sxs-lookup"><span data-stu-id="36e72-138">**Open Issue:** should we allow usages of a target-typed `new` with user-defined comparison and arithmetic operators?</span></span>

<span data-ttu-id="36e72-139">Zum Vergleich unterstützt `default` nur Gleichheits Operatoren (benutzerdefinierte und integrierte).</span><span class="sxs-lookup"><span data-stu-id="36e72-139">For comparison, `default` only supports equality (user-defined and built-in) operators.</span></span> <span data-ttu-id="36e72-140">Wäre es sinnvoll, auch andere Operatoren für die `new()` zu unterstützen?</span><span class="sxs-lookup"><span data-stu-id="36e72-140">Would it make sense to support other operators for `new()` as well?</span></span>

## <a name="drawbacks"></a><span data-ttu-id="36e72-141">Nachteile</span><span class="sxs-lookup"><span data-stu-id="36e72-141">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="36e72-142">None.</span><span class="sxs-lookup"><span data-stu-id="36e72-142">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="36e72-143">Alternativen</span><span class="sxs-lookup"><span data-stu-id="36e72-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="36e72-144">Die meisten Beschwerden über Typen, die zu lang sind, um bei der Feld Initialisierung duplizieren zu können, sind *Typargumente* , die nicht den Typ selbst aufweisen. wir könnten nur Typargumente wie `new Dictionary(...)` (oder ähnliches) ableiten und Typargumente lokal von Argumenten oder dem auflistungsiniti</span><span class="sxs-lookup"><span data-stu-id="36e72-144">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="36e72-145">Fragen</span><span class="sxs-lookup"><span data-stu-id="36e72-145">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="36e72-146">Sollten wir die Verwendung von Ausdrücken in Ausdrucks Baumstrukturen verbieten?</span><span class="sxs-lookup"><span data-stu-id="36e72-146">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="36e72-147">gar</span><span class="sxs-lookup"><span data-stu-id="36e72-147">(no)</span></span>
- <span data-ttu-id="36e72-148">Wie wird das Feature mit `dynamic` Argumenten interagiert?</span><span class="sxs-lookup"><span data-stu-id="36e72-148">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="36e72-149">(keine besondere Behandlung)</span><span class="sxs-lookup"><span data-stu-id="36e72-149">(no special treatment)</span></span>
- <span data-ttu-id="36e72-150">Wie funktioniert IntelliSense mit `new()`?</span><span class="sxs-lookup"><span data-stu-id="36e72-150">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="36e72-151">(nur bei einem einzelnen Zieltyp)</span><span class="sxs-lookup"><span data-stu-id="36e72-151">(only when there is a single target-type)</span></span>
## <a name="design-meetings"></a><span data-ttu-id="36e72-152">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="36e72-152">Design meetings</span></span>

- [<span data-ttu-id="36e72-153">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="36e72-153">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
