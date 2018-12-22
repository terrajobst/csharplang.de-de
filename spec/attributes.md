# <a name="attributes"></a><span data-ttu-id="9a3ef-101">Attribute</span><span class="sxs-lookup"><span data-stu-id="9a3ef-101">Attributes</span></span>

<span data-ttu-id="9a3ef-102">Ein Großteil der C#-Sprache ermöglicht es den Programmierer, geben Sie die deklarative Informationen zu den Entitäten, die im Programm definiert.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-102">Much of the C# language enables the programmer to specify declarative information about the entities defined in the program.</span></span> <span data-ttu-id="9a3ef-103">Z. B. der Zugriff auf eine Methode in einer Klasse angegeben ist, werden, indem diese mit der *Method_modifier*s `public`, `protected`, `internal`, und `private`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-103">For example, the accessibility of a method in a class is specified by decorating it with the *method_modifier*s `public`, `protected`, `internal`, and `private`.</span></span>

<span data-ttu-id="9a3ef-104">C# ermöglicht es Programmierern, neue Arten von deklarativer Informationen, so genannte erfinden ***Attribute***.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-104">C# enables programmers to invent new kinds of declarative information, called ***attributes***.</span></span> <span data-ttu-id="9a3ef-105">Programmierer können klicken Sie dann Attribute an verschiedene anfügen, und rufen Informationen über Bildattribute in einer Laufzeitumgebung.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-105">Programmers can then attach attributes to various program entities, and retrieve attribute information in a run-time environment.</span></span> <span data-ttu-id="9a3ef-106">Beispielsweise kann ein Framework definieren eine `HelpAttribute` -Attribut, das für bestimmte Programmelemente (z. B. Klassen und Methoden) platziert werden kann, um eine Zuordnung von diese Programmelemente in der entsprechenden Dokumentation bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-106">For instance, a framework might define a `HelpAttribute` attribute that can be placed on certain program elements (such as classes and methods) to provide a mapping from those program elements to their documentation.</span></span>

<span data-ttu-id="9a3ef-107">Attribute werden durch die Deklaration von Attributklassen definiert ([Attributklassen](attributes.md#attribute-classes)), die möglicherweise mit Feldern fester Breite und benannte Parameter ([Feldern fester Breite und benannte Parameter](attributes.md#positional-and-named-parameters)).</span><span class="sxs-lookup"><span data-stu-id="9a3ef-107">Attributes are defined through the declaration of attribute classes ([Attribute classes](attributes.md#attribute-classes)), which may have positional and named parameters ([Positional and named parameters](attributes.md#positional-and-named-parameters)).</span></span> <span data-ttu-id="9a3ef-108">Attribute werden angefügt, um Entitäten in einem C#-Programm mit Attributspezifikationen ([Attributspezifikation](attributes.md#attribute-specification)), und zur Laufzeit abgerufen werden kann, um als Attributinstanzen ([Attributinstanzen](attributes.md#attribute-instances)).</span><span class="sxs-lookup"><span data-stu-id="9a3ef-108">Attributes are attached to entities in a C# program using attribute specifications ([Attribute specification](attributes.md#attribute-specification)), and can be retrieved at run-time as attribute instances ([Attribute instances](attributes.md#attribute-instances)).</span></span>

## <a name="attribute-classes"></a><span data-ttu-id="9a3ef-109">Attributklassen</span><span class="sxs-lookup"><span data-stu-id="9a3ef-109">Attribute classes</span></span>

<span data-ttu-id="9a3ef-110">Eine von der abstrakten Klasse abgeleitete Klasse `System.Attribute`, entweder direkt oder indirekt ist ein ***Attributklasse***.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-110">A class that derives from the abstract class `System.Attribute`, whether directly or indirectly, is an ***attribute class***.</span></span> <span data-ttu-id="9a3ef-111">Die Deklaration einer Attributklasse definiert eine neue Art von ***Attribut*** dar, die in einer Deklaration platziert werden kann.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-111">The declaration of an attribute class defines a new kind of ***attribute*** that can be placed on a declaration.</span></span> <span data-ttu-id="9a3ef-112">Attributklassen heißen gemäß der Konvention, mit dem Suffix `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-112">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="9a3ef-113">Ein Attribut können entweder ein- oder lassen Sie das Suffix.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-113">Uses of an attribute may either include or omit this suffix.</span></span>

### <a name="attribute-usage"></a><span data-ttu-id="9a3ef-114">Attributverwendung</span><span class="sxs-lookup"><span data-stu-id="9a3ef-114">Attribute usage</span></span>

<span data-ttu-id="9a3ef-115">Das Attribut `AttributeUsage` ([das AttributeUsage-Attribut](attributes.md#the-attributeusage-attribute)) wird verwendet, um zu beschreiben, wie eine Attributklasse verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-115">The attribute `AttributeUsage` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)) is used to describe how an attribute class can be used.</span></span>

<span data-ttu-id="9a3ef-116">`AttributeUsage` verfügt über einen Parameter mit Feldern fester Breite ([Feldern fester Breite und benannte Parameter](attributes.md#positional-and-named-parameters)), ermöglicht eine Attributklasse an die Arten von Deklarationen, auf dem sie verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-116">`AttributeUsage` has a positional parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) that enables an attribute class to specify the kinds of declarations on which it can be used.</span></span> <span data-ttu-id="9a3ef-117">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="9a3ef-117">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

<span data-ttu-id="9a3ef-118">definiert eine Attributklasse, die mit dem Namen `SimpleAttribute` , platziert werden können, auf *Class_declaration*s und *Interface_declaration*nur.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-118">defines an attribute class named `SimpleAttribute` that can be placed on *class_declaration*s and *interface_declaration*s only.</span></span> <span data-ttu-id="9a3ef-119">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="9a3ef-119">The example</span></span>

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

<span data-ttu-id="9a3ef-120">Zeigt, verwendet der `Simple` Attribut.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-120">shows several uses of the `Simple` attribute.</span></span> <span data-ttu-id="9a3ef-121">Obwohl dieses Attribut, mit dem Namen definiert ist `SimpleAttribute`, wenn dieses Attribut verwendet wird, die `Attribute` Suffix kann ausgelassen werden, was den kurzen Namen `Simple`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-121">Although this attribute is defined with the name `SimpleAttribute`, when this attribute is used, the `Attribute` suffix may be omitted, resulting in the short name `Simple`.</span></span> <span data-ttu-id="9a3ef-122">Folglich ist das obige Beispiel semantisch äquivalent zu folgendem:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-122">Thus, the example above is semantically equivalent to the following:</span></span>

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

<span data-ttu-id="9a3ef-123">`AttributeUsage` verfügt über einen benannten Parameter ([Feldern fester Breite und benannte Parameter](attributes.md#positional-and-named-parameters)) namens `AllowMultiple`, der angibt, ob das Attribut für eine bestimmte Entität mehrmals angegeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-123">`AttributeUsage` has a named parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) called `AllowMultiple`, which indicates whether the attribute can be specified more than once for a given entity.</span></span> <span data-ttu-id="9a3ef-124">Wenn `AllowMultiple` Klasse für ein Attribut ist "true", und klicken Sie dann diese Attributklasse ist eine ***dienenden Attributklasse***, und kann für eine Entität mehrmals angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-124">If `AllowMultiple` for an attribute class is true, then that attribute class is a ***multi-use attribute class***, and can be specified more than once on an entity.</span></span> <span data-ttu-id="9a3ef-125">Wenn `AllowMultiple` Klasse für ein Attribut ist "false" oder ist nicht angegeben ist, lautet die Attributklasse eine ***einmaligen Verwendung Attributklasse***, und kann für eine Entität höchstens einmal angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-125">If `AllowMultiple` for an attribute class is false or it is unspecified, then that attribute class is a ***single-use attribute class***, and can be specified at most once on an entity.</span></span>

<span data-ttu-id="9a3ef-126">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="9a3ef-126">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

<span data-ttu-id="9a3ef-127">definiert eine Multi-Use-Attribut-Klasse, die mit dem Namen `AuthorAttribute`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-127">defines a multi-use attribute class named `AuthorAttribute`.</span></span> <span data-ttu-id="9a3ef-128">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="9a3ef-128">The example</span></span>

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

<span data-ttu-id="9a3ef-129">Zeigt eine Klassendeklaration mit zwei Verwendungen von der `Author` Attribut.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-129">shows a class declaration with two uses of the `Author` attribute.</span></span>

<span data-ttu-id="9a3ef-130">`AttributeUsage` verfügt über eine andere benannte Parameter namens `Inherited`, der angibt, ob das Attribut, wenn Sie in einer Basisklasse angegeben auch von Klassen geerbt wird, die von dieser Basisklasse abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-130">`AttributeUsage` has another named parameter called `Inherited`, which indicates whether the attribute, when specified on a base class, is also inherited by classes that derive from that base class.</span></span> <span data-ttu-id="9a3ef-131">Wenn `Inherited` Klasse für ein Attribut ist "true", und klicken Sie dann dieses Attribut geerbt.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-131">If `Inherited` for an attribute class is true, then that attribute is inherited.</span></span> <span data-ttu-id="9a3ef-132">Wenn `Inherited` Klasse für ein Attribut ist "false", und klicken Sie dann dieses Attribut wird nicht geerbt.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-132">If `Inherited` for an attribute class is false then that attribute is not inherited.</span></span> <span data-ttu-id="9a3ef-133">Wenn dies nicht angegeben ist, ist der Standardwert "true".</span><span class="sxs-lookup"><span data-stu-id="9a3ef-133">If it is unspecified, its default value is true.</span></span>

<span data-ttu-id="9a3ef-134">Eine Attributklasse `X` ohne eine `AttributeUsage` -Attribut zugewiesen ist, wie in</span><span class="sxs-lookup"><span data-stu-id="9a3ef-134">An attribute class `X` not having an `AttributeUsage` attribute attached to it, as in</span></span>

```csharp
using System;

class X: Attribute {...}
```

<span data-ttu-id="9a3ef-135">Ist äquivalent zu folgendem:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-135">is equivalent to the following:</span></span>

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a><span data-ttu-id="9a3ef-136">Positionelle und benannte Parameter</span><span class="sxs-lookup"><span data-stu-id="9a3ef-136">Positional and named parameters</span></span>

<span data-ttu-id="9a3ef-137">Attributklassen haben ***Positionsparameter*** und ***benannte Parameter***.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-137">Attribute classes can have ***positional parameters*** and ***named parameters***.</span></span> <span data-ttu-id="9a3ef-138">Jeder öffentlichen Instanzkonstruktor für eine Attributklasse definiert eine gültige Sequenz an positionelle Parameter für diese Attributklasse.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-138">Each public instance constructor for an attribute class defines a valid sequence of positional parameters for that attribute class.</span></span> <span data-ttu-id="9a3ef-139">Jede nicht statische öffentliche Lese-/ Schreibfeld und die Eigenschaft für eine Attributklasse definiert einen benannten Parameter für die Attributklasse.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-139">Each non-static public read-write field and property for an attribute class defines a named parameter for the attribute class.</span></span>

<span data-ttu-id="9a3ef-140">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="9a3ef-140">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

<span data-ttu-id="9a3ef-141">definiert eine Attributklasse, die mit dem Namen `HelpAttribute` einen positionelle Parameter, dessen `url`, und ein benannter Parameter, `Topic`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-141">defines an attribute class named `HelpAttribute` that has one positional parameter, `url`, and one named parameter, `Topic`.</span></span> <span data-ttu-id="9a3ef-142">Obwohl es nicht statisch und öffentlich und die Eigenschaft ist `Url` definiert keinen benannten Parameter aus, da er nicht über Lese-/ Schreibzugriff ist.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-142">Although it is non-static and public, the property `Url` does not define a named parameter, since it is not read-write.</span></span>

<span data-ttu-id="9a3ef-143">Diese Attributklasse kann wie folgt verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-143">This attribute class might be used as follows:</span></span>

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a><span data-ttu-id="9a3ef-144">Attributparametertypen</span><span class="sxs-lookup"><span data-stu-id="9a3ef-144">Attribute parameter types</span></span>

<span data-ttu-id="9a3ef-145">Die Typen der positionelle und benannte Parameter für eine Attributklasse sind auf die ***Parameter Attributtypen***, welche sind:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-145">The types of positional and named parameters for an attribute class are limited to the ***attribute parameter types***, which are:</span></span>

*  <span data-ttu-id="9a3ef-146">Einer der folgenden Typen: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-146">One of the following types: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span></span>
*  <span data-ttu-id="9a3ef-147">Typ `object`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-147">The type `object`.</span></span>
*  <span data-ttu-id="9a3ef-148">Typ `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-148">The type `System.Type`.</span></span>
*  <span data-ttu-id="9a3ef-149">Ein Enumerationstyp bereitgestellt, sie öffentlichen Zugriff hat und die Typen, die in der geschachtelt ist (sofern vorhanden) werden auch öffentliche zugreifbarkeit besitzen ([Attributspezifikation](attributes.md#attribute-specification)).</span><span class="sxs-lookup"><span data-stu-id="9a3ef-149">An enum type, provided it has public accessibility and the types in which it is nested (if any) also have public accessibility ([Attribute specification](attributes.md#attribute-specification)).</span></span>
*  <span data-ttu-id="9a3ef-150">Eindimensionale Arrays der oben genannten Typen.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-150">Single-dimensional arrays of the above types.</span></span>
*  <span data-ttu-id="9a3ef-151">Ein Konstruktorargument oder ein öffentliches Feld, der nicht einem dieser Typen, werden nicht verwendet als Positionsparameter oder benannte Parameter in einer Attributspezifikation.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-151">A constructor argument or public field which does not have one of these types, cannot be used as a positional or named parameter in an attribute specification.</span></span>

## <a name="attribute-specification"></a><span data-ttu-id="9a3ef-152">Attributspezifikation</span><span class="sxs-lookup"><span data-stu-id="9a3ef-152">Attribute specification</span></span>

<span data-ttu-id="9a3ef-153">***Attributspezifikation*** ist die Anwendung von einem zuvor definierten Attribut auf eine Deklaration.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-153">***Attribute specification*** is the application of a previously defined attribute to a declaration.</span></span> <span data-ttu-id="9a3ef-154">Ein Attribut ist ein Stück von zusätzlichen deklarativen Informationen, der für eine Deklaration angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-154">An attribute is a piece of additional declarative information that is specified for a declaration.</span></span> <span data-ttu-id="9a3ef-155">Attribute können im globalen Gültigkeitsbereich (zur Angabe von Attributen auf die enthaltende Assembly oder ein Modul) angegeben werden und für *Type_declaration*s ([Typdeklarationen](namespaces.md#type-declarations)), *Class_member_declaration* s ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)), *Interface_member_declaration*s ([Schnittstellenmember](interfaces.md#interface-members)), *Struct_member _declaration*s ([Strukturmember](structs.md#struct-members)), *Enum_member_declaration*s ([Enumerationsmember](enums.md#enum-members)), *Accessor_declarations*  ([Accessoren](classes.md#accessors)), *Event_accessor_declarations* ([Feldähnliche Ereignisse](classes.md#field-like-events)), und *Formal_parameter_list*s ([Methodenparameter](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="9a3ef-155">Attributes can be specified at global scope (to specify attributes on the containing assembly or module) and for *type_declaration*s ([Type declarations](namespaces.md#type-declarations)), *class_member_declaration*s ([Type parameter constraints](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([Interface members](interfaces.md#interface-members)), *struct_member_declaration*s ([Struct members](structs.md#struct-members)), *enum_member_declaration*s ([Enum members](enums.md#enum-members)), *accessor_declarations* ([Accessors](classes.md#accessors)), *event_accessor_declarations* ([Field-like events](classes.md#field-like-events)), and *formal_parameter_list*s ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="9a3ef-156">Attribute werden in angegeben ***Attribut Abschnitte***.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-156">Attributes are specified in ***attribute sections***.</span></span> <span data-ttu-id="9a3ef-157">Ein Attributabschnitt besteht aus einem Paar eckiger Klammern, die eine durch Trennzeichen getrennte Liste von einem oder mehreren Attributen zu umschließen.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-157">An attribute section consists of a pair of square brackets, which surround a comma-separated list of one or more attributes.</span></span> <span data-ttu-id="9a3ef-158">Die Reihenfolge, in der Attribute in einer solchen Liste angegeben werden, und die Reihenfolge, in der Abschnitte auf dieselbe Anwendung Entität angefügt, werden angeordnet, spielt keine.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-158">The order in which attributes are specified in such a list, and the order in which sections attached to the same program entity are arranged, is not significant.</span></span> <span data-ttu-id="9a3ef-159">Z. B. die Attributspezifikationen `[A][B]`, `[B][A]`, `[A,B]`, und `[B,A]` entsprechen.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-159">For instance, the attribute specifications `[A][B]`, `[B][A]`, `[A,B]`, and `[B,A]` are equivalent.</span></span>

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

<span data-ttu-id="9a3ef-160">Ein Attribut besteht aus einem *Attribute_name* und eine optionale Liste von benannten und Positionsargumenten.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-160">An attribute consists of an *attribute_name* and an optional list of positional and named arguments.</span></span> <span data-ttu-id="9a3ef-161">Die positionellen Argumente (sofern vorhanden) vorangestellt werden die benannten Argumente.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-161">The positional arguments (if any) precede the named arguments.</span></span> <span data-ttu-id="9a3ef-162">Ein Positionsargument besteht aus einer *Attribute_argument_expression*; ein benanntes Argument besteht aus einem Namen, gefolgt von einem Gleichheitszeichen, gefolgt von einem *Attribute_argument_expression*, das zusammen , werden durch dieselben Regeln wie für einfache Zuweisung eingeschränkt.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-162">A positional argument consists of an *attribute_argument_expression*; a named argument consists of a name, followed by an equal sign, followed by an *attribute_argument_expression*, which, together, are constrained by the same rules as simple assignment.</span></span> <span data-ttu-id="9a3ef-163">Die Reihenfolge der benannten Argumente ist nicht wichtig.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-163">The order of named arguments is not significant.</span></span>

<span data-ttu-id="9a3ef-164">Die *Attribute_name* identifiziert eine Attributklasse.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-164">The *attribute_name* identifies an attribute class.</span></span> <span data-ttu-id="9a3ef-165">Wenn die Form der *Attribute_name* ist *Type_name* wird dieser Name muss mit einer Attributklasse verweisen.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-165">If the form of *attribute_name* is *type_name* then this name must refer to an attribute class.</span></span> <span data-ttu-id="9a3ef-166">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-166">Otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="9a3ef-167">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="9a3ef-167">The example</span></span>

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

<span data-ttu-id="9a3ef-168">Führt einen Kompilierzeitfehler, da versucht wird, verwenden Sie `Class1` als Attribut Klasse an, wenn `Class1` ist keine Attributklasse.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-168">results in a compile-time error because it attempts to use `Class1` as an attribute class when `Class1` is not an attribute class.</span></span>

<span data-ttu-id="9a3ef-169">Bestimmte Kontexte ermöglichen die Angabe eines Attributs auf mehr als ein Ziel.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-169">Certain contexts permit the specification of an attribute on more than one target.</span></span> <span data-ttu-id="9a3ef-170">Ein Programm kann explizit Geben Sie das Ziel durch Einschließen einer *Attribute_target_specifier*.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-170">A program can explicitly specify the target by including an *attribute_target_specifier*.</span></span> <span data-ttu-id="9a3ef-171">Wenn ein Attribut auf globaler Ebene platziert wird eine *Global_attribute_target_specifier* ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-171">When an attribute is placed at the global level, a *global_attribute_target_specifier* is required.</span></span> <span data-ttu-id="9a3ef-172">In allen anderen Standorten, ein geeigneten Standardwert angewendet wird, aber ein *Attribute_target_specifier* kann verwendet werden, zu bestätigen, oder die Standardeinstellung in bestimmten Fällen mehrdeutigen überschreiben (oder nur der Standardwert in nicht-mehrdeutigen Fällen bestätigen).</span><span class="sxs-lookup"><span data-stu-id="9a3ef-172">In all other locations, a reasonable default is applied, but an *attribute_target_specifier* can be used to affirm or override the default in certain ambiguous cases (or to just affirm the default in non-ambiguous cases).</span></span> <span data-ttu-id="9a3ef-173">Daher, in der Regel *Attribute_target_specifier*s kann nur auf globaler Ebene ausgelassen werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-173">Thus, typically, *attribute_target_specifier*s can be omitted except at the global level.</span></span> <span data-ttu-id="9a3ef-174">Die potenziell mehrdeutigen Kontexte sind wie folgt aufgelöst:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-174">The potentially ambiguous contexts are resolved as follows:</span></span>

*  <span data-ttu-id="9a3ef-175">Ein Attribut im globalen Bereich angegeben kann es sich um die Zielassembly oder das Zielmodul angewendet.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-175">An attribute specified at global scope can apply either to the target assembly or the target module.</span></span> <span data-ttu-id="9a3ef-176">Kein Standardwert vorhanden ist für diesen Kontext, also eine *Attribute_target_specifier* ist in diesem Kontext immer erforderlich.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-176">No default exists for this context, so an *attribute_target_specifier* is always required in this context.</span></span> <span data-ttu-id="9a3ef-177">Das Vorhandensein der `assembly` *Attribute_target_specifier* gibt an, dass das Attribut für das Ziel gilt Assembly; das Vorhandensein der `module` *Attribute_target_specifier* Gibt an, dass das Attribut auf das Zielmodul angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-177">The presence of the `assembly` *attribute_target_specifier* indicates that the attribute applies to the target assembly; the presence of the `module` *attribute_target_specifier* indicates that the attribute applies to the target module.</span></span>
*  <span data-ttu-id="9a3ef-178">Ein Attribut in einer Delegatdeklaration angegeben kann entweder an den Delegaten deklariert wird oder ihren Rückgabewert anwenden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-178">An attribute specified on a delegate declaration can apply either to the delegate being declared or to its return value.</span></span> <span data-ttu-id="9a3ef-179">In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für den Delegaten.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-179">In the absence of an *attribute_target_specifier*, the attribute applies to the delegate.</span></span> <span data-ttu-id="9a3ef-180">Das Vorhandensein der `type` *Attribute_target_specifier* gibt an, dass das Attribut für den Delegaten, gilt das Vorhandensein der `return` *Attribute_target_specifier* Gibt an, dass das Attribut auf den Rückgabewert angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-180">The presence of the `type` *attribute_target_specifier* indicates that the attribute applies to the delegate; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="9a3ef-181">Ein Attribut in einer Methodendeklaration angegeben kann entweder an die Methode deklariert wird oder ihren Rückgabewert anwenden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-181">An attribute specified on a method declaration can apply either to the method being declared or to its return value.</span></span> <span data-ttu-id="9a3ef-182">In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für die Methode.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-182">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="9a3ef-183">Das Vorhandensein der `method` *Attribute_target_specifier* gibt an, dass das Attribut für die Methode gilt das Vorhandensein der `return` *Attribute_target_specifier* angibt Das Attribut auf den Rückgabewert angewendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-183">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="9a3ef-184">Ein Attribut auf einen Operatordeklaration angegebenen kann entweder an den Operator deklariert wird oder ihren Rückgabewert anwenden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-184">An attribute specified on an operator declaration can apply either to the operator being declared or to its return value.</span></span> <span data-ttu-id="9a3ef-185">In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für den Operator.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-185">In the absence of an *attribute_target_specifier*, the attribute applies to the operator.</span></span> <span data-ttu-id="9a3ef-186">Das Vorhandensein der `method` *Attribute_target_specifier* gibt an, dass das Attribut für den Operator, gilt das Vorhandensein der `return` *Attribute_target_specifier* Gibt an, dass das Attribut auf den Rückgabewert angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-186">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the operator; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="9a3ef-187">Ein Attribut in einer Ereignisdeklaration, die von Ereignisaccessoren ausgelassen angegeben kann angewendet werden, auf das Ereignis deklariert wird, auf das zugeordnete Feld (wenn das Ereignis nicht abstrakt ist) oder den zugehörigen hinzufügen und entfernen-Methoden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-187">An attribute specified on an event declaration that omits event accessors can apply to the event being declared, to the associated field (if the event is not abstract), or to the associated add and remove methods.</span></span> <span data-ttu-id="9a3ef-188">In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für das Ereignis.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-188">In the absence of an *attribute_target_specifier*, the attribute applies to the event.</span></span> <span data-ttu-id="9a3ef-189">Das Vorhandensein der `event` *Attribute_target_specifier* gibt an, dass das Attribut für das Ereignis gilt das Vorhandensein der `field` *Attribute_target_specifier* angibt Das Attribut auf das Feld angewendet werden; und das Vorhandensein der `method` *Attribute_target_specifier* gibt an, dass das Attribut für die Methoden gilt.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-189">The presence of the `event` *attribute_target_specifier* indicates that the attribute applies to the event; the presence of the `field` *attribute_target_specifier* indicates that the attribute applies to the field; and the presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the methods.</span></span>
*  <span data-ttu-id="9a3ef-190">Ein Attribut auf eine Get-Zugriffsmethoden-Deklaration für eine Eigenschaft oder der Indexer-Deklaration angegebenen kann entweder auf die zugehörige Methode oder ihren Rückgabewert anwenden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-190">An attribute specified on a get accessor declaration for a property or indexer declaration can apply either to the associated method or to its return value.</span></span> <span data-ttu-id="9a3ef-191">In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für die Methode.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-191">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="9a3ef-192">Das Vorhandensein der `method` *Attribute_target_specifier* gibt an, dass das Attribut für die Methode gilt das Vorhandensein der `return` *Attribute_target_specifier* angibt Das Attribut auf den Rückgabewert angewendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-192">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="9a3ef-193">Ein Attribut für einen Set-Accessor für eine Eigenschaft oder der Indexer-Deklaration angegeben kann entweder auf die zugehörige Methode oder als einzelne implizite Parameter angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-193">An attribute specified on a set accessor for a property or indexer declaration can apply either to the associated method or to its lone implicit parameter.</span></span> <span data-ttu-id="9a3ef-194">In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für die Methode.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-194">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="9a3ef-195">Das Vorhandensein der `method` *Attribute_target_specifier* gibt an, dass das Attribut für die Methode gilt das Vorhandensein der `param` *Attribute_target_specifier* angibt Das Attribut für den Parameter angewendet werden; das Vorhandensein der `return` *Attribute_target_specifier* gibt an, dass das Attribut auf den Rückgabewert angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-195">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="9a3ef-196">Ein Attribut auf ein Add- oder Remove-Zugriffsmethoden-Deklaration angegeben werden, für eine Ereignisdeklaration entweder auf die zugehörige Methode oder als einzelne Parameter angewendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-196">An attribute specified on an add or remove accessor declaration for an event declaration can apply either to the associated method or to its lone parameter.</span></span> <span data-ttu-id="9a3ef-197">In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für die Methode.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-197">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="9a3ef-198">Das Vorhandensein der `method` *Attribute_target_specifier* gibt an, dass das Attribut für die Methode gilt das Vorhandensein der `param` *Attribute_target_specifier* angibt Das Attribut für den Parameter angewendet werden; das Vorhandensein der `return` *Attribute_target_specifier* gibt an, dass das Attribut auf den Rückgabewert angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-198">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>

<span data-ttu-id="9a3ef-199">In anderen Kontexten Einbeziehung einer *Attribute_target_specifier* ist zulässig, aber nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-199">In other contexts, inclusion of an *attribute_target_specifier* is permitted but unnecessary.</span></span> <span data-ttu-id="9a3ef-200">Z. B. eine Klassendeklaration kann entweder eingeschlossen oder weglassen den Spezifizierer `type`:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-200">For instance, a class declaration may either include or omit the specifier `type`:</span></span>

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

<span data-ttu-id="9a3ef-201">Es ist ein Fehler an eine ungültige *Attribute_target_specifier*.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-201">It is an error to specify an invalid *attribute_target_specifier*.</span></span> <span data-ttu-id="9a3ef-202">Z. B. der Spezifizierer `param` kann nicht in einer Klassendeklaration verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-202">For instance, the specifier `param` cannot be used on a class declaration:</span></span>

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

<span data-ttu-id="9a3ef-203">Attributklassen heißen gemäß der Konvention, mit dem Suffix `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-203">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="9a3ef-204">Ein *Attribute_name* des Formulars *Type_name* kann entweder ein- oder lassen Sie das Suffix.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-204">An *attribute_name* of the form *type_name* may either include or omit this suffix.</span></span> <span data-ttu-id="9a3ef-205">Wenn eine Attributklasse mit und ohne das Suffix gefunden wird, eine Mehrdeutigkeit vorliegt, und führt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-205">If an attribute class is found both with and without this suffix, an ambiguity is present, and a compile-time error results.</span></span> <span data-ttu-id="9a3ef-206">Wenn der *Attribute_name* geschrieben ist, dass der äußersten *Bezeichner* wird als ausführlicher Bezeichner ([Bezeichner](lexical-structure.md#identifiers)), und klicken Sie dann nur ein Attribut ohne Suffix übereinstimmt, sodass solche Mehrdeutigkeiten aufgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-206">If the *attribute_name* is spelled such that its right-most *identifier* is a verbatim identifier ([Identifiers](lexical-structure.md#identifiers)), then only an attribute without a suffix is matched, thus enabling such an ambiguity to be resolved.</span></span> <span data-ttu-id="9a3ef-207">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="9a3ef-207">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

<span data-ttu-id="9a3ef-208">zeigt zwei Attributklassen, die mit dem Namen `X` und `XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-208">shows two attribute classes named `X` and `XAttribute`.</span></span> <span data-ttu-id="9a3ef-209">Das Attribut `[X]` ist mehrdeutig, da er entweder verweisen könnte `X` oder `XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-209">The attribute `[X]` is ambiguous, since it could refer to either `X` or `XAttribute`.</span></span> <span data-ttu-id="9a3ef-210">Als ausführlichen Bezeichner können die genaue Absicht in diesen seltenen Fällen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-210">Using a verbatim identifier allows the exact intent to be specified in such rare cases.</span></span> <span data-ttu-id="9a3ef-211">Das Attribut `[XAttribute]` ist nicht mehrdeutig (obwohl es der Fall wäre, wenn es eine Attributklasse, die mit dem Namen wurde `XAttributeAttribute`!).</span><span class="sxs-lookup"><span data-stu-id="9a3ef-211">The attribute `[XAttribute]` is not ambiguous (although it would be if there was an attribute class named `XAttributeAttribute`!).</span></span> <span data-ttu-id="9a3ef-212">Wenn die Deklaration für Klasse `X` wird entfernt, und klicken Sie dann beide Attribute auf die Attributklasse, die mit dem Namen verweisen `XAttribute`wie folgt:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-212">If the declaration for class `X` is removed, then both attributes refer to the attribute class named `XAttribute`, as follows:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

<span data-ttu-id="9a3ef-213">Es ist ein Fehler während der Kompilierung, um eine Single-Use-Attribut mehr als einmal auf die gleiche Entität verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-213">It is a compile-time error to use a single-use attribute class more than once on the same entity.</span></span> <span data-ttu-id="9a3ef-214">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="9a3ef-214">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

<span data-ttu-id="9a3ef-215">Führt einen Kompilierzeitfehler, da versucht wird, verwenden Sie `HelpString`, d.h. eine Single-Use-Attribut-Klasse, mehr als einmal in der Deklaration der `Class1`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-215">results in a compile-time error because it attempts to use `HelpString`, which is a single-use attribute class, more than once on the declaration of `Class1`.</span></span>

<span data-ttu-id="9a3ef-216">Ein Ausdruck `E` ist ein *Attribute_argument_expression* Wenn alle der folgenden Aussagen zutreffen:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-216">An expression `E` is an *attribute_argument_expression* if all of the following statements are true:</span></span>

*  <span data-ttu-id="9a3ef-217">Der Typ des `E` ein Attributparametertyps ist ([Parameter Attributtypen](attributes.md#attribute-parameter-types)).</span><span class="sxs-lookup"><span data-stu-id="9a3ef-217">The type of `E` is an attribute parameter type ([Attribute parameter types](attributes.md#attribute-parameter-types)).</span></span>
*  <span data-ttu-id="9a3ef-218">Zum Zeitpunkt der Kompilierung, den Wert der `E` aufgelöst werden kann, um einen der folgenden:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-218">At compile-time, the value of `E` can be resolved to one of the following:</span></span>
   * <span data-ttu-id="9a3ef-219">Ein konstanter Wert.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-219">A constant value.</span></span>
   * <span data-ttu-id="9a3ef-220">Ein `System.Type`-Objekt.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-220">A `System.Type` object.</span></span>
   * <span data-ttu-id="9a3ef-221">Ein eindimensionales Array von *Attribute_argument_expression*s.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-221">A one-dimensional array of *attribute_argument_expression*s.</span></span>

<span data-ttu-id="9a3ef-222">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-222">For example:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

<span data-ttu-id="9a3ef-223">Ein *Typeof_expression* ([der Typeof-Operator](expressions.md#the-typeof-operator)) verwendet, wie ein Argumentausdruck Attribut kann ein nicht generischer Typ, ein geschlossener konstruierter Typ oder einen ungebundenen generischen Typ verweisen, aber sie kann keine verweisen eine Öffnen Sie die geben.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-223">A *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)) used as an attribute argument expression can reference a non-generic type, a closed constructed type, or an unbound generic type, but it cannot reference an open type.</span></span> <span data-ttu-id="9a3ef-224">Dadurch wird sichergestellt, dass der Ausdruck kann, während der Kompilierung aufgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-224">This is to ensure that the expression can be resolved at compile-time.</span></span>

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a><span data-ttu-id="9a3ef-225">Attributinstanzen</span><span class="sxs-lookup"><span data-stu-id="9a3ef-225">Attribute instances</span></span>

<span data-ttu-id="9a3ef-226">Ein ***Attributinstanz*** ist eine Instanz, die ein Attribut zur Laufzeit darstellt.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-226">An ***attribute instance*** is an instance that represents an attribute at run-time.</span></span> <span data-ttu-id="9a3ef-227">Ein Attribut mit positionellen Argumenten eine Attributklasse definiert und benannte Argumente.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-227">An attribute is defined with an attribute class, positional arguments, and named arguments.</span></span> <span data-ttu-id="9a3ef-228">Eine Attributinstanz ist eine Instanz der Attributklasse, die mit benannten und Positionsargumenten Argumenten initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-228">An attribute instance is an instance of the attribute class that is initialized with the positional and named arguments.</span></span>

<span data-ttu-id="9a3ef-229">Abrufen einer Instanz des umfasst sowohl während der Kompilierung und Laufzeit-Verarbeitung, wie in den folgenden Abschnitten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-229">Retrieval of an attribute instance involves both compile-time and run-time processing, as described in the following sections.</span></span>

### <a name="compilation-of-an-attribute"></a><span data-ttu-id="9a3ef-230">Ein Attribut für die Quellcodekompilierung</span><span class="sxs-lookup"><span data-stu-id="9a3ef-230">Compilation of an attribute</span></span>

<span data-ttu-id="9a3ef-231">Die Kompilierung von einer *Attribut* mit Attributklasse `T`, *Positional_argument_list* `P` und *Named_argument_list* `N`, umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-231">The compilation of an *attribute* with attribute class `T`, *positional_argument_list* `P` and *named_argument_list* `N`, consists of the following steps:</span></span>

*  <span data-ttu-id="9a3ef-232">Führen Sie die während der Kompilierung Verarbeitungsschritte für die Kompilierung einer *Object_creation_expression* des Formulars `new T(P)`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-232">Follow the compile-time processing steps for compiling an *object_creation_expression* of the form `new T(P)`.</span></span> <span data-ttu-id="9a3ef-233">Diese Schritte führen zu einem Fehler während der Kompilierung oder bestimmen Instanzkonstruktor `C` auf `T` , die zur Laufzeit aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-233">These steps either result in a compile-time error, or determine an instance constructor `C` on `T` that can be invoked at run-time.</span></span>
*  <span data-ttu-id="9a3ef-234">Wenn `C` keinen öffentlichen Zugriff und dann ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-234">If `C` does not have public accessibility, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="9a3ef-235">Für jede *Named_argument* `Arg` in `N`:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-235">For each *named_argument* `Arg` in `N`:</span></span>
   * <span data-ttu-id="9a3ef-236">Lassen Sie `Name` werden die *Bezeichner* von der *Named_argument* `Arg`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-236">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span>
   * <span data-ttu-id="9a3ef-237">`Name` muss das Identifizieren von nicht statischen Lese-/ Schreibzugriff öffentliche Felder oder Eigenschaften auf `T`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-237">`Name` must identify a non-static read-write public field or property on `T`.</span></span> <span data-ttu-id="9a3ef-238">Wenn `T` keine solche Feld oder einer Eigenschaft hat, und klicken Sie dann ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-238">If `T` has no such field or property, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="9a3ef-239">Behalten Sie die folgende Informationen für die Laufzeitinstanziierung des Attributs: die Attributklasse `T`, den Instanzkonstruktor `C` auf `T`, *Positional_argument_list* `P` und die *Named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-239">Keep the following information for run-time instantiation of the attribute: the attribute class `T`, the instance constructor `C` on `T`, the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

### <a name="run-time-retrieval-of-an-attribute-instance"></a><span data-ttu-id="9a3ef-240">Abrufen der Laufzeit eine Instanz des</span><span class="sxs-lookup"><span data-stu-id="9a3ef-240">Run-time retrieval of an attribute instance</span></span>

<span data-ttu-id="9a3ef-241">Kompilierung von einer *Attribut* führt zu eine Attributklasse `T`, Instanzkonstruktor `C` auf `T`, *Positional_argument_list* `P`, und ein *Named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-241">Compilation of an *attribute* yields an attribute class `T`, an instance constructor `C` on `T`, a *positional_argument_list* `P`, and a *named_argument_list* `N`.</span></span> <span data-ttu-id="9a3ef-242">Mit diesen Informationen kann zur Laufzeit mit den folgenden Schritten eine Attributinstanz abgerufen werden:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-242">Given this information, an attribute instance can be retrieved at run-time using the following steps:</span></span>

*  <span data-ttu-id="9a3ef-243">Führen Sie die laufzeitverarbeitung Schritte für die Ausführung einer *Object_creation_expression* des Formulars `new T(P)`, mit dem Instanzkonstruktor `C` , die zum Zeitpunkt der Kompilierung festgelegt.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-243">Follow the run-time processing steps for executing an *object_creation_expression* of the form `new T(P)`, using the instance constructor `C` as determined at compile-time.</span></span> <span data-ttu-id="9a3ef-244">Diese Schritte wird eine Ausnahme ausgelöst oder erzeugen eine Instanz `O` von `T`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-244">These steps either result in an exception, or produce an instance `O` of `T`.</span></span>
*  <span data-ttu-id="9a3ef-245">Für jede *Named_argument* `Arg` in `N`, in der Reihenfolge:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-245">For each *named_argument* `Arg` in `N`, in order:</span></span>
   * <span data-ttu-id="9a3ef-246">Lassen Sie `Name` werden die *Bezeichner* von der *Named_argument* `Arg`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-246">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span> <span data-ttu-id="9a3ef-247">Wenn `Name` identifiziert keine nicht statischen öffentlichen Lese-/ Schreibzugriff Felder oder Eigenschaften auf `O`, und klicken Sie dann eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-247">If `Name` does not identify a non-static public read-write field or property on `O`, then an exception is thrown.</span></span>
   * <span data-ttu-id="9a3ef-248">Lassen Sie `Value` werden das Ergebnis der Auswertung der *Attribute_argument_expression* von `Arg`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-248">Let `Value` be the result of evaluating the *attribute_argument_expression* of `Arg`.</span></span>
   * <span data-ttu-id="9a3ef-249">Wenn `Name` gibt ein Feld auf `O`, legen Sie dieses Feld auf `Value`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-249">If `Name` identifies a field on `O`, then set this field to `Value`.</span></span>
   * <span data-ttu-id="9a3ef-250">Andernfalls `Name` gibt eine Eigenschaft auf `O`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-250">Otherwise, `Name` identifies a property on `O`.</span></span> <span data-ttu-id="9a3ef-251">Legen Sie diese Eigenschaft auf `Value`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-251">Set this property to `Value`.</span></span>
   * <span data-ttu-id="9a3ef-252">Das Ergebnis ist `O`, eine Instanz der Attributklasse `T` , wurde mit initialisiert die *Positional_argument_list* `P` und *Named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-252">The result is `O`, an instance of the attribute class `T` that has been initialized with the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

## <a name="reserved-attributes"></a><span data-ttu-id="9a3ef-253">Reservierte Attribute</span><span class="sxs-lookup"><span data-stu-id="9a3ef-253">Reserved attributes</span></span>

<span data-ttu-id="9a3ef-254">Eine kleine Anzahl von Attributen Auswirkungen auf die Sprache in irgendeiner Form.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-254">A small number of attributes affect the language in some way.</span></span> <span data-ttu-id="9a3ef-255">Diese Attribute enthalten:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-255">These attributes include:</span></span>

*  <span data-ttu-id="9a3ef-256">`System.AttributeUsageAttribute` ([Das AttributeUsage-Attribut](attributes.md#the-attributeusage-attribute)), womit die Möglichkeiten beschrieben, in denen eine Attributklasse verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-256">`System.AttributeUsageAttribute` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)), which is used to describe the ways in which an attribute class can be used.</span></span>
*  <span data-ttu-id="9a3ef-257">`System.Diagnostics.ConditionalAttribute` ([Das Conditional-Attribut](attributes.md#the-conditional-attribute)), womit bedingte Methoden definieren.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-257">`System.Diagnostics.ConditionalAttribute` ([The Conditional attribute](attributes.md#the-conditional-attribute)), which is used to define conditional methods.</span></span>
*  <span data-ttu-id="9a3ef-258">`System.ObsoleteAttribute` ([Das Obsolete-Attribut](attributes.md#the-obsolete-attribute)), die wird verwendet, um ein Element als veraltet zu markieren.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-258">`System.ObsoleteAttribute` ([The Obsolete attribute](attributes.md#the-obsolete-attribute)), which is used to mark a member as obsolete.</span></span>
*  <span data-ttu-id="9a3ef-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` und `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([Aufrufer-Informationsattribute](attributes.md#caller-info-attributes)), die zum Bereitstellen von Informationen über den aufrufenden Kontext auf optionale Parameter verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` and `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([Caller info attributes](attributes.md#caller-info-attributes)), which are used to supply information about the calling context to optional parameters.</span></span>

### <a name="the-attributeusage-attribute"></a><span data-ttu-id="9a3ef-260">Das AttributeUsage-Attribut</span><span class="sxs-lookup"><span data-stu-id="9a3ef-260">The AttributeUsage attribute</span></span>

<span data-ttu-id="9a3ef-261">Das Attribut `AttributeUsage` wird verwendet, um die Art und Weise zu beschreiben, in denen die Attributklasse verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-261">The attribute `AttributeUsage` is used to describe the manner in which the attribute class can be used.</span></span>

<span data-ttu-id="9a3ef-262">Eine Klasse, die mit versehen ist die `AttributeUsage` Attribut ableiten muss `System.Attribute`, entweder direkt oder indirekt.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-262">A class that is decorated with the `AttributeUsage` attribute must derive from `System.Attribute`, either directly or indirectly.</span></span> <span data-ttu-id="9a3ef-263">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-263">Otherwise, a compile-time error occurs.</span></span>

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a><span data-ttu-id="9a3ef-264">Das Conditional-Attribut</span><span class="sxs-lookup"><span data-stu-id="9a3ef-264">The Conditional attribute</span></span>

<span data-ttu-id="9a3ef-265">Das Attribut `Conditional` ermöglicht die Definition von ***bedingte Methoden*** und ***conditional-Attribut von Klassen***.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-265">The attribute `Conditional` enables the definition of ***conditional methods*** and ***conditional attribute classes***.</span></span>

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a><span data-ttu-id="9a3ef-266">Bedingte Methoden</span><span class="sxs-lookup"><span data-stu-id="9a3ef-266">Conditional methods</span></span>

<span data-ttu-id="9a3ef-267">Eine Methode mit ergänzt die `Conditional` -Attribut ist eine bedingte Methode.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-267">A method decorated with the `Conditional` attribute is a conditional method.</span></span> <span data-ttu-id="9a3ef-268">Die `Conditional` -Attribut gibt eine Bedingung an, durch ein Symbol für bedingte Kompilierung zu testen.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-268">The `Conditional` attribute indicates a condition by testing a conditional compilation symbol.</span></span> <span data-ttu-id="9a3ef-269">Aufrufe an eine bedingte Methode entweder eingeschlossen oder ausgelassen wird, je nachdem, ob dieses Symbol, zum Zeitpunkt des Aufrufs definiert ist.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-269">Calls to a conditional method are either included or omitted depending on whether this symbol is defined at the point of the call.</span></span> <span data-ttu-id="9a3ef-270">Wenn das Symbol definiert ist, wird der Aufruf einbezogen; Andernfalls wird der Aufruf (einschließlich der Auswertung des Empfängers und Parameter des Aufrufs) weggelassen.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-270">If the symbol is defined, the call is included; otherwise, the call (including evaluation of the receiver and parameters of the call) is omitted.</span></span>

<span data-ttu-id="9a3ef-271">Eine bedingte Methode ist jedoch mit folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-271">A conditional method is subject to the following restrictions:</span></span>

*  <span data-ttu-id="9a3ef-272">Die bedingte Methode muss eine Methode in einer *Class_declaration* oder *Struct_declaration*.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-272">The conditional method must be a method in a *class_declaration* or *struct_declaration*.</span></span> <span data-ttu-id="9a3ef-273">Ein Fehler während der Kompilierung tritt auf, wenn die `Conditional` -Attribut für eine Methode in einer Schnittstellendeklaration angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-273">A compile-time error occurs if the `Conditional` attribute is specified on a method in an interface declaration.</span></span>
*  <span data-ttu-id="9a3ef-274">Die bedingte Methode muss einen Rückgabetyp verfügen `void`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-274">The conditional method must have a return type of `void`.</span></span>
*  <span data-ttu-id="9a3ef-275">Die bedingte Methode muss nicht mit markiert werden die `override` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-275">The conditional method must not be marked with the `override` modifier.</span></span> <span data-ttu-id="9a3ef-276">Eine bedingte Methode markiert werden kann, mit der `virtual` Modifizierer jedoch.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-276">A conditional method may be marked with the `virtual` modifier, however.</span></span> <span data-ttu-id="9a3ef-277">Außerkraftsetzungen einer solchen Methode sind implizit bedingt und müssen nicht explizit markiert werden mit einem `Conditional` Attribut.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-277">Overrides of such a method are implicitly conditional, and must not be explicitly marked with a `Conditional` attribute.</span></span>
*  <span data-ttu-id="9a3ef-278">Die bedingte Methode muss eine Implementierung einer Schnittstellenmethode nicht.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-278">The conditional method must not be an implementation of an interface method.</span></span> <span data-ttu-id="9a3ef-279">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-279">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="9a3ef-280">Darüber hinaus tritt ein Kompilierungsfehler auf, wenn eine bedingte Methode, in verwendet wird einem *Delegate_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-280">In addition, a compile-time error occurs if a conditional method is used in a *delegate_creation_expression*.</span></span> <span data-ttu-id="9a3ef-281">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="9a3ef-281">The example</span></span>

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

<span data-ttu-id="9a3ef-282">deklariert `Class1.M` als eine bedingte Methode.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-282">declares `Class1.M` as a conditional method.</span></span> <span data-ttu-id="9a3ef-283">`Class2`die `Test` -Methode ruft diese Methode.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-283">`Class2`'s `Test` method calls this method.</span></span> <span data-ttu-id="9a3ef-284">Da das konditionale kompiliersymbol `DEBUG` definiert ist, wenn `Class2.Test` wird aufgerufen, ruft sie `M`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-284">Since the conditional compilation symbol `DEBUG` is defined, if `Class2.Test` is called, it will call `M`.</span></span> <span data-ttu-id="9a3ef-285">Wenn das Symbol `DEBUG` hatte nicht definiert wurde, klicken Sie dann `Class2.Test` nicht aufruft `Class1.M`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-285">If the symbol `DEBUG` had not been defined, then `Class2.Test` would not call `Class1.M`.</span></span>

<span data-ttu-id="9a3ef-286">Es ist wichtig zu beachten, dass der Einschluss oder Ausschluss eines Aufrufs einer bedingten Methode durch die Symbole für bedingte Kompilierung zum Zeitpunkt des Aufrufs gesteuert wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-286">It is important to note that the inclusion or exclusion of a call to a conditional method is controlled by the conditional compilation symbols at the point of the call.</span></span> <span data-ttu-id="9a3ef-287">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="9a3ef-287">In the example</span></span>

<span data-ttu-id="9a3ef-288">Datei `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-288">File `class1.cs`:</span></span>

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

<span data-ttu-id="9a3ef-289">Datei `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-289">File `class2.cs`:</span></span>

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

<span data-ttu-id="9a3ef-290">Datei `class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-290">File `class3.cs`:</span></span>

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

<span data-ttu-id="9a3ef-291">die Klassen `Class2` und `Class3` jeder enthält Aufrufe der bedingten Methode `Class1.F`, die bedingte basiert auf angibt, ob `DEBUG` definiert ist.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-291">the classes `Class2` and `Class3` each contain calls to the conditional method `Class1.F`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="9a3ef-292">Da dieses Symbol, im Kontext des definiert ist `Class2` , nicht jedoch `Class3`, den Aufruf von `F` in `Class2` enthalten ist, während der Aufruf von `F` in `Class3` weggelassen wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-292">Since this symbol is defined in the context of `Class2` but not `Class3`, the call to `F` in `Class2` is included, while the call to `F` in `Class3` is omitted.</span></span>

<span data-ttu-id="9a3ef-293">Die Verwendung von bedingten Methoden in einer Vererbungskette kann verwirrend sein.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-293">The use of conditional methods in an inheritance chain can be confusing.</span></span> <span data-ttu-id="9a3ef-294">Aufrufe an eine bedingte Methode über `base`, des Formulars `base.M`, gelten die Regeln der normale bedingte Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-294">Calls made to a conditional method through `base`, of the form `base.M`, are subject to the normal conditional method call rules.</span></span> <span data-ttu-id="9a3ef-295">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="9a3ef-295">In the example</span></span>

<span data-ttu-id="9a3ef-296">Datei `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-296">File `class1.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

<span data-ttu-id="9a3ef-297">Datei `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-297">File `class2.cs`:</span></span>

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

<span data-ttu-id="9a3ef-298">Datei `class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-298">File `class3.cs`:</span></span>

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

<span data-ttu-id="9a3ef-299">`Class2` enthält einen Aufruf an die `M` in seiner Basisklasse definiert.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-299">`Class2` includes a call to the `M` defined in its base class.</span></span> <span data-ttu-id="9a3ef-300">Dieser Aufruf wird ausgelassen, da die Basismethode bedingte basierend auf dem Vorhandensein des Symbols wird `DEBUG`, dies ist nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-300">This call is omitted because the base method is conditional based on the presence of the symbol `DEBUG`, which is undefined.</span></span> <span data-ttu-id="9a3ef-301">Daher wird die Methode an die Konsole schreibt "`Class2.M executed`" nur.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-301">Thus, the method writes to the console "`Class2.M executed`" only.</span></span> <span data-ttu-id="9a3ef-302">Umsichtiger Verwendung von *Pp_declaration*s kann derartige Probleme beseitigen.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-302">Judicious use of *pp_declaration*s can eliminate such problems.</span></span>

#### <a name="conditional-attribute-classes"></a><span data-ttu-id="9a3ef-303">Conditional-Attribut von Klassen</span><span class="sxs-lookup"><span data-stu-id="9a3ef-303">Conditional attribute classes</span></span>

<span data-ttu-id="9a3ef-304">Eine Attributklasse ([Attributklassen](attributes.md#attribute-classes)) mit einem oder mehreren ergänzt `Conditional` Attribute ist ein ***conditional-Attribut-Klasse***.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-304">An attribute class ([Attribute classes](attributes.md#attribute-classes)) decorated with one or more `Conditional` attributes is a ***conditional attribute class***.</span></span> <span data-ttu-id="9a3ef-305">Eine conditional-Attribut-Klasse ist mit der Symbole für bedingte Kompilierung in deklarierten daher zugeordneten seine `Conditional` Attribute.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-305">A conditional attribute class is thus associated with the conditional compilation symbols declared in its `Conditional` attributes.</span></span> <span data-ttu-id="9a3ef-306">In diesem Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-306">This example:</span></span>

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

<span data-ttu-id="9a3ef-307">deklariert `TestAttribute` als ein conditional-Attribut-Klasse, die die Symbole für bedingte Kompilierungen zugeordneten `ALPHA` und `BETA`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-307">declares `TestAttribute` as a conditional attribute class associated with the conditional compilations symbols `ALPHA` and `BETA`.</span></span>

<span data-ttu-id="9a3ef-308">Attribut-Spezifikationen ([Attributspezifikation](attributes.md#attribute-specification)) des ein conditional-Attribut sind enthalten, wenn mindestens eines der zugeordneten konditionale Kompilierungssymbole andernfalls zum Zeitpunkt der-Spezifikation definiert ist das Attribut Spezifikation wird weggelassen.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-308">Attribute specifications ([Attribute specification](attributes.md#attribute-specification)) of a conditional attribute are included if one or more of its associated conditional compilation symbols is defined at the point of specification, otherwise the attribute specification is omitted.</span></span>

<span data-ttu-id="9a3ef-309">Es ist wichtig zu beachten, dass der Einschluss oder Ausschluss eine Attributspezifikation einer Klasse conditional-Attribut durch die Symbole für bedingte Kompilierung zum Zeitpunkt der Spezifikation gesteuert wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-309">It is important to note that the inclusion or exclusion of an attribute specification of a conditional attribute class is controlled by the conditional compilation symbols at the point of the specification.</span></span> <span data-ttu-id="9a3ef-310">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="9a3ef-310">In the example</span></span>

<span data-ttu-id="9a3ef-311">Datei `test.cs`:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-311">File `test.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

<span data-ttu-id="9a3ef-312">Datei `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-312">File `class1.cs`:</span></span>

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

<span data-ttu-id="9a3ef-313">Datei `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-313">File `class2.cs`:</span></span>

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

<span data-ttu-id="9a3ef-314">die Klassen `Class1` und `Class2` sind jeweils mit-Attribut versehen `Test`, die bedingte basiert auf angibt, ob `DEBUG` definiert ist.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-314">the classes `Class1` and `Class2` are each decorated with attribute `Test`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="9a3ef-315">Da im Rahmen dieses Symbol definiert ist `Class1` aber nicht `Class2`, die Spezifikation des der `Test` -Attribut `Class1` enthalten ist, während die Spezifikation des der `Test` -Attribut `Class2` weggelassen wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-315">Since this symbol is defined in the context of `Class1` but not `Class2`, the specification of the `Test` attribute on `Class1` is included, while the specification of the `Test` attribute on `Class2` is omitted.</span></span>

### <a name="the-obsolete-attribute"></a><span data-ttu-id="9a3ef-316">Das Obsolete-Attribut</span><span class="sxs-lookup"><span data-stu-id="9a3ef-316">The Obsolete attribute</span></span>

<span data-ttu-id="9a3ef-317">Das Attribut `Obsolete` wird verwendet, um die Typen und Member von Typen, die nicht mehr verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-317">The attribute `Obsolete` is used to mark types and members of types that should no longer be used.</span></span>

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

<span data-ttu-id="9a3ef-318">Wenn ein Programm verwendet werden soll, einen Typ oder Member, die mit ergänzt wird die `Obsolete` -Attribut, gibt der Compiler eine Warnung oder einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-318">If a program uses a type or member that is decorated with the `Obsolete` attribute, the compiler issues a warning or an error.</span></span> <span data-ttu-id="9a3ef-319">Insbesondere gibt der Compiler eine Warnung, wenn keine Fehlerparameter angegeben wird, oder wenn der Fehlerparameter angegeben wird, und hat den Wert `false`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-319">Specifically, the compiler issues a warning if no error parameter is provided, or if the error parameter is provided and has the value `false`.</span></span> <span data-ttu-id="9a3ef-320">Der Compiler gibt einen Fehler aus, wenn der Fehlerparameter angegeben ist, und den Wert hat `true`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-320">The compiler issues an error if the error parameter is specified and has the value `true`.</span></span>

<span data-ttu-id="9a3ef-321">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="9a3ef-321">In the example</span></span>

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

<span data-ttu-id="9a3ef-322">die Klasse `A` ergänzt wird, mit der `Obsolete` Attribut.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-322">the class `A` is decorated with the `Obsolete` attribute.</span></span> <span data-ttu-id="9a3ef-323">Jede Verwendung von `A` in `Main` löst eine Warnung, die die angegebene Meldung enthält, "Diese Klasse ist veraltet; Stattdessen Sie Klasse B."</span><span class="sxs-lookup"><span data-stu-id="9a3ef-323">Each use of `A` in `Main` results in a warning that includes the specified message, "This class is obsolete; use class B instead."</span></span>

### <a name="caller-info-attributes"></a><span data-ttu-id="9a3ef-324">Aufruferinformationsattribute</span><span class="sxs-lookup"><span data-stu-id="9a3ef-324">Caller info attributes</span></span>

<span data-ttu-id="9a3ef-325">Für Zwecke, z. B. Protokollierung und berichterstellung ist es manchmal sinnvoll für ein Funktionselement zum Abrufen bestimmter Informationen während der Kompilierung zum aufrufenden Code.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-325">For purposes such as logging and reporting, it is sometimes useful for a function member to obtain certain compile-time information about the calling code.</span></span> <span data-ttu-id="9a3ef-326">Die Attribute "callerinfo" bieten eine Möglichkeit, solche Informationen transparent zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-326">The caller info attributes provide a way to pass such information transparently.</span></span>

<span data-ttu-id="9a3ef-327">Wenn ein optionaler Parameter mit einem der Attribute "callerinfo" kommentiert wird, bewirkt das Auslassen von des entsprechenden Arguments in einem Aufruf nicht, unbedingt den standardmäßige Parameterwert ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-327">When an optional parameter is annotated with one of the caller info attributes, omitting the corresponding argument in a call does not necessarily cause the default parameter value to be substituted.</span></span> <span data-ttu-id="9a3ef-328">Wenn die angegebene Informationen zu den aufrufenden Kontext verfügbar ist, wird diese Informationen stattdessen als Wert des Arguments übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-328">Instead, if the specified information about the calling context is available, that information will be passed as the argument value.</span></span>

<span data-ttu-id="9a3ef-329">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9a3ef-329">For example:</span></span>

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

<span data-ttu-id="9a3ef-330">Ein Aufruf von `Log()` ohne Argumente wird drucken, die Zeile Anzahl und den Dateipfad des Aufrufs sowie der Name des Elements in dem der Aufruf aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-330">A call to `Log()` with no arguments would print the line number and file path of the call, as well as the name of the member within which the call occurred.</span></span>

<span data-ttu-id="9a3ef-331">Attribute "callerinfo" können auf eine beliebige Stelle, optionale Parameter auftreten, einschließlich Delegaten verwendet.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-331">Caller info attributes can occur on optional parameters anywhere, including in delegate declarations.</span></span> <span data-ttu-id="9a3ef-332">Allerdings müssen die Attribute "callerinfo" bestimmte Einschränkungen für die Typen der Parameter, die sie Attribut können, damit es wird immer eine implizite Konvertierung von einem ersetzten Werts auf den Parametertyp vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-332">However, the specific caller info attributes have restrictions on the types of the parameters they can attribute, so that there will always be an implicit conversion from a substituted value to the parameter type.</span></span>

<span data-ttu-id="9a3ef-333">Es ist ein Fehler, wenn das gleiche Aufrufer Info-Attribut für einen Parameter sowohl die zum Definieren und Implementieren eines Teils der Deklaration einer partiellen Methode.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-333">It is an error to have the same caller info attribute on a parameter of both the defining and implementing part of a partial method declaration.</span></span> <span data-ttu-id="9a3ef-334">Nur Attribute "callerinfo" in der definierenden Teil werden angewendet, während die Aufrufer-Informationsattribute, die nur in der implementierenden Teil auftreten ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-334">Only caller info attributes in the defining part are applied, whereas caller info attributes occurring only in the implementing part are ignored.</span></span>

<span data-ttu-id="9a3ef-335">Aufruferinformationen hat keine Auswirkungen auf die überladungsauflösung.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-335">Caller information does not affect overload resolution.</span></span> <span data-ttu-id="9a3ef-336">Wie die attributierte optionalen Parameter noch aus dem Quellcode des Aufrufers ausgelassen werden, ignoriert Auflösung von funktionsüberladungen dieser Parameter auf die gleiche Weise, die sie andere ausgelassenen optionale Parameter ignoriert ([Überladungsauflösung](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="9a3ef-336">As the attributed optional parameters are still omitted from the source code of the caller, overload resolution ignores those parameters in the same way it ignores other omitted optional parameters ([Overload resolution](expressions.md#overload-resolution)).</span></span>

<span data-ttu-id="9a3ef-337">Aufruferinformationen wird nur ersetzt, wenn eine Funktion im Quellcode explizit aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-337">Caller information is only substituted when a function is explicitly invoked in source code.</span></span> <span data-ttu-id="9a3ef-338">Implizite Aufrufe wie z. B. Konstruktoraufrufe implizite übergeordnete Element haben keine Quellort und Aufruferinformationen nicht ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-338">Implicit invocations such as implicit parent constructor calls do not have a source location and will not substitute caller information.</span></span> <span data-ttu-id="9a3ef-339">Darüber hinaus werden Aufrufe, die dynamisch gebunden sind keine Aufruferinformationen ersetzen.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-339">Also, calls that are dynamically bound will not substitute caller information.</span></span> <span data-ttu-id="9a3ef-340">Wenn ein Aufrufer-Informationsattribute, die in solchen Fällen attributierten Parameter ausgelassen wird, wird stattdessen der angegebene Standardwert des Parameters verwendet.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-340">When a caller info attributed parameter is omitted in such cases, the specified default value of the parameter is used instead.</span></span>

<span data-ttu-id="9a3ef-341">Eine Ausnahme ist die Abfrage-Ausdrücken.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-341">One exception is query-expressions.</span></span> <span data-ttu-id="9a3ef-342">Gelten diese syntaktische Erweiterungen, und wenn die Aufrufe sie erweitert, um optionale Parameter mit der Attribute "callerinfo" weglassen, wird die Aufruferinformationen ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-342">These are considered syntactic expansions, and if the calls they expand to omit optional parameters with caller info attributes, caller information will be substituted.</span></span> <span data-ttu-id="9a3ef-343">Der Speicherort ist der Speicherort der Abfrageklausel, die der Aufruf von generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-343">The location used is the location of the query clause which the call was generated from.</span></span>

<span data-ttu-id="9a3ef-344">Wenn mehr als einem Aufrufer Info-Attribut auf einen bestimmten Parameter angegeben wird, werden sie in der folgenden Reihenfolge bevorzugt: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-344">If more than one caller info attribute is specified on a given parameter, they are preferred in the following order: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span></span>

#### <a name="the-callerlinenumber-attribute"></a><span data-ttu-id="9a3ef-345">Das Attribut CallerLineNumber</span><span class="sxs-lookup"><span data-stu-id="9a3ef-345">The CallerLineNumber attribute</span></span>

<span data-ttu-id="9a3ef-346">Die `System.Runtime.CompilerServices.CallerLineNumberAttribute` ist für optionale Parameter zulässig, wenn eine implizite standardkonvertierung besteht ([Standard implizite Konvertierungen](conversions.md#standard-implicit-conversions)) aus den konstanten Wert `int.MaxValue` auf den Typ des Parameters.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-346">The `System.Runtime.CompilerServices.CallerLineNumberAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from the constant value `int.MaxValue` to the parameter's type.</span></span> <span data-ttu-id="9a3ef-347">Dadurch wird sichergestellt, dass eine positive Zeilennummer bis zu diesem Wert kann, ohne Fehler übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-347">This ensures that any non-negative line number up to that value can be passed without error.</span></span>

<span data-ttu-id="9a3ef-348">Wenn ein Funktionsaufruf über einen Ort im Quellcode mit einen optionalen Parameter lässt die `CallerLineNumberAttribute`, und klicken Sie dann ein numerisches Literal, das die Nummer der Zeile des Speicherorts darstellt, die als Argument für den Aufruf statt des Standardwerts für den Parameter verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-348">If a function invocation from a location in source code omits an optional parameter with the `CallerLineNumberAttribute`, then a numeric literal representing that location's line number is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="9a3ef-349">Wenn der Aufruf mehrere Zeilen erstreckt, ist die ausgewählte Zeile implementierungsabhängig.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-349">If the invocation spans multiple lines, the line chosen is implementation-dependent.</span></span>

<span data-ttu-id="9a3ef-350">Beachten Sie, die die Nummer der Zeile betroffen werden möglicherweise `#line` Direktiven ([Zeile Direktiven](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="9a3ef-350">Note that the line number may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callerfilepath-attribute"></a><span data-ttu-id="9a3ef-351">Das Attribut CallerFilePath</span><span class="sxs-lookup"><span data-stu-id="9a3ef-351">The CallerFilePath attribute</span></span>

<span data-ttu-id="9a3ef-352">Die `System.Runtime.CompilerServices.CallerFilePathAttribute` ist für optionale Parameter zulässig, wenn eine implizite standardkonvertierung besteht ([Standard implizite Konvertierungen](conversions.md#standard-implicit-conversions)) von `string` auf den Typ des Parameters.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-352">The `System.Runtime.CompilerServices.CallerFilePathAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="9a3ef-353">Wenn ein Funktionsaufruf über einen Ort im Quellcode mit einen optionalen Parameter lässt die `CallerFilePathAttribute`, und klicken Sie dann ein Zeichenfolgenliteral, des Speicherorts Dateipfad darstellt, die als Argument für den Aufruf statt des Standardwerts für den Parameter verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-353">If a function invocation from a location in source code omits an optional parameter with the `CallerFilePathAttribute`, then a string literal representing that location's file path is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="9a3ef-354">Das Format des Dateipfads ist implementierungsabhängig.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-354">The format of the file path is implementation-dependent.</span></span>

<span data-ttu-id="9a3ef-355">Beachten Sie, die der Dateipfad von betroffen sein könnten `#line` Direktiven ([Zeile Direktiven](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="9a3ef-355">Note that the file path may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callermembername-attribute"></a><span data-ttu-id="9a3ef-356">Das CallerMemberName-Attribut</span><span class="sxs-lookup"><span data-stu-id="9a3ef-356">The CallerMemberName attribute</span></span>

<span data-ttu-id="9a3ef-357">Die `System.Runtime.CompilerServices.CallerMemberNameAttribute` ist für optionale Parameter zulässig, wenn eine implizite standardkonvertierung besteht ([Standard implizite Konvertierungen](conversions.md#standard-implicit-conversions)) von `string` auf den Typ des Parameters.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-357">The `System.Runtime.CompilerServices.CallerMemberNameAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="9a3ef-358">Wenn das Funktionselement selbst oder der Rückgabetyp, Parameter oder Typparameter in ein Funktionsaufruf in einen Speicherort innerhalb des Texts ein Funktionsmember oder innerhalb eines Attributs angewendet lässt Quellcode einen optionalen Parameter mit der `CallerMemberNameAttribute`, ein Zeichenfolgenliteral, die den Namen dieses Elements wird als Argument für den Aufruf statt des Standardwerts für den Parameter verwendet.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-358">If a function invocation from a location within the body of a function member or within an attribute applied to the function member itself or its return type, parameters or type parameters in source code omits an optional parameter with the `CallerMemberNameAttribute`, then a string literal representing the name of that member is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="9a3ef-359">Aufrufe, die innerhalb generischer Methoden auftreten, wird nur der Methodennamen selbst, ohne die Liste der Typparameter verwendet.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-359">For invocations that occur within generic methods, only the method name itself is used, without the type parameter list.</span></span>

<span data-ttu-id="9a3ef-360">Aufrufe, die in der expliziten Implementierungen eines Schnittstellenmembers auftreten, wird nur der Methodennamen selbst, ohne die Kennzeichnung durch einen vorherigen Schnittstelle verwendet.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-360">For invocations that occur within explicit interface member implementations, only the method name itself is used, without the preceding interface qualification.</span></span>

<span data-ttu-id="9a3ef-361">Für Aufrufe, die in Eigenschaften- oder Ereignisaccessoren auftreten, ist der Elementname verwendet, die der Eigenschaft oder Ereignis selbst.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-361">For invocations that occur within property or event accessors, the member name used is that of the property or event itself.</span></span>

<span data-ttu-id="9a3ef-362">Aufrufe, die in Indexeraccessoren auftreten, der Namen des Members verwendet wird, angegeben durch eine `IndexerNameAttribute` ([das IndexerName-Attribut](attributes.md#the-indexername-attribute)) auf das Indexer-Element, sofern vorhanden, oder den Standardnamen `Item` andernfalls.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-362">For invocations that occur within indexer accessors, the member name used is that supplied by an `IndexerNameAttribute` ([The IndexerName attribute](attributes.md#the-indexername-attribute)) on the indexer member, if present, or the default name `Item` otherwise.</span></span>

<span data-ttu-id="9a3ef-363">Aufrufe, die auftreten, in den Deklarationen von Instanzkonstruktoren, statischen Konstruktoren, Destruktoren und Operatoren den Member ist Name abhängig von der Implementierung.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-363">For invocations that occur within declarations of instance constructors, static constructors, destructors and operators the member name used is implementation-dependent.</span></span>

## <a name="attributes-for-interoperation"></a><span data-ttu-id="9a3ef-364">Attribute für die Interoperation</span><span class="sxs-lookup"><span data-stu-id="9a3ef-364">Attributes for Interoperation</span></span>

<span data-ttu-id="9a3ef-365">Hinweis: Dieser Abschnitt gilt nur für die Microsoft .NET Implementierung C#.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-365">Note: This section is applicable only to the Microsoft .NET implementation of C#.</span></span>

### <a name="interoperation-with-com-and-win32-components"></a><span data-ttu-id="9a3ef-366">Interoperation mit COM- und Win32-Komponenten</span><span class="sxs-lookup"><span data-stu-id="9a3ef-366">Interoperation with COM and Win32 components</span></span>

<span data-ttu-id="9a3ef-367">Die Laufzeit .NET bietet eine große Anzahl von Attributen, die C#-Programme für die Zusammenarbeit mit Komponenten geschrieben wurden, mithilfe von COM und Win32-DLLs zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-367">The .NET run-time provides a large number of attributes that enable C# programs to interoperate with components written using COM and Win32 DLLs.</span></span> <span data-ttu-id="9a3ef-368">Z. B. die `DllImport` Attribut kann verwendet werden, auf eine `static extern` Methode, um anzugeben, dass die Implementierung der Methode in einer Win32-DLL gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-368">For example, the `DllImport` attribute can be used on a `static extern` method to indicate that the implementation of the method is to be found in a Win32 DLL.</span></span> <span data-ttu-id="9a3ef-369">Diese Attribute finden Sie in der `System.Runtime.InteropServices` -Namespace, und eine detaillierte Dokumentation für diese Attribute in der Dokumentation von .NET Common Language Runtime befindet.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-369">These attributes are found in the `System.Runtime.InteropServices` namespace, and detailed documentation for these attributes is found in the .NET runtime documentation.</span></span>

### <a name="interoperation-with-other-net-languages"></a><span data-ttu-id="9a3ef-370">Interoperation mit anderen .NET-Sprachen</span><span class="sxs-lookup"><span data-stu-id="9a3ef-370">Interoperation with other .NET languages</span></span>

#### <a name="the-indexername-attribute"></a><span data-ttu-id="9a3ef-371">Das IndexerName-Attribut</span><span class="sxs-lookup"><span data-stu-id="9a3ef-371">The IndexerName attribute</span></span>

<span data-ttu-id="9a3ef-372">Indexer sind in .NET unter Verwendung von indizierten Eigenschaften implementiert, und einen Namen in den Metadaten für .NET.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-372">Indexers are implemented in .NET using indexed properties, and have a name in the .NET metadata.</span></span> <span data-ttu-id="9a3ef-373">Wenn kein `IndexerName` Attribut ist für einen Indexer, und klicken Sie dann auf den Namen `Item` wird standardmäßig verwendet.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-373">If no `IndexerName` attribute is present for an indexer, then the name `Item` is used by default.</span></span> <span data-ttu-id="9a3ef-374">Die `IndexerName` -Attribut ermöglicht dem Entwickler diesen Standardwert überschreiben und einen anderen Namen angeben.</span><span class="sxs-lookup"><span data-stu-id="9a3ef-374">The `IndexerName` attribute enables a developer to override this default and specify a different name.</span></span>

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
