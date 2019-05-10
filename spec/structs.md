---
ms.openlocfilehash: 72d17175dfb8ef284dce6cf7e5837420fa06f16a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488881"
---
# <a name="structs"></a><span data-ttu-id="99af7-101">Strukturen</span><span class="sxs-lookup"><span data-stu-id="99af7-101">Structs</span></span>

<span data-ttu-id="99af7-102">Strukturen sind ähnlich wie Klassen, Datenstrukturen dar, die Datenmember und Funktionsmember enthalten können.</span><span class="sxs-lookup"><span data-stu-id="99af7-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="99af7-103">Im Gegensatz zu Klassen, Strukturen sind allerdings Werttypen und erfordern keine Heapzuordnung.</span><span class="sxs-lookup"><span data-stu-id="99af7-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="99af7-104">Eine Variable eines Strukturtyps enthält die Daten der Struktur direkt, während eine Variable eines Klassentyps einen Verweis auf die Daten der letzten bekannten als Objekt enthält.</span><span class="sxs-lookup"><span data-stu-id="99af7-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="99af7-105">Strukturen sind besonders nützlich für kleine Datenstrukturen, die über Wertsemantik verfügen.</span><span class="sxs-lookup"><span data-stu-id="99af7-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="99af7-106">Komplexe Zahlen, Punkte in einem Koordinatensystem oder Schlüssel-Wert-Paare im Wörterbuch sind gute Beispiele für Strukturen.</span><span class="sxs-lookup"><span data-stu-id="99af7-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="99af7-107">Schlüssel für diese Datenstrukturen ist, dass sie einige Datenmember, das, dass keine Verwendung von Vererbung und referenzieller Identität erforderlich ist und sie problemlos implementiert werden können über Wertsemantik, in der Zuweisung des Werts anstelle des Verweises kopiert haben.</span><span class="sxs-lookup"><span data-stu-id="99af7-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="99af7-108">Siehe [einfache Typen](types.md#simple-types), die einfachen Typen wie z. B. von c# bereitgestellten `int`, `double`, und `bool`, tatsächlich alle Strukturtypen sind.</span><span class="sxs-lookup"><span data-stu-id="99af7-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="99af7-109">Genau wie diese vordefinierten Typen Strukturen sind, ist es auch möglich, Strukturen und Überladen von Operatoren zum Implementieren der neuer "Grundtypen" in der C#-Sprache verwenden.</span><span class="sxs-lookup"><span data-stu-id="99af7-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="99af7-110">Zwei Beispiele für solche Typen werden am Ende dieses Kapitels erhalten ([Struct Examples](structs.md#struct-examples)).</span><span class="sxs-lookup"><span data-stu-id="99af7-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="99af7-111">Strukturdeklarationen</span><span class="sxs-lookup"><span data-stu-id="99af7-111">Struct declarations</span></span>

<span data-ttu-id="99af7-112">Ein *Struct_declaration* ist eine *Type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), die eine neue Struktur deklariert:</span><span class="sxs-lookup"><span data-stu-id="99af7-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="99af7-113">Ein *Struct_declaration* besteht aus einer optionalen Gruppe von *Attribute* ([Attribute](attributes.md)), gefolgt von einer optionalen Gruppe von *Struct_modifier*s ([Struktur Modifizierer](structs.md#struct-modifiers)), gefolgt von einem optionalen `partial` Modifizierer, gefolgt vom Schlüsselwort `struct` und *Bezeichner* mit dem Namen der Struktur, gefolgt von einer optionale *Type_parameter_list* Spezifikation ([Typparameter](classes.md#type-parameters)), gefolgt von einem optionalen *Struct_interfaces* Spezifikation ([Partial-Modifizierer](structs.md#partial-modifier))), gefolgt von einem optionalen *Type_parameter_constraints_clause*s-Spezifikation ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)), gefolgt von einem *Struct_body* ([strukturtext](structs.md#struct-body)), optional gefolgt durch ein Semikolon.</span><span class="sxs-lookup"><span data-stu-id="99af7-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="99af7-114">Struktur-Modifizierer</span><span class="sxs-lookup"><span data-stu-id="99af7-114">Struct modifiers</span></span>

<span data-ttu-id="99af7-115">Ein *Struct_declaration* kann optional eine Sequenz von Struct-Modifizierer enthalten:</span><span class="sxs-lookup"><span data-stu-id="99af7-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

<span data-ttu-id="99af7-116">Es ist ein Fehler während der Kompilierung für den gleichen Modifizierer für mehrere Male in einer Strukturdeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="99af7-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="99af7-117">Die Modifizierer der einer Strukturdeklaration haben dieselbe Bedeutung wie die einer Klassendeklaration ([Klasse Deklarationen](classes.md#class-declarations)).</span><span class="sxs-lookup"><span data-stu-id="99af7-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="99af7-118">Ein partial-Modifizierer</span><span class="sxs-lookup"><span data-stu-id="99af7-118">Partial modifier</span></span>

<span data-ttu-id="99af7-119">Die `partial` Modifizierer gibt an, dass dies *Struct_declaration* ist eine Deklaration der partiellen Typ.</span><span class="sxs-lookup"><span data-stu-id="99af7-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="99af7-120">Mehreren partiellen Strukturdeklarationen mit demselben Namen in einer einschließenden Namespace oder Typ Deklaration kombinieren, um eine Strukturdeklaration zu bilden, im angegebenen gemäß den Regeln [partielle Typen](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="99af7-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="99af7-121">Struktur-Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="99af7-121">Struct interfaces</span></span>

<span data-ttu-id="99af7-122">Herausgeberkontos ausgewiesenen Form eine Strukturdeklaration eine *Struct_interfaces* -Spezifikation, in dem Fall der Struktur wird als die angegebene Schnittstelle-Typen direkt zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="99af7-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="99af7-123">Schnittstellenimplementierungen finden Sie weiter unten in [Schnittstellenimplementierungen](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="99af7-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="99af7-124">Strukturtext</span><span class="sxs-lookup"><span data-stu-id="99af7-124">Struct body</span></span>

<span data-ttu-id="99af7-125">Die *Struct_body* einer Struktur definiert die Elemente der Struktur.</span><span class="sxs-lookup"><span data-stu-id="99af7-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="99af7-126">Strukturmember</span><span class="sxs-lookup"><span data-stu-id="99af7-126">Struct members</span></span>

<span data-ttu-id="99af7-127">Die Member einer Struktur besteht aus der Elemente eingeführt, durch die *Struct_member_declaration*s und den Membern geerbt vom Typ `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="99af7-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

<span data-ttu-id="99af7-128">Mit Ausnahme der Unterschiede in notiert [Klassen- und Strukturmember Unterschiede](structs.md#class-and-struct-differences), die Beschreibungen der Klassenmember, die im bereitgestellten [Klassenmember](classes.md#class-members) über [Iteratoren](classes.md#iterators) gelten für Struktur auch Elemente.</span><span class="sxs-lookup"><span data-stu-id="99af7-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="99af7-129">Unterschiede für Klassen- und Strukturmember</span><span class="sxs-lookup"><span data-stu-id="99af7-129">Class and struct differences</span></span>

<span data-ttu-id="99af7-130">Strukturen unterscheiden sich von Klassen in mehreren wichtigen Punkten:</span><span class="sxs-lookup"><span data-stu-id="99af7-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="99af7-131">Strukturen sind Werttypen ([Wertsemantik](structs.md#value-semantics)).</span><span class="sxs-lookup"><span data-stu-id="99af7-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="99af7-132">Alle Strukturtypen erben implizit von der Klasse `System.ValueType` ([Vererbung](structs.md#inheritance)).</span><span class="sxs-lookup"><span data-stu-id="99af7-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="99af7-133">Zuweisung zu einer Variable eines Strukturtyps erstellt eine Kopie des Werts zugewiesen wird ([Zuweisung](structs.md#assignment)).</span><span class="sxs-lookup"><span data-stu-id="99af7-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="99af7-134">Der Standardwert einer Struktur ist der Wert, der erstellt werden, indem alle Werttypfelder auf ihre Standardwerte und alle Verweise auf Felder festlegen. `null` ([Standardwerte](structs.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="99af7-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="99af7-135">Boxing und unboxing-Vorgänge werden verwendet, um zwischen einem Strukturtyp zu konvertieren und `object` ([Boxing und unboxing](structs.md#boxing-and-unboxing)).</span><span class="sxs-lookup"><span data-stu-id="99af7-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="99af7-136">Die Bedeutung der `this` unterscheidet sich für Strukturen ([diesen Zugriff](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="99af7-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="99af7-137">Instanz Steuerelementfeld-Deklarationen für eine Struktur dürfen nicht Variableninitialisierern enthalten ([Feld Initialisierer](structs.md#field-initializers)).</span><span class="sxs-lookup"><span data-stu-id="99af7-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="99af7-138">Eine Struktur ist nicht zulässig, einen parameterlosen Instanzenkonstruktor deklarieren ([Konstruktoren](structs.md#constructors)).</span><span class="sxs-lookup"><span data-stu-id="99af7-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="99af7-139">Eine Struktur ist nicht zulässig, einen Destruktor deklarieren ([Destruktoren](structs.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="99af7-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="99af7-140">Wertsemantik</span><span class="sxs-lookup"><span data-stu-id="99af7-140">Value semantics</span></span>

<span data-ttu-id="99af7-141">Strukturen sind Werttypen ([Werttypen](types.md#value-types)) und Sie über Wertsemantik verfügen.</span><span class="sxs-lookup"><span data-stu-id="99af7-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="99af7-142">Auf der anderen Seite sind Klassen, Verweistypen ([Verweistypen](types.md#reference-types)) und bewirken, haben Verweissemantik.</span><span class="sxs-lookup"><span data-stu-id="99af7-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="99af7-143">Eine Variable eines Strukturtyps enthält die Daten der Struktur direkt, während eine Variable eines Klassentyps einen Verweis auf die Daten der letzten bekannten als Objekt enthält.</span><span class="sxs-lookup"><span data-stu-id="99af7-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="99af7-144">Wenn eine Struktur `B` enthält ein Instanzenfeld eines Typs `A` und `A` ist ein Strukturtyp, es ist ein Fehler während der Kompilierung für `A` hängt von `B` oder ein Typ erstellt, von `B`.</span><span class="sxs-lookup"><span data-stu-id="99af7-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="99af7-145">Eine Struktur `X` ***hängt direkt von*** eine Struktur `Y` Wenn `X` enthält ein Instanzenfeld eines Typs `Y`.</span><span class="sxs-lookup"><span data-stu-id="99af7-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="99af7-146">Dank dieser der vollständige Satz von Strukturen, die von der eine Struktur abhängig ist, wird den transitiven Abschluss von der ***hängt direkt von*** Beziehung.</span><span class="sxs-lookup"><span data-stu-id="99af7-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="99af7-147">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="99af7-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="99af7-148">ist ein Fehler, da `Node` ein Instanzenfeld des eigenen Typs enthält.</span><span class="sxs-lookup"><span data-stu-id="99af7-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="99af7-149">Ein weiteres Beispiel</span><span class="sxs-lookup"><span data-stu-id="99af7-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="99af7-150">ist ein Fehler, da alle Typen `A`, `B`, und `C` voneinander abhängig.</span><span class="sxs-lookup"><span data-stu-id="99af7-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="99af7-151">Mit Klassen ist es möglich, dass zwei Variablen auf dasselbe Objekt verweisen, und so können Vorgänge auf eine Variable auf das Objekt, das die andere Variable verweist auswirken.</span><span class="sxs-lookup"><span data-stu-id="99af7-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="99af7-152">Mit Strukturen besitzt jede Variable eine eigene Kopie der Daten (außer im Fall von `ref` und `out` Parametervariablen), und es ist nicht möglich, für Vorgänge auf einem anderen zu beeinflussen.</span><span class="sxs-lookup"><span data-stu-id="99af7-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="99af7-153">Darüber hinaus da Strukturen keine Verweistypen sind, ist es nicht möglich, dass die Werte eines Strukturtyps sein `null`.</span><span class="sxs-lookup"><span data-stu-id="99af7-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="99af7-154">Im Falle folgender Deklaration</span><span class="sxs-lookup"><span data-stu-id="99af7-154">Given the declaration</span></span>
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="99af7-155">das Codefragment</span><span class="sxs-lookup"><span data-stu-id="99af7-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="99af7-156">Gibt den Wert `10`.</span><span class="sxs-lookup"><span data-stu-id="99af7-156">outputs the value `10`.</span></span> <span data-ttu-id="99af7-157">Die Zuweisung von `a` zu `b` erstellt eine Kopie des Werts und `b` ist daher nicht betroffen, durch die Zuweisung zu `a.x`.</span><span class="sxs-lookup"><span data-stu-id="99af7-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="99af7-158">Hatte `Point` wurde stattdessen als eine Klasse deklariert, ist die Ausgabe wäre `100` da `a` und `b` würde das gleiche Objekt verweisen.</span><span class="sxs-lookup"><span data-stu-id="99af7-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="99af7-159">Vererbung</span><span class="sxs-lookup"><span data-stu-id="99af7-159">Inheritance</span></span>

<span data-ttu-id="99af7-160">Alle Strukturtypen erben implizit von der Klasse `System.ValueType`, das wiederum erbt von Klasse `object`.</span><span class="sxs-lookup"><span data-stu-id="99af7-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="99af7-161">Eine Strukturdeklaration kann eine Liste der implementierten Schnittstellen angeben, aber es ist nicht möglich, für die Strukturdeklaration einer, die eine Basisklasse angeben.</span><span class="sxs-lookup"><span data-stu-id="99af7-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="99af7-162">Strukturtypen sind nicht abstrakt und sind immer implizit versiegelt.</span><span class="sxs-lookup"><span data-stu-id="99af7-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="99af7-163">Die `abstract` und `sealed` Modifizierer sind in einer Strukturdeklaration daher nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="99af7-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="99af7-164">Da Vererbung für Strukturen nicht unterstützt wird, nicht die deklarierte Zugriffsart eines Members Struktur `protected` oder `protected internal`.</span><span class="sxs-lookup"><span data-stu-id="99af7-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="99af7-165">Funktionsmember in einer Struktur nicht `abstract` oder `virtual`, und die `override` Modifizierer kann nur für das Überschreiben von geerbten Methoden `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="99af7-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="99af7-166">Zuweisung</span><span class="sxs-lookup"><span data-stu-id="99af7-166">Assignment</span></span>

<span data-ttu-id="99af7-167">Zuweisung zu einer Variable eines Strukturtyps erstellt eine Kopie der Wert zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="99af7-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="99af7-168">Dies unterscheidet sich von Zuweisung einer Variable eines Klassentyps, die den Verweis aber nicht das Objekt identifiziert, die durch den Verweis kopiert.</span><span class="sxs-lookup"><span data-stu-id="99af7-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="99af7-169">Ähnlich wie bei einer Zuweisung ist, wenn eine Struktur als ein Value-Parameter übergeben oder als Ergebnis ein Funktionsmember zurückgegeben wird, wird eine Kopie der Struktur erstellt.</span><span class="sxs-lookup"><span data-stu-id="99af7-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="99af7-170">Eine Struktur kann übergeben werden, durch Verweis auf ein Funktionsmember mithilfe einer `ref` oder `out` Parameter.</span><span class="sxs-lookup"><span data-stu-id="99af7-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="99af7-171">Wenn eine Eigenschaft oder der Indexer einer Struktur das Ziel einer Zuweisung ist, muss der Instanzausdruck, der mit der Eigenschaft oder der Indexer verknüpft als Variable klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="99af7-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="99af7-172">Wenn der Instanzausdruck als Wert klassifiziert ist, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="99af7-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="99af7-173">Dies wird ausführlicher im beschrieben [einfache Zuweisung](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="99af7-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="99af7-174">Standardwerte</span><span class="sxs-lookup"><span data-stu-id="99af7-174">Default values</span></span>

<span data-ttu-id="99af7-175">Siehe [Standardwerte](variables.md#default-values), verschiedene Arten von Variablen werden automatisch auf ihren Standardwert initialisiert, wenn sie erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="99af7-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="99af7-176">Für die Variablen der Klasse und anderen Verweistypen, der Standardwert ist `null`.</span><span class="sxs-lookup"><span data-stu-id="99af7-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="99af7-177">Aber da Strukturen Werttypen sind, die nicht möglich `null`, der Standardwert einer Struktur ist der Wert, der erstellt werden, indem alle Werttypfelder auf ihre Standardwerte und alle Verweise auf Felder festlegen. `null`.</span><span class="sxs-lookup"><span data-stu-id="99af7-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="99af7-178">Verweisen auf die `Point` Struktur deklariert werden, im Beispiel oben</span><span class="sxs-lookup"><span data-stu-id="99af7-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="99af7-179">Initialisiert alle `Point` in das Array, das den Wert ergibt, wenn die `x` und `y` Felder auf 0 (null).</span><span class="sxs-lookup"><span data-stu-id="99af7-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="99af7-180">Der Standardwert einer Struktur entspricht dem Wert, der vom Standardkonstruktor der Struktur zurückgegeben ([Standardkonstruktoren](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="99af7-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="99af7-181">Im Gegensatz zu einer Klasse ist eine Struktur nicht zulässig, einen parameterlosen Instanzenkonstruktor deklarieren.</span><span class="sxs-lookup"><span data-stu-id="99af7-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="99af7-182">Stattdessen enthält jede Struktur implizit einen parameterlosen Instanzenkonstruktor die immer den Wert, die zurückgibt aus alle Werttypfelder auf ihre Standardwerte und alle Verweise auf Felder festlegen. `null`.</span><span class="sxs-lookup"><span data-stu-id="99af7-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="99af7-183">Strukturen sollten so entworfen werden, die Standard-Initialisierungszustand mit einem gültigen Zustand berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="99af7-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="99af7-184">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="99af7-184">In the example</span></span>
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
<span data-ttu-id="99af7-185">der Instanzkonstruktor für eine benutzerdefinierte schützt vor null-Werte nur, wenn es explizit aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="99af7-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="99af7-186">In Fällen, in denen eine `KeyValuePair` Variable unterliegt standardinitialisierung-Wert, der `key` und `value` Felder ist null, und die Struktur muss vorbereitet werden, um diesen Zustand zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="99af7-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="99af7-187">Boxing und Unboxing</span><span class="sxs-lookup"><span data-stu-id="99af7-187">Boxing and unboxing</span></span>

<span data-ttu-id="99af7-188">Der Wert ein Klassentyp konvertiert werden kann, Typ `object` oder in einen Schnittstellentyp, die von der Klasse implementiert wird, indem Sie einfach zum Behandeln des Verweis als einen anderen Typ zum Zeitpunkt der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="99af7-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="99af7-189">Ebenso wird ein Wert vom Typ `object` oder ein Wert eines Schnittstellentyps wieder in einen Klassentyp konvertiert werden kann, ohne den Verweis (aber natürlich eine Laufzeittyp-Überprüfung ist erforderlich, in diesem Fall).</span><span class="sxs-lookup"><span data-stu-id="99af7-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="99af7-190">Da Strukturen keine Verweistypen sind, werden diese Vorgänge für Strukturtypen anders implementiert.</span><span class="sxs-lookup"><span data-stu-id="99af7-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="99af7-191">Wenn der Wert einen Strukturtyp in den Typ konvertiert wird `object` oder in einen Schnittstellentyp, der durch die Struktur implementiert wird, ein Boxing-Vorgang stattfindet.</span><span class="sxs-lookup"><span data-stu-id="99af7-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="99af7-192">Ebenso, wenn ein Wert vom Typ `object` oder ein Wert eines Schnittstellentyps wieder in einen Strukturtyp konvertiert wird, ein unboxing-Vorgang stattfindet.</span><span class="sxs-lookup"><span data-stu-id="99af7-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="99af7-193">Ein wichtiger Unterschied zu der gleichen Vorgänge für Klassentypen ist, dass das boxing und unboxing den Strukturwert entweder in oder aus der geschachtelten Instanz kopiert.</span><span class="sxs-lookup"><span data-stu-id="99af7-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="99af7-194">Daher werden nach einem Boxing- oder unboxing-Vorgang an die nicht geschachtelten Struktur vorgenommenen Änderungen nicht in der geschachtelten Struktur wiedergegeben.</span><span class="sxs-lookup"><span data-stu-id="99af7-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="99af7-195">Wenn ein Strukturtyp überschreibt eine virtuelle Methode geerbt von `System.Object` (wie z. B. `Equals`, `GetHashCode`, oder `ToString`), Aufruf der virtuellen Methode durch eine Instanz des Strukturtyps verursacht keine Boxing auftreten.</span><span class="sxs-lookup"><span data-stu-id="99af7-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="99af7-196">Dies gilt auch, wenn die Struktur, wie ein Typparameter verwendet wird und der Aufruf durch eine Instanz des Type-Parameter erfolgt.</span><span class="sxs-lookup"><span data-stu-id="99af7-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="99af7-197">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="99af7-197">For example:</span></span>
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="99af7-198">Die Ausgabe des Programms lautet:</span><span class="sxs-lookup"><span data-stu-id="99af7-198">The output of the program is:</span></span>
```
1
2
3
```

<span data-ttu-id="99af7-199">Ungültiges Format für zwar `ToString` um Nebeneffekte haben, im Beispiel wird veranschaulicht, dass keine Boxing-Konvertierung für die drei Aufrufe von aufgetreten `x.ToString()`.</span><span class="sxs-lookup"><span data-stu-id="99af7-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="99af7-200">Auf ähnliche Weise beim boxing nie implizit ein Element in einem eingeschränkten Typparameter zugreifen auf.</span><span class="sxs-lookup"><span data-stu-id="99af7-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="99af7-201">Nehmen wir beispielsweise an eine Schnittstelle `ICounter` enthält eine Methode `Increment` die können verwendet werden, um einen Wert zu ändern.</span><span class="sxs-lookup"><span data-stu-id="99af7-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="99af7-202">Wenn `ICounter` dient als eine Einschränkung, die Implementierung der `Increment` Methode wird aufgerufen, mit einem Verweis auf die Variable, `Increment` für noch nie eine geschachtelte Kopie wurde aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="99af7-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="99af7-203">Der erste Aufruf `Increment` ändert den Wert in der Variablen `x`.</span><span class="sxs-lookup"><span data-stu-id="99af7-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="99af7-204">Dies entspricht nicht der zweite Aufruf von `Increment`, die geändert, dass des Wert in eine geschachtelte Kopie von `x`.</span><span class="sxs-lookup"><span data-stu-id="99af7-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="99af7-205">Daher ist die Ausgabe des Programms:</span><span class="sxs-lookup"><span data-stu-id="99af7-205">Thus, the output of the program is:</span></span>
```
0
1
1
```

<span data-ttu-id="99af7-206">Weitere Informationen zu Boxing und unboxing zu erhalten, finden Sie unter [Boxing und unboxing](types.md#boxing-and-unboxing).</span><span class="sxs-lookup"><span data-stu-id="99af7-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="99af7-207">Bedeutung</span><span class="sxs-lookup"><span data-stu-id="99af7-207">Meaning of this</span></span>

<span data-ttu-id="99af7-208">Innerhalb eines Instanzkonstruktors oder Funktionsmember der Member einer Klasse, Instanz `this` wird als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="99af7-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="99af7-209">Daher zwar `this` kann verwendet werden, verweisen Sie auf der Instanz für das das Funktionselement aufgerufen wurde, ist es nicht möglich, Zuweisen `this` in ein Funktionsmember der Member einer Klasse.</span><span class="sxs-lookup"><span data-stu-id="99af7-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="99af7-210">In einem Instanzenkonstruktor der eine Struktur `this` entspricht einer `out` Parameter des Strukturtyps und innerhalb einer Instanz Funktionsmember der Member einer Struktur, `this` entspricht einem `ref` Parameter des Strukturtyps.</span><span class="sxs-lookup"><span data-stu-id="99af7-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="99af7-211">In beiden Fällen `this` wird als Variable klassifiziert, und es ist möglich, die gesamte Struktur ändern, für die das Funktionselement aufgerufen wurde, indem zuweisen `this` oder durch Übergabe als eine `ref` oder `out` Parameter.</span><span class="sxs-lookup"><span data-stu-id="99af7-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="99af7-212">Feldinitialisierer</span><span class="sxs-lookup"><span data-stu-id="99af7-212">Field initializers</span></span>

<span data-ttu-id="99af7-213">Siehe [Standardwerte](structs.md#default-values), der Wert, der ergibt, wenn alle Werttypfelder auf ihre Standardwerte und alle Verweise auf Felder festlegen. der Standardwert einer Struktur besteht aus `null`.</span><span class="sxs-lookup"><span data-stu-id="99af7-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="99af7-214">Aus diesem Grund lässt eine Struktur nicht Felddeklarationen Variableninitialisierern eingeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="99af7-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="99af7-215">Diese Einschränkung gilt nur für Instanzfelder.</span><span class="sxs-lookup"><span data-stu-id="99af7-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="99af7-216">Statische Felder einer Struktur Variable Initialisierer enthalten dürfen.</span><span class="sxs-lookup"><span data-stu-id="99af7-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="99af7-217">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="99af7-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="99af7-218">Fehler ist, da die Instanz Felddeklarationen Variableninitialisierern enthalten.</span><span class="sxs-lookup"><span data-stu-id="99af7-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="99af7-219">Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="99af7-219">Constructors</span></span>

<span data-ttu-id="99af7-220">Im Gegensatz zu einer Klasse ist eine Struktur nicht zulässig, einen parameterlosen Instanzenkonstruktor deklarieren.</span><span class="sxs-lookup"><span data-stu-id="99af7-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="99af7-221">Stattdessen enthält jede Struktur implizit einen parameterlosen Instanzenkonstruktor die immer den Wert, die zurückgibt aus alle Werttypfelder auf ihre Standardwerte und alle Verweise auf null-Feldern festlegen ([Standardkonstruktoren](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="99af7-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="99af7-222">Eine Struktur kann Instanzkonstruktoren Parametern deklarieren.</span><span class="sxs-lookup"><span data-stu-id="99af7-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="99af7-223">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="99af7-223">For example</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

<span data-ttu-id="99af7-224">Wenn die obige Deklaration, die Anweisungen</span><span class="sxs-lookup"><span data-stu-id="99af7-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="99af7-225">Erstellen einer `Point` mit `x` und `y` auf 0 (null) initialisiert.</span><span class="sxs-lookup"><span data-stu-id="99af7-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="99af7-226">Ein Strukturkonstruktor-Instanz ist nicht zulässig, einem Konstruktorinitialisierer des Formulars einschließen `base(...)`.</span><span class="sxs-lookup"><span data-stu-id="99af7-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="99af7-227">Wenn die Struktur Instanzkonstruktor einem Konstruktorinitialisierer angeben, nicht der `this` Variable entspricht ein `out` -Parameter des Strukturtyps und ähnelt ein `out` Parameter `this` müssen (definitiv zugewiesen werden [Definitive Zuweisung](variables.md#definite-assignment)) an jedem Standort, in den Konstruktor zurück.</span><span class="sxs-lookup"><span data-stu-id="99af7-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="99af7-228">Wenn der Struktur-Instanzkonstruktor einem Konstruktorinitialisierer gibt an, die `this` Variable entspricht eine `ref` -Parameter des Strukturtyps und ähnelt ein `ref` -Parameter `this` gilt als definitiv zugewiesen, auf der Eintrag in den Text des Konstruktors.</span><span class="sxs-lookup"><span data-stu-id="99af7-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="99af7-229">Beachten Sie die Instanz Konstruktorimplementierung unten:</span><span class="sxs-lookup"><span data-stu-id="99af7-229">Consider the instance constructor implementation below:</span></span>
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

<span data-ttu-id="99af7-230">Keine Instanz-Member-Funktion (einschließlich der Set-Accessoren für die Eigenschaften `X` und `Y`) aufgerufen werden kann, bis alle Felder der Struktur, die erstellt wird definitiv zugewiesen wurden.</span><span class="sxs-lookup"><span data-stu-id="99af7-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="99af7-231">Die einzige Ausnahme umfasst automatisch implementierte Eigenschaften ([automatisch implementierten Eigenschaften](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="99af7-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="99af7-232">Die Regeln für definitive Zuweisungen ([Ausdrücke für einfache Zuweisung](variables.md#simple-assignment-expressions)) schließen Sie die Zuweisung zu einer Auto-Eigenschaft eines Strukturtyps innerhalb eines Instanzkonstruktors dieses Strukturtyps speziell: eine solche Zuweisung gilt eine definitive das ausgeblendete dahinter liegende Feld der Auto-Eigenschaft zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="99af7-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="99af7-233">Daher ist Folgendes zulässig:</span><span class="sxs-lookup"><span data-stu-id="99af7-233">Thus, the following is allowed:</span></span>

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a><span data-ttu-id="99af7-234">Destruktoren</span><span class="sxs-lookup"><span data-stu-id="99af7-234">Destructors</span></span>

<span data-ttu-id="99af7-235">Eine Struktur ist nicht zulässig, einen Destruktor deklarieren.</span><span class="sxs-lookup"><span data-stu-id="99af7-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="99af7-236">Statische Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="99af7-236">Static constructors</span></span>

<span data-ttu-id="99af7-237">Statische Konstruktoren für Strukturen führen Sie die meisten hier dieselben Regeln wie bei Klassen.</span><span class="sxs-lookup"><span data-stu-id="99af7-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="99af7-238">Die Ausführung eines statischen Konstruktors für einen Strukturtyp wird durch das erste der folgenden Ereignisse innerhalb einer Anwendungsdomäne ausgelöst:</span><span class="sxs-lookup"><span data-stu-id="99af7-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="99af7-239">Ein statischer Member des Strukturtyps ist auf die verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="99af7-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="99af7-240">Ein Konstruktor explizit deklarierter des Strukturtyps wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="99af7-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="99af7-241">Die Erstellung von Standardwerten ([Standardwerte](structs.md#default-values)) der Struktur löst Typen keinen statischen Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="99af7-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="99af7-242">(Ein Beispiel hierfür ist der Anfangswert der Elemente in einem Array.)</span><span class="sxs-lookup"><span data-stu-id="99af7-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="99af7-243">Beispiele für Strukturen</span><span class="sxs-lookup"><span data-stu-id="99af7-243">Struct examples</span></span>

<span data-ttu-id="99af7-244">Das folgende Beispiel zeigt zwei wichtige Beispiele für die Verwendung `struct` Typen zum Erstellen von Typen, die auf ähnliche Weise auf die vordefinierten Typen der Sprache, aber mit der geänderten Semantik verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="99af7-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="99af7-245">Datenbank-Integer-Datentyp</span><span class="sxs-lookup"><span data-stu-id="99af7-245">Database integer type</span></span>

<span data-ttu-id="99af7-246">Die `DBInt` -Struktur implementiert, einen ganzzahliger Typ, der den vollständigen Satz von Werten darstellen, kann die `int` -Typ sowie einen zusätzlichen Zustand, der einen unbekannten Wert angibt.</span><span class="sxs-lookup"><span data-stu-id="99af7-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="99af7-247">Ein Typ mit folgenden Merkmalen wird häufig in Datenbanken verwendet.</span><span class="sxs-lookup"><span data-stu-id="99af7-247">A type with these characteristics is commonly used in databases.</span></span>

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a><span data-ttu-id="99af7-248">Datenbank-boolean-Typ</span><span class="sxs-lookup"><span data-stu-id="99af7-248">Database boolean type</span></span>

<span data-ttu-id="99af7-249">Die `DBBool` folgende Struktur implementiert einen logischen dreiwertige-Typ.</span><span class="sxs-lookup"><span data-stu-id="99af7-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="99af7-250">Die möglichen Werte dieses Typs sind `DBBool.True`, `DBBool.False`, und `DBBool.Null`, wobei die `Null` Element gibt einen unbekannten Wert an.</span><span class="sxs-lookup"><span data-stu-id="99af7-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="99af7-251">Diese logischen dreiwertige-Typen werden häufig in Datenbanken verwendet.</span><span class="sxs-lookup"><span data-stu-id="99af7-251">Such three-valued logical types are commonly used in databases.</span></span>

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
