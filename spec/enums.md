---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703965"
---
# <a name="enums"></a><span data-ttu-id="8e964-101">Enumerationen</span><span class="sxs-lookup"><span data-stu-id="8e964-101">Enums</span></span>

<span data-ttu-id="8e964-102">Ein ***Enumerationstyp*** ist ein eindeutiger Werttyp ([Werttypen](types.md#value-types)), der einen Satz benannter Konstanten deklariert.</span><span class="sxs-lookup"><span data-stu-id="8e964-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="8e964-103">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="8e964-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="8e964-104">deklariert einen Enumerationstyp mit dem Namen `Color` mit Membern `Red`, `Green`und `Blue`.</span><span class="sxs-lookup"><span data-stu-id="8e964-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="8e964-105">Enumerationsdeklarationen</span><span class="sxs-lookup"><span data-stu-id="8e964-105">Enum declarations</span></span>

<span data-ttu-id="8e964-106">Eine Aufzählungs Deklaration deklariert einen neuen Aufzählungstyp.</span><span class="sxs-lookup"><span data-stu-id="8e964-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="8e964-107">Eine Enumerationsdeklaration beginnt mit dem Schlüsselwort `enum`und definiert den Namen, die Barrierefreiheit, den zugrunde liegenden Typ und die Member der Enumeration.</span><span class="sxs-lookup"><span data-stu-id="8e964-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

<span data-ttu-id="8e964-108">Jeder Aufzählungstyp verfügt über einen entsprechenden ganzzahligen Typ, der als ***zugrunde liegenden Typ*** des Aufzählungs Typs bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="8e964-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="8e964-109">Dieser zugrunde liegende Typ muss in der Lage sein, alle Enumeratorwerte darzustellen, die in der-Enumeration definiert sind.</span><span class="sxs-lookup"><span data-stu-id="8e964-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="8e964-110">Eine Aufzählungs Deklaration kann explizit einen zugrunde liegenden Typ von `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` oder `ulong`deklarieren.</span><span class="sxs-lookup"><span data-stu-id="8e964-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="8e964-111">Beachten Sie, dass `char` nicht als zugrunde liegender Typ verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="8e964-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="8e964-112">Eine Aufzählungs Deklaration, die einen zugrunde liegenden Typ nicht explizit deklariert, verfügt über einen zugrunde liegenden Typ von `int`.</span><span class="sxs-lookup"><span data-stu-id="8e964-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="8e964-113">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="8e964-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="8e964-114">deklariert eine-Aufzählung mit einem zugrunde liegenden Typ von `long`.</span><span class="sxs-lookup"><span data-stu-id="8e964-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="8e964-115">Ein Entwickler kann einen zugrunde liegenden Typ von `long`verwenden, wie z. b., um die Verwendung von Werten zu aktivieren, die sich im Bereich von `long`, aber nicht im Bereich der `int`befinden, oder um diese Option für die Zukunft beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="8e964-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="8e964-116">Aufzählungs modifiziererer</span><span class="sxs-lookup"><span data-stu-id="8e964-116">Enum modifiers</span></span>

<span data-ttu-id="8e964-117">Eine *enum_declaration* kann optional eine Sequenz von enumerationsmodifiziererwerten enthalten:</span><span class="sxs-lookup"><span data-stu-id="8e964-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="8e964-118">Es ist ein Kompilierzeitfehler, damit derselbe Modifizierer mehrmals in einer Enumeration-Deklaration angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="8e964-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="8e964-119">Die Modifizierer einer Enumerationsdeklaration haben dieselbe Bedeutung wie die einer Klassen Deklaration ([Klassenmodifizierer](classes.md#class-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="8e964-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="8e964-120">Beachten Sie jedoch, dass die `abstract`-und `sealed` Modifizierer in einer Aufzählungs Deklaration nicht zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="8e964-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="8e964-121">Auffüge Zeichen können nicht abstrakt sein und dürfen keine Ableitung zulassen.</span><span class="sxs-lookup"><span data-stu-id="8e964-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="8e964-122">Enumerationsmember</span><span class="sxs-lookup"><span data-stu-id="8e964-122">Enum members</span></span>

<span data-ttu-id="8e964-123">Der Text einer Enumerationstypdeklaration definiert NULL oder mehr Enumerationsmember, bei denen es sich um die benannten Konstanten des Enumerationstyps handelt.</span><span class="sxs-lookup"><span data-stu-id="8e964-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="8e964-124">Es können nicht zwei Enumerationsmember denselben Namen haben.</span><span class="sxs-lookup"><span data-stu-id="8e964-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="8e964-125">Jedem Enumerationsmember ist ein konstanter Wert zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="8e964-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="8e964-126">Der Typ dieses Werts ist der zugrunde liegende Typ für die enthaltende Enumeration.</span><span class="sxs-lookup"><span data-stu-id="8e964-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="8e964-127">Der Konstante Wert für jedes Enumerationsmember muss im Bereich des zugrunde liegenden Typs für die Enumeration liegen.</span><span class="sxs-lookup"><span data-stu-id="8e964-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="8e964-128">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="8e964-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="8e964-129">führt zu einem Kompilierzeitfehler, da die Konstanten Werte `-1`, `-2`und `-3` sich nicht im Bereich des zugrunde liegenden ganzzahligen Typs `uint`befinden.</span><span class="sxs-lookup"><span data-stu-id="8e964-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="8e964-130">Mehrere Enumerationsmember können denselben zugeordneten Wert gemeinsam verwenden.</span><span class="sxs-lookup"><span data-stu-id="8e964-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="8e964-131">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="8e964-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="8e964-132">zeigt eine Enumeration an, in der zwei Enumerationsmember--`Blue` und `Max`--den gleichen zugeordneten Wert aufweisen.</span><span class="sxs-lookup"><span data-stu-id="8e964-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="8e964-133">Der zugeordnete Wert eines Enumerationsmembers wird entweder implizit oder explizit zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="8e964-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="8e964-134">Wenn die Deklaration des Enumerationsmembers über einen *constant_expression* Initialisierer verfügt, ist der Wert dieses konstanten Ausdrucks, der implizit in den zugrunde liegenden Typ der Enumeration konvertiert wird, der zugeordnete Wert des Enumerationsmembers.</span><span class="sxs-lookup"><span data-stu-id="8e964-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="8e964-135">Wenn die Deklaration des Enumerationsmembers keinen Initialisierer aufweist, wird der zugehörige Wert implizit wie folgt festgelegt:</span><span class="sxs-lookup"><span data-stu-id="8e964-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="8e964-136">Wenn das Enumerationsmember das erste Enumerationsmember ist, das im Enumerationstyp deklariert wurde, ist der zugeordnete Wert 0 (null).</span><span class="sxs-lookup"><span data-stu-id="8e964-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="8e964-137">Andernfalls wird der zugeordnete Wert des Enumerationsmembers abgerufen, indem der zugeordnete Wert des texthin vorangehenden Enumerationsmembers um eins erhöht wird.</span><span class="sxs-lookup"><span data-stu-id="8e964-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="8e964-138">Dieser erweiterte Wert muss innerhalb des Bereichs von Werten liegen, der durch den zugrunde liegenden Typ dargestellt werden kann. andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="8e964-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="8e964-139">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="8e964-139">The example</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

<span data-ttu-id="8e964-140">druckt die enumerationselementnamen und die zugehörigen Werte aus.</span><span class="sxs-lookup"><span data-stu-id="8e964-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="8e964-141">Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="8e964-141">The output is:</span></span>

```console
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="8e964-142">aus den folgenden Gründen:</span><span class="sxs-lookup"><span data-stu-id="8e964-142">for the following reasons:</span></span>

*  <span data-ttu-id="8e964-143">dem Enumerationsmember `Red` wird automatisch der Wert 0 (null) zugewiesen (da er über keinen Initialisierer verfügt und der erste Enumerationsmember ist);</span><span class="sxs-lookup"><span data-stu-id="8e964-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="8e964-144">Das Enumerationsmember `Green` erhält explizit den Wert `10`;</span><span class="sxs-lookup"><span data-stu-id="8e964-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="8e964-145">und dem Enumerationsmember `Blue` wird automatisch der Wert zugewiesen, der größer ist als der Member, der ihm texthal vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="8e964-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="8e964-146">Der zugeordnete Wert eines Enumerationsmembers darf nicht direkt oder indirekt den Wert seines eigenen zugeordneten Enumerationsmembers verwenden.</span><span class="sxs-lookup"><span data-stu-id="8e964-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="8e964-147">Abgesehen von dieser zirkulareinschränkungs Einschränkung verweisen Enumerationsmember-Initialisierer möglicherweise unabhängig von ihrer Textposition auf andere Initialisierer von Enumerationsmembern.</span><span class="sxs-lookup"><span data-stu-id="8e964-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="8e964-148">In einem enumerationselementinitialisierer werden Werte anderer Enumerationsmember immer so behandelt, als hätten Sie den Typ des zugrunde liegenden Typs, sodass Umwandlungen beim Verweis auf andere Enumerationsmember nicht erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="8e964-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="8e964-149">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="8e964-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="8e964-150">führt zu einem Kompilierzeitfehler, da die Deklarationen von `A` und `B` zirkulär sind.</span><span class="sxs-lookup"><span data-stu-id="8e964-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="8e964-151">`A` ist explizit von `B` abhängig, und `B` von `A` implizit abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="8e964-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="8e964-152">Enumerationsmember werden genau analog zu den Feldern innerhalb von Klassen benannt und auf eine Weise festgelegt.</span><span class="sxs-lookup"><span data-stu-id="8e964-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="8e964-153">Der Gültigkeitsbereich eines Enumerationsmembers ist der Text seines enthaltenden Enumerationstyps.</span><span class="sxs-lookup"><span data-stu-id="8e964-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="8e964-154">Innerhalb dieses Bereichs kann mit dem einfachen Namen auf Enumerationsmember verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="8e964-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="8e964-155">Aus dem gesamten anderen Code muss der Name eines Enumerationsmembers mit dem Namen seines Enumerationstyps qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="8e964-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="8e964-156">Enumerationsmember haben keine deklarierten Barrierefreiheits Elemente. ein Enumerationsmember ist verfügbar, wenn auf den enthaltenden Enumerationstyp zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="8e964-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="8e964-157">Der System. Aufzählungs Typen</span><span class="sxs-lookup"><span data-stu-id="8e964-157">The System.Enum type</span></span>

<span data-ttu-id="8e964-158">Der Typ `System.Enum` ist die abstrakte Basisklasse aller Enumerationstypen (Dies unterscheidet sich von dem zugrunde liegenden Typ des Enumerationstyps), und die von `System.Enum` geerbten Member sind in jedem Enumerationstyp verfügbar.</span><span class="sxs-lookup"><span data-stu-id="8e964-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="8e964-159">Eine Boxing-Konvertierung ([Boxing-Konvertierungen](types.md#boxing-conversions)) ist von einem beliebigen Aufgabentyp zu `System.Enum`vorhanden, und eine Unboxing-Konvertierung ([Unboxing-Konvertierungen](types.md#unboxing-conversions)) ist von `System.Enum` bis zu einem beliebigen Aufzählungs Typ vorhanden</span><span class="sxs-lookup"><span data-stu-id="8e964-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="8e964-160">Beachten Sie, dass `System.Enum` nicht selbst ein *enum_type*ist.</span><span class="sxs-lookup"><span data-stu-id="8e964-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="8e964-161">Vielmehr handelt es sich um eine *class_type* , von der alle *enum_type*s abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="8e964-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="8e964-162">Der Typ `System.Enum` erbt vom Typ `System.ValueType` ([dem System. ValueType-Typ](types.md#the-systemvaluetype-type)), der wiederum vom Typ `object`erbt.</span><span class="sxs-lookup"><span data-stu-id="8e964-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="8e964-163">Zur Laufzeit kann ein Wert vom Typ `System.Enum` `null` oder ein Verweis auf einen geachtelten Wert eines beliebigen Enumerationstyps sein.</span><span class="sxs-lookup"><span data-stu-id="8e964-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="8e964-164">Enumerationswerte und-Vorgänge</span><span class="sxs-lookup"><span data-stu-id="8e964-164">Enum values and operations</span></span>

<span data-ttu-id="8e964-165">Jeder Aufzählungstyp definiert einen eindeutigen Typ. eine explizite Enumerationskonvertierung ([Explizite Enumerationskonvertierungen](conversions.md#explicit-enumeration-conversions)) ist erforderlich, um zwischen einem Enumerationstyp und einem ganzzahligen Typ oder zwischen zwei Enumerationstypen zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="8e964-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="8e964-166">Der Satz von Werten, der von einem Enumerationstyp übernommen werden kann, wird nicht durch seine Enumerationsmember eingeschränkt.</span><span class="sxs-lookup"><span data-stu-id="8e964-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="8e964-167">Insbesondere kann jeder Wert des zugrunde liegenden Typs einer Enumeration in den Enumerationstyp umgewandelt werden und ist ein eindeutiger gültiger Wert dieses Enumerationstyps.</span><span class="sxs-lookup"><span data-stu-id="8e964-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="8e964-168">Enumerationsmember haben den Typ Ihres enthaltenden Enumerationstyps (außer innerhalb anderer Enumerationmember-Initialisierer: siehe [Enumeration](enums.md#enum-members)-Member).</span><span class="sxs-lookup"><span data-stu-id="8e964-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="8e964-169">Der Wert eines enumerationmembers, der im Enumerationstyp deklariert wurde `E` mit zugeordneter Wert `v` ist `(E)v`.</span><span class="sxs-lookup"><span data-stu-id="8e964-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="8e964-170">Die folgenden Operatoren können für Werte von Enumerationstypen verwendet werden: `==`, `!=`, `<`, `>`, `<=`, `>=` ([enumerationsvergleichs-Operatoren](expressions.md#enumeration-comparison-operators)), binäre `+` (Additions[Operator](expressions.md#addition-operator)), Binär `-` ([Subtraktions Operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([logische enumerationsoperatoren](expressions.md#enumeration-logical-operators)), `~` ([bitweiser Komplement](expressions.md#bitwise-complement-operator)Operator), [Postfix Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators) und [Präfix Inkrement-und Dekrementoperatoren](expressions.md#prefix-increment-and-decrement-operators)).`++``--` </span><span class="sxs-lookup"><span data-stu-id="8e964-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="8e964-171">Jeder Enumerationstyp wird automatisch von der-Klasse `System.Enum` abgeleitet (die wiederum von `System.ValueType` und `object`abgeleitet ist).</span><span class="sxs-lookup"><span data-stu-id="8e964-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="8e964-172">Daher können geerbte Methoden und Eigenschaften dieser Klasse für Werte eines Enumerationstyps verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8e964-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
