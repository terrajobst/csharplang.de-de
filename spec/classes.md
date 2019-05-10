---
ms.openlocfilehash: af7af574814dc04ee3ece0396b7ae5f86b3ec8eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488899"
---
# <a name="classes"></a><span data-ttu-id="cfde9-101">Klassen</span><span class="sxs-lookup"><span data-stu-id="cfde9-101">Classes</span></span>

<span data-ttu-id="cfde9-102">Eine Klasse ist eine Datenstruktur, die Daten Mitglieder (Konstanten und Feldern), Funktionsmember (Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren, Instanzkonstruktoren, Destruktoren und statische Konstruktoren) und geschachtelte Typen enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="cfde9-103">Klassentypen unterstützen die Vererbung, einen Mechanismus, bei dem eine abgeleitete Klasse erweitern und spezialisiert eine Basisklasse kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="cfde9-104">Klassendeklarationen</span><span class="sxs-lookup"><span data-stu-id="cfde9-104">Class declarations</span></span>

<span data-ttu-id="cfde9-105">Ein *Class_declaration* ist eine *Type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), die eine neue Klasse deklariert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="cfde9-106">Ein *Class_declaration* besteht aus einer optionalen Gruppe von *Attribute* ([Attribute](attributes.md)), gefolgt von einer optionalen Gruppe von *Class_modifier*s ([Klasse Modifizierer](classes.md#class-modifiers)), gefolgt von einem optionalen `partial` Modifizierer, gefolgt vom Schlüsselwort `class` und ein *Bezeichner* mit dem Namen der Klasse, gefolgt von einem optionale *Type_parameter_list* ([Typparameter](classes.md#type-parameters)), gefolgt von einem optionalen *Class_base* Spezifikation ([Klasse Basis Spezifikation](classes.md#class-base-specification)), gefolgt von einer optionalen Gruppe von *Type_parameter_constraints_clause*s ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)), gefolgt von einem *Class_body*  ([Klasse Text](classes.md#class-body)), optional gefolgt durch ein Semikolon.</span><span class="sxs-lookup"><span data-stu-id="cfde9-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="cfde9-107">Eine Klassendeklaration kann nicht bereitstellen *Type_parameter_constraints_clause*s es sei denn, sie verfügt auch über eine *Type_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="cfde9-108">Eine Klassendeklaration, die bereitstellt eine *Type_parameter_list* ist eine ***generischen Klassendeklaration***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="cfde9-109">Darüber hinaus ist jeder Klasse, die in der Deklaration einer generischen Klasse oder einer generischen Strukturdeklaration geschachtelt selbst die Deklaration einer generischen Klasse, da der Typparameter für den enthaltenden Typ angegeben werden müssen, um einen konstruierten Typ zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="cfde9-110">Klassenmodifizierer</span><span class="sxs-lookup"><span data-stu-id="cfde9-110">Class modifiers</span></span>

<span data-ttu-id="cfde9-111">Ein *Class_declaration* kann optional eine Sequenz von Klassenmodifizierer enthalten:</span><span class="sxs-lookup"><span data-stu-id="cfde9-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

<span data-ttu-id="cfde9-112">Es ist ein Fehler während der Kompilierung für den gleichen Modifizierer für mehrere Male in einer Klassendeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="cfde9-113">Die `new` -Modifizierer ist für geschachtelte Klassen zulässig.</span><span class="sxs-lookup"><span data-stu-id="cfde9-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="cfde9-114">Es gibt an, dass die Klasse mit dem gleichen Namen, Blendet einen geerbten Member aus wie in beschrieben [der new-Modifizierer](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="cfde9-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="cfde9-115">Es ist ein Fehler während der Kompilierung für die `new` Modifizierer, um in einer Klassendeklaration angezeigt werden, die nicht auf eine Deklaration der geschachtelten Klasse ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="cfde9-116">Die `public`, `protected`, `internal`, und `private` Modifizierern steuern den Zugriff auf die Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="cfde9-117">Je nach Kontext, in der Deklaration der Klasse steht, einige dieser Modifizierer kann nicht gestattet ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="cfde9-118">Die `abstract`, `sealed` und `static` Modifizierer werden in den folgenden Abschnitten erläutert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="cfde9-119">Abstrakte Klassen</span><span class="sxs-lookup"><span data-stu-id="cfde9-119">Abstract classes</span></span>

<span data-ttu-id="cfde9-120">Die `abstract` Modifizierer wird verwendet, um anzugeben, dass eine Klasse unvollständig ist und es wird nur als Basisklasse verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="cfde9-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="cfde9-121">Eine abstrakte Klasse unterscheidet sich von einer nicht abstrakten Klasse gibt folgenden Möglichkeiten:</span><span class="sxs-lookup"><span data-stu-id="cfde9-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="cfde9-122">Eine abstrakte Klasse kann nicht direkt instanziiert werden, und es ist ein Fehler während der Kompilierung mit der `new` Operator für eine abstrakte Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="cfde9-123">Es ist, zwar möglich, Variablen und Werte, deren Typen während der Kompilierung abstrakt sind diesen Variablen und Werten handelt es sich entweder unbedingt `null` oder enthält Verweise auf Instanzen von nicht abstrakte Klassen, die von der abstrakten Typen abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="cfde9-124">Eine abstrakte Klasse ist zulässig (jedoch nicht erforderlich) abstrakte Member enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="cfde9-125">Eine abstrakte Klasse kann nicht versiegelt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="cfde9-126">Wenn eine nicht abstrakte Klasse von einer abstrakten Klasse abgeleitet ist, muss der nicht abstrakten Klasse Implementierungen aller geerbten abstrakten Member, und überschreiben die abstrakte Member enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="cfde9-127">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-127">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
<span data-ttu-id="cfde9-128">die abstrakte Klasse `A` stellt eine abstrakte Methode `F`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="cfde9-129">Klasse `B` führt eine zusätzliche Methode `G`, da es eine Implementierung bereitstellen, nicht jedoch `F`, `B` muss auch als abstrakt deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="cfde9-130">Klasse `C` überschreibt `F` und eine tatsächliche Implementierung bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="cfde9-131">Da es sich um keine abstrakten Member in `C`, `C` ist zulässig (jedoch nicht erforderlich), der nicht abstrakt ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="cfde9-132">Versiegelte Klassen</span><span class="sxs-lookup"><span data-stu-id="cfde9-132">Sealed classes</span></span>

<span data-ttu-id="cfde9-133">Die `sealed` Modifizierer wird verwendet, um zu verhindern, dass bei der Ableitung von einer Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="cfde9-134">Ein Fehler während der Kompilierung tritt auf, wenn eine versiegelte Klasse als Basisklasse einer anderen Klasse angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="cfde9-135">Eine versiegelte Klasse darf nicht auch eine abstrakte Klasse sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="cfde9-136">Die `sealed` Modifizierer wird in erster Linie zum Verhindern von unbeabsichtigten ableitungen, aber sie können zudem bestimmte Optimierungen für die Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="cfde9-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="cfde9-137">Insbesondere, da eine versiegelte Klasse alle abgeleiteten Klassen nie damit bekannt ist, ist es möglich, eine virtuelle Funktion, die von Memberaufrufen für versiegelte Klasse-Instanzen in nicht-virtuelle Aufrufe zu transformieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="cfde9-138">Statische Klassen</span><span class="sxs-lookup"><span data-stu-id="cfde9-138">Static classes</span></span>

<span data-ttu-id="cfde9-139">Die `static` Modifizierer wird verwendet, um die Klasse deklariert wird, als Markieren einer ***statische Klasse***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="cfde9-140">Eine statische Klasse kann nicht instanziiert werden, kann nicht als Typ verwendet werden und kann nur statische Member enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="cfde9-141">Nur eine statische Klasse Deklarationen von Erweiterungsmethoden enthalten kann ([Erweiterungsmethoden](classes.md#extension-methods)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="cfde9-142">Eine statische Klasse-Deklaration ist jedoch mit folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="cfde9-143">Eine statische Klasse enthält möglicherweise keine `sealed` oder `abstract` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="cfde9-144">Beachten Sie jedoch, da eine statische Klasse kann nicht instanziiert oder abgeleitet werden, verhält sich als wäre es versiegelt und abstrakt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="cfde9-145">Eine statische Klasse enthält möglicherweise keine *Class_base* Spezifikation ([Klasse basisspezifizierung](classes.md#class-base-specification)) und nicht explizit angeben, eine Basisklasse oder eine Liste der implementierten Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="cfde9-146">Eine statische Klasse erbt implizit vom Typ `object`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="cfde9-147">Eine statische Klasse kann nur statische Member enthalten ([statische und Instanzmember](classes.md#static-and-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="cfde9-148">Beachten Sie, dass Konstanten und geschachtelte Typen als statische Member klassifiziert sind.</span><span class="sxs-lookup"><span data-stu-id="cfde9-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="cfde9-149">Eine statische Klasse sind keine Elemente mit `protected` oder `protected internal` Barrierefreiheit deklariert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="cfde9-150">Es ist ein Fehler während der Kompilierung, diese Einschränkungen zu verletzen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="cfde9-151">Eine statische Klasse verfügt über keine Instanzkonstruktoren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-151">A static class has no instance constructors.</span></span> <span data-ttu-id="cfde9-152">Es ist nicht möglich, Deklaration eines Instanzkonstruktors in einer statischen Klasse und kein Standardkonstruktor für die Instanz ([Standardkonstruktoren](classes.md#default-constructors)) wird bereitgestellt, um eine statische Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="cfde9-153">Die Member einer statischen Klasse sind nicht automatisch statisch, und die Memberdeklarationen müssen explizit eine `static` Modifizierer (mit Ausnahme von Konstanten und geschachtelte Typen).</span><span class="sxs-lookup"><span data-stu-id="cfde9-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="cfde9-154">Wenn eine Klasse in einer statischen äußeren Klasse geschachtelt ist, die geschachtelte Klasse ist keine statische Klasse, sofern es explizit enthält eine `static` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="cfde9-155">__Statische Klasse Verweistypen__</span><span class="sxs-lookup"><span data-stu-id="cfde9-155">__Referencing static class types__</span></span>

<span data-ttu-id="cfde9-156">Ein *Namespace_or_type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)) ist auf eine statische Klasse verweisen, wenn zulässig</span><span class="sxs-lookup"><span data-stu-id="cfde9-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="cfde9-157">Die *Namespace_or_type_name* ist die `T` in einem *Namespace_or_type_name* des Formulars `T.I`, oder</span><span class="sxs-lookup"><span data-stu-id="cfde9-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="cfde9-158">Die *Namespace_or_type_name* ist die `T` in einem *Typeof_expression* ([Argumentlisten](expressions.md#argument-lists)1) des Formulars `typeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="cfde9-159">Ein *Primary_expression* ([Funktionsmember](expressions.md#function-members)) ist auf eine statische Klasse verweisen, wenn zulässig</span><span class="sxs-lookup"><span data-stu-id="cfde9-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="cfde9-160">Die *Primary_expression* ist die `E` in einem *Member_access* ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) des Formulars `E.I`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="cfde9-161">In einem anderen Kontext ist es ein Fehler während der Kompilierung, um auf eine statische Klasse verweisen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="cfde9-162">Es ist z. B. einen Fehler für eine statische Klasse als eine Basisklasse, die einen einzelnen Typ verwendet werden soll ([geschachtelte Typen](classes.md#nested-types)) ein Element, ein generisches Typargument oder eine Einschränkung für einen Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="cfde9-163">Ebenso eine statische Klasse kann nicht in einen Arraytyp, ein Zeigertyp verwendet werden kann eine `new` Ausdruck, der ein Cast-Ausdruck, ein `is` Ausdruck eine `as` Ausdruck eine `sizeof` Ausdruck oder eine Default-Wertausdruck.</span><span class="sxs-lookup"><span data-stu-id="cfde9-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="cfde9-164">Ein partial-Modifizierer</span><span class="sxs-lookup"><span data-stu-id="cfde9-164">Partial modifier</span></span>

<span data-ttu-id="cfde9-165">Die `partial` Modifizierer verwendet, um anzugeben, das von diesem *Class_declaration* ist eine Deklaration der partiellen Typ.</span><span class="sxs-lookup"><span data-stu-id="cfde9-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="cfde9-166">Mehrere partielle Typdeklarationen mit dem gleichen Namen in einer einschließenden Namespace oder Typ Deklaration kombiniert werden, um eine Typdeklaration Form, in angegebenen gemäß den Regeln [partielle Typen](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="cfde9-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="cfde9-167">Müssen die Deklaration einer Klasse, die über separate Segmente des Programmtexts verteilt kann nützlich sein, wenn diese Segmente erstellt oder in unterschiedlichen Kontexten verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="cfde9-168">Z. B. möglicherweise ein Teil einer Klassendeklaration generiert, Computer, während die andere manuell erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="cfde9-169">Text Trennung zwischen den beiden verhindert, dass Updates von einem von in Konflikt stehende mit Updates von anderen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="cfde9-170">Typparameter</span><span class="sxs-lookup"><span data-stu-id="cfde9-170">Type parameters</span></span>

<span data-ttu-id="cfde9-171">Ein Typparameter ist ein einfacher Bezeichner, der einen Platzhalter für ein Typargument, das zum Erstellen eines konstruierten Typs bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="cfde9-172">Ein Typparameter ist eine formale Platzhalter für einen Typ, der später angegeben werden, wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="cfde9-173">Im Gegensatz dazu ein Typargument ([Typargumente](types.md#type-arguments)) ist der tatsächliche Typ, der für den Typparameter ersetzt wird, wenn ein konstruierter Typ erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

<span data-ttu-id="cfde9-174">Jeden Typparameter in einer Klassendeklaration definiert einen Namen im Deklarationsbereich ([Deklarationen](basic-concepts.md#declarations)) dieser Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="cfde9-175">Also, es kann nicht den gleichen Namen wie ein anderer Typparameter aufweisen oder ein Member, die in dieser Klasse deklariert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="cfde9-176">Ein Typparameter kann nicht den gleichen Namen wie der Typ selbst aufweisen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="cfde9-177">Die Basisspezifikationen Klasse</span><span class="sxs-lookup"><span data-stu-id="cfde9-177">Class base specification</span></span>

<span data-ttu-id="cfde9-178">Eine Klassendeklaration enthalten möglicherweise eine *Class_base* -Spezifikation, die die direkte Basisklasse der Klasse und die Schnittstellen definiert ([Schnittstellen](interfaces.md)) direkt von der Klasse implementiert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

<span data-ttu-id="cfde9-179">Die Basisklasse, die in einer Klassendeklaration angegeben, kann ein konstruierten Klasse-Typ sein ([Typen konstruiert](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="cfde9-180">Eine Basisklasse darf nicht Typparameter in ihren eigenen sein, obwohl er die Typparameter umfassen kann, die im Gültigkeitsbereich befinden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="cfde9-181">Basisklassen</span><span class="sxs-lookup"><span data-stu-id="cfde9-181">Base classes</span></span>

<span data-ttu-id="cfde9-182">Wenn eine *Class_type* befindet sich auf die *Class_base*, wird die direkte Basisklasse der Klasse deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="cfde9-183">Wenn eine Deklaration der Klasse keine *Class_base*, oder wenn die *Class_base* Listen nur Schnittstellentypen, die direkte Basisklasse wird als `object`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="cfde9-184">Eine Klasse erbt von der direkten Basisklasse, Member, wie in beschrieben [Vererbung](classes.md#inheritance).</span><span class="sxs-lookup"><span data-stu-id="cfde9-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="cfde9-185">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="cfde9-186">Klasse `A` heißt es, die direkte Basisklasse von `B`, und `B` gilt als abgeleitet werden `A`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="cfde9-187">Da `A` ist keine direkte Basisklasse nicht explizit angeben, die direkte Basisklasse ist implizit `object`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="cfde9-188">Für einen konstruierten Klasse, wenn eine Basisklasse in der generischen Klassendeklaration angegeben ist die Basisklasse des konstruierten Typs wird abgerufen, indem ersetzen, für die einzelnen *Type_parameter* in der entsprechenden Basisklassendeklaration *Type_argument* des konstruierten Typs.</span><span class="sxs-lookup"><span data-stu-id="cfde9-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="cfde9-189">Erhalten die generische Klassendeklarationen</span><span class="sxs-lookup"><span data-stu-id="cfde9-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="cfde9-190">die Basisklasse des konstruierten Typs `G<int>` wäre `B<string,int[]>`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="cfde9-191">Die direkte Basisklasse eines Klassentyps muss mindestens dieselben zugriffsmöglichkeiten wie der Klassentyp selbst bieten ([Barrierefreiheit Domänen](basic-concepts.md#accessibility-domains)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="cfde9-192">Es ist z. B. ein Fehler während der Kompilierung für eine `public` Klasse abgeleitet eine `private` oder `internal` Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="cfde9-193">Die direkte Basisklasse eines Klassentyps muss keine der folgenden Typen sein: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, oder `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="cfde9-194">Darüber hinaus Deklaration einer generischen Klasse können keine `System.Attribute` als eine direkte oder indirekte Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="cfde9-195">Beim Bestimmen der Bedeutung der direkten Basisklasse-Spezifikation `A` einer Klasse `B`, die direkte Basisklasse von `B` wird vorübergehend als `object`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="cfde9-196">Intuitiv Dadurch wird sichergestellt, dass die Bedeutung einer basisklassenspezifikation rekursiv kann nicht auf sich selbst abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="cfde9-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="cfde9-197">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cfde9-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="cfde9-198">ist fehlerhaft. seit in der Basisklasse-Spezifikation `A<C.B>` die direkte Basisklasse von `C` gilt `object`, und somit (durch die Regeln zur [Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)) `C` nicht gilt kein Element `B`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="cfde9-199">Die Basisklassen eines Klassentyps sind die direkte Basisklasse und ihre Basisklassen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="cfde9-200">Das heißt, ist die Reihe von Basisklassen den transitiven Abschluss von der direkten basisklassenbeziehung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="cfde9-201">Wie im Beispiel oben die Basisklassen `B` sind `A` und `object`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="cfde9-202">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="cfde9-203">die Basisklassen `D<int>` sind `C<int[]>`, `B<IComparable<int[]>>`, `A`, und `object`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="cfde9-204">Mit Ausnahme der Klasse `object`, jeder Klassentyp verfügt über genau eine direkte Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="cfde9-205">Die `object` Klasse hat keine direkte Basisklasse und ist die ultimative Basisklasse aller anderen Klassen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="cfde9-206">Wenn eine Klasse `B` leitet sich von einer Klasse `A`, es ist ein Fehler während der Kompilierung für `A` hängt `B`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="cfde9-207">Eine Klasse ***hängt direkt von*** seine direkte Basisklasse (sofern vorhanden) und ***hängt direkt von*** der Klasse, in dem es sofort geschachtelt ist (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="cfde9-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="cfde9-208">Dank dieser der vollständige Satz von Klassen, die von dem eine Klasse abhängig ist, wird den reflexiv und transitiv Abschluss von der ***hängt direkt von*** Beziehung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="cfde9-209">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="cfde9-210">ist fehlerhaft, da die Klasse von sich selbst abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="cfde9-211">Ebenso, Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="cfde9-212">ist fehlerhaft, da die Klassen zirkulär voneinander abhängig.</span><span class="sxs-lookup"><span data-stu-id="cfde9-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="cfde9-213">Zum Schluss das Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="cfde9-214">führt zu einem Kompilierzeitfehler, da `A` hängt `B.C` (seine direkte Basisklasse), abhängig vom `B` (der unmittelbar einschließenden Klasse), zirkulär abhängig vom `A`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="cfde9-215">Beachten Sie, dass eine Klasse nicht von den Klassen abhängt, die darin geschachtelt sind.</span><span class="sxs-lookup"><span data-stu-id="cfde9-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="cfde9-216">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="cfde9-217">`B` hängt von `A` (da `A` ist seine direkte Basisklasse und die Sie unmittelbar umschließende Klasse), aber `A` hängt nicht `B` (da `B` ist weder eine Basisklasse als auch von einer einschließenden Klasse `A` ).</span><span class="sxs-lookup"><span data-stu-id="cfde9-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="cfde9-218">Im Beispiel ist daher gültig.</span><span class="sxs-lookup"><span data-stu-id="cfde9-218">Thus, the example is valid.</span></span>

<span data-ttu-id="cfde9-219">Es ist nicht möglich, für die Ableitung einer `sealed` Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="cfde9-220">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="cfde9-221">Klasse `B` ist falsch, weil er versucht, für die Ableitung der `sealed` Klasse `A`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="cfde9-222">Schnittstellenimplementierungen</span><span class="sxs-lookup"><span data-stu-id="cfde9-222">Interface implementations</span></span>

<span data-ttu-id="cfde9-223">Ein *Class_base* möglicherweise eine Spezifikation enthalten eine Liste der Schnittstellentypen, in dem Fall die Klasse wird als die angegebene Schnittstelle-Typen direkt zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="cfde9-224">Schnittstellenimplementierungen finden Sie weiter unten in [Schnittstellenimplementierungen](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="cfde9-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="cfde9-225">Einschränkungen für Typparameter</span><span class="sxs-lookup"><span data-stu-id="cfde9-225">Type parameter constraints</span></span>

<span data-ttu-id="cfde9-226">Generische Typ- und Methodendeklarationen können optional die Einschränkungen für Typparameter angeben, indem einschließlich *Type_parameter_constraints_clause*s.</span><span class="sxs-lookup"><span data-stu-id="cfde9-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

<span data-ttu-id="cfde9-227">Jede *Type_parameter_constraints_clause* besteht aus dem Token `where`, gefolgt vom Namen eines Typparameters, gefolgt von einem Doppelpunkt und die Liste der Einschränkungen für den Typparameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="cfde9-228">Es kann höchstens eine `where` -Klausel für jeden Typparameter, und die `where` Klauseln können in beliebiger Reihenfolge aufgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="cfde9-229">Wie die `get` und `set` Token in einem Eigenschaftenaccessor der `where` Token ist kein Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="cfde9-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="cfde9-230">Die Liste der Einschränkungen, die im angegebenen ein `where` -Klausel kann eine der folgenden Komponenten, in der angegebenen Reihenfolge enthalten: eine einzelne primary-Einschränkung, einer oder mehreren sekundären Einschränkungen und die Konstruktoreinschränkung `new()`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="cfde9-231">Eine primary-Einschränkung kann ein Klassentyp sein oder die ***verweisen auf eine Einschränkung*** `class` oder ***Wert typeinschränkung*** `struct`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="cfde9-232">Eine sekundäre Einschränkung möglich einen *Type_parameter* oder *Interface_type*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="cfde9-233">Der verweistypeinschränkung gibt an, dass ein Typargument verwendet, für den Typparameter ein Verweistyp sein muss.</span><span class="sxs-lookup"><span data-stu-id="cfde9-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="cfde9-234">Alle Klassentypen, Schnittstellentypen, Delegattypen, Arraytypen und Parameter vom Typ bekannt, dass ein Verweistyp (wie unten definiert) werden diese Einschränkung zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="cfde9-235">Der werttypeinschränkung gibt an, dass ein Typargument verwendet, für den Parameter ein NULL-Wert sein muss.</span><span class="sxs-lookup"><span data-stu-id="cfde9-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="cfde9-236">Alle NULL-Strukturtypen, Enumerationstypen und Parameter vom Typ der werttypeinschränkung müssen diese Einschränkung zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="cfde9-237">Beachten Sie, dass, obwohl Sie als ein Werttyp, der einen nullable-Typ klassifiziert ([auf NULL festlegbare Typen](types.md#nullable-types)) erfüllt nicht der werttypeinschränkung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="cfde9-238">Ein Typparameter mit der werttypeinschränkung keine auch die *Constructor_constraint*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="cfde9-239">Zeigertypen dürfen keine Typargumente werden und gelten nicht als entweder die Referenz Typ oder Wert typeinschränkungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="cfde9-240">Wenn eine Einschränkung eines Klassentyps, einen Schnittstellentyp aufweisen oder ein Typparameter ist, gibt dieses Typs ein minimaler "Basistyp", die jedes Typargument, das für den Typparameter verwendete unterstützen muss.</span><span class="sxs-lookup"><span data-stu-id="cfde9-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="cfde9-241">Wenn es sich bei einem konstruierten Typ oder eine generische Methode verwendet wird, wird das Typargument für die Einschränkungen für den Typparameter zum Zeitpunkt der Kompilierung überprüft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="cfde9-242">Das Typargument muss die in beschriebenen Bedingungen erfüllen, [Einschränkungen erfüllen](types.md#satisfying-constraints).</span><span class="sxs-lookup"><span data-stu-id="cfde9-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="cfde9-243">Ein *Class_type* Einschränkung muss die folgenden Regeln entsprechen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="cfde9-244">Der Typ muss ein Klassentyp sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-244">The type must be a class type.</span></span>
*  <span data-ttu-id="cfde9-245">Der Typ muss nicht `sealed`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="cfde9-246">Der Typ muss nicht einer der folgenden Typen sein: `System.Array`, `System.Delegate`, `System.Enum`, oder `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="cfde9-247">Der Typ muss nicht `object`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-247">The type must not be `object`.</span></span> <span data-ttu-id="cfde9-248">Da alle Typen abgeleitet `object`, eine solche Einschränkung hätte keine Auswirkung, wenn sie zulässig wären.</span><span class="sxs-lookup"><span data-stu-id="cfde9-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="cfde9-249">Maximal eine Einschränkung für einen bestimmten Typ-Parameter kann ein Klassentyp sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="cfde9-250">Ein Typ, der als ein *Interface_type* Einschränkung muss die folgenden Regeln entsprechen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="cfde9-251">Der Typ muss ein Schnittstellentyp sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="cfde9-252">Ein Typ nicht angegeben werden mehr als einmal einer angegebenen `where` Klausel.</span><span class="sxs-lookup"><span data-stu-id="cfde9-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="cfde9-253">In beiden Fällen die Einschränkung kann einer der Typparameter des zugeordneten Typs oder der Deklaration der Methode als Teil eines konstruierten Typs umfassen, und kann den deklarierenden Typ umfassen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="cfde9-254">Jede Klasse oder Schnittstelle-Typ angegeben wird, eine Einschränkung für einen Parameter muss mindestens dieselben Zugriffstypen unterstützt werden ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)) als generischen Typ oder Methode, die deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="cfde9-255">Ein Typ, der als ein *Type_parameter* Einschränkung muss die folgenden Regeln entsprechen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="cfde9-256">Der Typ muss es sich um ein Typparameter sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="cfde9-257">Ein Typ nicht angegeben werden mehr als einmal einer angegebenen `where` Klausel.</span><span class="sxs-lookup"><span data-stu-id="cfde9-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="cfde9-258">Darüber hinaus muss gibt es keine Zyklen im Abhängigkeitsdiagramm an Typparametern, in dem Abhängigkeit eine transitive Beziehung durch definiert ist:</span><span class="sxs-lookup"><span data-stu-id="cfde9-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="cfde9-259">Wenn ein Typparameter `T` dient als Einschränkung für den Typparameter `S` dann `S` ***hängt*** `T`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="cfde9-260">Wenn ein Typparameter `S` hängt von einem Typparameter `T` und `T` hängt von einem Typparameter `U` dann `S` ***hängt von*** `U`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="cfde9-261">Wenn diese Beziehung ist, ist es ein Fehler während der Kompilierung für einen Typparameter auf sich selbst (direkt oder indirekt) abhängig sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="cfde9-262">Alle Einschränkungen müssen zwischen abhängigen Typparameter konsistent sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="cfde9-263">Wenn der Typparameter `S` hängt von den Typparameter `T` dann:</span><span class="sxs-lookup"><span data-stu-id="cfde9-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="cfde9-264">`T` darf nicht der werttypeinschränkung sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="cfde9-265">Andernfalls `T` ist so effektiv versiegelt `S` möchten, müssen Sie den gleichen Typ wie werden `T`, beseitigt die Notwendigkeit zwei Typparametern.</span><span class="sxs-lookup"><span data-stu-id="cfde9-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="cfde9-266">Wenn `S` hat der werttypeinschränkung `T` darf keine *Class_type* Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="cfde9-267">Wenn `S` verfügt über eine *Class_type* Einschränkung `A` und `T` verfügt über eine *Class_type* Einschränkung `B` darf eine identitätskonvertierung werden oder implizit Verweisen auf die Konvertierung von `A` zu `B` oder eine implizite verweiskonvertierung von `B` zu `A`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="cfde9-268">Wenn `S` hängt auch von Typparameter `U` und `U` verfügt über eine *Class_type* Einschränkung `A` und `T` verfügt über eine *Class_type* Einschränkung `B` dann vorhanden, eine identitätskonvertierung oder implizite verweiskonvertierung von sein müssen `A` zu `B` oder eine implizite verweiskonvertierung von `B` zu `A`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="cfde9-269">Es gilt für `S` der werttypeinschränkung haben und `T` der verweistypeinschränkung haben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="cfde9-270">Effektiv begrenzt `T` zu den Typen `System.Object`, `System.ValueType`, `System.Enum`, und einen beliebigen anderen Schnittstellentyp.</span><span class="sxs-lookup"><span data-stu-id="cfde9-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="cfde9-271">Wenn die `where` -Klausel für einen Typparameter enthält eine Konstruktoreinschränkung (die weist das Format `new()`), es ist möglich, verwenden Sie die `new` Operator, um Instanzen des Typs zu erstellen ([Objektausdrücke bei der Erstellung](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="cfde9-272">Jeder Typ verwendete Argument für ein Typparameter eine Konstruktoreinschränkung für einen öffentlichen parameterlosen Konstruktor aufweisen muss (von diesem Konstruktor implizit für jeden Werttyp vorhanden) oder ein Typparameter mit der werttypeinschränkung oder Konstruktoreinschränkung (Siehe werden [Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints) Einzelheiten).</span><span class="sxs-lookup"><span data-stu-id="cfde9-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="cfde9-273">Es folgen Beispiele für Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-273">The following are examples of constraints:</span></span>
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

<span data-ttu-id="cfde9-274">Im folgende Beispiel ist fehlerhaft, da es eine Zirkularität bewirkt, in dem Abhängigkeitsdiagramm der Typparameter dass:</span><span class="sxs-lookup"><span data-stu-id="cfde9-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="cfde9-275">Die folgenden Beispiele veranschaulichen weitere ungültige Situationen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-275">The following examples illustrate additional invalid situations:</span></span>
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

<span data-ttu-id="cfde9-276">Die ***effektive Basisklasse*** eines Typparameters `T` ist wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="cfde9-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="cfde9-277">Wenn `T` besitzt keine primary-Einschränkungen oder Einschränkungen für Typparameter, effektive Basisklasse `object`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="cfde9-278">Wenn `T` hat der werttypeinschränkung, ist der effektive Basisklasse `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="cfde9-279">Wenn `T` verfügt über eine *Class_type* Einschränkung `C` aber kein *Type_parameter* Einschränkungen, die effektive Basisklasse ist `C`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="cfde9-280">Wenn `T` hat keine *Class_type* Einschränkung weist jedoch mindestens eine *Type_parameter* Einschränkungen, die effektive Basisklasse ist der am stärksten umfasste ([Konvertierung aufgehoben Operatoren](conversions.md#lifted-conversion-operators)) in der Menge der effektiven Basisklassen der *Type_parameter* Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="cfde9-281">Die Konsistenzregeln stellen Sie sicher, dass diese am stärksten umfasste ein Typ vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="cfde9-282">Wenn `T` weist sowohl ein *Class_type* Einschränkung und eine oder mehrere *Type_parameter* Einschränkungen, die effektive Basisklasse ist der am stärksten umfasste ([Konvertierung aufgehoben Operatoren](conversions.md#lifted-conversion-operators)) im Satz bestehend aus dem *Class_type* Einschränkung `T` und die effektive Basisklassen seine *Type_parameter* Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="cfde9-283">Die Konsistenzregeln stellen Sie sicher, dass diese am stärksten umfasste ein Typ vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="cfde9-284">Wenn `T` der verweistypeinschränkung hat, aber keine *Class_type* Einschränkungen, die effektive Basisklasse ist `object`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="cfde9-285">Für diese Regeln, wenn T eine Einschränkung wurde `V` d. h. eine *Value_type*, verwenden Sie stattdessen den spezifischsten Basistyp `V` , eine *Class_type*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="cfde9-286">Dies kann in einem explizit angegebenen Einschränkung nie auftreten, aber kann auftreten, wenn die Einschränkungen einer generischen Methode implizit durch einen überschreibenden Deklaration der Methode oder eine explizite Implementierung einer Schnittstellenmethode geerbt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="cfde9-287">Diese Regeln stellen sicher, dass die effektive Basisklasse immer ist eine *Class_type*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="cfde9-288">Die ***effektive Satz*** eines Typparameters `T` ist wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="cfde9-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="cfde9-289">Wenn `T` hat keine *Secondary_constraints*, dessen effektive Schnittstelle-Satz ist leer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="cfde9-290">Wenn `T` hat *Interface_type* Einschränkungen, aber keine *Type_parameter* Einschränkungen, die effektive Satz ist die Sammlung von *Interface_type* Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="cfde9-291">Wenn `T` hat keine *Interface_type* weist Einschränkungen jedoch *Type_parameter* Einschränkungen, die effektive Satz ist die Vereinigung der effektive Schnittstelle Sätze von dessen *Type_ Parameter* Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="cfde9-292">Wenn `T` weist sowohl *Interface_type* Einschränkungen und *Type_parameter* Einschränkungen, die effektive Satz ist die Vereinigung der einen Satz von *Interface_type* Einschränkungen und die effektive Schnittstelle Sätze von dessen *Type_parameter* Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="cfde9-293">Ein Typparameter ist ***bekannt, dass ein Verweistyp sein*** ist der verweistypeinschränkung hat oder ihre effektiven Basisklasse nicht `object` oder `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="cfde9-294">Werte einen eingeschränkten Typ Parametertyp können verwendet werden, auf die Instanz-Elemente, die durch die Einschränkungen impliziert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="cfde9-295">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-295">In the example</span></span>
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
<span data-ttu-id="cfde9-296">die Methoden der `IPrintable` kann aufgerufen werden, direkt auf `x` da `T` immer implementieren `IPrintable`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="cfde9-297">Text der Klasse</span><span class="sxs-lookup"><span data-stu-id="cfde9-297">Class body</span></span>

<span data-ttu-id="cfde9-298">Die *Class_body* einer Klasse definiert die Elemente dieser Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="cfde9-299">Partial types (Partielle Typen)</span><span class="sxs-lookup"><span data-stu-id="cfde9-299">Partial types</span></span>

<span data-ttu-id="cfde9-300">Eine Typdeklaration aufgeteilt werden kann, über mehrere ***partielle Typdeklarationen***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="cfde9-301">Die Deklaration des Typs wird aus seiner Teile erstellt, wird entsprechend den Regeln in diesem Abschnitt, wonach es als eine einzelne Deklaration während der Rest der Verarbeitung während der Kompilierung und Laufzeit des Programms behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="cfde9-302">Ein *Class_declaration*, *Struct_declaration* oder *Interface_declaration* eine Deklaration der partiellen Typ darstellt, sofern er enthält eine `partial` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="cfde9-303">`partial` ist kein Schlüsselwort und dient nur als ein Modifizierer, wenn es angezeigt, unmittelbar vor der eines der Schlüsselwörter wird `class`, `struct` oder `interface` in einer Typdeklaration oder vor dem Typ `void` in einer Methodendeklaration.</span><span class="sxs-lookup"><span data-stu-id="cfde9-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="cfde9-304">In anderen Kontexten können sie als ein normaler Bezeichner verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="cfde9-305">Jeder Teil einer Deklaration der partiellen Typs muss enthalten eine `partial` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="cfde9-306">Sie müssen den gleichen Namen aufweisen und in den gleichen Namespace oder die Typdeklaration als die anderen Teile deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="cfde9-307">Die `partial` Modifizierer gibt an, dass zusätzliche Teile der Typdeklaration möglicherweise an anderer Stelle vorhanden, aber das Vorhandensein von zusätzlichen Komponenten nicht erforderlich ist; es gilt für einen Typ mit einer einzigen Deklaration sollen die `partial` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="cfde9-308">Alle Teile eines partiellen Typs müssen zusammen kompiliert werden, sodass Teile zum Zeitpunkt der Kompilierung in eine einzelne Typdeklaration zusammengeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="cfde9-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="cfde9-309">Partielle Typen lassen nicht speziell bereits kompilierte Typen erweitert werden soll.</span><span class="sxs-lookup"><span data-stu-id="cfde9-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="cfde9-310">Geschachtelte Typen können aus mehreren Teilen deklariert werden, mithilfe der `partial` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="cfde9-311">In der Regel wird der enthaltende Typ mit deklariert `partial` gut, und jeder Teil des geschachtelten Typs in einem anderen Teil des enthaltenden Typs deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="cfde9-312">Die `partial` Modifizierer ist für Delegat- oder Enum-Deklarationen nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="cfde9-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="cfde9-313">Attribute</span><span class="sxs-lookup"><span data-stu-id="cfde9-313">Attributes</span></span>

<span data-ttu-id="cfde9-314">Die Attribute eines partiellen Typs werden durch die Kombination von können Sie in einer nicht vorgegebenen Reihenfolge, die Attribute der einzelnen Teile bestimmt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="cfde9-315">Wenn ein Attribut auf mehrere Teile platziert wird, entspricht das Attribut mehrmals für den Typ angegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="cfde9-316">Beispielsweise die beiden Teile:</span><span class="sxs-lookup"><span data-stu-id="cfde9-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="cfde9-317">gibt z. B. eine Deklaration entspricht:</span><span class="sxs-lookup"><span data-stu-id="cfde9-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="cfde9-318">Attribute für Typparameter auf ähnliche Weise zu kombinieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="cfde9-319">Modifizierer</span><span class="sxs-lookup"><span data-stu-id="cfde9-319">Modifiers</span></span>

<span data-ttu-id="cfde9-320">Wenn eine Deklaration einer partiellen Typ enthält eine Spezifikation für die Barrierefreiheit (die `public`, `protected`, `internal`, und `private` Modifizierer) sie müssen zustimmen, andere Komponenten, die eine Spezifikation für die Barrierefreiheit enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="cfde9-321">Wenn kein Teil eines partiellen Typs eine Spezifikation für die Barrierefreiheit enthält, wird der Typ der entsprechenden Standardzugriff angegeben ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="cfde9-322">Wenn ein oder mehr partielle Deklarationen eines geschachtelten Typs enthalten eine `new` Modifizierer keine Warnung wird ausgegeben, wenn der geschachtelte Typ einen geerbten Member Blendet ([ausblenden, die durch Vererbung von](basic-concepts.md#hiding-through-inheritance)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="cfde9-323">Wenn eine oder mehrere partielle Deklarationen, die von einer Klasse umfassen eine `abstract` Modifizierer die Klasse gilt als abstrakt ([abstrakte Klassen](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="cfde9-324">Andernfalls gilt die Klasse abstrakt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="cfde9-325">Wenn eine oder mehrere partielle Deklarationen, die von einer Klasse enthalten eine `sealed` Modifizierer die Klasse wird als versiegelt betrachtet ([versiegelte Klassen](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="cfde9-326">Andernfalls gilt die Klasse nicht versiegelten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="cfde9-327">Beachten Sie, dass eine Klasse nicht gleichzeitig abstrakt und versiegelt sein kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="cfde9-328">Wenn die `unsafe` Modifizierer ist für eine partielle Typdeklaration verwendet, nur diesen bestimmte Teil als unsicherer Kontext angesehen wird ([nicht sicheren Kontexten](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="cfde9-329">Typparameter und Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="cfde9-329">Type parameters and constraints</span></span>

<span data-ttu-id="cfde9-330">Wenn in mehreren Teilen ein generisches Typs deklariert wird, muss jeder Teil die Typparameter angeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="cfde9-331">Jeder Teil muss die gleiche Anzahl von Typparametern und den gleichen Namen für jeden Typparameter in der Reihenfolge haben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="cfde9-332">Wenn eine partielle generischen Typdeklaration enthält Einschränkungen (`where` Klauseln), die Einschränkungen müssen zustimmen, andere Komponenten, die Einschränkungen enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="cfde9-333">Insbesondere jeder Teil, der Einschränkungen enthält, müssen Einschränkungen für den gleichen Satz an Typparametern und für jeden Typparameter der Sätze von primären, sekundären und konstruktoreinschränkungen müssen äquivalent sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="cfde9-334">Zwei Sätze von Integritätsregeln sind äquivalent, wenn sie dieselben Elemente enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="cfde9-335">Wenn kein Teil eines partiellen Typs für die generische Einschränkungen für Typparameter angegeben wird, ist kein Hindernis für der Typ, Parameter berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="cfde9-336">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-336">The example</span></span>
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
<span data-ttu-id="cfde9-337">ist richtig, da die Teile, die Einschränkungen (die ersten beiden) effektiv enthalten die gleichen Satz von primären, sekundären und konstruktoreinschränkungen für den gleichen Satz an Typparametern angeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="cfde9-338">Basisklasse</span><span class="sxs-lookup"><span data-stu-id="cfde9-338">Base class</span></span>

<span data-ttu-id="cfde9-339">Wenn eine Deklaration der partiellen Klasse eine basisklassenspezifikation enthält müssen sie alle anderen Teile zustimmen, die eine basisklassenspezifikation enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="cfde9-340">Wenn kein Teil einer partiellen Klasse eine basisklassenspezifikation enthält, wird die Basisklasse `System.Object` ([Basisklassen](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="cfde9-341">Basisschnittstellen</span><span class="sxs-lookup"><span data-stu-id="cfde9-341">Base interfaces</span></span>

<span data-ttu-id="cfde9-342">Der Satz von Schnittstellen für einen Typ, der aus mehreren Teilen deklariert ist die Union der Basisschnittstellen für jede Komponente angegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="cfde9-343">Eine bestimmte Basis-Schnittstelle kann nur einmal für jede Komponente namens werden, aber es ist zulässig, für mehrere Teile, benennen Sie die gleichen basisschnitstelle.</span><span class="sxs-lookup"><span data-stu-id="cfde9-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="cfde9-344">Es muss nur eine Implementierung der Elemente einer angegebenen Basis-Schnittstelle vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="cfde9-345">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="cfde9-346">der Satz von Schnittstellen für die Klasse `C` ist `IA`, `IB`, und `IC`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="cfde9-347">In der Regel stellt jeder Teil eine Implementierung der Schnittstellen, die für diesen Teil deklariert; Dies ist jedoch keine Voraussetzung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="cfde9-348">Ein Teil kann die Implementierung einer Schnittstelle deklariert, die auf einem anderen Teil bereitstellen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-348">A part may provide the implementation for an interface declared on a different part:</span></span>
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a><span data-ttu-id="cfde9-349">Member</span><span class="sxs-lookup"><span data-stu-id="cfde9-349">Members</span></span>

<span data-ttu-id="cfde9-350">Mit Ausnahme von partiellen Methoden ([partielle Methoden](classes.md#partial-methods)), die Menge der Elemente eines Typs, die in mehreren Teilen deklariert ist, einfach die Vereinigung der Menge der Elemente in jedem Teil deklariert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="cfde9-351">Die Texte der alle Teile der Typdeklaration Freigabe im selben Deklarationsabschnitt ([Deklarationen](basic-concepts.md#declarations)), und der Bereich für jedes Element ([Bereiche](basic-concepts.md#scopes)) erstreckt sich auch auf die Texte der alle Teile.</span><span class="sxs-lookup"><span data-stu-id="cfde9-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="cfde9-352">Die Zugriffsdomäne eines Members enthält immer alle Teile des einschließenden Typs; eine `private` in einem Bereich deklarierten Member aus einem anderen Teil frei zugänglich ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="cfde9-353">Es ist ein Fehler während der Kompilierung, um dasselbe Element in mehr als ein Teil des Typs zu deklarieren, es sei denn, dieser Member ein Typ mit ist der `partial` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

<span data-ttu-id="cfde9-354">Die Reihenfolge der Elemente in einem Typ wird nur selten relevant, für die c#-Code, aber kann erheblich sein, wenn für die Kommunikation mit anderen Sprachen und Umgebungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="cfde9-355">In diesen Fällen ist es nicht definiert, die Reihenfolge der Member innerhalb eines Typs, die in mehreren Teilen deklariert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="cfde9-356">Partielle Methoden</span><span class="sxs-lookup"><span data-stu-id="cfde9-356">Partial methods</span></span>

<span data-ttu-id="cfde9-357">Partielle Methoden in einem Teil einer Typdeklaration definiert, und in einem anderen implementiert werden können.</span><span class="sxs-lookup"><span data-stu-id="cfde9-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="cfde9-358">Die Implementierung ist optional. Wenn kein Teil die partielle Methode implementiert, werden die Deklaration der partiellen Methode und alle Aufrufe aus die Typdeklaration, die durch die Kombination aus den Teilen entfernt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="cfde9-359">Partielle Methoden können keine Zugriffsmodifizierer definieren, jedoch handelt es sich implizit `private`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="cfde9-360">Ihren Rückgabetyp muss `void`, und ihre Parameter darf keine der `out` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="cfde9-361">Der Bezeichner `partial` wird als eine spezielle Schlüsselwort in einer Methodendeklaration erkannt, nur, wenn Sie anscheinend direkt vor der `void` geben; andernfalls kann es als ein normaler Bezeichner verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="cfde9-362">Eine partielle Methode kann keine Schnittstellenmethoden explizit implementieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="cfde9-363">Es gibt zwei Arten von Deklarationen der partiellen Methode ein: Wenn der Hauptteil der Deklaration der Methode ein Semikolon ist, die Deklaration wird als eine ***definierende Deklaration der partiellen Methode***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="cfde9-364">Wenn Text, als angegeben wird eine *Block*, die Deklaration wird als ein ***implementierende Deklaration der partiellen Methode***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="cfde9-365">Für die Teile einer Typdeklaration es kann nur eine definierende Deklaration der partiellen Methode mit einem angegebenen Signatur-, und es kann nur eine implementierende Deklaration der partiellen Methode mit einer bestimmten Signatur.</span><span class="sxs-lookup"><span data-stu-id="cfde9-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="cfde9-366">Wenn eine implementierende Deklaration der partiellen Methode angegeben, eine entsprechende definierende Deklaration der partiellen Methode vorhanden sein muss und müssen die Deklarationen als übereinstimmen, die in der folgenden angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="cfde9-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="cfde9-367">Die Deklarationen müssen die gleichen Modifizierer (obwohl nicht unbedingt in der gleichen Reihenfolge), Methodennamen, der Anzahl von Typparametern und der Anzahl von Parametern.</span><span class="sxs-lookup"><span data-stu-id="cfde9-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="cfde9-368">Entsprechende Parameter in den Deklarationen müssen die gleichen Modifizierer (obwohl nicht unbedingt in der gleichen Reihenfolge) und den gleichen Typ (modulo Unterschiede bei Typparameternamen).</span><span class="sxs-lookup"><span data-stu-id="cfde9-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="cfde9-369">Entsprechende Typparameter in den Deklarationen müssen dieselben Einschränkungen (modulo Unterschiede bei Typparameternamen).</span><span class="sxs-lookup"><span data-stu-id="cfde9-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="cfde9-370">Eine implementierende Deklaration der partiellen Methode kann in den gleichen Teil als entsprechende definierende Deklaration der partiellen Methode erscheinen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="cfde9-371">Nur eine definierende partielle Methode, die an der überladungsauflösung beteiligt ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="cfde9-372">Also, und zwar unabhängig davon, ob eine implementierende Deklaration angegeben ist, können auf Aufrufe der partiellen Methode Aufrufausdrücke beheben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="cfde9-373">Da eine partielle Methode immer zurückgibt `void`, solche Aufrufausdrücke werden sich immer auf die Tabellenausdruck-Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="cfde9-374">Darüber hinaus, da eine partielle Methode implizit wird `private`, solche Anweisungen treten immer innerhalb einer der Bestandteile der Typdeklaration, die in dem die partielle Methode deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="cfde9-375">Wenn kein Teil einer Deklaration der partiellen Typ eine implementierende Deklaration für eine bestimmte partielle Methode enthält, wird Ausdrucksanweisung Aufrufen aus der kombinierten Typdeklaration einfach entfernt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="cfde9-376">Daher ist der Aufrufausdruck, einschließlich der enthaltenen Ausdrücke, hat keine Auswirkungen zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="cfde9-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="cfde9-377">Die partielle Methode selbst wird wird ebenfalls entfernt und nicht Mitglied der kombinierten Typdeklaration.</span><span class="sxs-lookup"><span data-stu-id="cfde9-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="cfde9-378">Wenn die implementierende Deklaration für eine bestimmte partielle Methode vorhanden sind, werden die Aufrufe der partiellen Methoden beibehalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="cfde9-379">Die partielle Methode führt zu einer Methodendeklaration, die die implementierende Deklaration der partiellen Methode mit Ausnahme der folgenden ähnelt:</span><span class="sxs-lookup"><span data-stu-id="cfde9-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="cfde9-380">Die `partial` Modifizierer nicht enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="cfde9-381">Die Attribute in der resultierenden Methodendeklaration sind die kombinierten Attribute definieren und die implementierende Deklaration der partiellen Methode Erweiterungsvalidierungsmethoden ohne spezifische Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="cfde9-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="cfde9-382">Duplikate werden nicht entfernt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="cfde9-383">Die Attribute für die Parameter, der sich ergebenden Methodendeklaration sind der Kombination der Eigenschaften der entsprechenden Parameter definieren und die implementierende Deklaration der partiellen Methode in einer nicht vorgegebenen Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="cfde9-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="cfde9-384">Duplikate werden nicht entfernt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-384">Duplicates are not removed.</span></span>

<span data-ttu-id="cfde9-385">Wenn eine definierende Deklaration jedoch keine implementierende Deklaration für eine partielle Methode M gegeben wird, gelten die folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="cfde9-386">Es ist ein Fehler während der Kompilierung zum Erstellen eines Delegaten-Methode ([delegieren erstellen Ausdrücke](expressions.md#delegate-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="cfde9-387">Es ist ein Fehler während der Kompilierung zum Verweisen auf `M` innerhalb einer anonymen Funktion, die in einen Typ für die Ausdrucksbaumstruktur konvertiert wird ([Auswertung der anonymen Funktion-Konvertierungen in ausdrucksbaumstrukturtypen](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="cfde9-388">Ausdrücke, die auftreten, die als Teil eines Aufrufs der `M` wirken sich nicht auf den Zustand für definitive Zuweisungen ([definitive Zuweisung](variables.md#definite-assignment)), dies kann potenziell zu Kompilierungsfehlern führen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="cfde9-389">`M` der Einstiegspunkt für eine Anwendung nicht möglich ([Anwendungsstart](basic-concepts.md#application-startup)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="cfde9-390">Partielle Methoden sind hilfreich für das Zulassen von einem Teil einer Typdeklaration, um das Verhalten von einem anderen Teil, z. B. eine anzupassen, die von einem Tool generiert wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="cfde9-391">Beachten Sie die folgende Deklaration der partiellen Klasse ein:</span><span class="sxs-lookup"><span data-stu-id="cfde9-391">Consider the following partial class declaration:</span></span>
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

<span data-ttu-id="cfde9-392">Wenn diese Klasse, ohne auf andere Teile kompiliert wird, die definierende Deklarationen der partiellen Methode und ihre Aufrufe werden entfernt, und die resultierende kombinierten Klassendeklaration werden äquivalent zu folgendem:</span><span class="sxs-lookup"><span data-stu-id="cfde9-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

<span data-ttu-id="cfde9-393">Nehmen Sie an, dass ein anderer Teil, jedoch verfügt die implementierende Deklarationen der partiellen Methoden bereitstellt:</span><span class="sxs-lookup"><span data-stu-id="cfde9-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

<span data-ttu-id="cfde9-394">Anschließend werden die resultierenden kombinierten Klassendeklaration äquivalent zu folgendem:</span><span class="sxs-lookup"><span data-stu-id="cfde9-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a><span data-ttu-id="cfde9-395">-Bindungen</span><span class="sxs-lookup"><span data-stu-id="cfde9-395">Name binding</span></span>

<span data-ttu-id="cfde9-396">Obwohl jeder Teil eine erweiterbare Typ in den gleichen Namespace deklariert werden muss, werden die Teile in der Regel in den anderen Namespace-Deklarationen geschrieben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="cfde9-397">Daher verschiedene `using` Directives ([Using-Direktiven](namespaces.md#using-directives)) kann für die einzelnen Teile vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="cfde9-398">Beim Interpretieren von einfachen Namen ([Typrückschluss](expressions.md#type-inference)) in nur ein Teil der `using` gelten als Anweisungen von der dieses Teils einschließenden Namespace zurückzusenden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="cfde9-399">Dies kann dazu führen, dass den gleichen Bezeichner müssen verschiedene Bedeutungen in verschiedenen Teilen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-399">This may result in the same identifier having different meanings in different parts:</span></span>
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a><span data-ttu-id="cfde9-400">Klassenmember</span><span class="sxs-lookup"><span data-stu-id="cfde9-400">Class members</span></span>

<span data-ttu-id="cfde9-401">Die Member einer Klasse bestehen aus der Elemente eingeführt, durch die *Class_member_declaration*s und die Elemente, die von der direkten Basisklasse geerbt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

<span data-ttu-id="cfde9-402">Die Member eines Klassentyps sind in folgenden Kategorien unterteilt:</span><span class="sxs-lookup"><span data-stu-id="cfde9-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="cfde9-403">Konstanten, die Konstanten Werte, die der Klasse zugeordneten darstellen ([Konstanten](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="cfde9-404">Felder, die die Variablen der Klasse ([Felder](classes.md#fields)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="cfde9-405">Methoden, die Implementierung der Berechnungen und Aktionen, die von der Klasse ausgeführt werden können ([Methoden](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="cfde9-406">Eigenschaften, die definieren, mit dem Namen Merkmale und der Aktionen in Verbindung mit dem Lesen und Schreiben diese Eigenschaften ([Eigenschaften](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="cfde9-407">Ereignisse, die Benachrichtigungen zu definieren, die von der Klasse generiert werden kann ([Ereignisse](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="cfde9-408">Indexer, die zulassen von Instanzen der Klasse, die auf die gleiche Weise indiziert werden als Arrays (syntaktisch) ([Indexer](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="cfde9-409">Operatoren, die die Operatoren für Ausdrücke zu definieren, die auf Instanzen der Klasse angewendet werden können ([Operatoren](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="cfde9-410">Instanzkonstruktoren, die die erforderlichen Aktionen zum Initialisieren von Instanzen der Klasse zu implementieren ([Instanzkonstruktoren](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="cfde9-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="cfde9-411">Destruktoren, die die Aktionen, die ausgeführt werden, bevor Instanzen der Klasse dauerhaft verworfen werden implementieren ([Destruktoren](classes.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="cfde9-412">Statische Konstruktoren, die die erforderlichen Aktionen zum Initialisieren der Klasse selbst zu implementieren ([statische Konstruktoren](classes.md#static-constructors)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="cfde9-413">Typen, die die Typen darstellen, die lokal auf die Klasse sind ([geschachtelte Typen](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="cfde9-414">Elemente, die ausführbaren Code enthalten können, werden zusammen als bezeichnet die *Funktionsmember* des Klassentyps.</span><span class="sxs-lookup"><span data-stu-id="cfde9-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="cfde9-415">Die Funktionsmember eines Klassentyps sind die Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren, Instanzkonstruktoren, Destruktoren und statischen Konstruktoren des Klassentyps.</span><span class="sxs-lookup"><span data-stu-id="cfde9-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="cfde9-416">Ein *Class_declaration* erstellt einen neuen Deklarationsabschnitt ([Deklarationen](basic-concepts.md#declarations)), und die *Class_member_declaration*s sofort enthalten die *Klasse _declaration* neue Elemente in diesen Deklarationsabschnitt einführen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="cfde9-417">Die folgenden Regeln gelten für *Class_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cfde9-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="cfde9-418">Instanzkonstruktoren, Destruktoren und statische Konstruktoren müssen den gleichen Namen wie die Sie unmittelbar umschließende Klasse aufweisen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="cfde9-419">Alle anderen Elemente müssen Namen haben, die von den Namen der unmittelbar einschließenden Klasse abweichen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="cfde9-420">Der Name der eine Konstante, ein Feld, Eigenschaft, Ereignis oder Typ muss die Namen aller anderen Elemente, die in der gleichen Klasse deklariert unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="cfde9-421">Der Name einer Methode muss die Namen aller anderen nicht--Methoden, in der gleichen Klasse deklariert unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="cfde9-422">Außerdem wird die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) von eine Methode muss die Signaturen aller anderen in derselben Klasse deklarierten Methoden unterscheiden, und zwei Methoden, die in der gleichen Klasse deklariert dürfen keine Signaturen, die ausschließlich unterscheiden. durch `ref` und `out`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="cfde9-423">Die Signatur eines Instanzkonstruktors muss von den Signaturen aller anderen in derselben Klasse deklarierten Instanzkonstruktoren unterscheiden, und zwei Konstruktoren, die in der gleichen Klasse deklariert dürfen keine Signaturen, die ausschließlich vom unterscheiden `ref` und `out`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="cfde9-424">Die Signatur eines Indexers muss die Signaturen aller anderen in derselben Klasse deklarierten Indexer unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="cfde9-425">Die Signatur eines Operators muss die Signaturen aller anderen Operatoren, die in der gleichen Klasse deklariert unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="cfde9-426">Die geerbten Member eines Klassentyps ([Vererbung](classes.md#inheritance)) sind nicht Teil des Speicherplatzes Deklaration einer Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="cfde9-427">Daher kann eine abgeleitete Klasse ein Element mit demselben Namen bzw. derselben Signatur als einen geerbten Member zu deklarieren (d. Blendet die geerbten Member in Kraft).</span><span class="sxs-lookup"><span data-stu-id="cfde9-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="cfde9-428">Der Instanztyp</span><span class="sxs-lookup"><span data-stu-id="cfde9-428">The instance type</span></span>

<span data-ttu-id="cfde9-429">Jede Deklaration der Klasse verfügt über einen zugeordneten gebundenen Typ ([gebunden und Typen von nicht gebundenen](types.md#bound-and-unbound-types)), wird die ***Instanztyp***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="cfde9-430">Für die Deklaration einer generischen Klasse, wird der Instanztyp durch das Erstellen eines konstruierten Typs gebildet ([Typen konstruiert](types.md#constructed-types)) aus der Deklaration des Typs mit den angegebenen Typ Argumente werden den entsprechenden Typparameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="cfde9-431">Da der Instanztyp die Typparameter verwendet, können sie nur, in dem sich die Typparameter im Gültigkeitsbereich befinden verwendet werden. d. h. in der Klassendeklaration.</span><span class="sxs-lookup"><span data-stu-id="cfde9-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="cfde9-432">Der Instanztyp ist der Typ der `this` für Code in der Klassendeklaration geschrieben wurde.</span><span class="sxs-lookup"><span data-stu-id="cfde9-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="cfde9-433">Für nicht generische Klassen ist der Instanztyp einfach die deklarierte-Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="cfde9-434">Das folgende Beispiel zeigt mehrere Klassendeklarationen zusammen mit ihren Instanztypen aus:</span><span class="sxs-lookup"><span data-stu-id="cfde9-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="cfde9-435">Mitglieder der konstruierte Typen</span><span class="sxs-lookup"><span data-stu-id="cfde9-435">Members of constructed types</span></span>

<span data-ttu-id="cfde9-436">Die nicht geerbte Member eines konstruierten Typs erhalten Sie durch Ersetzen der für die einzelnen *Type_parameter* in der Element-Deklaration wird das entsprechende *Type_argument* des konstruierten Typs.</span><span class="sxs-lookup"><span data-stu-id="cfde9-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="cfde9-437">Die Ersetzung Prozess basiert auf die semantische Bedeutung der Deklarationen von Typen und ist nicht einfach Text ersetzen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="cfde9-438">Wenn z. B. die generische Klasse-Deklaration</span><span class="sxs-lookup"><span data-stu-id="cfde9-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="cfde9-439">Der konstruierte Typ `Gen<int[],IComparable<string>>` hat die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="cfde9-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="cfde9-440">Der Typ des Members `a` in der generischen Klassendeklaration `Gen` ist "zweidimensionales Array von `T`", sodass der Typ des Members `a` im oben erstellten Typ ist "zweidimensionales Array von eindimensionales Array von `int`", oder `int[,][]`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="cfde9-441">Innerhalb der Funktion Instanzmember, der Typ des `this` ist der Instanztyp ([den Instanztyp](classes.md#the-instance-type)) der Deklaration enthält.</span><span class="sxs-lookup"><span data-stu-id="cfde9-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="cfde9-442">Alle Member einer generischen Klasse können Typparameter von einer einschließenden Klasse, entweder direkt oder als Teil eines konstruierten Typs verwenden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="cfde9-443">Beim Schließen von einer bestimmtes konstruierten Typ ([offene und geschlossene Typen](types.md#open-and-closed-types)) wird verwendet, zur Laufzeit, wird jede Verwendung von einem Typparameter durch die tatsächliche, für den konstruierten Typ angegebenes Typargument ersetzt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="cfde9-444">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cfde9-444">For example:</span></span>
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a><span data-ttu-id="cfde9-445">Vererbung</span><span class="sxs-lookup"><span data-stu-id="cfde9-445">Inheritance</span></span>

<span data-ttu-id="cfde9-446">Eine Klasse ***erbt*** die Member dieses Typs direkte Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="cfde9-447">Vererbung bedeutet, dass eine Klasse implizit alle Member seines Typs direkte Basisklasse mit Ausnahme der Instanzkonstruktoren, Destruktoren und statische Konstruktoren der Basisklasse enthält.</span><span class="sxs-lookup"><span data-stu-id="cfde9-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="cfde9-448">Einige wichtige Aspekte bei der Vererbung sind:</span><span class="sxs-lookup"><span data-stu-id="cfde9-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="cfde9-449">Vererbung ist transitiv.</span><span class="sxs-lookup"><span data-stu-id="cfde9-449">Inheritance is transitive.</span></span> <span data-ttu-id="cfde9-450">Wenn `C` ergibt sich aus `B`, und `B` ergibt sich aus `A`, klicken Sie dann `C` im deklarierten Member erbt `B` sowie im deklarierten Member `A`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="cfde9-451">Eine abgeleitete Klasse erweitert seine direkte Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="cfde9-452">Eine abgeleitete Klasse kann den geerbten Membern neue Member hinzufügen, aber die Definition eines geerbten Members kann nicht entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="cfde9-453">Instanzkonstruktoren, Destruktoren und statischen Konstruktoren nicht geerbt werden, aber alle anderen Elemente sind, unabhängig von deren deklarierte Zugriffsart ([Memberzugriff](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="cfde9-454">Allerdings können je nach ihrem deklarierten Zugriff geerbte Member in einer abgeleiteten Klasse nicht zugreifen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="cfde9-455">Kann eine abgeleitete Klasse ***ausblenden*** ([ausblenden, die durch Vererbung von](basic-concepts.md#hiding-through-inheritance)) geerbte Member durch neue Member mit demselben Namen bzw. derselben Signatur deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="cfde9-456">Beachten Sie jedoch, die durch das Ausblenden eines geerbten Members dieses Element nicht entfernt wird, lediglich erleichtert diesen Member nicht direkt über der abgeleiteten Klasse zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="cfde9-457">Eine Instanz einer Klasse enthält einen Satz von alle Instanzenfelder in der Klasse und ihre Basisklassen und eine implizite Konvertierung deklariert ([implizite Verweis-](conversions.md#implicit-reference-conversions)) von einem abgeleiteten Typ vorhanden ist, um eine der zugehörigen Basisklassentyp konvertiert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="cfde9-458">Daher kann ein Verweis auf eine Instanz einer abgeleiteten Klasse als Verweis auf eine Instanz einer seiner Basisklassen behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="cfde9-459">Eine Klasse kann virtuelle Methoden, Eigenschaften und Indexer deklarieren, und abgeleitete Klassen können die Implementierung dieser Funktion Member überschreiben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="cfde9-460">Dies ermöglicht es sich um Klassen, weisen polymorphes Verhalten, bei dem die Aktionen, die durch einen Aufruf der Funktion Mitglied variiert je nach den Laufzeittyp der Instanz über den diese Funktionsmember der Member aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="cfde9-461">Der geerbte Member eines Typs konstruierten Klasse sind die Member des Typs direkten Basisklasse ([Basisklassen](classes.md#base-classes)), die wird ermittelt, indem Sie die Typargumente für jedes Vorkommen des entsprechenden Typs des konstruierten Typs ersetzen Parameter in der *Class_base* Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="cfde9-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="cfde9-462">Diese Elemente werden wiederum durch Ersetzen der für die einzelnen transformiert *Type_parameter* in der Element-Deklaration wird das entsprechende *Type_argument* von der *Class_base* Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="cfde9-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

<span data-ttu-id="cfde9-463">Im obigen Beispiel den konstruierten Typ `D<int>` verfügt über einen nicht geerbte Member `public int G(string s)` abgerufen, indem Sie das Typargument ersetzt `int` für den Typparameter `T`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="cfde9-464">`D<int>` verfügt auch über einen geerbten Member aus der Klassendeklaration `B`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="cfde9-465">Diese geerbte Member wird bestimmt, indem er zuerst ermittelt den Basisklassentyp `B<int[]>` von `D<int>` ersetzen `int` für `T` in der Basisklasse-Spezifikation `B<T[]>`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="cfde9-466">Klicken Sie dann als Typargument für `B`, `int[]` durch ersetzt, `U` in `public U F(long index)`, stellt des geerbten Members `public int[] F(long index)`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="cfde9-467">Das new-Modifizierer</span><span class="sxs-lookup"><span data-stu-id="cfde9-467">The new modifier</span></span>

<span data-ttu-id="cfde9-468">Ein *Class_member_declaration* ist zulässig, einen Member mit demselben Namen bzw. derselben Signatur als einen geerbten Member deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="cfde9-469">In diesem Fall Members der abgeleiteten Klasse gilt als ***ausblenden*** Member der Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="cfde9-470">Durch das Ausblenden eines geerbten Members ist kein Fehler betrachtet, aber es führt dazu, dass des Compilers eine Warnung ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="cfde9-471">Um die Warnung zu unterdrücken, zählen die Deklaration des Members der abgeleiteten Klasse eine `new` Modifizierer, um anzugeben, dass der abgeleitete Member die Member den Basisklassen ausblenden vorgesehen ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="cfde9-472">In diesem Thema wird erläutert. im weiteren [ausblenden, die durch Vererbung von](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="cfde9-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="cfde9-473">Wenn eine `new` Modifizierer in einer Deklaration, der einen geerbten Member auszublenden, nicht enthalten ist, eine Warnung zu diesem Zweck wird ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="cfde9-474">Diese Warnung unterdrückt wird, durch das Entfernen der `new` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="cfde9-475">Zugriffsmodifizierer</span><span class="sxs-lookup"><span data-stu-id="cfde9-475">Access modifiers</span></span>

<span data-ttu-id="cfde9-476">Ein *Class_member_declaration* haben eines der fünf Arten von deklarierte Zugriffsart ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` , oder `private`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="cfde9-477">Mit Ausnahme der `protected internal` Kombination, es ist ein Fehler während der Kompilierung mehr als eine Zugriffsmodifizierer angeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="cfde9-478">Wenn eine *Class_member_declaration* enthält keinen Zugriffsmodifizierer, `private` wird angenommen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="cfde9-479">Enthaltenen Typen</span><span class="sxs-lookup"><span data-stu-id="cfde9-479">Constituent types</span></span>

<span data-ttu-id="cfde9-480">Typen, die in der Deklaration eines Elements verwendet werden, sind die einzelnen Typen von diesen Member aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="cfde9-481">Mögliche enthaltenen Typen werden der Typ, der eine Konstante, Feld, Eigenschaft, Ereignis oder des Indexers, der Rückgabetyp einer Methode oder einen Operator und die Parametertypen einer Methode, Indexer, Operator oder Instanzenkonstruktor.</span><span class="sxs-lookup"><span data-stu-id="cfde9-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="cfde9-482">Die einzelnen Typen eines Elements müssen mindestens dieselben zugriffsmöglichkeiten bieten wie dieser Member selbst sein ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="cfde9-483">Statische und Mitglieder</span><span class="sxs-lookup"><span data-stu-id="cfde9-483">Static and instance members</span></span>

<span data-ttu-id="cfde9-484">Member einer Klasse sind entweder ***statische Member*** oder ***Instanzmember***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="cfde9-485">Im Allgemeinen ist es nützlich, um statische Member als Klassentypen und Instanzmember als von Objekten (Instanzen von Klassentypen) gehören vorstellen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="cfde9-486">Wenn eine Deklaration Feld, Methode, Eigenschaft, Ereignis, Operator oder Konstruktor enthält einen `static` Modifizierer verwenden, wird einen statischen Member deklariert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="cfde9-487">Darüber hinaus deklariert die Deklaration einer Konstante oder der Typ implizit einen statischen Member.</span><span class="sxs-lookup"><span data-stu-id="cfde9-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="cfde9-488">Statische Member weisen folgende Merkmale auf:</span><span class="sxs-lookup"><span data-stu-id="cfde9-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="cfde9-489">Wenn ein statischer Member `M` verwiesen wird, einem *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `E.M`, `E` müssen kennzeichnen ein Typ mit `M`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="cfde9-490">Es ist ein Fehler während der Kompilierung für `E` um eine Instanz zu kennzeichnen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="cfde9-491">Ein statisches Feld identifiziert genau einen Speicherort für alle Instanzen des angegebenen Klassentyps geschlossene freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="cfde9-492">Unabhängig davon, wie viele Instanzen eines bestimmten geschlossene Klassentyps erstellt werden müssen Sie immer nur eine Kopie eines statischen Felds vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="cfde9-493">Ein statischer Funktionsmember (Methode, Eigenschaft, Ereignis, Operator oder Konstruktor) kann nicht in einer bestimmten Instanz ausgeführt werden, und es ist ein Fehler während der Kompilierung zum Verweisen auf `this` in diese ein Funktionsmember.</span><span class="sxs-lookup"><span data-stu-id="cfde9-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="cfde9-494">Wenn eine Feld, Methode, Eigenschaft, Ereignis, Indexer, Konstruktor oder Destruktor Deklaration enthält keine `static` Modifizierer verwenden, wird einen Instanzmember deklariert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="cfde9-495">(Ein Instanzmember ist einen nicht statisches Member bezeichnet.) Instanzmember weisen folgende Merkmale auf:</span><span class="sxs-lookup"><span data-stu-id="cfde9-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="cfde9-496">Wenn ein Instanzmember `M` verwiesen wird, eine *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `E.M`, `E` muss eine Instanz eines Typs mit deuten`M`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="cfde9-497">Es ist ein Fehler während der Bindung für `E` um einen Typ anzugeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="cfde9-498">Jede Instanz einer Klasse enthält einen separaten Satz aller Instanzfelder der Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="cfde9-499">Ein Instanzmember-Funktion (Methode, Eigenschaft, Indexer, Instance-Konstruktor oder Destruktor), die für eine bestimmte Instanz der Klasse ausgeführt wird, und diese Instanz zugegriffen werden kann, als `this` ([diesen Zugriff](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="cfde9-500">Das folgende Beispiel veranschaulicht die Regeln für den Zugriff auf statische und Instanzmember:</span><span class="sxs-lookup"><span data-stu-id="cfde9-500">The following example illustrates the rules for accessing static and instance members:</span></span>
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

<span data-ttu-id="cfde9-501">Die `F` Methode zeigt, dass in der Funktion einen Instanzmember einer *Simple_name* ([einfache Namen](expressions.md#simple-names)) Zugriff auf Instanzmember und statische Member verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="cfde9-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="cfde9-502">Die `G` Methode zeigt, in einem statischen Funktionsmember, ein Fehler während der Kompilierung einen Instanzmember über den Zugriff auf eine *Simple_name*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="cfde9-503">Die `Main` wird verdeutlicht, dass in einem *Member_access* ([Memberzugriff](expressions.md#member-access)) Instanzmember über Instanzen zugegriffen werden müssen und statische Member über Typen zugegriffen werden müssen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="cfde9-504">Geschachtelte Typen</span><span class="sxs-lookup"><span data-stu-id="cfde9-504">Nested types</span></span>

<span data-ttu-id="cfde9-505">Ein Typ, in die Deklaration einer Klasse oder Struktur deklariert wird aufgerufen, eine ***geschachtelter Typ***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="cfde9-506">Ein Typ, der innerhalb einer Kompilierungseinheit oder dem Namespace deklariert wird aufgerufen, wird eine ***nicht geschachtelten Typ***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="cfde9-507">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-507">In the example</span></span>
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
<span data-ttu-id="cfde9-508">Klasse `B` ein geschachtelter Typ ist, da es in der Klasse deklariert ist `A`, und die Klasse `A` ein nicht geschachtelter Typ ist, da er innerhalb einer Kompilierungseinheit deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="cfde9-509">Vollqualifizierte Namen</span><span class="sxs-lookup"><span data-stu-id="cfde9-509">Fully qualified name</span></span>

<span data-ttu-id="cfde9-510">Der vollqualifizierte Name ([vollständig qualifizierten Namen](basic-concepts.md#fully-qualified-names)) für ein geschachtelter Typ ist `S.N` , in denen `S` ist der vollqualifizierte Name des Typs in der `N` deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="cfde9-511">Deklarierter Zugriff</span><span class="sxs-lookup"><span data-stu-id="cfde9-511">Declared accessibility</span></span>

<span data-ttu-id="cfde9-512">Nicht geschachtelten Typen haben `public` oder `internal` deklariert Barrierefreiheit und `internal` Barrierefreiheit werden standardmäßig deklariert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="cfde9-513">Geschachtelte Typen haben diese Formen des deklarierte Zugriffsart, sowie eine oder mehrere weitere Formen der deklarierte Zugriffsart, je nachdem, ob der enthaltende Typ eine Klasse oder Struktur:</span><span class="sxs-lookup"><span data-stu-id="cfde9-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="cfde9-514">Ein geschachtelter Typ, der in einer Klasse deklariert ist, kann jeden der fünf Arten von deklarierte Zugriffsart aufweisen (`public`, `protected internal`, `protected`, `internal`, oder `private`), und wie andere Klassenmember standardmäßig `private` deklariert Barrierefreiheit.</span><span class="sxs-lookup"><span data-stu-id="cfde9-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="cfde9-515">Ein geschachtelter Typ, der in einer Struktur deklariert ist, kann jeden der deklarierte Zugriffsart in drei Formen aufweisen (`public`, `internal`, oder `private`), und wie andere Strukturmember standardmäßig `private` Barrierefreiheit deklariert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="cfde9-516">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-516">The example</span></span>
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
<span data-ttu-id="cfde9-517">deklariert eine private geschachtelte Klasse `Node`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="cfde9-518">Durch das Ausblenden</span><span class="sxs-lookup"><span data-stu-id="cfde9-518">Hiding</span></span>

<span data-ttu-id="cfde9-519">Ein geschachtelter Typ kann ausblenden ([Ausblenden von Namen](basic-concepts.md#name-hiding)) ein Member der Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="cfde9-520">Die `new` Modifizierer ist in Deklarationen von geschachtelten Typen zulässig, sodass ausblenden explizit ausgedrückt werden kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="cfde9-521">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-521">The example</span></span>
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
<span data-ttu-id="cfde9-522">Zeigt eine geschachtelte Klasse `M` , die die Methode blendet `M` in definierten `Base`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="cfde9-523">Dieser Zugriff</span><span class="sxs-lookup"><span data-stu-id="cfde9-523">this access</span></span>

<span data-ttu-id="cfde9-524">Ein geschachtelter Typ und seinem enthaltenden Typ haben eine besondere Beziehung in Bezug auf nicht *This_access* ([diesen Zugriff](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="cfde9-525">Insbesondere `this` innerhalb eines geschachtelten Typs kann nicht verwendet werden, um auf Instanzmember des enthaltenden Typs verweisen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="cfde9-526">In Fällen, in denen ein geschachtelter Typ Zugriff auf die Instanzmember des enthaltenden Typs benötigt, Zugriff bereitgestellt werden kann, durch die Bereitstellung der `this` für die Instanz des enthaltenden Typs als Konstruktorargument für den geschachtelten Typ.</span><span class="sxs-lookup"><span data-stu-id="cfde9-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="cfde9-527">Im folgenden Beispiel wird</span><span class="sxs-lookup"><span data-stu-id="cfde9-527">The following example</span></span>
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
<span data-ttu-id="cfde9-528">Dieses Verfahren veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="cfde9-528">shows this technique.</span></span> <span data-ttu-id="cfde9-529">Eine Instanz von `C` erstellt eine Instanz des `Nested` und übergibt Sie eine eigene `this` zu `Nested`Konstruktor, um den Zugriff auf bereitzustellen `C`des Instanzmember.</span><span class="sxs-lookup"><span data-stu-id="cfde9-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="cfde9-530">Zugriff auf private und geschützte Member des enthaltenden Typs</span><span class="sxs-lookup"><span data-stu-id="cfde9-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="cfde9-531">Ein geschachtelter Typ hat Zugriff auf alle Elemente, die auf seinem enthaltenden Typ, einschließlich, die Mitglieder des enthaltenden Typs zugegriffen werden `private` und `protected` Barrierefreiheit deklariert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="cfde9-532">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-532">The example</span></span>
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
<span data-ttu-id="cfde9-533">Zeigt eine Klasse `C` , enthält eine geschachtelte Klasse `Nested`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="cfde9-534">In `Nested`, die Methode `G` Ruft die statische Methode `F` in definierten `C`, und `F` wurde deklariert privaten Zugriff.</span><span class="sxs-lookup"><span data-stu-id="cfde9-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="cfde9-535">Ein geschachtelter Typ kann auch geschützte Member, die in einem Basistyp des enthaltenden Typs definiert zugreifen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="cfde9-536">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-536">In the example</span></span>
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
<span data-ttu-id="cfde9-537">die geschachtelte Klasse `Derived.Nested` greift auf die geschützte Methode `F` in definierten `Derived`Basisklasse, `Base`, von Aufrufen durch eine Instanz der `Derived`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="cfde9-538">Geschachtelte Typen in generische Klassen</span><span class="sxs-lookup"><span data-stu-id="cfde9-538">Nested types in generic classes</span></span>

<span data-ttu-id="cfde9-539">Deklaration einer generischen Klasse kann über geschachtelte Typdeklarationen enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="cfde9-540">Die Typparameter der einschließenden Klasse können innerhalb der geschachtelten Typen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="cfde9-541">Eine geschachtelten Typdeklaration kann weitere Typparameter enthalten, die nur für den geschachtelten Typ gelten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="cfde9-542">Jede Typdeklaration, die innerhalb der Deklaration einer generischen Klasse ist implizit die Deklaration eines generischen Typs.</span><span class="sxs-lookup"><span data-stu-id="cfde9-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="cfde9-543">Schreiben einen Verweis auf einen Typ in einem generischen Typ geschachtelt werden, muss mit konstruierten Typs, einschließlich seiner Typargumente benannt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="cfde9-544">Allerdings kann von innerhalb der äußeren Klasse der geschachtelte Typ ohne Qualifikation verwendet werden; der Instanztyp der äußeren Klasse kann implizit verwendet werden, beim Erstellen des geschachtelten Typs.</span><span class="sxs-lookup"><span data-stu-id="cfde9-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="cfde9-545">Das folgende Beispiel zeigt drei richtige Möglichkeiten zum Verweisen auf einen konstruierten Typ aus erstellt `Inner`; die ersten beiden sind gleichwertig:</span><span class="sxs-lookup"><span data-stu-id="cfde9-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

<span data-ttu-id="cfde9-546">Obwohl das Gegenteil kann Mitglied Programmierstil, ein Typparameter in einem geschachtelten Typ oder ausblenden Typparameter des äußeren Typs deklariert:</span><span class="sxs-lookup"><span data-stu-id="cfde9-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="cfde9-547">Reservierte Namen</span><span class="sxs-lookup"><span data-stu-id="cfde9-547">Reserved member names</span></span>

<span data-ttu-id="cfde9-548">Um die zugrunde liegenden C#-Laufzeit-Implementierung für jede Quelle-Memberdeklaration zu ermöglichen, die eine Eigenschaft, Ereignis oder Indexer muss die Implementierung zwei Methodensignaturen, die je nach der Art der Memberdeklaration, den Namen und den Typ reservieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="cfde9-549">Es ist ein Fehler während der Kompilierung für ein Programm, auch wenn die zugrunde liegende Implementierung für die Laufzeit keine vornimmt deklariert ein Element, dessen Signatur, eines der folgenden übereinstimmt, reservierten Signaturen, verwenden Sie diese Reservierungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="cfde9-550">Die reservierten Namen keine Deklarationen einführen, daher beim Nachschlagen von Membern nicht beteiligt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="cfde9-551">Eine Deklaration zugeordneten jedoch reservierte Methode Signaturen bei der Vererbung teilnehmen ([Vererbung](classes.md#inheritance)), und können ausgeblendet werden, mit der `new` Modifizierer ([der new-Modifizierer](classes.md#the-new-modifier)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="cfde9-552">Die Reservierung dieser Namen erfüllt drei Zwecke:</span><span class="sxs-lookup"><span data-stu-id="cfde9-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="cfde9-553">Um die zugrunde liegende Implementierung einen normalen Bezeichner als Methodenname für Get verwenden, oder legen Sie den Zugriff auf die C#-Sprachfunktion zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="cfde9-554">Um andere Sprachen zusammenarbeiten, verwenden einen normalen Bezeichner als Methodenname für Get oder legen Sie den Zugriff auf die C#-Sprachfunktion zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="cfde9-555">Um sicherzustellen, dass die Quelle von einem konformen Compiler akzeptiert werden durch einen anderen, akzeptiert, dazu die Einzelheiten der reservierten Member Namen konsistent über alle C#-Implementierungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="cfde9-556">Die Deklaration eines Destruktors ([Destruktoren](classes.md#destructors)) bewirkt auch, dass eine Signatur reserviert werden sollen ([für Destruktoren Membernamen](classes.md#member-names-reserved-for-destructors)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="cfde9-557">Elementnamen, die für Eigenschaften reserviert</span><span class="sxs-lookup"><span data-stu-id="cfde9-557">Member names reserved for properties</span></span>

<span data-ttu-id="cfde9-558">Für eine Eigenschaft `P` ([Eigenschaften](classes.md#properties)) vom Typ `T`, die folgenden Signaturen sind reserviert:</span><span class="sxs-lookup"><span data-stu-id="cfde9-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="cfde9-559">Beide Signaturen sind reserviert, auch wenn die Eigenschaft schreibgeschützt oder lesegeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="cfde9-560">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-560">In the example</span></span>
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
<span data-ttu-id="cfde9-561">eine Klasse `A` definiert eine schreibgeschützte Eigenschaft `P`, also Reservieren von Signaturen für `get_P` und `set_P` Methoden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="cfde9-562">Eine Klasse `B` leitet sich von `A` und blendet Sie aus dieser reservierte Signaturen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="cfde9-563">Das Beispiel erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="cfde9-563">The example produces the output:</span></span>
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="cfde9-564">Elementnamen, die für Ereignisse reserviert</span><span class="sxs-lookup"><span data-stu-id="cfde9-564">Member names reserved for events</span></span>

<span data-ttu-id="cfde9-565">Für ein Ereignis `E` ([Ereignisse](classes.md#events)) eines Delegattyps `T`, die folgenden Signaturen sind reserviert:</span><span class="sxs-lookup"><span data-stu-id="cfde9-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="cfde9-566">Für Indexer reservierte Elementnamen</span><span class="sxs-lookup"><span data-stu-id="cfde9-566">Member names reserved for indexers</span></span>

<span data-ttu-id="cfde9-567">Für einen Indexer ([Indexer](classes.md#indexers)) vom Typ `T` mit Parameter-List `L`, die folgenden Signaturen sind reserviert:</span><span class="sxs-lookup"><span data-stu-id="cfde9-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="cfde9-568">Beide Signaturen sind reserviert, selbst wenn der Indexer schreibgeschützt oder lesegeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="cfde9-569">Außerdem den Namen des Members `Item` ist reserviert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="cfde9-570">Elementnamen, die für Destruktoren reserviert</span><span class="sxs-lookup"><span data-stu-id="cfde9-570">Member names reserved for destructors</span></span>

<span data-ttu-id="cfde9-571">Für eine Klasse, die einen Destruktor enthält ([Destruktoren](classes.md#destructors)), die folgende Signatur ist reserviert:</span><span class="sxs-lookup"><span data-stu-id="cfde9-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="cfde9-572">Konstanten</span><span class="sxs-lookup"><span data-stu-id="cfde9-572">Constants</span></span>

<span data-ttu-id="cfde9-573">Ein ***Konstanten*** ist ein Klassenmember, der einen konstanten Wert darstellt: ein Wert, der zur Kompilierungszeit berechnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="cfde9-574">Ein *Constant_declaration* führt eine oder mehrere Konstanten eines bestimmten Typs.</span><span class="sxs-lookup"><span data-stu-id="cfde9-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="cfde9-575">Ein *Constant_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)), ein `new` Modifizierer ([der new-Modifizierer](classes.md#the-new-modifier)), und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="cfde9-576">Die Attribute und Modifizierer, gelten für alle Member deklariert, indem die *Constant_declaration*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="cfde9-577">Obwohl Konstanten statische Member, gelten eine *Constant_declaration* erfordert und keine ermöglicht eine `static` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="cfde9-578">Es ist ein Fehler für den gleichen Modifizierer für mehrere Male in einer Konstantendeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="cfde9-579">Die *Typ* von einem *Constant_declaration* gibt den Typ der Elemente, die durch die Deklaration eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="cfde9-580">Der Typ folgt eine Liste der *Constant_declarator*s, von denen jede einen neuen Member führt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="cfde9-581">Ein *Constant_declarator* besteht aus einer *Bezeichner* mit dem das Element, gefolgt vom Namen einer "`=`" token, gefolgt von einer *Constant_expression* ([ Konstante Ausdrücke](expressions.md#constant-expressions)), die den Wert des Elements bietet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="cfde9-582">Die *Typ* in eine Konstante angegeben Deklaration muss `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, *Enum_type*, oder ein *Reference_type*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="cfde9-583">Jede *Constant_expression* muss einen Wert des Zieltyps oder eines Typs, der in den Zieltyp konvertiert werden kann, indem eine implizite Konvertierung werden liefern ([implizite Konvertierungen](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="cfde9-584">Die *Typ* einer Konstante muss mindestens dieselben zugriffsmöglichkeiten bieten wie die Konstante selbst ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cfde9-585">Der Wert einer Konstante wird ermittelt, in ein Ausdruck mit einem *Simple_name* ([einfache Namen](expressions.md#simple-names)) oder ein *Member_access* ([Memberzugriff](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="cfde9-586">Eine Konstante selbst teilnehmen kann eine *Constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="cfde9-587">Daher kann eine Konstante verwendet werden, in einem beliebigen Konstrukt, das erfordert eine *Constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="cfde9-588">Beispiele für solche Konstrukte `case` Bezeichnungen `goto case` Anweisungen `enum` Memberdeklarationen, Attribute und andere Konstanten Deklarationen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="cfde9-589">Siehe [Konstante Ausdrücke](expressions.md#constant-expressions), *Constant_expression* ist ein Ausdruck, der zum Zeitpunkt der Kompilierung vollständig ausgewertet werden kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="cfde9-590">Da die einzige Möglichkeit zum Erstellen eines nicht-Null-Wert, der eine *Reference_type* außer `string` angewendet werden soll die `new` -Operator, und da die `new` Operator ist nicht zulässig, eine *Constant_ Ausdruck*, der einzig mögliche Wert für Konstanten von *Reference_type*s außer `string` ist `null`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="cfde9-591">Wenn ein symbolische Namen für einen konstanten Wert gewünscht ist, aber wenn der Typ des Werts in einer Konstantendeklaration nicht zulässig ist oder wenn der Wert nicht zum Zeitpunkt der Kompilierung von berechnet werden kann eine *Constant_expression*, `readonly` (Feld [Schreibgeschützte Felder](classes.md#readonly-fields)) kann stattdessen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="cfde9-592">Deklaration eine Konstante, die mehrere Konstanten deklariert entspricht mehreren Deklarationen der einzelnen Konstanten, mit der gleichen Attribute, Modifizierer, und geben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="cfde9-593">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cfde9-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="cfde9-594">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="cfde9-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="cfde9-595">Konstanten sind andere Konstanten innerhalb desselben Programms abhängig ist, solange die Abhängigkeiten keine zirkuläre Natur sind zulässig.</span><span class="sxs-lookup"><span data-stu-id="cfde9-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="cfde9-596">Der Compiler ordnet automatisch die Konstanten Deklarationen in der richtigen Reihenfolge ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="cfde9-597">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-597">In the example</span></span>
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
<span data-ttu-id="cfde9-598">der Compiler wertet zunächst `A.Y`, wertet dann `B.Z`, und schließlich wertet `A.X`, die Werte erzeugen `10`, `11`, und `12`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="cfde9-599">Konstante Deklarationen können auf Konstanten aus anderen Programmen abhängig sind, aber solche Abhängigkeiten sind nur in einer Richtung möglich.</span><span class="sxs-lookup"><span data-stu-id="cfde9-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="cfde9-600">Im obigen Beispiel auf, wenn `A` und `B` deklariert wurden in verschiedenen Programmen, es wäre möglich, dass `A.X` hängt von `B.Z`, aber `B.Z` können nicht gleichzeitig klicken Sie dann abhängig `A.Y`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="cfde9-601">Felder</span><span class="sxs-lookup"><span data-stu-id="cfde9-601">Fields</span></span>

<span data-ttu-id="cfde9-602">Ein ***Feld*** ist ein Member, der eine Variable zugeordnet, ein Objekt oder eine Klasse darstellt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="cfde9-603">Ein *Field_declaration* führt eine oder mehrere Felder eines angegebenen Typs.</span><span class="sxs-lookup"><span data-stu-id="cfde9-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="cfde9-604">Ein *Field_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)), ein `new` Modifizierer ([der new-Modifizierer](classes.md#the-new-modifier)), gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)), und ein `static` Modifizierer ([statische Felder und Instanzfelder](classes.md#static-and-instance-fields)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="cfde9-605">Darüber hinaus eine *Field_declaration* eventuell eine `readonly` Modifizierer ([schreibgeschützte Felder](classes.md#readonly-fields)) oder ein `volatile` Modifizierer ([flüchtige Felder](classes.md#volatile-fields)) jedoch nicht beides.</span><span class="sxs-lookup"><span data-stu-id="cfde9-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="cfde9-606">Die Attribute und Modifizierer, gelten für alle Member deklariert, indem die *Field_declaration*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="cfde9-607">Es ist ein Fehler für den gleichen Modifizierer für mehrere Male in einer Felddeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="cfde9-608">Die *Typ* von einem *Field_declaration* gibt den Typ der Elemente, die durch die Deklaration eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="cfde9-609">Der Typ folgt eine Liste der *Variable_declarator*s, von denen jede einen neuen Member führt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="cfde9-610">Ein *Variable_declarator* besteht aus einer *Bezeichner* mit dem Namen dieses Members, optional gefolgt von einer "`=`" token und einem *Variable_initializer* ([ Variableninitialisierern](classes.md#variable-initializers)), die ermöglicht, dass des Anfangswert dieses Members.</span><span class="sxs-lookup"><span data-stu-id="cfde9-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="cfde9-611">Die *Typ* eines Felds muss mindestens dieselben zugriffsmöglichkeiten bieten wie das Feld selbst ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cfde9-612">Der Wert eines Felds wird ermittelt, in einem Ausdruck mit einer *Simple_name* ([einfache Namen](expressions.md#simple-names)) oder ein *Member_access* ([Memberzugriff](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="cfde9-613">Der Wert eines Felds nicht schreibgeschützte geändert wird, mithilfe einer *Zuweisung* ([Zuweisungsoperatoren](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="cfde9-614">Der Wert eines Felds nicht schreibgeschützte kann sein, abgerufen und das Bearbeiten mit postfix-Inkrement und Dekrement-Operatoren ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators)) und Präfix-Inkrement und Dekrement-Operatoren ([Präfix Inkrement-und Dekrementoperatoren](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="cfde9-615">Eine Felddeklaration, die mehrere Felder deklariert entspricht mehreren Deklarationen der einzelnen Felder mit der gleichen Attribute, Modifizierer, und geben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="cfde9-616">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cfde9-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="cfde9-617">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="cfde9-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="cfde9-618">Statische und Felder</span><span class="sxs-lookup"><span data-stu-id="cfde9-618">Static and instance fields</span></span>

<span data-ttu-id="cfde9-619">Wenn eine Felddeklaration enthält eine `static` Modifizierer, die Felder, die durch die Deklaration eingeführt sind ***statische Felder***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="cfde9-620">Wenn kein `static` Modifizierer vorhanden ist, wird die Felder, die durch die Deklaration eingeführt sind ***Instanzfelder***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="cfde9-621">Statische Felder und Instanzfelder sind zwei verschiedene Arten von Variablen ([Variablen](variables.md)) von c# unterstützt, und gelegentlich werden sie bezeichnet als ***statische Variablen*** und ***Instanzvariablen*** bzw.</span><span class="sxs-lookup"><span data-stu-id="cfde9-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="cfde9-622">Ein statisches Feld gehört nicht zu einer bestimmten Instanz. Stattdessen es für alle Instanzen eines Typs geschlossene genutzt ([offene und geschlossene Typen](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="cfde9-623">Unabhängig davon, wie viele Instanzen eines geschlossenen Klassentyps erstellt werden ist es immer nur eine Kopie eines statischen Felds für die Domäne der zugehörigen Anwendung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="cfde9-624">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cfde9-624">For example:</span></span>
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

<span data-ttu-id="cfde9-625">Ein Instanzenfeld gehört zu einer Instanz.</span><span class="sxs-lookup"><span data-stu-id="cfde9-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="cfde9-626">Insbesondere enthält jede Instanz einer Klasse einen separaten Satz von aller Instanzfelder dieser Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="cfde9-627">Wenn ein Feld verwiesen wird, einem *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `E.M`, wenn `M` ist ein statisches Feld `E` müssen kennzeichnen ein Typ mit `M` , und wenn `M` ist ein Instanzfeld E muss eine Instanz von einem Typ mit deuten `M`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="cfde9-628">Die Unterschiede zwischen statischen und Instanzmember finden Sie weiter unten in [statische und Instanzmember](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="cfde9-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="cfde9-629">Schreibgeschützte Felder</span><span class="sxs-lookup"><span data-stu-id="cfde9-629">Readonly fields</span></span>

<span data-ttu-id="cfde9-630">Wenn eine *Field_declaration* enthält eine `readonly` Modifizierer, die Felder, die durch die Deklaration eingeführt sind ***schreibgeschützte Felder***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="cfde9-631">Direkte Zuweisungen an schreibgeschützte Felder können nur als Teil dieser Deklaration oder in einem Instanzenkonstruktor oder einen statischen Konstruktor in derselben Klasse auftreten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="cfde9-632">(Ein schreibgeschütztes Feld kann mehrere Male in diesen Kontexten zugewiesen werden.) Insbesondere direkten Zuweisungen für eine `readonly` Feld sind nur in den folgenden Kontexten zulässig:</span><span class="sxs-lookup"><span data-stu-id="cfde9-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="cfde9-633">In der *Variable_declarator* wird das Feld (mit einem *Variable_initializer* in der Deklaration).</span><span class="sxs-lookup"><span data-stu-id="cfde9-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="cfde9-634">Für ein Instanzenfeld in den Instanzkonstruktoren der Klasse, die die Felddeklaration enthält. für ein statisches Feld im statischen Konstruktor der Klasse, die die Felddeklaration enthält.</span><span class="sxs-lookup"><span data-stu-id="cfde9-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="cfde9-635">Hierbei handelt es sich auch um die einzigen Fälle, in dem Übergeben einer `readonly` Feld als ein `out` oder `ref` Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="cfde9-636">Beim Zuweisen einer `readonly` Feld ein, oder übergeben Sie sie als ein `out` oder `ref` Parameter in einem anderen Kontext ist ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="cfde9-637">Verwenden für Konstanten statische schreibgeschützte Felder</span><span class="sxs-lookup"><span data-stu-id="cfde9-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="cfde9-638">Ein `static readonly` Feld ist hilfreich, wenn ein symbolische Namen für einen konstanten Wert gewünscht ist, aber wenn der Typ des Werts in nicht zulässig ist eine `const` Deklaration, oder wenn der Wert nicht zur Kompilierzeit berechnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="cfde9-639">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-639">In the example</span></span>
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
<span data-ttu-id="cfde9-640">die `Black`, `White`, `Red`, `Green`, und `Blue` Member können nicht deklariert werden, als `const` Member, da ihre Werte nicht zur Kompilierungszeit berechnet werden können.</span><span class="sxs-lookup"><span data-stu-id="cfde9-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="cfde9-641">Deklarieren Sie sie jedoch `static readonly` stattdessen nahezu die gleiche Wirkung hat.</span><span class="sxs-lookup"><span data-stu-id="cfde9-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="cfde9-642">Versionsverwaltung von Konstanten und statische schreibgeschützte Felder</span><span class="sxs-lookup"><span data-stu-id="cfde9-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="cfde9-643">Konstanten und schreibgeschützte Felder haben eine unterschiedliche binäre versionsverwaltung-Semantik.</span><span class="sxs-lookup"><span data-stu-id="cfde9-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="cfde9-644">Wenn eine Konstante in ein Ausdruck verwiesen wird, wird der Wert der Konstanten zur Kompilierzeit abgerufen, aber wenn ein Ausdruck ein schreibgeschütztes Feld verwiesen wird, wird der Wert des Felds nicht bis zur Laufzeit abgerufen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="cfde9-645">Betrachten Sie eine Anwendung, die aus zwei verschiedenen Programmen besteht aus:</span><span class="sxs-lookup"><span data-stu-id="cfde9-645">Consider an application that consists of two separate programs:</span></span>
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

<span data-ttu-id="cfde9-646">Die `Program1` und `Program2` Namespaces kennzeichnen die beiden Programme, die separat kompiliert werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="cfde9-647">Da `Program1.Utils.X` ist als ein statisches schreibgeschütztes Feld, die Ausgabe des Werts von deklariert die `Console.WriteLine` Anweisung zur Kompilierzeit nicht bekannt, aber stattdessen zur Laufzeit abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="cfde9-648">Also wenn der Wert des `X` geändert wird und `Program1` erneut kompiliert wird, die `Console.WriteLine` Anweisung gibt der neue Wert, selbst wenn `Program2` ist nicht neu kompiliert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="cfde9-649">Hatte jedoch `X` wurde eine Konstante, die den Wert der `X` würde abgerufen haben, wurden zum Zeitpunkt `Program2` kompiliert wurde, und würde nicht betroffen von Änderungen in `Program1` bis `Program2` neu kompiliert wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="cfde9-650">Flüchtige Felder</span><span class="sxs-lookup"><span data-stu-id="cfde9-650">Volatile fields</span></span>

<span data-ttu-id="cfde9-651">Wenn eine *Field_declaration* enthält eine `volatile` Modifizierer, die Felder, der durch diese Deklaration sind ***flüchtige Felder***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="cfde9-652">Für nicht flüchtige Felder Optimierungstechniken, die Anweisungen neu anordnet können zu unerwarteten und unvorhersehbaren Ergebnissen führen in Multithreaded-Programmen, die Zugriff auf Felder ohne Synchronisierung, z. B. über die *Lock_statement*  ([Die sperranweisung](statements.md#the-lock-statement)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="cfde9-653">Diese Optimierungen können durch den Compiler, durch die Laufzeit-System oder von der Hardware ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="cfde9-654">Für flüchtige Felder sind solche Neuanordnen von Optimierungen beschränkt:</span><span class="sxs-lookup"><span data-stu-id="cfde9-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="cfde9-655">Ein Lesevorgang eines flüchtigen Felds wird aufgerufen, eine ***Volatile lesen***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="cfde9-656">Ein flüchtiger Lesevorgang wurde "acquire-Semantik;" Es ist garantiert, also bevor alle Verweise auf den Speicher auftreten, die auftreten, nachdem sie in der anweisungsabfolge.</span><span class="sxs-lookup"><span data-stu-id="cfde9-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="cfde9-657">Ein Schreibvorgang eines flüchtigen Felds wird aufgerufen, eine ***flüchtiger Schreibvorgang***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="cfde9-658">Ein flüchtiger Schreibvorgang hat "release Semantik"; Es ist garantiert, also nach Verweise Arbeitsspeicher vor der Write-Anweisung in der anweisungsabfolge ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="cfde9-659">Diese Einschränkungen stellen sicher, dass alle Threads volatile-Schreibvorgänge, die von einem anderen Thread ausgeführt werden, in der Reihenfolge ihrer Ausführung berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="cfde9-660">Keine entsprechende Implementierung ist nicht erforderlich, zu der eine einzelne gesamte Sortierung von flüchtigen Schreibvorgänge von allen Threads der Ausführung sehen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="cfde9-661">Der Typ eines flüchtigen Felds muss es sich um eine der folgenden sein:</span><span class="sxs-lookup"><span data-stu-id="cfde9-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="cfde9-662">Ein *Reference_type*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-662">A *reference_type*.</span></span>
*  <span data-ttu-id="cfde9-663">Der Typ `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, oder` System.UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or` System.UIntPtr`.</span></span>
*  <span data-ttu-id="cfde9-664">Ein *Enum_type* müssen einen Enum-Basistyp des `byte`, `sbyte`, `short`, `ushort`, `int`, oder `uint`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="cfde9-665">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-665">The example</span></span>
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
<span data-ttu-id="cfde9-666">erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="cfde9-666">produces the output:</span></span>
```
result = 143
```

<span data-ttu-id="cfde9-667">In diesem Beispiel ist die Methode `Main` startet einen neuen Thread, der die Methode ausgeführt wird `Thread2`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="cfde9-668">Diese Methode speichert einen Wert in ein nicht flüchtiges Feld namens `result`, dann speichert `true` im flüchtigen Felds `finished`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="cfde9-669">Der Hauptthread wartet auf das Feld `finished` festgelegt werden, um `true`, liest dann das Feld `result`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="cfde9-670">Da `finished` deklariert wurde, `volatile`, der Haupt-Thread muss den Wert lesen `143` aus dem Feld `result`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="cfde9-671">Wenn das Feld `finished` nicht deklariert wurde `volatile`, es wäre für den Store, um zulässige `result` an den primären Thread nach dem Speichervorgang für sichtbar `finished`, und somit für den Hauptthread, lesen den Wert `0` aus der Feld `result`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="cfde9-672">Deklarieren von `finished` als eine `volatile` Feld verhindert, dass solche Inkonsistenzen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="cfde9-673">Feld-Initialisierung</span><span class="sxs-lookup"><span data-stu-id="cfde9-673">Field initialization</span></span>

<span data-ttu-id="cfde9-674">Der Anfangswert eines Felds, ob es ein statisches Feld oder ein Instanzenfeld, ist der Standardwert ([Standardwerte](variables.md#default-values)) der Typ des Felds.</span><span class="sxs-lookup"><span data-stu-id="cfde9-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="cfde9-675">Es ist nicht möglich, beachten Sie den Wert eines Felds aus, bevor diese standardinitialisierung aufgetreten, und ein Feld daher nie ist "nicht initialisiert".</span><span class="sxs-lookup"><span data-stu-id="cfde9-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="cfde9-676">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-676">The example</span></span>
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
<span data-ttu-id="cfde9-677">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="cfde9-677">produces the output</span></span>
```
b = False, i = 0
```
<span data-ttu-id="cfde9-678">Da `b` und `i` sind beide automatisch mit Standardwerten initialisiert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="cfde9-679">Variableninitialisierern</span><span class="sxs-lookup"><span data-stu-id="cfde9-679">Variable initializers</span></span>

<span data-ttu-id="cfde9-680">Herausgeberkontos ausgewiesenen Form Felddeklarationen *Variable_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="cfde9-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="cfde9-681">Für statische Felder entsprechen Variableninitialisierern zuweisungsanweisungen, die während der klasseninitialisierung ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="cfde9-682">Beispielsweise entsprechen Feldern Variableninitialisierern zuweisungsanweisungen, die ausgeführt werden, wenn eine Instanz der Klasse erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="cfde9-683">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-683">The example</span></span>
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
<span data-ttu-id="cfde9-684">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="cfde9-684">produces the output</span></span>
```
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="cfde9-685">Da eine Zuweisung zu `x` tritt auf, wenn statischen Feldinitialisierer ausführen und Zuweisungen zu `i` und `s` auftreten, wenn die Instanzenfeldinitialisierer ausführen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="cfde9-686">Die standardinitialisierung-Wert in beschriebenen [Feld Initialisierung](classes.md#field-initialization) tritt für alle Felder einschließlich der Felder, die Variable initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="cfde9-687">Wenn eine Klasse initialisiert wird, daher alle statischen Felder in dieser Klasse werden zuerst auf ihre Standardwerte initialisiert, und klicken Sie dann die statischen Feldinitialisierer in der Reihenfolge im Text ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="cfde9-688">Wenn eine Instanz einer Klasse erstellt wird, ebenso alle Instanzenfelder in dieser Instanz werden zuerst auf ihre Standardwerte initialisiert, und klicken Sie dann die Instanzenfeldinitialisierer in der Reihenfolge im Text ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="cfde9-689">Es ist möglich, für statische Felder mit Variableninitialisierern an, die im Standardzustand Wert beachtet werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="cfde9-690">Dies ist jedoch dringend davon abgeraten, als eine Frage des Stils.</span><span class="sxs-lookup"><span data-stu-id="cfde9-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="cfde9-691">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-691">The example</span></span>
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
<span data-ttu-id="cfde9-692">Dieses Verhalten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-692">exhibits this behavior.</span></span> <span data-ttu-id="cfde9-693">Trotz der zirkulären Definitionen von einer und b, das Programm ist ungültig.</span><span class="sxs-lookup"><span data-stu-id="cfde9-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="cfde9-694">Dies führt in der Ausgabe</span><span class="sxs-lookup"><span data-stu-id="cfde9-694">It results in the output</span></span>
```
a = 1, b = 2
```
<span data-ttu-id="cfde9-695">Da die statischen Felder `a` und `b` werden initialisiert, um `0` (der Standardwert für `int`) vor der Ausführung sind.</span><span class="sxs-lookup"><span data-stu-id="cfde9-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="cfde9-696">Bei den Initialisierer für `a` ausgeführt wird, wird den Wert des `b` ist 0 (null), und daher `a` wird initialisiert `1`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="cfde9-697">Bei den Initialisierer für `b` ausgeführt wird, wird den Wert des `a` ist bereits `1`, und daher `b` wird initialisiert `2`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="cfde9-698">Initialisierung statischer Felder</span><span class="sxs-lookup"><span data-stu-id="cfde9-698">Static field initialization</span></span>

<span data-ttu-id="cfde9-699">Die statische Variable Feldinitialisierer einer Klasse entsprechen einer Sequenz von Zuweisungen, die in der Reihenfolge im Text ausgeführt werden, in denen sie in der Klassendeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="cfde9-700">Wenn ein statischer Konstruktor ([statische Konstruktoren](classes.md#static-constructors)) vorhanden ist in der Klasse, von der statischen Feldinitialisierer ausgeführt direkt vor dem Ausführen dieser statischen Konstruktors.</span><span class="sxs-lookup"><span data-stu-id="cfde9-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="cfde9-701">Andernfalls werden die statischen Feldinitialisierer zu einem Zeitpunkt abhängig von der Implementierung vor der ersten Verwendung eines statischen Felds dieser Klasse ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="cfde9-702">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-702">The example</span></span>
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="cfde9-703">kann entweder die Ausgabe erzeugt:</span><span class="sxs-lookup"><span data-stu-id="cfde9-703">might produce either the output:</span></span>
```
Init A
Init B
1 1
```
<span data-ttu-id="cfde9-704">oder der Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="cfde9-704">or the output:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="cfde9-705">Da die Ausführung von `X`Initialisierer und `Y`Initialisierer konnte in beliebiger Reihenfolge auftreten; diese sind nur eingeschränkt, die auftreten, bevor Sie die Verweise auf diese Felder.</span><span class="sxs-lookup"><span data-stu-id="cfde9-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="cfde9-706">Aber im Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cfde9-706">However, in the example:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="cfde9-707">die Ausgabe muss sein:</span><span class="sxs-lookup"><span data-stu-id="cfde9-707">the output must be:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="cfde9-708">Da die Regeln für die beim Ausführen der statischer Konstruktors (gemäß [statische Konstruktoren](classes.md#static-constructors)) bereitstellen, die `B`des statischen Konstruktor (und somit `B`des statischen Feldinitialisierer) müssen ausführen, bevor `A`des statischen Konstruktor und Feldinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="cfde9-709">Initialisierung von Instanzfeldern</span><span class="sxs-lookup"><span data-stu-id="cfde9-709">Instance field initialization</span></span>

<span data-ttu-id="cfde9-710">Die Variable Instanzenfeld einer Klasse entsprechen einer Sequenz von Zuweisungen, die beim Einstieg in eines der Instanzkonstruktoren sofort ausgeführt werden ([Konstruktorinitialisierer](classes.md#constructor-initializers)) dieser Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="cfde9-711">Die Variable Initialisierer werden in der Reihenfolge im Text ausgeführt in denen sie in der Klassendeklaration vorkommen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="cfde9-712">Die Klasse erstellen und initialisieren Prozess ist unter [Instanzkonstruktoren](classes.md#instance-constructors).</span><span class="sxs-lookup"><span data-stu-id="cfde9-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="cfde9-713">Einem Variableninitialisierer für ein Instanzfeld kann nicht zu erstellende Instanz verweisen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="cfde9-714">Daher wird ein Kompilierzeitfehler auf `this` in einem Variableninitialisierer, als es ist ein Fehler während der Kompilierung für eine Variable Initialisierer auf beliebiger Instanzmember über eine *Simple_name*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="cfde9-715">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="cfde9-716">die Variable Initialisierer für `y` führt zu einem Fehler während der Kompilierung, da sie ein Mitglied der zu erstellenden Instanz verweist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="cfde9-717">Methoden</span><span class="sxs-lookup"><span data-stu-id="cfde9-717">Methods</span></span>

<span data-ttu-id="cfde9-718">Eine ***Methode*** ist ein Member, das eine Berechnung oder eine Aktion implementiert, die durch ein Objekt oder eine Klasse durchgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="cfde9-719">Methoden deklariert werden, mithilfe von *Method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cfde9-719">Methods are declared using *method_declaration*s:</span></span>

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="cfde9-720">Ein *Method_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer ](classes.md#access-modifiers)), wird die `new` ([der new-Modifizierer](classes.md#the-new-modifier)), `static` ([statische und Instanzmethoden](classes.md#static-and-instance-methods)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Methoden außer Kraft setzen](classes.md#override-methods)), `sealed` ([Versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakten Methoden](classes.md#abstract-methods)), und `extern` ([Externe Methoden](classes.md#external-methods)) Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="cfde9-721">Eine Deklaration verfügt über eine gültige Kombination von Modifizierern aus, wenn alle der folgenden Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="cfde9-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="cfde9-722">Die Deklaration enthält eine gültige Kombination von Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="cfde9-723">Die Deklaration ist nicht derselben Modifizierer mehrmals enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="cfde9-724">Die Deklaration enthält mindestens eine der folgenden Modifizierer: `static`, `virtual`, und `override`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="cfde9-725">Die Deklaration enthält mindestens eine der folgenden Modifizierer: `new` und `override`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="cfde9-726">Wenn die Deklaration enthält die `abstract` Modifizierer, und klicken Sie dann auf die Deklaration umfasst keine der folgenden Modifizierer: `static`, `virtual`, `sealed` oder `extern`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="cfde9-727">Wenn die Deklaration enthält die `private` Modifizierer, und klicken Sie dann auf die Deklaration umfasst keine der folgenden Modifizierer: `virtual`, `override`, oder `abstract`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="cfde9-728">Wenn die Deklaration enthält die `sealed` Modifizierer, und klicken Sie dann auf die Deklaration enthält auch die `override` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="cfde9-729">Wenn die Deklaration enthält die `partial` Modifizierer, dann umfasst keine der folgenden Modifizierer: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`, oder `extern`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="cfde9-730">Eine Methode mit dem `async` -Modifizierer ist eine asynchrone Funktion und die in beschriebenen Regeln folgt [asynchrone Funktionen](classes.md#async-functions).</span><span class="sxs-lookup"><span data-stu-id="cfde9-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="cfde9-731">Die *Return_type* Deklaration gibt den Typ des Werts berechnet und zurückgegeben wird, von der Methode einer Methode an.</span><span class="sxs-lookup"><span data-stu-id="cfde9-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="cfde9-732">Die *Return_type* ist `void` , wenn die Methode keinen Wert zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="cfde9-733">Wenn die Deklaration enthält die `partial` Modifizierer, und klicken Sie dann auf der Rückgabetyp muss `void`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="cfde9-734">Die *Member_name* gibt den Namen der Methode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="cfde9-735">Wenn die Methode eine explizite Schnittstellenmember-Implementierung ist ([explizite Implementierungen eines Schnittstellenmembers](interfaces.md#explicit-interface-member-implementations)), wird die *Member_name* ist einfach ein *Bezeichner*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="cfde9-736">Für eine explizite Schnittstellenmember-Implementierung die *Member_name* besteht aus einer *Interface_type* gefolgt von einem "`.`" und ein *Bezeichner*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="cfde9-737">Der optionale *Type_parameter_list* gibt an, die Typparameter der Methode ([Typparameter](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="cfde9-738">Wenn eine *Type_parameter_list* angegeben ist, wird die Methode eine ***generische Methode***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="cfde9-739">Wenn die Methode verfügt über eine `extern` Modifizierer eine *Type_parameter_list* kann nicht angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="cfde9-740">Der optionale *Formal_parameter_list* gibt die Parameter der Methode ([Methodenparameter](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="cfde9-741">Der optionale *Type_parameter_constraints_clause*s angeben von Einschränkungen für einzelne Typparameter ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)) und kann nur angegeben werden, wenn eine *Type_parameter_ Liste* ebenfalls angegeben wird, und die Methode verfügt nicht über eine `override` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="cfde9-742">Die *Return_type* und alle referenzierten Typen der *Formal_parameter_list* einer Methode muss mindestens dieselben zugriffsmöglichkeiten bieten wie die Methode selbst ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cfde9-743">Die *Method_body* ist entweder ein Semikolon, ein ***Anweisungstext*** oder ***ausdruckskörper***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="cfde9-744">Eine-Anweisungstext besteht aus einem *Block*, der angibt, dass der Anweisungen ausgeführt werden, wenn die Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="cfde9-745">Ein ausdruckskörper besteht aus `=>` gefolgt von einem *Ausdruck* und ein Semikolon, und gibt einen einzelnen Ausdruck ausführen, wenn die Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="cfde9-746">Für `abstract` und `extern` Methoden, die *Method_body* besteht einfach aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="cfde9-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="cfde9-747">Für `partial` Methoden der *Method_body* kann entweder ein Semikolon, einem Blocktext oder einen ausdruckskörper bestehen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="cfde9-748">Für alle anderen Methoden die *Method_body* ist ein Blocktext oder einen ausdruckskörper.</span><span class="sxs-lookup"><span data-stu-id="cfde9-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="cfde9-749">Wenn die *Method_body* besteht aus einem Semikolon, und klicken Sie dann die Deklaration möglicherweise nicht enthalten. die `async` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="cfde9-750">Der Name, der Liste der Typparameter und die Liste der formalen Parameter einer Methode definieren, die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) der Methode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="cfde9-751">Insbesondere besteht aus die Signatur einer Methode den Namen, die Anzahl der Parameter vom Typ und die Anzahl, Modifizierer, und Typen der formalen Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="cfde9-752">Für diese Zwecke wird einen Typparameter der Methode, die in den Typ eines formalen Parameters auftritt, nicht mit dem Namen, aber anhand ihrer Ordnungsposition in der Liste der Typargumente der Methode identifiziert. Der Rückgabetyp ist nicht Teil der Signatur einer Methode, noch werden die Namen der Typparameter oder die formalen Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="cfde9-753">Der Name einer Methode muss die Namen aller anderen nicht--Methoden, in der gleichen Klasse deklariert unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="cfde9-754">Darüber hinaus die Signatur einer Methode muss von den Signaturen aller anderen in derselben Klasse deklarierten Methoden unterscheiden, und zwei in der gleichen Klasse deklarierte Methoden dürfen keine Signaturen, die ausschließlich vom unterscheiden `ref` und `out`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="cfde9-755">Der Methode *Type_parameter*sind im Gültigkeitsbereich der *Method_declaration*, und können verwendet werden, um die von Formulartypen in diesem Bereich in der gesamten *Return_type*, *Method_body*, und *Type_parameter_constraints_clause*s jedoch nicht in *Attribute*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="cfde9-756">Alle formalen Parametern und Typparametern müssen unterschiedliche Namen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="cfde9-757">Methodenparameter</span><span class="sxs-lookup"><span data-stu-id="cfde9-757">Method parameters</span></span>

<span data-ttu-id="cfde9-758">Die Parameter einer Methode, sofern vorhanden, werden von der Methode deklariert *Formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

<span data-ttu-id="cfde9-759">Liste der formalen Parameter besteht aus einen oder mehrere durch Trennzeichen getrennte Parameter von denen nur die letzte möglicherweise eine *Parameter_array*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="cfde9-760">Ein *Fixed_parameter* besteht aus einer optionalen Gruppe von *Attribute* ([Attribute](attributes.md)), ein optionales `ref`, `out` oder `this` Modifizierer ein *Typ*, *Bezeichner* und einem optionalen *Default_argument*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="cfde9-761">Jede *Fixed_parameter* deklariert einen Parameter des angegebenen Typs mit dem angegebenen Namen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="cfde9-762">Die `this` kennzeichnet die Methode als eine Erweiterungsmethode und ist nur für den ersten Parameter einer statischen Methode zulässig.</span><span class="sxs-lookup"><span data-stu-id="cfde9-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="cfde9-763">Erweiterungsmethoden werden ausführlicher beschrieben [Erweiterungsmethoden](classes.md#extension-methods).</span><span class="sxs-lookup"><span data-stu-id="cfde9-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="cfde9-764">Ein *Fixed_parameter* mit einer *Default_argument* heißt ein ***Optionaler Parameter***, während eine *Fixed_parameter* ohne eine *Default_argument* ist eine ***Erforderlicher Parameter***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="cfde9-765">Ein erforderlicher Parameter möglicherweise nicht angezeigt, nachdem ein optionaler Parameter in einer *Formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="cfde9-766">Ein `ref` oder `out` sind keine Parameter ein *Default_argument*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="cfde9-767">Die *Ausdruck* in einem *Default_argument* muss eine der folgenden sein:</span><span class="sxs-lookup"><span data-stu-id="cfde9-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="cfde9-768">a *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="cfde9-768">a *constant_expression*</span></span>
*  <span data-ttu-id="cfde9-769">Ein Ausdruck der Form `new S()` , in denen `S` ist ein Werttyp.</span><span class="sxs-lookup"><span data-stu-id="cfde9-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="cfde9-770">Ein Ausdruck der Form `default(S)` , in denen `S` ist ein Werttyp.</span><span class="sxs-lookup"><span data-stu-id="cfde9-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="cfde9-771">Die *Ausdruck* muss implizit von einer Identität oder ein NULL-Werte zulassen Konvertierung in den Typ des Parameters konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="cfde9-772">Wenn optionale Parameter in eine implementierende Deklaration der partiellen Methode auftreten ([partielle Methoden](classes.md#partial-methods)), eine explizite Schnittstellenmember-Implementierung ([explizite Implementierungen eines Schnittstellenmembers](interfaces.md#explicit-interface-member-implementations)) oder in einem einzigen Parameter Indexerdeklaration ([Indexer](classes.md#indexers)) der Compiler erhalten eine Warnung aus, da diese Member nicht auf eine Weise können, die Argumente aufgerufen werden, die ausgelassen werden können.</span><span class="sxs-lookup"><span data-stu-id="cfde9-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="cfde9-773">Ein *Parameter_array* besteht aus einer optionalen Gruppe von *Attribute* ([Attribute](attributes.md)), ein `params` Modifizierer eine *Array_type*, und ein *Bezeichner*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="cfde9-774">Ein Parameterarray deklariert einen einzelnen Parameter des Typs angegebenen Array mit dem angegebenen Namen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="cfde9-775">Die *Array_type* eines Parameters Arrays ein eindimensionales Array sein muss ([Arraytypen](arrays.md#array-types)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="cfde9-776">In einem Methodenaufruf einem Parameterarray können entweder ein einzelnes Argument des Typs angegebenen Array angegeben werden, oder er lässt NULL oder mehr Argumente den Elementtyp des Arrays angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="cfde9-777">Parameter-Arrays werden ausführlich in [Parameterarrays](classes.md#parameter-arrays).</span><span class="sxs-lookup"><span data-stu-id="cfde9-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="cfde9-778">Ein *Parameter_array* nach einem optionalen Parameter auftreten können, jedoch keinen Standardwert – die Auslassung der Argumente für eine *Parameter_array* würde stattdessen bei der Erstellung eines leeren Arrays.</span><span class="sxs-lookup"><span data-stu-id="cfde9-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="cfde9-779">Das folgende Beispiel veranschaulicht verschiedene Arten von Parametern:</span><span class="sxs-lookup"><span data-stu-id="cfde9-779">The following example illustrates different kinds of parameters:</span></span>
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

<span data-ttu-id="cfde9-780">In der *Formal_parameter_list* für `M`, `i` ist ein erforderliche Ref-Parameter, `d` Parameter ist ein erforderlicher Wert, `b`, `s`, `o` und `t` Optionale Parameter sind und `a` ein Parameterarray.</span><span class="sxs-lookup"><span data-stu-id="cfde9-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="cfde9-781">Deklaration einer Methode erstellt einen separaten Deklarationsabschnitt für Parameter "," Parameter vom Typ "und" lokale Variablen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="cfde9-782">Namen werden in diesen Deklarationsabschnitt eingeführt, durch die Liste der Typparameter und die Liste der formalen Parameter der Methode und Deklarationen von lokalen Variablen in der *Block* der Methode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="cfde9-783">Es ist ein Fehler bei zwei Mitgliedern eines Deklarationsabschnitts der Methode, die den gleichen Namen haben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="cfde9-784">Es ist ein Fehler für die Methode Deklaration und der Deklaration lokaler Variablen Speicherplatz eines geschachtelten Deklarationsabschnitts Elemente mit dem gleichen Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="cfde9-785">Aufruf einer Methode ([Methodenaufrufe](expressions.md#method-invocations)) erstellt eine Kopie, die für diesen Aufruf spezifischen, weist der formalen Parameter und lokalen Variablen der Methode und der Argumentliste des Aufrufs, Werte oder Variablenverweise an das neu erstellte formalen Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="cfde9-786">In der *Block* einer Methode, formalen Parameter verwiesen werden können, durch die IDs in *Simple_name* Ausdrücke ([einfache Namen](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="cfde9-787">Es gibt vier Arten der formalen Parameter:</span><span class="sxs-lookup"><span data-stu-id="cfde9-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="cfde9-788">Parameter, die ohne Modifizierer deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="cfde9-789">Verweisparameter, die mit deklariert werden die `ref` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="cfde9-790">Output-Parameter, die mit deklariert werden die `out` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="cfde9-791">Parameterarrays, die mit deklariert werden die `params` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="cfde9-792">Siehe [Signaturen und überladen](basic-concepts.md#signatures-and-overloading), `ref` und `out` Modifizierer sind Teil der Signatur einer Methode, aber die `params` -Modifizierer ist nicht.</span><span class="sxs-lookup"><span data-stu-id="cfde9-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="cfde9-793">Wert-Parametern</span><span class="sxs-lookup"><span data-stu-id="cfde9-793">Value parameters</span></span>

<span data-ttu-id="cfde9-794">Ein Parameter mit dem keine Modifizierer deklariert ist eine Value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="cfde9-795">Ein Wertparameter entspricht einer lokalen Variablen, die den Anfangswert von das entsprechende Argument im Aufruf Methode angegebenen erhält.</span><span class="sxs-lookup"><span data-stu-id="cfde9-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="cfde9-796">Wenn ein formaler Parameter ein Werteparameter ist, muss das entsprechende Argument in einen Methodenaufruf ein Ausdruck, der implizit konvertiert werden kann ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den formalen Parametertyp.</span><span class="sxs-lookup"><span data-stu-id="cfde9-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="cfde9-797">Eine Methode ist zulässig, einen Wertparameter neue Werte zuweisen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="cfde9-798">Diese Zuweisungen wirken sich nur auf den Speicherort der lokalen Speicherressource, dargestellt durch den Wertparameter – sie haben keine Auswirkungen auf das tatsächliche Argument im Methodenaufruf angegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="cfde9-799">Verweisparameter</span><span class="sxs-lookup"><span data-stu-id="cfde9-799">Reference parameters</span></span>

<span data-ttu-id="cfde9-800">Ein Parameter deklariert, mit einem `ref` Modifizierer ist ein Verweisparameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="cfde9-801">Im Gegensatz zu einem Value-Parameter erstellt ein Verweisparameter nicht auf einen neuen Speicherort zu.</span><span class="sxs-lookup"><span data-stu-id="cfde9-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="cfde9-802">Stattdessen stellt ein Verweisparameter am gleichen Speicherort wie die Variable als Argument im Methodenaufruf angegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="cfde9-803">Wenn ein formaler Parameter einen Verweisparameter handelt, muss das entsprechende Argument in einen Methodenaufruf des Schlüsselworts bestehen `ref` gefolgt von einem *Variable_reference* ([präzise Regeln für die Bestimmung definitive Zuweisung](variables.md#precise-rules-for-determining-definite-assignment)) des gleichen Typs wie der formale Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="cfde9-804">Eine Variable muss definitiv zugewiesen werden, bevor es als Verweisparameter übergeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="cfde9-805">Innerhalb einer Methode gilt ein Verweisparameter immer als definitiv zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="cfde9-806">Eine Methode, die als ein Iterator deklariert ([Iteratoren](classes.md#iterators)) keine Verweisparameter angeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="cfde9-807">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-807">The example</span></span>
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
<span data-ttu-id="cfde9-808">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="cfde9-808">produces the output</span></span>
```
i = 2, j = 1
```

<span data-ttu-id="cfde9-809">Für den Aufruf von `Swap` in `Main`, `x` stellt `i` und `y` stellt `j`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="cfde9-810">Der Aufruf ist daher die Auswirkungen der Tauschen der Werte der `i` und `j`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="cfde9-811">In einer Methode, die mehrere Namen am gleichen Speicherort darstellen kann, Verweisparameter akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="cfde9-812">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-812">In the example</span></span>
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
<span data-ttu-id="cfde9-813">der Aufruf von `F` in `G` übergibt einen Verweis auf `s` für beide `a` und `b`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="cfde9-814">Daher ist es bei diesen Aufruf, der die Namen `s`, `a`, und `b` verweisen alle auf den gleichen Speicherort aus, und die drei alle Zuweisungen Ändern des Instanzfelds `s`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="cfde9-815">Output-Parameter</span><span class="sxs-lookup"><span data-stu-id="cfde9-815">Output parameters</span></span>

<span data-ttu-id="cfde9-816">Ein Parameter deklariert, mit einem `out` Modifizierer ist ein Output-Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="cfde9-817">Ein Verweisparameter ähnlich, ist ein Output-Parameter keinen neuen Speicherort erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="cfde9-818">Stattdessen stellt ein Output-Parameter am gleichen Speicherort wie die Variable als Argument im Methodenaufruf angegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="cfde9-819">Wenn ein formaler Parameter ein Ausgabeparameter ist, muss das entsprechende Argument in einen Methodenaufruf des Schlüsselworts bestehen `out` gefolgt von einem *Variable_reference* ([präzise Regeln für die Bestimmung definitive Zuweisung](variables.md#precise-rules-for-determining-definite-assignment)) des gleichen Typs wie der formale Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="cfde9-820">Eine Variable muss nicht definitiv zugewiesen werden, bevor es als Output-Parameter übergeben werden kann, aber nach einem Aufruf, in dem eine Variable als Output-Parameter übergeben wurde, wird die Variable definitiv zugewiesen betrachtet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="cfde9-821">Innerhalb einer Methode, so wie eine lokale Variable, ein Output-Parameter zunächst gilt als nicht zugewiesene und müssen definitiv zugewiesen werden, bevor der Wert verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="cfde9-822">Alle Ausgabeparameter einer Methode muss definitiv zugewiesen werden, bevor die Methode zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="cfde9-823">Eine Methode, die als partielle Methode deklariert ([partielle Methoden](classes.md#partial-methods)) oder eines Iterators ([Iteratoren](classes.md#iterators)) können keine Ausgabeparameter enthalten haben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="cfde9-824">Output-Parameter werden in der Regel in Methoden verwendet, die Rückgabe mehrerer Werte zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="cfde9-825">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cfde9-825">For example:</span></span>
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

<span data-ttu-id="cfde9-826">Das Beispiel erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="cfde9-826">The example produces the output:</span></span>
```
c:\Windows\System\
hello.txt
```

<span data-ttu-id="cfde9-827">Beachten Sie, dass die `dir` und `name` Variablen können nicht zugewiesen werden, vor der Übergabe an `SplitPath`, und dass sie als definitiv zugewiesen, nach dem Aufruf angesehen werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="cfde9-828">Parameterarrays</span><span class="sxs-lookup"><span data-stu-id="cfde9-828">Parameter arrays</span></span>

<span data-ttu-id="cfde9-829">Ein Parameter deklariert, mit einem `params` Modifizierer ist ein Parameterarray.</span><span class="sxs-lookup"><span data-stu-id="cfde9-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="cfde9-830">Wenn eine Liste formaler Parameter ein Parameterarray enthält, muss der letzte Parameter in der Liste sein, und es muss ein eindimensionales Array-Typ aufweisen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="cfde9-831">Beispielsweise die Typen `string[]` und `string[][]` kann verwendet werden, wie der Typ eines Parameterarrays, aber der Typ `string[,]` nicht können.</span><span class="sxs-lookup"><span data-stu-id="cfde9-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="cfde9-832">Es ist nicht möglich, zum Kombinieren der `params` Modifizierer mit den Modifizierern `ref` und `out`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="cfde9-833">Ein Parameterarray kann die Argumente in eine von zwei Arten in einem Methodenaufruf angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="cfde9-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="cfde9-834">Das Argument angegeben wird, für ein Parameterarray kann ein einzelner Ausdruck sein, die implizit konvertiert werden kann ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den Parameter-Arraytyp.</span><span class="sxs-lookup"><span data-stu-id="cfde9-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="cfde9-835">In diesem Fall verhält sich das Parameterarray genau wie ein Value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="cfde9-836">Der Aufruf kann auch angeben, NULL oder mehr Argumente für das Parameterarray, wobei jedes Argument ist ein Ausdruck, der implizit konvertiert werden kann ([implizite Konvertierungen](conversions.md#implicit-conversions)), die den Elementtyp des Parameterarrays.</span><span class="sxs-lookup"><span data-stu-id="cfde9-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="cfde9-837">In diesem Fall der Aufruf erstellt eine Instanz des Parameterarraytyps mit einer Länge, die Anzahl von Argumenten entspricht, initialisiert die Elemente der Arrayinstanz mit den Werten des angegebenen Arguments und verwendet die neu erstellte Array-Instanz als die tatsächliche Argument.</span><span class="sxs-lookup"><span data-stu-id="cfde9-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="cfde9-838">Mit Ausnahme von ermöglichen, dass eine Variable Anzahl von Argumenten in einen Aufruf, ein Parameterarray entspricht exakt dem Value-Parameter ([-Wertparameter](classes.md#value-parameters)) des gleichen Typs.</span><span class="sxs-lookup"><span data-stu-id="cfde9-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="cfde9-839">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-839">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
<span data-ttu-id="cfde9-840">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="cfde9-840">produces the output</span></span>
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="cfde9-841">Der erste Aufruf der `F` einfach das Array übergibt `a` als ein Value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="cfde9-842">Der zweite Aufruf von `F` erstellt automatisch eine vier-Elemente `int[]` mit dem angegebenen Elementwerte und übergibt, die array-Instanz als ein Value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="cfde9-843">Entsprechend der dritte Aufruf von `F` wird ein NULL-Element erstellt `int[]` und übergibt diese Instanz als ein Value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="cfde9-844">Die zweite und dritte Aufrufe sind wie folgt:</span><span class="sxs-lookup"><span data-stu-id="cfde9-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="cfde9-845">Bei der Auflösung von funktionsüberladungen durchführen zu können, eine Methode mit einem Parameterarray möglicherweise auf, in seiner normalen Form oder in der erweiterten Form ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="cfde9-846">Die erweiterte Form von einer Methode ist nur dann, wenn die normale Form der Methode nicht anwendbar ist, und nur dann, wenn eine entsprechende Methode mit der gleichen Signatur wie die erweiterte Form noch nicht in denselben Typ deklariert ist verfügbar.</span><span class="sxs-lookup"><span data-stu-id="cfde9-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="cfde9-847">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-847">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
<span data-ttu-id="cfde9-848">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="cfde9-848">produces the output</span></span>
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="cfde9-849">Im Beispiel sind zwei der möglichen erweiterten Formulare der Methode mit einem Parameterarray bereits in der Klasse als reguläre Methoden enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="cfde9-850">Diese erweiterten datenformen sind aus diesem Grund nicht berücksichtigt, wenn die Auflösung von funktionsüberladungen ausführen, und wählen Sie die erste und dritte Methodenaufrufe daher die regulären Methoden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="cfde9-851">Wenn eine Klasse eine Methode mit einem Parameterarray deklariert, ist es nicht ungewöhnlich, dass auch einige der erweiterten Formen als reguläre Methoden eingeschlossen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="cfde9-852">-Instanz, die bei der erweiterten Zustand einer Methode mit einem Parameterarray tritt auf, wird aufgerufen, wodurch es möglich ist, die die Zuordnung eines Arrays zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="cfde9-853">Wenn der Typ, der ein Parameterarray ist `object[]`, eine mögliche Mehrdeutigkeit zwischen die normale Form der Methode und der erweiterten Form für ein einzelnes entsteht `object` Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="cfde9-854">Der Grund für die Mehrdeutigkeit ist, dass ein `object[]` ist selbst in den Typ implizit `object`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="cfde9-855">Die Mehrdeutigkeit stellt kein Problem, jedoch ein, da er aufgelöst werden kann, indem Sie eine Umwandlung einfügen, bei Bedarf.</span><span class="sxs-lookup"><span data-stu-id="cfde9-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="cfde9-856">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-856">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
<span data-ttu-id="cfde9-857">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="cfde9-857">produces the output</span></span>
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="cfde9-858">In der ersten und letzten Aufrufen von `F`, die normale Form von `F` ist anwendbar, da eine implizite Konvertierung des Argumenttyps in den Parametertyp vorhanden ist (beide sind vom Typ `object[]`).</span><span class="sxs-lookup"><span data-stu-id="cfde9-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="cfde9-859">Daher wählt die überladungsauflösung die normale Form von `F`, und das Argument wird als reguläre Werteparameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="cfde9-860">In der zweiten und dritten aufrufen, die normale Form von `F` ist nicht anwendbar, da keine implizite Konvertierung des Argumenttyps in den Parametertyp vorhanden ist (Typ `object` kann nicht implizit konvertiert Typ `object[]`).</span><span class="sxs-lookup"><span data-stu-id="cfde9-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="cfde9-861">Allerdings die erweiterte Form von `F` gilt, damit es von der Auflösung von funktionsüberladungen aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="cfde9-862">Als Ergebnis einem Element `object[]` wird durch den Aufruf erstellt und das einzige Element des Arrays mit dem angegebenen Argumentwert initialisiert wird (die selbst ist ein Verweis auf ein `object[]`).</span><span class="sxs-lookup"><span data-stu-id="cfde9-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="cfde9-863">Statische Methoden und Instanzmethoden</span><span class="sxs-lookup"><span data-stu-id="cfde9-863">Static and instance methods</span></span>

<span data-ttu-id="cfde9-864">Wenn die Deklaration einer Methode enthält einen `static` -Modifizierer ist, wird diese Methode wird als eine statische Methode sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="cfde9-865">Wenn kein `static` Modifizierer vorhanden ist, wird die Methode wird als eine Instanzmethode sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="cfde9-866">Eine statische Methode in einer bestimmten Instanz kann nicht ausgeführt werden, und es ist ein Fehler während der Kompilierung zum Verweisen auf `this` in einer statischen Methode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="cfde9-867">Eine Instanzmethode funktioniert für eine bestimmte Instanz einer Klasse, und diese Instanz zugegriffen werden kann, als `this` ([diesen Zugriff](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="cfde9-868">Wenn eine Methode verwiesen wird, einem *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `E.M`, wenn `M` ist eine statische Methode, `E` muss einen Typ mit deuten`M`, und wenn `M` ist eine Instanzmethode `E` muss eine Instanz von einem Typ mit deuten `M`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="cfde9-869">Die Unterschiede zwischen statischen und Instanzmember finden Sie weiter unten in [statische und Instanzmember](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="cfde9-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="cfde9-870">Virtuelle Methoden</span><span class="sxs-lookup"><span data-stu-id="cfde9-870">Virtual methods</span></span>

<span data-ttu-id="cfde9-871">Wenn eine Instanzmethodendeklaration enthält eine `virtual` -Modifizierer ist, wird diese Methode wird als eine virtuelle Methode sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="cfde9-872">Wenn kein `virtual` Modifizierer vorhanden ist, wird die Methode ist eine nicht virtuelle Methode bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="cfde9-873">Die Implementierung einer nicht virtuellen Methode ist unveränderlich: Die Implementierung ist identisch, ob die Methode, auf einer Instanz der Klasse in aufgerufen wird der sie deklariert ist, oder eine Instanz einer abgeleiteten Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="cfde9-874">Im Gegensatz dazu kann die Implementierung einer virtuellen Methode durch abgeleitete Klassen abgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="cfde9-875">Der Prozess der Implementierung einer geerbten virtuellen Methode heißt ***überschreiben*** diese Methode ([Methoden außer Kraft setzen](classes.md#override-methods)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="cfde9-876">Im Aufruf einer virtuellen Methode die ***Laufzeittyp*** der Instanz, die diesen Aufruf benötigt, direkten legt fest, die Implementierung der tatsächlichen Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="cfde9-877">Im Aufruf einer nicht virtuellen Methode die ***Kompilierzeittyp*** der Instanz ist der bestimmende Faktor.</span><span class="sxs-lookup"><span data-stu-id="cfde9-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="cfde9-878">Genau bedeutet dies, wenn eine Methode namens `N` wird aufgerufen, mit einer Argumentliste `A` auf einer Instanz mit einem Typ während der Kompilierung `C` und einen Laufzeittyp `R` (, in dem `R` ist entweder `C` oder eine abgeleitete Klasse von `C`), der Aufruf wird wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="cfde9-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="cfde9-879">Zunächst wird die Auflösung von funktionsüberladungen auf angewendet `C`, `N`, und `A`, auswählen eine bestimmte Methode `M` aus dem Satz von Methoden deklariert "und" geerbt `C`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="cfde9-880">Finden Sie im [Methodenaufrufe](expressions.md#method-invocations).</span><span class="sxs-lookup"><span data-stu-id="cfde9-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="cfde9-881">Wenn danach `M` ist eine nicht virtuelle Methode, `M` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="cfde9-882">Andernfalls `M` ist eine virtuelle Methode und der am weitesten abgeleiteten Implementierung der `M` in Bezug auf `R` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="cfde9-883">Für jede virtuelle Methode von einer Klasse deklariert oder geerbt, vorhanden ist eine ***am weitesten abgeleiteten Implementierung*** der Methode in Bezug auf die Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="cfde9-884">Die am weitesten abgeleiteten Implementierung einer virtuellen Methode `M` in Bezug auf eine Klasse `R` wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="cfde9-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="cfde9-885">Wenn `R` enthält, die Einführung von `virtual` Deklaration `M`, ist dies der am weitesten abgeleiteten Implementierung der `M`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="cfde9-886">Andernfalls gilt: Wenn `R` enthält ein `override` von `M`, ist dies der am weitesten abgeleiteten Implementierung der `M`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="cfde9-887">Andernfalls am meisten Implementierung der abgeleiteten der `M` in Bezug auf `R` ist identisch mit den am weitesten abgeleiteten Implementierung der `M` in Bezug auf die direkte Basisklasse von `R`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="cfde9-888">Im folgende Beispiel werden die Unterschiede zwischen virtuellen und nicht virtuelle Methoden veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="cfde9-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

<span data-ttu-id="cfde9-889">Im Beispiel `A` führt eine nicht virtuelle Methode `F` und eine virtuelle Methode `G`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="cfde9-890">Die Klasse `B` führt eine neue nicht virtuelle Methode `F`, daher durch das Ausblenden der geerbten `F`, und außerdem überschreibt die geerbte Methode `G`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="cfde9-891">Das Beispiel erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="cfde9-891">The example produces the output:</span></span>
```
A.F
B.F
B.G
B.G
```

<span data-ttu-id="cfde9-892">Beachten Sie, dass die Anweisung `a.G()` ruft `B.G`, nicht `A.G`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="cfde9-893">Dies ist, da der Laufzeittyp der Instanz (d.h. `B`), nicht der Kompilierzeit-Typ der Instanz (d.h. `A`), bestimmt die Implementierung der tatsächlichen Methode zum Aufrufen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="cfde9-894">Da die Methoden zum Ausblenden von geerbter Methoden zulässig sind, ist es möglich, eine Klasse kann mehrere virtuelle Methoden mit derselben Signatur enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="cfde9-895">Dies ist keiner Mehrdeutigkeiten, da alle bis auf die am häufigsten abgerufene Methode ausgeblendet sind.</span><span class="sxs-lookup"><span data-stu-id="cfde9-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="cfde9-896">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-896">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
<span data-ttu-id="cfde9-897">die `C` und `D` Klassen enthalten, zwei virtuelle Methoden, mit der gleichen Signatur: Der eingeschleuste `A` und der eingeschleuste `C`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="cfde9-898">Die Methode, die von eingeführte `C` Blendet die geerbte Methode `A`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="cfde9-899">Daher die außer Kraft setzen-Deklaration in `D` überschreibt die Methode, die von eingeführt `C`, und es ist nicht möglich, dass `D` , die Methode eingeführt, durch Überschreiben `A`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="cfde9-900">Das Beispiel erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="cfde9-900">The example produces the output:</span></span>
```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="cfde9-901">Beachten Sie, dass es möglich ist, rufen Sie die virtuelle Methode die ausgeblendete durch den Zugriff auf eine Instanz von `D` über einen weniger abgeleiteten Typ, die in der die Methode nicht ausgeblendet ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="cfde9-902">Methoden außer Kraft setzen</span><span class="sxs-lookup"><span data-stu-id="cfde9-902">Override methods</span></span>

<span data-ttu-id="cfde9-903">Wenn eine Instanzmethodendeklaration enthält ein `override` Modifizierer, die Methode gilt eine ***Überschreibungsmethode***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="cfde9-904">Eine Überschreibungsmethode überschreibt eine geerbte virtuelle Methode mit der gleichen Signatur.</span><span class="sxs-lookup"><span data-stu-id="cfde9-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="cfde9-905">Während eine Deklaration einer virtuellen Methode eine neue Methode einführt, spezialisiert eine Deklaration einer überschriebenen Methode eine vorhandene geerbte virtuelle Methode, indem eine neue Implementierung dieser Methode bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="cfde9-906">Die Methode überschrieben, indem ein `override` Deklaration wird als bezeichnet die ***Basismethode überschreiben***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="cfde9-907">Für eine Überschreibungsmethode `M` in einer Klasse deklarierten `C`, die überschriebene Basismethode richtet sich nach der Untersuchung jeder Typ von der Basisklasse `C`, beginnend mit dem direkten Basisklasse `C` und mit jedem aufeinander folgenden direkte Basisklasse-Typ, bis in einem angegebenen Basisklassentyp mindestens eine zugängliche Methode ist die befindet, hat die gleiche Signatur wie `M` nach dem Ersetzen der Typargumente.</span><span class="sxs-lookup"><span data-stu-id="cfde9-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="cfde9-908">Im Rahmen, suchen die überschriebene Basismethode, eine Methode wird als verfügbar betrachtet, ist dies `public`, sofern sie `protected`, sofern sie `protected internal`, oder es ist `internal` und im selben Programm als deklariert `C`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="cfde9-909">Ein Fehler während der Kompilierung tritt auf, es sei denn, alle der folgenden "true" für eine Deklaration außer Kraft setzen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="cfde9-910">Eine überschriebene Basismethode kann gefunden werden, wie oben beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="cfde9-911">Es gibt genau einen solchen überschriebene Basismethode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="cfde9-912">Diese Einschränkung wirkt sich nur, wenn Sie der Basisklassentyp ein konstruierter Typ ist, macht die Ersetzung der Argumente des Typs der Signatur der beiden Methoden identisch.</span><span class="sxs-lookup"><span data-stu-id="cfde9-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="cfde9-913">Die überschriebene Basismethode ist einer virtuell, abstrakt oder Überschreiben der Methode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="cfde9-914">Die überschriebene Basismethode darf nicht in anderen Worten: statisch oder nicht virtuell sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="cfde9-915">Die überschriebene Basismethode ist keine versiegelten Methode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="cfde9-916">Die Methode zum Überschreiben und die überschriebene Basismethode haben den gleichen Rückgabetyp auf.</span><span class="sxs-lookup"><span data-stu-id="cfde9-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="cfde9-917">Die Deklaration außer Kraft setzen und die überschriebene Basismethode haben die gleiche deklarierte Zugriffsart auf.</span><span class="sxs-lookup"><span data-stu-id="cfde9-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="cfde9-918">Eine außer Kraft setzen-Deklaration kann nicht in anderen Worten, den Zugriff auf die virtuelle Methode ändern.</span><span class="sxs-lookup"><span data-stu-id="cfde9-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="cfde9-919">Allerdings muss, wenn die überschriebene Basismethode wird intern geschützt, und sie in einer anderen Assembly deklariert wird, als die Assembly mit der Methode außer Kraft setzen und dann der Überschreibungsmethode deklariert Zugriff geschützt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="cfde9-920">Die außer Kraft setzen-Deklaration ist nicht mit Typ-Parameter-Einschränkungen-Klauseln angegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="cfde9-921">Stattdessen werden die Einschränkungen von die überschriebene Basismethode geerbt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="cfde9-922">Beachten Sie, dass die Einschränkungen, die Typparameter in der überschriebenen Methode sind in der geerbten Einschränkung durch Typargumente ersetzt werden können.</span><span class="sxs-lookup"><span data-stu-id="cfde9-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="cfde9-923">Dies kann zu Einschränkungen führen, die nicht gültig, wenn explizit angegeben, z. B. Werttypen oder versiegelte Typen sind.</span><span class="sxs-lookup"><span data-stu-id="cfde9-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="cfde9-924">Das folgende Beispiel zeigt, wie die überschreibenden Regeln für generische Klassen funktionieren:</span><span class="sxs-lookup"><span data-stu-id="cfde9-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

<span data-ttu-id="cfde9-925">Eine Deklaration für die Außerkraftsetzung stehen die überschriebene Basismethode mithilfe einer *Base_access* ([Base Access](expressions.md#base-access)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="cfde9-926">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-926">In the example</span></span>
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
<span data-ttu-id="cfde9-927">die `base.PrintFields()` Aufruf im `B` Ruft die `PrintFields` Methode deklariert `A`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="cfde9-928">Ein *Base_access* den virtuellen Aufruf-Mechanismus deaktiviert und einfach die Basismethode behandelt, als eine nicht virtuelle Methode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="cfde9-929">Hatte den Aufruf im `B` wurde geschrieben `((A)this).PrintFields()`, wäre Sie rekursiv Aufrufen der `PrintFields` Methode deklariert werden, `B`, nicht zu derjenigen, die in deklariert `A`, da `PrintFields` ist virtuell und den Laufzeittyp der `((A)this)` ist `B`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="cfde9-930">Nur durch Einfügen einer `override` Modifizierer kann eine Methode eine andere Methode zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="cfde9-931">In allen anderen Fällen wird eine Methode mit der gleichen Signatur wie eine geerbte Methode einfach die geerbte Methode ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="cfde9-932">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-932">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
<span data-ttu-id="cfde9-933">die `F` -Methode in der `B` enthält kein `override` Modifizierer und wird daher nicht überschrieben der `F` -Methode in der `A`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="cfde9-934">Vielmehr die `F` -Methode in der `B` Blendet die Methode in `A`, und eine Warnung wird ausgegeben, da die Deklaration keine umfasst eine `new` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="cfde9-935">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-935">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
<span data-ttu-id="cfde9-936">die `F` -Methode in der `B` verbirgt den virtuellen `F` Methode geerbt von `A`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="cfde9-937">Das neue `F` in `B` privaten Zugriff, der Bereich enthält nur die Klassendefinition von `B` und erstreckt sich nicht um `C`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="cfde9-938">Daher ist die Deklaration der `F` in `C` ist zulässig, außer Kraft setzen der `F` vererbt `A`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="cfde9-939">Versiegelte Methoden</span><span class="sxs-lookup"><span data-stu-id="cfde9-939">Sealed methods</span></span>

<span data-ttu-id="cfde9-940">Wenn eine Instanzmethodendeklaration enthält eine `sealed` -Modifizierer ist, wird diese Methode wird als bezeichnet ein ***versiegelt Methode***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="cfde9-941">Wenn eine Instanzmethodendeklaration enthält die `sealed` Modifizierer verwenden, muss auch enthalten die `override` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="cfde9-942">Verwenden der `sealed` -Modifizierer wird verhindert, dass eine abgeleitete Klasse weiter durch Überschreiben der Methode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="cfde9-943">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-943">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
<span data-ttu-id="cfde9-944">die Klasse `B` bietet zwei Methoden außer Kraft setzen: eine `F` Methode, die `sealed` Modifizierer und eine `G` -Methode, die nicht der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="cfde9-945">`B`die Nutzung von der versiegelten `modifier` wird verhindert, dass `C` weiter überschreiben `F`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="cfde9-946">Abstrakte Methoden</span><span class="sxs-lookup"><span data-stu-id="cfde9-946">Abstract methods</span></span>

<span data-ttu-id="cfde9-947">Wenn eine Instanzmethodendeklaration enthält ein `abstract` -Modifizierer ist, wird diese Methode wird als bezeichnet ein ***abstrakte Methode***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="cfde9-948">Obwohl eine abstrakte Methode implizit auch eine virtuelle Methode ist, können keine Modifizierer besitzen `virtual`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="cfde9-949">Eine abstrakte Methodendeklaration führt eine neue virtuelle Methode, aber es bietet eine Implementierung dieser Methode keine.</span><span class="sxs-lookup"><span data-stu-id="cfde9-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="cfde9-950">Stattdessen müssen nicht abstrakte abgeleitete Klassen ihre eigene Implementierung bereitstellen, indem Sie diese Methode überschreiben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="cfde9-951">Da eine abstrakte Methode keine Implementierungen, bietet der *Method_body* einer abstrakten Methode einfach besteht aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="cfde9-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="cfde9-952">Abstrakte Methodendeklarationen sind nur in abstrakten Klassen zulässig ([abstrakte Klassen](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="cfde9-953">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-953">In the example</span></span>
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
<span data-ttu-id="cfde9-954">die `Shape` -Klasse definiert die abstrakte Vorstellung einer geometrischen Formobjekt, das sich selbst zu zeichnen kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="cfde9-955">Die `Paint` Methode ist abstrakt, da es keine sinnvolle Standardimplementierung ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="cfde9-956">Die `Ellipse` und `Box` Klassen sind konkrete `Shape` Implementierungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="cfde9-957">Da diese Klassen abstrakt sind, müssen sie außer Kraft setzen der `Paint` Methode und eine tatsächliche Implementierung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="cfde9-958">Es ist ein Fehler während der Kompilierung für eine *Base_access* ([Base Access](expressions.md#base-access)) auf eine abstrakte Methode zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="cfde9-959">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-959">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
<span data-ttu-id="cfde9-960">ein Kompilierung ein Fehler wird gemeldet, für die `base.F()` aufrufen, da es sich um eine abstrakte Methode verweist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="cfde9-961">Eine abstrakte Methodendeklaration ist zulässig, um eine virtuelle Methode zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="cfde9-962">Dadurch können eine abstrakte Klasse, die erneute Implementierung der Methode in abgeleiteten Klassen zu erzwingen, und die ursprüngliche Implementierung der Methode nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="cfde9-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="cfde9-963">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-963">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
<span data-ttu-id="cfde9-964">Klasse `A` deklariert eine virtuelle Methode, Klasse `B` überschreibt diese Methode mit dem eine abstrakte Methode, und die Klasse `C` überschreibt die abstrakte Methode, um eine eigene Implementierung bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="cfde9-965">Externe Methoden</span><span class="sxs-lookup"><span data-stu-id="cfde9-965">External methods</span></span>

<span data-ttu-id="cfde9-966">Wenn eine Methodendeklaration enthält ein `extern` -Modifizierer ist, wird diese Methode wird als bezeichnet ein ***externe Methode***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="cfde9-967">Externe Methoden werden extern implementiert, in der Regel mit einer anderen Sprache als C# -Code.</span><span class="sxs-lookup"><span data-stu-id="cfde9-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="cfde9-968">Da eine externe Methodendeklaration keine Implementierungen, bietet der *Method_body* besteht aus einer externen Methode einfach aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="cfde9-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="cfde9-969">Eine externe Methode kann nicht generisch sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-969">An external method may not be generic.</span></span>

<span data-ttu-id="cfde9-970">Die `extern` Modifizierer wird normalerweise verwendet, in Verbindung mit einer `DllImport` Attribut ([Interoperation mit COM- und Win32-Komponenten](attributes.md#interoperation-with-com-and-win32-components)), können externe Methoden von DLLs (Dynamic Link Libraries) implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="cfde9-971">Die ausführungsumgebung möglicherweise andere Mechanismen unterstützt durch Implementierungen der externe Methoden bereitgestellt werden kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="cfde9-972">Wenn eine externe Methode enthält einen `DllImport` -Attribut, die Deklaration der Methode muss auch enthalten eine `static` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="cfde9-973">Dieses Beispiel veranschaulicht die Verwendung von der `extern` Modifizierer und der `DllImport` Attribut:</span><span class="sxs-lookup"><span data-stu-id="cfde9-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a><span data-ttu-id="cfde9-974">Partielle Methoden (Zusammenfassung)</span><span class="sxs-lookup"><span data-stu-id="cfde9-974">Partial methods (recap)</span></span>

<span data-ttu-id="cfde9-975">Wenn die Deklaration einer Methode enthält einen `partial` -Modifizierer ist, wird diese Methode wird als bezeichnet ein ***partielle Methode***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="cfde9-976">Partielle Methoden können nur Mitglieder der partiellen Typen deklariert werden ([partielle Typen](classes.md#partial-types)), und eine Reihe von Einschränkungen unterliegen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="cfde9-977">Partielle Methoden ausführlicher beschrieben werden [partielle Methoden](classes.md#partial-methods).</span><span class="sxs-lookup"><span data-stu-id="cfde9-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="cfde9-978">Erweiterungsmethoden</span><span class="sxs-lookup"><span data-stu-id="cfde9-978">Extension methods</span></span>

<span data-ttu-id="cfde9-979">Wenn der erste Parameter einer Methode enthält die `this` -Modifizierer ist, wird diese Methode wird als bezeichnet ein ***Erweiterungsmethode***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="cfde9-980">Erweiterungsmethoden können nur in nicht-generische, nicht geschachtelten statischen Klassen deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="cfde9-981">Der erste Parameter einer Erweiterungsmethode haben keine Modifizierer außer `this`, und der Parametertyp kann kein Zeigertyp sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="cfde9-982">Im folgenden finden ein Beispiel für eine statische Klasse, die zwei Erweiterungsmethoden deklariert:</span><span class="sxs-lookup"><span data-stu-id="cfde9-982">The following is an example of a static class that declares two extension methods:</span></span>
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

<span data-ttu-id="cfde9-983">Eine Erweiterungsmethode ist eine normale statische Methode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-983">An extension method is a regular static method.</span></span> <span data-ttu-id="cfde9-984">Darüber hinaus im Gültigkeitsbereich der einschließenden statische Klasse ist, eine Erweiterungsmethode kann aufgerufen werden mithilfe einer Instanzmethodensyntax Aufruf ([Erweiterung Methodenaufrufe](expressions.md#extension-method-invocations)), mit dem Empfänger-Ausdruck als erstes Argument.</span><span class="sxs-lookup"><span data-stu-id="cfde9-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="cfde9-985">Das folgende Programm verwendet die Erweiterungsmethoden, die oben deklariert:</span><span class="sxs-lookup"><span data-stu-id="cfde9-985">The following program uses the extension methods declared above:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

<span data-ttu-id="cfde9-986">Die `Slice` Methode steht für die `string[]`, und die `ToInt32` Methode steht für `string`, da sie als Erweiterungsmethoden deklariert wurden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="cfde9-987">Die Bedeutung des Programms ist identisch mit den folgenden, wobei normale statische Methodenaufrufen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a><span data-ttu-id="cfde9-988">Methodentext</span><span class="sxs-lookup"><span data-stu-id="cfde9-988">Method body</span></span>

<span data-ttu-id="cfde9-989">Die *Method_body* Deklaration einer Methode besteht aus einer Blocktext, einen ausdruckskörper oder ein Semikolon.</span><span class="sxs-lookup"><span data-stu-id="cfde9-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="cfde9-990">Die ***Ergebnistyp*** einer Methode ist `void` ist der Rückgabetyp `void`, oder wenn die Methode asynchron ist und der Rückgabetyp ist `System.Threading.Tasks.Task`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="cfde9-991">Andernfalls ist der Ergebnistyp einer nicht-Async-Methode, der Rückgabetyp und dem Rückgabetyp einer Async-Methode mit Rückgabetyp `System.Threading.Tasks.Task<T>` ist `T`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="cfde9-992">Wenn eine Methode verfügt über eine `void` dazu führen, Typ und einem Blocktext `return` Anweisungen ([die return-Anweisung](statements.md#the-return-statement)) im-Block dürfen nicht auf einen Ausdruck angeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="cfde9-993">Wenn die Ausführung des Blocks einer "void" Methode normal abgeschlossen wird (d. h. Kontrollflüsse am Ende des Methodentexts), dass die Methode einfach an den aktuellen Aufrufer zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="cfde9-994">Wenn eine Methode verfügt über eine `void` Ergebnis und einen ausdruckskörper, den Ausdruck `E` muss eine *Statement_expression*, und der Text entspricht genau einen Blocktext des Formulars `{ E; }`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="cfde9-995">Wenn eine Methode einen für nicht-Void-Ergebnistyp und einem Block Anforderungstext, jede `return` -Anweisung in den Block muss einen Ausdruck, der implizit in den Ergebnistyp konvertiert werden angeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="cfde9-996">Der Endpunkt eines Block-Texts, der eine Methode einen Wert zurückgibt, darf nicht erreichbar sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="cfde9-997">Das heißt, in einer Methode Wertrückgabe mit einem Blocktext von Steuerelement darf nicht über das Ende des Methodentexts hinausgehen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="cfde9-998">Wenn eine Methode ein für nicht-Void-Ergebnistyp und einen ausdruckskörper hat, der Ausdruck implizit in den Ergebnistyp konvertiert werden muss und der Text genau einen Blocktext des Formulars entspricht `{ return E; }`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="cfde9-999">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-999">In the example</span></span>
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
<span data-ttu-id="cfde9-1000">die Wertrückgabe `F` Methode führt ein Fehler während der Kompilierung, da das Ende des Methodentexts ablaufsteuerung können.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="cfde9-1001">Die `G` und `H` Methoden richtig sind, da sämtliche möglichen ausführungswege eine return-Anweisung enden, der angibt, einen Wert zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="cfde9-1002">Die `I` Methode korrekt ist, da Text einen Anweisungsblock mit nur eine einzelne rückgabeanweisung darin entspricht.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="cfde9-1003">Methodenüberladung</span><span class="sxs-lookup"><span data-stu-id="cfde9-1003">Method overloading</span></span>

<span data-ttu-id="cfde9-1004">Die Regeln der überladungsauflösung Methode werden in beschrieben [Typrückschluss](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="cfde9-1005">Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="cfde9-1005">Properties</span></span>

<span data-ttu-id="cfde9-1006">Ein ***Eigenschaft*** ist ein Element, das Zugriff auf ein Merkmal eines Objekts oder einer Klasse bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="cfde9-1007">Beispiele für Eigenschaften enthalten die Länge einer Zeichenfolge, die Größe einer Schriftart, die Titelleiste eines Fensters, den Namen eines Kunden und so weiter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="cfde9-1008">Eigenschaften sind eine natürliche Erweiterung von Feldern, beide sind benannte Member mit zugeordneten Typen, und die Syntax für den Zugriff auf Felder und Eigenschaften entspricht.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="cfde9-1009">Im Gegensatz zu Feldern bezeichnen Eigenschaften jedoch keine Speicherorte.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="cfde9-1010">Stattdessen verfügen Eigenschaften über ***Accessors*** zum Angeben der Anweisungen, die beim Lesen oder Schreiben ihrer Werte ausgeführt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="cfde9-1011">Eigenschaften stellen somit einen Mechanismus zum Zuordnen von Aktionen mit dem Lesen und Schreiben der Attribute eines Objekts bereit; Außerdem ermöglichen sie solche Attribute berechnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="cfde9-1012">Eigenschaften werden deklariert, die mit *Property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1012">Properties are declared using *property_declaration*s:</span></span>

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

<span data-ttu-id="cfde9-1013">Ein *Property_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer ](classes.md#access-modifiers)), wird die `new` ([der new-Modifizierer](classes.md#the-new-modifier)), `static` ([statische und Instanzmethoden](classes.md#static-and-instance-methods)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Methoden außer Kraft setzen](classes.md#override-methods)), `sealed` ([Versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakten Methoden](classes.md#abstract-methods)), und `extern` ([Externe Methoden](classes.md#external-methods)) Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="cfde9-1014">Eigenschaftendeklarationen gelten dieselben Regeln wie Methodendeklarationen ([Methoden](classes.md#methods)) im Hinblick auf die gültigen Kombinationen der Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="cfde9-1015">Die *Typ* Deklaration gibt den Typ der Eigenschaft, der durch die Deklaration einer Eigenschaft an und *Member_name* gibt den Namen der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="cfde9-1016">Wenn die Eigenschaft eine explizite Schnittstellenmember-Implementierung, ist die *Member_name* ist einfach ein *Bezeichner*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="cfde9-1017">Für eine explizite Schnittstellenmember-Implementierung ([explizite Implementierungen eines Schnittstellenmembers](interfaces.md#explicit-interface-member-implementations)), wird die *Member_name* besteht aus einer *Interface_type* gefolgt von einer " `.`"und ein *Bezeichner*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="cfde9-1018">Die *Typ* einer Eigenschaft muss mindestens dieselben zugriffsmöglichkeiten bieten wie die Eigenschaft selbst ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cfde9-1019">Ein *Property_body* entweder besteht möglicherweise aus einem ***Accessor-Body*** oder ***ausdruckskörper***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="cfde9-1020">In einem Accessortext *Accessor_declarations*, die eingeschlossen werden müssen, "`{`"und"`}`" Token, deklarieren die Accessoren ([Accessoren](classes.md#accessors)) der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="cfde9-1021">Die Accessoren angeben, die ausführbaren Anweisungen lesen und schreiben die Eigenschaft zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="cfde9-1022">Ein ausdruckskörper bestehend aus `=>` gefolgt von einer *Ausdruck* `E` und ein Semikolon entspricht genau der Anweisungstext `{ get { return E; } }`, und kann daher nur nur-Getter an verwendet werden Eigenschaften, in dem das Ergebnis der getter-Methode von einem einzelnen Ausdruck angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="cfde9-1023">Ein *Property_initializer* kann nur für eine automatisch implementierte Eigenschaft zugewiesen werden ([automatisch implementierten Eigenschaften](classes.md#automatically-implemented-properties)), und bewirkt, dass die Initialisierung des zugrunde liegenden Felds solcher Eigenschaften mit dem angegebenen Wert der *Ausdruck*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="cfde9-1024">Obwohl die Syntax für den Zugriff auf eine Eigenschaft, die für ein Feld übereinstimmt, wird eine Eigenschaft nicht als Variable klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="cfde9-1025">Es ist daher nicht möglich, eine Eigenschaft als übergeben eine `ref` oder `out` Argument.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="cfde9-1026">Wenn eine Eigenschaftendeklaration enthält ein `extern` Modifizierer, die Eigenschaft gilt eine ***Eigenschaft "external"***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="cfde9-1027">Da eine Deklaration der Eigenschaft "external" keine Implementierung, jede bietet die *Accessor_declarations* besteht aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="cfde9-1028">Statische und Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="cfde9-1028">Static and instance properties</span></span>

<span data-ttu-id="cfde9-1029">Wenn eine Eigenschaftendeklaration enthält eine `static` Modifizierer, die Eigenschaft gilt eine ***statische Eigenschaft***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="cfde9-1030">Wenn kein `static` Modifizierer vorhanden ist, wird die Eigenschaft gilt eine ***-Instanzeigenschaft***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="cfde9-1031">Eine statische Eigenschaft ist nicht mit einer bestimmten Instanz verknüpft, und es ist ein Fehler während der Kompilierung zum Verweisen auf `this` in den Accessoren für eine statische Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="cfde9-1032">Eine Instance-Eigenschaft ist eine bestimmte Instanz einer Klasse zugeordnet, und diese Instanz zugegriffen werden kann, als `this` ([diesen Zugriff](expressions.md#this-access)) in den Accessoren der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="cfde9-1033">Wenn eine Eigenschaft verwiesen wird, einem *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `E.M`, wenn `M` ist eine statische Eigenschaft, `E` muss einen Typ mit deuten`M`, und wenn `M` ist eine Instanzeigenschaft E muss eine Instanz von einem Typ mit deuten `M`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="cfde9-1034">Die Unterschiede zwischen statischen und Instanzmember finden Sie weiter unten in [statische und Instanzmember](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="cfde9-1035">Zugriffsmethoden</span><span class="sxs-lookup"><span data-stu-id="cfde9-1035">Accessors</span></span>

<span data-ttu-id="cfde9-1036">Die *Accessor_declarations* Geben Sie eine Eigenschaft der ausführbaren Anweisungen lesen und Schreiben von dieser Eigenschaft zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="cfde9-1037">Die Accessordeklarationen bestehen aus einer *Get_accessor_declaration*, *Set_accessor_declaration*, oder beides.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="cfde9-1038">Jede Zugriffsmethoden-Deklaration besteht aus dem Token `get` oder `set` gefolgt von einem optionalen *Accessor_modifier* und *Accessor_body*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="cfde9-1039">Die Verwendung von *Accessor_modifier*s unterliegt folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="cfde9-1040">Ein *Accessor_modifier* kann nicht in einer Schnittstelle oder in eine explizite Schnittstellenmember-Implementierung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="cfde9-1041">Für eine Eigenschaft oder einen Indexer, der keine `override` Modifizierer, ein *Accessor_modifier* ist zulässig, nur, wenn die Eigenschaft oder der Indexer sowohl einen `get` und `set` -Accessor, und klicken Sie dann nur für eine solche darf Accessoren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="cfde9-1042">Für eine Eigenschaft oder einen Indexer, die enthält ein `override` Modifizierer, ein Accessor muss übereinstimmen. die *Accessor_modifier*, falls vorhanden, von der Zugriffsmethode, die überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="cfde9-1043">Die *Accessor_modifier* muss einen Zugriff, die streng restriktiver ist als die deklarierte Zugriffsart von Eigenschaft oder des Indexers selbst deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="cfde9-1044">Um genau zu sein:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1044">To be precise:</span></span>
   * <span data-ttu-id="cfde9-1045">Wenn die Eigenschaft oder des Indexers eine deklarierte Zugriffsart von ist `public`, *Accessor_modifier* entweder `protected internal`, `internal`, `protected`, oder `private`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="cfde9-1046">Wenn die Eigenschaft oder des Indexers eine deklarierte Zugriffsart von ist `protected internal`, *Accessor_modifier* entweder `internal`, `protected`, oder `private`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="cfde9-1047">Wenn die Eigenschaft oder des Indexers eine deklarierte Zugriffsart von ist `internal` oder `protected`, *Accessor_modifier* muss `private`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="cfde9-1048">Wenn die Eigenschaft oder des Indexers eine deklarierte Zugriffsart von ist `private`, keine *Accessor_modifier* kann verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="cfde9-1049">Für `abstract` und `extern` Eigenschaften, die *Accessor_body* für jeden Accessor angegeben einfach ein Semikolon ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="cfde9-1050">Eine nicht abstrakte, nicht Extern Eigenschaft möglicherweise jede *Accessor_body* können Sie ein Semikolon, in diesem Fall ist es ein ***automatisch implementierte Eigenschaft*** ([automatisch implementierte Eigenschaften ](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="cfde9-1051">Eine automatisch implementierte Eigenschaft müssen mindestens einen Get-Accessor.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="cfde9-1052">Für den Accessoren für eine andere nicht abstrakten, nicht Extern Eigenschaft die *Accessor_body* ist eine *Block* der gibt an, die Anweisungen ausgeführt werden, wenn der entsprechende-Accessor aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="cfde9-1053">Ein `get` -Accessor entspricht einer Methode ohne Parameter mit einem Rückgabewert des Eigenschaftstyps.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="cfde9-1054">Mit Ausnahme der als Ziel einer Zuweisung, wenn eine Eigenschaft in einem Ausdruck verwiesen wird die `get` -Accessor der Eigenschaft wird aufgerufen, um den Wert der Eigenschaft zu berechnen ([Werte Ausdrücke](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="cfde9-1055">Der Text, der eine `get` Accessor entsprechen den Regeln für die Wertrückgabe beschriebenen Methoden [Methodentext](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="cfde9-1056">Insbesondere alle `return` Anweisungen im Text einer `get` Accessor muss einen Ausdruck implizit in den Eigenschaftentyp angeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="cfde9-1057">Darüber hinaus den Endpunkt des eine `get` Accessor nicht erreicht werden kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="cfde9-1058">Ein `set` Accessor entspricht einer Methode mit einem einzelnen Werteparameter des Eigenschaftstyps und `void` Typ zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="cfde9-1059">Der implizite Parameter eines eine `set` Accessor wird stets der Name `value`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="cfde9-1060">Wenn eine Eigenschaft verwiesen wird, als Ziel einer Zuweisung ([Zuweisungsoperatoren](expressions.md#assignment-operators)), oder als Operand `++` oder `--` ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators), [ Präfix-Inkrement und Dekrement-Operatoren](expressions.md#prefix-increment-and-decrement-operators)), wird die `set` -Accessor wird aufgerufen, mit einem Argument (, deren Wert ist, die von der rechten Seite der Zuweisung oder der Operand des der `++` oder `--` Operator), Gibt den neuen Wert ([einfache Zuweisung](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="cfde9-1061">Der Text, der eine `set` Accessor entsprechen den Regeln für `void` beschriebenen Methoden [Methodentext](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="cfde9-1062">Insbesondere `return` Anweisungen in der `set` Accessor-Body sind nicht zulässig, einen Ausdruck angeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="cfde9-1063">Da eine `set` Accessor weist implizit einen Parameter namens `value`, es ist ein Fehler während der Kompilierung für die Deklaration einer lokalen Variable oder Konstante in einen `set` Accessor diesen Namen vor.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="cfde9-1064">Basierend auf das Vorhandensein oder fehlen von der `get` und `set` Accessoren, eine Eigenschaft wird wie folgt klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="cfde9-1065">Eine Eigenschaft, die beides umfasst eine `get` Accessor und einen `set` Accessor gilt eine ***Lese-/ Schreibzugriff*** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="cfde9-1066">Eine Eigenschaft, die nur eine `get` Accessor gilt eine ***schreibgeschützte*** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="cfde9-1067">Es ist ein Fehler während der Kompilierung für eine schreibgeschützte Eigenschaft, die das Ziel einer Zuweisung sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="cfde9-1068">Eine Eigenschaft, die nur eine `set` Accessor gilt eine ***lesegeschützte*** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="cfde9-1069">Mit der Ausnahme als Ziel einer Zuweisung, es einen Fehler während der Kompilierung, um eine Nur-Schreiben-Eigenschaft in einem Ausdruck zu verweisen ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="cfde9-1070">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1070">In the example</span></span>
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
<span data-ttu-id="cfde9-1071">die `Button` Steuerelement deklariert ein öffentliches `Caption` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="cfde9-1072">Die `get` Accessor die `Caption` Eigenschaft gibt die Zeichenfolge, die in der privaten gespeicherten `caption` Feld.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="cfde9-1073">Die `set` Accessor überprüft, ob der neue Wert unterscheidet sich von den aktuellen Wert aus, und wenn dies der Fall ist, es den neuen Wert speichert und wird neu aufgebaut, das Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="cfde9-1074">Eigenschaften führen Sie oft auf das oben gezeigte Muster aus: Die `get` Accessor gibt einfach einen Wert, der in einem privaten Feld gespeichert und die `set` Accessor ändert das private Feld und führt dann zusätzlichen Aktionen erforderlich, um den Zustand des Objekts vollständig zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="cfde9-1075">Erhält die `Button` oben gezeigte Klasse, die folgenden ist ein Beispiel mit der `Caption` Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="cfde9-1076">Hier ist die `set` -Accessor wird aufgerufen, durch das Zuweisen eines Werts der Eigenschaft und die `get` Accessor wird aufgerufen, indem Sie auf die Eigenschaft in einem Ausdruck verweisen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="cfde9-1077">Die `get` und `set` Accessor einer Eigenschaft sind nicht unterschiedliche Elemente aus, und es ist nicht möglich, um die Accessoren der Eigenschaft separat zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="cfde9-1078">Daher ist es nicht möglich, für die beiden Accessoren Schreib-Lese-Eigenschaft auf unterschiedlichen Zugriff haben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="cfde9-1079">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1079">The example</span></span>
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
<span data-ttu-id="cfde9-1080">eine einzelne Lese-/ Schreibzugriff-Eigenschaft ist nicht deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="cfde9-1081">Stattdessen deklariert zwei Eigenschaften mit dem gleichen Namen, eine schreibgeschützte und lesegeschützte.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="cfde9-1082">Da zwei Elemente, die in der gleichen Klasse deklariert den gleichen Namen haben können, wird im Beispiel wird einen Fehler während der Kompilierung auftreten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="cfde9-1083">Wenn eine abgeleitete Klasse eine Eigenschaft mit dem gleichen Namen wie eine geerbte Eigenschaft deklariert, blendet die abgeleitete Eigenschaft die geerbte Eigenschaft in Bezug auf das Lesen und schreiben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="cfde9-1084">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1084">In the example</span></span>
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
<span data-ttu-id="cfde9-1085">die `P` -Eigenschaft in `B` Blendet die `P` -Eigenschaft in `A` in Bezug auf das Lesen und schreiben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="cfde9-1086">Daher in den Anweisungen</span><span class="sxs-lookup"><span data-stu-id="cfde9-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="cfde9-1087">die Zuweisung zu `b.P` verursacht einen Fehler während der Kompilierung gemeldet werden, da die schreibgeschützte `P` -Eigenschaft in `B` Blendet Sie aus, die nur-schreiben- `P` -Eigenschaft in `A`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="cfde9-1088">Beachten Sie jedoch, dass eine Umwandlung verwendet werden kann, auf die ausgeblendeten `P` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="cfde9-1089">Geben Sie im Gegensatz zu öffentlichen Feldern Eigenschaften eine Trennung zwischen internen Zustand eines Objekts und dessen öffentliche Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="cfde9-1090">Betrachten Sie das Beispiel aus:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1090">Consider the example:</span></span>
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="cfde9-1091">Hier ist die `Label` -Klasse verwendet zwei `int` Felder `x` und `y`, um den Speicherort zu speichern.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="cfde9-1092">Der Speicherort ist öffentlich verfügbar gemacht, als ein `X` und `Y` Eigenschaft und als eine `Location` Eigenschaft vom Typ `Point`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="cfde9-1093">Im Rahmen einer zukünftigen Version von `Label`, wird es einfacher, den Ort als eine `Point` intern für die Änderung vorgenommen werden kann, ohne Auswirkungen auf die öffentliche Schnittstelle der Klasse:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="cfde9-1094">Hatte `x` und `y` wurde stattdessen `public readonly` Felder, war es unmöglich, solche eine Änderung an der `Label` Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="cfde9-1095">Zustand durch Eigenschaften verfügbar zu machen, ist nicht unbedingt weniger effizient als Felder direkt verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="cfde9-1096">Insbesondere, wenn eine Eigenschaft nicht virtuell ist und nur eine kleine Menge an Code enthält, kann die ausführungsumgebung Aufrufe von Accessoren mit den eigentlichen Code der Accessoren ersetzen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="cfde9-1097">Dieser Prozess wird als bezeichnet ***inlining***, und es wird der Zugriff auf Eigenschaften so effizient wie Feldzugriff, jedoch behält die höhere Flexibilität von Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="cfde9-1098">Seit dem Aufrufen einer `get` Accessor Konzept entspricht dem Lesen des Werts eines Felds, gilt dies Unzulässiger Programmierstil für `get` Accessoren, um sichtbare Nebeneffekte haben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="cfde9-1099">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="cfde9-1100">Der Wert des der `Next` Eigenschaft hängt die Anzahl der Male, die die Eigenschaft zuvor zugegriffen wurde.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="cfde9-1101">Klicken Sie daher Zugriff auf die Eigenschaft einen Observable Nebeneffekt erzeugt, und die Eigenschaft sollte stattdessen als eine Methode implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="cfde9-1102">Die "keine Nebeneffekte" Konvention für `get` Accessoren nicht bedeuten, dass `get` Accessoren sollte stets so verfasst werden einfach in Feldern gespeicherte Werte zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="cfde9-1103">In der Tat `get` Accessoren häufig berechnen Sie den Wert einer Eigenschaft, indem Sie den Zugriff auf mehrere Felder oder Methoden aufrufen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="cfde9-1104">Allerdings ein ordnungsgemäß entwickeltes `get` Accessor führt keine Aktionen, die dazu führen, wahrnehmbare Änderungen in den Zustand des Objekts dass.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="cfde9-1105">Eigenschaften können verwendet werden, um die Initialisierung einer Ressource bis zum Moment zu verzögern, die es erstmalig verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="cfde9-1106">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1106">For example:</span></span>
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

<span data-ttu-id="cfde9-1107">Die `Console` -Klasse enthält drei Eigenschaften `In`, `Out`, und `Error`, die Eingabe, Ausgabe und Fehler Geräten bzw. darstellen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="cfde9-1108">Durch diese Member als Eigenschaften verfügbar macht, die `Console` Klasse kann die Initialisierung verzögert, bis sie tatsächlich verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="cfde9-1109">Beispiel: nach dem ersten Verweis auf die `Out` Eigenschaft, wie in</span><span class="sxs-lookup"><span data-stu-id="cfde9-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="cfde9-1110">die zugrunde liegende `TextWriter` Ausgabegerät erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="cfde9-1111">Aber wenn die Anwendung keinen Verweis auf die `In` und `Error` Eigenschaften, keine Objekte für diese Geräte erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="cfde9-1112">Automatisch implementierte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="cfde9-1112">Automatically implemented properties</span></span>

<span data-ttu-id="cfde9-1113">Eine automatisch implementierte Eigenschaft (oder ***Auto-Eigenschaft*** kurz), ist eine nicht abstrakte nicht Extern-Eigenschaft mit dem Text von Accessoren für reine durch Semikolons.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="cfde9-1114">Auto-Eigenschaften können müssen einen Get-Accessor und optional auch einen Set-Accessor.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="cfde9-1115">Wenn eine Eigenschaft als eine automatisch implementierte Eigenschaft angegeben wird, ein ausgeblendetes dahinter liegendes Feld wird automatisch für die Eigenschaft zur Verfügung, und die Accessoren implementiert, um Lese- und Schreibzugriff auf dieses dahinter liegende Feld.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="cfde9-1116">Wenn die automatische Eigenschaft keine Set-Accessor verfügt, gilt das dahinter liegende Feld `readonly` ([schreibgeschützte Felder](classes.md#readonly-fields)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="cfde9-1117">Genau wie ein `readonly` Feld eine nur-Getter-Auto-Eigenschaft kann auch zugewiesen werden im Text eines Konstruktors der einschließenden Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="cfde9-1118">Eine solche Zuweisung wird direkt das Readonly dahinter liegende Feld der Eigenschaft zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="cfde9-1119">Eine Auto-Eigenschaft möglicherweise optional ein *Property_initializer*, das angewendet wird, direkt auf das dahinter liegende Feld als ein *Variable_initializer* ([Variableninitialisierern](classes.md#variable-initializers)) .</span><span class="sxs-lookup"><span data-stu-id="cfde9-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="cfde9-1120">Im Beispiel unten geschieht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="cfde9-1121">entspricht die folgende Deklaration:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="cfde9-1122">Im Beispiel unten geschieht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="cfde9-1123">entspricht die folgende Deklaration:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1123">is equivalent to the following declaration:</span></span>
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

<span data-ttu-id="cfde9-1124">Beachten Sie, dass die Zuweisungen für das schreibgeschützte Feld zulässig, da sie innerhalb des Konstruktors auftreten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="cfde9-1125">Zugriff</span><span class="sxs-lookup"><span data-stu-id="cfde9-1125">Accessibility</span></span>

<span data-ttu-id="cfde9-1126">Besitzt ein Accessor einer *Accessor_modifier*, die Zugriffsdomäne ([Barrierefreiheit Domänen](basic-concepts.md#accessibility-domains)) des Accessors bestimmt ist, verwenden die deklarierte Zugriffsart von der *Accessor_modifier* .</span><span class="sxs-lookup"><span data-stu-id="cfde9-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="cfde9-1127">Wenn ein Accessor kein *Accessor_modifier*, die Zugriffsdomäne des Accessors wird über die deklarierte Zugriffsart von Eigenschaft oder des Indexers bestimmt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="cfde9-1128">Das Vorhandensein einer *Accessor_modifier* wirkt sich nicht auf die Suche nach Membern ([Operatoren](expressions.md#operators)) oder der überladungsauflösung ([Überladungsauflösung](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="cfde9-1129">Der Modifizierer für die Eigenschaft oder der Indexer zu immer bestimmen, welche Eigenschaft oder des Indexers, unabhängig vom Kontext des Zugriffs gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="cfde9-1130">Nachdem eine bestimmte Eigenschaft oder einen Indexer ausgewählt wurde, werden die Barrierefreiheit Domänen der beteiligten bestimmten Accessoren verwendet, um festzustellen, ob diese Nutzung gültig ist:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="cfde9-1131">Ist die Verwendung als Wert ([Werte Ausdrücke](expressions.md#values-of-expressions)), wird die `get` -Accessor vorhanden und zugänglich sein muss.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="cfde9-1132">Wenn die Verwendung als Ziel für eine einfache Zuweisung ([einfache Zuweisung](expressions.md#simple-assignment)), wird die `set` -Accessor vorhanden und zugänglich sein muss.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="cfde9-1133">Wenn die Auslastung als Ziel für zusammengesetzte Zuweisung ist ([Verbundzuweisung](expressions.md#compound-assignment)), oder als Ziel für die `++` oder `--` Operatoren ([Funktionsmember](expressions.md#function-members).9, [ Aufrufausdrücke](expressions.md#invocation-expressions)), werden beide die `get` Accessoren und `set` -Accessor vorhanden und zugänglich sein muss.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="cfde9-1134">Im folgenden Beispiel die Eigenschaft `A.Text` ist ausgeblendet, die Eigenschaft `B.Text`, dies gilt auch in Kontexten, in dem nur die `set` Accessor wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="cfde9-1135">Im Gegensatz dazu sind die Eigenschaft `B.Count` kann nicht zugegriffen werden, um die Klasse `M`, sodass die Eigenschaft zugegriffen werden kann `A.Count` wird stattdessen verwendet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

<span data-ttu-id="cfde9-1136">Ein Accessor, der verwendet wird, um eine Schnittstelle implementieren, möglicherweise kein *Accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="cfde9-1137">Wenn nur ein Accessor verwendet wird, eine Schnittstelle implementieren, kann der andere Accessor deklariert werden, mit einem *Accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="cfde9-1138">Virtuelle, versiegelt, überschriebene und abstrakte eigenschaftenzugriffsmethoden</span><span class="sxs-lookup"><span data-stu-id="cfde9-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="cfde9-1139">Ein `virtual` -Eigenschaftendeklaration gibt an, dass die Accessoren der Eigenschaft virtuell sind.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="cfde9-1140">Die `virtual` Modifizierer gilt für beide Accessoren der Schreib-Lese-Eigenschaft – es ist nicht möglich, dass nur ein Accessor einer Eigenschaft Lese-/ Schreibzugriff auf nicht virtuell sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="cfde9-1141">Ein `abstract` Eigenschaftendeklaration gibt an, dass die Accessoren der Eigenschaft sind virtuell, jedoch bietet keine tatsächliche Implementierung der Accessoren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="cfde9-1142">Stattdessen müssen nicht abstrakte abgeleitete Klassen ihre eigene Implementierung für die Accessoren durch Überschreiben der Eigenschaft bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="cfde9-1143">Da Sie ein Accessor für eine abstrakte Eigenschaftendeklaration keine Implementierungen, bietet die *Accessor_body* besteht einfach aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="cfde9-1144">Eine Eigenschaftendeklaration, die beide enthält die `abstract` und `override` Modifizierer gibt an, dass die Eigenschaft ist abstrakt und überschreibt eine Basiseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="cfde9-1145">Accessoren für diese Eigenschaft ist auch abstrakt sind.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="cfde9-1146">Abstrakte Eigenschaftendeklarationen sind nur in abstrakten Klassen zulässig ([abstrakte Klassen](classes.md#abstract-classes)). Die Zugriffsmethoden einer geerbten virtuellen-Eigenschaft können in einer abgeleiteten Klasse überschrieben werden, durch eine Eigenschaftendeklaration, der angibt, ein `override` Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="cfde9-1147">Dies bezeichnet man als ein ***überschreiben Eigenschaftendeklaration***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="cfde9-1148">Eine überschreibende Eigenschaftsdeklaration ist eine neue Eigenschaft nicht deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="cfde9-1149">Stattdessen ist einfach die Implementierungen der Accessoren einer vorhandenen virtuellen Eigenschaft spezialisiert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="cfde9-1150">Eine überschreibende Eigenschaftsdeklaration muss genau gleichen Zugriffsmodifizierer, Typ und Namen als die geerbte Eigenschaft angeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="cfde9-1151">Wenn die geerbte Eigenschaft hat nur einen einzelnen Accessor (d. h., wenn die geerbte Eigenschaft schreibgeschützt oder lesegeschützt ist), das die überschreibende Eigenschaft muss nur diesen Accessor enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="cfde9-1152">Wenn die geerbte Eigenschaft enthält beide Accessoren (d. h., wenn die geerbte Eigenschaft Lese-/ Schreibzugriff ist), die überschreibende Eigenschaft kann entweder eines einzelnen Accessors oder beide Accessoren enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="cfde9-1153">Eine überschreibende Eigenschaftsdeklaration herausgeberkontos ausgewiesenen Form der `sealed` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="cfde9-1154">Verwendung dieser Modifizierer wird verhindert, dass eine abgeleitete Klasse weiter Überschreiben der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="cfde9-1155">Die Zugriffsmethoden einer versiegelten Eigenschaft sind ebenfalls versiegelt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="cfde9-1156">Verhalten Syntax, virtuellen, "sealed" außer Kraft setzen und abstrakten Accessor genau wie virtuelle, "sealed" Override "und" abstrakte Methoden, mit Ausnahme der Unterschiede in der Deklarations- und Aufrufsyntax.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="cfde9-1157">Insbesondere die Regeln, die in beschriebenen [virtuelle Methoden](classes.md#virtual-methods), [Methoden außer Kraft setzen](classes.md#override-methods), [Versiegelte Methoden](classes.md#sealed-methods), und [abstrakten Methoden](classes.md#abstract-methods) gelten wie Accessoren wurden die Methoden der entsprechenden:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="cfde9-1158">Ein `get` -Accessor entspricht einer Methode ohne Parameter mit einem Rückgabewert von den Eigenschaftentyp und den gleichen Modifizierer wie die Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="cfde9-1159">Ein `set` Accessor entspricht einer Methode mit einem Einzelwert-Parameter, der den Typ der Eigenschaft, ein `void` Typ und die gleichen Modifizierer wie die Eigenschaft zurück.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="cfde9-1160">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1160">In the example</span></span>
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
<span data-ttu-id="cfde9-1161">`X` ist eine virtuelle nur-Lese Eigenschaft, `Y` ist eine virtuelle Lese-/ Schreibzugriff-Eigenschaft, und `Z` ist eine abstrakte Eigenschaft mit Lese-/ Schreibzugriff.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="cfde9-1162">Da `Z` ist eine abstrakte Methode der enthaltenden Klasse `A` muss auch als abstrakt deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="cfde9-1163">Eine abgeleitete Klasse `A` wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1163">A class that derives from `A` is show below:</span></span>
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

<span data-ttu-id="cfde9-1164">Hier wird die Deklarationen der `X`, `Y`, und `Z` Eigenschaftendeklarationen überschreiben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="cfde9-1165">Jede Eigenschaftendeklaration entspricht genau der Zugriffsmodifizierer, Typ und Namen der entsprechenden geerbte Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="cfde9-1166">Die `get` Accessor `X` und `set` Accessor `Y` verwenden die `base` Schlüsselwort, um die geerbten Accessoren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="cfde9-1167">Die Deklaration von `Z` beide Accessoren abstrakte überschreibungen – es gibt also keine ausstehenden abstrakte Funktionsmember in `B`, und `B` ist zulässig, eine nicht abstrakte Klasse sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="cfde9-1168">Wenn eine Eigenschaft deklariert ist, als ein `override`, alle überschriebenen Accessoren müssen für den überschreibenden Code zugänglich sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="cfde9-1169">Darüber hinaus muss die deklarierte Zugriffsart von sowohl die Eigenschaft oder den Indexer und Accessoren, mit dem überschriebenen Member und den Accessoren übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="cfde9-1170">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1170">For example:</span></span>
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a><span data-ttu-id="cfde9-1171">Ereignisse</span><span class="sxs-lookup"><span data-stu-id="cfde9-1171">Events</span></span>

<span data-ttu-id="cfde9-1172">Ein ***Ereignis*** ist ein Element, das ein Objekt oder eine Klasse, um Benachrichtigungen zu senden zu können.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="cfde9-1173">Clients können ausführbaren Code für Ereignisse anfügen, indem Sie die Angabe ***Ereignishandler***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="cfde9-1174">Ereignisse werden mit deklariert *Event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1174">Events are declared using *event_declaration*s:</span></span>

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

<span data-ttu-id="cfde9-1175">Ein *Event_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer ](classes.md#access-modifiers)), wird die `new` ([der new-Modifizierer](classes.md#the-new-modifier)), `static` ([statische und Instanzmethoden](classes.md#static-and-instance-methods)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Methoden außer Kraft setzen](classes.md#override-methods)), `sealed` ([Versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakten Methoden](classes.md#abstract-methods)), und `extern` ([Externe Methoden](classes.md#external-methods)) Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="cfde9-1176">Ereignisdeklarationen gelten dieselben Regeln wie Methodendeklarationen ([Methoden](classes.md#methods)) im Hinblick auf die gültigen Kombinationen der Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="cfde9-1177">Die *Typ* eines Ereignisses muss die Deklaration einer *Delegate_type* ([Verweistypen](types.md#reference-types)), und dass *Delegate_type* muss mindestens als wie das Ereignis selbst zugegriffen werden kann ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cfde9-1178">Eine Ereignisdeklaration herausgeberkontos ausgewiesenen Form *Event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="cfde9-1179">Jedoch wenn, für nicht-Extern, nicht abstrakte Ereignisse nicht der Compiler sie automatisch bereitgestellt ([Feldähnliche Ereignisse](classes.md#field-like-events)); für "extern"-Ereignisse, die Accessoren extern bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="cfde9-1180">Ein Event-Deklaration, die ausgelassen *Event_accessor_declarations* definiert eine oder mehrere Ereignisse – eine für jede der *Variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="cfde9-1181">Die Attribute und Modifizierer, gelten für alle Member deklariert, indem z. B. eine *Event_declaration*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="cfde9-1182">Es ist ein Fehler während der Kompilierung für eine *Event_declaration* zu beiden enthalten die `abstract` Modifizierer und geschweifte Klammern getrennter *Event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="cfde9-1183">Wenn eine Ereignisdeklaration enthält ein `extern` Modifizierer verwenden, das Ereignis wird als ein ***externes Ereignis***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="cfde9-1184">Da eine externes Ereignisdeklaration keine tatsächliche Implementierung bereitstellt, ist ein Fehler für sie zu beiden enthalten die `extern` Modifizierer und *Event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="cfde9-1185">Es ist ein Fehler während der Kompilierung für eine *Variable_declarator* einer Event-Deklaration mit einer `abstract` oder `external` Modifizierer enthalten eine *Variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="cfde9-1186">Ein Ereignis kann verwendet werden, wie der linke Operand der `+=` und `-=` Operatoren ([Ereignis Zuweisung](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="cfde9-1187">Diese Operatoren werden, bzw. zum Anfügen von Ereignishandlern zu oder Entfernen von-Ereignishandler von einem Ereignis, und der Zugriffsmodifizierer des Ereignisses steuern die Kontexte, in denen solche Vorgänge zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="cfde9-1188">Da `+=` und `-=` sind die einzigen Operationen, die auf ein Ereignis außerhalb des Typs, der deklariert zulässig sind, das Ereignis, den externen Code können hinzufügen und entfernen-Handler für ein Ereignis, jedoch kann nicht auf andere Weise zu erhalten oder ändern die zugrunde liegenden Liste des Ereignisses Handler.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="cfde9-1189">In einem Vorgang des Formulars `x += y` oder `x -= y`Wenn `x` ist ein Ereignis aus, und der Verweis erfolgt außerhalb des Typs, die die Deklaration enthält `x`, weist das Ergebnis des Vorgangs den Typ `void` (im Gegensatz zur Verwendung Der Typ des `x`, mit dem Wert des `x` nach der Zuweisung).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="cfde9-1190">Mit dieser Regel wird verhindert, dass externen Code indirekt überprüft den zugrunde liegenden Delegaten eines Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="cfde9-1191">Das folgende Beispiel zeigt, wie Ereignishandler angefügt werden, um Instanzen von der `Button` Klasse:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

<span data-ttu-id="cfde9-1192">Hier die `LoginDialog` Instanzkonstruktor erstellt zwei `Button` Instanzen und fügt der Ereignishandler die `Click` Ereignisse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="cfde9-1193">Feldähnliche Ereignisse</span><span class="sxs-lookup"><span data-stu-id="cfde9-1193">Field-like events</span></span>

<span data-ttu-id="cfde9-1194">In den Programmtext der Klasse oder Struktur, die die Deklaration eines Ereignisses enthält, können bestimmte Ereignisse wie Felder verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="cfde9-1195">Auf diese Weise verwendet werden soll, ein Ereignis darf nicht sein `abstract` oder `extern`, und müssen nicht explizit *Event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="cfde9-1196">Derartiges Ereignis kann in jedem Kontext verwendet werden, die ein Feld zulässt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="cfde9-1197">Das Feld enthält einen Delegaten ([Delegaten](delegates.md)) die bezieht sich auf die Liste der Ereignishandler, die das Ereignis hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="cfde9-1198">Wenn keine Ereignishandler hinzugefügt wurden, enthält das Feld `null`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="cfde9-1199">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1199">In the example</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
<span data-ttu-id="cfde9-1200">`Click` Dient als ein Feld innerhalb der `Button` Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="cfde9-1201">Wie im Beispiel wird veranschaulicht, kann das Feld werden überprüft, geändert und in den Delegaten aufrufen von Ausdrücken verwendet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="cfde9-1202">Die `OnClick` -Methode in der die `Button` "löst" die `Click` Ereignis.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="cfde9-1203">Das Auslösen eines Ereignisses entspricht exakt dem Aufrufen des Delegaten, der durch das Ereignis repräsentiert wird, es gibt deshalb keine besonderen Sprachkonstrukte zum Auslösen von Ereignissen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="cfde9-1204">Beachten Sie, dass der Delegataufruf eine Überprüfung vorangestellt ist, die sicherstellt, dass der Delegat ungleich Null ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="cfde9-1205">Außerhalb der Variablendeklaration der `Button` -Klasse, die `Click` Member kann nur verwendet werden, auf der linken Seite von der `+=` und `-=` Operatoren, wie in</span><span class="sxs-lookup"><span data-stu-id="cfde9-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="cfde9-1206">Ein Delegat der Aufrufliste von fügt die `Click` Ereignis und</span><span class="sxs-lookup"><span data-stu-id="cfde9-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="cfde9-1207">das einen Delegaten aus der Aufrufliste entfernt die `Click` Ereignis.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="cfde9-1208">Beim Kompilieren ein feldähnliches Ereignis der Compiler erstellt automatisch Speicher zum Speichern von Delegaten, und Zugriffsmethoden für das Ereignis, die hinzufügen oder Entfernen von Ereignishandlern auf das Delegatfeld erstellt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="cfde9-1209">Die Vorgänge hinzufügen und entfernen sind threadsicher, und möglicherweise (jedoch nicht erforderlich, um) Fertig Zeit belegen die Sperre ([die Lock-Anweisung](statements.md#the-lock-statement)) auf das enthaltende Objekt für ein Instanzereignis oder die Typ-Objekt ([anonym Objektausdrücke Erstellung](expressions.md#anonymous-object-creation-expressions)) für ein statisches Ereignis.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="cfde9-1210">Daher eine Instanzereignisdeklaration des Formulars:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="cfde9-1211">wird in etwa gleich kompiliert werden:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1211">will be compiled to something equivalent to:</span></span>
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
<span data-ttu-id="cfde9-1212">In der Klasse `X`, Verweise auf `Ev` auf der linken Seite von der `+=` und `-=` Operatoren dazu führen, dass das Hinzufügen und remove-Accessoren besitzen, die aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="cfde9-1213">Alle anderen Verweise auf `Ev` werden kompiliert, um das ausgeblendete Feld verweisen `__Ev` stattdessen ([Memberzugriff](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="cfde9-1214">Der Name "`__Ev`" ist ein beliebiger; das ausgeblendete Feld möglicherweise eine beliebige oder gar kein Name überhaupt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="cfde9-1215">Ereignisaccessoren</span><span class="sxs-lookup"><span data-stu-id="cfde9-1215">Event accessors</span></span>

<span data-ttu-id="cfde9-1216">Ereignisdeklarationen in der Regel auslassen *Event_accessor_declarations*, z. B. die `Button` Beispiel oben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="cfde9-1217">Eine Situation dafür umfasst also den Fall, in dem die Speicherkosten, die ein Feld pro Ereignis nicht zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="cfde9-1218">In solchen Fällen kann eine Klasse enthalten *Event_accessor_declarations* und einen privaten Mechanismus zum Speichern der Liste der Ereignishandler verwenden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="cfde9-1219">Die *Event_accessor_declarations* eines Ereignisses Geben Sie die ausführbare Anweisungen, die mit dem Hinzufügen und Entfernen von Ereignishandlern verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="cfde9-1220">Die Accessordeklarationen bestehen aus einer *Add_accessor_declaration* und *Remove_accessor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="cfde9-1221">Jede Zugriffsmethoden-Deklaration besteht aus dem Token `add` oder `remove` gefolgt von einem *Block*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="cfde9-1222">Die *Block* zugeordneten ein *Add_accessor_declaration* gibt an, die Anweisungen, die ausgeführt werden, wenn ein Ereignishandler hinzugefügt wird, und die *Block* eine zugeordnet*Remove_accessor_declaration* gibt an, die Anweisungen, die ausgeführt werden, wenn ein Ereignishandler entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="cfde9-1223">Jede *Add_accessor_declaration* und *Remove_accessor_declaration* entspricht einer Methode mit einem einzelnen Werteparameter des Ereignistyps und `void` Typ zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="cfde9-1224">Den Namen der impliziten Parameters mit der ein Ereignisaccessor `value`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="cfde9-1225">Wenn ein Ereignis in einer Ereignis-Zuweisung verwendet wird, wird der entsprechende Ereignisaccessor verwendet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="cfde9-1226">Insbesondere ist der Zuweisungsoperator `+=` der Add-Accessor verwendet wird, und wenn der Zuweisungsoperator `-=` der Remove-Accessor verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="cfde9-1227">In beiden Fällen wird der Rechte Operand des Zuweisungsoperators als Argument für den Ereignisaccessor verwendet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="cfde9-1228">Der Block eine *Add_accessor_declaration* oder *Remove_accessor_declaration* entsprechen den Regeln für `void` beschriebenen Methoden [Methodentext](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="cfde9-1229">Insbesondere `return` Anweisungen in einem solchen Block sind nicht zulässig, einen Ausdruck angeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="cfde9-1230">Da ein Ereignisaccessor implizit einen Parameter namens weist `value`, es ist ein Fehler während der Kompilierung für eine lokale Variable oder Konstante in einem Ereignisaccessor auf diesen Namen nicht bereits deklariert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="cfde9-1231">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1231">In the example</span></span>
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
<span data-ttu-id="cfde9-1232">die `Control` -Klasse implementiert einen internen Speichermechanismus für Ereignisse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="cfde9-1233">Die `AddEventHandler` Methode ordnet einen Delegatwert mit einem Schlüssel, der `GetEventHandler` Methodenrückgabe der Delegat, der derzeit mit einem Schlüssel zugeordnet sind und die `RemoveEventHandler` Methode entfernt einen Delegaten als Ereignishandler für das angegebene Ereignis.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="cfde9-1234">Es empfiehlt sich, der zugrunde liegenden Speichermechanismus ist so entworfen, dass es keine Kosten für das Zuordnen fallen einer `null` delegieren Wert mit einem Schlüssel und somit Ereignisse ohne Behandlung keinen Speicher nutzen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="cfde9-1235">Statische und Ereignisse</span><span class="sxs-lookup"><span data-stu-id="cfde9-1235">Static and instance events</span></span>

<span data-ttu-id="cfde9-1236">Wenn eine Ereignisdeklaration enthält eine `static` Modifizierer verwenden, das Ereignis gilt eine ***statisches Ereignis***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="cfde9-1237">Wenn kein `static` Modifizierer vorhanden ist, wird das Ereignis wird als ein ***Instanzereignisses***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="cfde9-1238">Ein statisches Ereignis ist nicht mit einer bestimmten Instanz verknüpft, und es ist ein Fehler während der Kompilierung zum Verweisen auf `this` in den Accessoren für ein statisches Ereignis.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="cfde9-1239">Ein Instanzereignis eine bestimmte Instanz einer Klasse zugeordnet ist, und diese Instanz zugegriffen werden kann, als `this` ([diesen Zugriff](expressions.md#this-access)) in den Zugriffsmethoden des Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="cfde9-1240">Wenn ein Ereignis verwiesen wird, einem *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `E.M`, wenn `M` ist ein statisches Ereignis, `E` muss einen Typ mit deuten`M`, und wenn `M` ist ein Instanzereignis E muss eine Instanz von einem Typ mit deuten `M`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="cfde9-1241">Die Unterschiede zwischen statischen und Instanzmember finden Sie weiter unten in [statische und Instanzmember](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="cfde9-1242">Virtuelle, versiegelt, überschriebene und abstrakte ereigniszugriffsmethoden</span><span class="sxs-lookup"><span data-stu-id="cfde9-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="cfde9-1243">Ein `virtual` gibt an, dass die Accessoren dieses Ereignisses virtuell sind.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="cfde9-1244">Die `virtual` Modifizierer gilt für beide Accessoren eines Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="cfde9-1245">Ein `abstract` Ereignisdeklaration gibt an, dass die Accessoren des Ereignisses sind virtuell, jedoch bietet keine tatsächliche Implementierung der Accessoren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="cfde9-1246">Stattdessen müssen nicht abstrakte abgeleitete Klassen ihre eigene Implementierung für die Accessoren angeben, durch Überschreiben des Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="cfde9-1247">Da eine abstrakte Ereignisdeklaration keine tatsächliche Implementierung bereitstellt, kann keine es bereitstellen, geschweifte Klammern getrennter *Event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="cfde9-1248">Eine Ereignisdeklaration, das beide enthält die `abstract` und `override` Modifizierer gibt an, dass das Ereignis ist abstrakt und überschreibt eine grundlegende-Ereignis.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="cfde9-1249">Die Accessoren eines solchen Ereignisses sind auch abstrakt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="cfde9-1250">Abstraktes Ereignis-Deklarationen sind nur in abstrakten Klassen zulässig ([abstrakte Klassen](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="cfde9-1251">Die zugriffmethoden von einer geerbten virtuellen Veranstaltung können in einer abgeleiteten Klasse überschrieben werden, durch eine Ereignisdeklaration, der angibt, einschließlich einer `override` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="cfde9-1252">Dies bezeichnet man als ein ***überschreiben Ereignisdeklaration***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="cfde9-1253">Eine überschreibende Ereignisdeklaration wird ein neues Ereignis nicht deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="cfde9-1254">Stattdessen ist einfach die Implementierungen der Accessoren für ein vorhandenes virtuelles Ereignis spezialisiert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="cfde9-1255">Eine überschreibende Ereignisdeklaration muss dem genau gleichen Zugriffsmodifizierer, Typ und Namen als das außer Kraft gesetzte Ereignis angeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="cfde9-1256">Eine überschreibende Event-Deklaration enthalten möglicherweise die `sealed` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="cfde9-1257">Verwendung dieser Modifizierer wird verhindert, dass eine abgeleitete Klasse weiter Überschreiben des Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="cfde9-1258">Die Accessoren der versiegelten Ereignisses sind ebenfalls versiegelt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="cfde9-1259">Es ist ein Fehler während der Kompilierung für eine überschreibende Ereignisdeklaration sollen eine `new` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="cfde9-1260">Verhalten Syntax, virtuellen, "sealed" außer Kraft setzen und abstrakten Accessor genau wie virtuelle, "sealed" Override "und" abstrakte Methoden, mit Ausnahme der Unterschiede in der Deklarations- und Aufrufsyntax.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="cfde9-1261">Insbesondere die Regeln, die in beschriebenen [virtuelle Methoden](classes.md#virtual-methods), [Methoden außer Kraft setzen](classes.md#override-methods), [Versiegelte Methoden](classes.md#sealed-methods), und [abstrakten Methoden](classes.md#abstract-methods) gelten wie Accessoren wurden die Methoden der entsprechenden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="cfde9-1262">Jeder Accessor entspricht einer Methode mit einem Einzelwert-Parameter, der den Ereignistyp vorgesehen, eine `void` Typ und die gleichen Modifizierer wie das enthaltende Ereignis zurück.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="cfde9-1263">Indexer</span><span class="sxs-lookup"><span data-stu-id="cfde9-1263">Indexers</span></span>

<span data-ttu-id="cfde9-1264">Ein ***Indexer*** ist ein Element, das ein Objekt, auf die gleiche Weise wie ein Array indiziert werden kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="cfde9-1265">Indexer sind mit deklariert *Indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1265">Indexers are declared using *indexer_declaration*s:</span></span>

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

<span data-ttu-id="cfde9-1266">Ein *Indexer_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer ](classes.md#access-modifiers)), wird die `new` ([der new-Modifizierer](classes.md#the-new-modifier)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Methoden außer Kraft setzen](classes.md#override-methods) ), `sealed` ([Versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakten Methoden](classes.md#abstract-methods)), und `extern` ([externe Methoden](classes.md#external-methods)) Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="cfde9-1267">Indexerdeklarationen sind, gelten dieselben Regeln wie Methodendeklarationen ([Methoden](classes.md#methods)) im Hinblick auf die gültigen Kombinationen von Modifizierern, mit der eine Ausnahme, die den static-Modifizierer darf nicht auf eine Indexerdeklaration.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="cfde9-1268">Die Modifizierer `virtual`, `override`, und `abstract` gegenseitig außer in einem Fall.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="cfde9-1269">Die `abstract` und `override` Modifizierer können zusammen verwendet werden, sodass abstrakte Indexer einen virtuellen überschreiben kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="cfde9-1270">Die *Typ* Deklaration gibt den Typ des Elements des Indexers, der durch die Deklaration eines Indexers an.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="cfde9-1271">Wenn der Indexer eine explizite Schnittstellenmember-Implementierung, ist die *Typ* folgt das Schlüsselwort `this`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="cfde9-1272">Für eine explizite Schnittstellenmember-Implementierung die *Typ* gefolgt von einer *Interface_type*, eine "`.`", und das Schlüsselwort `this`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="cfde9-1273">Im Gegensatz zu anderen Membern haben Indexer keine benutzerdefinierten Namen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="cfde9-1274">Die *Formal_parameter_list* gibt die Parameter des Indexers.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="cfde9-1275">Die Liste der formalen Parameter eines Indexers entspricht einer Methode ([Methodenparameter](classes.md#method-parameters)), mit dem Unterschied, dass mindestens ein Parameter angegeben werden muss, und dass die `ref` und `out` Parametermodifizierern sind nicht zulässig. .</span><span class="sxs-lookup"><span data-stu-id="cfde9-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="cfde9-1276">Die *Typ* einen Indexer und alle Typen verwiesen wird, der *Formal_parameter_list* muss mindestens dieselben zugriffsmöglichkeiten bieten wie der Indexer selbst ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cfde9-1277">Ein *Indexer_body* entweder besteht möglicherweise aus einem ***Accessor-Body*** oder ***ausdruckskörper***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="cfde9-1278">In einem Accessortext *Accessor_declarations*, die eingeschlossen werden müssen, "`{`"und"`}`" Token, deklarieren die Accessoren ([Accessoren](classes.md#accessors)) der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="cfde9-1279">Die Accessoren angeben, die ausführbaren Anweisungen lesen und schreiben die Eigenschaft zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="cfde9-1280">Ein ausdruckskörper bestehend aus "`=>`" gefolgt von einem Ausdruck `E` und ein Semikolon entspricht genau der Anweisungstext `{ get { return E; } }`, und kann daher nur verwendet werden nur-Getter-Indexer angeben, wo ist das Ergebnis der getter-Methode durch einen einzelnen Ausdruck angegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="cfde9-1281">Obwohl die Syntax für den Zugriff auf eine Indexer-Element identisch, die für ein Arrayelement ist, wird eine Indexer-Element nicht als Variable klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="cfde9-1282">Folglich ist es nicht möglich, übergeben einen Indexer-Element als ein `ref` oder `out` Argument.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="cfde9-1283">Die Liste der formalen Parameter eines Indexers definiert die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) des Indexers.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="cfde9-1284">Die Signatur eines Indexers besteht insbesondere die Anzahl und Typen der formalen Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="cfde9-1285">Der Elementtyp und den Namen der formalen Parameter sind nicht Teil der Signatur eines Indexers.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="cfde9-1286">Die Signatur eines Indexers muss die Signaturen aller anderen in derselben Klasse deklarierten Indexer unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="cfde9-1287">Indexer und Eigenschaften sind konzeptionell sehr ähnlich, unterscheiden sich jedoch auf folgende Weise:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="cfde9-1288">Eine Eigenschaft wird mit seinem Namen identifiziert, während von der Signatur ein Indexers identifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="cfde9-1289">Eine Eigenschaft erfolgt über eine *Simple_name* ([einfache Namen](expressions.md#simple-names)) oder ein *Member_access* ([Memberzugriff](expressions.md#member-access)), während eines Indexers Element erfolgt über eine *Element_access* ([Indexerzugriff](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="cfde9-1290">Eine Eigenschaft kann sein, eine `static` Member, während ein Indexers immer ein Instanzmember ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="cfde9-1291">Ein `get` Accessor einer Eigenschaft entspricht einer Methode ohne Parameter, während eine `get` Accessor eines Indexers entspricht einer Methode mit dem dieselbe Liste formaler Parameter wie der Indexer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="cfde9-1292">Ein `set` Accessor einer Eigenschaft entspricht einer Methode mit einem einzigen Parameter, die mit dem Namen `value`, während eine `set` Accessor eines Indexers entspricht einer Methode mit dem dieselbe Liste formaler Parameter wie der Indexer sowie einen zusätzlichen Parameter mit dem Namen `value`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="cfde9-1293">Es ist ein Fehler während der Kompilierung für einen Indexeraccessor für eine lokale Variable mit dem gleichen Namen wie ein Indexerparameter zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="cfde9-1294">In eine überschreibende Eigenschaftsdeklaration die geerbte Eigenschaft erfolgt mithilfe der Syntax `base.P`, wobei `P` ist der Eigenschaftenname.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="cfde9-1295">In der Indexerdeklaration einer überschreibenden der geerbte Indexer erfolgt mithilfe der Syntax `base[E]`, wobei `E` ist eine durch Trennzeichen getrennte Liste von Ausdrücken.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="cfde9-1296">Es gibt kein Konzept für "automatisch implementierte Indexer".</span><span class="sxs-lookup"><span data-stu-id="cfde9-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="cfde9-1297">Es ist ein Fehler auf einen nicht abstrakten, nicht extern-Indexer mit Semikolon-Accessoren aufweisen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="cfde9-1298">Abgesehen von diesen unterschieden werden alle Regeln in definierten [Accessoren](classes.md#accessors) und [automatisch implementierten Eigenschaften](classes.md#automatically-implemented-properties) als auch für Eigenschaftenaccessoren angewendet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="cfde9-1299">Wenn eine Indexerdeklaration enthält ein `extern` Modifizierer, der Indexer gilt eine ***externe Indexer***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="cfde9-1300">Da eine externe Indexerdeklaration keine Implementierung, jede bietet die *Accessor_declarations* besteht aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="cfde9-1301">Das folgende Beispiel deklariert eine `BitArray` -Klasse, die einen Indexer für den Zugriff auf den einzelnen Bits im BitArray implementiert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

<span data-ttu-id="cfde9-1302">Einer Instanz von der `BitArray` Klasse erheblich weniger Arbeitsspeicher belegt als eine entsprechende `bool[]` (da jeder Wert der ersten nur ein Bit anstelle von belegt Letzteres des ein Byte), aber es ermöglicht die gleichen Vorgänge wie eine `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="cfde9-1303">Die folgenden `CountPrimes` -Klasse verwendet ein `BitArray` und den klassischen "Sieb"-Algorithmus berechnet die Anzahl der Primzahlen wie folgt zwischen 1 und eine festgelegte maximale:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

<span data-ttu-id="cfde9-1304">Beachten Sie, dass die Syntax für den Zugriff auf Elemente der `BitArray` ist genau dieselbe wie für eine `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="cfde9-1305">Das folgende Beispiel zeigt eine Raster mit 26 \* 10-Klasse, die einen Indexer mit zwei Parametern verfügt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="cfde9-1306">Der erste Parameter ist erforderlich, um ein Groß- oder Kleinbuchstaben im Bereich von A bis Z sein, und die zweite ist erforderlich, um eine ganze Zahl im Bereich von 0 bis 9 sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a><span data-ttu-id="cfde9-1307">Indexer überladen</span><span class="sxs-lookup"><span data-stu-id="cfde9-1307">Indexer overloading</span></span>

<span data-ttu-id="cfde9-1308">Die Regeln der überladungsauflösung Indexer werden im beschrieben [Typrückschluss](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="cfde9-1309">Operatoren</span><span class="sxs-lookup"><span data-stu-id="cfde9-1309">Operators</span></span>

<span data-ttu-id="cfde9-1310">Ein ***Operator*** ist ein Member, die Bedeutung eines Operators Ausdruck definiert, die auf Instanzen der Klasse angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="cfde9-1311">Operatoren werden mit deklariert *Operator_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1311">Operators are declared using *operator_declaration*s:</span></span>

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="cfde9-1312">Es gibt drei Kategorien von Überladbare Operatoren: Unäre Operatoren ([unäre Operatoren](classes.md#unary-operators)), binäre Operatoren ([binären Operatoren](classes.md#binary-operators)), und Konvertierungsoperatoren ([Konvertierungsoperatoren](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="cfde9-1313">Die *Operator_body* ist entweder ein Semikolon, ein ***Anweisungstext*** oder ***ausdruckskörper***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="cfde9-1314">Eine-Anweisungstext besteht aus einem *Block*, die angibt, dass die Anweisungen, die ausgeführt werden, wenn der Operator aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="cfde9-1315">Die *Block* entsprechen den Regeln für die Wertrückgabe beschriebenen Methoden [Methodentext](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="cfde9-1316">Ein ausdruckskörper besteht aus `=>` gefolgt von einem Ausdruck und ein Semikolon, und gibt einen einzelnen Ausdruck ausführen, wenn der Operator aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="cfde9-1317">Für `extern` Operatoren, die *Operator_body* besteht einfach aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="cfde9-1318">Für alle anderen Operatoren wird die *Operator_body* ist ein Blocktext oder einen ausdruckskörper.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="cfde9-1319">Die folgenden Regeln gelten für alle Operatordeklarationen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="cfde9-1320">Eine Operatordeklaration muss enthalten beide eine `public` und `static` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="cfde9-1321">Die Parameter eines Operators muss den-Parameter ([-Wertparameter](variables.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="cfde9-1322">Es ist ein Fehler während der Kompilierung für eine Operatordeklaration an `ref` oder `out` Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="cfde9-1323">Die Signatur eines Operators ([unäre Operatoren](classes.md#unary-operators), [binären Operatoren](classes.md#binary-operators), [Konvertierungsoperatoren](classes.md#conversion-operators)) muss sich von den Signaturen aller anderen Operatoren, die im deklarierten unterscheiden die derselben Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="cfde9-1324">Alle Typen, die auf die verwiesen wird in der Operatordeklaration eines müssen mindestens dieselben zugriffsmöglichkeiten bieten wie der Operator selbst sein ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="cfde9-1325">Es ist ein Fehler für den gleichen Modifizierer für mehrere Male in einem Operatordeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="cfde9-1326">Jeder Operatorkategorie schränkt, wie in den folgenden Abschnitten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="cfde9-1327">Wie andere Member werden die Operatoren, die in einer Basisklasse von abgeleiteten Klassen geerbt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="cfde9-1328">Da Operatordeklarationen immer erforderlich, die Klasse oder Struktur, die in der der Operator, zur Teilnahme an der Signatur des Operators deklariert wird, ist es nicht möglich, für einen Operator in einer abgeleiteten Klasse deklariert, um einen Operator in einer Basisklasse deklariert auszublenden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="cfde9-1329">Daher die `new` Modifizierer ist nicht erforderlich, und daher nicht zulässig, in der Operatordeklaration eines.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="cfde9-1330">Weitere Informationen zur unären und binären Operatoren finden Sie im [Operatoren](expressions.md#operators).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="cfde9-1331">Weitere Informationen zu Konvertierungsoperatoren befinden sich [benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="cfde9-1332">Unäre Operatoren</span><span class="sxs-lookup"><span data-stu-id="cfde9-1332">Unary operators</span></span>

<span data-ttu-id="cfde9-1333">Die folgenden Regeln gelten für unären Operator-Deklarationen, wobei `T` bezeichnet den Instanztyp der Klasse oder Struktur, die sich die Operatordeklaration enthält:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="cfde9-1334">Ein unäres `+`, `-`, `!`, oder `~` Operator muss einen einzelnen Parameter vom Typ akzeptieren `T` oder `T?` und können jeden Typ zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="cfde9-1335">Ein unäres `++` oder `--` Operator muss einen einzelnen Parameter vom Typ akzeptieren `T` oder `T?` und denselben Typ oder einen davon Typ abgeleiteten zurückgeben müssen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="cfde9-1336">Ein unäres `true` oder `false` Operator muss einen einzelnen Parameter vom Typ akzeptieren `T` oder `T?` und müssen Rückgabetyp `bool`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="cfde9-1337">Die Signatur eines unären Operators besteht aus den Operatortoken (`+`, `-`, `!`, `~`, `++`, `--`, `true`, oder `false`) und den Typ des den einzelnen formalen Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="cfde9-1338">Der Rückgabetyp ist nicht Teil der Signatur für einen unäroperator, noch ist der Name des formalen Parameters.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="cfde9-1339">Die `true` und `false` unäre Operatoren müssen paarweise Deklaration.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="cfde9-1340">Ein Fehler während der Kompilierung tritt auf, wenn eine Klasse einer dieser Operatoren deklariert, ohne auch die andere zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="cfde9-1341">Die `true` und `false` Operatoren werden ausführlich in [bedingten logischen Operatoren eine benutzerdefinierte](expressions.md#user-defined-conditional-logical-operators) und [boolesche Ausdrücke](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="cfde9-1342">Das folgende Beispiel zeigt eine Implementierung und die anschließende Verwendung von `operator ++` für eine ganze Zahl Vector-Klasse:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

<span data-ttu-id="cfde9-1343">Beachten Sie, wie der Operator den Wert 1 mit dem Operanden, wie das Postfixinkrement zu addieren erzeugten Methodenrückgabe und Dekrement-Operatoren ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators)), und die Präfix-Inkrement und Dekrement Operatoren ([Präfix-Inkrement und Dekrement-Operatoren](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="cfde9-1344">Im Gegensatz zu muss in C++ wird diese Methode nicht den Wert des Operanden direkt ändern.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="cfde9-1345">Ändern den Wert des Operanden würde in der Tat die Standardsemantik des Postfixinkrement-Operators verletzt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="cfde9-1346">Binäre Operatoren</span><span class="sxs-lookup"><span data-stu-id="cfde9-1346">Binary operators</span></span>

<span data-ttu-id="cfde9-1347">Die folgenden Regeln gelten für binären Operator-Deklarationen, wobei `T` bezeichnet den Instanztyp der Klasse oder Struktur, die sich die Operatordeklaration enthält:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="cfde9-1348">Ein binärer nicht Shift-Operator muss zwei Parameter, die mindestens eine der Typ aufweisen einen muss annehmen `T` oder `T?`, und Sie können jeden Typ zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="cfde9-1349">Ein binäres `<<` oder `>>` -Operator muss zwei Parameter, die das erste Argument Typ aufweisen einen muss annehmen `T` oder `T?` und dem zweiten muss einen Typ aufweisen `int` oder `int?`, und Sie können jeden Typ zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="cfde9-1350">Die Signatur eines binären Operators besteht aus den Operatortoken (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, oder `<=`) und die Typen der formalen Parameter zwei.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="cfde9-1351">Der Rückgabetyp und die Namen der formalen Parameter sind nicht Teil eines binären Operators-Signatur.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="cfde9-1352">Bestimmte binären Operatoren müssen paarweise Deklaration.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="cfde9-1353">Für jede Deklaration entweder ein paar-Operators muss eine entsprechende Deklaration des anderen Operators des Paars vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="cfde9-1354">Zwei Operatordeklarationen Abgleich aus, wenn sie den gleichen Rückgabetyp und den gleichen Typ für jeden Parameter aufweisen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="cfde9-1355">Die folgenden Operatoren müssen paarweise Deklaration:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="cfde9-1356">`operator ==` und `operator !=`</span><span class="sxs-lookup"><span data-stu-id="cfde9-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="cfde9-1357">`operator >` und `operator <`</span><span class="sxs-lookup"><span data-stu-id="cfde9-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="cfde9-1358">`operator >=` und `operator <=`</span><span class="sxs-lookup"><span data-stu-id="cfde9-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="cfde9-1359">Konvertierungsoperatoren</span><span class="sxs-lookup"><span data-stu-id="cfde9-1359">Conversion operators</span></span>

<span data-ttu-id="cfde9-1360">Eine Konvertierung Operator-Deklaration führt eine ***benutzerdefinierte Konvertierung*** ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)) das Standardfehlerprotokoll erweitert, der vordefinierten impliziten und expliziten Konvertierungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="cfde9-1361">Eine Konvertierung Operator-Deklaration, enthält die `implicit` -Schlüsselwort Führt eine implizite Konvertierung von benutzerdefinierten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="cfde9-1362">Implizite Konvertierungen können in einer Vielzahl von Situationen, wie z.B. Memberaufrufen für die Funktion Cast-Ausdrücke und Zuweisungen auftreten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="cfde9-1363">Dies wird weiter in [implizite Konvertierungen](conversions.md#implicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="cfde9-1364">Eine Konvertierung Operator-Deklaration, enthält die `explicit` -Schlüsselwort Führt eine benutzerdefinierte explizite Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="cfde9-1365">Explizite Konvertierungen können in der Cast-Ausdrücke auftreten, und werden ausführlich in [explizite Konvertierungen](conversions.md#explicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="cfde9-1366">Ein Konvertierungsoperator konvertiert aus einer Datenquelle, die von der Parametertyp des Konvertierungsoperators in einen Zieltyp, angegeben durch den Rückgabetyp der Operator für die Konvertierung angegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="cfde9-1367">Für einen bestimmten Quell- `S` und einen Zieltyp `T`, wenn `S` oder `T` werden auf NULL festlegbare Typen können `S0` und `T0` finden Sie in die zugrunde liegende Typen, andernfalls `S0` und `T0` sind gleich `S` und `T` bzw.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="cfde9-1368">Eine Klasse oder Struktur ist zulässig, deklarieren Sie eine Konvertierung aus einer Datenquelle `S` in einen Zieltyp `T` nur dann, wenn alle der folgenden Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="cfde9-1369">`S0` und `T0` gibt verschiedene Typen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="cfde9-1370">Entweder `S0` oder `T0` ist der Typ Klasse oder Struktur, die in der die Operatordeklaration erfolgt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="cfde9-1371">Weder `S0` noch `T0` ist ein *Interface_type*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="cfde9-1372">Mit Ausnahme von benutzerdefinierten Konvertierungen, eine Konvertierung ist nicht vom `S` zu `T` oder `T` zu `S`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="cfde9-1373">Im Rahmen dieser Regeln werden alle zugeordneten Parameter geben `S` oder `T` gelten als eindeutige Typen, die keine vererbungsbeziehung mit anderen Typen, und alle Einschränkungen für diesen Typ mit dem Parameter werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="cfde9-1374">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="cfde9-1375">die ersten zwei Operatordeklarationen sind zulässig, da für die Zwecke [Indexer](classes.md#indexers).3 und `T` und `int` und `string` gelten jeweils eindeutige Typen ohne Beziehung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="cfde9-1376">Der dritte Operator ist jedoch ein Fehler, da `C<T>` ist die Basisklasse der `D<T>`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="cfde9-1377">Ein Konvertierungsoperator muss aus der zweiten Regel ergibt sich, dass konvertieren, auf ein oder von der Klasse oder Struktur-Typ, in dem der Operator deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="cfde9-1378">Beispielsweise ist es möglich, dass eine Klasse oder Struktur-Typ `C` definieren Sie eine Konvertierung von `C` zu `int` und `int` zu `C`, jedoch nicht von `int` zu `bool`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="cfde9-1379">Es ist nicht möglich, um eine vordefinierte Konvertierung direkt neu zu definieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="cfde9-1380">Konvertierungsoperatoren sind daher nicht zulässig, Konvertieren von oder zu `object` Da implizite und explizite Konvertierungen zwischen bereits vorhanden sind `object` und alle anderen Typen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="cfde9-1381">Ebenso können weder Quelle noch die Zieltypen einer Konvertierung sein, ein Basistyp des anderen, da eine Konvertierung, klicken Sie dann noch vorhanden wäre.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="cfde9-1382">Allerdings ist es möglich, die Operatoren in generischen Typen deklarieren, die bei bestimmten Typargumenten, geben Sie als vordefinierte Konvertierungen Konvertierungen, die bereits vorhanden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="cfde9-1383">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="cfde9-1384">Geben Sie bei `object` angegeben ist, als Typargument für `T`, der zweite Operator deklariert eine Konvertierung, die bereits vorhanden ist (eine implizite, und daher auch eine explizite Konvertierung von jedem Typ vorhanden ist `object`).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="cfde9-1385">In Fällen, in dem eine vordefinierte Konvertierung zwischen zwei Typen vorhanden ist, werden keine benutzerdefinierten Konvertierungen zwischen diesen Typen ignoriert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="cfde9-1386">Dies gilt insbesondere in folgenden Fällen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1386">Specifically:</span></span>

*  <span data-ttu-id="cfde9-1387">Wenn eine vordefinierte implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) vom Typ `S` eingeben `T`, alle benutzerdefinierten Konvertierungen (implizit oder explizit) von `S` zu `T` werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="cfde9-1388">Wenn die vordefinierten explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-conversions)) vom Typ `S` eingeben `T`, eine benutzerdefinierte, explizite Konvertierung von `S` zu `T` werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="cfde9-1389">Darüber hinaus:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1389">Furthermore:</span></span>

<span data-ttu-id="cfde9-1390">Wenn `T` ist ein Schnittstellentyp, der den benutzerdefinierten impliziten Konvertierungen von `S` zu `T` werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="cfde9-1391">Andernfalls, eine benutzerdefinierte impliziten Konvertierungen von `S` zu `T` gelten weiterhin.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="cfde9-1392">Für alle Typen jedoch `object`, die Operatoren deklariert, indem die `Convertible<T>` oben genannten Typ nicht in Konflikt mit vordefinierten Konvertierungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="cfde9-1393">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="cfde9-1394">Für den Typ jedoch `object`, vordefinierte Konvertierungen ausblenden, die eine benutzerdefinierte Konvertierungen in allen Fällen jedoch eine:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="cfde9-1395">Benutzerdefinierte Konvertierungen sind nicht zulässig, Konvertieren von oder zu *Interface_type*s.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="cfde9-1396">Insbesondere diese Beschränkung wird sichergestellt, dass keine benutzerdefinierten Transformationen erfolgen, bei der Konvertierung in eine *Interface_type*, und dass eine Konvertierung in eine *Interface_type* nur erfolgreich, wenn das Objekt konvertierte tatsächlich implementiert den angegebenen *Interface_type*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="cfde9-1397">Die Signatur eines Konvertierungsoperators besteht aus den Quelltyp und der Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="cfde9-1398">(Beachten Sie, dass dies die einzige Form des Elements ist der Rückgabetyp für die in der Signatur beteiligt ist.) Die `implicit` oder `explicit` Klassifizierung eines Konvertierungsoperators ist nicht Teil der Signatur des Operators.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="cfde9-1399">Daher eine Klasse oder Struktur kann nicht deklariert sowohl eine `implicit` und `explicit` Konvertierungsoperator mit den gleichen Typen von Quelle und Ziel.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="cfde9-1400">Im Allgemeinen sollten implizite Konvertierungen benutzerdefinierte entworfen werden, keine Ausnahmen auslösen und keine Informationen verlieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="cfde9-1401">Wenn eine benutzerdefinierte Konvertierung Anstieg auf Ausnahmen erhalten (z. B. weil das Quellargument außerhalb des gültigen Bereichs ist) oder Verlust von Informationen (z. B. das höherwertige Bits werden verworfen), und klicken Sie dann auf diese Konvertierung als eine explizite Konvertierung definiert werden sollte.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="cfde9-1402">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1402">In the example</span></span>
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
<span data-ttu-id="cfde9-1403">die Konvertierung von `Digit` zu `byte` ist implizit, da sie nie Ausnahmen auslöst oder Informationen, aber die Konvertierung von verliert `byte` zu `Digit` ist seit explizit `Digit` können nur eine Teilmenge der möglichen darstellen Werte von einem `byte`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="cfde9-1404">Instanzkonstruktoren</span><span class="sxs-lookup"><span data-stu-id="cfde9-1404">Instance constructors</span></span>

<span data-ttu-id="cfde9-1405">Ein ***Instanzkonstruktor*** ist ein Member, der die erforderlichen Aktionen zum Initialisieren einer Instanz einer Klasse implementiert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="cfde9-1406">Instanzkonstruktoren werden mit deklariert *Constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="cfde9-1407">Ein *Constructor_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)), eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer ](classes.md#access-modifiers)), und ein `extern` ([externe Methoden](classes.md#external-methods)) Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="cfde9-1408">Eine Konstruktordeklaration verwendet darf nicht den gleichen Modifizierer mehrfach enthalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="cfde9-1409">Die *Bezeichner* von einem *Constructor_declarator* müssen den Namen der Klasse, die in der die Instanzkonstruktor deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="cfde9-1410">Wenn ein anderen Namen angegeben wird, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="cfde9-1411">Der optionale *Formal_parameter_list* einer Instanz unterliegt Konstruktor dieselben Regeln wie für die *Formal_parameter_list* einer Methode ([Methoden](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="cfde9-1412">Liste der formalen Parameter definiert die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) eines Instanzkonstruktors und steuert den Prozess, bei dem der überladungsauflösung ([Typrückschluss](expressions.md#type-inference)) Wählt eine bestimmte Bei einem Aufruf Instance-Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="cfde9-1413">Alle Typen verwiesen wird, der *Formal_parameter_list* einer Instanz-Konstruktor muss mindestens dieselben zugriffsmöglichkeiten bieten wie der Konstruktor selbst sein ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cfde9-1414">Der optionale *Constructor_initializer* gibt an, eine andere Instanzkonstruktor aufrufen, die vor dem Ausführen der Anweisungen, die im angegebenen die *Constructor_body* dieses Konstruktors Instanz.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="cfde9-1415">Dies wird weiter in [Konstruktorinitialisierer](classes.md#constructor-initializers).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="cfde9-1416">Wenn eine Konstruktordeklaration verwendet enthält ein `extern` Modifizierer, der Konstruktor wird als ein ***externer Konstruktor***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="cfde9-1417">Da eine externe Konstruktordeklaration keine Implementierungen, bietet die *Constructor_body* besteht aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="cfde9-1418">Für alle übrigen Konstruktoren die *Constructor_body* besteht aus einem *Block* der gibt an, die Anweisungen, um eine neue Instanz der Klasse zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="cfde9-1419">Dies entspricht genau dem der *Block* einer Instanzmethode mit einem `void` Rückgabetyp ([Methodentext](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="cfde9-1420">Instanzkonstruktoren werden nicht geerbt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="cfde9-1421">Daher verfügt eine Klasse keine Instanzkonstruktoren auf als die tatsächlich in der Klasse deklariert wurden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="cfde9-1422">Wenn eine Klasse keine Instanz-Deklarationen enthält, wird ein Standardkonstruktor für die Instanz wird automatisch bereitgestellt ([Standardkonstruktoren](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="cfde9-1423">Instanzkonstruktoren werden aufgerufen, indem *Object_creation_expression*s ([Erstellung Objektausdrücke](expressions.md#object-creation-expressions)) und über *Constructor_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="cfde9-1424">Konstruktorinitialisierer</span><span class="sxs-lookup"><span data-stu-id="cfde9-1424">Constructor initializers</span></span>

<span data-ttu-id="cfde9-1425">Alle Instanzkonstruktoren (mit Ausnahme für die Klasse `object`) sind implizit einen Aufruf des Instanzkonstruktors für eine andere unmittelbar vor der *Constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="cfde9-1426">Der Konstruktor, der implizit aufgerufen richtet sich nach der *Constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="cfde9-1427">Ein Instanz Konstruktorinitialisierer des Formulars `base(argument_list)` oder `base()` bewirkt, dass ein Instanzkonstruktor aus der direkten Basisklasse aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="cfde9-1428">Dieser Konstruktor wird mit ausgewählt *Argument_list* wenn vorhanden und die Regeln der überladungsauflösung des [Überladungsauflösung](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="cfde9-1429">Der Satz von Kandidaten Instanzkonstruktoren besteht aus allen verfügbaren Instanzkonstruktoren, die in der direkten Basisklasse enthalten oder die Standard-Konstruktor ([Standardkonstruktoren](classes.md#default-constructors)), wenn keine Instanzkonstruktoren, in deklariert sind der direkte Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="cfde9-1430">Wenn dieser Satz leer ist, oder ein einzelnen bewährte Instanzenkonstruktor nicht identifiziert werden kann, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="cfde9-1431">Ein Instanz Konstruktorinitialisierer des Formulars `this(argument-list)` oder `this()` bewirkt, dass ein Instanzkonstruktor aus der Klasse selbst aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="cfde9-1432">Der Konstruktor ausgewählt ist, mithilfe von *Argument_list* wenn vorhanden und die Regeln der überladungsauflösung des [Überladungsauflösung](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="cfde9-1433">Der Satz von Kandidaten Instanzkonstruktoren besteht aus allen verfügbaren Instanzkonstruktoren, die in der Klasse selbst deklariert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="cfde9-1434">Wenn dieser Satz leer ist, oder ein einzelnen bewährte Instanzenkonstruktor nicht identifiziert werden kann, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="cfde9-1435">Wenn eine Instanzkonstruktordeklaration einem Konstruktorinitialisierer, die den Konstruktor selbst aufruft enthält, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="cfde9-1436">Verfügt ein Instanzkonstruktor ohne Konstruktorinitialisierer, einem Konstruktorinitialisierer des Formulars `base()` wird impliziert bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="cfde9-1437">Daher eine Instanzkonstruktordeklaration des Formulars</span><span class="sxs-lookup"><span data-stu-id="cfde9-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="cfde9-1438">ist genau gleich</span><span class="sxs-lookup"><span data-stu-id="cfde9-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="cfde9-1439">Der Bereich der angegebenen Parameter die *Formal_parameter_list* eines Instanzkonstruktors Deklaration konstruktorinitialisierers diese Deklaration enthält.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="cfde9-1440">Daher ist einem Konstruktorinitialisierer auf die Parameter des Konstruktors zulässig.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="cfde9-1441">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1441">For example:</span></span>
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

<span data-ttu-id="cfde9-1442">Ein Konstruktorinitialisierer Instanz kann nicht zu erstellende Instanz zugreifen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="cfde9-1443">Daher ist es ein Fehler während der Kompilierung auf `this` in ein Argumentausdruck des konstruktorinitialisierers, wie ist es ein Fehler während der Kompilierung ein Argumentausdruck auf beliebiger Instanzmember über eine *Simple_name*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="cfde9-1444">Instanz Variableninitialisierern</span><span class="sxs-lookup"><span data-stu-id="cfde9-1444">Instance variable initializers</span></span>

<span data-ttu-id="cfde9-1445">Wenn ein Instanzkonstruktor hat keinen Konstruktor/Initialisierer aufweisen oder einem Konstruktorinitialisierer des Formulars kann `base(...)`, dieser Konstruktor führt implizit die Initialisierungen, die gemäß der *Variable_initializer*s die Instanzfelder werden in der Klasse deklariert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="cfde9-1446">Dies entspricht einer Sequenz von Zuweisungen, die bei der Eingabe an den Konstruktor und vor den impliziten Aufruf des Konstruktors der direkten Basisklasse sofort ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="cfde9-1447">Die Variable Initialisierer werden in der Reihenfolge im Text ausgeführt in denen sie in der Klassendeklaration vorkommen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="cfde9-1448">Konstruktor-Ausführung</span><span class="sxs-lookup"><span data-stu-id="cfde9-1448">Constructor execution</span></span>

<span data-ttu-id="cfde9-1449">Variableninitialisierern in zuweisungsanweisungen transformiert werden, und diese Anweisungen werden ausgeführt, bevor Sie den Aufruf des Konstruktors der Basisklasse-Instanz.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="cfde9-1450">Diese Reihenfolge wird sichergestellt, dass alle Instanzenfelder von ihren Variableninitialisierern initialisiert werden, bevor alle Anweisungen, die Zugriff auf diese Instanz ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="cfde9-1451">Im Beispiel angegeben</span><span class="sxs-lookup"><span data-stu-id="cfde9-1451">Given the example</span></span>
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
<span data-ttu-id="cfde9-1452">Wenn `new B()` dient zum Erstellen einer Instanz von `B`, wird die folgende Ausgabe generiert:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```
x = 1, y = 0
```

<span data-ttu-id="cfde9-1453">Der Wert des `x` ist 1, da die Variableninitialisierer ausgeführt wird, bevor der Konstruktor der Basisklasse-Instanz aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="cfde9-1454">Jedoch den Wert der `y` ist 0 (standardmäßig den Wert ein `int`) da die Zuweisung zu `y` wird erst ausgeführt, nachdem Konstruktor der Basisklasse zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="cfde9-1455">Es ist hilfreich, die Instanz Variableninitialisierern und Konstruktorinitialisierer wie Anweisungen vorstellen, die automatisch, bevor eingefügt werden die *Constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="cfde9-1456">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1456">The example</span></span>
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
<span data-ttu-id="cfde9-1457">enthält mehrere Variableninitialisierern; Es enthält auch eine Konstruktor-Initialisierungen der beiden Formen (`base` und `this`).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="cfde9-1458">Das Beispiel entspricht dem folgenden Code, wobei jeder Kommentar eine automatisch eingefügte Anweisung gibt an, (die Syntax für die Konstruktoraufrufe automatisch eingefügte ist nicht gültig, aber dient lediglich zur Veranschaulichung des Mechanismus).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a><span data-ttu-id="cfde9-1459">Standardkonstruktoren</span><span class="sxs-lookup"><span data-stu-id="cfde9-1459">Default constructors</span></span>

<span data-ttu-id="cfde9-1460">Wenn eine Klasse keine Instanz-Deklarationen enthält, wird automatisch ein Standardkonstruktor für die Instanz bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="cfde9-1461">Diese Standard-Konstruktor ruft einfach den parameterlosen Konstruktor der direkten Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="cfde9-1462">Wenn die Klasse abstrakt ist, ist dann die deklarierte Zugriffsart für den Standardkonstruktor geschützt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="cfde9-1463">Andernfalls ist die deklarierte Zugriffsart für den Standardkonstruktor öffentlich.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="cfde9-1464">Ist daher der Standardkonstruktor immer im Format</span><span class="sxs-lookup"><span data-stu-id="cfde9-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="cfde9-1465">oder</span><span class="sxs-lookup"><span data-stu-id="cfde9-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="cfde9-1466">wo `C` ist der Name der Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="cfde9-1467">Wenn die Auflösung von funktionsüberladungen kann nicht auf einen eindeutige besten Kandidaten für die Basisklasse konstruktorinitialisierers zu bestimmen, und klicken Sie dann ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="cfde9-1468">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="cfde9-1469">ein Standardkonstruktor bereitgestellt, da die Klasse keine Instanz-Deklarationen enthält.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="cfde9-1470">Daher ist das Beispiel genaue Entsprechung</span><span class="sxs-lookup"><span data-stu-id="cfde9-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="cfde9-1471">Private Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="cfde9-1471">Private constructors</span></span>

<span data-ttu-id="cfde9-1472">Wenn eine Klasse `T` nur private Instanzkonstruktoren deklariert, ist nicht möglich, dass Klassen außerhalb der Programmtext des `T` für die Ableitung `T` oder direkt erstellen von Instanzen von `T`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="cfde9-1473">Daher, wenn eine Klasse nur statische Member enthält und wird nicht instanziiert werden soll, Hinzufügen einer leeren, privaten Instanzkonstruktor Instanziierung verhindert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="cfde9-1474">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1474">For example:</span></span>
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

<span data-ttu-id="cfde9-1475">Die `Trig` Klasse Gruppen von verwandten Methoden und Konstanten, aber nicht instanziiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="cfde9-1476">Aus diesem Grund deklariert einen leeren privaten Instanzkonstruktor.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="cfde9-1477">Mindestens ein Instanzkonstruktor muss deklariert werden, um die automatische Generierung des Standardkonstruktors zu unterdrücken.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="cfde9-1478">Optionale Instanz Konstruktorparameter</span><span class="sxs-lookup"><span data-stu-id="cfde9-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="cfde9-1479">Die `this(...)` Form der Konstruktorinitialisierer wird meist in Verbindung mit der Überladung um optionale Instanz Konstruktorparameter zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="cfde9-1480">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1480">In the example</span></span>
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
<span data-ttu-id="cfde9-1481">die ersten zwei Instanzkonstruktoren geben lediglich die Standardwerte für die fehlenden Argumente an.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="cfde9-1482">Beide verwenden eine `this(...)` Konstruktor/Initialisierer aufweisen, den dritten Instanzkonstruktor aufrufen, sodass die Arbeit wird die neue Instanz initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="cfde9-1483">Der Effekt ist der optionale Konstruktorparameter:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="cfde9-1484">Statische Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="cfde9-1484">Static constructors</span></span>

<span data-ttu-id="cfde9-1485">Ein ***statischen Konstruktor*** ist ein Element, das die erforderlichen Aktionen zum Initialisieren eines geschlossenen Klassentyps implementiert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="cfde9-1486">Statische Konstruktoren deklariert werden, mithilfe von *Static_constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="cfde9-1487">Ein *Static_constructor_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)) und ein `extern` Modifizierer ([externe Methoden](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="cfde9-1488">Die *Bezeichner* von einem *Static_constructor_declaration* müssen den Namen der Klasse, die in der der statische Konstruktor deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="cfde9-1489">Wenn ein anderen Namen angegeben wird, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="cfde9-1490">Wenn eine statische Destruktordeklaration enthält ein `extern` Modifizierer, der statische Konstruktor gilt eine ***externen statischen Konstruktor***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="cfde9-1491">Da eine externe statische Destruktordeklaration keine Implementierungen, bietet die *Static_constructor_body* besteht aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="cfde9-1492">Für alle anderen statischen Konstruktor-Deklarationen die *Static_constructor_body* besteht aus einem *Block* der gibt an, die Anweisungen ausführen, um die Klasse zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="cfde9-1493">Dies entspricht genau der *Method_body* einer statischen Methode mit einer `void` Rückgabetyp ([Methodentext](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="cfde9-1494">Statische Konstruktoren nicht vererbt werden und nicht direkt aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="cfde9-1495">Der statische Konstruktor für einen geschlossenen Klassentyp wird höchstens einmal in einer bestimmten Anwendungsdomäne ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="cfde9-1496">Die Ausführung eines statischen Konstruktors wird durch das erste der folgenden Ereignisse innerhalb einer Anwendungsdomäne ausgelöst:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="cfde9-1497">Eine Instanz des Klassentyps wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="cfde9-1498">Einer der statischen Member des Klassentyps sind auf die verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="cfde9-1499">Wenn eine Klasse enthält die `Main` Methode ([Anwendungsstart](basic-concepts.md#application-startup)) in die die Ausführung beginnt, den statischen Konstruktor für diese Klasse vor ausgeführt wird die `Main` Methode wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="cfde9-1500">Zum Initialisieren Sie eines neuen geschlossene Klassentyps, zunächst eines neuen Satz von statischen Feldern ([statische Felder und Instanzfelder](classes.md#static-and-instance-fields)) für den gewählten geschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="cfde9-1501">Jedes der statische Felder, die auf den Standardwert initialisiert wird ([Standardwerte](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="cfde9-1502">Anschließend wird die statischen Feldinitialisierer ([Initialisierung statischer Felder](classes.md#static-field-initialization)) für die statische Felder ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="cfde9-1503">Schließlich wird der statische Konstruktor ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="cfde9-1504">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1504">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
<span data-ttu-id="cfde9-1505">muss die Ausgabe erzeugen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1505">must produce the output:</span></span>
```
Init A
A.F
Init B
B.F
```
<span data-ttu-id="cfde9-1506">Da die Ausführung von `A`des statischer Konstruktor wird ausgelöst, durch den Aufruf von `A.F`, und die Ausführung des `B`des statischer Konstruktor wird ausgelöst, durch den Aufruf von `B.F`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="cfde9-1507">Es ist möglich, die ringabhängigkeiten erstellen, mit denen statische Felder mit Variableninitialisierern an, die im Standardzustand Wert beachtet werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="cfde9-1508">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="cfde9-1508">The example</span></span>
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
<span data-ttu-id="cfde9-1509">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="cfde9-1509">produces the output</span></span>
```
X = 1, Y = 2
```

<span data-ttu-id="cfde9-1510">Zum Ausführen der `Main` -Methode, führt das System zunächst den Initialisierer für `B.Y`vor der Klasse `B`des statischen Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="cfde9-1511">`Y`die führt dazu, dass der Initialisierer `A`des statischen Konstruktor verfügt, ausgeführt werden, da der Wert des `A.X` verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="cfde9-1512">Der statische Konstruktor des `A` wiederum geht, der Berechnung des Werts der `X`, und dies der Fall ist, ruft des Standardwerts von `Y`, d.h. 0 (null).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="cfde9-1513">`A.X` 1 wird so initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="cfde9-1514">Der Prozess zur Ausführung `A`des statischen Feldinitialisierer und statischen Konstruktor, und klicken Sie dann abgeschlossen ist, und die Berechnung des Anfangswerts der `Y`, deren Ergebnis ist 2.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="cfde9-1515">Da der statische Konstruktor genau einmal ausgeführt wird, für jede erstellte Klassentyp geschlossen, es ist eine bequeme Möglichkeit, Überprüfungen zur Laufzeit für den Typparameter zu erzwingen, die zum Zeitpunkt der Kompilierung über die Einschränkungen nicht überprüft werden kann ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="cfde9-1516">Beispielsweise verwendet der folgende Typ einen statischen Konstruktor erzwingen, dass das Typargument eine Enumeration ist:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a><span data-ttu-id="cfde9-1517">Destruktoren</span><span class="sxs-lookup"><span data-stu-id="cfde9-1517">Destructors</span></span>

<span data-ttu-id="cfde9-1518">Ein ***Destruktor*** ist ein Element, das die erforderlichen Aktionen zum zerstört einer Instanz einer Klasse implementiert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="cfde9-1519">Ein Destruktor deklariert wird, mithilfe einer *Destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1519">A destructor is declared using a *destructor_declaration*:</span></span>

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="cfde9-1520">Ein *Destructor_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="cfde9-1521">Die *Bezeichner* von einem *Destructor_declaration* müssen den Namen der Klasse, die in der der Destruktor deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="cfde9-1522">Wenn ein anderen Namen angegeben wird, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="cfde9-1523">Wenn eine Destruktordeklaration enthält eine `extern` Modifizierer, der Destruktor gilt eine ***externe Destruktor***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="cfde9-1524">Da eine externe Destruktordeklaration keine Implementierungen, bietet die *Destructor_body* besteht aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="cfde9-1525">Für alle anderen Destruktoren der *Destructor_body* besteht aus einem *Block* der gibt an, die Anweisungen ausführen, um eine Instanz der Klasse zerstört.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="cfde9-1526">Ein *Destructor_body* entspricht genau der *Method_body* einer Instanzmethode mit einer `void` Rückgabetyp ([Methodentext](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="cfde9-1527">Destruktoren werden nicht geerbt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1527">Destructors are not inherited.</span></span> <span data-ttu-id="cfde9-1528">Daher verfügt über eine Klasse keine Destruktoren, die in dieser Klasse deklariert werden kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="cfde9-1529">Da Sie ein Destruktor ohne Parameter erforderlich ist, darf nicht überladene, sein, damit eine Klasse, höchstens einen Destruktor verfügen kann.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="cfde9-1530">Destruktoren werden automatisch aufgerufen und können nicht explizit aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="cfde9-1531">Eine Instanz wird zerstört, wenn er nicht mehr möglich, dass jeder Code, verwenden Sie diese Instanz ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="cfde9-1532">Der Destruktor für die Instanz kann auftreten, zu einem beliebigen Zeitpunkt nach dem die Instanz für die Zerstörung freigegeben wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="cfde9-1533">Wenn eine Instanz zerstört wird, die Destruktoren in diese Instanz die Vererbungskette werden aufgerufen, in der Reihenfolge, die meisten abgeleitet, um am wenigsten abgeleiteten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="cfde9-1534">Ein Destruktor kann auf jedem Thread ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="cfde9-1535">Weitere Informationen zu den Regeln, die steuern, wann und wie ein Destruktor ausgeführt wird, finden Sie unter [automatische Speicherverwaltung](basic-concepts.md#automatic-memory-management).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="cfde9-1536">Die Ausgabe des Beispiels</span><span class="sxs-lookup"><span data-stu-id="cfde9-1536">The output of the example</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
<span data-ttu-id="cfde9-1537">echt</span><span class="sxs-lookup"><span data-stu-id="cfde9-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="cfde9-1538">da Destruktoren in einer Vererbungskette nacheinander aufgerufen werden die meisten abgeleitet, um am wenigsten abgeleiteten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="cfde9-1539">Destruktoren werden durch Überschreiben der virtuellen Methode implementiert `Finalize` auf `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="cfde9-1540">C#-Programme sind nicht zulässig, diese Methode überschreiben, oder rufen sie (oder überschreibt es) direkt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="cfde9-1541">Beispielsweise das Programm</span><span class="sxs-lookup"><span data-stu-id="cfde9-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="cfde9-1542">enthält zwei Fehler an.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1542">contains two errors.</span></span>

<span data-ttu-id="cfde9-1543">Der Compiler verhält sich wie diese Methode, und klicken Sie auf Außerkraftsetzungen, überhaupt nicht vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="cfde9-1544">Daher ist dieses Programm:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="cfde9-1545">ist gültig, und die Methode blendet `System.Object`des `Finalize` Methode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="cfde9-1546">Eine Erläuterung des Verhaltens bei der von einem Destruktor eine Ausnahme ausgelöst wird, finden Sie unter [Behandlung von Ausnahmen](exceptions.md#how-exceptions-are-handled).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="cfde9-1547">Iterators</span><span class="sxs-lookup"><span data-stu-id="cfde9-1547">Iterators</span></span>

<span data-ttu-id="cfde9-1548">Ein Funktionsmember ([Funktionsmember](expressions.md#function-members)) mit einem Iteratorblock implementiert ([Blöcke](statements.md#blocks)) wird aufgerufen, eine ***Iterator***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="cfde9-1549">Kein Iteratorblock kann als Text ein Funktionsmember verwendet werden, solange der Rückgabetyp des entsprechenden Elements Funktion eine der Enumeratorschnittstellen ([Enumeratorschnittstellen](classes.md#enumerator-interfaces)) mindestens die enumerable-Schnittstellen ([Enumerable-Schnittstellen](classes.md#enumerable-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="cfde9-1550">Es ist möglich, als eine *Method_body*, *Operator_body* oder *Accessor_body*, während Ereignisse, Instanzkonstruktoren, statische Konstruktoren und Destruktoren nicht möglich als Iteratoren implementiert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="cfde9-1551">Wenn ein Funktionsmember mit Iteratorblock implementiert ist, es ist ein Fehler während der Kompilierung für die Liste der formalen Parameter, der das Funktionselement zum angeben `ref` oder `out` Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="cfde9-1552">Enumerator-Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="cfde9-1552">Enumerator interfaces</span></span>

<span data-ttu-id="cfde9-1553">Die ***Enumeratorschnittstellen*** sind die nicht generische Schnittstelle `System.Collections.IEnumerator` und alle Instanziierungen an der die generische Schnittstelle `System.Collections.Generic.IEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="cfde9-1554">Aus Gründen der Übersichtlichkeit sind in diesem Kapitel diese Schnittstellen werden auf die verwiesen wird als `IEnumerator` und `IEnumerator<T>`bzw.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="cfde9-1555">Aufzählbare Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="cfde9-1555">Enumerable interfaces</span></span>

<span data-ttu-id="cfde9-1556">Die ***enumerable-Schnittstellen*** sind die nicht generische Schnittstelle `System.Collections.IEnumerable` und alle Instanziierungen an der die generische Schnittstelle `System.Collections.Generic.IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="cfde9-1557">Aus Gründen der Übersichtlichkeit sind in diesem Kapitel diese Schnittstellen werden auf die verwiesen wird als `IEnumerable` und `IEnumerable<T>`bzw.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="cfde9-1558">Yield-Typ</span><span class="sxs-lookup"><span data-stu-id="cfde9-1558">Yield type</span></span>

<span data-ttu-id="cfde9-1559">Ein Iterator erzeugt eine Sequenz von Werten, alle den gleichen Typ aufweisen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="cfde9-1560">Dieser Typ wird aufgerufen, die ***yield-Typ*** des Iterators.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="cfde9-1561">Die Yield-Typ, der ein Iterator, der gibt `IEnumerator` oder `IEnumerable` ist `object`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="cfde9-1562">Die Yield-Typ, der ein Iterator, der gibt `IEnumerator<T>` oder `IEnumerable<T>` ist `T`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="cfde9-1563">Enumerator-Objekte</span><span class="sxs-lookup"><span data-stu-id="cfde9-1563">Enumerator objects</span></span>

<span data-ttu-id="cfde9-1564">Wenn ein Funktionsmember ein Enumerator zurückgegeben, der den Typ mit einem Iteratorblock implementiert ist, wird das Funktionselement Aufrufen nicht sofort den Code im Iteratorblock ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="cfde9-1565">Stattdessen eine ***Enumeratorobjekt*** erstellt und zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="cfde9-1566">Dieses Objekt kapselt den Code im Iteratorblock angegeben und Ausführung des Codes im Iteratorblock tritt auf, wenn des Enumeratorobjekts `MoveNext` -Methode wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="cfde9-1567">Ein Enumeratorobjekt weist folgende Merkmale auf:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="cfde9-1568">Es implementiert `IEnumerator` und `IEnumerator<T>`, wobei `T` ist der Yield-Typ des Iterators.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="cfde9-1569">Sie implementiert `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="cfde9-1570">Initialisiert mit einer Kopie die Argumentwerte gelten (sofern vorhanden) und Instanzwert an das Funktionselement übergeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="cfde9-1571">Hat vier mögliche Zustände, ***vor***, ***ausführen***, ***angehalten***, und ***nach***, und befindet sich zunächst im der ***vor***  Zustand.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="cfde9-1572">Ein Enumeratorobjekt ist in der Regel eine Instanz einer vom Compiler generierter Enumerator-Klasse, die den Code im Iteratorblock kapselt und die Enumeratorschnittstellen implementiert, aber andere Methoden der Implementierung sind möglich.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="cfde9-1573">Wenn eine Enumeratorklasse, die vom Compiler generiert wird, wird diese Klasse geschachtelt werden, direkt oder indirekt in der Klasse, die mit das Funktionselement muss private zugriffsmöglichkeiten und muss einen Namen für die Verwendung durch den Compiler reserviert ([Bezeichner ](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="cfde9-1574">Ein Enumeratorobjekt kann mehrere Schnittstellen als die oben angegebenen implementieren.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="cfde9-1575">Die folgenden Abschnitte beschreiben das genaue Verhalten der der `MoveNext`, `Current`, und `Dispose` Mitglied der `IEnumerable` und `IEnumerable<T>` schnittstellenimplementierungen, die von einem Enumeratorobjekt bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="cfde9-1576">Beachten Sie, dass der Enumerator-Objekte nicht unterstützen die `IEnumerator.Reset` Methode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="cfde9-1577">Das Aufrufen dieser Methode bewirkt, dass eine `System.NotSupportedException` ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="cfde9-1578">Die MoveNext-Methode</span><span class="sxs-lookup"><span data-stu-id="cfde9-1578">The MoveNext method</span></span>

<span data-ttu-id="cfde9-1579">Die `MoveNext` Methode eines Enumerators-Objekts kapselt den Code der Iteratorblock.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="cfde9-1580">Aufrufen der `MoveNext` Methode führt Code in der Iteratorblock und legt die `Current` Eigenschaft des Enumeratorobjekts nach Bedarf.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="cfde9-1581">Der genaue vom durchgeführten Aktion `MoveNext` hängt vom Zustand des Enumeratorobjekts beim `MoveNext` aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="cfde9-1582">Wenn der Status des Enumeratorobjekts ist ***vor***, wird durch Aufrufen `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="cfde9-1583">Ändert den Zustand in ***ausführen***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="cfde9-1584">Die Parameter initialisiert (einschließlich `this`) des Iteratorblocks auf den Argumentwerten und Instanzwert gespeichert, wenn der Enumerator-Objekt initialisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="cfde9-1585">Führt Iteratorblock von Anfang an, bis die Ausführung unterbrochen wird (siehe Beschreibung unten).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="cfde9-1586">Wenn der Status des Enumeratorobjekts ist ***ausführen***, das Ergebnis des Aufrufs `MoveNext` ist nicht angegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="cfde9-1587">Wenn der Status des Enumeratorobjekts ist ***angehalten***, wird durch Aufrufen `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="cfde9-1588">Ändert den Zustand in ***ausführen***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="cfde9-1589">Stellt die Werte aller lokalen Variablen und Parameter, die (einschließlich dieses) wieder her Werte, die bei der Ausführung des Iteratorblocks zuletzt angehalten wurde.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="cfde9-1590">Beachten Sie, dass der Inhalt der alle Objekte, die auf diese Variablen verweist seit dem vorherigen Aufruf von MoveNext geändert haben können.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="cfde9-1591">Setzt die Ausführung der Iteratorblock unmittelbar nach der `yield return` -Anweisung, verursacht das Anhalten der Ausführung und wird fortgesetzt, bis die Ausführung unterbrochen wird (siehe Beschreibung unten).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="cfde9-1592">Wenn der Status des Enumeratorobjekts ist ***nach***, wird durch Aufrufen `MoveNext` gibt `false`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="cfde9-1593">Wenn `MoveNext` Iteratorblock führt auf vier Arten unterbrochen werden können: Durch eine `yield return` Anweisung, durch eine `yield break` Anweisung, durch das Ende der Iteratorblocks auftreten und eine Ausnahme ausgelöst und aus dem Iteratorblock übertragen wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="cfde9-1594">Wenn eine `yield return` -Anweisung gefunden ([die Yield-Anweisung](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="cfde9-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="cfde9-1595">Die in der Anweisung angegebene Ausdruck wird ausgewertet, implizit in den Typ "yield" konvertiert und zugewiesen der `Current` Eigenschaft des Enumeratorobjekts.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="cfde9-1596">Die Ausführung des iteratortexts wird angehalten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="cfde9-1597">Die Werte aller lokalen Variablen und Parameter (einschließlich `this`) gespeichert sind, wie der Speicherort dieser `yield return` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="cfde9-1598">Wenn die `yield return` -Anweisung ist in einem oder mehreren `try` blockiert wird, zugeordneten `finally` Blöcke werden zu diesem Zeitpunkt nicht ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="cfde9-1599">Der Status des Enumeratorobjekts wird geändert, um ***angehalten***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="cfde9-1600">Die `MoveNext` Methodenrückgabe `true` an den Aufrufer, der angibt, dass die Iteration erfolgreich auf den nächsten Wert gesetzt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="cfde9-1601">Wenn eine `yield break` -Anweisung gefunden ([die Yield-Anweisung](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="cfde9-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="cfde9-1602">Wenn die `yield break` -Anweisung ist in einem oder mehreren `try` blockiert wird, zugeordneten `finally` -blocke ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="cfde9-1603">Der Status des Enumeratorobjekts wird geändert, um ***nach***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="cfde9-1604">Die `MoveNext` Methodenrückgabe `false` an den Aufrufer, der angibt, dass die Iteration abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="cfde9-1605">Wenn das Ende des iteratortexts erreicht wird:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="cfde9-1606">Der Status des Enumeratorobjekts wird geändert, um ***nach***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="cfde9-1607">Die `MoveNext` Methodenrückgabe `false` an den Aufrufer, der angibt, dass die Iteration abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="cfde9-1608">Wenn eine Ausnahme ausgelöst und aus dem Iteratorblock übertragen:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="cfde9-1609">Entsprechende `finally` Blöcke im des iteratortexts werden durch die ausnahmeweitergabe ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="cfde9-1610">Der Status des Enumeratorobjekts wird geändert, um ***nach***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="cfde9-1611">Die ausnahmeweitergabe wird fortgesetzt, an den Aufrufer von der `MoveNext` Methode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="cfde9-1612">Die Current-Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="cfde9-1612">The Current property</span></span>

<span data-ttu-id="cfde9-1613">Ein Enumeratorobjekt `Current` Eigenschaft betroffen `yield return` Anweisungen im Iteratorblock.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="cfde9-1614">Wenn ein Enumeratorobjekt ist der ***angehalten*** Zustand, den Wert der `Current` ist der Wert festlegen, indem dem vorherigen Aufruf von `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="cfde9-1615">Wenn ein Enumeratorobjekt ist der ***vor***, ***ausgeführt***, oder ***nach*** angibt, das Ergebnis des Zugriffs auf `Current` ist nicht angegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="cfde9-1616">Geben Sie für einen Iterator mit einem "yield" als `object`, das Ergebnis des Zugriffs auf `Current` über des Enumeratorobjekts `IEnumerable` Implementierung entspricht, für den Zugriff auf `Current` über des Enumeratorobjekts `IEnumerator<T>` Implementierung und das Ergebnis umwandeln `object`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="cfde9-1617">Die Dispose-Methode</span><span class="sxs-lookup"><span data-stu-id="cfde9-1617">The Dispose method</span></span>

<span data-ttu-id="cfde9-1618">Die `Dispose` Methode wird verwendet, um die Iteration zu bereinigen, indem Sie direkt die Enumerator-Objekt an die ***nach*** Zustand.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="cfde9-1619">Wenn der Status des Enumeratorobjekts ist ***vor***, wird durch Aufrufen `Dispose` ändert den Zustand in ***nach***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="cfde9-1620">Wenn der Status des Enumeratorobjekts ist ***ausführen***, das Ergebnis des Aufrufs `Dispose` ist nicht angegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="cfde9-1621">Wenn der Status des Enumeratorobjekts ist ***angehalten***, wird durch Aufrufen `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="cfde9-1622">Ändert den Zustand in ***ausführen***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="cfde9-1623">Führt alle schließlich Blöcke, als ob die zuletzt ausgeführte `yield return` Anweisung wurden eine `yield break` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="cfde9-1624">Wenn dies bewirkt, eine Ausnahme ausgelöst und aus dem Iterator-Text übertragen werden dass, ist der Status des Enumeratorobjekts auf festgelegt ***nach*** und die Ausnahme wird an den Aufrufer der übertragen die `Dispose` Methode.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="cfde9-1625">Ändert den Zustand in ***nach***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="cfde9-1626">Wenn der Status des Enumeratorobjekts ist ***nach***, wird durch Aufrufen `Dispose` hat keine Auswirkungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="cfde9-1627">Aufzählbare Objekte</span><span class="sxs-lookup"><span data-stu-id="cfde9-1627">Enumerable objects</span></span>

<span data-ttu-id="cfde9-1628">Wenn ein Funktionsmember einen enumerable Schnittstellentyp zurückgeben mit einem Iteratorblock implementiert ist, wird das Funktionselement Aufrufen nicht sofort den Code im Iteratorblock ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="cfde9-1629">Stattdessen eine ***aufzählbares Objekt*** erstellt und zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="cfde9-1630">Des aufzählbaren Objekts `GetEnumerator` Methode gibt ein Enumeratorobjekt, das den Code kapselt im Iteratorblock angegeben und Ausführung des Codes im Iteratorblock tritt auf, wenn des Enumeratorobjekts `MoveNext` -Methode wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="cfde9-1631">Ein aufzählbares Objekt weist folgende Merkmale auf:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="cfde9-1632">Es implementiert `IEnumerable` und `IEnumerable<T>`, wobei `T` ist der Yield-Typ des Iterators.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="cfde9-1633">Initialisiert mit einer Kopie die Argumentwerte gelten (sofern vorhanden) und Instanzwert an das Funktionselement übergeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="cfde9-1634">Ein aufzählbares Objekt ist in der Regel eine Instanz einer vom Compiler generierter enumerable-Klasse, die den Code im Iteratorblock kapselt und die enumerable-Schnittstellen implementiert, aber andere Methoden der Implementierung sind möglich.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="cfde9-1635">Wenn eine enumerable-Klasse, die vom Compiler generiert wird, wird diese Klasse geschachtelt werden, direkt oder indirekt in der Klasse, die mit das Funktionselement muss private zugriffsmöglichkeiten und muss einen Namen für die Verwendung durch den Compiler reserviert ([Bezeichner ](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="cfde9-1636">Ein aufzählbares Objekt möglicherweise mehr als die oben angegebenen Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="cfde9-1637">Insbesondere kann ein aufzählbares Objekt auch implementieren `IEnumerator` und `IEnumerator<T>`, er ermöglicht, sowohl als ein aufzählbares Element einen Enumerator dient.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="cfde9-1638">In dieser Art der Implementierung, die beim ersten Öffnen eines aufzählbaren Objekts `GetEnumerator` Methode wird aufgerufen, das aufzählbare Objekt selbst wird zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="cfde9-1639">Nachfolgende Aufrufe von des aufzählbaren Objekts `GetEnumerator`, sofern vorhanden, die eine Kopie des aufzählbaren Objekts zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="cfde9-1640">Daher zurückgegeben jeder Enumerator hat seinen eigenen Status und Änderungen in einem Enumerator hat keinen Einfluss auf einen anderen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="cfde9-1641">Der GetEnumerator-Methode</span><span class="sxs-lookup"><span data-stu-id="cfde9-1641">The GetEnumerator method</span></span>

<span data-ttu-id="cfde9-1642">Ein aufzählbares Objekt stellt eine Implementierung der `GetEnumerator` Methoden der `IEnumerable` und `IEnumerable<T>` Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="cfde9-1643">Die beiden `GetEnumerator` Methoden gemeinsam nutzen, eine gängige Implementierung, die reserviert, und gibt ein verfügbaren Enumeratorobjekt zurück.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="cfde9-1644">Das Enumeratorobjekt mit den Argumentwerten initialisiert wird und der Instanz ein Wert gespeichert, wenn das aufzählbare Objekt initialisiert, aber ansonsten wurde die Enumerator-Objekt-Funktionen wie in beschrieben [Enumeratorobjekte](classes.md#enumerator-objects).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="cfde9-1645">Beispiel für die Implementierung</span><span class="sxs-lookup"><span data-stu-id="cfde9-1645">Implementation example</span></span>

<span data-ttu-id="cfde9-1646">Dieser Abschnitt beschreibt eine mögliche Implementierung der Iteratoren in Bezug auf die standardmäßige C#-Konstrukte.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="cfde9-1647">Die hier beschriebene Implementierung basiert darauf, dass die gleichen Prinzipien, die vom Microsoft C#-Compiler verwendet, aber es ist keine beauftragten Implementierung oder das einzig mögliche.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="cfde9-1648">Die folgenden `Stack<T>` -Klasse implementiert die `GetEnumerator` Methode, die mithilfe eines Iterators.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="cfde9-1649">Der Iterator Listet die Elemente des Stapels in von oben nach unten.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

<span data-ttu-id="cfde9-1650">Die `GetEnumerator` Methode kann in eine Instanziierung einer vom Compiler generierter Enumerator-Klasse, die den Code im Iteratorblock kapselt übersetzt werden, wie im folgenden dargestellt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="cfde9-1651">Der Code im Iteratorblock in einen Zustandsautomaten aktiviert ist, und die in der vorherigen Übersetzung, platziert Sie der `MoveNext` -Methode der Enumerator-Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="cfde9-1652">Darüber hinaus die lokale Variable `i` in ein Feld im Enumerator-Objekt aktiviert ist, damit er fortfahren kann, über Aufrufe der vorhanden sein, `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="cfde9-1653">Im folgende Beispiel wird eine einfache Tabelle der Multiplikation von ganzen Zahlen 1 bis 10 gedruckt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="cfde9-1654">Die `FromTo` -Methode im Beispiel gibt ein aufzählbares Objekt zurück und wird mithilfe eines Iterators implementiert.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="cfde9-1655">Die `FromTo` Methode kann in eine Instanziierung einer vom Compiler generierter enumerable-Klasse, die den Code im Iteratorblock kapselt übersetzt werden, wie im folgenden dargestellt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="cfde9-1656">Die enumerable-Klasse implementiert sowohl die enumerable-Schnittstellen als auch die Enumeratorschnittstellen, die er ermöglicht, sowohl als ein aufzählbares Element einen Enumerator dient.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="cfde9-1657">Beim ersten die `GetEnumerator` Methode wird aufgerufen, das aufzählbare Objekt selbst wird zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="cfde9-1658">Nachfolgende Aufrufe von des aufzählbaren Objekts `GetEnumerator`, sofern vorhanden, die eine Kopie des aufzählbaren Objekts zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="cfde9-1659">Daher zurückgegeben jeder Enumerator hat seinen eigenen Status und Änderungen in einem Enumerator hat keinen Einfluss auf einen anderen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="cfde9-1660">Die `Interlocked.CompareExchange` Methode wird verwendet, um die Thread-sichere Betrieb sicherzustellen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="cfde9-1661">Die `from` und `to` Parameter werden in die Felder in der enumerable-Klasse umgewandelt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="cfde9-1662">Da `from` geändert wird, im Iteratorblock, eine zusätzliche `__from` Feld wird eingeführt, um den ersten Wert speichern `from` in foreach-Enumerator.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="cfde9-1663">Die `MoveNext` -Methode löst eine `InvalidOperationException` , wenn er, wenn aufgerufen wird `__state` ist `0`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="cfde9-1664">Dies schützt vor der Verwendung des aufzählbaren Objekts als ein Enumeratorobjekt erst nach Aufrufen von `GetEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="cfde9-1665">Das folgende Beispiel zeigt eine einfache Struktur-Klasse.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="cfde9-1666">Die `Tree<T>` -Klasse implementiert die `GetEnumerator` Methode, die mithilfe eines Iterators.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="cfde9-1667">Der Iterator Listet die Elemente der Struktur in Infix-Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="cfde9-1668">Die `GetEnumerator` Methode kann in eine Instanziierung einer vom Compiler generierter Enumerator-Klasse, die den Code im Iteratorblock kapselt übersetzt werden, wie im folgenden dargestellt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="cfde9-1669">Die vom Compiler generierten temporären Dateien, die in verwendet die `foreach` Anweisungen werden herausgehoben, in der `__left` und `__right` Felder des Enumeratorobjekts.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="cfde9-1670">Die `__state` Feld des Enumeratorobjekts sorgfältig aktualisiert, damit die richtige `Dispose()` Methode wird ordnungsgemäß aufgerufen werden, wenn eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="cfde9-1671">Beachten Sie, dass es nicht möglich, schreiben den übersetzten Code mit einfachen `foreach` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="cfde9-1672">Asynchrone Funktionen</span><span class="sxs-lookup"><span data-stu-id="cfde9-1672">Async functions</span></span>

<span data-ttu-id="cfde9-1673">Eine Methode ([Methoden](classes.md#methods)) oder eine anonyme Funktion ([anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions)) mit der `async` Modifizierer wird aufgerufen, eine ***Async-Funktion***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="cfde9-1674">Im Allgemeinen den Begriff ***Async*** wird verwendet, um jede Art von Funktion beschreiben, das die `async` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="cfde9-1675">Es ist ein Fehler während der Kompilierung für die Liste der formalen Parameter zum Angeben einer Async-Funktion `ref` oder `out` Parameter.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="cfde9-1676">Die *Return_type* von Async-Methode muss entweder `void` oder ***Aufgabentyp***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="cfde9-1677">Die Tasktypen sind `System.Threading.Tasks.Task` und Typen aus `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="cfde9-1678">Aus Gründen der Übersichtlichkeit sind in diesem Kapitel diese Typen verwiesen wird als `Task` und `Task<T>`bzw.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="cfde9-1679">Eine asynchrone Methode zurückgeben von Typ "Task" wird als Task zurückgeben bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="cfde9-1680">Die genaue Definition der Tasktypen ist die Implementierung definiert, aber aus Sicht der Sprache wird Typ "Task" in einem der Zustände nicht abgeschlossen wurde, erfolgreich war oder fehlgeschlagen ist.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="cfde9-1681">Eine fehlgeschlagene Aufgabe zeichnet eine wichtige Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="cfde9-1682">Eine erfolgreiche `Task<T>` zeichnet ein Ergebnis vom Typ `T`.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="cfde9-1683">Tasktypen sind "awaitable", und können daher die Operanden des await-Ausdrücken ([Await-Ausdrücken](expressions.md#await-expressions)).</span><span class="sxs-lookup"><span data-stu-id="cfde9-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="cfde9-1684">Eine Async-Funktionsaufruf bietet die Möglichkeit, die Auswertung Anhalten von await-Ausdrücken ([Await-Ausdrücken](expressions.md#await-expressions)) in ihrem Text.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="cfde9-1685">Auswertung kann später fortgesetzt werden, zum Zeitpunkt der anhalten await-Ausdruck von einem ***Wiederaufnahmedelegat***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="cfde9-1686">Der Wiederaufnahmedelegat ist vom Typ `System.Action`, und wenn er aufgerufen wird, wird Auswertung der Async-Funktionsaufruf aus der Await-Ausdruck, der an der Stelle fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="cfde9-1687">Die ***aktuellen Aufrufer*** einer asynchronen Funktion Aufruf ist des ursprünglichen Aufrufers, wenn der Funktionsaufruf nie angehalten wurde, oder der letzte Aufrufer der Wiederaufnahmedelegat andernfalls.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="cfde9-1688">Eine Aufgabe zurückgibt Async-Funktion</span><span class="sxs-lookup"><span data-stu-id="cfde9-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="cfde9-1689">Aufruf einer Aufgabe zurückgibt Async-Funktion bewirkt, dass eine Instanz des Typs die zurückgegebene Aufgabe generiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="cfde9-1690">Dies wird aufgerufen, die ***Task zurückgeben*** der Async-Funktion.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="cfde9-1691">Der Task ist anfangs in einem unvollständigen Status.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="cfde9-1692">Der Hauptteil der Async-Funktion wird dann ausgewertet bis sie (durch erreichen einen Await-Ausdruck) entweder angehalten oder beendet wird, an dem Verwaltungspunkt-Steuerungs an den Aufrufer, zusammen mit dem Rückgabewert Task zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="cfde9-1693">Wenn der Hauptteil der Async-Funktion beendet wird, wird die return-Aufgabe beendet den unvollständigen Zustand verschoben:</span><span class="sxs-lookup"><span data-stu-id="cfde9-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="cfde9-1694">Wenn der Hauptteil der Funktion als Ergebnis erreicht wird, eine return-Anweisung oder das Ende des Texts beendet wird, wird alle Ergebniswert in der Aufgabe zurückgeben aufgezeichnet, die in einem Zustand "erfolgreich" versetzt wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="cfde9-1695">Wenn der Hauptteil der Funktion als Ergebnis einer nicht abgefangenen Ausnahme beendet wird ([die Throw-Anweisung](statements.md#the-throw-statement)) wird die Ausnahme in der return-Aufgabe das in einem fehlerhaften Zustand versetzt wird aufgezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="cfde9-1696">Auswertung einer "void" zurückgebende asynchrone-Funktion</span><span class="sxs-lookup"><span data-stu-id="cfde9-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="cfde9-1697">Wenn der Rückgabetyp der asynchronen Funktion `void`, Auswertung unterscheidet sich von den oben genannten auf folgende Weise: Da keine Aufgabe zurückgegeben wird, kommuniziert die Funktion Vervollständigung und Ausnahmen für des aktuellen Threads stattdessen ***Synchronisierungskontext***.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="cfde9-1698">Die genaue Definition von Synchronisierungskontext ist implementierungsabhängig, aber es ist eine Darstellung des "where" der aktuelle Thread ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="cfde9-1699">Der Synchronisierungskontext wird benachrichtigt, wenn es sich bei Auswertung einer "void" zurückgebende asynchrone Funktion beginnt, wird erfolgreich abgeschlossen oder führt dazu, dass eine nicht abgefangene Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="cfde9-1700">Dadurch wird den Kontext, behalten Sie verfolgen, wie viele "void" zurückgebende asynchrone Funktionen, darunter ausgeführt werden, und klicken Sie, wie Ausnahmen, die sie stammenden weitergegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="cfde9-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
