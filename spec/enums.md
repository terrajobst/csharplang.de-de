---
ms.openlocfilehash: c3b716e6eb331be2ee33fffeb42c1e2406cd3a5c
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488757"
---
# <a name="enums"></a><span data-ttu-id="0cea4-101">Enumerationen</span><span class="sxs-lookup"><span data-stu-id="0cea4-101">Enums</span></span>

<span data-ttu-id="0cea4-102">Ein ***Enumerationstyp*** ist ein unverwechselbarer Werttyp ([Werttypen](types.md#value-types)), die einen Satz benannter Konstanten deklariert.</span><span class="sxs-lookup"><span data-stu-id="0cea4-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="0cea4-103">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="0cea4-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="0cea4-104">deklariert einen Enum-Typ, der mit dem Namen `Color` mit Mitgliedern `Red`, `Green`, und `Blue`.</span><span class="sxs-lookup"><span data-stu-id="0cea4-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="0cea4-105">Enum-Deklarationen</span><span class="sxs-lookup"><span data-stu-id="0cea4-105">Enum declarations</span></span>

<span data-ttu-id="0cea4-106">Eine Enum-Deklaration deklariert einen neue Enum-Typ.</span><span class="sxs-lookup"><span data-stu-id="0cea4-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="0cea4-107">Eine Enum-Deklaration beginnt mit dem Schlüsselwort `enum`, und definiert den Namen, die Barrierefreiheit, die zugrunde liegenden Typ und die Member der Enumeration.</span><span class="sxs-lookup"><span data-stu-id="0cea4-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

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

<span data-ttu-id="0cea4-108">Jeder Enumerationstyp hat einen entsprechenden ganzzahligen Typ, der Namen der ***zugrunde liegender Typ*** des Enum-Typs.</span><span class="sxs-lookup"><span data-stu-id="0cea4-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="0cea4-109">Diese zugrunde liegenden Typ muss alle in der Enumeration definierten Enumeratorwerte darstellen können.</span><span class="sxs-lookup"><span data-stu-id="0cea4-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="0cea4-110">Eine Enum-Deklaration kann explizit einen zugrunde liegenden Typ deklarieren `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` oder `ulong`.</span><span class="sxs-lookup"><span data-stu-id="0cea4-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="0cea4-111">Beachten Sie, dass `char` nicht als zugrunde liegender Typ verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0cea4-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="0cea4-112">Eine Enum-Deklaration, der nicht explizit einen zugrunde liegenden Typ deklariert, hat einen zugrunde liegenden Typ `int`.</span><span class="sxs-lookup"><span data-stu-id="0cea4-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="0cea4-113">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="0cea4-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="0cea4-114">deklariert eine Enumeration mit zugrunde liegender Typ `long`.</span><span class="sxs-lookup"><span data-stu-id="0cea4-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="0cea4-115">Ein Entwickler kann auswählen, einen zugrunde liegenden Typ verwenden `long`, wie im Beispiel verwenden, um die Verwendung von Werten zu ermöglichen, die im Bereich von `long` , aber nicht in den Bereich der `int`, oder klicken Sie auf diese Option für die Zukunft zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="0cea4-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="0cea4-116">Enum-Modifizierer</span><span class="sxs-lookup"><span data-stu-id="0cea4-116">Enum modifiers</span></span>

<span data-ttu-id="0cea4-117">Ein *Enum_declaration* kann optional eine Sequenz von Enum-Modifizierer enthalten:</span><span class="sxs-lookup"><span data-stu-id="0cea4-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="0cea4-118">Es ist ein Fehler während der Kompilierung für den gleichen Modifizierer für mehrere Male in einer Enum-Deklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0cea4-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="0cea4-119">Der Modifizierer, der eine Enum-Deklaration haben dieselbe Bedeutung wie die einer Klassendeklaration ([Klasse Modifizierer](classes.md#class-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="0cea4-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="0cea4-120">Beachten Sie jedoch, die die `abstract` und `sealed` Modifizierer dürfen nicht in eine Enum-Deklaration.</span><span class="sxs-lookup"><span data-stu-id="0cea4-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="0cea4-121">Enumerationen darf nicht abstrakt sein und lassen sich nicht auf die Ableitung.</span><span class="sxs-lookup"><span data-stu-id="0cea4-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="0cea4-122">Mitglieder aufzählen</span><span class="sxs-lookup"><span data-stu-id="0cea4-122">Enum members</span></span>

<span data-ttu-id="0cea4-123">Der Hauptteil der Deklaration eines Enum-Typs definiert NULL oder mehr Enum-Member, die benannten Konstanten des Enum-Typs sind.</span><span class="sxs-lookup"><span data-stu-id="0cea4-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="0cea4-124">Enumerationsmember nicht, können die gleichen Namen haben.</span><span class="sxs-lookup"><span data-stu-id="0cea4-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="0cea4-125">Jedem Enumerationsmember ist einen konstanten Wert zugeordneten.</span><span class="sxs-lookup"><span data-stu-id="0cea4-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="0cea4-126">Der Typ dieses Werts wird der zugrunde liegende Typ der entsprechenden Enumeration.</span><span class="sxs-lookup"><span data-stu-id="0cea4-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="0cea4-127">Der Konstante Wert für jede Enum-Element muss im Bereich von den zugrunde liegenden Typ der Enumeration.</span><span class="sxs-lookup"><span data-stu-id="0cea4-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="0cea4-128">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="0cea4-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="0cea4-129">führt zu einem Kompilierzeitfehler, da die Konstanten Werte `-1`, `-2`, und `-3` sind nicht im Bereich des zugrunde liegenden ganzzahligen Typs `uint`.</span><span class="sxs-lookup"><span data-stu-id="0cea4-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="0cea4-130">Mehrere Enumerationsmember darf es sich um den gleichen zugeordneten Wert weitergeben.</span><span class="sxs-lookup"><span data-stu-id="0cea4-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="0cea4-131">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="0cea4-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="0cea4-132">Zeigt eine Enumeration in die zwei Enumerationsmember-- `Blue` und `Max` – der gleiche Wert verknüpft haben.</span><span class="sxs-lookup"><span data-stu-id="0cea4-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="0cea4-133">Der zugeordnete Wert eines Enumerationsmembers wird entweder implizit oder explizit zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="0cea4-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="0cea4-134">Wenn die Deklaration des Enumerationsmembers eine *Constant_expression* Initialisierer, der Wert dieses konstanten Ausdrucks, in der zugrunde liegende Typ der Enumeration implizit konvertiert wird, den zugeordneten Wert, der dem Enumerationsmember.</span><span class="sxs-lookup"><span data-stu-id="0cea4-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="0cea4-135">Wenn die Deklaration des Enumerationsmembers keinen Initialisierer aufweist, wird der zugehörige Wert festgelegt, implizit, wie folgt:</span><span class="sxs-lookup"><span data-stu-id="0cea4-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="0cea4-136">Wenn der Enum-Element der ersten Enum-Element, das in Enum-Typs deklariert ist, ist der zugehörige Wert 0 (null).</span><span class="sxs-lookup"><span data-stu-id="0cea4-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="0cea4-137">Andernfalls wird der zugeordnete Wert des Enumerationsmembers abgerufen, durch den zugeordneten Wert des textlich vorausgehenden Enumerationsmembers um eins erhöhen.</span><span class="sxs-lookup"><span data-stu-id="0cea4-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="0cea4-138">Dieser höhere Wert muss innerhalb des Bereichs von Werten, die durch den zugrunde liegenden Typ dargestellt werden kann, andernfalls tritt ein Fehler während der Kompilierung auf.</span><span class="sxs-lookup"><span data-stu-id="0cea4-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="0cea4-139">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="0cea4-139">The example</span></span>

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

<span data-ttu-id="0cea4-140">Gibt den Elementnamen Enumeration und die zugehörigen Werte.</span><span class="sxs-lookup"><span data-stu-id="0cea4-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="0cea4-141">Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="0cea4-141">The output is:</span></span>

```
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="0cea4-142">aus den folgenden Gründen:</span><span class="sxs-lookup"><span data-stu-id="0cea4-142">for the following reasons:</span></span>

*  <span data-ttu-id="0cea4-143">der Enumerationsmember `Red` erhält automatisch den Wert 0 (da es keinen Initialisierer aufweist und das erste Enum-Element ist);</span><span class="sxs-lookup"><span data-stu-id="0cea4-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="0cea4-144">der Enumerationsmember `Green` erhält den Wert explizit `10`;</span><span class="sxs-lookup"><span data-stu-id="0cea4-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="0cea4-145">und der Enumerationsmember `Blue` wird automatisch der Wert eins größer ist als das Element, das textlich vorausgehenden zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="0cea4-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="0cea4-146">Der zugeordnete Wert eines Enumerationsmembers möglicherweise nicht der Fall, direkt oder indirekt, verwenden Sie den Wert der eigenen zugehörigen Enum-Element.</span><span class="sxs-lookup"><span data-stu-id="0cea4-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="0cea4-147">Außer dieser Einschränkung Zirkularität können die Enum-Standardmember-Initialisierer frei auf andere Enumeration Standardmember-Initialisierer, unabhängig von deren Text Position verweisen.</span><span class="sxs-lookup"><span data-stu-id="0cea4-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="0cea4-148">Innerhalb einer enumerationsmemberinitialisierer werden Werte anderer Enumerationsmember immer als müssen den Typ des ihre zugrunde liegenden Typ und behandelt, sodass Umwandlungen nicht erforderlich, beim Verweisen auf andere Enumerationsmember sind.</span><span class="sxs-lookup"><span data-stu-id="0cea4-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="0cea4-149">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="0cea4-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="0cea4-150">führt zu einem Kompilierzeitfehler, da die Deklarationen der `A` und `B` sind zirkulär.</span><span class="sxs-lookup"><span data-stu-id="0cea4-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="0cea4-151">`A` hängt von `B` explizit und `B` hängt `A` implizit.</span><span class="sxs-lookup"><span data-stu-id="0cea4-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="0cea4-152">Enumerationsmember sind mit dem Namen und in einer Weise, die analog zu Feldern in Klassen beschränkt.</span><span class="sxs-lookup"><span data-stu-id="0cea4-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="0cea4-153">Der Bereich eines Enumerationsmembers ist der Text, den enthaltenden Typ der Enumeration.</span><span class="sxs-lookup"><span data-stu-id="0cea4-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="0cea4-154">Innerhalb des Bereichs können die Enum-Elementen, mit ihrem einfachen Namen verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="0cea4-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="0cea4-155">Von anderem Code muss der Name des Enum-Element mit dem Namen der Enum-Typs qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="0cea4-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="0cea4-156">Enum-Mitglieder haben keiner deklarierten Zugriffsebenen – ein Enum-Element zugegriffen werden kann, wenn die enthaltende Enum-Typ zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="0cea4-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="0cea4-157">Der System.Enum-Typ</span><span class="sxs-lookup"><span data-stu-id="0cea4-157">The System.Enum type</span></span>

<span data-ttu-id="0cea4-158">Der Typ `System.Enum` ist die abstrakte Basisklasse aller Enum-Typen (Dies ist distinct und unterscheidet sich von den zugrunde liegenden Typ des Enum-Typs), und die Mitglieder von geerbt `System.Enum` stehen in jeder Enumerationstyp.</span><span class="sxs-lookup"><span data-stu-id="0cea4-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="0cea4-159">Eine Boxingkonvertierung ([Boxing-Konvertierung](types.md#boxing-conversions)) vorhanden ist, aus jeder Enumerationstyp, `System.Enum`, und einer unboxing-Konvertierung ([Unboxing-Konvertierungen](types.md#unboxing-conversions)) vorhanden ist, von `System.Enum` auf einen beliebigen Enumerationstyp.</span><span class="sxs-lookup"><span data-stu-id="0cea4-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="0cea4-160">Beachten Sie, dass `System.Enum` ist nicht selbst eine *Enum_type*.</span><span class="sxs-lookup"><span data-stu-id="0cea4-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="0cea4-161">Es handelt sich vielmehr eine *Class_type* von der alle *Enum_type*s abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="0cea4-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="0cea4-162">Der Typ `System.Enum` erbt vom Typ `System.ValueType` ([die System.ValueType-Typ](types.md#the-systemvaluetype-type)), das wiederum vom Typ erbt `object`.</span><span class="sxs-lookup"><span data-stu-id="0cea4-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="0cea4-163">Zur Laufzeit, ein Wert vom Typ `System.Enum` kann `null` oder ein Verweis auf einen geschachtelten Wert eines beliebigen Enumerationstyps.</span><span class="sxs-lookup"><span data-stu-id="0cea4-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="0cea4-164">Enum-Werte und Vorgänge</span><span class="sxs-lookup"><span data-stu-id="0cea4-164">Enum values and operations</span></span>

<span data-ttu-id="0cea4-165">Jeder Enumerationstyp definiert einen gesonderter Typ ist. eine Enumeration der expliziten Konvertierung ([explizite Enumeration Konvertierungen](conversions.md#explicit-enumeration-conversions)) ist erforderlich, um zwischen einer Enum-Typ und einen ganzzahligen Typ oder zwischen zwei Enum-Typen zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="0cea4-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="0cea4-166">Der Satz von Werten, die ein Enum-Typ übernehmen kann, ist nicht durch die Enumerationsmember beschränkt.</span><span class="sxs-lookup"><span data-stu-id="0cea4-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="0cea4-167">Insbesondere ein Wert für den zugrunde liegenden Typ einer Enumeration in Enum-Typs umgewandelt werden kann, und ist ein eindeutiger gültiger Wert dieses Enum-Typs.</span><span class="sxs-lookup"><span data-stu-id="0cea4-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="0cea4-168">Enum-Member haben den Typ ihres enthaltenden Enum-Typs (mit Ausnahme der in anderen Enum-Standardmember-Initialisierer: finden Sie unter [Enumerationsmember](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="0cea4-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="0cea4-169">Der Wert eines Enumerationsmembers in Enum-Typs deklariert `E` mit zugeordneten Wert `v` ist `(E)v`.</span><span class="sxs-lookup"><span data-stu-id="0cea4-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="0cea4-170">Die folgenden Operatoren verwendet werden können, auf den Werten der Enum-Typen: `==`, `!=`, `<`, `>`, `<=`, `>=`  ([Enumeration Vergleichsoperatoren](expressions.md#enumeration-comparison-operators)) , binäre `+`  ([Additionsoperator](expressions.md#addition-operator)), binäre `-`  ([Subtraktionsoperator](expressions.md#subtraction-operator)), `^`, `&` , `|`  ([Enumeration logische Operatoren](expressions.md#enumeration-logical-operators)), `~`  ([bitweiser Komplementoperator](expressions.md#bitwise-complement-operator)), `++` und `--`  ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators) und [Präfix-Inkrement und Dekrement-Operatoren](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="0cea4-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="0cea4-171">Jeder Enumerationstyp wird automatisch von der Klasse `System.Enum` (abgeleitet, die wiederum `System.ValueType` und `object`).</span><span class="sxs-lookup"><span data-stu-id="0cea4-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="0cea4-172">Daher können für die Werte eines Enumerationstyps geerbter Methoden und Eigenschaften dieser Klasse verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0cea4-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
