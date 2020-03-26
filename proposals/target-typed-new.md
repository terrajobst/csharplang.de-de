---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281969"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="cfdef-101">`new` Ausdrücke mit Ziel Typisierung</span><span class="sxs-lookup"><span data-stu-id="cfdef-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="cfdef-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="cfdef-102">[x] Proposed</span></span>
* <span data-ttu-id="cfdef-103">[x] [Prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="cfdef-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="cfdef-104">[]-Implementierung</span><span class="sxs-lookup"><span data-stu-id="cfdef-104">[ ] Implementation</span></span>
* <span data-ttu-id="cfdef-105">[]-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="cfdef-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="cfdef-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="cfdef-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="cfdef-107">Sie benötigen keine Typspezifikation für Konstruktoren, wenn der Typ bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="cfdef-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="cfdef-108">Motivation</span><span class="sxs-lookup"><span data-stu-id="cfdef-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="cfdef-109">Ermöglicht die Feld Initialisierung ohne Duplizieren des Typs.</span><span class="sxs-lookup"><span data-stu-id="cfdef-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="cfdef-110">Lassen Sie den Typ weglassen aus, wenn er von der Verwendung abgeleitet werden kann.</span><span class="sxs-lookup"><span data-stu-id="cfdef-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="cfdef-111">Instanziieren Sie ein-Objekt, ohne den Typ zu benennen.</span><span class="sxs-lookup"><span data-stu-id="cfdef-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="specification"></a><span data-ttu-id="cfdef-112">Spezifikation</span><span class="sxs-lookup"><span data-stu-id="cfdef-112">Specification</span></span>
[design]: #detailed-design

<span data-ttu-id="cfdef-113">Eine neue syntaktische Form, *target_typed_new* des *object_creation_expression* wird akzeptiert, in der der *Typ* optional ist.</span><span class="sxs-lookup"><span data-stu-id="cfdef-113">A new syntactic form, *target_typed_new* of the *object_creation_expression* is accepted in which the *type* is optional.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

<span data-ttu-id="cfdef-114">Ein *target_typed_new* Ausdruck weist keinen Typ auf.</span><span class="sxs-lookup"><span data-stu-id="cfdef-114">A *target_typed_new* expression does not have a type.</span></span> <span data-ttu-id="cfdef-115">Es gibt jedoch eine neue *Konvertierung der Objekt Erstellung* , bei der es sich um eine implizite Konvertierung von Expression handelt, die von einer *target_typed_new* zu jedem Typ vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="cfdef-115">However, there is a new *object creation conversion* that is an implicit conversion from expression, that exists from a *target_typed_new* to every type.</span></span>

<span data-ttu-id="cfdef-116">Wenn ein Zieltyp `T`ist, ist der Typ `T0` `T`zugrunde liegende Typ, wenn `T` eine Instanz von `System.Nullable`ist.</span><span class="sxs-lookup"><span data-stu-id="cfdef-116">Given a target type `T`, the type `T0` is `T`'s underlying type if `T` is an instance of `System.Nullable`.</span></span> <span data-ttu-id="cfdef-117">Andernfalls wird `T0` `T`.</span><span class="sxs-lookup"><span data-stu-id="cfdef-117">Otherwise `T0` is `T`.</span></span> <span data-ttu-id="cfdef-118">Die Bedeutung eines *target_typed_new* Ausdrucks, der in den-Typ konvertiert wird `T` entspricht der Bedeutung einer entsprechenden *object_creation_expression* , die `T0` als Typ angibt.</span><span class="sxs-lookup"><span data-stu-id="cfdef-118">The meaning of a *target_typed_new* expression that is converted to the type `T` is the same as the meaning of a corresponding *object_creation_expression* that specifies `T0` as the type.</span></span>

<span data-ttu-id="cfdef-119">Es handelt sich um einen Kompilierzeitfehler, wenn eine *target_typed_new* als Operand eines unären oder binären Operators verwendet wird, oder wenn Sie verwendet wird, wenn Sie nicht einer *Objekt Erstellungs Konvertierung*unterliegt.</span><span class="sxs-lookup"><span data-stu-id="cfdef-119">It is a compile-time error if a *target_typed_new* is used as an operand of a unary or binary operator, or if it is used where it is not subject to an *object creation conversion*.</span></span>

> <span data-ttu-id="cfdef-120">**Open Issue:** sollten wir Delegaten und Tupel als Zieltyp zulassen?</span><span class="sxs-lookup"><span data-stu-id="cfdef-120">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="cfdef-121">Die oben genannten Regeln enthalten Delegaten (einen Verweistyp) und Tupel (einen Strukturtyp).</span><span class="sxs-lookup"><span data-stu-id="cfdef-121">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="cfdef-122">Obwohl beide Typen konstruiert werden können, kann die Verwendung einer anonymen Funktion oder eines tupelliterals bereits verwendet werden, wenn der Typ abgeleitet werden kann.</span><span class="sxs-lookup"><span data-stu-id="cfdef-122">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="cfdef-123">Sonstiges</span><span class="sxs-lookup"><span data-stu-id="cfdef-123">Miscellaneous</span></span>

<span data-ttu-id="cfdef-124">Die Spezifikation hat folgende Konsequenzen:</span><span class="sxs-lookup"><span data-stu-id="cfdef-124">The following are consequences of the specification:</span></span>

- <span data-ttu-id="cfdef-125">`throw new()` ist zulässig (der Zieltyp ist `System.Exception`).</span><span class="sxs-lookup"><span data-stu-id="cfdef-125">`throw new()` is allowed (the target type is `System.Exception`)</span></span>
- <span data-ttu-id="cfdef-126">Mit dem Ziel typisierte `new` ist bei binären Operatoren nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="cfdef-126">Target-typed `new` is not allowed with binary operators.</span></span>
- <span data-ttu-id="cfdef-127">Es ist nicht zulässig, wenn kein Typ zum Ziel vorhanden ist: unäre Operatoren, Auflistung eines `foreach`in einer `using`in einer Dekonstruktion in einem `await` Ausdruck als anonyme Typeigenschaft (`new { Prop = new() }`) in einer `lock`-Anweisung in einer `sizeof`in einer `fixed`-Anweisung in einem Element Zugriff (`new().field`) in einem dynamisch verteilten Vorgang (`someDynamic.Method(new())`) in einer LINQ-Abfrage als Operand des `is` Operators, als Linker Operand des `??`-Operators. ,  ...</span><span class="sxs-lookup"><span data-stu-id="cfdef-127">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>
- <span data-ttu-id="cfdef-128">Dies ist auch als `ref`unzulässig.</span><span class="sxs-lookup"><span data-stu-id="cfdef-128">It is also disallowed as a `ref`.</span></span>
- <span data-ttu-id="cfdef-129">Die folgenden Arten von Typen sind nicht als Ziele der Konvertierung zulässig.</span><span class="sxs-lookup"><span data-stu-id="cfdef-129">The following kinds of types are not permitted as targets of the conversion</span></span>
  - <span data-ttu-id="cfdef-130">**Enumerationstypen:** `new()` funktioniert (wie `new Enum()`, um den Standardwert zu verwenden), aber `new(1)` funktioniert nicht, weil Enumerationstypen keinen Konstruktor aufweisen.</span><span class="sxs-lookup"><span data-stu-id="cfdef-130">**Enum types:** `new()` will work (as `new Enum()` works to give the default value), but `new(1)` will not work as enum types do not have a constructor.</span></span>
  - <span data-ttu-id="cfdef-131">**Schnittstellentypen:** Dies funktioniert genauso wie der entsprechende Erstellungs Ausdruck für COM-Typen.</span><span class="sxs-lookup"><span data-stu-id="cfdef-131">**Interface types:** This would work the same as the corresponding creation expression for COM types.</span></span>
  - <span data-ttu-id="cfdef-132">**Array Typen:** Arrays benötigen eine spezielle Syntax, um die Länge bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="cfdef-132">**Array types:** arrays need a special syntax to provide the length.</span></span>    
  - <span data-ttu-id="cfdef-133">**dynamisch:** wir lassen `new dynamic()`nicht zu, daher lassen wir `new()` mit `dynamic` nicht als Zieltyp zu.</span><span class="sxs-lookup"><span data-stu-id="cfdef-133">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>
  - <span data-ttu-id="cfdef-134">**Tupel:** Diese haben dieselbe Bedeutung wie eine Objekt Erstellung mit dem zugrunde liegenden Typ.</span><span class="sxs-lookup"><span data-stu-id="cfdef-134">**tuples:** These have the same meaning as an object creation using the underlying type.</span></span>
  - <span data-ttu-id="cfdef-135">Alle anderen Typen, die in der *object_creation_expression* nicht zulässig sind, werden ebenfalls ausgeschlossen, z.b. Zeiger Typen.</span><span class="sxs-lookup"><span data-stu-id="cfdef-135">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>   

## <a name="drawbacks"></a><span data-ttu-id="cfdef-136">Nachteile</span><span class="sxs-lookup"><span data-stu-id="cfdef-136">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="cfdef-137">Es gab einige Probleme mit Ziel typisierten `new` das Erstellen neuer Kategorien von wichtigen Änderungen, aber wir haben dies bereits mit `null` und `default`, und das ist kein erhebliches Problem.</span><span class="sxs-lookup"><span data-stu-id="cfdef-137">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="cfdef-138">Alternativen</span><span class="sxs-lookup"><span data-stu-id="cfdef-138">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="cfdef-139">Die meisten Beschwerden über Typen, die zu lang sind, um bei der Feld Initialisierung duplizieren zu können, sind *Typargumente* , die nicht den Typ selbst aufweisen. wir könnten nur Typargumente wie `new Dictionary(...)` (oder ähnliches) ableiten und Typargumente lokal von Argumenten oder dem auflistungsiniti</span><span class="sxs-lookup"><span data-stu-id="cfdef-139">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="cfdef-140">Fragen</span><span class="sxs-lookup"><span data-stu-id="cfdef-140">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="cfdef-141">Sollten wir die Verwendung von Ausdrücken in Ausdrucks Baumstrukturen verbieten?</span><span class="sxs-lookup"><span data-stu-id="cfdef-141">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="cfdef-142">gar</span><span class="sxs-lookup"><span data-stu-id="cfdef-142">(no)</span></span>
- <span data-ttu-id="cfdef-143">Wie wird das Feature mit `dynamic` Argumenten interagiert?</span><span class="sxs-lookup"><span data-stu-id="cfdef-143">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="cfdef-144">(keine besondere Behandlung)</span><span class="sxs-lookup"><span data-stu-id="cfdef-144">(no special treatment)</span></span>
- <span data-ttu-id="cfdef-145">Wie funktioniert IntelliSense mit `new()`?</span><span class="sxs-lookup"><span data-stu-id="cfdef-145">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="cfdef-146">(nur bei einem einzelnen Zieltyp)</span><span class="sxs-lookup"><span data-stu-id="cfdef-146">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="cfdef-147">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="cfdef-147">Design meetings</span></span>

- [<span data-ttu-id="cfdef-148">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="cfdef-148">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="cfdef-149">LDM-2018-05-21</span><span class="sxs-lookup"><span data-stu-id="cfdef-149">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="cfdef-150">LDM-2018-06-25</span><span class="sxs-lookup"><span data-stu-id="cfdef-150">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="cfdef-151">LDM-2018-08-22</span><span class="sxs-lookup"><span data-stu-id="cfdef-151">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="cfdef-152">LDM-2018-10-17</span><span class="sxs-lookup"><span data-stu-id="cfdef-152">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
- [<span data-ttu-id="cfdef-153">LDM-2020-03-25</span><span class="sxs-lookup"><span data-stu-id="cfdef-153">LDM-2020-03-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
