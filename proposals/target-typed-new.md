---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217202"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="de38c-101">`new` Ausdrücke mit Ziel Typisierung</span><span class="sxs-lookup"><span data-stu-id="de38c-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="de38c-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="de38c-102">[x] Proposed</span></span>
* <span data-ttu-id="de38c-103">[x] [Prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="de38c-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="de38c-104">[]-Implementierung</span><span class="sxs-lookup"><span data-stu-id="de38c-104">[ ] Implementation</span></span>
* <span data-ttu-id="de38c-105">[]-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="de38c-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="de38c-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="de38c-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="de38c-107">Sie benötigen keine Typspezifikation für Konstruktoren, wenn der Typ bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="de38c-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="de38c-108">Motivation</span><span class="sxs-lookup"><span data-stu-id="de38c-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="de38c-109">Ermöglicht die Feld Initialisierung ohne Duplizieren des Typs.</span><span class="sxs-lookup"><span data-stu-id="de38c-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="de38c-110">Lassen Sie den Typ weglassen aus, wenn er von der Verwendung abgeleitet werden kann.</span><span class="sxs-lookup"><span data-stu-id="de38c-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="de38c-111">Instanziieren Sie ein-Objekt, ohne den Typ zu benennen.</span><span class="sxs-lookup"><span data-stu-id="de38c-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="de38c-112">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="de38c-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="de38c-113">Die *object_creation_expression* Syntax wird so geändert, dass der *Typ* optional ist, wenn Klammern vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="de38c-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="de38c-114">Dies ist erforderlich, um die Mehrdeutigkeit mit *anonymous_object_creation_expression*zu beheben.</span><span class="sxs-lookup"><span data-stu-id="de38c-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="de38c-115">Ein mit dem Ziel typisiertes `new` kann in einen beliebigen Typ konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="de38c-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="de38c-116">Dies hat zur Folge, dass Sie nicht zur Überladungs Auflösung beiträgt.</span><span class="sxs-lookup"><span data-stu-id="de38c-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="de38c-117">Dies dient vor allem dazu, unvorhersehbare wichtige Änderungen zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="de38c-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="de38c-118">Die Argumentliste und die initialisiererausdrücke werden gebunden, nachdem der Typ bestimmt wurde.</span><span class="sxs-lookup"><span data-stu-id="de38c-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="de38c-119">Der Typ des Ausdrucks wird vom Zieltyp abgeleitet, der einen der folgenden Werte benötigen würde:</span><span class="sxs-lookup"><span data-stu-id="de38c-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="de38c-120">**Beliebiger Strukturtyp** (einschließlich Tupeltypen)</span><span class="sxs-lookup"><span data-stu-id="de38c-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="de38c-121">**Beliebiger Verweistyp** (einschließlich Delegattypen)</span><span class="sxs-lookup"><span data-stu-id="de38c-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="de38c-122">Ein **beliebiger Typparameter** mit einem Konstruktor oder einer `struct`-Einschränkung</span><span class="sxs-lookup"><span data-stu-id="de38c-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="de38c-123">mit den folgenden Ausnahmen:</span><span class="sxs-lookup"><span data-stu-id="de38c-123">with the following exceptions:</span></span>

- <span data-ttu-id="de38c-124">**Enumerationstypen:** nicht alle Enumerationstypen enthalten die Konstante NULL. Daher sollte es wünschenswert sein, den expliziten Enumerationsmember zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="de38c-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="de38c-125">**Schnittstellentypen:** Dies ist eine Nische Funktion, und es sollte besser sein, den Typ explizit zu erwähnen.</span><span class="sxs-lookup"><span data-stu-id="de38c-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="de38c-126">**Array Typen:** Arrays benötigen eine spezielle Syntax, um die Länge bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="de38c-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="de38c-127">**dynamisch:** wir lassen `new dynamic()`nicht zu, daher lassen wir `new()` mit `dynamic` nicht als Zieltyp zu.</span><span class="sxs-lookup"><span data-stu-id="de38c-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="de38c-128">Alle anderen Typen, die in der *object_creation_expression* nicht zulässig sind, werden ebenfalls ausgeschlossen, z.b. Zeiger Typen.</span><span class="sxs-lookup"><span data-stu-id="de38c-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="de38c-129">Wenn der Zieltyp ein auf NULL festleg barer Werttyp ist, wird der `new` vom Typ Target in den zugrunde liegenden Typ und nicht in den Typ konvertiert, der NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="de38c-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="de38c-130">**Open Issue:** sollten wir Delegaten und Tupel als Zieltyp zulassen?</span><span class="sxs-lookup"><span data-stu-id="de38c-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="de38c-131">Die oben genannten Regeln enthalten Delegaten (einen Verweistyp) und Tupel (einen Strukturtyp).</span><span class="sxs-lookup"><span data-stu-id="de38c-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="de38c-132">Obwohl beide Typen konstruiert werden können, kann die Verwendung einer anonymen Funktion oder eines tupelliterals bereits verwendet werden, wenn der Typ abgeleitet werden kann.</span><span class="sxs-lookup"><span data-stu-id="de38c-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="de38c-133">Verschiedenes</span><span class="sxs-lookup"><span data-stu-id="de38c-133">Miscellaneous</span></span>

<span data-ttu-id="de38c-134">`throw new()` ist nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="de38c-134">`throw new()` is disallowed.</span></span>

<span data-ttu-id="de38c-135">Mit dem Ziel typisierte `new` ist bei binären Operatoren nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="de38c-135">Target-typed `new` is not allowed with binary operators.</span></span>

<span data-ttu-id="de38c-136">Es ist nicht zulässig, wenn kein Typ zum Ziel vorhanden ist: unäre Operatoren, Auflistung eines `foreach`in einer `using`in einer Dekonstruktion in einem `await` Ausdruck als anonyme Typeigenschaft (`new { Prop = new() }`) in einer `lock`-Anweisung in einer `sizeof`in einer `fixed`-Anweisung in einem Element Zugriff (`new().field`) in einem dynamisch verteilten Vorgang (`someDynamic.Method(new())`) in einer LINQ-Abfrage als Operand des `is` Operators, als Linker Operand des `??`-Operators. ,  ...</span><span class="sxs-lookup"><span data-stu-id="de38c-136">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>

<span data-ttu-id="de38c-137">Dies ist auch als `ref`unzulässig.</span><span class="sxs-lookup"><span data-stu-id="de38c-137">It is also disallowed as a `ref`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="de38c-138">Nachteile</span><span class="sxs-lookup"><span data-stu-id="de38c-138">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="de38c-139">Es gab einige Probleme mit Ziel typisierten `new` das Erstellen neuer Kategorien von wichtigen Änderungen, aber wir haben dies bereits mit `null` und `default`, und das ist kein erhebliches Problem.</span><span class="sxs-lookup"><span data-stu-id="de38c-139">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="de38c-140">Alternativen</span><span class="sxs-lookup"><span data-stu-id="de38c-140">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="de38c-141">Die meisten Beschwerden über Typen, die zu lang sind, um bei der Feld Initialisierung duplizieren zu können, sind *Typargumente* , die nicht den Typ selbst aufweisen. wir könnten nur Typargumente wie `new Dictionary(...)` (oder ähnliches) ableiten und Typargumente lokal von Argumenten oder dem auflistungsiniti</span><span class="sxs-lookup"><span data-stu-id="de38c-141">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="de38c-142">Fragen</span><span class="sxs-lookup"><span data-stu-id="de38c-142">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="de38c-143">Sollten wir die Verwendung von Ausdrücken in Ausdrucks Baumstrukturen verbieten?</span><span class="sxs-lookup"><span data-stu-id="de38c-143">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="de38c-144">gar</span><span class="sxs-lookup"><span data-stu-id="de38c-144">(no)</span></span>
- <span data-ttu-id="de38c-145">Wie wird das Feature mit `dynamic` Argumenten interagiert?</span><span class="sxs-lookup"><span data-stu-id="de38c-145">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="de38c-146">(keine besondere Behandlung)</span><span class="sxs-lookup"><span data-stu-id="de38c-146">(no special treatment)</span></span>
- <span data-ttu-id="de38c-147">Wie funktioniert IntelliSense mit `new()`?</span><span class="sxs-lookup"><span data-stu-id="de38c-147">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="de38c-148">(nur bei einem einzelnen Zieltyp)</span><span class="sxs-lookup"><span data-stu-id="de38c-148">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="de38c-149">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="de38c-149">Design meetings</span></span>

- [<span data-ttu-id="de38c-150">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="de38c-150">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="de38c-151">LDM-2018-05-21</span><span class="sxs-lookup"><span data-stu-id="de38c-151">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="de38c-152">LDM-2018-06-25</span><span class="sxs-lookup"><span data-stu-id="de38c-152">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="de38c-153">LDM-2018-08-22</span><span class="sxs-lookup"><span data-stu-id="de38c-153">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="de38c-154">LDM-2018-10-17</span><span class="sxs-lookup"><span data-stu-id="de38c-154">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
