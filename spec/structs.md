---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704028"
---
# <a name="structs"></a><span data-ttu-id="ba241-101">Strukturen</span><span class="sxs-lookup"><span data-stu-id="ba241-101">Structs</span></span>

<span data-ttu-id="ba241-102">Strukturen ähneln Klassen darin, dass Sie Datenstrukturen darstellen, die Datenmember und Funktionsmember enthalten können.</span><span class="sxs-lookup"><span data-stu-id="ba241-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="ba241-103">Im Unterschied zu Klassen sind Strukturen jedoch Werttypen und erfordern keine Heap Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="ba241-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="ba241-104">Eine Variable eines Struktur Typs enthält direkt die Daten der Struktur, während eine Variable eines Klassen Typs einen Verweis auf die Daten enthält, wobei es sich um ein Objekt handelt, das als Objekt bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="ba241-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="ba241-105">Strukturen sind besonders nützlich für kleine Datenstrukturen, die über Wertsemantik verfügen.</span><span class="sxs-lookup"><span data-stu-id="ba241-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="ba241-106">Komplexe Zahlen, Punkte in einem Koordinatensystem oder Schlüssel-Wert-Paare im Wörterbuch sind gute Beispiele für Strukturen.</span><span class="sxs-lookup"><span data-stu-id="ba241-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="ba241-107">Der Schlüssel für diese Datenstrukturen besteht darin, dass Sie nur wenige Datenmember haben, dass Sie keine Vererbung oder referenzielle Identität benötigen, und dass Sie mithilfe der Wert Semantik, bei der die Zuweisung den Wert anstelle des Verweises kopiert, bequem implementiert werden können.</span><span class="sxs-lookup"><span data-stu-id="ba241-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="ba241-108">Wie in [einfachen Typen](types.md#simple-types)beschrieben, sind die einfachen Typen, C#die von bereitgestellt werden, z. b. `int`, `double` und `bool`, tatsächlich alle Strukturtypen.</span><span class="sxs-lookup"><span data-stu-id="ba241-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="ba241-109">Ebenso wie diese vordefinierten Typen Strukturen sind, ist es auch möglich, Strukturen und Operator Überladung zu verwenden, um neue "primitive" Typen in der C# Sprache zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="ba241-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="ba241-110">Am Ende dieses Kapitels ([Struktur Beispiele](structs.md#struct-examples)) werden zwei Beispiele für solche Typen angegeben.</span><span class="sxs-lookup"><span data-stu-id="ba241-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="ba241-111">Struktur Deklarationen</span><span class="sxs-lookup"><span data-stu-id="ba241-111">Struct declarations</span></span>

<span data-ttu-id="ba241-112">Ein *struct_declaration* ist ein *type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), der eine neue Struktur deklariert:</span><span class="sxs-lookup"><span data-stu-id="ba241-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="ba241-113">Ein *struct_declaration* besteht aus einem optionalen Satz von *Attributen* ([Attributen](attributes.md)), gefolgt von einem optionalen Satz von *struct_modifier*s ([Strukturmodifizierer](structs.md#struct-modifiers)), gefolgt von einem optionalen `partial`-Modifizierer, gefolgt vom Schlüsselwort `struct` und ein *Bezeichner* , der die Struktur benennt, gefolgt von einer optionalen *type_parameter_list* -Spezifikation ([Typparameter](classes.md#type-parameters)), gefolgt von einer optionalen *struct_interfaces* -Spezifikation ([partiell -Modifizierer](structs.md#partial-modifier)), gefolgt von einer optionalen *type_parameter_constraints_clause*s-Spezifikation ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)), gefolgt von einem *struct_body* ([Struktur Text](structs.md#struct-body)), optional gefolgt von einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="ba241-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="ba241-114">Strukturmodifizierer</span><span class="sxs-lookup"><span data-stu-id="ba241-114">Struct modifiers</span></span>

<span data-ttu-id="ba241-115">Ein *struct_declaration* kann optional eine Sequenz von strukturmodifizierermodifizierer einschließen:</span><span class="sxs-lookup"><span data-stu-id="ba241-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

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

<span data-ttu-id="ba241-116">Es ist ein Kompilierzeitfehler, damit derselbe Modifizierer mehrmals in einer Struktur Deklaration angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ba241-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="ba241-117">Die Modifizierer einer Struktur Deklaration haben dieselbe Bedeutung wie die einer Klassen Deklaration ([Klassen Deklarationen](classes.md#class-declarations)).</span><span class="sxs-lookup"><span data-stu-id="ba241-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="ba241-118">Partieller Modifizierer</span><span class="sxs-lookup"><span data-stu-id="ba241-118">Partial modifier</span></span>

<span data-ttu-id="ba241-119">Der `partial`-Modifizierer gibt an, dass dieses *struct_declaration* eine partielle Typdeklaration ist.</span><span class="sxs-lookup"><span data-stu-id="ba241-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="ba241-120">Mehrere partielle Struktur Deklarationen mit demselben Namen innerhalb eines einschließenden Namespace oder einer Typdeklaration kombinieren eine Struktur Deklaration, die den in [partiellen Typen](classes.md#partial-types)angegebenen Regeln folgt.</span><span class="sxs-lookup"><span data-stu-id="ba241-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="ba241-121">Struktur Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="ba241-121">Struct interfaces</span></span>

<span data-ttu-id="ba241-122">Eine Struktur Deklaration kann eine *struct_interfaces* -Spezifikation enthalten. in diesem Fall wird die Struktur so bezeichnet, dass die angegebenen Schnittstellentypen direkt implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="ba241-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="ba241-123">Schnittstellen Implementierungen werden in [Schnittstellen Implementierungen](interfaces.md#interface-implementations)ausführlicher erläutert.</span><span class="sxs-lookup"><span data-stu-id="ba241-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="ba241-124">Struktur Text</span><span class="sxs-lookup"><span data-stu-id="ba241-124">Struct body</span></span>

<span data-ttu-id="ba241-125">Der *struct_body* einer Struktur definiert die Member der Struktur.</span><span class="sxs-lookup"><span data-stu-id="ba241-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="ba241-126">Strukturmember</span><span class="sxs-lookup"><span data-stu-id="ba241-126">Struct members</span></span>

<span data-ttu-id="ba241-127">Die Member einer Struktur bestehen aus den Membern, die von den *struct_member_declaration*s eingeführt wurden, und den Membern, die vom Typ `System.ValueType` geerbt wurden.</span><span class="sxs-lookup"><span data-stu-id="ba241-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

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

<span data-ttu-id="ba241-128">Mit Ausnahme der Unterschiede in den [Klassen-und Struktur unterschieden](structs.md#class-and-struct-differences)gelten die Beschreibungen von Klassenmembern, die in [Klassenmembern](classes.md#class-members) durch [Iteratoren](classes.md#iterators) bereitgestellt werden, auch für Strukturmember.</span><span class="sxs-lookup"><span data-stu-id="ba241-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="ba241-129">Klassen-und Strukturunterschiede</span><span class="sxs-lookup"><span data-stu-id="ba241-129">Class and struct differences</span></span>

<span data-ttu-id="ba241-130">Strukturen unterscheiden sich in mehreren wichtigen Punkten von Klassen:</span><span class="sxs-lookup"><span data-stu-id="ba241-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="ba241-131">Strukturen sind Werttypen ([Wert Semantik](structs.md#value-semantics)).</span><span class="sxs-lookup"><span data-stu-id="ba241-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="ba241-132">Alle Strukturtypen erben implizit von der-Klasse `System.ValueType` ([Vererbung](structs.md#inheritance)).</span><span class="sxs-lookup"><span data-stu-id="ba241-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="ba241-133">Durch die Zuweisung zu einer Variablen eines Struktur Typs wird eine Kopie des zugewiesenen Werts ([Zuweisung](structs.md#assignment)) erstellt.</span><span class="sxs-lookup"><span data-stu-id="ba241-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="ba241-134">Der Standardwert einer Struktur ist der Wert, der erzeugt wird, indem alle Werttyp Felder auf ihren Standardwert und alle Verweistyp Felder auf `null` ([Standardwerte](structs.md#default-values)) festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="ba241-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="ba241-135">Boxing-und Unboxing-Vorgänge werden verwendet, um zwischen einem Strukturtyp und `object` ([Boxing und Unboxing](structs.md#boxing-and-unboxing)) zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="ba241-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="ba241-136">Die Bedeutung von "`this`" unterscheidet sich für Strukturen ([dieser Zugriff](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="ba241-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="ba241-137">Instanzfelddeklarationen für eine Struktur dürfen keine Variableninitialisierer ([Feldinitialisierer](structs.md#field-initializers)) enthalten.</span><span class="sxs-lookup"><span data-stu-id="ba241-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="ba241-138">Eine Struktur darf keinen Parameter losen Instanzenkonstruktor ([Konstruktoren](structs.md#constructors)) deklarieren.</span><span class="sxs-lookup"><span data-stu-id="ba241-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="ba241-139">Es ist nicht zulässig, einen destrukturtor ([destrukturtoren](structs.md#destructors)) zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="ba241-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="ba241-140">Wert Semantik</span><span class="sxs-lookup"><span data-stu-id="ba241-140">Value semantics</span></span>

<span data-ttu-id="ba241-141">Strukturen sind Werttypen ([Werttypen](types.md#value-types)) und werden als Wert Semantik bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="ba241-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="ba241-142">Klassen hingegen sind Verweis Typen ([Verweis Typen](types.md#reference-types)) und werden als Verweis Semantik bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="ba241-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="ba241-143">Eine Variable eines Struktur Typs enthält direkt die Daten der Struktur, während eine Variable eines Klassen Typs einen Verweis auf die Daten enthält, wobei es sich um ein Objekt handelt, das als Objekt bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="ba241-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="ba241-144">Wenn eine Struktur `B` ein Instanzfeld vom Typ `A` und `A` ein Strukturtyp ist, ist dies ein Kompilierzeitfehler, damit `A` von `B` oder einem von `B` erstellten Typ abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="ba241-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="ba241-145">Eine Struktur `X` ist ***direkt von*** einer Struktur abhängig `Y`, wenn `X` ein Instanzfeld vom Typ `Y` enthält.</span><span class="sxs-lookup"><span data-stu-id="ba241-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="ba241-146">Bei dieser Definition ist der gesamte Satz von Strukturen, von denen eine Struktur abhängt, das transitiv Schließen der direkt von der Beziehung ***abhängig*** .</span><span class="sxs-lookup"><span data-stu-id="ba241-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="ba241-147">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ba241-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="ba241-148">ist ein Fehler, da `Node` ein Instanzfeld seines eigenen Typs enthält.</span><span class="sxs-lookup"><span data-stu-id="ba241-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="ba241-149">Ein weiteres Beispiel</span><span class="sxs-lookup"><span data-stu-id="ba241-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="ba241-150">ist ein Fehler, da jeder der Typen `A`, `B` und `C` voneinander abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="ba241-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="ba241-151">Mit-Klassen können zwei Variablen auf das gleiche Objekt verweisen, und so können Vorgänge in einer Variablen das Objekt beeinflussen, auf das von der anderen Variablen verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="ba241-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="ba241-152">Bei Strukturen verfügen die Variablen jeweils über eine eigene Kopie der Daten (außer im Fall der Parameter Variablen "`ref`" und "`out`"), und es ist nicht möglich, dass Vorgänge auf einem anderen die anderen beeinflussen.</span><span class="sxs-lookup"><span data-stu-id="ba241-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="ba241-153">Da Strukturen keine Verweis Typen sind, ist es nicht möglich, dass die Werte eines Struktur Typs `null` sind.</span><span class="sxs-lookup"><span data-stu-id="ba241-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="ba241-154">Bei Angabe der Deklaration</span><span class="sxs-lookup"><span data-stu-id="ba241-154">Given the declaration</span></span>
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
<span data-ttu-id="ba241-155">das Code Fragment</span><span class="sxs-lookup"><span data-stu-id="ba241-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="ba241-156">Gibt den Wert `10` aus.</span><span class="sxs-lookup"><span data-stu-id="ba241-156">outputs the value `10`.</span></span> <span data-ttu-id="ba241-157">Durch die Zuweisung von `a` zu `b` wird eine Kopie des Werts erstellt, und `b` ist von der Zuweisung zu `a.x` nicht betroffen.</span><span class="sxs-lookup"><span data-stu-id="ba241-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="ba241-158">Hätte `Point` stattdessen als Klasse deklariert, würde die Ausgabe `100` lauten, da `a` und `b` auf das gleiche Objekt verweisen würden.</span><span class="sxs-lookup"><span data-stu-id="ba241-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="ba241-159">Vererbung</span><span class="sxs-lookup"><span data-stu-id="ba241-159">Inheritance</span></span>

<span data-ttu-id="ba241-160">Alle Strukturtypen erben implizit von der-Klasse `System.ValueType`, die wiederum von der-Klasse `object` erbt.</span><span class="sxs-lookup"><span data-stu-id="ba241-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="ba241-161">Eine Struktur Deklaration kann eine Liste der implementierten Schnittstellen angeben, aber es ist nicht möglich, dass eine Struktur Deklaration eine Basisklasse angibt.</span><span class="sxs-lookup"><span data-stu-id="ba241-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="ba241-162">Strukturtypen sind niemals abstrakt und sind immer implizit versiegelt.</span><span class="sxs-lookup"><span data-stu-id="ba241-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="ba241-163">Die Modifizierer "`abstract`" und "`sealed`" sind daher in einer Struktur Deklaration nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="ba241-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="ba241-164">Da die Vererbung für Strukturen nicht unterstützt wird, kann die deklarierte Barrierefreiheit eines Strukturmembers nicht `protected` oder `protected internal` sein.</span><span class="sxs-lookup"><span data-stu-id="ba241-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="ba241-165">Funktionsmember in einer Struktur können nicht `abstract` oder `virtual` sein, und der `override`-Modifizierer darf nur Methoden überschreiben, die von `System.ValueType` geerbt wurden.</span><span class="sxs-lookup"><span data-stu-id="ba241-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="ba241-166">Zuweisung</span><span class="sxs-lookup"><span data-stu-id="ba241-166">Assignment</span></span>

<span data-ttu-id="ba241-167">Durch die Zuweisung zu einer Variablen eines Struktur Typs wird eine Kopie des zugewiesenen Werts erstellt.</span><span class="sxs-lookup"><span data-stu-id="ba241-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="ba241-168">Dies unterscheidet sich von der Zuweisung zu einer Variablen eines Klassen Typs, die den Verweis, aber nicht das durch den Verweis identifizierte Objekt kopiert.</span><span class="sxs-lookup"><span data-stu-id="ba241-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="ba241-169">Ähnlich wie bei einer Zuweisung wird eine Kopie der Struktur erstellt, wenn eine Struktur als Wert Parameter übergeben oder als Ergebnis eines Funktionsmembers zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="ba241-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="ba241-170">Eine Struktur kann mithilfe eines `ref`-oder `out`-Parameters als Verweis an einen Funktionsmember übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="ba241-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="ba241-171">Wenn eine Eigenschaft oder ein Indexer einer Struktur das Ziel einer Zuweisung ist, muss der Instanzausdruck, der der Eigenschaft oder dem Indexer-Zugriff zugeordnet ist, als Variable klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="ba241-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="ba241-172">Wenn der Instanzausdruck als Wert klassifiziert wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="ba241-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="ba241-173">Dies wird in " [einfache Zuweisung](expressions.md#simple-assignment)" ausführlicher beschrieben.</span><span class="sxs-lookup"><span data-stu-id="ba241-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="ba241-174">Standardwerte</span><span class="sxs-lookup"><span data-stu-id="ba241-174">Default values</span></span>

<span data-ttu-id="ba241-175">Wie in [Standardwerte](variables.md#default-values)beschrieben, werden bei der Erstellung automatisch verschiedene Arten von Variablen auf ihren Standardwert initialisiert.</span><span class="sxs-lookup"><span data-stu-id="ba241-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="ba241-176">Für Variablen von Klassentypen und anderen Verweis Typen ist dieser Standardwert `null`.</span><span class="sxs-lookup"><span data-stu-id="ba241-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="ba241-177">Da Strukturen jedoch Werttypen sind, die nicht `null` sein können, ist der Standardwert einer Struktur der Wert, der erzeugt wird, indem alle Werttyp Felder auf ihren Standardwert und alle Verweistyp Felder auf `null` festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="ba241-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="ba241-178">Im Beispiel wird auf die oben deklarierte `Point`-Struktur verwiesen.</span><span class="sxs-lookup"><span data-stu-id="ba241-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="ba241-179">Initialisiert jede `Point` im-Array mit dem Wert, der erstellt wird, indem die Felder `x` und `y` auf NULL festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="ba241-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="ba241-180">Der Standardwert einer Struktur entspricht dem Wert, der vom Standardkonstruktor der Struktur zurückgegeben wird ([Standardkonstruktoren](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="ba241-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="ba241-181">Anders als bei einer Klasse ist es nicht zulässig, einen Parameter losen Instanzkonstruktor zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="ba241-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="ba241-182">Stattdessen verfügt jede Struktur implizit über einen Parameter losen Instanzenkonstruktor, der immer den Wert zurückgibt, der sich aus dem Festlegen aller Werttyp Felder auf ihren Standardwert und alle Verweistyp Felder auf `null` ergibt.</span><span class="sxs-lookup"><span data-stu-id="ba241-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="ba241-183">Strukturen sollten so entworfen werden, dass der standardmäßige Initialisierungs Zustand einen gültigen Status aufweist.</span><span class="sxs-lookup"><span data-stu-id="ba241-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="ba241-184">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="ba241-184">In the example</span></span>
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
<span data-ttu-id="ba241-185">der benutzerdefinierte Instanzkonstruktor schützt nur bei NULL-Werten, wenn er explizit aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="ba241-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="ba241-186">In Fällen, in denen eine `KeyValuePair`-Variable der Standardwert Initialisierung unterliegt, sind die Felder `key` und `value` NULL, und die Struktur muss darauf vorbereitet sein, diesen Zustand zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="ba241-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="ba241-187">Boxing und Unboxing</span><span class="sxs-lookup"><span data-stu-id="ba241-187">Boxing and unboxing</span></span>

<span data-ttu-id="ba241-188">Ein Wert eines Klassen Typs kann in den Typ `object` oder in einen Schnittstellentyp konvertiert werden, der von der-Klasse implementiert wird, indem der Verweis zum Zeitpunkt der Kompilierung als anderer Typ behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="ba241-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="ba241-189">Ebenso kann ein Wert vom Typ "`object`" oder ein Wert eines Schnittstellen Typs wieder in einen Klassentyp konvertiert werden, ohne den Verweis zu ändern (in diesem Fall ist jedoch eine Lauf Zeittyp Überprüfung erforderlich).</span><span class="sxs-lookup"><span data-stu-id="ba241-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="ba241-190">Da Strukturen keine Verweis Typen sind, werden diese Vorgänge für Strukturtypen unterschiedlich implementiert.</span><span class="sxs-lookup"><span data-stu-id="ba241-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="ba241-191">Wenn ein Wert eines Strukturtyps in den Typ `object` oder in einen Schnittstellentyp konvertiert wird, der von der Struktur implementiert wird, wird ein Boxing-Vorgang durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="ba241-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="ba241-192">Wenn ein Wert des Typs `object` oder ein Wert eines Schnittstellen Typs zurück in einen Strukturtyp konvertiert wird, erfolgt auch ein Unboxing-Vorgang.</span><span class="sxs-lookup"><span data-stu-id="ba241-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="ba241-193">Ein wichtiger Unterschied von denselben Vorgängen für Klassentypen besteht darin, dass Boxing und Unboxing den Struktur Wert entweder in die oder aus der geachtelten Instanz kopieren.</span><span class="sxs-lookup"><span data-stu-id="ba241-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="ba241-194">Folglich werden Änderungen an der Unboxing-Struktur, die an der Unboxing-Struktur vorgenommen werden, nicht in der Struktur der Struktur spiegele widergespiegelt.</span><span class="sxs-lookup"><span data-stu-id="ba241-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="ba241-195">Wenn ein Strukturtyp eine virtuelle Methode überschreibt, die von `System.Object` geerbt wurde (z. b. `Equals`, `GetHashCode` oder `ToString`), bewirkt der Aufruf der virtuellen Methode über eine Instanz des Struktur Typs nicht, dass Boxing erfolgt.</span><span class="sxs-lookup"><span data-stu-id="ba241-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="ba241-196">Dies gilt auch, wenn die Struktur als Typparameter verwendet wird und der Aufruf durch eine Instanz des Typparameter Typs erfolgt.</span><span class="sxs-lookup"><span data-stu-id="ba241-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="ba241-197">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ba241-197">For example:</span></span>
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

<span data-ttu-id="ba241-198">Die Ausgabe des Programms lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="ba241-198">The output of the program is:</span></span>
```console
1
2
3
```

<span data-ttu-id="ba241-199">Obwohl es für `ToString` nicht möglich ist, Nebeneffekte zu haben, zeigt das Beispiel, dass kein Boxing für die drei Aufrufe von `x.ToString()` aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="ba241-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="ba241-200">Auf ähnliche Weise tritt beim Zugriff auf ein Element in einem eingeschränkten Typparameter nie implizit ein Boxing auf.</span><span class="sxs-lookup"><span data-stu-id="ba241-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="ba241-201">Angenommen, eine Schnittstelle `ICounter` enthält eine Methode `Increment`, die zum Ändern eines Werts verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="ba241-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="ba241-202">Wenn `ICounter` als Einschränkung verwendet wird, wird die Implementierung der `Increment`-Methode mit einem Verweis auf die Variable aufgerufen, für die `Increment` aufgerufen wurde, nie eine gepackte Kopie.</span><span class="sxs-lookup"><span data-stu-id="ba241-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

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

<span data-ttu-id="ba241-203">Beim ersten `Increment`-Aufrufwert wird der Wert in der Variablen `x` geändert.</span><span class="sxs-lookup"><span data-stu-id="ba241-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="ba241-204">Dies entspricht nicht dem zweiten `Increment`-Aufrufwert, durch den der Wert in einer geboxten Kopie von `x` geändert wird.</span><span class="sxs-lookup"><span data-stu-id="ba241-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="ba241-205">Folglich lautet die Ausgabe des Programms wie folgt:</span><span class="sxs-lookup"><span data-stu-id="ba241-205">Thus, the output of the program is:</span></span>
```console
0
1
1
```

<span data-ttu-id="ba241-206">Weitere Informationen zu Boxing und Unboxing finden Sie unter [Boxing und Unboxing](types.md#boxing-and-unboxing).</span><span class="sxs-lookup"><span data-stu-id="ba241-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="ba241-207">Bedeutung dieses</span><span class="sxs-lookup"><span data-stu-id="ba241-207">Meaning of this</span></span>

<span data-ttu-id="ba241-208">Innerhalb eines Instanzkonstruktors oder Instanzfunktionsmembers einer Klasse wird `this` als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="ba241-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="ba241-209">Wenn `this` verwendet werden kann, um auf die Instanz zu verweisen, für die das Funktionsmember aufgerufen wurde, ist es daher nicht möglich, `this` in einem Funktionsmember einer Klasse zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="ba241-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="ba241-210">Innerhalb eines Instanzkonstruktors einer Struktur entspricht `this` einem `out`-Parameter des Struktur Typs, und innerhalb eines Instanzfunktionsmembers einer Struktur entspricht `this` einem `ref`-Parameter des Struktur Typs.</span><span class="sxs-lookup"><span data-stu-id="ba241-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="ba241-211">In beiden Fällen wird `this` als Variable klassifiziert, und es ist möglich, die gesamte Struktur zu ändern, für die der Funktionsmember aufgerufen wurde, indem er `this` zugewiesen wurde oder indem er als `ref`-oder `out`-Parameter übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="ba241-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="ba241-212">Feldinitialisierer</span><span class="sxs-lookup"><span data-stu-id="ba241-212">Field initializers</span></span>

<span data-ttu-id="ba241-213">Wie in [Standardwerte](structs.md#default-values)beschrieben, besteht der Standardwert einer Struktur aus dem Wert, der sich aus dem Festlegen aller Werttyp Felder auf den Standardwert und alle Verweistyp Felder auf `null` ergibt.</span><span class="sxs-lookup"><span data-stu-id="ba241-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="ba241-214">Aus diesem Grund lässt eine Struktur nicht zu, dass Instanzfelddeklarationen Variableninitialisierer einschließen.</span><span class="sxs-lookup"><span data-stu-id="ba241-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="ba241-215">Diese Einschränkung gilt nur für Instanzfelder.</span><span class="sxs-lookup"><span data-stu-id="ba241-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="ba241-216">Statische Felder einer Struktur dürfen Variableninitialisierer einschließen.</span><span class="sxs-lookup"><span data-stu-id="ba241-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="ba241-217">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="ba241-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="ba241-218">ist fehlerhaft, da die Instanzen Feld Deklarationen Variableninitialisierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="ba241-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="ba241-219">Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="ba241-219">Constructors</span></span>

<span data-ttu-id="ba241-220">Anders als bei einer Klasse ist es nicht zulässig, einen Parameter losen Instanzkonstruktor zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="ba241-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="ba241-221">Stattdessen verfügt jede Struktur implizit über einen Parameter losen Instanzenkonstruktor, der immer den Wert zurückgibt, der sich aus dem Festlegen aller Werttyp Felder auf ihren Standardwert und alle Verweistyp Felder auf NULL ([Standardkonstruktoren](types.md#default-constructors)) ergibt.</span><span class="sxs-lookup"><span data-stu-id="ba241-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="ba241-222">Eine Struktur kann Instanzkonstruktoren mit Parametern deklarieren.</span><span class="sxs-lookup"><span data-stu-id="ba241-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="ba241-223">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ba241-223">For example</span></span>
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

<span data-ttu-id="ba241-224">Bei der obigen Deklaration werden die Anweisungen</span><span class="sxs-lookup"><span data-stu-id="ba241-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="ba241-225">Beide erstellen eine `Point` mit `x` und `y` mit 0 (null) initialisiert.</span><span class="sxs-lookup"><span data-stu-id="ba241-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="ba241-226">Ein Strukturinstanzkonstruktor darf keinen Konstruktorinitialisierer in der Form `base(...)` einschließen.</span><span class="sxs-lookup"><span data-stu-id="ba241-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="ba241-227">Wenn der Strukturinstanzkonstruktor keinen Konstruktorinitialisierer angibt, entspricht die `this`-Variable einem `out`-Parameter des Struktur Typs. ähnlich wie bei einem `out`-Parameter muss `this` definitiv zugewiesen werden ([definitive Zuweisung). ](variables.md#definite-assignment)) an jedem Speicherort, an dem der Konstruktor zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="ba241-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="ba241-228">Wenn der Strukturinstanzkonstruktor einen Konstruktorinitialisierer angibt, entspricht die `this`-Variable einem `ref`-Parameter des Struktur Typs. ähnlich wie bei einem `ref`-Parameter wird `this` als definitiv beim Eintrag dem Konstruktortext zugewiesen. .</span><span class="sxs-lookup"><span data-stu-id="ba241-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="ba241-229">Beachten Sie die folgende instanzkonstruktorimplementierung:</span><span class="sxs-lookup"><span data-stu-id="ba241-229">Consider the instance constructor implementation below:</span></span>
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

<span data-ttu-id="ba241-230">Es kann keine Instanzmember-Funktion (einschließlich der Set-Accessoren für die Eigenschaften `X` und `Y`) aufgerufen werden, bis alle Felder der Struktur, die erstellt wird, definitiv zugewiesen wurden.</span><span class="sxs-lookup"><span data-stu-id="ba241-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="ba241-231">Die einzige Ausnahme betrifft automatisch implementierte Eigenschaften ([automatisch implementierte Eigenschaften](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="ba241-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="ba241-232">Die konkreten Zuweisungs Regeln ([einfache Zuweisungs Ausdrücke](variables.md#simple-assignment-expressions)) geben explizit die Zuweisung zu einer automatischen Eigenschaft eines Struktur Typs innerhalb eines Instanzkonstruktors dieses Struktur Typs aus: eine solche Zuweisung wird als eindeutige Zuweisung der ausgeblendeten dahinter liegendes Feld der Auto-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="ba241-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="ba241-233">Daher ist Folgendes zulässig:</span><span class="sxs-lookup"><span data-stu-id="ba241-233">Thus, the following is allowed:</span></span>

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

### <a name="destructors"></a><span data-ttu-id="ba241-234">Destruktoren</span><span class="sxs-lookup"><span data-stu-id="ba241-234">Destructors</span></span>

<span data-ttu-id="ba241-235">Es ist nicht zulässig, einen Dekonstruktor zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="ba241-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="ba241-236">Statische Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="ba241-236">Static constructors</span></span>

<span data-ttu-id="ba241-237">Statische Konstruktoren für Strukturen folgen den meisten der gleichen Regeln wie für-Klassen.</span><span class="sxs-lookup"><span data-stu-id="ba241-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="ba241-238">Die Ausführung eines statischen Konstruktors für einen Strukturtyp wird durch das erste der folgenden Ereignisse ausgelöst, die innerhalb einer Anwendungsdomäne auftreten:</span><span class="sxs-lookup"><span data-stu-id="ba241-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="ba241-239">Es wird auf einen statischen Member des Struktur Typs verwiesen.</span><span class="sxs-lookup"><span data-stu-id="ba241-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="ba241-240">Ein explizit deklarierter Konstruktor des Struktur Typs wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="ba241-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="ba241-241">Das Erstellen von Standardwerten ([Standardwerte](structs.md#default-values)) von Strukturtypen löst nicht den statischen Konstruktor aus.</span><span class="sxs-lookup"><span data-stu-id="ba241-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="ba241-242">(Ein Beispiel hierfür ist der Anfangswert von Elementen in einem Array.)</span><span class="sxs-lookup"><span data-stu-id="ba241-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="ba241-243">Struktur Beispiele</span><span class="sxs-lookup"><span data-stu-id="ba241-243">Struct examples</span></span>

<span data-ttu-id="ba241-244">Das folgende Beispiel zeigt zwei bedeutende Beispiele für die Verwendung von `struct`-Typen, um Typen zu erstellen, die ähnlich wie die vordefinierten Typen der Sprache verwendet werden können, jedoch mit einer geänderten Semantik.</span><span class="sxs-lookup"><span data-stu-id="ba241-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="ba241-245">Daten Bank Integertyp</span><span class="sxs-lookup"><span data-stu-id="ba241-245">Database integer type</span></span>

<span data-ttu-id="ba241-246">Die folgende `DBInt`-Struktur implementiert einen ganzzahligen Typ, der den gesamten Satz von Werten des `int`-Typs darstellen kann, zuzüglich eines zusätzlichen Zustands, der einen unbekannten Wert angibt.</span><span class="sxs-lookup"><span data-stu-id="ba241-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="ba241-247">Ein Typ mit diesen Merkmalen wird häufig in Datenbanken verwendet.</span><span class="sxs-lookup"><span data-stu-id="ba241-247">A type with these characteristics is commonly used in databases.</span></span>

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

### <a name="database-boolean-type"></a><span data-ttu-id="ba241-248">Boolescher Dateityp</span><span class="sxs-lookup"><span data-stu-id="ba241-248">Database boolean type</span></span>

<span data-ttu-id="ba241-249">In der folgenden `DBBool`-Struktur ist ein logischer Typ mit drei Werten implementiert.</span><span class="sxs-lookup"><span data-stu-id="ba241-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="ba241-250">Mögliche Werte dieses Typs sind `DBBool.True`, `DBBool.False` und `DBBool.Null`, wobei der `Null`-Member einen unbekannten Wert angibt.</span><span class="sxs-lookup"><span data-stu-id="ba241-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="ba241-251">Solche logischen Typen mit drei Werten werden häufig in Datenbanken verwendet.</span><span class="sxs-lookup"><span data-stu-id="ba241-251">Such three-valued logical types are commonly used in databases.</span></span>

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
