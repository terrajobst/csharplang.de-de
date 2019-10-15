---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703982"
---
# <a name="classes"></a><span data-ttu-id="6443e-101">Klassen</span><span class="sxs-lookup"><span data-stu-id="6443e-101">Classes</span></span>

<span data-ttu-id="6443e-102">Eine-Klasse ist eine Datenstruktur, die Datenmember (Konstanten und Felder), Funktionsmember (Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren, Instanzkonstruktoren, Dekonstruktoren und statische Konstruktoren) und die in der Struktur enthaltenen Typen enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="6443e-103">Klassentypen unterstützen Vererbung, einen Mechanismus, mit dem eine abgeleitete Klasse eine Basisklasse erweitern und spezialisieren kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="6443e-104">Klassen Deklarationen</span><span class="sxs-lookup"><span data-stu-id="6443e-104">Class declarations</span></span>

<span data-ttu-id="6443e-105">Ein *class_declaration* ist ein *type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), der eine neue Klasse deklariert.</span><span class="sxs-lookup"><span data-stu-id="6443e-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="6443e-106">Ein *class_declaration* besteht aus einem optionalen Satz von *Attributen* ([Attributen](attributes.md)), gefolgt von einem optionalen Satz von *class_modifier*s ([Klassenmodifizierer](classes.md#class-modifiers)), gefolgt von einem optionalen `partial`-Modifizierer, gefolgt vom-Schlüsselwort. `class` und ein *Bezeichner* , der die-Klasse benennt, gefolgt von einem optionalen *type_parameter_list* ([Typparameter](classes.md#type-parameters)), gefolgt von einer optionalen *class_base* -Spezifikation ([Klassenbasis Spezifikation](classes.md#class-base-specification)), gefolgt von ein optionaler Satz von *type_parameter_constraints_clause*s ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)), gefolgt von einem *class_body* ([Klassen Text](classes.md#class-body)), optional gefolgt von einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="6443e-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="6443e-107">Eine Klassen Deklaration kann keine *type_parameter_constraints_clause*s bereitstellen, es sei denn, Sie stellt auch ein *type_parameter_list*</span><span class="sxs-lookup"><span data-stu-id="6443e-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="6443e-108">Eine Klassen Deklaration, die ein *type_parameter_list* bereitstellt, ist eine ***generische Klassen Deklaration***.</span><span class="sxs-lookup"><span data-stu-id="6443e-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="6443e-109">Außerdem ist jede Klasse, die in einer generischen Klassen Deklaration oder generischen Struktur Deklaration geschachtelt ist, selbst eine generische Klassen Deklaration, da Typparameter für den enthaltenden Typ angegeben werden müssen, um einen konstruierten Typ zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6443e-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="6443e-110">Klassenmodifizierer</span><span class="sxs-lookup"><span data-stu-id="6443e-110">Class modifiers</span></span>

<span data-ttu-id="6443e-111">Ein *class_declaration* kann optional eine Sequenz von Klassenmodifizierer einschließen:</span><span class="sxs-lookup"><span data-stu-id="6443e-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

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

<span data-ttu-id="6443e-112">Es ist ein Kompilierzeitfehler, damit derselbe Modifizierer mehrmals in einer Klassen Deklaration angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="6443e-113">Der `new` -Modifizierer ist für-Klassen zulässig.</span><span class="sxs-lookup"><span data-stu-id="6443e-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="6443e-114">Er gibt an, dass die Klasse einen geerbten Member mit demselben Namen verbirgt, wie im [neuen Modifizierer](classes.md#the-new-modifier)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="6443e-115">Es ist ein Kompilierzeitfehler, damit `new` der-Modifizierer in einer Klassen Deklaration angezeigt wird, die keine Klassen Deklaration ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="6443e-116">Die `public`Modifizierer `protected`, `private` , `internal`und Steuern den Zugriff auf die-Klasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="6443e-117">Abhängig vom Kontext, in dem die Klassen Deklaration auftritt, sind einige dieser Modifizierer möglicherweise nicht zulässig (als[Barrierefreiheit deklariert](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="6443e-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="6443e-118">Die `abstract`modifiziererer, `sealed` und `static` werden in den folgenden Abschnitten erläutert.</span><span class="sxs-lookup"><span data-stu-id="6443e-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="6443e-119">Abstrakte Klassen</span><span class="sxs-lookup"><span data-stu-id="6443e-119">Abstract classes</span></span>

<span data-ttu-id="6443e-120">Der `abstract` -Modifizierer wird verwendet, um anzugeben, dass eine Klasse unvollständig ist und nur als Basisklasse verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="6443e-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="6443e-121">Eine abstrakte Klasse unterscheidet sich wie folgt von einer nicht abstrakten Klasse:</span><span class="sxs-lookup"><span data-stu-id="6443e-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="6443e-122">Eine abstrakte Klasse kann nicht direkt instanziiert werden, und es handelt sich um einen Kompilierzeitfehler `new` , wenn der Operator für eine abstrakte Klasse verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="6443e-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="6443e-123">Obwohl es möglich ist, Variablen und Werte zu haben, deren Kompilier Zeittypen abstrakt sind, sind diese Variablen und Werte `null` notwendigerweise entweder ein oder enthalten Verweise auf Instanzen von nicht abstrakten Klassen, die von den abstrakten Typen abgeleitet sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="6443e-124">Eine abstrakte Klasse ist zulässig (jedoch nicht erforderlich), um abstrakte Member zu enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="6443e-125">Eine abstrakte Klasse kann nicht versiegelt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="6443e-126">Wenn eine nicht abstrakte Klasse von einer abstrakten Klasse abgeleitet wird, muss die nicht abstrakte Klasse tatsächliche Implementierungen aller geerbten abstrakten Member enthalten, wodurch diese abstrakten Member überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="6443e-127">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-127">In the example</span></span>
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
<span data-ttu-id="6443e-128">die abstrakte- `A` Klasse führt eine abstrakte `F`Methode ein.</span><span class="sxs-lookup"><span data-stu-id="6443e-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="6443e-129">Die `B` -Klasse führt eine `G`zusätzliche-Methode ein, aber da Sie keine `F`Implementierung `B` von bereitstellt, muss auch als abstrakt deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="6443e-130">Die `C` Klasse über `F` schreibt und stellt eine tatsächliche Implementierung bereit.</span><span class="sxs-lookup"><span data-stu-id="6443e-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="6443e-131">Da keine abstrakten Member in `C`vorhanden sind, `C` ist zulässig (jedoch nicht erforderlich), um nicht abstrakt zu sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="6443e-132">Versiegelte Klassen</span><span class="sxs-lookup"><span data-stu-id="6443e-132">Sealed classes</span></span>

<span data-ttu-id="6443e-133">Der `sealed` -Modifizierer wird verwendet, um die Ableitung von einer Klasse zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="6443e-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="6443e-134">Ein Kompilierzeitfehler tritt auf, wenn eine versiegelte Klasse als Basisklasse einer anderen Klasse angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="6443e-135">Eine versiegelte Klasse kann nicht auch eine abstrakte Klasse sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="6443e-136">Der `sealed` -Modifizierer wird hauptsächlich verwendet, um eine unbeabsichtigte Ableitung zu verhindern, er ermöglicht aber auch bestimmte Lauf Zeit Optimierungen.</span><span class="sxs-lookup"><span data-stu-id="6443e-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="6443e-137">Insbesondere weil eine versiegelte Klasse bekanntermaßen keine abgeleiteten Klassen hat, ist es möglich, die Aufrufe virtueller Funktionsmember für versiegelte Klassen Instanzen in nicht virtuelle Aufrufe umzuwandeln.</span><span class="sxs-lookup"><span data-stu-id="6443e-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="6443e-138">Statische Klassen</span><span class="sxs-lookup"><span data-stu-id="6443e-138">Static classes</span></span>

<span data-ttu-id="6443e-139">Der `static` -Modifizierer wird verwendet, um die Klasse zu markieren, die als ***statische Klasse***deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="6443e-140">Eine statische Klasse kann nicht instanziiert werden, kann nicht als Typ verwendet werden und darf nur statische Member enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="6443e-141">Nur eine statische Klasse kann Deklarationen von Erweiterungs Methoden ([Erweiterungs Methoden](classes.md#extension-methods)) enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="6443e-142">Eine statische Klassen Deklaration unterliegt den folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="6443e-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="6443e-143">Eine statische Klasse darf keinen-Modifizierer `sealed` oder `abstract` -Modifizierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="6443e-144">Beachten Sie jedoch, dass eine statische Klasse, die nicht von instanziiert oder abgeleitet werden kann, so verhält, als ob Sie sowohl versiegelt als auch abstrakt wäre.</span><span class="sxs-lookup"><span data-stu-id="6443e-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="6443e-145">Eine statische Klasse darf keine *class_base* Specification ([Klassenbasis Spezifikation](classes.md#class-base-specification)) enthalten und kann weder eine Basisklasse noch eine Liste implementierter Schnittstellen explizit angeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="6443e-146">Eine statische Klasse erbt implizit vom Typ `object`.</span><span class="sxs-lookup"><span data-stu-id="6443e-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="6443e-147">Eine statische Klasse kann nur statische Member ([statische Member und Instanzmember](classes.md#static-and-instance-members)) enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="6443e-148">Beachten Sie, dass Konstanten und Untertypen als statische Member klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="6443e-149">Eine statische Klasse kann keine Member mit `protected` oder `protected internal` deklarierter Barrierefreiheit haben.</span><span class="sxs-lookup"><span data-stu-id="6443e-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="6443e-150">Es handelt sich um einen Kompilierzeitfehler, der gegen diese Einschränkungen verstößt.</span><span class="sxs-lookup"><span data-stu-id="6443e-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="6443e-151">Eine statische Klasse hat keine Instanzkonstruktoren.</span><span class="sxs-lookup"><span data-stu-id="6443e-151">A static class has no instance constructors.</span></span> <span data-ttu-id="6443e-152">Es ist nicht möglich, einen Instanzkonstruktor in einer statischen Klasse zu deklarieren, und für eine statische Klasse wird kein Standardinstanzkonstruktor ([Standardkonstruktoren](classes.md#default-constructors)) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="6443e-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="6443e-153">Die Member einer statischen Klasse sind nicht automatisch statisch, und die Element Deklarationen müssen explizit einen `static` Modifizierer einschließen (mit Ausnahme von Konstanten und Typen).</span><span class="sxs-lookup"><span data-stu-id="6443e-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="6443e-154">Wenn eine Klasse in einer statischen äußeren Klasse geschachtelt ist, ist die geschachtelte Klasse keine statische Klasse, es sei denn `static` , Sie enthält explizit einen Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6443e-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="6443e-155">__Verweisen auf statische Klassentypen__</span><span class="sxs-lookup"><span data-stu-id="6443e-155">__Referencing static class types__</span></span>

<span data-ttu-id="6443e-156">Ein *namespace_or_type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)) darf auf eine statische Klasse verweisen, wenn</span><span class="sxs-lookup"><span data-stu-id="6443e-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="6443e-157">*Namespace_or_type_name* ist der `T` in einem *namespace_or_type_name* -Format `T.I` oder</span><span class="sxs-lookup"><span data-stu-id="6443e-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="6443e-158">*Namespace_or_type_name* ist der `T` in einer *typeof_expression* ([Argument Liste](expressions.md#argument-lists)1) der Form `typeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="6443e-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="6443e-159">Ein *primary_expression* ([Funktionsmember](expressions.md#function-members)) darf auf eine statische Klasse verweisen, wenn</span><span class="sxs-lookup"><span data-stu-id="6443e-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="6443e-160">*Primary_expression* ist der `E` in einer *member_access* ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) der Form `E.I`.</span><span class="sxs-lookup"><span data-stu-id="6443e-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="6443e-161">In jedem anderen Kontext ist es ein Kompilierzeitfehler, um auf eine statische Klasse zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="6443e-162">Es ist z. b. ein Fehler für eine statische Klasse, die als Basisklasse, als konstituierender Typ (in Form von[Typen](classes.md#nested-types)) eines Members, als generisches Typargument oder als Typparameter Einschränkung verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="6443e-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="6443e-163">Ebenso kann eine statische Klasse nicht in einem Arraytyp, einem Zeigertyp, einem `new` Ausdruck, einem Umwandlungs Ausdruck, `is` einem Ausdruck, `as` einem Ausdruck, `sizeof` einem Ausdruck oder einem Standardwert Ausdruck verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="6443e-164">Partieller Modifizierer</span><span class="sxs-lookup"><span data-stu-id="6443e-164">Partial modifier</span></span>

<span data-ttu-id="6443e-165">Der `partial`-Modifizierer wird verwendet, um anzugeben, dass dieses *class_declaration* eine partielle Typdeklaration ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="6443e-166">Mehrere partielle Typdeklarationen mit demselben Namen innerhalb eines einschließenden Namespace oder einer Typdeklaration kombinieren eine Typdeklaration, die den in [partiellen Typen](classes.md#partial-types)angegebenen Regeln folgt.</span><span class="sxs-lookup"><span data-stu-id="6443e-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="6443e-167">Die Deklaration einer Klasse, die über separate Segmente von Programmtext verteilt ist, kann nützlich sein, wenn diese Segmente in verschiedenen Kontexten erstellt oder verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="6443e-168">Beispielsweise kann ein Teil einer Klassen Deklaration maschinell generiert werden, während der andere manuell erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="6443e-169">Die Text Trennung der beiden verhindert, dass Updates durch eine in Konflikt mit Updates durch die andere verursacht werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="6443e-170">Typparameter</span><span class="sxs-lookup"><span data-stu-id="6443e-170">Type parameters</span></span>

<span data-ttu-id="6443e-171">Ein Typparameter ist ein einfacher Bezeichner, der einen Platzhalter für ein Typargument angibt, das zum Erstellen eines konstruierten Typs bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="6443e-172">Ein Typparameter ist ein formaler Platzhalter für einen Typ, der später bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="6443e-173">Im Gegensatz dazu ist ein[Typargument (Typargumente](types.md#type-arguments)) der tatsächliche Typ, der beim Erstellen eines konstruierten Typs den Typparameter ersetzt.</span><span class="sxs-lookup"><span data-stu-id="6443e-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

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

<span data-ttu-id="6443e-174">Jeder Typparameter in einer Klassen Deklaration definiert einen Namen im Deklarations Raum ([Deklarationen](basic-concepts.md#declarations)) dieser Klasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="6443e-175">Daher kann er nicht denselben Namen wie ein anderer Typparameter oder ein Member haben, der in dieser Klasse deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="6443e-176">Ein Typparameter kann nicht den gleichen Namen haben wie der Typ selbst.</span><span class="sxs-lookup"><span data-stu-id="6443e-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="6443e-177">Klassenbasis Spezifikation</span><span class="sxs-lookup"><span data-stu-id="6443e-177">Class base specification</span></span>

<span data-ttu-id="6443e-178">Eine Klassen Deklaration kann eine *class_base* -Spezifikation enthalten, die die direkte Basisklasse der Klasse und die Schnittstellen ([Schnittstellen](interfaces.md)) definiert, die von der-Klasse direkt implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

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

<span data-ttu-id="6443e-179">Die in einer Klassen Deklaration angegebene Basisklasse kann ein konstruierter Klassentyp ([konstruierte Typen](types.md#constructed-types)) sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="6443e-180">Eine Basisklasse kann nicht eigenständig ein Typparameter sein, Sie kann jedoch die Typparameter enthalten, die sich im Gültigkeitsbereich befinden.</span><span class="sxs-lookup"><span data-stu-id="6443e-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="6443e-181">Basisklassen</span><span class="sxs-lookup"><span data-stu-id="6443e-181">Base classes</span></span>

<span data-ttu-id="6443e-182">Wenn ein *class_type* in der *class_base*enthalten ist, gibt es die direkte Basisklasse der Klasse an, die deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="6443e-183">Wenn eine Klassen Deklaration keine *class_base*hat oder wenn die *class_base* nur Schnittstellentypen auflistet, wird davon ausgegangen, dass die direkte Basisklasse `object` ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="6443e-184">Eine Klasse erbt Member von ihrer direkten Basisklasse, wie in [Vererbung](classes.md#inheritance)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="6443e-185">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="6443e-186">die `A` Klasse ist die direkte Basisklasse von `B`, und `B` wird als abgeleitet `A`bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="6443e-187">Da `A` nicht explizit eine direkte Basisklasse angibt, ist die direkte Basisklasse implizit `object`.</span><span class="sxs-lookup"><span data-stu-id="6443e-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="6443e-188">Wenn eine Basisklasse für einen konstruierten Klassentyp in der Deklaration der generischen Klasse angegeben wird, wird die Basisklasse des konstruierten Typs abgerufen, indem für jede *type_parameter* in der Basisklassen Deklaration der entsprechende *type_argument* des konstruierten Typs.</span><span class="sxs-lookup"><span data-stu-id="6443e-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="6443e-189">Bei Angabe der generischen Klassen Deklarationen</span><span class="sxs-lookup"><span data-stu-id="6443e-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="6443e-190">die Basisklasse des konstruierten Typs `G<int>` `B<string,int[]>`wäre.</span><span class="sxs-lookup"><span data-stu-id="6443e-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="6443e-191">Die direkte Basisklasse eines Klassen Typs muss mindestens so zugänglich sein wie der Klassentyp selbst ([Barrierefreiheits Domänen](basic-concepts.md#accessibility-domains)).</span><span class="sxs-lookup"><span data-stu-id="6443e-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="6443e-192">Beispielsweise ist es ein Kompilierzeitfehler für eine `public` Klasse, die von einer `private` -Klasse `internal` oder-Klasse abgeleitet werden soll.</span><span class="sxs-lookup"><span data-stu-id="6443e-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="6443e-193">Die direkte Basisklasse eines Klassen Typs darf keinem der folgenden Typen sein: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`oder `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="6443e-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="6443e-194">Darüber hinaus kann eine generische Klassen Deklaration `System.Attribute` nicht als direkte oder indirekte Basisklasse verwenden.</span><span class="sxs-lookup"><span data-stu-id="6443e-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="6443e-195">Bei der Bestimmung der Bedeutung der direkten Basisklassen Spezifikation `A` einer Klasse `B`wird die direkte Basisklasse von `B` vorübergehend `object`als festgelegt.</span><span class="sxs-lookup"><span data-stu-id="6443e-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="6443e-196">Intuitiv wird dadurch sichergestellt, dass die Bedeutung einer Basisklassen Spezifikation nicht rekursiv von sich selbst abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="6443e-197">Das Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6443e-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="6443e-198">`A<C.B>` ist fehlerhaft `object`, da in der Basisklassen Spezifikation die direkte Basisklasse `C` von als betrachtet wird und daher (durch die Regeln von [Namespace-und Typnamen](basic-concepts.md#namespace-and-type-names)) `C` nicht als Member `B`angesehenwird.</span><span class="sxs-lookup"><span data-stu-id="6443e-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="6443e-199">Die Basisklassen eines Klassen Typs sind die direkte Basisklasse und deren Basisklassen.</span><span class="sxs-lookup"><span data-stu-id="6443e-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="6443e-200">Mit anderen Worten: der Satz von Basisklassen ist der transitiv Abschluss der direkten Basisklassen Beziehung.</span><span class="sxs-lookup"><span data-stu-id="6443e-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="6443e-201">Im obigen Beispiel sind `B` `A` die Basisklassen von und `object`.</span><span class="sxs-lookup"><span data-stu-id="6443e-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="6443e-202">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="6443e-203">die Basisklassen von `D<int>` sind `C<int[]>`, `B<IComparable<int[]>>`, `A`und. `object`</span><span class="sxs-lookup"><span data-stu-id="6443e-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="6443e-204">Mit Ausnahme von `object`Class hat jeder Klassentyp genau eine direkte Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="6443e-205">Die `object` Klasse verfügt über keine direkte Basisklasse und ist die ultimative Basisklasse aller anderen Klassen.</span><span class="sxs-lookup"><span data-stu-id="6443e-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="6443e-206">Wenn eine Klasse `B` von einer Klasse `A`abgeleitet ist, ist dies ein `A` Kompilier `B`Zeitfehler, von dem abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="6443e-207">Eine Klasse ***hängt direkt*** von ihrer direkten Basisklasse (sofern vorhanden) ab und ***hängt direkt*** von der Klasse ab, in der Sie sofort geschachtelt ist (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="6443e-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="6443e-208">Bei dieser Definition ist der gesamte Satz von Klassen, von dem eine Klasse abhängt, die reflexive und transitiv Schließung der ***direkt*** von Beziehung abhängig.</span><span class="sxs-lookup"><span data-stu-id="6443e-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="6443e-209">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="6443e-210">ist fehlerhaft, da die-Klasse von sich selbst abhängt.</span><span class="sxs-lookup"><span data-stu-id="6443e-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="6443e-211">Ebenso ist das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="6443e-212">ist fehlerhaft, da die Klassen zirkulär von sich selbst abhängen.</span><span class="sxs-lookup"><span data-stu-id="6443e-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="6443e-213">Abschließend wird das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="6443e-214">`A` führt zu `B.C` einem Kompilierzeitfehler, da von (seiner direkten Basisklasse `B` ) abhängt, das von (seiner unmittelbar einschließenden `A`Klasse) abhängt, von dem zirkulär abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="6443e-215">Beachten Sie, dass eine Klasse nicht von den Klassen abhängig ist, die darin geschachtelt sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="6443e-216">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="6443e-217">`B``B` `B` `A` hängt von ab `A`(da sowohl die direkte Basisklasse als auch die unmittelbar einschließende Klasse ist), aber nicht von abhängt (da weder eine Basisklasse noch eine einschließende Klasse von ist). `A` `A`).</span><span class="sxs-lookup"><span data-stu-id="6443e-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="6443e-218">Daher ist das Beispiel gültig.</span><span class="sxs-lookup"><span data-stu-id="6443e-218">Thus, the example is valid.</span></span>

<span data-ttu-id="6443e-219">Es ist nicht möglich, von einer `sealed` Klasse abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="6443e-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="6443e-220">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="6443e-221">die `B` Klasse ist fehlerhaft, da Sie versucht, von `sealed` der `A`-Klasse abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="6443e-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="6443e-222">Schnittstellenimplementierungen</span><span class="sxs-lookup"><span data-stu-id="6443e-222">Interface implementations</span></span>

<span data-ttu-id="6443e-223">Eine *class_base* -Spezifikation kann eine Liste von Schnittstellentypen enthalten. in diesem Fall wird die Klasse so genannte, dass die angegebenen Schnittstellentypen direkt implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="6443e-224">Schnittstellen Implementierungen werden in [Schnittstellen Implementierungen](interfaces.md#interface-implementations)ausführlicher erläutert.</span><span class="sxs-lookup"><span data-stu-id="6443e-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="6443e-225">Typparameter Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="6443e-225">Type parameter constraints</span></span>

<span data-ttu-id="6443e-226">Generische Typ-und Methoden Deklarationen können optional Typparameter Einschränkungen durch Einschließen von *type_parameter_constraints_clause*s angeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

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

<span data-ttu-id="6443e-227">Jede *type_parameter_constraints_clause* besteht aus dem Token `where`, gefolgt vom Namen eines Typparameters, gefolgt von einem Doppelpunkt und der Liste der Einschränkungen für diesen Typparameter.</span><span class="sxs-lookup"><span data-stu-id="6443e-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="6443e-228">Für jeden Typparameter kann höchstens `where` eine Klausel vorhanden sein, und die `where` Klauseln können in beliebiger Reihenfolge aufgelistet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="6443e-229">Wie das `get` - `set` Token und das-Token in einem Eigenschaften `where` Accessor ist das Token kein Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="6443e-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="6443e-230">Die Liste der Einschränkungen, die in `where` einer-Klausel angegeben werden, kann eine der folgenden Komponenten in dieser Reihenfolge enthalten: eine einzelne primäre Einschränkung, eine oder mehrere sekundäre Einschränkungen und die `new()`Konstruktoreinschränkung.</span><span class="sxs-lookup"><span data-stu-id="6443e-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="6443e-231">Eine primäre Einschränkung kann ein Klassentyp oder eine ***Verweistyp Einschränkung*** `class` oder die ***Werttyp Einschränkung*** `struct`sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="6443e-232">Eine sekundäre Einschränkung kann ein *type_parameter* oder *INTERFACE_TYPE*sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="6443e-233">Die Verweistyp Einschränkung gibt an, dass ein für den Typparameter verwendetes Typargument ein Verweistyp sein muss.</span><span class="sxs-lookup"><span data-stu-id="6443e-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="6443e-234">Alle Klassentypen, Schnittstellentypen, Delegattypen, Array Typen und Typparameter, die als Verweistyp bekannt sind (wie unten definiert), erfüllen diese Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="6443e-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="6443e-235">Die Werttyp Einschränkung gibt an, dass das für den Typparameter verwendete Typargument ein Werttyp sein muss, der keine NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="6443e-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="6443e-236">Alle Strukturtypen, Enumerationstypen und Typparameter, die keine NULL-Werte zulassen und die Werttyp Einschränkung aufweisen, erfüllen diese Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="6443e-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="6443e-237">Beachten Sie, dass ein Typ, der NULL-Werte zulässt (Typen, die[null](types.md#nullable-types)-Werte zulassen), nicht der Werttyp Einschränkung entspricht.</span><span class="sxs-lookup"><span data-stu-id="6443e-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="6443e-238">Ein Typparameter mit der Werttyp Einschränkung kann nicht auch über *constructor_constraint*verfügen.</span><span class="sxs-lookup"><span data-stu-id="6443e-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="6443e-239">Zeiger Typen dürfen nicht als Typargumente eingestuft werden und werden nicht berücksichtigt, um entweder den Verweistyp oder die Werttyp Einschränkungen zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="6443e-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="6443e-240">Wenn es sich bei einer Einschränkung um einen Klassentyp, einen Schnittstellentyp oder einen Typparameter handelt, gibt dieser Typ einen minimalen "Basistyp" an, den jedes Typargument für diesen Typparameter unterstützen muss.</span><span class="sxs-lookup"><span data-stu-id="6443e-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="6443e-241">Wenn ein konstruierter Typ oder eine generische Methode verwendet wird, wird das Typargument zur Kompilierzeit mit den Einschränkungen für den Typparameter verglichen.</span><span class="sxs-lookup"><span data-stu-id="6443e-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="6443e-242">Das angegebene Typargument muss die unter " [erfüllen von Einschränkungen](types.md#satisfying-constraints)" beschriebenen Bedingungen erfüllen.</span><span class="sxs-lookup"><span data-stu-id="6443e-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="6443e-243">Eine *class_type* -Einschränkung muss die folgenden Regeln erfüllen:</span><span class="sxs-lookup"><span data-stu-id="6443e-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="6443e-244">Der Typ muss ein Klassentyp sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-244">The type must be a class type.</span></span>
*  <span data-ttu-id="6443e-245">Der Typ darf nicht sein `sealed`.</span><span class="sxs-lookup"><span data-stu-id="6443e-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="6443e-246">Der Typ darf keinem der folgenden Typen sein `System.Array`:, `System.Delegate`, `System.Enum`oder `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="6443e-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="6443e-247">Der Typ darf nicht sein `object`.</span><span class="sxs-lookup"><span data-stu-id="6443e-247">The type must not be `object`.</span></span> <span data-ttu-id="6443e-248">Da alle Typen von `object`abgeleitet sind, hätte eine solche Einschränkung keine Auswirkung, wenn Sie zulässig wäre.</span><span class="sxs-lookup"><span data-stu-id="6443e-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="6443e-249">Höchstens eine Einschränkung für einen angegebenen Typparameter kann ein Klassentyp sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="6443e-250">Ein Typ, der als *INTERFACE_TYPE* -Einschränkung angegeben ist, muss die folgenden Regeln erfüllen:</span><span class="sxs-lookup"><span data-stu-id="6443e-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="6443e-251">Der Typ muss ein Schnittstellentyp sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="6443e-252">Ein Typ darf in einer gegebenen `where` Klausel nicht mehrmals angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="6443e-253">In beiden Fällen kann die Einschränkung einen der Typparameter der zugeordneten Typ-oder Methoden Deklaration als Teil eines konstruierten Typs einschließen und den Typ einschließen, der deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="6443e-254">Jeder Klassen-oder Schnittstellentyp, der als Typparameter Einschränkung angegeben ist, muss mindestens so zugänglich sein (Barrierefreiheits[Einschränkungen](basic-concepts.md#accessibility-constraints)) wie der generische Typ oder die Methode, der deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="6443e-255">Ein Typ, der als *type_parameter* -Einschränkung angegeben ist, muss die folgenden Regeln erfüllen:</span><span class="sxs-lookup"><span data-stu-id="6443e-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="6443e-256">Der Typ muss ein Typparameter sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="6443e-257">Ein Typ darf in einer gegebenen `where` Klausel nicht mehrmals angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="6443e-258">Außerdem dürfen im Abhängigkeits Diagramm der Typparameter keine Zyklen vorhanden sein, bei denen die Abhängigkeit eine transitiv Beziehung ist, die durch definiert wird:</span><span class="sxs-lookup"><span data-stu-id="6443e-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="6443e-259">Wenn ein Typparameter `T` als Einschränkung für den `S` Typparameter `S` verwendet wird, ***hängt von ab*** `T`.</span><span class="sxs-lookup"><span data-stu-id="6443e-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="6443e-260">Wenn ein Typparameter `S` von einem Typparameter `T` abhängt und `T` von einem Typparameter `U` `U` `S` abhängt, hängt von ***ab*** .</span><span class="sxs-lookup"><span data-stu-id="6443e-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="6443e-261">Bei dieser Beziehung handelt es sich um einen Kompilierzeitfehler für einen Typparameter, der direkt oder indirekt von sich selbst abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="6443e-262">Alle Einschränkungen müssen zwischen abhängigen Typparametern einheitlich sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="6443e-263">Wenn der Typparameter `S` vom Typparameter `T` abhängt, dann:</span><span class="sxs-lookup"><span data-stu-id="6443e-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="6443e-264">`T`darf nicht über die Werttyp Einschränkung verfügen.</span><span class="sxs-lookup"><span data-stu-id="6443e-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="6443e-265">Andernfalls ist tatsächlich versiegelt, sodass `S` gezwungen wird, denselben Typ wie `T`zu haben, sodass zwei Typparameter nicht mehr benötigt werden. `T`</span><span class="sxs-lookup"><span data-stu-id="6443e-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="6443e-266">Wenn `S` die Werttyp Einschränkung aufweist, darf `T` keine *class_type* -Einschränkung aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="6443e-267">Wenn `S` über eine *class_type* -Einschränkung verfügt `A` und `T` eine *class_type* -Einschränkung `B`, muss eine Identitäts Konvertierung oder eine implizite Verweis Konvertierung von `A` in `B` oder eine implizite Verweis Konvertierung von `B` auf `A`.</span><span class="sxs-lookup"><span data-stu-id="6443e-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="6443e-268">Wenn `S` auch vom Typparameter `U` und `U` über eine *class_type* -Einschränkung verfügt `A` und `T` eine *class_type* -Einschränkung `B`, muss eine Identitäts Konvertierung oder eine implizite Verweis Konvertierung von `A` erfolgen. zum `B` oder eine implizite Verweis Konvertierung von 0 in 1.</span><span class="sxs-lookup"><span data-stu-id="6443e-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="6443e-269">Es ist zulässig, `S` dass die Werttyp Einschränkung und `T` die Verweistyp Einschränkung aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="6443e-270">Dies schränkt `T` praktisch die Typen `System.Object`, `System.ValueType`, `System.Enum`und alle Schnittstellentypen ein.</span><span class="sxs-lookup"><span data-stu-id="6443e-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="6443e-271">Wenn die `where` -Klausel für einen Typparameter eine Konstruktoreinschränkung (die `new()`das-Format aufweist) enthält, kann der `new` -Operator verwendet werden, um Instanzen des-Typs ([Objekt Erstellungs Ausdrücke](expressions.md#object-creation-expressions)) zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6443e-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="6443e-272">Alle Typargumente, die für einen Typparameter mit einer Konstruktoreinschränkung verwendet werden, müssen über einen öffentlichen Parameter losen Konstruktor verfügen (dieser Konstruktor ist implizit für jeden Werttyp vorhanden), oder es handelt sich um einen Typparameter mit der Werttyp Einschränkung oder Konstruktoreinschränkung (siehe [Typparameter Einschränkungen](classes.md#type-parameter-constraints) für Details).</span><span class="sxs-lookup"><span data-stu-id="6443e-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="6443e-273">Im folgenden finden Sie Beispiele für Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="6443e-273">The following are examples of constraints:</span></span>
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

<span data-ttu-id="6443e-274">Das folgende Beispiel ist fehlerhaft, da es eine Zirkularität im Abhängigkeits Diagramm der Typparameter verursacht:</span><span class="sxs-lookup"><span data-stu-id="6443e-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="6443e-275">In den folgenden Beispielen werden zusätzliche ungültige Situationen veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="6443e-275">The following examples illustrate additional invalid situations:</span></span>
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

<span data-ttu-id="6443e-276">Die ***effektive Basisklasse*** eines Typparameters `T` wird wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="6443e-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="6443e-277">Wenn `T` keine Primary-Einschränkungen oder Typparameter Einschränkungen aufweist, ist `object`die effektive Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="6443e-278">Wenn `T` die Werttyp Einschränkung aufweist, ist `System.ValueType`die effektive Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="6443e-279">Wenn `T` über eine *class_type* -Einschränkung `C`, aber keine *type_parameter* -Einschränkungen verfügt, ist die effektive Basisklasse `C`.</span><span class="sxs-lookup"><span data-stu-id="6443e-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="6443e-280">Wenn `T` keine *class_type* -Einschränkung hat, aber mindestens eine *type_parameter* -Einschränkung aufweist, handelt es sich bei der effektiven Basisklasse um den am häufigsten als Typ ([gesteigerte Konvertierungs Operator](conversions.md#lifted-conversion-operators)) im Satz effektiver Basisklassen der *Type_ Parameter* Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="6443e-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="6443e-281">Durch die Konsistenzregeln wird sichergestellt, dass ein solcher Typ vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="6443e-282">Wenn `T` sowohl eine *class_type* -Einschränkung als auch eine oder mehrere *type_parameter* -Einschränkungen aufweist, ist die effektive Basisklasse der am meisten eingeschlossenen Typ ([gesteigerte Konvertierungs Operatoren](conversions.md#lifted-conversion-operators)) in der Menge, die aus dem *class_type* besteht. Einschränkung von `T` und den effektiven Basisklassen der *type_parameter* -Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="6443e-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="6443e-283">Durch die Konsistenzregeln wird sichergestellt, dass ein solcher Typ vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="6443e-284">Wenn `T` die Verweistyp Einschränkung, aber keine *class_type* -Einschränkungen aufweist, ist die effektive Basisklasse `object`.</span><span class="sxs-lookup"><span data-stu-id="6443e-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="6443e-285">Verwenden Sie für diese Regeln stattdessen den spezifischsten Basistyp von `V`, bei dem es sich um eine Einschränkung `V` handelt, bei der es sich um eine *value_type* *handelt.*</span><span class="sxs-lookup"><span data-stu-id="6443e-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="6443e-286">Dies kann in einer explizit angegebenen Einschränkung nie vorkommen, kann jedoch auftreten, wenn die Einschränkungen einer generischen Methode implizit von einer über schreibenden Methoden Deklaration oder einer expliziten Implementierung einer Schnittstellen Methode geerbt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="6443e-287">Diese Regeln stellen sicher, dass die effektive Basisklasse immer ein *class_type*ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="6443e-288">Der ***effektive Schnittstellen Satz*** eines Typparameters `T` wird wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="6443e-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="6443e-289">Wenn `T` keinen *secondary_constraints*hat, ist der effektive Schnittstellen Satz leer.</span><span class="sxs-lookup"><span data-stu-id="6443e-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="6443e-290">Wenn `T` *INTERFACE_TYPE* -Einschränkungen, aber keine *type_parameter* -Einschränkungen aufweist, ist der effektive Schnittstellen Satz der Satz von *INTERFACE_TYPE* -Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="6443e-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="6443e-291">Wenn `T` keine *INTERFACE_TYPE* -Einschränkungen aufweist, aber über *type_parameter* -Einschränkungen verfügt, ist der effektive Schnittstellen Satz die Vereinigung der effektiven Schnittstellen Sätze der *type_parameter* -Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="6443e-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="6443e-292">Wenn `T` sowohl *INTERFACE_TYPE* -Einschränkungen als auch *type_parameter* -Einschränkungen aufweist, ist der effektive Schnittstellen Satz die Kombination aus dem Satz von *INTERFACE_TYPE* -Einschränkungen und den effektiven Schnittstellen Sätzen seiner *type_parameter* Auflagen.</span><span class="sxs-lookup"><span data-stu-id="6443e-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="6443e-293">Ein Typparameter ist ***bekanntermaßen ein Referenztyp,*** wenn er über die Verweistyp Einschränkung oder seine effektive Basisklasse nicht `object` oder `System.ValueType`ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="6443e-294">Werte eines eingeschränkten Typparameter Typs können für den Zugriff auf die Instanzmember verwendet werden, die durch die Einschränkungen impliziert sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="6443e-295">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-295">In the example</span></span>
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
<span data-ttu-id="6443e-296">die-Methoden `IPrintable` von können direkt auf `x` aufgerufen werden `T` , da eingeschränkt ist, `IPrintable`um immer zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="6443e-297">Klassen Text</span><span class="sxs-lookup"><span data-stu-id="6443e-297">Class body</span></span>

<span data-ttu-id="6443e-298">Der *class_body* einer Klasse definiert die Member dieser Klasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="6443e-299">Partial types (Partielle Typen)</span><span class="sxs-lookup"><span data-stu-id="6443e-299">Partial types</span></span>

<span data-ttu-id="6443e-300">Eine Typdeklaration kann über mehrere ***partielle Typdeklarationen***hinweg aufgeteilt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="6443e-301">Die Typdeklaration wird anhand der in diesem Abschnitt aufgeführten Regeln erstellt, woraufhin Sie im Rest der Kompilierzeit-und Lauf Zeit Verarbeitung des Programms als eine einzige Deklaration behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="6443e-302">Eine *class_declaration*, *struct_declaration* oder *interface_declaration* stellt eine partielle Typdeklaration dar, wenn Sie einen `partial`-Modifizierer enthält.</span><span class="sxs-lookup"><span data-stu-id="6443e-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="6443e-303">`partial`ist kein Schlüsselwort und fungiert nur als Modifizierer, wenn er unmittelbar vor `class`einem der Schlüsselwörter `struct` oder `interface` in einer Typdeklaration oder vor dem Typ `void` in einer Methoden Deklaration angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="6443e-304">In anderen Kontexten kann es als normaler Bezeichner verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="6443e-305">Jeder Teil einer partiellen Typdeklaration muss einen `partial` Modifizierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="6443e-306">Er muss denselben Namen haben und in derselben Namespace-oder Typdeklaration wie die anderen Teile deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="6443e-307">Der `partial` -Modifizierer gibt an, dass zusätzliche Teile der Typdeklaration an anderer Stelle vorhanden sein können, aber das vorhanden sein solcher zusätzlicher Teile ist nicht erforderlich. es ist für einen Typ mit einer `partial` einzelnen Deklaration gültig, den Modifizierer einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="6443e-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="6443e-308">Alle Teile eines partiellen Typs müssen zusammen kompiliert werden, sodass die Teile zur Kompilierzeit in eine einzelne Typdeklaration zusammengeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="6443e-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="6443e-309">Bei partiellen Typen können nicht bereits kompilierte Typen erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="6443e-310">Mit dem-Modifizierer können mit dem `partial` -Modifizierer in mehreren Teilen deklarierte Typen deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="6443e-311">In der Regel wird der enthaltende Typ `partial` auch mit deklariert, und jeder Teil des untergeordneten Typs wird in einem anderen Teil des enthaltenden Typs deklariert.</span><span class="sxs-lookup"><span data-stu-id="6443e-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="6443e-312">Der `partial` -Modifizierer ist in Delegaten-oder Enumerationsdeklarationen unzulässig.</span><span class="sxs-lookup"><span data-stu-id="6443e-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="6443e-313">Attribute</span><span class="sxs-lookup"><span data-stu-id="6443e-313">Attributes</span></span>

<span data-ttu-id="6443e-314">Die Attribute eines partiellen Typs werden festgelegt, indem die Attribute der einzelnen Teile in einer nicht angegebenen Reihenfolge kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="6443e-315">Wenn ein Attribut in mehreren Teilen platziert wird, entspricht es dem mehrfachen angeben des Attributs für den Typ.</span><span class="sxs-lookup"><span data-stu-id="6443e-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="6443e-316">Die beiden Teile sind z. b.:</span><span class="sxs-lookup"><span data-stu-id="6443e-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="6443e-317">Äquivalent zu einer Deklaration, z. b.:</span><span class="sxs-lookup"><span data-stu-id="6443e-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="6443e-318">Attribute für Typparameter werden auf ähnliche Weise kombiniert.</span><span class="sxs-lookup"><span data-stu-id="6443e-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="6443e-319">Modifizierer</span><span class="sxs-lookup"><span data-stu-id="6443e-319">Modifiers</span></span>

<span data-ttu-id="6443e-320">Wenn eine partielle Typdeklaration eine Barrierefreiheits Spezifikation `public`( `protected`die `internal`Modifizierer,, und `private` ) enthält, muss Sie mit allen anderen Teilen übereinstimmen, die eine Barrierefreiheits Spezifikation enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="6443e-321">Wenn kein Teil eines partiellen Typs eine Barrierefreiheits Spezifikation enthält, erhält der Typ die entsprechende Standard Barrierefreiheit (als[Barrierefreiheit deklariert](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="6443e-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="6443e-322">Wenn eine oder mehrere partielle Deklarationen eines in einem Typ geänderten Typs einen `new` Modifizierer enthalten, wird keine Warnung ausgegeben, wenn der Typ eines geerbten Members ([durch Vererbung](basic-concepts.md#hiding-through-inheritance)ausblenden) ausgeblendet wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="6443e-323">Wenn eine oder mehrere partielle Deklarationen einer Klasse einen `abstract` Modifizierer enthalten, gilt die Klasse als abstrakt ([abstrakte Klassen](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="6443e-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="6443e-324">Andernfalls gilt die Klasse als nicht abstrakt.</span><span class="sxs-lookup"><span data-stu-id="6443e-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="6443e-325">Wenn mindestens eine partielle Deklaration einer Klasse einen `sealed` Modifizierer enthält, gilt die Klasse als versiegelt ([versiegelte Klassen](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="6443e-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="6443e-326">Andernfalls wird die Klasse als nicht versiegelt angesehen.</span><span class="sxs-lookup"><span data-stu-id="6443e-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="6443e-327">Beachten Sie, dass eine Klasse nicht gleichzeitig abstrakt und versiegelt sein kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="6443e-328">Wenn der `unsafe` -Modifizierer für eine partielle Typdeklaration verwendet wird, wird nur dieser bestimmte Teil als unsicherer Kontext ([unsichere Kontexte](unsafe-code.md#unsafe-contexts)) betrachtet.</span><span class="sxs-lookup"><span data-stu-id="6443e-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="6443e-329">Typparameter und Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="6443e-329">Type parameters and constraints</span></span>

<span data-ttu-id="6443e-330">Wenn ein generischer Typ in mehreren Teilen deklariert ist, muss jeder Teil die Typparameter angeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="6443e-331">Jeder Teil muss die gleiche Anzahl von Typparametern und den gleichen Namen für jeden Typparameter in der richtigen Reihenfolge aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="6443e-332">Wenn eine partielle generische Typdeklaration Einschränkungen`where` (Klauseln) enthält, müssen die Einschränkungen allen anderen Teilen, die Einschränkungen einschließen, zustimmen.</span><span class="sxs-lookup"><span data-stu-id="6443e-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="6443e-333">Insbesondere müssen alle Teile, die Einschränkungen enthalten, Einschränkungen für denselben Satz von Typparametern aufweisen, und für jeden Typparameter müssen die Sätze der primären, sekundären und Konstruktoreinschränkungen gleichwertig sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="6443e-334">Zwei Sätze von Einschränkungen sind äquivalent, wenn Sie dieselben Member enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="6443e-335">Wenn kein Teil eines partiellen generischen Typs Typparameter Einschränkungen angibt, gelten die Typparameter als nicht eingeschränkt.</span><span class="sxs-lookup"><span data-stu-id="6443e-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="6443e-336">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-336">The example</span></span>
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
<span data-ttu-id="6443e-337">ist richtig, da diese Teile, die Einschränkungen enthalten (die ersten beiden), effektiv denselben Satz von primären, sekundären und Konstruktoreinschränkungen für denselben Satz von Typparametern angeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="6443e-338">Basisklasse</span><span class="sxs-lookup"><span data-stu-id="6443e-338">Base class</span></span>

<span data-ttu-id="6443e-339">Wenn eine partielle Klassen Deklaration eine Basisklassen Spezifikation enthält, muss Sie mit allen anderen Teilen übereinstimmen, die eine Basisklassen Spezifikation enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="6443e-340">Wenn kein Teil einer partiellen Klasse eine Basisklassen Spezifikation enthält, wird die Basisklasse `System.Object` zu ([Basisklassen](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="6443e-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="6443e-341">Basis Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="6443e-341">Base interfaces</span></span>

<span data-ttu-id="6443e-342">Der Satz von Basis Schnittstellen für einen in mehreren Teilen deklarierten Typ ist die Vereinigung der Basis Schnittstellen, die für jeden Teil angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="6443e-343">Eine bestimmte Basisschnittstelle kann nur einmal pro Teil benannt werden, es ist jedoch zulässig, dass mehrere Teile die gleichen Basis Schnittstellen benennen.</span><span class="sxs-lookup"><span data-stu-id="6443e-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="6443e-344">Es darf nur eine Implementierung der Member einer bestimmten Basisschnittstelle vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="6443e-345">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="6443e-346">der Satz von Basis Schnittstellen für die `C` - `IA`Klasse `IB`ist, `IC`und.</span><span class="sxs-lookup"><span data-stu-id="6443e-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="6443e-347">In der Regel stellt jeder Teil eine Implementierung der Schnittstellen bereit, die für diesen Teil deklariert werden. Dies ist jedoch keine Voraussetzung.</span><span class="sxs-lookup"><span data-stu-id="6443e-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="6443e-348">Ein Teil kann die Implementierung für eine Schnittstelle bereitstellen, die für einen anderen Teil deklariert wurde:</span><span class="sxs-lookup"><span data-stu-id="6443e-348">A part may provide the implementation for an interface declared on a different part:</span></span>
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

### <a name="members"></a><span data-ttu-id="6443e-349">Member</span><span class="sxs-lookup"><span data-stu-id="6443e-349">Members</span></span>

<span data-ttu-id="6443e-350">Mit Ausnahme von partiellen Methoden ([partielle Methoden](classes.md#partial-methods)) ist der Satz von Membern eines Typs, der in mehreren Teilen deklariert wurde, einfach die Vereinigung der Menge von Membern, die in jedem Teil deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="6443e-351">Die Texte aller Teile der Typdeklaration verwenden denselben Deklarations Bereich ([Deklarationen](basic-concepts.md#declarations)), und der[Gültigkeits](basic-concepts.md#scopes)Bereich der einzelnen Member (Bereiche) erstreckt sich auf den Text aller Teile.</span><span class="sxs-lookup"><span data-stu-id="6443e-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="6443e-352">Die Zugriffs Domäne eines beliebigen Members enthält immer alle Teile des einschließenden Typs. ein `private` Member, der in einem Teil deklariert ist, kann aus einem anderen Teil frei zugänglich sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="6443e-353">Es handelt sich um einen Kompilierzeitfehler, um denselben Member in mehr als einem Teil des Typs zu deklarieren, es sei denn, dieser `partial` Member ist ein Typ mit dem-Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6443e-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

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

<span data-ttu-id="6443e-354">Die Reihenfolge von Membern innerhalb eines Typs ist nur C# selten wichtig für Code, kann jedoch bei der Schnittstelle mit anderen Sprachen und Umgebungen von Bedeutung sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="6443e-355">In diesen Fällen ist die Reihenfolge von Membern innerhalb eines in mehreren Teilen deklarierten Typs nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="6443e-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="6443e-356">Partielle Methoden</span><span class="sxs-lookup"><span data-stu-id="6443e-356">Partial methods</span></span>

<span data-ttu-id="6443e-357">Partielle Methoden können in einem Teil einer Typdeklaration definiert und in einem anderen implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="6443e-358">Die Implementierung ist optional. Wenn kein Teil die partielle Methode implementiert, werden die Deklaration der partiellen Methode und alle Aufrufe an die Deklaration aus der Typdeklaration entfernt, die sich aus der Kombination der Teile ergibt.</span><span class="sxs-lookup"><span data-stu-id="6443e-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="6443e-359">Partielle Methoden können keine Zugriffsmodifizierer definieren, sondern implizit `private`.</span><span class="sxs-lookup"><span data-stu-id="6443e-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="6443e-360">Der Rückgabetyp `void`muss sein, und ihre Parameter dürfen `out` nicht den-Modifizierer aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="6443e-361">Der Bezeichner `partial` wird in einer Methoden Deklaration nur dann als sonderschlüsselwort erkannt, wenn `void` er direkt vor dem Typ angezeigt wird. andernfalls kann er als normaler Bezeichner verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="6443e-362">Eine partielle Methode kann Schnittstellen Methoden nicht explizit implementieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="6443e-363">Es gibt zwei Arten von partiellen Methoden Deklarationen: Wenn der Text der Methoden Deklaration ein Semikolon ist, wird die Deklaration als eine ***definierende partielle Methoden Deklaration***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="6443e-364">Wenn der Text als- *Block*angegeben wird, wird die Deklaration als eine ***implementierende partielle Methoden Deklaration***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="6443e-365">In den Teilen einer Typdeklaration darf nur eine partielle Methoden Deklaration mit einer bestimmten Signatur definiert werden, und es kann nur eine partielle Methoden Deklaration mit einer bestimmten Signatur implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="6443e-366">Wenn eine implementierende partielle Methoden Deklaration angegeben wird, muss eine entsprechende definierende partielle Methoden Deklaration vorhanden sein, und die Deklarationen müssen übereinstimmen, wie im folgenden angegeben:</span><span class="sxs-lookup"><span data-stu-id="6443e-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="6443e-367">Die Deklarationen müssen die gleichen modifiziererer (auch nicht unbedingt in derselben Reihenfolge), den Methodennamen, die Anzahl der Typparameter und die Anzahl von Parametern aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="6443e-368">Die entsprechenden Parameter in den Deklarationen müssen dieselben Modifizierer aufweisen (obwohl Sie nicht notwendigerweise in derselben Reihenfolge sind) und dieselben Typen (Modulo-Unterschiede in Typparameter Namen).</span><span class="sxs-lookup"><span data-stu-id="6443e-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="6443e-369">Die entsprechenden Typparameter in den Deklarationen müssen dieselben Einschränkungen aufweisen (Modulo-Unterschiede in Typparameter Namen).</span><span class="sxs-lookup"><span data-stu-id="6443e-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="6443e-370">Eine implementierende partielle Methoden Deklaration kann im gleichen Teil wie die entsprechende definierende partielle Methoden Deklaration vorkommen.</span><span class="sxs-lookup"><span data-stu-id="6443e-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="6443e-371">Nur eine definierende partielle Methode ist an der Überladungs Auflösung beteiligt.</span><span class="sxs-lookup"><span data-stu-id="6443e-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="6443e-372">Unabhängig davon, ob eine implementierende Deklaration angegeben wird, können Aufruf Ausdrücke in Aufrufe der partiellen Methode aufgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="6443e-373">Da eine partielle Methode immer `void`zurückgibt, sind solche Aufruf Ausdrücke immer Ausdrucks Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="6443e-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="6443e-374">Da eine partielle Methode implizit `private`ist, treten diese Anweisungen immer innerhalb eines der Teile der Typdeklaration auf, in der die partielle Methode deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="6443e-375">Wenn kein Teil einer partiellen Typdeklaration eine implementierende Deklaration für eine bestimmte partielle Methode enthält, wird jede Ausdrucks Anweisung, die Sie aufruft, einfach aus der kombinierten Typdeklaration entfernt.</span><span class="sxs-lookup"><span data-stu-id="6443e-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="6443e-376">Folglich hat der Aufruf Ausdruck, einschließlich aller konstituierender Ausdrücke, keine Auswirkung zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="6443e-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="6443e-377">Die partielle Methode selbst wird ebenfalls entfernt und ist kein Member der kombinierten Typdeklaration.</span><span class="sxs-lookup"><span data-stu-id="6443e-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="6443e-378">Wenn eine implementierende Deklaration für eine bestimmte partielle Methode vorhanden ist, werden die Aufrufe der partiellen Methoden beibehalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="6443e-379">Die partielle Methode führt zu einer Methoden Deklaration, die der Implementierung der partiellen Methoden Deklaration ähnelt, mit Ausnahme folgender:</span><span class="sxs-lookup"><span data-stu-id="6443e-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="6443e-380">Der `partial` -Modifizierer ist nicht eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="6443e-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="6443e-381">Die Attribute in der resultierenden Methoden Deklaration sind die kombinierten Attribute der definierenden und der implementierenden partiellen Methoden Deklaration in einer nicht angegebenen Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="6443e-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="6443e-382">Duplikate werden nicht entfernt.</span><span class="sxs-lookup"><span data-stu-id="6443e-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="6443e-383">Die Attribute für die Parameter der resultierenden Methoden Deklaration sind die kombinierten Attribute der entsprechenden Parameter der definierenden und der implementierenden partiellen Methoden Deklaration in einer nicht angegebenen Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="6443e-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="6443e-384">Duplikate werden nicht entfernt.</span><span class="sxs-lookup"><span data-stu-id="6443e-384">Duplicates are not removed.</span></span>

<span data-ttu-id="6443e-385">Wenn eine definierende Deklaration, aber keine implementierende Deklaration für eine partielle Methode M angegeben wird, gelten die folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="6443e-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="6443e-386">Es handelt sich um einen Kompilierzeitfehler zum Erstellen eines Delegaten für die Methode ([delegaterstellungs-Ausdrücke](expressions.md#delegate-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="6443e-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="6443e-387">Es handelt sich um einen Kompilierzeitfehler, `M` auf den in einer anonymen Funktion verwiesen wird, die in einen Ausdrucks Strukturtyp konvertiert wird ([Auswertung anonymer Funktions Konvertierungen in Ausdrucks Baumstruktur Typen](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="6443e-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="6443e-388">Ausdrücke, die als Teil des aufzurufenden von auftreten, wirken sich nicht auf den eindeutigen Zuweisungs Zustand ( `M` [definitive Zuweisung](variables.md#definite-assignment)) aus, der potenziell zu Kompilier Zeitfehlern führen kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="6443e-389">`M`kann nicht der Einstiegspunkt für eine Anwendung ([Anwendungsstart](basic-concepts.md#application-startup)) sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="6443e-390">Partielle Methoden sind hilfreich, um einem Teil einer Typdeklaration das Anpassen des Verhaltens eines anderen Teils zu ermöglichen, z. b. eines, das von einem Tool generiert wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="6443e-391">Beachten Sie die folgende Deklaration der partiellen Klasse:</span><span class="sxs-lookup"><span data-stu-id="6443e-391">Consider the following partial class declaration:</span></span>
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

<span data-ttu-id="6443e-392">Wenn diese Klasse ohne andere Teile kompiliert wird, werden die definierenden partiellen Methoden Deklarationen und deren Aufrufe entfernt, und die resultierende kombinierte Klassen Deklaration entspricht Folgendem:</span><span class="sxs-lookup"><span data-stu-id="6443e-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
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

<span data-ttu-id="6443e-393">Angenommen, es wird jedoch ein anderer Teil angegeben, der die Implementierung von Deklarationen der partiellen Methoden bereitstellt:</span><span class="sxs-lookup"><span data-stu-id="6443e-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
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

<span data-ttu-id="6443e-394">Dann entspricht die resultierende kombinierte Klassen Deklaration folgendem:</span><span class="sxs-lookup"><span data-stu-id="6443e-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
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

### <a name="name-binding"></a><span data-ttu-id="6443e-395">Namens Bindung</span><span class="sxs-lookup"><span data-stu-id="6443e-395">Name binding</span></span>

<span data-ttu-id="6443e-396">Obwohl jeder Teil eines erweiterbaren Typs innerhalb desselben Namespace deklariert werden muss, werden die Teile in der Regel in verschiedenen Namespace Deklarationen geschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="6443e-397">Daher können für `using` jeden Teil verschiedene Direktiven ([using-Direktiven](namespaces.md#using-directives)) vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="6443e-398">Bei der Interpretation von einfachen Namen ([Typrückschluss](expressions.md#type-inference)) innerhalb eines Teils `using` werden nur die Direktiven der Namespace Deklaration (en) berücksichtigt, die diesen Teil einschließen.</span><span class="sxs-lookup"><span data-stu-id="6443e-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="6443e-399">Dies kann dazu führen, dass derselbe Bezeichner in unterschiedlichen Teilen mit unterschiedlichen Bedeutungen übereinstimmen:</span><span class="sxs-lookup"><span data-stu-id="6443e-399">This may result in the same identifier having different meanings in different parts:</span></span>
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

## <a name="class-members"></a><span data-ttu-id="6443e-400">Klassenmember</span><span class="sxs-lookup"><span data-stu-id="6443e-400">Class members</span></span>

<span data-ttu-id="6443e-401">Die Member einer Klasse bestehen aus den Membern, die von den *class_member_declaration*s eingeführt wurden, und den Membern, die von der direkten Basisklasse geerbt wurden.</span><span class="sxs-lookup"><span data-stu-id="6443e-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

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

<span data-ttu-id="6443e-402">Die Member eines Klassen Typs sind in die folgenden Kategorien unterteilt:</span><span class="sxs-lookup"><span data-stu-id="6443e-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="6443e-403">Konstanten, die Konstante Werte darstellen, die der-Klasse ([Konstanten](classes.md#constants)) zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="6443e-404">Felder, bei denen es sich um die Variablen der-Klasse ([Felder](classes.md#fields)) handelt.</span><span class="sxs-lookup"><span data-stu-id="6443e-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="6443e-405">-Methoden, die die Berechnungen und Aktionen implementieren, die von der-Klasse ausgeführt werden können (-[Methoden](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="6443e-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="6443e-406">Eigenschaften, die benannte Merkmale und die Aktionen definieren, die mit dem Lesen und Schreiben dieser Eigenschaften verknüpft sind ([Eigenschaften](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="6443e-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="6443e-407">Ereignisse, die Benachrichtigungen definieren, die von der-Klasse ([Ereignissen](classes.md#events)) generiert werden können.</span><span class="sxs-lookup"><span data-stu-id="6443e-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="6443e-408">Indexer, die es ermöglichen, Instanzen der Klasse auf die gleiche Weise (syntaktisch) als Arrays ([Indexer](classes.md#indexers)) zu indizieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="6443e-409">Operatoren, die die Ausdrucks Operatoren definieren, die auf Instanzen der-Klasse ([Operatoren](classes.md#operators)) angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="6443e-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="6443e-410">Instanzkonstruktoren, die die zum Initialisieren von Instanzen der-Klasse erforderlichen Aktionen implementieren ([Instanzkonstruktoren](classes.md#instance-constructors)).</span><span class="sxs-lookup"><span data-stu-id="6443e-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="6443e-411">Dekonstruktoren, die die auszuführenden Aktionen implementieren, bevor Instanzen der Klasse dauerhaft verworfen werden ([Dekonstruktoren](classes.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="6443e-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="6443e-412">Statische Konstruktoren, die die zum Initialisieren der Klasse selbst erforderlichen Aktionen implementieren ([statische Konstruktoren](classes.md#static-constructors)).</span><span class="sxs-lookup"><span data-stu-id="6443e-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="6443e-413">Typen, die die Typen darstellen, die für die-Klasse lokal sind (unter[Typen](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="6443e-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="6443e-414">Member, die ausführbaren Code enthalten können, werden zusammen mit den Funktionsmembern des Klassen Typs bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="6443e-415">Die Funktionsmember eines Klassen Typs sind die Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren, Instanzkonstruktoren, destrukturtoren und statischen Konstruktoren dieses Klassen Typs.</span><span class="sxs-lookup"><span data-stu-id="6443e-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="6443e-416">Ein *class_declaration* erstellt einen neuen Deklarations Raum ([Deklarationen](basic-concepts.md#declarations)), und die *class_member_declaration*-Elemente, die direkt in der *class_declaration* enthalten sind, stellen neue Member in diesen Deklarations Bereich ein.</span><span class="sxs-lookup"><span data-stu-id="6443e-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="6443e-417">Die folgenden Regeln gelten für *class_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6443e-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="6443e-418">Instanzkonstruktoren, Dekonstruktoren und statische Konstruktoren müssen den gleichen Namen wie die unmittelbar einschließende Klasse haben.</span><span class="sxs-lookup"><span data-stu-id="6443e-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="6443e-419">Alle anderen Member müssen Namen haben, die sich von dem Namen der unmittelbar einschließenden Klasse unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="6443e-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="6443e-420">Der Name einer Konstante, eines Felds, einer Eigenschaft, eines Ereignisses oder eines Typs muss sich von den Namen aller anderen Member unterscheiden, die in derselben Klasse deklariert sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="6443e-421">Der Name einer Methode muss sich von den Namen aller anderen nicht-Methoden unterscheiden, die in derselben Klasse deklariert sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="6443e-422">Außerdem müssen sich die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) einer Methode von den Signaturen aller anderen Methoden unterscheiden, die in derselben Klasse deklariert sind, und zwei Methoden, die in derselben Klasse deklariert sind, dürfen keine Signaturen aufweisen, `ref` die sich ausschließlich durch und unterscheiden. `out`.</span><span class="sxs-lookup"><span data-stu-id="6443e-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="6443e-423">Die Signatur eines Instanzkonstruktors muss sich von den Signaturen aller anderen Instanzkonstruktoren unterscheiden, die in derselben Klasse deklariert sind, und zwei in derselben Klasse deklarierte Konstruktoren verfügen möglicherweise nicht `ref` über `out`Signaturen, die sich ausschließlich durch und unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="6443e-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="6443e-424">Die Signatur eines Indexer muss sich von den Signaturen aller anderen Indexer unterscheiden, die in derselben Klasse deklariert sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="6443e-425">Die Signatur eines Operators muss sich von den Signaturen aller anderen Operatoren unterscheiden, die in derselben Klasse deklariert sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="6443e-426">Die geerbten Member eines Klassen Typs ([Vererbung](classes.md#inheritance)) sind nicht Teil des Deklarations Raums einer Klasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="6443e-427">Folglich kann eine abgeleitete Klasse einen Member mit demselben Namen oder derselben Signatur wie ein geerbten Member deklarieren (wodurch der geerbte Member in der Tat ausgeblendet wird).</span><span class="sxs-lookup"><span data-stu-id="6443e-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="6443e-428">Der Instanztyp.</span><span class="sxs-lookup"><span data-stu-id="6443e-428">The instance type</span></span>

<span data-ttu-id="6443e-429">Jede Klassen Deklaration verfügt über einen zugeordneten gebundenen Typ ([gebundene und ungebundene Typen](types.md#bound-and-unbound-types)), den ***Instanztyp***.</span><span class="sxs-lookup"><span data-stu-id="6443e-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="6443e-430">Bei einer generischen Klassen Deklaration wird der Instanztyp durch Erstellen eines konstruierten Typs ([konstruierte Typen](types.md#constructed-types)) aus der Typdeklaration gebildet, wobei jedes der angegebenen Typargumente der entsprechende Typparameter ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="6443e-431">Da der Instanztyp die Typparameter verwendet, kann er nur dort verwendet werden, wo sich die Typparameter im Gültigkeitsbereich befinden. Das heißt, innerhalb der Klassen Deklaration.</span><span class="sxs-lookup"><span data-stu-id="6443e-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="6443e-432">Der Instanztyp ist der Typ `this` von für Code, der innerhalb der Klassen Deklaration geschrieben wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="6443e-433">Bei nicht generischen Klassen ist der Instanztyp einfach die deklarierte Klasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="6443e-434">Das folgende Beispiel zeigt mehrere Klassen Deklarationen zusammen mit ihren Instanztypen:</span><span class="sxs-lookup"><span data-stu-id="6443e-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="6443e-435">Member konstruierter Typen</span><span class="sxs-lookup"><span data-stu-id="6443e-435">Members of constructed types</span></span>

<span data-ttu-id="6443e-436">Die nicht geerbten Member eines konstruierten Typs werden abgerufen, indem für jede *type_parameter* in der Element Deklaration der entsprechende *type_argument* des konstruierten Typs ersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="6443e-437">Der Ersetzungs Vorgang basiert auf der semantischen Bedeutung von Typdeklarationen und ist nicht einfach die Text Ersetzung.</span><span class="sxs-lookup"><span data-stu-id="6443e-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="6443e-438">Beispielsweise bei der Deklaration der generischen Klasse</span><span class="sxs-lookup"><span data-stu-id="6443e-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="6443e-439">der konstruierte Typ `Gen<int[],IComparable<string>>` verfügt über die folgenden Member:</span><span class="sxs-lookup"><span data-stu-id="6443e-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="6443e-440">Der Typ des `a` Members in der generischen Klassen Deklaration `Gen` ist das zweidimensionale Array von `T`, sodass der Typ des `a` Members im konstruierten Typ oben "zweidimensionales Array eines eindimensionalen Arrays aus "ist.`int`", oder `int[,][]`.</span><span class="sxs-lookup"><span data-stu-id="6443e-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="6443e-441">Innerhalb von instanzfunktionsmembern `this` ist der Typ von der Instanztyp ([der Instanztyp](classes.md#the-instance-type)) der enthaltenden Deklaration.</span><span class="sxs-lookup"><span data-stu-id="6443e-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="6443e-442">Alle Member einer generischen Klasse können Typparameter aus einer beliebigen einschließenden Klasse verwenden, entweder direkt oder als Teil eines konstruierten Typs.</span><span class="sxs-lookup"><span data-stu-id="6443e-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="6443e-443">Wenn ein bestimmter, von einem Typ geschlossenes konstruierter Typ ([Open-und Closed-Typen](types.md#open-and-closed-types)) zur Laufzeit verwendet wird, wird jede Verwendung eines Typparameters durch das tatsächliche Typargument ersetzt, das für den konstruierten Typ angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="6443e-444">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6443e-444">For example:</span></span>
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

### <a name="inheritance"></a><span data-ttu-id="6443e-445">Vererbung</span><span class="sxs-lookup"><span data-stu-id="6443e-445">Inheritance</span></span>

<span data-ttu-id="6443e-446">Eine Klasse ***erbt*** die Member ihres direkten Basisklassen Typs.</span><span class="sxs-lookup"><span data-stu-id="6443e-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="6443e-447">Vererbung bedeutet, dass eine Klasse implizit alle Member ihres direkten Basisklassen Typs enthält, mit Ausnahme der Instanzkonstruktoren, Dekonstruktoren und statischer Konstruktoren der Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="6443e-448">Einige wichtige Aspekte der Vererbung sind:</span><span class="sxs-lookup"><span data-stu-id="6443e-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="6443e-449">Vererbung ist transitiv.</span><span class="sxs-lookup"><span data-stu-id="6443e-449">Inheritance is transitive.</span></span> <span data-ttu-id="6443e-450">Wenn `C` von `C` `A` `B` abgeleitet ist und `B` von`A`abgeleitet ist, erbt die in deklarierten Member sowie die in deklarierten Member. `B`</span><span class="sxs-lookup"><span data-stu-id="6443e-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="6443e-451">Eine abgeleitete Klasse erweitert die direkte Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="6443e-452">Eine abgeleitete Klasse kann den geerbten Membern neue Member hinzufügen, aber die Definition eines geerbten Members kann nicht entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="6443e-453">Instanzkonstruktoren, destrukturtoren und statische Konstruktoren werden nicht geerbt, aber alle anderen Member sind, unabhängig von ihrer deklarierten Barrierefreiheit ([Member Access](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="6443e-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="6443e-454">Abhängig von der deklarierten Barrierefreiheit sind vererbte Member jedoch möglicherweise in einer abgeleiteten Klasse nicht zugänglich.</span><span class="sxs-lookup"><span data-stu-id="6443e-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="6443e-455">Eine abgeleitete Klasse kann vererbte Member ausblenden ([durch Vererbung](basic-concepts.md#hiding-through-inheritance) ***Ausblenden*** ), indem neue Member mit demselben Namen oder derselben Signatur deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="6443e-456">Beachten Sie jedoch, dass beim Ausblenden eines geerbten Members dieser Member nicht entfernt wird – er macht diesen Member lediglich direkt über die abgeleitete Klasse zugänglich.</span><span class="sxs-lookup"><span data-stu-id="6443e-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="6443e-457">Eine Instanz einer Klasse enthält einen Satz aller Instanzfelder, die in der-Klasse und den zugehörigen Basisklassen deklariert sind, und eine implizite Konvertierung ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions)) ist von einem abgeleiteten Klassentyp zu einem der zugehörigen Basisklassen Typen vorhanden.</span><span class="sxs-lookup"><span data-stu-id="6443e-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="6443e-458">Folglich kann ein Verweis auf eine Instanz einer abgeleiteten Klasse als Verweis auf eine Instanz der zugehörigen Basisklassen behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="6443e-459">Eine Klasse kann virtuelle Methoden, Eigenschaften und Indexer deklarieren, und abgeleitete Klassen können die Implementierung dieser Funktionsmember überschreiben.</span><span class="sxs-lookup"><span data-stu-id="6443e-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="6443e-460">Dadurch können Klassen polymorphes Verhalten darstellen, wobei die von einem Funktionselement Aufruf ausgeführten Aktionen je nach Lauf Zeittyp der Instanz variieren, durch die der Funktions Member aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="6443e-461">Der geerbte Member eines konstruierten Klassen Typs sind die Member des unmittelbaren Basisklassen Typs ([Basisklassen](classes.md#base-classes)), die gefunden werden, indem die Typargumente des konstruierten Typs für jedes Vorkommen der entsprechenden Typparameter im  *class_base* -Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="6443e-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="6443e-462">Diese Member werden wiederum transformiert, indem Sie für jede *type_parameter* in der Element Deklaration den entsprechenden *type_argument* der *class_base* -Spezifikation ersetzen.</span><span class="sxs-lookup"><span data-stu-id="6443e-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

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

<span data-ttu-id="6443e-463">Im obigen Beispiel hat der konstruierte `D<int>` Typ einen nicht geerbten Member `public int G(string s)` , der durch das Typargument `int` für den Typparameter `T`abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="6443e-464">`D<int>`verfügt auch über einen geerbten Member aus der `B`Klassen Deklaration.</span><span class="sxs-lookup"><span data-stu-id="6443e-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="6443e-465">Dieser geerbte Member wird bestimmt, indem zuerst `B<int[]>` der Basis Klassentyp `D<int>` bestimmt wird `int` , `T` indem in der Basisklassen `B<T[]>`Spezifikation ersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="6443e-466">Anschließend `int[]` wird als Typargument `B`für `U` in `public U F(long index)`durch ersetzt, wodurch der geerbte Member `public int[] F(long index)`bereitstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="6443e-467">Der New-Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6443e-467">The new modifier</span></span>

<span data-ttu-id="6443e-468">Ein *class_member_declaration* -Element darf einen Member mit demselben Namen oder derselben Signatur wie ein geerbte Member deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="6443e-469">In diesem Fall wird der Member der abgeleiteten Klasse zum ***Ausblenden*** des Basisklassenmembers bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="6443e-470">Das Ausblenden eines geerbten Members wird nicht als Fehler betrachtet, bewirkt jedoch, dass der Compiler eine Warnung ausgibt.</span><span class="sxs-lookup"><span data-stu-id="6443e-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="6443e-471">Um die Warnung zu unterdrücken, kann die Deklaration des Members der abgeleiteten Klasse einen `new` -Modifizierer einschließen, um anzugeben, dass der abgeleitete Member den Basismember ausblenden soll.</span><span class="sxs-lookup"><span data-stu-id="6443e-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="6443e-472">Dieses Thema wird unter Ausblenden [durch Vererbung](basic-concepts.md#hiding-through-inheritance)ausführlicher erläutert.</span><span class="sxs-lookup"><span data-stu-id="6443e-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="6443e-473">Wenn ein `new` Modifizierer in einer Deklaration enthalten ist, die einen geerbten Member nicht verbirgt, wird eine Warnung für diesen Effekt ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="6443e-474">Diese Warnung wird durch Entfernen des `new` -Modifizierers unterdrückt.</span><span class="sxs-lookup"><span data-stu-id="6443e-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="6443e-475">Modifizierer für Zugriffe</span><span class="sxs-lookup"><span data-stu-id="6443e-475">Access modifiers</span></span>

<span data-ttu-id="6443e-476">Ein *class_member_declaration* kann eine der fünf möglichen Arten von deklarierten Barrierefreiheits ([deklarierter Barrierefreiheit](basic-concepts.md#declared-accessibility)) haben: `public`, `protected internal`, `protected`, `internal` oder `private`.</span><span class="sxs-lookup"><span data-stu-id="6443e-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="6443e-477">Mit Ausnahme der `protected internal` Kombination ist es ein Kompilierzeitfehler, mehr als einen Zugriffsmodifizierer anzugeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="6443e-478">Wenn ein *class_member_declaration* keine Zugriffsmodifizierer enthält, wird `private` angenommen.</span><span class="sxs-lookup"><span data-stu-id="6443e-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="6443e-479">Konstituierende Typen</span><span class="sxs-lookup"><span data-stu-id="6443e-479">Constituent types</span></span>

<span data-ttu-id="6443e-480">Typen, die in der Deklaration eines Members verwendet werden, werden als konstituierende Typen dieses Members bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="6443e-481">Mögliche Typen sind der Typ einer Konstante, eines Felds, einer Eigenschaft, eines Ereignisses oder eines Indexers, der Rückgabetyp einer Methode oder eines Operators und die Parametertypen einer Methode, eines Indexers, eines Operators oder eines Instanzkonstruktors.</span><span class="sxs-lookup"><span data-stu-id="6443e-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="6443e-482">Die einzelnen Typen eines Members müssen mindestens so zugänglich sein, wie der Member selbst (Barrierefreiheits[Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6443e-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="6443e-483">Statische Member und Instanzmember</span><span class="sxs-lookup"><span data-stu-id="6443e-483">Static and instance members</span></span>

<span data-ttu-id="6443e-484">Member einer Klasse sind entweder ***statische*** Member oder ***Instanzmember***.</span><span class="sxs-lookup"><span data-stu-id="6443e-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="6443e-485">Im Allgemeinen ist es sinnvoll, statische Member als zu den Klassentypen und Instanzmembern gehörend zu betrachten (Instanzen von Klassentypen).</span><span class="sxs-lookup"><span data-stu-id="6443e-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="6443e-486">Wenn ein Feld, eine Methode, eine Eigenschaft, eine Ereignis-, Operator-oder Konstruktordeklaration einen `static` -Modifizierer enthält, wird ein statischer Member deklariert.</span><span class="sxs-lookup"><span data-stu-id="6443e-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="6443e-487">Außerdem deklariert eine Konstante oder Typdeklaration implizit einen statischen Member.</span><span class="sxs-lookup"><span data-stu-id="6443e-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="6443e-488">Statische Member haben die folgenden Merkmale:</span><span class="sxs-lookup"><span data-stu-id="6443e-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="6443e-489">Wenn auf einen statischen Member `M` in einem *member_access* -Element ([Member Access](expressions.md#member-access)) der Form `E.M` verwiesen wird, muss `E` einen Typ mit `M` bezeichnen.</span><span class="sxs-lookup"><span data-stu-id="6443e-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="6443e-490">Es handelt sich um einen Kompilierzeitfehler `E` , mit dem eine Instanz bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="6443e-491">Ein statisches Feld identifiziert genau einen Speicherort, der von allen Instanzen eines gegebenen geschlossenen Klassen Typs freigegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="6443e-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="6443e-492">Unabhängig davon, wie viele Instanzen eines bestimmten geschlossenen Klassen Typs erstellt werden, gibt es nur eine Kopie eines statischen Felds.</span><span class="sxs-lookup"><span data-stu-id="6443e-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="6443e-493">Ein statisches Funktionsmember (Methode, Eigenschaft, Ereignis, Operator oder Konstruktor) funktioniert nicht für eine bestimmte Instanz, und es handelt sich um einen Kompilierzeitfehler, `this` auf den in einem derartigen Funktionsmember verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="6443e-494">Wenn ein Feld, eine Methode, eine Eigenschaft, ein Ereignis, ein Indexer, ein Konstruktor oder eine `static` destrukturerdeklaration keinen Modifizierer enthält, wird ein Instanzmember deklariert.</span><span class="sxs-lookup"><span data-stu-id="6443e-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="6443e-495">(Ein Instanzmember wird manchmal als nicht statischer Member bezeichnet.) Instanzmember haben die folgenden Merkmale:</span><span class="sxs-lookup"><span data-stu-id="6443e-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="6443e-496">Wenn auf einen Instanzmember `M` in einem *member_access* ([Member Access](expressions.md#member-access)) der Form `E.M` verwiesen wird, muss `E` eine Instanz eines Typs mit `M` bezeichnen.</span><span class="sxs-lookup"><span data-stu-id="6443e-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="6443e-497">Es handelt sich um einen Bindungs Zeit Fehler `E` , mit dem ein Typ bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="6443e-498">Jede Instanz einer Klasse enthält einen separaten Satz aller Instanzfelder der Klasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="6443e-499">Ein Instanzfunktionsmember (Methode, Eigenschaft, Indexer, Instanzkonstruktor oder Dekonstruktor) arbeitet auf einer bestimmten Instanz der Klasse, und auf diese Instanz kann `this` als ([dieser Zugriff](expressions.md#this-access)) zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="6443e-500">Das folgende Beispiel veranschaulicht die Regeln für den Zugriff auf statische Member und Instanzmember:</span><span class="sxs-lookup"><span data-stu-id="6443e-500">The following example illustrates the rules for accessing static and instance members:</span></span>
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

<span data-ttu-id="6443e-501">Die `F`-Methode zeigt, dass in einem Instanzfunktionsmember ein *Simple_name* ([simple names](expressions.md#simple-names)) für den Zugriff auf Instanzmember und statische Member verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="6443e-502">Die `G`-Methode zeigt, dass es sich bei einem statischen Funktionsmember um einen Kompilierzeitfehler handelt, der über eine *Simple_name*auf einen Instanzmember zugreifen kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="6443e-503">Die `Main`-Methode zeigt, dass in einem *member_access* ([Member Access](expressions.md#member-access)) auf Instanzmember über-Instanzen zugegriffen werden muss und auf statische Member über-Typen zugegriffen werden muss.</span><span class="sxs-lookup"><span data-stu-id="6443e-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="6443e-504">Geschachtelte Typen</span><span class="sxs-lookup"><span data-stu-id="6443e-504">Nested types</span></span>

<span data-ttu-id="6443e-505">Ein Typ, der innerhalb einer Klassen-oder Struktur Deklaration deklariert wird, wird als geschachtelter ***Typ***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="6443e-506">Ein Typ, der in einer Kompilierungseinheit oder einem Namespace deklariert wird, wird als ***nicht geschachtelter Typ***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="6443e-507">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-507">In the example</span></span>
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
<span data-ttu-id="6443e-508">die `B` Klasse ist ein geschachtelter Typ, da Sie innerhalb `A`der Klasse deklariert `A` wird und die Klasse ein nicht geschachtelter Typ ist, da Sie innerhalb einer Kompilierungseinheit deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="6443e-509">Voll qualifizierter Name</span><span class="sxs-lookup"><span data-stu-id="6443e-509">Fully qualified name</span></span>

<span data-ttu-id="6443e-510">Der voll qualifizierte Name ([voll](basic-concepts.md#fully-qualified-names)gekennzeichnete Namen) für einen Typ ist `S.N` , wobei `S` der voll qualifizierte Name des Typs ist, in dem der `N` Typ deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="6443e-511">Deklarierter Zugriff</span><span class="sxs-lookup"><span data-stu-id="6443e-511">Declared accessibility</span></span>

<span data-ttu-id="6443e-512">Nicht--nicht--- `public` - `internal` ---typtypen können Barrierefreiheit haben oder deklarieren und sind `internal` standardmäßig als</span><span class="sxs-lookup"><span data-stu-id="6443e-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="6443e-513">In Form von untergeordneten Typen können auch diese Formen der deklarierten Barrierefreiheit sowie eine oder mehrere zusätzliche Formen der deklarierten Barrierefreiheit enthalten sein, je nachdem, ob der enthaltende Typ eine Klasse oder Struktur ist:</span><span class="sxs-lookup"><span data-stu-id="6443e-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="6443e-514">Ein in einer Klasse deklarierter`public` `protected`, in einer Klasse deklarierter Typ kann eine beliebige von fünf Formen der deklarierten Barrierefreiheit `internal`aufweisen ( `private`, `protected internal`,, `private` oder), und wie bei anderen Klassenmembern werden standardmäßig deklarierte Barrierefreiheit.</span><span class="sxs-lookup"><span data-stu-id="6443e-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="6443e-515">Ein in einer Struktur deklarierter, in einer Struktur deklarierter Typ kann über eine von drei Formen der`public`deklarierten Barrierefreiheit `private`(, `internal`oder) verfügen, und wie bei anderen `private` Strukturmembern werden standardmäßig deklarierte Zugriffsmöglichkeiten verwendet.</span><span class="sxs-lookup"><span data-stu-id="6443e-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="6443e-516">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-516">The example</span></span>
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
<span data-ttu-id="6443e-517">deklariert eine private, in einer `Node`Klasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="6443e-518">Zieher</span><span class="sxs-lookup"><span data-stu-id="6443e-518">Hiding</span></span>

<span data-ttu-id="6443e-519">Ein-Typ kann einen Basismember ausblenden ([Name](basic-concepts.md#name-hiding)ausblenden).</span><span class="sxs-lookup"><span data-stu-id="6443e-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="6443e-520">Der `new` -Modifizierer ist für Klassentyp Deklarationen zulässig, damit das Ausblenden explizit ausgedrückt werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="6443e-521">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-521">The example</span></span>
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
<span data-ttu-id="6443e-522">zeigt eine in der Liste `M` definierte Klasse, die `M` die in `Base`definierte Methode verbirgt.</span><span class="sxs-lookup"><span data-stu-id="6443e-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="6443e-523">Dieser Zugriff</span><span class="sxs-lookup"><span data-stu-id="6443e-523">this access</span></span>

<span data-ttu-id="6443e-524">Ein-Typ und der enthaltende Typ haben keine besondere Beziehung hinsichtlich *this_access* ([dieser Zugriff](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="6443e-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="6443e-525">`this` Insbesondere innerhalb eines geschachtelten Typs kann nicht verwendet werden, um auf Instanzmember des enthaltenden Typs zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="6443e-526">In Fällen, in denen ein geclusterter Typ Zugriff auf die Instanzmember seines enthaltenden Typs benötigt, kann der `this` Zugriff bereitgestellt werden, indem der für die Instanz des enthaltenden Typs als Konstruktorargument für den schsted Type bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="6443e-527">Im folgenden Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-527">The following example</span></span>
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
<span data-ttu-id="6443e-528">zeigt diese Methode.</span><span class="sxs-lookup"><span data-stu-id="6443e-528">shows this technique.</span></span> <span data-ttu-id="6443e-529">Eine Instanz von `C` erstellt eine Instanz von `Nested` und übergibt ihren eigenen `this` an `Nested`den-Konstruktor, um den nachfolgenden Zugriff `C`auf die Instanzmember bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="6443e-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="6443e-530">Zugriff auf private und geschützte Member des enthaltenden Typs</span><span class="sxs-lookup"><span data-stu-id="6443e-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="6443e-531">Ein Typ, der einen Typ aufweist, kann auf alle Member zugreifen, auf die der enthaltende Typ zugreifen kann, einschließlich der Member des `private` enthaltenden Typs, die über die Berechtigung "Barrierefreiheit" verfügen `protected` .</span><span class="sxs-lookup"><span data-stu-id="6443e-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="6443e-532">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-532">The example</span></span>
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
<span data-ttu-id="6443e-533">zeigt eine Klasse `C` an, die eine `Nested`-Klasse enthält.</span><span class="sxs-lookup"><span data-stu-id="6443e-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="6443e-534">In `Nested`Ruft die- `G` Methode die statische Methode `F` auf, `C`die in `F` definiert ist, und verfügt über eine private deklarierte Barrierefreiheit.</span><span class="sxs-lookup"><span data-stu-id="6443e-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="6443e-535">Ein-Typ kann auch auf geschützte Member zugreifen, die in einem Basistyp seines enthaltenden Typs definiert sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="6443e-536">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-536">In the example</span></span>
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
<span data-ttu-id="6443e-537">die schsted Class `Derived.Nested` greift auf die geschützte `F` Methode zu `Derived`, die in der `Base`Basisklasse von definiert ist, indem `Derived`Sie durch eine Instanz von aufruft.</span><span class="sxs-lookup"><span data-stu-id="6443e-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="6443e-538">Typen in generischen Klassen in generischen Klassen</span><span class="sxs-lookup"><span data-stu-id="6443e-538">Nested types in generic classes</span></span>

<span data-ttu-id="6443e-539">Eine generische Klassen Deklaration kann eine Typdeklaration für einen Typ enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="6443e-540">Die Typparameter der einschließenden Klasse können innerhalb der geschachtelten Typen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="6443e-541">Eine Typdeklaration in einem Typ kann zusätzliche Typparameter enthalten, die nur für den Typ "nsted" gelten.</span><span class="sxs-lookup"><span data-stu-id="6443e-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="6443e-542">Jede in einer generischen Klassen Deklaration enthaltene Typdeklaration ist implizit eine generische Typdeklaration.</span><span class="sxs-lookup"><span data-stu-id="6443e-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="6443e-543">Beim Schreiben eines Verweises auf einen Typ, der in einem generischen Typ geschachtelt ist, muss der enthaltende konstruierte Typ, einschließlich der Typargumente, benannt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="6443e-544">Allerdings kann der geschachtelte Typ innerhalb der äußeren Klasse ohne Qualifizierung verwendet werden. der Instanztyp der äußeren Klasse kann beim Konstruieren des-Typs implizit verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="6443e-545">Im folgenden Beispiel werden drei verschiedene korrekte Möglichkeiten zum Verweisen auf einen konstruierten Typ gezeigt `Inner`, der aus erstellt wurde. die ersten beiden sind äquivalent:</span><span class="sxs-lookup"><span data-stu-id="6443e-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
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

<span data-ttu-id="6443e-546">Obwohl es sich um einen ungültigen Programmierstil handelt, kann ein Typparameter in einem Schraffurtyp einen Member oder Typparameter ausblenden, der im äußeren Typ deklariert ist:</span><span class="sxs-lookup"><span data-stu-id="6443e-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="6443e-547">Reservierte Elementnamen</span><span class="sxs-lookup"><span data-stu-id="6443e-547">Reserved member names</span></span>

<span data-ttu-id="6443e-548">Um die zugrunde liegende C# Lauf Zeit Implementierung zu vereinfachen, muss die Implementierung für jede Deklaration des Quell Members, bei der es sich um eine Eigenschaft, ein Ereignis oder einen Indexer handelt, zwei Methoden Signaturen reservieren, basierend auf der Art der Element Deklaration, dem Namen und dem Typ.</span><span class="sxs-lookup"><span data-stu-id="6443e-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="6443e-549">Es ist ein Kompilierzeitfehler für ein Programm, einen Member zu deklarieren, dessen Signatur mit einer der reservierten Signaturen übereinstimmt, auch wenn die zugrunde liegende Lauf Zeit Implementierung diese Reservierungen nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="6443e-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="6443e-550">Die reservierten Namen führen keine Deklarationen ein, sodass Sie nicht an der Member-Suche beteiligt sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="6443e-551">Die zugeordneten reservierten Methoden Signaturen einer Deklaration nehmen jedoch an der Vererbung ([Vererbung](classes.md#inheritance)) Teil und können mit `new` dem Modifizierer ([dem neuen Modifizierer](classes.md#the-new-modifier)) ausgeblendet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="6443e-552">Die Reservierung dieser Namen dient drei Zwecken:</span><span class="sxs-lookup"><span data-stu-id="6443e-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="6443e-553">, Damit die zugrunde liegende Implementierung einen normalen Bezeichner als Methodennamen zum Abrufen oder Festlegen des Zugriffs auf die C# Sprachfunktion verwendet.</span><span class="sxs-lookup"><span data-stu-id="6443e-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="6443e-554">, Damit andere Sprachen mithilfe eines normalen Bezeichners als Methodenname interagieren können, um den Zugriff auf die C# Sprachfunktion zu erhalten oder festzulegen.</span><span class="sxs-lookup"><span data-stu-id="6443e-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="6443e-555">Um sicherzustellen, dass die von einem konformen Compiler akzeptierte Quelle von einer anderen akzeptiert wird, indem die Besonderheiten von reservierten Elementnamen für C# alle Implementierungen konsistent gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="6443e-556">Die Deklaration eines destrukturtors[(](classes.md#destructors)destrukturatoren) bewirkt auch, dass eine Signatur reserviert wird ([für destrukturtoren reservierte Elementnamen](classes.md#member-names-reserved-for-destructors)).</span><span class="sxs-lookup"><span data-stu-id="6443e-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="6443e-557">Für Eigenschaften reservierte Elementnamen</span><span class="sxs-lookup"><span data-stu-id="6443e-557">Member names reserved for properties</span></span>

<span data-ttu-id="6443e-558">Für eine Eigenschaft `P` ([Eigenschaften](classes.md#properties)) vom Typ `T`sind die folgenden Signaturen reserviert:</span><span class="sxs-lookup"><span data-stu-id="6443e-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="6443e-559">Beide Signaturen sind reserviert, auch wenn die Eigenschaft schreibgeschützt oder schreibgeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="6443e-560">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-560">In the example</span></span>
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
<span data-ttu-id="6443e-561">eine Klasse `A` definiert eine schreibgeschützte Eigenschaft `P`und reserviert somit Signaturen für `get_P` -und- `set_P` Methoden.</span><span class="sxs-lookup"><span data-stu-id="6443e-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="6443e-562">Eine- `B` Klasse wird `A` von abgeleitet und verbirgt beide reservierten Signaturen.</span><span class="sxs-lookup"><span data-stu-id="6443e-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="6443e-563">Das Beispiel erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6443e-563">The example produces the output:</span></span>
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="6443e-564">Für Ereignisse reservierte Elementnamen</span><span class="sxs-lookup"><span data-stu-id="6443e-564">Member names reserved for events</span></span>

<span data-ttu-id="6443e-565">Für ein Ereignis `E` ([Ereignisse](classes.md#events)) des Delegattyps `T`sind die folgenden Signaturen reserviert:</span><span class="sxs-lookup"><span data-stu-id="6443e-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="6443e-566">Für Indexer reservierte Elementnamen</span><span class="sxs-lookup"><span data-stu-id="6443e-566">Member names reserved for indexers</span></span>

<span data-ttu-id="6443e-567">Für einen Indexer ([Indexer](classes.md#indexers)) vom Typ `T` mit Parameter-List `L`sind die folgenden Signaturen reserviert:</span><span class="sxs-lookup"><span data-stu-id="6443e-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="6443e-568">Beide Signaturen sind reserviert, auch wenn der Indexer schreibgeschützt oder schreibgeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="6443e-569">Außerdem ist der Element `Item` Name reserviert.</span><span class="sxs-lookup"><span data-stu-id="6443e-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="6443e-570">Für destrukturtoren reservierte Elementnamen</span><span class="sxs-lookup"><span data-stu-id="6443e-570">Member names reserved for destructors</span></span>

<span data-ttu-id="6443e-571">Für eine Klasse, die einen Dekonstruktor ([Dekonstruktoren](classes.md#destructors)) enthält, ist die folgende Signatur reserviert:</span><span class="sxs-lookup"><span data-stu-id="6443e-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="6443e-572">Konstanten</span><span class="sxs-lookup"><span data-stu-id="6443e-572">Constants</span></span>

<span data-ttu-id="6443e-573">Eine ***Konstante*** ist ein Klassenmember, der einen konstanten Wert darstellt: einen Wert, der zur Kompilierzeit berechnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="6443e-574">Ein *constant_declaration* führt eine oder mehrere Konstanten eines bestimmten Typs ein.</span><span class="sxs-lookup"><span data-stu-id="6443e-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

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

<span data-ttu-id="6443e-575">Ein *constant_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)), einen `new`-Modifizierer ([den neuen Modifizierer](classes.md#the-new-modifier)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)) enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="6443e-576">Die Attribute und Modifizierer gelten für alle Member, die von *constant_declaration*deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="6443e-577">Obwohl Konstanten als statische Member angesehen werden, ist ein *constant_declaration* weder erforderlich noch ein `static`-Modifizierer zulässig.</span><span class="sxs-lookup"><span data-stu-id="6443e-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="6443e-578">Es ist ein Fehler, dass derselbe Modifizierer mehrmals in einer Konstanten Deklaration angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="6443e-579">Der *Typ* eines *constant_declaration* gibt den Typ der Member an, die von der Deklaration eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="6443e-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="6443e-580">Auf den Typ folgt eine Liste von *constant_declarator*s, von denen jeder einen neuen Member einführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="6443e-581">Ein *constant_declarator* besteht aus einem *Bezeichner* , der den Member benennt, gefolgt von einem "`=`"-Token, gefolgt von einem *constant_expression* ([Konstantenausdrücken](expressions.md#constant-expressions)), das den Wert des Members liefert.</span><span class="sxs-lookup"><span data-stu-id="6443e-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="6443e-582">Der in einer Konstanten Deklaration angegebene *Typ* muss `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3, 4, ein *enum_type*oder ein *reference_ Geben*Sie ein.</span><span class="sxs-lookup"><span data-stu-id="6443e-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="6443e-583">Jede *constant_expression* muss einen Wert des Zieltyps oder eines Typs liefern, der durch eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den Zieltyp konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="6443e-584">Der *Typ* einer Konstanten muss mindestens so zugänglich sein, wie die Konstante selbst (Barrierefreiheits[Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6443e-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6443e-585">Der Wert einer Konstanten wird in einem Ausdruck mithilfe eines *Simple_name* ([simple names](expressions.md#simple-names)) oder eines *member_access* ([Member Access](expressions.md#member-access)) abgerufen.</span><span class="sxs-lookup"><span data-stu-id="6443e-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="6443e-586">Eine Konstante kann selbst an einem *constant_expression*teilnehmen.</span><span class="sxs-lookup"><span data-stu-id="6443e-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="6443e-587">Daher kann eine Konstante in jedem Konstrukt verwendet werden, das ein *constant_expression*erfordert.</span><span class="sxs-lookup"><span data-stu-id="6443e-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="6443e-588">Beispiele für derartige Konstrukte `case` sind Bezeichnungen `goto case` , Anweisungen `enum` , Element Deklarationen, Attribute und andere Konstante Deklarationen.</span><span class="sxs-lookup"><span data-stu-id="6443e-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="6443e-589">Wie in [konstanten Ausdrücken](expressions.md#constant-expressions)beschrieben, ist ein *constant_expression* ein Ausdruck, der zur Kompilierzeit vollständig ausgewertet werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="6443e-590">Da die einzige Möglichkeit zum Erstellen eines nicht-NULL-Werts eines anderen *reference_type* als `string` ist, den `new`-Operator anzuwenden, und da der `new`-Operator in einem *constant_expression*-Operator nicht zulässig ist, ist der einzige mögliche Wert für Konstanten von *. reference_type*s (außer `string`) ist `null`.</span><span class="sxs-lookup"><span data-stu-id="6443e-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="6443e-591">Wenn ein symbolischer Name für einen konstanten Wert gewünscht ist, aber wenn der Typ dieses Werts in einer Konstanten Deklaration nicht zulässig ist oder wenn der Wert nicht zur Kompilierzeit von einem *constant_expression*berechnet werden kann, kann ein `readonly`-Feld (schreibgeschützte[Felder](classes.md#readonly-fields)) Verwenden Sie stattdessen.</span><span class="sxs-lookup"><span data-stu-id="6443e-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="6443e-592">Eine Konstante Deklaration, die mehrere Konstanten deklariert, entspricht mehreren Deklarationen von einzelnen Konstanten mit denselben Attributen, modifizierertypen und Typen.</span><span class="sxs-lookup"><span data-stu-id="6443e-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="6443e-593">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6443e-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="6443e-594">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="6443e-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="6443e-595">Konstanten dürfen von anderen Konstanten innerhalb desselben Programms abhängen, solange die Abhängigkeiten nicht aus Zirkel Natur bestehen.</span><span class="sxs-lookup"><span data-stu-id="6443e-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="6443e-596">Der Compiler ordnet automatisch an, die Konstanten Deklarationen in der entsprechenden Reihenfolge auszuwerten.</span><span class="sxs-lookup"><span data-stu-id="6443e-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="6443e-597">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-597">In the example</span></span>
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
<span data-ttu-id="6443e-598">der Compiler `A.Y`wertet zunächst die Werte `B.Z` `A.X` `10`, `11`und ausundwertetSieanschließendaus.`12`</span><span class="sxs-lookup"><span data-stu-id="6443e-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="6443e-599">Konstante Deklarationen können von Konstanten anderer Programme abhängen, aber solche Abhängigkeiten sind nur in einer Richtung möglich.</span><span class="sxs-lookup"><span data-stu-id="6443e-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="6443e-600">`A` Wenn und `B.Z` `A.X` `A.Y` `B.Z` in separaten Programmen in Bezug auf das obige Beispiel deklariert wurden, kann es möglich sein, von abhängig zu sein, aber es kann nicht gleichzeitig von abhängen. `B`</span><span class="sxs-lookup"><span data-stu-id="6443e-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="6443e-601">Felder</span><span class="sxs-lookup"><span data-stu-id="6443e-601">Fields</span></span>

<span data-ttu-id="6443e-602">Ein ***Feld*** ist ein Member, der eine Variable darstellt, die einem Objekt oder einer Klasse zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="6443e-603">Ein *field_declaration* führt ein oder mehrere Felder eines bestimmten Typs ein.</span><span class="sxs-lookup"><span data-stu-id="6443e-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

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

<span data-ttu-id="6443e-604">Ein *field_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)), einen `new`-Modifizierer ([den neuen Modifizierer](classes.md#the-new-modifier)), eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)) und einen `static`-Modifizierer ([ Statische Felder und Instanzfelder](classes.md#static-and-instance-fields)).</span><span class="sxs-lookup"><span data-stu-id="6443e-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="6443e-605">Außerdem kann ein *field_declaration* einen `readonly`-Modifizierer (schreibgeschützte[Felder](classes.md#readonly-fields)) oder einen `volatile`-Modifizierer ([flüchtige Felder](classes.md#volatile-fields)) enthalten, aber nicht beides.</span><span class="sxs-lookup"><span data-stu-id="6443e-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="6443e-606">Die Attribute und Modifizierer gelten für alle Member, die von *field_declaration*deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="6443e-607">Es ist ein Fehler, dass derselbe Modifizierer mehrmals in einer Feld Deklaration angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="6443e-608">Der *Typ* eines *field_declaration* gibt den Typ der Member an, die von der Deklaration eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="6443e-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="6443e-609">Auf den Typ folgt eine Liste von *variable_declarator*s, von denen jeder einen neuen Member einführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="6443e-610">Ein *variable_declarator* besteht aus einem *Bezeichner* , der diesen Member benennt, optional gefolgt von einem "`=`"-Token und einem *variable_initializer* ([Variableninitialisierer](classes.md#variable-initializers)), der den Anfangswert dieses Members enthält.</span><span class="sxs-lookup"><span data-stu-id="6443e-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="6443e-611">Der *Typ* eines Felds muss mindestens so zugänglich sein wie das Feld selbst (Barrierefreiheits[Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6443e-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6443e-612">Der Wert eines Felds wird in einem Ausdruck mithilfe eines *Simple_name* ([simple names](expressions.md#simple-names)) oder eines *member_access* ([Member Access](expressions.md#member-access)) abgerufen.</span><span class="sxs-lookup"><span data-stu-id="6443e-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="6443e-613">Der Wert eines nicht schreibgeschützten Felds wird mithilfe einer *Zuweisung* ([Zuweisungs Operatoren](expressions.md#assignment-operators)) geändert.</span><span class="sxs-lookup"><span data-stu-id="6443e-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="6443e-614">Der Wert eines nicht schreibgeschützten Felds kann mit Postfix-Inkrement-und Dekrementoperatoren ([postfix-Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators)) und Präfix Inkrement-und Dekrementoperatoren ([Präfix Inkrement und Dekrement) abgerufen und geändert werden. Operatoren](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="6443e-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="6443e-615">Eine Feld Deklaration, die mehrere Felder deklariert, entspricht mehreren Deklarationen von einzelnen Feldern mit denselben Attributen, modifizierertypen und Typen.</span><span class="sxs-lookup"><span data-stu-id="6443e-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="6443e-616">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6443e-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="6443e-617">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="6443e-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="6443e-618">Statische Felder und Instanzfelder</span><span class="sxs-lookup"><span data-stu-id="6443e-618">Static and instance fields</span></span>

<span data-ttu-id="6443e-619">Wenn eine Feld Deklaration einen `static` Modifizierer enthält, sind die von der Deklaration eingeführten Felder ***statische Felder***.</span><span class="sxs-lookup"><span data-stu-id="6443e-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="6443e-620">Wenn kein `static` Modifizierer vorhanden ist, sind die von der Deklaration eingeführten Felder ***Instanzfelder***.</span><span class="sxs-lookup"><span data-stu-id="6443e-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="6443e-621">Statische Felder und Instanzfelder sind zwei der verschiedenen Arten von Variablen ([Variablen](variables.md)), die C#von unterstützt werden. manchmal werden Sie als ***statische Variablen*** bzw. ***Instanzvariablen***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="6443e-622">Ein statisches Feld ist nicht Teil einer bestimmten Instanz. Stattdessen wird Sie von allen Instanzen eines geschlossenen Typs ([offene und geschlossene Typen](types.md#open-and-closed-types)) gemeinsam verwendet.</span><span class="sxs-lookup"><span data-stu-id="6443e-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="6443e-623">Unabhängig davon, wie viele Instanzen eines geschlossenen Klassen Typs erstellt werden, gibt es nur eine Kopie eines statischen Felds für die zugehörige Anwendungsdomäne.</span><span class="sxs-lookup"><span data-stu-id="6443e-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="6443e-624">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6443e-624">For example:</span></span>
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

<span data-ttu-id="6443e-625">Ein Instanzfeld gehört zu einer Instanz.</span><span class="sxs-lookup"><span data-stu-id="6443e-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="6443e-626">Insbesondere enthält jede Instanz einer Klasse einen separaten Satz aller Instanzfelder dieser Klasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="6443e-627">Wenn auf ein Feld in einem *member_access* -Element ([Member Access](expressions.md#member-access)) der Form `E.M` verwiesen wird, wenn `M` ein statisches Feld ist, muss `E` einen Typ mit `M` angeben. wenn `M` ein Instanzfeld ist, muss E eine Instanz eines Typs angeben, der enthält. `M`.</span><span class="sxs-lookup"><span data-stu-id="6443e-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6443e-628">Die Unterschiede zwischen statischen und Instanzmembern werden in [statischen und Instanzmembern](classes.md#static-and-instance-members)ausführlicher erläutert.</span><span class="sxs-lookup"><span data-stu-id="6443e-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="6443e-629">Schreibgeschützte Felder</span><span class="sxs-lookup"><span data-stu-id="6443e-629">Readonly fields</span></span>

<span data-ttu-id="6443e-630">Wenn ein *field_declaration* einen `readonly`-Modifizierer enthält, sind die von der Deklaration eingeführten Felder schreibgeschützte ***Felder***.</span><span class="sxs-lookup"><span data-stu-id="6443e-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="6443e-631">Direkte Zuweisungen zu schreibgeschützten Feldern können nur als Teil der Deklaration oder in einem Instanzkonstruktor oder statischen Konstruktor in derselben Klasse erfolgen.</span><span class="sxs-lookup"><span data-stu-id="6443e-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="6443e-632">(Ein Schreib geschütztes Feld kann in diesen Kontexten mehrmals zugewiesen werden.) Direkte Zuweisungen zu einem `readonly` Feld sind nur in den folgenden Kontexten zulässig:</span><span class="sxs-lookup"><span data-stu-id="6443e-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="6443e-633">In der *variable_declarator* , die das Feld einführt (durch Einschließen eines *variable_initializer* in die Deklaration).</span><span class="sxs-lookup"><span data-stu-id="6443e-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="6443e-634">Für ein Instanzfeld in den Instanzkonstruktoren der-Klasse, die die Feld Deklaration enthält; für ein statisches Feld im statischen Konstruktor der Klasse, die die Feld Deklaration enthält.</span><span class="sxs-lookup"><span data-stu-id="6443e-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="6443e-635">Dies sind auch die einzigen Kontexte, in denen es zulässig ist, ein `readonly` Feld `out` als-oder `ref` -Parameter zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="6443e-636">Der Versuch, einem `readonly` Feld zuzuweisen oder es `out` als-oder `ref` -Parameter in einem anderen Kontext zu übergeben, ist ein Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="6443e-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="6443e-637">Verwenden statischer Schreib geschützter Felder für Konstanten</span><span class="sxs-lookup"><span data-stu-id="6443e-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="6443e-638">Ein `static readonly` Feld ist nützlich, wenn ein symbolischer Name für einen konstanten Wert erwünscht ist, aber wenn der Typ des Werts nicht in einer `const` Deklaration zulässig ist oder wenn der Wert nicht zur Kompilierzeit berechnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="6443e-639">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-639">In the example</span></span>
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
<span data-ttu-id="6443e-640">die `Black`Member `White`,, `Red`, undkönnen`Blue` nicht als`const` Member deklariert werden, da ihre Werte zur Kompilierzeit nicht berechnet werden können. `Green`</span><span class="sxs-lookup"><span data-stu-id="6443e-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="6443e-641">Das deklarieren `static readonly` der Dateien hat jedoch die gleiche Wirkung.</span><span class="sxs-lookup"><span data-stu-id="6443e-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="6443e-642">Versionierung von Konstanten und statischen schreibgeschützten Feldern</span><span class="sxs-lookup"><span data-stu-id="6443e-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="6443e-643">Konstanten und schreibgeschützte Felder haben unterschiedliche Semantik für die binäre Versionsverwaltung.</span><span class="sxs-lookup"><span data-stu-id="6443e-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="6443e-644">Wenn ein Ausdruck auf eine Konstante verweist, wird der Wert der Konstante zur Kompilierzeit abgerufen. Wenn jedoch ein Ausdruck auf ein Schreib geschütztes Feld verweist, wird der Wert des Felds bis zur Laufzeit nicht abgerufen.</span><span class="sxs-lookup"><span data-stu-id="6443e-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="6443e-645">Stellen Sie sich eine Anwendung vor, die aus zwei separaten Programmen besteht:</span><span class="sxs-lookup"><span data-stu-id="6443e-645">Consider an application that consists of two separate programs:</span></span>
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

<span data-ttu-id="6443e-646">Die `Program1` Namespaces und `Program2` bezeichnen zwei Programme, die separat kompiliert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="6443e-647">Da `Program1.Utils.X` als statisches Schreib geschütztes Feld deklariert ist, ist der Wert, der `Console.WriteLine` von der-Anweisung ausgegeben wird, zur Kompilierzeit nicht bekannt, sondern wird zur Laufzeit abgerufen.</span><span class="sxs-lookup"><span data-stu-id="6443e-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="6443e-648">Wenn also `X` der Wert von geändert und `Program1` neu kompiliert wird, gibt die `Console.WriteLine` Anweisung den neuen Wert auch dann aus, wenn `Program2` nicht neu kompiliert wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="6443e-649">`Program2` `X` `Program2` `Program1` War jedoch eine Konstante. der Wert von wurde zum Zeitpunkt der Kompilierung abgerufen und bleibt von Änderungen in bis zur erneuten Kompilierung nicht beeinträchtigt. `X`</span><span class="sxs-lookup"><span data-stu-id="6443e-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="6443e-650">Flüchtige Felder</span><span class="sxs-lookup"><span data-stu-id="6443e-650">Volatile fields</span></span>

<span data-ttu-id="6443e-651">Wenn ein *field_declaration* einen `volatile`-Modifizierer enthält, sind die von dieser Deklaration eingeführten Felder ***flüchtige Felder***.</span><span class="sxs-lookup"><span data-stu-id="6443e-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="6443e-652">Bei nicht flüchtigen Feldern können Optimierungstechniken, die Anweisungen neu anordnen, zu unerwarteten und unvorhersehbaren Ergebnissen in Multithreadprogrammen führen, die auf Felder ohne Synchronisierung zugreifen, wie z. b. die von *lock_statement* bereitgestellte ([die Lock-Anweisung](statements.md#the-lock-statement)).</span><span class="sxs-lookup"><span data-stu-id="6443e-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="6443e-653">Diese Optimierungen können vom Compiler, vom Laufzeitsystem oder von der Hardware ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="6443e-654">Für flüchtige Felder sind solche neubestellungs Optimierungen eingeschränkt:</span><span class="sxs-lookup"><span data-stu-id="6443e-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="6443e-655">Ein Lesevorgang eines flüchtigen Felds wird als ***flüchtiger Lese***Vorgang bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="6443e-656">Ein flüchtiger Lesevorgang hat "Semantik abrufen". Das heißt, es wird sichergestellt, dass es vor allen in der Anweisungs Sequenz auftretenden verweisen auf den Speicher auftritt.</span><span class="sxs-lookup"><span data-stu-id="6443e-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="6443e-657">Ein Schreibvorgang eines flüchtigen Felds wird als ***flüchtiges schreiben***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="6443e-658">Ein flüchtiger Schreibvorgang weist die Freigabe Semantik auf. Das heißt, es wird sichergestellt, dass es nach allen Speicher verweisen vor der Schreib Anweisung in der Anweisungs Sequenz erfolgt.</span><span class="sxs-lookup"><span data-stu-id="6443e-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="6443e-659">Diese Einschränkungen stellen sicher, dass alle Threads volatile-Schreibvorgänge, die von einem anderen Thread ausgeführt werden, in der Reihenfolge ihrer Ausführung berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="6443e-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="6443e-660">Eine konforme Implementierung ist nicht erforderlich, um eine einzelne Gesamt Reihenfolge von flüchtigen Schreibvorgängen zu gewährleisten, die von allen Ausführungs Threads erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="6443e-661">Der Typ eines flüchtigen Felds muss einer der folgenden sein:</span><span class="sxs-lookup"><span data-stu-id="6443e-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="6443e-662">Ein *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="6443e-662">A *reference_type*.</span></span>
*  <span data-ttu-id="6443e-663">Der Typ `byte`, `sbyte`, `short`, ,`ushort`, ,`float`,,, oder`System.IntPtr`. `char` `uint` `int` `bool` `System.UIntPtr`</span><span class="sxs-lookup"><span data-stu-id="6443e-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or `System.UIntPtr`.</span></span>
*  <span data-ttu-id="6443e-664">Ein *enum_type* mit dem Aufzählungs Basistyp `byte`, `sbyte`, `short`, `ushort`, `int` oder `uint`.</span><span class="sxs-lookup"><span data-stu-id="6443e-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="6443e-665">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-665">The example</span></span>
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
<span data-ttu-id="6443e-666">erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6443e-666">produces the output:</span></span>
```console
result = 143
```

<span data-ttu-id="6443e-667">In diesem Beispiel startet die- `Main` Methode einen neuen Thread, der die- `Thread2`Methode ausführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="6443e-668">Diese Methode speichert einen Wert in ein nicht flüchtiges Feld mit `result`dem Namen und `true` speichert dann im flüchtigen `finished`Feld.</span><span class="sxs-lookup"><span data-stu-id="6443e-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="6443e-669">Der Haupt Thread wartet darauf, dass `finished` das Feld auf `true`festgelegt wird, und liest `result`dann das Feld.</span><span class="sxs-lookup"><span data-stu-id="6443e-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="6443e-670">Da `finished` `143` `result`deklariert `volatile`wurde, muss der Haupt Thread den Wert aus dem Feld lesen.</span><span class="sxs-lookup"><span data-stu-id="6443e-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="6443e-671">Wenn das Feld `finished` nicht deklariert `volatile`wurde, ist es `result` zulässig, dass der Speicher für den Haupt Thread nach dem Speichern in `finished`sichtbar ist, sodass der Haupt Thread den Wert `0` aus dem ein `result`.</span><span class="sxs-lookup"><span data-stu-id="6443e-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="6443e-672">Das `finished` deklarieren `volatile` als Feld verhindert jegliche Inkonsistenz.</span><span class="sxs-lookup"><span data-stu-id="6443e-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="6443e-673">Feld Initialisierung</span><span class="sxs-lookup"><span data-stu-id="6443e-673">Field initialization</span></span>

<span data-ttu-id="6443e-674">Der Anfangswert eines Felds, unabhängig davon, ob es sich um ein statisches Feld oder ein Instanzfeld handelt, ist der Standardwert ([Standardwerte](variables.md#default-values)) des Feldtyps.</span><span class="sxs-lookup"><span data-stu-id="6443e-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="6443e-675">Es ist nicht möglich, den Wert eines Felds zu beobachten, bevor diese Standard Initialisierung erfolgt ist, und ein Feld wird daher nie "nicht initialisiert".</span><span class="sxs-lookup"><span data-stu-id="6443e-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="6443e-676">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-676">The example</span></span>
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
<span data-ttu-id="6443e-677">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="6443e-677">produces the output</span></span>
```console
b = False, i = 0
```
<span data-ttu-id="6443e-678">, `b` da `i` und automatisch mit den Standardwerten initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="6443e-679">Variableninitialisierer</span><span class="sxs-lookup"><span data-stu-id="6443e-679">Variable initializers</span></span>

<span data-ttu-id="6443e-680">Feld Deklarationen können *variable_initializer*s enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="6443e-681">Bei statischen Feldern entsprechen Variableninitialisierern Zuweisungs Anweisungen, die während der Initialisierung der Klasse ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="6443e-682">Bei Instanzfeldern entsprechen Variableninitialisierern Zuweisungs Anweisungen, die ausgeführt werden, wenn eine Instanz der Klasse erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="6443e-683">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-683">The example</span></span>
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
<span data-ttu-id="6443e-684">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="6443e-684">produces the output</span></span>
```console
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="6443e-685">eine Zuweisung zu erfolgt `x` , wenn statische Feldinitialisierer ausgeführt werden `i` und `s` Zuweisungen zu und auftreten, wenn die instanzfeldinitialisierer ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="6443e-686">Die in der [Feld Initialisierung](classes.md#field-initialization) beschriebene Standardwert Initialisierung tritt für alle Felder auf, einschließlich der Felder, die über Variableninitialisierer verfügen.</span><span class="sxs-lookup"><span data-stu-id="6443e-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="6443e-687">Wenn eine Klasse initialisiert wird, werden daher alle statischen Felder in dieser Klasse zuerst mit ihren Standardwerten initialisiert, und anschließend werden die statischen Feldinitialisierer in der Textfolge ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="6443e-688">Wenn eine Instanz einer Klasse erstellt wird, werden alle Instanzfelder in dieser Instanz zuerst mit ihren Standardwerten initialisiert, und anschließend werden die instanzfeldinitialisierer in der Textfolge ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="6443e-689">Es ist möglich, dass statische Felder mit Variableninitialisierern in ihrem Standardwert Zustand beobachtet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="6443e-690">Dies wird jedoch dringend von einem Stil abgeraten.</span><span class="sxs-lookup"><span data-stu-id="6443e-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="6443e-691">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-691">The example</span></span>
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
<span data-ttu-id="6443e-692">weist dieses Verhalten auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-692">exhibits this behavior.</span></span> <span data-ttu-id="6443e-693">Trotz der zirkulären Definitionen von a und b ist das Programm gültig.</span><span class="sxs-lookup"><span data-stu-id="6443e-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="6443e-694">Dadurch wird die Ausgabe ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-694">It results in the output</span></span>
```console
a = 1, b = 2
```
<span data-ttu-id="6443e-695">Da die statischen Felder `a` und `b` auf (den Standard `0` Wert für `int`) initialisiert werden, bevor Ihre Initialisierer ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="6443e-696">Wenn der Initialisierer für `a` ausgeführt wird, ist der `b` Wert von 0 (NULL `a` ) und wird daher `1`mit initialisiert.</span><span class="sxs-lookup"><span data-stu-id="6443e-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="6443e-697">Wenn der Initialisierer für `b` ausgeführt wird, ist der `a` Wert von `1`bereits, und `b` wird daher mit `2`initialisiert.</span><span class="sxs-lookup"><span data-stu-id="6443e-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="6443e-698">Initialisierung statischer Felder</span><span class="sxs-lookup"><span data-stu-id="6443e-698">Static field initialization</span></span>

<span data-ttu-id="6443e-699">Die statischen feldvariableninitialisierer einer Klasse entsprechen einer Sequenz von Zuweisungen, die in der Text Reihenfolge ausgeführt werden, in der Sie in der Klassen Deklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="6443e-700">Wenn ein statischer Konstruktor ([statischer Konstruktor](classes.md#static-constructors)) in der Klasse vorhanden ist, erfolgt die Ausführung der statischen Feldinitialisierer unmittelbar vor der Ausführung dieses statischen Konstruktors.</span><span class="sxs-lookup"><span data-stu-id="6443e-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="6443e-701">Andernfalls werden die statischen Feldinitialisierer zu einem Implementierungs abhängigen Zeitpunkt vor der ersten Verwendung eines statischen Felds dieser Klasse ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="6443e-702">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-702">The example</span></span>
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
<span data-ttu-id="6443e-703">erzeugt möglicherweise die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6443e-703">might produce either the output:</span></span>
```console
Init A
Init B
1 1
```
<span data-ttu-id="6443e-704">oder die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6443e-704">or the output:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="6443e-705">Da die Ausführung des `X`Initialisierers und `Y`des Initialisierers in einer der beiden Reihenfolge auftreten kann, sind Sie nur so eingeschränkt, dass Sie vor den verweisen auf diese Felder auftreten.</span><span class="sxs-lookup"><span data-stu-id="6443e-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="6443e-706">Im Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6443e-706">However, in the example:</span></span>
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
<span data-ttu-id="6443e-707">die Ausgabe muss wie folgt lauten:</span><span class="sxs-lookup"><span data-stu-id="6443e-707">the output must be:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="6443e-708">Da die Regeln für den Fall, dass statische Konstruktoren ausgeführt werden (wie in [statischen Konstruktoren](classes.md#static-constructors)definiert), den statischen Konstruktor `B`(und somit die statischen Feldinitialisierer) bereitstellen, `B`müssen vor `A`der statischen ausgeführt werden. Konstruktor und Feldinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="6443e-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="6443e-709">Instanzfeldinitialisierung</span><span class="sxs-lookup"><span data-stu-id="6443e-709">Instance field initialization</span></span>

<span data-ttu-id="6443e-710">Die instanzfeldvariableninitialisierer einer Klasse entsprechen einer Sequenz von Zuweisungen, die unmittelbar nach dem Eintrag für einen der Instanzkonstruktoren ([Konstruktorinitialisierer](classes.md#constructor-initializers)) dieser Klasse ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="6443e-711">Die Variableninitialisierer werden in der Text Reihenfolge ausgeführt, in der Sie in der Klassen Deklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="6443e-712">Die Erstellung und Initialisierung der Klasseninstanz wird in [Instanzkonstruktoren](classes.md#instance-constructors)ausführlicher beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="6443e-713">Ein Variableninitialisierer für ein Instanzfeld kann nicht auf die erstellte Instanz verweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="6443e-714">Daher ist es ein Kompilierzeitfehler, auf `this` in einem Variableninitialisierer zu verweisen, da es sich um einen Kompilierzeitfehler für einen Variableninitialisierer handelt, der auf ein beliebiges Instanzmember über eine *Simple_name*verweist.</span><span class="sxs-lookup"><span data-stu-id="6443e-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="6443e-715">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="6443e-716">der Variableninitialisierer `y` für führt zu einem Kompilierzeitfehler, da er auf einen Member der Instanz verweist, die erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="6443e-717">Methoden</span><span class="sxs-lookup"><span data-stu-id="6443e-717">Methods</span></span>

<span data-ttu-id="6443e-718">Eine ***Methode*** ist ein Member, das eine Berechnung oder eine Aktion implementiert, die durch ein Objekt oder eine Klasse durchgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="6443e-719">Methoden werden mit *method_declaration*s deklariert:</span><span class="sxs-lookup"><span data-stu-id="6443e-719">Methods are declared using *method_declaration*s:</span></span>

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

<span data-ttu-id="6443e-720">Ein *method_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)), den `new` ([den neuen Modifizierer](classes.md#the-new-modifier)), `static` ([statisch und Instanz) enthalten. Methoden](classes.md#static-and-instance-methods)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Überschreibungs Methoden](classes.md#override-methods)), `sealed` ([versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakte Methoden](classes.md#abstract-methods)) und `extern`-Modifizierer ([Externe Methoden](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="6443e-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6443e-721">Eine-Deklaration hat eine gültige Kombination von Modifizierer, wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="6443e-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="6443e-722">Die-Deklaration enthält eine gültige Kombination von [zugriffsmodifizierermodifizierer](classes.md#access-modifiers)</span><span class="sxs-lookup"><span data-stu-id="6443e-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="6443e-723">Die-Deklaration enthält nicht denselben Modifizierer mehrmals.</span><span class="sxs-lookup"><span data-stu-id="6443e-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="6443e-724">Die-Deklaration enthält höchstens einen der folgenden Modifizierer `static`: `virtual`, und `override`.</span><span class="sxs-lookup"><span data-stu-id="6443e-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="6443e-725">Die-Deklaration enthält höchstens einen der folgenden Modifizierer `new` : `override`und.</span><span class="sxs-lookup"><span data-stu-id="6443e-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="6443e-726">Wenn die Deklaration den `abstract` -Modifizierer enthält, enthält die Deklaration keinen der folgenden Modifizierer `static`: `virtual`, `sealed` oder `extern`.</span><span class="sxs-lookup"><span data-stu-id="6443e-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="6443e-727">Wenn die Deklaration den `private` -Modifizierer enthält, enthält die Deklaration keinen der folgenden Modifizierer `virtual`: `override`, oder `abstract`.</span><span class="sxs-lookup"><span data-stu-id="6443e-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="6443e-728">Wenn die Deklaration den `sealed` -Modifizierer enthält, enthält die Deklaration auch den `override` -Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6443e-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="6443e-729">Wenn die Deklaration den `partial` -Modifizierer enthält, enthält Sie keinen der folgenden Modifizierer: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`oder .`extern`</span><span class="sxs-lookup"><span data-stu-id="6443e-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="6443e-730">Eine Methode, die über `async` den-Modifizierer verfügt, ist eine Async-Funktion und befolgt die in [Async-Funktionen](classes.md#async-functions)beschriebenen Regeln.</span><span class="sxs-lookup"><span data-stu-id="6443e-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="6443e-731">Der *return_type* einer Methoden Deklaration gibt den Typ des Werts an, der von der-Methode berechnet und zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="6443e-732">Der *return_type* -Wert ist `void`, wenn die Methode keinen Wert zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="6443e-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="6443e-733">Wenn die Deklaration den `partial` -Modifizierer enthält, muss der Rückgabetyp sein. `void`</span><span class="sxs-lookup"><span data-stu-id="6443e-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="6443e-734">*MEMBER_NAME* gibt den Namen der Methode an.</span><span class="sxs-lookup"><span data-stu-id="6443e-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="6443e-735">Wenn die Methode keine explizite Schnittstellenmember-Implementierung ist ([explizite Schnittstellenmember-Implementierungen](interfaces.md#explicit-interface-member-implementations)), ist *MEMBER_NAME* einfach ein *Bezeichner*.</span><span class="sxs-lookup"><span data-stu-id="6443e-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="6443e-736">Bei einer expliziten Schnittstellenmember-Implementierung besteht das *MEMBER_NAME* aus einem *INTERFACE_TYPE* , gefolgt von einem "`.`" und einem *Bezeichner*.</span><span class="sxs-lookup"><span data-stu-id="6443e-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="6443e-737">Der optionale *type_parameter_list* -Parameter gibt die Typparameter der Methode ([Typparameter](classes.md#type-parameters)) an.</span><span class="sxs-lookup"><span data-stu-id="6443e-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="6443e-738">Wenn *type_parameter_list* angegeben wird, handelt es sich bei der Methode um eine ***generische Methode***.</span><span class="sxs-lookup"><span data-stu-id="6443e-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="6443e-739">Wenn die Methode einen `extern`-Modifizierer aufweist, kann kein *type_parameter_list* angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="6443e-740">Mit dem optionalen *formal_parameter_list* werden die Parameter der Methode ([Methoden Parameter](classes.md#method-parameters)) angegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="6443e-741">Die optionalen *type_parameter_constraints_clause*s geben Einschränkungen für einzelne Typparameter an ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) und können nur angegeben werden, wenn eine *type_parameter_list* ebenfalls bereitgestellt wird, und die Methode hat keine `override`-Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6443e-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="6443e-742">Die *return_type* und alle Typen, auf die im *formal_parameter_list* einer Methode verwiesen wird, müssen mindestens so zugänglich sein wie die Methode selbst ([Barrierefreiheits Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6443e-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6443e-743">Der *method_body* ist entweder ein Semikolon, ein ***Anweisungs Text*** oder ein ***Ausdrucks Körper***.</span><span class="sxs-lookup"><span data-stu-id="6443e-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="6443e-744">Ein Anweisungs Text besteht aus einem- *Block*, der die auszuführenden Anweisungen angibt, wenn die-Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="6443e-745">Ein Ausdrucks Körper besteht aus `=>` , gefolgt von einem *Ausdruck* und einem Semikolon und deutet auf einen einzelnen Ausdruck hin, der beim Aufrufen der Methode ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="6443e-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="6443e-746">Bei `abstract`-und `extern`-Methode besteht das *method_body* einfach aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="6443e-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="6443e-747">Bei `partial`-Methoden kann der *method_body* entweder aus einem Semikolon, einem Block Text oder einem Ausdrucks Körper bestehen.</span><span class="sxs-lookup"><span data-stu-id="6443e-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="6443e-748">Für alle anderen Methoden ist *method_body* entweder ein Block Körper oder ein Ausdrucks Text.</span><span class="sxs-lookup"><span data-stu-id="6443e-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="6443e-749">Wenn *method_body* aus einem Semikolon besteht, enthält die Deklaration möglicherweise nicht den `async`-Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6443e-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="6443e-750">Der Name, die Typparameter Liste und die Liste der formalen Parameter einer Methode definieren die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) der Methode.</span><span class="sxs-lookup"><span data-stu-id="6443e-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="6443e-751">Insbesondere besteht die Signatur einer Methode aus dem Namen, der Anzahl der Typparameter und der Anzahl, den modifizierertypen und den Typen der formalen Parameter.</span><span class="sxs-lookup"><span data-stu-id="6443e-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="6443e-752">Zu diesem Zweck wird jeder Typparameter der Methode, die im Typ eines formalen Parameters vorkommt, nicht anhand seines Namens identifiziert, sondern anhand seiner Ordinalposition in der Typargument Liste der Methode. Der Rückgabetyp ist weder Teil der Signatur einer Methode noch die Namen der Typparameter oder der formalen Parameter.</span><span class="sxs-lookup"><span data-stu-id="6443e-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="6443e-753">Der Name einer Methode muss sich von den Namen aller anderen nicht-Methoden unterscheiden, die in derselben Klasse deklariert sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="6443e-754">Außerdem muss sich die Signatur einer Methode von den Signaturen aller anderen Methoden unterscheiden, die in derselben Klasse deklariert sind, und zwei Methoden, die in derselben Klasse deklariert werden, dürfen keine Signaturen aufweisen, `ref` die `out`sich ausschließlich durch und unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="6443e-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="6443e-755">Die *type_parameter*s der Methode befinden sich im Gültigkeitsbereich des *method_declaration*und können zum bilden von Typen in diesem Bereich in *return_type*, *method_body*und *type_parameter_constraints_clause*s verwendet werden, aber nicht in *Attribute*.</span><span class="sxs-lookup"><span data-stu-id="6443e-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="6443e-756">Alle formalen Parameter und Typparameter müssen unterschiedliche Namen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="6443e-757">Methoden Parameter</span><span class="sxs-lookup"><span data-stu-id="6443e-757">Method parameters</span></span>

<span data-ttu-id="6443e-758">Die Parameter einer Methode, sofern vorhanden, werden vom *formal_parameter_list*der Methode deklariert.</span><span class="sxs-lookup"><span data-stu-id="6443e-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

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

<span data-ttu-id="6443e-759">Die Liste der formalen Parameter besteht aus mindestens einem durch Trennzeichen getrennten Parameter, bei dem nur der letzte ein *parameter_array*sein kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="6443e-760">Ein *fixed_parameter* besteht aus einem optionalen Satz von *Attributen* ([Attributen](attributes.md)), einem optionalen `ref`-, `out`-oder `this`-Modifizierer, einem *Typ*, einem *Bezeichner* und einem optionalen *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="6443e-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="6443e-761">Jede *fixed_parameter* deklariert einen Parameter des angegebenen Typs mit dem angegebenen Namen.</span><span class="sxs-lookup"><span data-stu-id="6443e-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="6443e-762">Der `this` -Modifizierer legt die-Methode als Erweiterungsmethode fest und ist nur für den ersten Parameter einer statischen Methode zulässig.</span><span class="sxs-lookup"><span data-stu-id="6443e-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="6443e-763">Erweiterungs Methoden werden weiter unten in den [Erweiterungs Methoden](classes.md#extension-methods)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="6443e-764">Ein *fixed_parameter* mit einem *default_argument* -Wert wird als ***optionaler Parameter***bezeichnet, wohingegen ein *fixed_parameter* ohne *default_argument* ein ***erforderlicher Parameter***ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="6443e-765">Ein erforderlicher Parameter darf nicht nach einem optionalen Parameter in einer *formal_parameter_list*angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="6443e-766">Ein `ref`-oder `out`-Parameter darf keinen *default_argument*aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="6443e-767">Der *Ausdruck* in einem *default_argument* muss einer der folgenden sein:</span><span class="sxs-lookup"><span data-stu-id="6443e-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="6443e-768">eine *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="6443e-768">a *constant_expression*</span></span>
*  <span data-ttu-id="6443e-769">ein Ausdruck der Form `new S()` , in `S` der ein Werttyp ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="6443e-770">ein Ausdruck der Form `default(S)` , in `S` der ein Werttyp ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="6443e-771">Der *Ausdruck* muss implizit durch eine Identität oder eine Konvertierung in den Typ des Parameters konvertiert werden können.</span><span class="sxs-lookup"><span data-stu-id="6443e-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="6443e-772">Wenn optionale Parameter in einer implementierenden partiellen Methoden Deklaration ([partielle Methoden](classes.md#partial-methods)) auftreten, eine explizite Schnittstellenmember-Implementierung ([explizite Schnittstellenmember-Implementierungen](interfaces.md#explicit-interface-member-implementations)) oder in einer Indexer-Deklaration mit einem einzelnen Parameter ([ Indexer](classes.md#indexers)) der Compiler sollte eine Warnung angeben, da diese Member niemals auf eine Weise aufgerufen werden können, die das Weglassen von Argumenten zulässt.</span><span class="sxs-lookup"><span data-stu-id="6443e-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="6443e-773">Ein *parameter_array* besteht aus einem optionalen Satz von *Attributen* ([Attributen](attributes.md)), einem `params`-Modifizierer, einem *array_type*und einem *Bezeichner*.</span><span class="sxs-lookup"><span data-stu-id="6443e-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="6443e-774">Ein Parameter Array deklariert einen einzelnen Parameter des angegebenen Array Typs mit dem angegebenen Namen.</span><span class="sxs-lookup"><span data-stu-id="6443e-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="6443e-775">Der *array_type* eines Parameter Arrays muss ein eindimensionaler Arraytyp ([Array Typen](arrays.md#array-types)) sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="6443e-776">Bei einem Methodenaufruf ermöglicht ein Parameter Array, dass entweder ein einzelnes Argument des angegebenen Array Typs angegeben wird, oder es lässt NULL oder mehr Argumente des Array Elementtyps angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="6443e-777">Parameter Arrays werden in [Parameter Arrays](classes.md#parameter-arrays)ausführlicher beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="6443e-778">Ein *parameter_array* kann nach einem optionalen Parameter auftreten, kann aber keinen Standardwert aufweisen. das Weglassen von Argumenten für eine *parameter_array* würde stattdessen zur Erstellung eines leeren Arrays führen.</span><span class="sxs-lookup"><span data-stu-id="6443e-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="6443e-779">Das folgende Beispiel veranschaulicht verschiedene Arten von Parametern:</span><span class="sxs-lookup"><span data-stu-id="6443e-779">The following example illustrates different kinds of parameters:</span></span>
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

<span data-ttu-id="6443e-780">In *formal_parameter_list* für `M` ist `i` ein erforderlicher ref-Parameter. `d` ist ein erforderlicher Wert Parameter, `b`, `s`, `o` und `t` sind optionale Wert Parameter und `a` ein Parameter Array.</span><span class="sxs-lookup"><span data-stu-id="6443e-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="6443e-781">Eine Methoden Deklaration erstellt einen separaten Deklarations Bereich für Parameter, Typparameter und lokale Variablen.</span><span class="sxs-lookup"><span data-stu-id="6443e-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="6443e-782">Namen werden in diesem Deklarations Bereich durch die Typparameter Liste und die Liste formaler Parameter der-Methode und durch lokale Variablen Deklarationen im- *Block* der-Methode eingeführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="6443e-783">Es ist ein Fehler, wenn zwei Member eines Methoden Deklarations Raums denselben Namen haben.</span><span class="sxs-lookup"><span data-stu-id="6443e-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="6443e-784">Es ist ein Fehler für den Deklarations Bereich der Methode und der Deklarations Bereich der lokalen Variablen eines in der Tabelle enthaltenen Deklarations Raums, der Elemente mit dem gleichen Namen enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="6443e-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="6443e-785">Ein Methodenaufruf ([Methodenaufrufe](expressions.md#method-invocations)) erstellt eine Kopie, die für diesen Aufruf spezifisch ist, der formalen Parameter und lokalen Variablen der Methode, und die Argumentliste des Aufrufs weist Werte oder Variablen Verweise auf den neu erstellten formalen Metern.</span><span class="sxs-lookup"><span data-stu-id="6443e-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="6443e-786">Innerhalb des- *Blocks* einer Methode kann von ihren bezeichtern in *Simple_name* -Ausdrücken ([einfachen Namen](expressions.md#simple-names)) auf formale Parameter verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="6443e-787">Es gibt vier Arten von formalen Parametern:</span><span class="sxs-lookup"><span data-stu-id="6443e-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="6443e-788">Wert Parameter, die ohne modifiziererer deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="6443e-789">Verweis Parameter, die mit dem `ref` -Modifizierer deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="6443e-790">Ausgabeparameter, die mit dem `out` -Modifizierer deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="6443e-791">Parameter Arrays, die mit dem `params` -Modifizierer deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="6443e-792">Wie in [Signaturen und überladen](basic-concepts.md#signatures-and-overloading)beschrieben, sind `ref` die `out` Modifizierer und Teil der Signatur einer Methode, der `params` -Modifizierer ist jedoch nicht.</span><span class="sxs-lookup"><span data-stu-id="6443e-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="6443e-793">Wert Parameter</span><span class="sxs-lookup"><span data-stu-id="6443e-793">Value parameters</span></span>

<span data-ttu-id="6443e-794">Ein Parameter, der ohne Modifizierer deklariert wird, ist ein value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="6443e-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="6443e-795">Ein value-Parameter entspricht einer lokalen Variablen, die den Anfangswert aus dem entsprechenden Argument abruft, das im Methodenaufruf angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="6443e-796">Wenn ein formaler Parameter ein value-Parameter ist, muss das entsprechende Argument in einem Methodenaufruf ein Ausdruck sein, der implizit konvertierbar ist ([implizite Konvertierungen](conversions.md#implicit-conversions)), in den formalen Parametertyp.</span><span class="sxs-lookup"><span data-stu-id="6443e-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="6443e-797">Eine Methode darf einem Wert Parameter neue Werte zuweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="6443e-798">Solche Zuweisungen wirken sich nur auf den lokalen Speicherort aus, der durch den value-Parameter dargestellt wird – Sie haben keine Auswirkung auf das tatsächliche Argument, das im Methodenaufruf angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="6443e-799">Verweisparameter</span><span class="sxs-lookup"><span data-stu-id="6443e-799">Reference parameters</span></span>

<span data-ttu-id="6443e-800">Ein Parameter, der mit `ref` einem-Modifizierer deklariert wird, ist ein Verweis Parameter.</span><span class="sxs-lookup"><span data-stu-id="6443e-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="6443e-801">Anders als bei einem value-Parameter erstellt ein Verweis Parameter keinen neuen Speicherort.</span><span class="sxs-lookup"><span data-stu-id="6443e-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="6443e-802">Stattdessen stellt ein Verweis Parameter denselben Speicherort wie die Variable dar, die im Methodenaufruf als Argument angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="6443e-803">Wenn ein formaler Parameter ein Verweis Parameter ist, muss das entsprechende Argument in einem Methodenaufruf aus dem Schlüsselwort `ref` gefolgt von einem *variable_reference* ([exakte Regeln zum Bestimmen der eindeutigen Zuweisung](variables.md#precise-rules-for-determining-definite-assignment)) desselben Typs bestehen wie der formale Parameter.</span><span class="sxs-lookup"><span data-stu-id="6443e-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="6443e-804">Eine Variable muss definitiv zugewiesen werden, bevor Sie als Verweis Parameter übergeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="6443e-805">Innerhalb einer Methode wird ein Verweis Parameter immer als definitiv zugewiesen betrachtet.</span><span class="sxs-lookup"><span data-stu-id="6443e-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="6443e-806">Eine Methode, die als Iterator ([Iteratoren](classes.md#iterators)) deklariert ist, darf keine Verweis Parameter aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="6443e-807">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-807">The example</span></span>
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
<span data-ttu-id="6443e-808">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="6443e-808">produces the output</span></span>
```console
i = 2, j = 1
```

<span data-ttu-id="6443e-809">Für den `Swap` Aufruf von `Main`in stellt `i`und dar`y` .`j` `x`</span><span class="sxs-lookup"><span data-stu-id="6443e-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="6443e-810">Folglich hat der Aufruf die Auswirkungen, die Werte von `i` und `j`auszutauschen.</span><span class="sxs-lookup"><span data-stu-id="6443e-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="6443e-811">In einer Methode, die Verweis Parameter annimmt, ist es möglich, dass mehrere Namen denselben Speicherort darstellen.</span><span class="sxs-lookup"><span data-stu-id="6443e-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="6443e-812">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-812">In the example</span></span>
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
<span data-ttu-id="6443e-813">der `F` Aufruf von `b`in `G` übergibt einen Verweis auf `s` sowohl `a` für als auch für.</span><span class="sxs-lookup"><span data-stu-id="6443e-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="6443e-814">Daher beziehen sich bei `s`diesem Aufruf die Namen `a`, und `b` alle auf denselben Speicherort, und die drei Zuweisungen ändern das Instanzfeld `s`.</span><span class="sxs-lookup"><span data-stu-id="6443e-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="6443e-815">Ausgabeparameter</span><span class="sxs-lookup"><span data-stu-id="6443e-815">Output parameters</span></span>

<span data-ttu-id="6443e-816">Ein mit einem `out` -Modifizierer deklarierter Parameter ist ein Output-Parameter.</span><span class="sxs-lookup"><span data-stu-id="6443e-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="6443e-817">Ähnlich wie ein Verweis Parameter wird von einem Output-Parameter kein neuer Speicherort erstellt.</span><span class="sxs-lookup"><span data-stu-id="6443e-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="6443e-818">Stattdessen stellt ein Ausgabeparameter denselben Speicherort wie die Variable dar, die im Methodenaufruf als Argument angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="6443e-819">Wenn ein formaler Parameter ein Ausgabeparameter ist, muss das entsprechende Argument in einem Methodenaufruf aus dem Schlüsselwort `out` gefolgt von einem *variable_reference* ([exakte Regeln zum Bestimmen der eindeutigen Zuweisung](variables.md#precise-rules-for-determining-definite-assignment)) desselben Typs wie dem bestehen. formaler Parameter.</span><span class="sxs-lookup"><span data-stu-id="6443e-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="6443e-820">Eine Variable muss nicht definitiv zugewiesen werden, bevor Sie als Output-Parameter übergeben werden kann. nach einem Aufruf, bei dem eine Variable als Output-Parameter übergeben wurde, wird die Variable als definitiv zugewiesen betrachtet.</span><span class="sxs-lookup"><span data-stu-id="6443e-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="6443e-821">Innerhalb einer Methode wird ein Ausgabeparameter, wie eine lokale Variable, anfänglich als nicht zugewiesen betrachtet und muss definitiv zugewiesen werden, bevor der Wert verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="6443e-822">Jeder Ausgabeparameter einer Methode muss definitiv zugewiesen werden, bevor die Methode zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="6443e-823">Eine Methode, die als partielle Methode ([partielle Methoden](classes.md#partial-methods)) oder Iterator ([Iteratoren](classes.md#iterators)) deklariert ist, darf keine Ausgabeparameter aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="6443e-824">Ausgabeparameter werden in der Regel in Methoden verwendet, die mehrere Rückgabewerte liefern.</span><span class="sxs-lookup"><span data-stu-id="6443e-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="6443e-825">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6443e-825">For example:</span></span>
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

<span data-ttu-id="6443e-826">Das Beispiel erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6443e-826">The example produces the output:</span></span>
```console
c:\Windows\System\
hello.txt
```

<span data-ttu-id="6443e-827">Beachten Sie, `dir` dass `name` die Zuweisung der-Variable und der-Variablen aufgeh `SplitPath`oben werden kann, bevor Sie an übermittelt werden, und dass Sie nach dem-Befehl definitiv zugewiesen werden</span><span class="sxs-lookup"><span data-stu-id="6443e-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="6443e-828">Parameterarrays</span><span class="sxs-lookup"><span data-stu-id="6443e-828">Parameter arrays</span></span>

<span data-ttu-id="6443e-829">Ein Parameter, der mit `params` einem-Modifizierer deklariert wird, ist ein Parameter Array.</span><span class="sxs-lookup"><span data-stu-id="6443e-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="6443e-830">Wenn eine Liste formaler Parameter ein Parameter Array enthält, muss es sich um den letzten Parameter in der Liste handeln, und es muss sich um einen eindimensionalen Arraytyp handeln.</span><span class="sxs-lookup"><span data-stu-id="6443e-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="6443e-831">Beispielsweise können die- `string[]` Typen `string[][]` und als Typ eines Parameter Arrays verwendet werden, aber der-Typ `string[,]` ist nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="6443e-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="6443e-832">Es ist nicht möglich, den `params` -Modifizierer mit den Modifizierern `ref` und `out`zu kombinieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="6443e-833">Ein Parameter Array ermöglicht das Angeben von Argumenten in einem Methodenaufruf auf zwei Arten:</span><span class="sxs-lookup"><span data-stu-id="6443e-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="6443e-834">Das für ein Parameter Array angegebene Argument kann ein einzelner Ausdruck sein, der implizit konvertierbar ist ([implizite Konvertierungen](conversions.md#implicit-conversions)), in den Typ des Parameter Arrays.</span><span class="sxs-lookup"><span data-stu-id="6443e-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="6443e-835">In diesem Fall verhält sich das Parameter Array genau wie ein value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="6443e-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="6443e-836">Alternativ kann der Aufruf NULL oder mehr Argumente für das Parameter Array angeben, wobei jedes Argument ein Ausdruck ist, der implizit konvertierbar ist ([implizite Konvertierungen](conversions.md#implicit-conversions)), in den Elementtyp des Parameter Arrays.</span><span class="sxs-lookup"><span data-stu-id="6443e-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="6443e-837">In diesem Fall erstellt der Aufruf eine Instanz des Parameter Array Typs mit einer Länge, die der Anzahl der Argumente entspricht, initialisiert die Elemente der Array Instanz mit den angegebenen Argument Werten und verwendet die neu erstellte Array Instanz als die tatsächliche gestritten.</span><span class="sxs-lookup"><span data-stu-id="6443e-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="6443e-838">Mit der Ausnahme, dass eine Variable Anzahl von Argumenten in einem Aufruf zugelassen wird, entspricht ein Parameter Array exakt einem Wert Parameter ([value-Parameter](classes.md#value-parameters)) desselben Typs.</span><span class="sxs-lookup"><span data-stu-id="6443e-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="6443e-839">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-839">The example</span></span>
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
<span data-ttu-id="6443e-840">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="6443e-840">produces the output</span></span>
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="6443e-841">Beim ersten Aufruf von `F` wird das Array `a` einfach als value-Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="6443e-842">Der zweite Aufruf von `F` erstellt automatisch ein vier-Element `int[]` mit den angegebenen Element Werten und übergibt diese Array Instanz als Wert Parameter.</span><span class="sxs-lookup"><span data-stu-id="6443e-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="6443e-843">Ebenso erstellt der dritte Aufruf von `F` ein NULL-Element `int[]` und übergibt diese Instanz als value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="6443e-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="6443e-844">Der zweite und der dritte Aufruf entsprechen genau dem Schreiben:</span><span class="sxs-lookup"><span data-stu-id="6443e-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="6443e-845">Beim Durchführen der Überladungs Auflösung kann eine Methode mit einem Parameter Array entweder in der normalen Form oder in der erweiterten Form ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) anwendbar sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="6443e-846">Die erweiterte Form einer Methode ist nur verfügbar, wenn die normale Form der Methode nicht anwendbar ist und nur, wenn eine anwendbare Methode mit derselben Signatur wie das erweiterte Formular nicht bereits im selben Typ deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="6443e-847">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-847">The example</span></span>
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
<span data-ttu-id="6443e-848">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="6443e-848">produces the output</span></span>
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="6443e-849">Im Beispiel sind zwei der möglichen erweiterten Formen der-Methode mit einem Parameter Array bereits als reguläre Methoden in der-Klasse enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="6443e-850">Diese erweiterten Formulare werden daher bei der Überladungs Auflösung nicht berücksichtigt, und der erste und dritte Methodenaufruf wählen daher die regulären Methoden aus.</span><span class="sxs-lookup"><span data-stu-id="6443e-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="6443e-851">Wenn eine Klasse eine Methode mit einem Parameter Array deklariert, ist es nicht ungewöhnlich, dass auch einige der erweiterten Formulare als reguläre Methoden enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="6443e-852">Auf diese Weise ist es möglich, die Zuordnung einer Array Instanz zu vermeiden, die auftritt, wenn eine erweiterte Form einer Methode mit einem Parameter Array aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="6443e-853">Wenn der Typ eines Parameter Arrays ist `object[]`, entsteht eine potenzielle Mehrdeutigkeit zwischen der normalen Form der-Methode und dem aufgewendet-Formular für einen einzelnen `object` Parameter.</span><span class="sxs-lookup"><span data-stu-id="6443e-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="6443e-854">Der Grund für die Mehrdeutigkeit besteht darin `object[]` , dass eine selbst implizit in `object`den Typ konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="6443e-855">Die Mehrdeutigkeit stellt jedoch kein Problem dar, da Sie durch Einfügen einer Umwandlung bei Bedarf aufgelöst werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="6443e-856">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-856">The example</span></span>
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
<span data-ttu-id="6443e-857">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="6443e-857">produces the output</span></span>
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="6443e-858">Im ersten und letzten Aufruf von `F`ist die normale Form von `F` anwendbar, da eine implizite Konvertierung vom Argumenttyp in den Parametertyp besteht (beide sind vom Typ `object[]`).</span><span class="sxs-lookup"><span data-stu-id="6443e-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="6443e-859">Folglich wählt die Überladungs Auflösung die normale Form `F`von aus, und das-Argument wird als regulärer Wert Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="6443e-860">Im zweiten und dritten Aufruf ist die normale Form von `F` nicht anwendbar, weil keine implizite Konvertierung vom Argumenttyp zum Parametertyp vorhanden ist (der Typ `object` kann nicht implizit in den Typ `object[]`konvertiert werden).</span><span class="sxs-lookup"><span data-stu-id="6443e-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="6443e-861">Allerdings ist die erweiterte Form `F` von anwendbar, sodass Sie durch Überladungs Auflösung ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="6443e-862">Folglich wird ein One-Element `object[]` durch den Aufruf erstellt, und das einzelne Element des Arrays wird mit dem angegebenen Argument Wert initialisiert (der selbst ein Verweis auf ein `object[]`ist).</span><span class="sxs-lookup"><span data-stu-id="6443e-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="6443e-863">Statische Methoden und Instanzmethoden</span><span class="sxs-lookup"><span data-stu-id="6443e-863">Static and instance methods</span></span>

<span data-ttu-id="6443e-864">Wenn eine Methoden Deklaration einen `static` -Modifizierer enthält, wird diese Methode als statische Methode bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="6443e-865">Wenn kein `static` Modifizierer vorhanden ist, wird die Methode als Instanzmethode bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="6443e-866">Eine statische Methode funktioniert nicht für eine bestimmte Instanz, und Sie ist ein Kompilierzeitfehler, auf den `this` in einer statischen Methode verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="6443e-867">Eine Instanzmethode arbeitet auf einer bestimmten Instanz einer Klasse, und auf diese Instanz kann als `this` ([dieser Zugriff](expressions.md#this-access)) zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="6443e-868">Wenn auf eine Methode in einem *member_access* -Element ([Member Access](expressions.md#member-access)) der Form `E.M` verwiesen wird, wenn `M` eine statische Methode ist, muss `E` einen Typ angeben, der `M` enthält, und wenn `M` eine Instanzmethode ist, muss `E` eine Instanz eines Typs bezeichnen. mit `M`.</span><span class="sxs-lookup"><span data-stu-id="6443e-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6443e-869">Die Unterschiede zwischen statischen und Instanzmembern werden in [statischen und Instanzmembern](classes.md#static-and-instance-members)ausführlicher erläutert.</span><span class="sxs-lookup"><span data-stu-id="6443e-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="6443e-870">Virtuelle Methoden</span><span class="sxs-lookup"><span data-stu-id="6443e-870">Virtual methods</span></span>

<span data-ttu-id="6443e-871">Wenn eine Instanzmethodendeklaration einen `virtual` -Modifizierer enthält, wird diese Methode als virtuelle Methode bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="6443e-872">Wenn kein `virtual` Modifizierer vorhanden ist, wird die Methode als nicht virtuelle Methode bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="6443e-873">Die Implementierung einer nicht virtuellen Methode ist unveränderlich: Die-Implementierung ist identisch, unabhängig davon, ob die-Methode für eine Instanz der-Klasse, in der Sie deklariert ist, oder für eine Instanz einer abgeleiteten Klasse aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="6443e-874">Im Gegensatz dazu kann die Implementierung einer virtuellen Methode durch abgeleitete Klassen abgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="6443e-875">Der Prozess der Ersetzung der Implementierung einer geerbten virtuellen Methode ***wird als über*** Schreiben dieser Methode bezeichnet ([Überschreibungs Methoden](classes.md#override-methods)).</span><span class="sxs-lookup"><span data-stu-id="6443e-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="6443e-876">Beim Aufruf einer virtuellen Methode bestimmt der ***Lauf Zeittyp*** der Instanz, für die dieser Aufruf erfolgt, die tatsächliche Methoden Implementierung, die aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="6443e-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="6443e-877">Bei einem Aufruf der nicht virtuellen Methode ist der ***Kompilier Zeittyp*** der Instanz der bestimmende Faktor.</span><span class="sxs-lookup"><span data-stu-id="6443e-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="6443e-878">Genauer gesagt, wenn eine Methode mit dem `N` Namen mit einer Argumentliste `A` für eine Instanz mit einem Kompilier Zeittyp `C` und einem Lauf Zeittyp `R` aufgerufen wird `R` (wobei `C` entweder oder eine von abgeleitete Klasse ist). von `C`) wird der Aufruf wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="6443e-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="6443e-879">Zuerst wird die Überladungs Auflösung auf `C`, `N`und `A`angewendet, um eine bestimmte Methode `M` aus dem Satz von Methoden auszuwählen, der in deklariert und `C`von geerbt wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="6443e-880">Dies wird unter [Methodenaufrufe](expressions.md#method-invocations)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="6443e-881">Wenn `M` dann eine nicht virtuelle Methode ist, `M` wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="6443e-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="6443e-882">Andernfalls ist eine virtuelle Methode, und die am meisten abgeleitete Implementierung `M` von in Bezug `R` auf wird aufgerufen. `M`</span><span class="sxs-lookup"><span data-stu-id="6443e-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="6443e-883">Für jede virtuelle Methode, die in der Klasse deklariert oder von einer Klasse geerbt wurde, gibt es in Bezug auf diese Klasse eine ***am meisten abgeleitete Implementierung*** der-Methode.</span><span class="sxs-lookup"><span data-stu-id="6443e-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="6443e-884">Die am weitesten abgeleitete Implementierung einer virtuellen `M` Methode in Bezug auf eine `R` Klasse wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="6443e-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="6443e-885">Wenn `R` die Introducing `virtual` -Deklaration `M`von enthält, dann handelt es sich hierbei um `M`die am meisten abgeleitete Implementierung von.</span><span class="sxs-lookup"><span data-stu-id="6443e-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="6443e-886">Wenn `R` andernfalls einen `override` von `M`enthält, ist dies die am meisten abgeleitete Implementierung von `M`.</span><span class="sxs-lookup"><span data-stu-id="6443e-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="6443e-887">Andernfalls ist die am meisten abgeleitete `M` Implementierung von in `R` Bezug auf die gleiche wie die am meisten abgeleitete Implementierung von `M` in Bezug auf die direkte `R`Basisklasse von.</span><span class="sxs-lookup"><span data-stu-id="6443e-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="6443e-888">Im folgenden Beispiel werden die Unterschiede zwischen virtuellen und nicht virtuellen Methoden veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="6443e-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
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

<span data-ttu-id="6443e-889">Im Beispiel wird `A` eine nicht virtuelle Methode `F` und eine virtuelle Methode `G`eingeführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="6443e-890">Die- `B` Klasse führt eine neue nicht virtuelle Methode `F`ein, wodurch die geerbte `F`-Methode ausgeblendet wird, und über `G`schreibt auch die geerbte-Methode.</span><span class="sxs-lookup"><span data-stu-id="6443e-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="6443e-891">Das Beispiel erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6443e-891">The example produces the output:</span></span>
```console
A.F
B.F
B.G
B.G
```

<span data-ttu-id="6443e-892">Beachten Sie `B.G`, dass `a.G()` die-Anweisung `A.G`aufruft, nicht.</span><span class="sxs-lookup"><span data-stu-id="6443e-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="6443e-893">Dies liegt daran, dass der Lauf Zeittyp der-Instanz ( `B`d. h.), nicht der Kompilier Zeittyp der `A`-Instanz (d. h.), die tatsächliche Methoden Implementierung bestimmt, die aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="6443e-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="6443e-894">Da Methoden vererbte Methoden ausblenden dürfen, kann eine Klasse mehrere virtuelle Methoden mit der gleichen Signatur enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="6443e-895">Dies stellt kein mehrdeutigkeitsproblem dar, da alle außer der am weitesten abgeleiteten Methode ausgeblendet sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="6443e-896">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-896">In the example</span></span>
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
<span data-ttu-id="6443e-897">die `C` Klassen `D` und enthalten zwei virtuelle Methoden mit der gleichen Signatur: Der von `A` eingeführte und der von `C`eingeführte.</span><span class="sxs-lookup"><span data-stu-id="6443e-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="6443e-898">Die durch `C` eingeführte Methode blendet die von geerbte Methode aus `A`.</span><span class="sxs-lookup"><span data-stu-id="6443e-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="6443e-899">Folglich überschreibt die Überschreibungs `D` Deklaration in die von `C`eingeführte Methode, und `D` es ist nicht möglich, die durch `A`eingeführte Methode zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="6443e-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="6443e-900">Das Beispiel erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6443e-900">The example produces the output:</span></span>
```console
B.F
B.F
D.F
D.F
```

<span data-ttu-id="6443e-901">Beachten Sie, dass es möglich ist, die verborgene virtuelle Methode aufzurufen, indem `D` Sie auf eine Instanz von über einen weniger abgeleiteten Typ zugreifen, in dem die Methode nicht ausgeblendet ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="6443e-902">Überschreibungs Methoden</span><span class="sxs-lookup"><span data-stu-id="6443e-902">Override methods</span></span>

<span data-ttu-id="6443e-903">Wenn eine Instanzmethodendeklaration einen `override` -Modifizierer enthält, wird die-Methode als ***Überschreibungs Methode***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="6443e-904">Eine Überschreibungs Methode überschreibt eine geerbte virtuelle Methode mit derselben Signatur.</span><span class="sxs-lookup"><span data-stu-id="6443e-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="6443e-905">Während eine Deklaration einer virtuellen Methode eine neue Methode einführt, spezialisiert eine Deklaration einer überschriebenen Methode eine vorhandene geerbte virtuelle Methode, indem eine neue Implementierung dieser Methode bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="6443e-906">Die Methode, die durch eine `override` -Deklaration überschrieben wird, wird als die ***überschriebene Basis Methode***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="6443e-907">Bei einer in einer `M` Klasse `C`deklarierten Überschreibungs Methode wird die überschriebene Basis Methode bestimmt, indem jeder Basis Klassentyp von `C`untersucht wird, beginnend mit `C` dem direkten Basis Klassentyp von und mit jeder nachfolgenden direkter Basis Klassentyp, bis in einem gegebenen Basis Klassentyp mindestens eine barrierefreie Methode gefunden wird, die die gleiche Signatur `M` wie nach der Ersetzung von Typargumenten aufweist.</span><span class="sxs-lookup"><span data-stu-id="6443e-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="6443e-908">Zum Ermitteln der überschriebenen Basis Methode wird eine Methode `public`als verfügbar `protected` `protected internal`betrachtet, wenn dies der Fall ist, wenn dies der Fall ist, oder wenn Sie in demselben `internal` Programm wie `C`deklariert und deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="6443e-909">Ein Kompilierzeitfehler tritt auf, es sei denn, für eine Überschreibungs Deklaration gilt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="6443e-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="6443e-910">Eine überschriebene Basis Methode kann wie oben beschrieben gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="6443e-911">Es gibt genau eine solche überschriebene Basis Methode.</span><span class="sxs-lookup"><span data-stu-id="6443e-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="6443e-912">Diese Einschränkung hat nur Auswirkungen, wenn der Basis Klassentyp ein konstruierter Typ ist, bei dem durch die Ersetzung von Typargumenten die Signatur zweier Methoden identisch ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="6443e-913">Die überschriebene Basis Methode ist eine virtuelle Methode, eine abstrakte Methode oder eine Überschreibungs Methode.</span><span class="sxs-lookup"><span data-stu-id="6443e-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="6443e-914">Anders ausgedrückt: die überschriebene Basis Methode kann nicht statisch oder nicht virtuell sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="6443e-915">Die überschriebene Basis Methode ist keine versiegelte Methode.</span><span class="sxs-lookup"><span data-stu-id="6443e-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="6443e-916">Die Überschreibungs Methode und die überschriebene Basis Methode haben denselben Rückgabetyp.</span><span class="sxs-lookup"><span data-stu-id="6443e-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="6443e-917">Die Überschreibungs Deklaration und die überschriebene Basis Methode haben dieselbe deklarierte Zugriffsmethode.</span><span class="sxs-lookup"><span data-stu-id="6443e-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="6443e-918">Anders ausgedrückt: eine Überschreibungs Deklaration kann nicht den Zugriff auf die virtuelle Methode ändern.</span><span class="sxs-lookup"><span data-stu-id="6443e-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="6443e-919">Wenn die überschriebene Basis Methode jedoch intern geschützt ist und in einer anderen Assembly als der Assembly deklariert ist, die die Überschreibungs Methode enthält, muss die deklarierte Barrierefreiheit der Überschreibungs Methode geschützt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="6443e-920">Die Überschreibungs Deklaration gibt keine Type-Parameter-Einschränkungs Klauseln an.</span><span class="sxs-lookup"><span data-stu-id="6443e-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="6443e-921">Stattdessen werden die Einschränkungen von der überschriebenen Basis Methode geerbt.</span><span class="sxs-lookup"><span data-stu-id="6443e-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="6443e-922">Beachten Sie, dass Einschränkungen, die Typparameter in der überschriebenen Methode sind, durch Typargumente in der geerbten Einschränkung ersetzt werden können.</span><span class="sxs-lookup"><span data-stu-id="6443e-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="6443e-923">Dies kann zu Einschränkungen führen, die nicht zulässig sind, wenn Sie explizit angegeben werden, z. b. Werttypen oder versiegelte Typen.</span><span class="sxs-lookup"><span data-stu-id="6443e-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="6443e-924">Im folgenden Beispiel wird veranschaulicht, wie die über schreibenden Regeln für generische Klassen funktionieren:</span><span class="sxs-lookup"><span data-stu-id="6443e-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
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

<span data-ttu-id="6443e-925">Eine Überschreibungs Deklaration kann mithilfe eines *base_access* ([Basis Zugriffs](expressions.md#base-access)) auf die überschriebene Basis Methode zugreifen.</span><span class="sxs-lookup"><span data-stu-id="6443e-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="6443e-926">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-926">In the example</span></span>
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
<span data-ttu-id="6443e-927">der `base.PrintFields()` Aufruf in `B` Ruft die `PrintFields` in `A`deklarierte Methode auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="6443e-928">Ein *base_access* deaktiviert den virtuellen Aufruf Mechanismus und behandelt die Basis Methode einfach als nicht virtuelle Methode.</span><span class="sxs-lookup"><span data-stu-id="6443e-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="6443e-929">Wenn `B` der Aufruf von geschrieben `((A)this).PrintFields()`wurde, würde er die `PrintFields` in `B`deklarierte Methode rekursiv aufrufen, nicht die in `A`deklarierte Methode `PrintFields` , da der virtuelle und der Lauf Zeittyp von ist.`((A)this)` ist .`B`</span><span class="sxs-lookup"><span data-stu-id="6443e-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="6443e-930">Nur durch das einschließen `override` eines-Modifizierers kann eine Methode eine andere Methode überschreiben.</span><span class="sxs-lookup"><span data-stu-id="6443e-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="6443e-931">In allen anderen Fällen verbirgt eine Methode, die dieselbe Signatur wie eine geerbte Methode hat, einfach die geerbte Methode.</span><span class="sxs-lookup"><span data-stu-id="6443e-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="6443e-932">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-932">In the example</span></span>
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
<span data-ttu-id="6443e-933">die `F` -Methode `B` in `override` enthält keinen-Modifizierer und überschreibt daher nicht `F` die- `A`Methode in.</span><span class="sxs-lookup"><span data-stu-id="6443e-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="6443e-934">Stattdessen verbirgt die `F` -Methode `B` in die-Methode `A`in, und es wird eine Warnung ausgegeben, da die Deklaration `new` keinen Modifizierer enthält.</span><span class="sxs-lookup"><span data-stu-id="6443e-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="6443e-935">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-935">In the example</span></span>
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
<span data-ttu-id="6443e-936">die `F` -Methode `B` in blendet `F` die von geerbte virtuelle Methode aus `A`.</span><span class="sxs-lookup"><span data-stu-id="6443e-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="6443e-937">Da das neue `F` in `B` privaten Zugriff hat, umfasst sein Bereich nur den Klassen Text von `B` und wird nicht auf `C`erweitert.</span><span class="sxs-lookup"><span data-stu-id="6443e-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="6443e-938">Daher ist es zulässig, `F` die `C` Deklaration von in zu `F` überschreiben `A`, das von geerbt wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="6443e-939">Versiegelte Methoden</span><span class="sxs-lookup"><span data-stu-id="6443e-939">Sealed methods</span></span>

<span data-ttu-id="6443e-940">Wenn eine Instanzmethodendeklaration einen `sealed` -Modifizierer enthält, wird diese Methode als ***versiegelte Methode***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="6443e-941">Wenn eine Instanzmethodendeklaration den `sealed` -Modifizierer enthält, muss Sie auch den `override` -Modifizierer einschließen.</span><span class="sxs-lookup"><span data-stu-id="6443e-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="6443e-942">Die Verwendung des `sealed` -Modifizierers verhindert, dass eine abgeleitete Klasse die-Methode weiter überschreibt.</span><span class="sxs-lookup"><span data-stu-id="6443e-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="6443e-943">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-943">In the example</span></span>
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
<span data-ttu-id="6443e-944">die- `B` Klasse stellt zwei Überschreibungs Methoden `F` bereit: eine Methode `sealed` , die über den `G` -Modifizierer und eine-Methode verfügt, die nicht.</span><span class="sxs-lookup"><span data-stu-id="6443e-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="6443e-945">`B`die Verwendung des versiegelten `modifier` verhindert `C` , dass weiter `F`überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="6443e-946">Abstrakte Methoden</span><span class="sxs-lookup"><span data-stu-id="6443e-946">Abstract methods</span></span>

<span data-ttu-id="6443e-947">Wenn eine Instanzmethodendeklaration einen `abstract` -Modifizierer enthält, wird diese Methode als ***abstrakte Methode***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="6443e-948">Obwohl eine abstrakte Methode implizit auch eine virtuelle Methode ist, kann Sie nicht den-Modifizierer `virtual`aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="6443e-949">Eine abstrakte Methoden Deklaration führt eine neue virtuelle Methode ein, stellt jedoch keine Implementierung dieser Methode bereit.</span><span class="sxs-lookup"><span data-stu-id="6443e-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="6443e-950">Stattdessen sind nicht abstrakte abgeleitete Klassen erforderlich, um eine eigene Implementierung bereitzustellen, indem diese Methode überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="6443e-951">Da eine abstrakte Methode keine tatsächliche Implementierung bereitstellt, besteht das *method_body* einer abstrakten Methode einfach aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="6443e-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="6443e-952">Abstrakte Methoden Deklarationen sind nur in abstrakten Klassen zulässig ([abstrakte Klassen](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="6443e-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="6443e-953">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-953">In the example</span></span>
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
<span data-ttu-id="6443e-954">die `Shape` -Klasse definiert das abstrakte Konzept eines geometrischen Shape-Objekts, das sich selbst zeichnen kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="6443e-955">Die `Paint` Methode ist abstrakt, da keine sinnvolle Standard Implementierung vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="6443e-956">Die `Ellipse` Klassen `Box` und sind konkrete `Shape` Implementierungen.</span><span class="sxs-lookup"><span data-stu-id="6443e-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="6443e-957">Da diese Klassen nicht abstrakt sind, müssen Sie die `Paint` -Methode überschreiben und eine tatsächliche Implementierung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="6443e-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="6443e-958">Es handelt sich um einen Kompilierzeitfehler für ein *base_access* ([Base Access](expressions.md#base-access)), der auf eine abstrakte Methode verweist.</span><span class="sxs-lookup"><span data-stu-id="6443e-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="6443e-959">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-959">In the example</span></span>
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
<span data-ttu-id="6443e-960">für den `base.F()` Aufruf wird ein Kompilierzeitfehler gemeldet, weil er auf eine abstrakte Methode verweist.</span><span class="sxs-lookup"><span data-stu-id="6443e-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="6443e-961">Eine abstrakte Methoden Deklaration darf eine virtuelle Methode überschreiben.</span><span class="sxs-lookup"><span data-stu-id="6443e-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="6443e-962">Dadurch kann eine abstrakte Klasse die erneute Implementierung der Methode in abgeleiteten Klassen erzwingen und die ursprüngliche Implementierung der Methode nicht verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="6443e-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="6443e-963">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-963">In the example</span></span>
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
<span data-ttu-id="6443e-964">die `A` -Klasse deklariert eine virtuelle Methode `B` , die-Klasse überschreibt diese Methode mit einer abstrakten `C` -Methode, und die-Klasse überschreibt die abstrakte-Methode, um eine eigene Implementierung bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="6443e-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="6443e-965">Externe Methoden</span><span class="sxs-lookup"><span data-stu-id="6443e-965">External methods</span></span>

<span data-ttu-id="6443e-966">Wenn eine Methoden Deklaration einen `extern` -Modifizierer enthält, wird diese Methode als ***externe Methode***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="6443e-967">Externe Methoden werden extern implementiert, in der Regel mit einer anderen C#Sprache als.</span><span class="sxs-lookup"><span data-stu-id="6443e-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="6443e-968">Da eine externe Methoden Deklaration keine tatsächliche Implementierung bereitstellt, besteht das *method_body* einer externen Methode einfach aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="6443e-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="6443e-969">Eine externe Methode darf nicht generisch sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-969">An external method may not be generic.</span></span>

<span data-ttu-id="6443e-970">Der `extern` -Modifizierer wird in der Regel in `DllImport` Verbindung mit einem-Attribut ([Interoperation mit com-und Win32-Komponenten](attributes.md#interoperation-with-com-and-win32-components)) verwendet, sodass externe Methoden durch DLLs (Dynamic Link Libraries) implementiert werden können.</span><span class="sxs-lookup"><span data-stu-id="6443e-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="6443e-971">Die Ausführungsumgebung unterstützt möglicherweise andere Mechanismen, bei denen Implementierungen externer Methoden bereitgestellt werden können.</span><span class="sxs-lookup"><span data-stu-id="6443e-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="6443e-972">Wenn eine externe Methode ein `DllImport` -Attribut enthält, muss die Methoden Deklaration auch einen `static` -Modifizierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="6443e-973">Dieses Beispiel veranschaulicht die Verwendung des `extern` -Modifizierers und des `DllImport` -Attributs:</span><span class="sxs-lookup"><span data-stu-id="6443e-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
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

### <a name="partial-methods-recap"></a><span data-ttu-id="6443e-974">Partielle Methoden (Recap)</span><span class="sxs-lookup"><span data-stu-id="6443e-974">Partial methods (recap)</span></span>

<span data-ttu-id="6443e-975">Wenn eine Methoden Deklaration einen `partial` -Modifizierer enthält, wird diese Methode als ***partielle Methode***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="6443e-976">Partielle Methoden können nur als Member von partiellen Typen ([partielle Typen](classes.md#partial-types)) deklariert werden und unterliegen einer Reihe von Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="6443e-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="6443e-977">Partielle Methoden werden weiter unten in [partiellen Methoden](classes.md#partial-methods)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="6443e-978">Erweiterungsmethoden</span><span class="sxs-lookup"><span data-stu-id="6443e-978">Extension methods</span></span>

<span data-ttu-id="6443e-979">Wenn der erste Parameter einer Methode den `this` -Modifizierer enthält, wird diese Methode als ***Erweiterungsmethode***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="6443e-980">Erweiterungs Methoden können nur in nicht generischen, nicht in der Tabelle statischen Klassen deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="6443e-981">Der erste Parameter einer Erweiterungsmethode kann keine anderen Modifizierer als `this`aufweisen, und der Parametertyp darf kein Zeigertyp sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="6443e-982">Im folgenden finden Sie ein Beispiel für eine statische Klasse, die zwei Erweiterungs Methoden deklariert:</span><span class="sxs-lookup"><span data-stu-id="6443e-982">The following is an example of a static class that declares two extension methods:</span></span>
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

<span data-ttu-id="6443e-983">Eine Erweiterungsmethode ist eine reguläre statische Methode.</span><span class="sxs-lookup"><span data-stu-id="6443e-983">An extension method is a regular static method.</span></span> <span data-ttu-id="6443e-984">Außerdem kann eine Erweiterungsmethode mit instanzmethodenaufruf-Syntax ([Erweiterungs Methodenaufrufe](expressions.md#extension-method-invocations)) mithilfe des Empfänger Ausdrucks als erstes Argument aufgerufen werden, wenn sich Ihre einschließende statische Klasse im Gültigkeitsbereich befindet.</span><span class="sxs-lookup"><span data-stu-id="6443e-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="6443e-985">Im folgenden Programm werden die oben deklarierten Erweiterungs Methoden verwendet:</span><span class="sxs-lookup"><span data-stu-id="6443e-985">The following program uses the extension methods declared above:</span></span>
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

<span data-ttu-id="6443e-986">Die `Slice` -Methode ist `string[]`in verfügbar, und die `ToInt32` -Methode ist in `string`verfügbar, da Sie als Erweiterungs Methoden deklariert wurden.</span><span class="sxs-lookup"><span data-stu-id="6443e-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="6443e-987">Die Bedeutung des Programms ist mit den folgenden statischen Methoden aufrufen identisch:</span><span class="sxs-lookup"><span data-stu-id="6443e-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
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

### <a name="method-body"></a><span data-ttu-id="6443e-988">Methoden Text</span><span class="sxs-lookup"><span data-stu-id="6443e-988">Method body</span></span>

<span data-ttu-id="6443e-989">Der *method_body* einer Methoden Deklaration besteht aus einem Block Körper, einem Ausdrucks Körper oder einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="6443e-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="6443e-990">Der ***Ergebnistyp*** einer Methode ist `void` , wenn der Rückgabetyp ist `void`, oder wenn die Methode Async ist und der `System.Threading.Tasks.Task`Rückgabetyp ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="6443e-991">Andernfalls ist der Ergebnistyp einer nicht Async-Methode der Rückgabetyp, und der Ergebnistyp einer Async-Methode mit `System.Threading.Tasks.Task<T>` dem `T`Rückgabetyp ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="6443e-992">Wenn eine Methode über einen `void` Ergebnistyp und einen Block Text `return` verfügt, ist es nicht zulässig, Anweisungen ([die Return-Anweisung](statements.md#the-return-statement)) im-Block anzugeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="6443e-993">Wenn die Ausführung des Blocks einer void-Methode normal abgeschlossen ist (d. h., der Ablauf der Steuerelemente wird vom Ende des Methoden Texts entfernt), kehrt diese Methode einfach an den aktuellen Aufrufer zurück.</span><span class="sxs-lookup"><span data-stu-id="6443e-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="6443e-994">Wenn eine Methode über ein `void`-Ergebnis und einen Ausdrucks Text verfügt, muss der Ausdruck `E` ein *statement_expression*sein, und der Text entspricht genau einem Block Text in der Form `{ E; }`.</span><span class="sxs-lookup"><span data-stu-id="6443e-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="6443e-995">Wenn eine Methode einen nicht leeren Ergebnistyp und einen Block Text hat, muss `return` jede Anweisung im Block einen Ausdruck angeben, der implizit in den Ergebnistyp konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="6443e-996">Der Endpunkt eines Block Texts einer Wert Rückgabe Methode darf nicht erreichbar sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="6443e-997">Anders ausgedrückt: in einer Rückgabe Methode mit einem Block Text ist das Steuerelement nicht berechtigt, das Ende des Methoden Texts zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="6443e-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="6443e-998">Wenn eine Methode einen nicht leeren Ergebnistyp und einen Ausdrucks Körper aufweist, muss der Ausdruck implizit in den Ergebnistyp konvertiert werden, und der Text entspricht genau dem Block Text des Formulars `{ return E; }`.</span><span class="sxs-lookup"><span data-stu-id="6443e-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="6443e-999">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-999">In the example</span></span>
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
<span data-ttu-id="6443e-1000">die Wert Rückgabe `F` Methode führt zu einem Kompilierzeitfehler, da das Steuerelement vom Ende des Methoden Texts entfernt werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="6443e-1001">Die `G` - `H` Methode und die-Methode sind korrekt, da alle möglichen Ausführungs Pfade in einer Return-Anweisung enden, die einen Rückgabewert angibt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="6443e-1002">Die `I` Methode ist korrekt, da Ihr Text einem Anweisungsblock mit nur einer einzigen return-Anweisung entspricht.</span><span class="sxs-lookup"><span data-stu-id="6443e-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="6443e-1003">Methodenüberladung</span><span class="sxs-lookup"><span data-stu-id="6443e-1003">Method overloading</span></span>

<span data-ttu-id="6443e-1004">Die Regeln zur Auflösung von Methoden Überladungen werden unter [Typrückschluss](expressions.md#type-inference)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="6443e-1005">Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="6443e-1005">Properties</span></span>

<span data-ttu-id="6443e-1006">Eine ***Eigenschaft*** ist ein Member, der den Zugriff auf ein Merkmal eines Objekts oder einer Klasse ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="6443e-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="6443e-1007">Beispiele für Eigenschaften sind die Länge einer Zeichenfolge, die Größe einer Schriftart, die Beschriftung eines Fensters, der Name eines Kunden usw.</span><span class="sxs-lookup"><span data-stu-id="6443e-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="6443e-1008">Eigenschaften sind eine natürliche Erweiterung von Feldern – beide sind benannte Member mit zugeordneten Typen, und die Syntax für den Zugriff auf Felder und Eigenschaften ist identisch.</span><span class="sxs-lookup"><span data-stu-id="6443e-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="6443e-1009">Im Gegensatz zu Feldern bezeichnen Eigenschaften jedoch keine Speicherorte.</span><span class="sxs-lookup"><span data-stu-id="6443e-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="6443e-1010">Stattdessen verfügen Eigenschaften über ***Accessors*** zum Angeben der Anweisungen, die beim Lesen oder Schreiben ihrer Werte ausgeführt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="6443e-1011">Eigenschaften bieten somit einen Mechanismus zum Zuordnen von Aktionen zum Lesen und Schreiben der Attribute eines Objekts. Außerdem können solche Attribute berechnet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="6443e-1012">Eigenschaften werden mit *property_declaration*s deklariert:</span><span class="sxs-lookup"><span data-stu-id="6443e-1012">Properties are declared using *property_declaration*s:</span></span>

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

<span data-ttu-id="6443e-1013">Ein *property_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)), den `new` ([den neuen Modifizierer](classes.md#the-new-modifier)), `static` ([statisch und Instanz) enthalten. Methoden](classes.md#static-and-instance-methods)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Überschreibungs Methoden](classes.md#override-methods)), `sealed` ([versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakte Methoden](classes.md#abstract-methods)) und `extern`-Modifizierer ([Externe Methoden](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6443e-1014">Eigenschafts Deklarationen unterliegen den gleichen Regeln wie Methoden Deklarationen ([Methoden](classes.md#methods)) in Bezug auf gültige Kombinationen von modifiziererobjekten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="6443e-1015">Der *Typ* einer Eigenschafts Deklaration gibt den Typ der Eigenschaft an, die von der Deklaration eingeführt wurde, und *MEMBER_NAME* gibt den Namen der Eigenschaft an.</span><span class="sxs-lookup"><span data-stu-id="6443e-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="6443e-1016">Wenn die Eigenschaft keine explizite Schnittstellenmember-Implementierung ist, ist *MEMBER_NAME* einfach ein *Bezeichner*.</span><span class="sxs-lookup"><span data-stu-id="6443e-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="6443e-1017">Bei einer expliziten Schnittstellenmember-Implementierung ([explizite Schnittstellenmember-Implementierungen](interfaces.md#explicit-interface-member-implementations)) besteht das *MEMBER_NAME* aus einem *INTERFACE_TYPE* , gefolgt von einem "`.`" und einem *Bezeichner*.</span><span class="sxs-lookup"><span data-stu-id="6443e-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="6443e-1018">Der *Typ* einer Eigenschaft muss mindestens so zugänglich sein wie die Eigenschaft selbst (Barrierefreiheits[Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6443e-1019">Ein *property_body* kann entweder aus einem ***Accessor-Text*** oder einem ***Ausdrucks Körper***bestehen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="6443e-1020">In einem Accessor-Text muss *accessor_declarations*, der in "`{`" und "`}`"-Token eingeschlossen werden muss, die Accessoren ([Accessoren](classes.md#accessors)) der Eigenschaft deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="6443e-1021">Die Accessoren geben die ausführbaren Anweisungen an, die dem Lesen und Schreiben der Eigenschaft zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="6443e-1022">Ein Ausdrucks Text, der `=>` aus gefolgt von einem *Ausdruck* `E` und einem Semikolon besteht, entspricht exakt dem `{ get { return E; } }`Anweisungs Text und kann daher nur zum Angeben von nur-Getter-Eigenschaften verwendet werden, bei denen das Ergebnis von der Getter wird durch einen einzelnen Ausdruck angegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="6443e-1023">Ein *property_initializer* kann nur für eine automatisch implementierte Eigenschaft ([automatisch implementierte Eigenschaften](classes.md#automatically-implemented-properties)) angegeben werden und bewirkt die Initialisierung des zugrunde liegenden Felds dieser Eigenschaften mit dem Wert, der vom *Ausdruck angegeben wird.* .</span><span class="sxs-lookup"><span data-stu-id="6443e-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="6443e-1024">Obwohl die Syntax für den Zugriff auf eine Eigenschaft mit der Syntax für ein Feld identisch ist, wird eine Eigenschaft nicht als Variable klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="6443e-1025">Daher ist es nicht möglich, eine Eigenschaft als `ref` -oder `out` -Argument zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="6443e-1026">Wenn eine Eigenschafts Deklaration `extern` einen-Modifizierer enthält, wird die-Eigenschaft als ***externe Eigenschaft***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="6443e-1027">Da eine externe Eigenschaften Deklaration keine tatsächliche Implementierung bereitstellt, besteht jede Ihrer *accessor_declarations* aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="6443e-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="6443e-1028">Statische Eigenschaften und Instanzeigenschaften</span><span class="sxs-lookup"><span data-stu-id="6443e-1028">Static and instance properties</span></span>

<span data-ttu-id="6443e-1029">Wenn eine Eigenschafts Deklaration `static` einen-Modifizierer enthält, wird die-Eigenschaft als ***statische Eigenschaft***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="6443e-1030">Wenn kein `static` Modifizierer vorhanden ist, wird die Eigenschaft als ***Instanzeigenschaft***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="6443e-1031">Eine statische Eigenschaft ist keiner bestimmten Instanz zugeordnet, und es handelt sich um einen Kompilierzeitfehler, auf den `this` in den Accessoren einer statischen Eigenschaft verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="6443e-1032">Eine Instanzeigenschaft ist einer bestimmten Instanz einer Klasse zugeordnet, und auf diese Instanz kann als `this` ([dieser Zugriff](expressions.md#this-access)) in den Accessoren dieser Eigenschaft zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="6443e-1033">Wenn auf eine Eigenschaft in einem *member_access* ([Member Access](expressions.md#member-access)) der Form `E.M` verwiesen wird, wenn `M` eine statische Eigenschaft ist, muss `E` einen Typ angeben, der `M` enthält, und wenn `M` eine Instanzeigenschaft ist, muss E eine Instanz eines Typs angeben. mit `M`.</span><span class="sxs-lookup"><span data-stu-id="6443e-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6443e-1034">Die Unterschiede zwischen statischen und Instanzmembern werden in [statischen und Instanzmembern](classes.md#static-and-instance-members)ausführlicher erläutert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="6443e-1035">Accessoren</span><span class="sxs-lookup"><span data-stu-id="6443e-1035">Accessors</span></span>

<span data-ttu-id="6443e-1036">Der *accessor_declarations* -Wert einer Eigenschaft gibt die ausführbaren Anweisungen an, die dem Lesen und Schreiben dieser Eigenschaft zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

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

<span data-ttu-id="6443e-1037">Die Accessordeklarationen bestehen aus *get_accessor_declaration*, *set_accessor_declaration*oder beidem.</span><span class="sxs-lookup"><span data-stu-id="6443e-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="6443e-1038">Jede Accessor-Deklaration besteht aus dem Token `get` oder `set`, gefolgt von einem optionalen *accessor_modifier* und einem *accessor_body*.</span><span class="sxs-lookup"><span data-stu-id="6443e-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="6443e-1039">Die Verwendung von *accessor_modifier*s unterliegt den folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="6443e-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="6443e-1040">Ein *accessor_modifier* kann nicht in einer Schnittstelle oder in einer expliziten Schnittstellenmember-Implementierung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="6443e-1041">Für eine Eigenschaft oder einen Indexer, der über keinen `override`-Modifizierer verfügt, ist ein *accessor_modifier* nur zulässig, wenn die Eigenschaft oder der Indexer sowohl einen `get`-als auch einen `set`-Accessor hat und dann nur für einen dieser Accessoren zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="6443e-1042">Für eine Eigenschaft oder einen Indexer, die einen `override`-Modifizierer enthält, muss ein Accessor mit dem *accessor_modifier*(sofern vorhanden) der über schreibenden Zugriffsmethode identisch sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="6443e-1043">*Accessor_modifier* muss eine Barrierefreiheit deklarieren, die strikt restriktiver ist als der deklarierte Zugriff auf die Eigenschaft oder den Indexer selbst.</span><span class="sxs-lookup"><span data-stu-id="6443e-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="6443e-1044">Genauer gesagt:</span><span class="sxs-lookup"><span data-stu-id="6443e-1044">To be precise:</span></span>
   * <span data-ttu-id="6443e-1045">Wenn die Eigenschaft oder der Indexer über eine deklarierte Zugriffsmöglichkeit für `public` verfügt, kann *accessor_modifier* entweder `protected internal`, `internal`, `protected` oder `private` sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="6443e-1046">Wenn die Eigenschaft oder der Indexer über eine deklarierte Zugriffsmöglichkeit für `protected internal` verfügt, kann *accessor_modifier* entweder `internal`, `protected` oder `private` sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="6443e-1047">Wenn die Eigenschaft oder der Indexer eine deklarierte Zugriffsmöglichkeit für `internal` oder `protected` aufweist, muss *accessor_modifier* `private` sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="6443e-1048">Wenn die Eigenschaft oder der Indexer über eine deklarierte Zugriffsmöglichkeit für `private` verfügt, darf kein *accessor_modifier* verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="6443e-1049">Bei `abstract`-und `extern`-Eigenschaften ist die *accessor_body* für jeden angegebenen Accessor einfach ein Semikolon.</span><span class="sxs-lookup"><span data-stu-id="6443e-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="6443e-1050">Eine nicht abstrakte, nicht externe Eigenschaft kann jede *accessor_body* ein Semikolon sein. in diesem Fall handelt es sich um eine ***automatisch implementierte Eigenschaft*** ([automatisch implementierte Eigenschaften](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="6443e-1051">Eine automatisch implementierte Eigenschaft muss mindestens über einen get-Accessor verfügen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="6443e-1052">Für die Accessoren anderer nicht abstrakter, nicht externer Eigenschaften ist *accessor_body* ein- *Block* , der die auszuführenden Anweisungen angibt, wenn der entsprechende-Accessor aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="6443e-1053">Ein `get` -Accessor entspricht einer Parameter losen Methode mit einem Rückgabewert des Eigenschaftentyps.</span><span class="sxs-lookup"><span data-stu-id="6443e-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="6443e-1054">Wenn in einem Ausdruck `get` auf eine Eigenschaft verwiesen wird, wird der-Accessor der Eigenschaft aufgerufen, um den Wert der-Eigenschaft ([Werte von Ausdrücken](expressions.md#values-of-expressions)) zu berechnen, außer als Ziel einer Zuweisung.</span><span class="sxs-lookup"><span data-stu-id="6443e-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="6443e-1055">Der Text einer `get` -Zugriffsmethode muss den Regeln für die Rückgabe von Werten entsprechen, die im [Methoden Text](classes.md#method-body)beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6443e-1056">Insbesondere müssen alle `return` Anweisungen im Textkörper einer `get` -Zugriffsmethode einen Ausdruck angeben, der implizit in den Eigenschaftentyp konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="6443e-1057">Außerdem darf der Endpunkt einer `get` -Zugriffsmethode nicht erreichbar sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="6443e-1058">Ein `set` -Accessor entspricht einer Methode mit einem einzelnen Wert Parameter des Eigenschaftentyps `void` und einem Rückgabetyp.</span><span class="sxs-lookup"><span data-stu-id="6443e-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="6443e-1059">Der implizite Parameter einer `set` -Zugriffsmethode wird immer benannt. `value`</span><span class="sxs-lookup"><span data-stu-id="6443e-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="6443e-1060">Wenn auf eine Eigenschaft als Ziel einer Zuweisung ([Zuweisungs Operatoren](expressions.md#assignment-operators)) oder `++` als Operand von oder `--` ([postfix Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators), [Präfix Inkrement-und Dekrementoperatoren](expressions.md#prefix-increment-and-decrement-operators)) verwiesen wird, wird der der Accessor wird mit einem Argument aufgerufen (dessen Wert dem der rechten Seite der Zuweisung oder dem Operanden `++` des or `--` -Operators entspricht), der den neuen Wert ([einfache Zuweisung](expressions.md#simple-assignment)) bereitstellt. `set`</span><span class="sxs-lookup"><span data-stu-id="6443e-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="6443e-1061">Der Text einer `set` -Zugriffsmethode muss den Regeln für `void` Methoden entsprechen, die im [Methoden Text](classes.md#method-body)beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6443e-1062">Insbesondere können `return` Anweisungen `set` im Accessor-Text keinen Ausdruck angeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="6443e-1063">Da ein `set` -Accessor implizit einen Parameter mit `value`dem Namen aufweist, handelt es sich um einen Kompilierzeitfehler für eine lokale Variable `set` oder eine Konstante Deklaration in einem-Accessor, der über diesen Namen verfügt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="6443e-1064">Basierend auf dem vorhanden sein oder fehlen `get` der-und- `set` Accessoren wird eine Eigenschaft wie folgt klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="6443e-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="6443e-1065">Eine Eigenschaft, die sowohl einen `get` -Accessor als `set` auch einen-Accessor enthält, wird als ***Lese-/Schreib-Eigenschaft*** bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="6443e-1066">Eine Eigenschaft, die nur einen `get` -Accessor aufweist, wird als Schreib ***geschützte Eigenschaft bezeichnet*** .</span><span class="sxs-lookup"><span data-stu-id="6443e-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="6443e-1067">Es handelt sich um einen Kompilierzeitfehler für eine schreibgeschützte Eigenschaft, die das Ziel einer Zuweisung ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="6443e-1068">Eine Eigenschaft, die nur einen `set` -Accessor aufweist, wird als ***Schreib*** geschützte Eigenschaft bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="6443e-1069">Außer als Ziel einer Zuweisung ist es ein Kompilierzeitfehler, der auf eine schreibgeschützte Eigenschaft in einem Ausdruck verweist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="6443e-1070">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1070">In the example</span></span>
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
<span data-ttu-id="6443e-1071">Das `Button` -Steuerelement deklariert `Caption` eine öffentliche Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6443e-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="6443e-1072">Der `get` -Accessor `Caption` der-Eigenschaft gibt die Zeichenfolge zurück, `caption` die im privaten Feld gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="6443e-1073">Der `set` -Accessor überprüft, ob sich der neue Wert vom aktuellen Wert unterscheidet. wenn dies der Fall ist, wird der neue Wert gespeichert und das Steuerelement neu gezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="6443e-1074">Eigenschaften folgen häufig dem oben gezeigten Muster: Der `get` -Accessor gibt einfach einen in einem privaten Feld gespeicherten Wert zurück, `set` und der-Accessor ändert dieses private Feld und führt dann alle zusätzlichen Aktionen aus, die erforderlich sind, um den Status des Objekts vollständig zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="6443e-1075">Bei der `Button` obigen Klasse ist Folgendes ein Beispiel für die `Caption` Verwendung der-Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="6443e-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="6443e-1076">Hier wird der `set` -Accessor aufgerufen, indem der-Eigenschaft ein Wert zugewiesen wird, `get` und der-Accessor wird aufgerufen, indem auf die-Eigenschaft in einem Ausdruck verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="6443e-1077">Die `get` - `set` und-Accessoren einer Eigenschaft sind keine unterschiedlichen Member, und es ist nicht möglich, die Accessoren einer Eigenschaft separat zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="6443e-1078">Daher ist es nicht möglich, dass die beiden Accessoren einer Eigenschaft mit Lese-/Schreibzugriff über unterschiedliche Zugriffsberechtigungen verfügen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="6443e-1079">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1079">The example</span></span>
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
<span data-ttu-id="6443e-1080">deklariert keine einzige Lese-/Schreibeigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6443e-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="6443e-1081">Stattdessen werden zwei Eigenschaften mit demselben Namen deklariert, ein Schreib geschützter und ein Schreib geschützter.</span><span class="sxs-lookup"><span data-stu-id="6443e-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="6443e-1082">Da zwei Member, die in derselben Klasse deklariert werden, nicht denselben Namen haben können, bewirkt das Beispiel, dass ein Kompilierzeitfehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="6443e-1083">Wenn eine abgeleitete Klasse eine Eigenschaft mit demselben Namen wie eine geerbte Eigenschaft deklariert, verbirgt die abgeleitete Eigenschaft die geerbte Eigenschaft in Bezug auf Lese-und Schreibvorgänge.</span><span class="sxs-lookup"><span data-stu-id="6443e-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="6443e-1084">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1084">In the example</span></span>
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
<span data-ttu-id="6443e-1085">die `P` -Eigenschaft `B` in blendet die `A` -Eigenschaft in in Bezug auf das `P` lesen und schreiben aus.</span><span class="sxs-lookup"><span data-stu-id="6443e-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="6443e-1086">Folglich in den Anweisungen</span><span class="sxs-lookup"><span data-stu-id="6443e-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="6443e-1087">die Zuweisung von `b.P` bewirkt, dass ein Kompilierzeitfehler gemeldet wird, da die `P` schreibgeschützte Eigenschaft in `B` die Schreib `P` geschützte Eigenschaft in `A`ausblendet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="6443e-1088">Beachten Sie jedoch, dass eine Umwandlung verwendet werden kann, um auf die `P` Hidden-Eigenschaft zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="6443e-1089">Im Gegensatz zu öffentlichen Feldern stellen Eigenschaften eine Trennung zwischen dem internen Zustand eines Objekts und seiner öffentlichen Schnittstelle dar.</span><span class="sxs-lookup"><span data-stu-id="6443e-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="6443e-1090">Sehen Sie sich das Beispiel an:</span><span class="sxs-lookup"><span data-stu-id="6443e-1090">Consider the example:</span></span>
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

<span data-ttu-id="6443e-1091">Hier verwendet die `Label` -Klasse zwei `int` Felder, `x` und `y`, um ihren Speicherort zu speichern.</span><span class="sxs-lookup"><span data-stu-id="6443e-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="6443e-1092">`X` Der Speicherort wird sowohl als als `Y` auch `Location` als Eigenschaft des Typs `Point`öffentlich verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="6443e-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="6443e-1093">Wenn es in einer zukünftigen Version von `Label`bequemer wird, den Speicherort `Point` als intern zu speichern, kann die Änderung vorgenommen werden, ohne dass sich dies auf die öffentliche Schnittstelle der Klasse auswirkt:</span><span class="sxs-lookup"><span data-stu-id="6443e-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
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

<span data-ttu-id="6443e-1094">Waren `x` und `y` stattdessen `Label` Felder, wäre es unmöglich, eine solche Änderung an der-Klasse vorzunehmen. `public readonly`</span><span class="sxs-lookup"><span data-stu-id="6443e-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="6443e-1095">Das verfügbar machen des Zustands durch Eigenschaften ist nicht notwendigerweise weniger effizient, als Felder direkt verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="6443e-1096">Insbesondere wenn eine Eigenschaft nicht virtuell ist und nur eine kleine Menge an Code enthält, kann die Ausführungsumgebung Aufrufe von Accessoren durch den tatsächlichen Code der Accessoren ersetzen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="6443e-1097">Dieser Prozess wird als ***Inlining***bezeichnet und macht den Eigenschafts Zugriff so effizient wie der Feld Zugriff, behält jedoch die höhere Flexibilität von Eigenschaften bei.</span><span class="sxs-lookup"><span data-stu-id="6443e-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="6443e-1098">Da das Aufrufen `get` eines Accessoren konzeptionell Äquivalent zum Lesen des Werts eines Felds ist, gilt es als ungültiges Programmier Format `get` für Accessoren, um Observable-Nebeneffekte zu haben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="6443e-1099">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="6443e-1100">der Wert `Next` der-Eigenschaft hängt von der Häufigkeit ab, mit der zuvor auf die Eigenschaft zugegriffen wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="6443e-1101">Folglich erzeugt der Zugriff auf die-Eigenschaft einen beobachtbaren Nebeneffekt, und die-Eigenschaft sollte stattdessen als Methode implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="6443e-1102">Die "No Side-Effects"-Konvention `get` für Accessoren bedeutet nicht `get` , dass Accessoren immer so geschrieben werden müssen, dass Sie einfach in Feldern gespeicherte Werte zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="6443e-1103">Accessoren `get` berechnen häufig den Wert einer Eigenschaft, indem Sie auf mehrere Felder zugreifen oder Methoden aufrufen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="6443e-1104">Ein ordnungsgemäß entworfener `get` Accessor führt jedoch keine Aktionen aus, die Observable-Änderungen im Status des Objekts bewirken.</span><span class="sxs-lookup"><span data-stu-id="6443e-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="6443e-1105">Eigenschaften können verwendet werden, um die Initialisierung einer Ressource zu verzögern, bis zu dem Zeitpunkt, zu dem Sie erstmals referenziert wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="6443e-1106">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6443e-1106">For example:</span></span>
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

<span data-ttu-id="6443e-1107">Die `Console` -Klasse enthält die drei `In`Eigenschaften `Out`,, `Error`und, die jeweils die standardmäßigen Eingabe-, Ausgabe-und Fehler Geräte darstellen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="6443e-1108">Wenn diese Member als Eigenschaften verfügbar gemacht werden `Console` , kann die-Klasse Ihre Initialisierung verzögern, bis Sie tatsächlich verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="6443e-1109">Beispielsweise nach dem ersten Verweis auf die `Out` -Eigenschaft, wie in</span><span class="sxs-lookup"><span data-stu-id="6443e-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="6443e-1110">der zugrunde `TextWriter` liegende für das Ausgabegerät wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="6443e-1111">Wenn die Anwendung jedoch keinen Verweis auf die- `In` Eigenschaft `Error` und die-Eigenschaft durchführt, werden für diese Geräte keine Objekte erstellt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="6443e-1112">Automatisch implementierte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="6443e-1112">Automatically implemented properties</span></span>

<span data-ttu-id="6443e-1113">Eine automatisch implementierte Eigenschaft (oder ***Auto-Eigenschaft*** für Short) ist eine nicht abstrakte nicht-externe Eigenschaft mit nur Semikolon-Zugriffsmethoden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="6443e-1114">Auto-Eigenschaften müssen über einen get-Accessor verfügen und optional über einen Set-Accessor verfügen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="6443e-1115">Wenn eine Eigenschaft als automatisch implementierte Eigenschaft angegeben wird, ist ein verborgenes dahinter liegendes Feld für die Eigenschaft automatisch verfügbar, und die Accessoren werden implementiert, um aus dem dahinter liegenden Feld zu lesen und in dieses zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="6443e-1116">Wenn die Auto-Eigenschaft keinen Set-Accessor aufweist, wird das Unterstützungs Feld `readonly` als (schreibgeschützte[Felder](classes.md#readonly-fields)) betrachtet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="6443e-1117">Genau wie ein `readonly` Feld kann auch eine nur-Getter-Auto-Eigenschaft im Text eines Konstruktors der einschließenden Klasse zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="6443e-1118">Eine solche Zuweisung wird direkt dem schreibgeschützten Unterstützungs Feld der-Eigenschaft zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="6443e-1119">Eine Auto-Eigenschaft kann optional über ein *property_initializer*verfügen, das direkt auf das Unterstützungs Feld als *variable_initializer* ([Variableninitialisierer](classes.md#variable-initializers)) angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="6443e-1120">Im Beispiel unten geschieht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="6443e-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="6443e-1121">entspricht der folgenden Deklaration:</span><span class="sxs-lookup"><span data-stu-id="6443e-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="6443e-1122">Im Beispiel unten geschieht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="6443e-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="6443e-1123">entspricht der folgenden Deklaration:</span><span class="sxs-lookup"><span data-stu-id="6443e-1123">is equivalent to the following declaration:</span></span>
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

<span data-ttu-id="6443e-1124">Beachten Sie, dass die Zuweisungen zum schreibgeschützten Feld zulässig sind, da Sie im Konstruktor auftreten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="6443e-1125">Zugriff</span><span class="sxs-lookup"><span data-stu-id="6443e-1125">Accessibility</span></span>

<span data-ttu-id="6443e-1126">Wenn ein Accessor über eine *accessor_modifier*verfügt, wird die Zugriffs Domäne ([Barrierefreiheits Domänen](basic-concepts.md#accessibility-domains)) der Zugriffsmethode mithilfe des deklarierten zugriffszugriffs auf die *accessor_modifier*festgelegt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="6443e-1127">Wenn ein Accessor nicht über eine *accessor_modifier*verfügt, wird die Zugriffs Domäne des Accessors anhand der deklarierten Zugriffsmethode der Eigenschaft oder des Indexers bestimmt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="6443e-1128">Das vorhanden sein eines *accessor_modifier* hat niemals Auswirkungen auf die Suche nach Membern ([Operatoren](expressions.md#operators)) oder Überladungs Auflösung ([Überladungs Auflösung](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="6443e-1129">Die Modifizierer für die Eigenschaft oder den Indexer bestimmen unabhängig vom Kontext des Zugriffs immer, an welche Eigenschaft oder welcher Indexer gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="6443e-1130">Nachdem eine bestimmte Eigenschaft oder ein Indexer ausgewählt wurde, werden die Barrierefreiheits Domänen der beteiligten spezifischen Accessoren verwendet, um zu bestimmen, ob diese Verwendung gültig ist:</span><span class="sxs-lookup"><span data-stu-id="6443e-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="6443e-1131">Wenn die Verwendung als Wert ([Werte von Ausdrücken](expressions.md#values-of-expressions)) verwendet wird, muss `get` der Accessor vorhanden und zugänglich sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="6443e-1132">Wenn die Verwendung als Ziel einer einfachen Zuweisung ([einfache Zuweisung](expressions.md#simple-assignment)) verwendet wird, muss der `set` -Accessor vorhanden und zugänglich sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="6443e-1133">Wenn die Verwendung als Ziel der Verbund Zuweisung ([Verbund Zuweisung](expressions.md#compound-assignment)) oder `++` als Ziel der Operatoren oder `--` ([Funktionsmember](expressions.md#function-members)0,9, [Aufruf Ausdrücke](expressions.md#invocation-expressions)) verwendet wird, werden sowohl die `get` Accessoren als auch der `set` -Accessor muss vorhanden und zugänglich sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="6443e-1134">Im folgenden Beispiel wird die-Eigenschaft `A.Text` durch die-Eigenschaft `B.Text`ausgeblendet, auch in Kontexten, in denen `set` nur der-Accessor aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="6443e-1135">Im Gegensatz dazu kann die `B.Count` -Eigenschaft nicht von der `M`-Klasse aufgerufen werden, `A.Count` sodass stattdessen die barrierefreie Eigenschaft verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

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

<span data-ttu-id="6443e-1136">Ein Accessor, der zur Implementierung einer Schnittstelle verwendet wird, verfügt möglicherweise nicht über eine *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="6443e-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="6443e-1137">Wenn nur ein Accessor verwendet wird, um eine Schnittstelle zu implementieren, kann der andere Accessor mit einem *accessor_modifier*deklariert werden:</span><span class="sxs-lookup"><span data-stu-id="6443e-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="6443e-1138">Virtual-, sealed-, override-und Abstract-Eigenschaftenaccessoren</span><span class="sxs-lookup"><span data-stu-id="6443e-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="6443e-1139">Eine `virtual` Eigenschafts Deklaration gibt an, dass die Accessoren der Eigenschaft virtuell sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="6443e-1140">Der `virtual` -Modifizierer gilt für beide Accessoren einer Lese-/Schreibeigenschaft – es ist nicht möglich, dass nur ein Accessor einer Eigenschaft mit Lese-/Schreibzugriff virtuell ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="6443e-1141">Eine `abstract` Eigenschafts Deklaration gibt an, dass die Accessoren der Eigenschaft virtuell sind, aber keine tatsächliche Implementierung der Accessoren bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="6443e-1142">Stattdessen sind nicht abstrakte abgeleitete Klassen erforderlich, um eine eigene Implementierung für die Accessoren bereitzustellen, indem die-Eigenschaft überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="6443e-1143">Da ein Accessor für eine abstrakte Eigenschaften Deklaration keine tatsächliche Implementierung bereitstellt, besteht seine *accessor_body* einfach aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="6443e-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="6443e-1144">Eine Eigenschafts Deklaration, die `abstract` den `override` -Modifizierer und den-Modifizierer enthält, gibt an, dass die Eigenschaft abstrakt ist, und überschreibt</span><span class="sxs-lookup"><span data-stu-id="6443e-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="6443e-1145">Die Accessoren einer solchen Eigenschaft sind ebenfalls abstrakt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="6443e-1146">Abstrakte Eigenschafts Deklarationen sind nur in abstrakten Klassen zulässig ([abstrakte Klassen](classes.md#abstract-classes)). Die Accessoren einer geerbten virtuellen Eigenschaft können in einer abgeleiteten Klasse überschrieben werden, indem Sie eine Eigenschaften Deklaration `override` einschließen, die eine-Direktive angibt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="6443e-1147">Dies wird als über schreibende ***Eigenschaften Deklaration***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="6443e-1148">Eine über schreibende Eigenschaften Deklaration deklariert keine neue Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6443e-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="6443e-1149">Stattdessen werden lediglich die Implementierungen der Accessoren einer vorhandenen virtuellen Eigenschaft spezialisiert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="6443e-1150">Eine über schreibende Eigenschaften Deklaration muss genau dieselben Zugriffsmodifizierer, denselben Typ und denselben Namen wie die geerbte Eigenschaft angeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="6443e-1151">Wenn die geerbte Eigenschaft nur über einen einzigen Accessor verfügt (d. h., wenn die geerbte Eigenschaft schreibgeschützt oder schreibgeschützt ist), muss die über schreibende Eigenschaft nur den Accessor enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="6443e-1152">Wenn die geerbte Eigenschaft beide Accessoren enthält (d. h., wenn die geerbte Eigenschaft Lese-/Schreibzugriff hat), kann die über schreibende Eigenschaft entweder einen einzelnen Accessor oder beide Accessoren einschließen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="6443e-1153">Eine über schreibende Eigenschaften Deklaration `sealed` kann den-Modifizierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="6443e-1154">Durch die Verwendung dieses Modifizierers wird verhindert, dass eine abgeleitete Klasse die Eigenschaft weiter überschreibt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="6443e-1155">Die Accessoren einer versiegelten Eigenschaft sind ebenfalls versiegelt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="6443e-1156">Mit Ausnahme der Unterschiede in der Deklaration und der Aufruf Syntax Verhalten sich virtuelle, versiegelte, Überschreibungs-und abstrakte Accessoren genauso wie virtuelle, versiegelte, Überschreibungs-und abstrakte Methoden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="6443e-1157">Insbesondere gelten die in [virtuellen Methoden](classes.md#virtual-methods), [Methoden](classes.md#override-methods)zum überschreiben, [versiegelten Methoden](classes.md#sealed-methods)und [abstrakten Methoden](classes.md#abstract-methods) beschriebenen Regeln so, als wären Accessoren Methoden einer entsprechenden Form:</span><span class="sxs-lookup"><span data-stu-id="6443e-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="6443e-1158">Ein `get` -Accessor entspricht einer Parameter losen Methode mit einem Rückgabewert des Eigenschaftentyps und denselben Modifiziererwerten wie die enthaltende Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6443e-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="6443e-1159">Ein `set` -Accessor entspricht einer Methode mit einem einzelnen Wert Parameter des Eigenschaftentyps `void` , einem Rückgabetyp und denselben Modifiziererwerten wie die enthaltende Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6443e-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="6443e-1160">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1160">In the example</span></span>
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
<span data-ttu-id="6443e-1161">`X`ist eine virtuelle schreibgeschützte Eigenschaft, `Y` ist eine virtuelle Lese-/Schreibeigenschaft und `Z` eine abstrakte Lese-/Schreibeigenschaft.</span><span class="sxs-lookup"><span data-stu-id="6443e-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="6443e-1162">Da `Z` abstrakt ist, muss die enthaltende Klasse `A` auch als abstrakt deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="6443e-1163">Eine Klasse, die von `A` abgeleitet wird, wird unten angezeigt:</span><span class="sxs-lookup"><span data-stu-id="6443e-1163">A class that derives from `A` is show below:</span></span>
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

<span data-ttu-id="6443e-1164">Hier überschreiben die Deklarationen `Y`von `X`, `Z` und Eigenschafts Deklarationen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="6443e-1165">Jede Eigenschaften Deklaration stimmt genau mit den zugriffsmodifizierertypen und dem Namen der entsprechenden geerbten Eigenschaft überein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="6443e-1166">Der `get` -Accessor `X` von und `set` der-Accessor von `base` `Y` verwenden das-Schlüsselwort, um auf die geerbten Accessoren zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="6443e-1167">Die Deklaration `Z` von überschreibt beide abstrakten Accessoren – folglich gibt es keine ausstehenden abstrakten Funktionsmember `B`in, `B` und es darf sich um eine nicht abstrakte Klasse handeln.</span><span class="sxs-lookup"><span data-stu-id="6443e-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="6443e-1168">Wenn eine Eigenschaft als `override`deklariert wird, müssen alle überschriebenen Accessoren für den über schreibenden Code zugänglich sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="6443e-1169">Außerdem muss der deklarierte Zugriff sowohl für die Eigenschaft oder den Indexer selbst als auch für die Accessoren mit der der überschriebenen Member und Accessoren identisch sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="6443e-1170">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6443e-1170">For example:</span></span>
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

## <a name="events"></a><span data-ttu-id="6443e-1171">Ereignisse</span><span class="sxs-lookup"><span data-stu-id="6443e-1171">Events</span></span>

<span data-ttu-id="6443e-1172">Ein ***Ereignis*** ist ein Member, mit dem ein Objekt oder eine Klasse Benachrichtigungen bereitstellen kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="6443e-1173">Clients können ausführbaren Code durch Bereitstellen von ***Ereignis Handlern***für Ereignisse anfügen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="6443e-1174">Ereignisse werden mit *event_declaration*s deklariert:</span><span class="sxs-lookup"><span data-stu-id="6443e-1174">Events are declared using *event_declaration*s:</span></span>

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

<span data-ttu-id="6443e-1175">Ein *event_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)), den `new` ([den neuen Modifizierer](classes.md#the-new-modifier)), `static` ([statisch und Instanz) enthalten. Methoden](classes.md#static-and-instance-methods)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Überschreibungs Methoden](classes.md#override-methods)), `sealed` ([versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakte Methoden](classes.md#abstract-methods)) und `extern`-Modifizierer ([Externe Methoden](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6443e-1176">Ereignis Deklarationen unterliegen den gleichen Regeln wie Methoden Deklarationen ([Methoden](classes.md#methods)) in Bezug auf gültige Kombinationen von modifiziererereignissen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="6443e-1177">Der *Typ* einer Ereignis Deklaration muss ein *delegate_type* ([Verweis Typen](types.md#reference-types)) sein, und *delegate_type* muss mindestens so zugänglich sein wie das Ereignis selbst ([Barrierefreiheits Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6443e-1178">Eine Ereignis Deklaration kann *event_accessor_declarations*enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="6443e-1179">Wenn dies jedoch nicht der Fall ist, stellt der Compiler Sie für nicht-externe, nicht abstrakte Ereignisse automatisch bereit ([Feld ähnliche Ereignisse](classes.md#field-like-events)). bei externen Ereignissen werden die Accessoren extern bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="6443e-1180">Eine Ereignis Deklaration, die *event_accessor_declarations* auslässt, definiert mindestens ein Ereignis – eines für jede *variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="6443e-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="6443e-1181">Die Attribute und Modifizierer gelten für alle Member, die von einem solchen *event_declaration*deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="6443e-1182">Es handelt sich um einen Kompilierzeitfehler für ein *event_declaration* -Objekt, das sowohl den `abstract`-Modifizierer als auch das mit geschweiften Klammern getrennte *event_accessor_declarations*einschließt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="6443e-1183">Wenn eine Ereignis Deklaration einen `extern` Modifizierer enthält, wird das Ereignis als ***externes Ereignis***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="6443e-1184">Da eine externe Ereignis Deklaration keine tatsächliche Implementierung bereitstellt, ist es ein Fehler, dass Sie sowohl den `extern`-Modifizierer als auch *event_accessor_declarations*einschließen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="6443e-1185">Es handelt sich um einen Kompilierzeitfehler für eine *variable_declarator* einer Ereignis Deklaration mit einem `abstract`-oder `external`-Modifizierer, um ein *variable_initializer*-Zeichen einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="6443e-1186">Ein Ereignis kann als Linker Operand des `+=` Operators und `-=` ([Ereignis Zuweisung](expressions.md#event-assignment)) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="6443e-1187">Diese Operatoren werden zum Anfügen von Ereignis Handlern an oder zum Entfernen von Ereignis Handlern aus einem Ereignis verwendet, und die Zugriffsmodifizierer des Ereignisses steuern die Kontexte, in denen solche Vorgänge zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="6443e-1188">Da `+=` und`-=` die einzigen Vorgänge sind, die für ein Ereignis außerhalb des Typs zulässig sind, der das Ereignis deklariert, kann externer Code Handler für ein Ereignis hinzufügen und entfernen, aber nicht auf andere Weise die zugrunde liegende Ereignisliste abrufen oder ändern. Handler.</span><span class="sxs-lookup"><span data-stu-id="6443e-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="6443e-1189">Bei einem Vorgang des Formulars `x += y` oder `x -= y`, wenn `x` ein Ereignis und der Verweis außerhalb des Typs, der die Deklaration von `x`enthält, erfolgt, hat das Ergebnis des Vorgangs den Typ `void` (im Gegensatz zu der Typ von `x`mit dem Wert von `x` nach der Zuweisung).</span><span class="sxs-lookup"><span data-stu-id="6443e-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="6443e-1190">Diese Regel verhindert, dass externer Code indirekt den zugrunde liegenden Delegaten eines Ereignisses untersucht.</span><span class="sxs-lookup"><span data-stu-id="6443e-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="6443e-1191">Das folgende Beispiel zeigt, wie Ereignishandler an Instanzen der `Button` -Klasse angefügt werden:</span><span class="sxs-lookup"><span data-stu-id="6443e-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
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

<span data-ttu-id="6443e-1192">Hier erstellt der `LoginDialog` Instanzkonstruktor zwei `Button` -Instanzen und fügt Ereignishandler an die `Click` -Ereignisse an.</span><span class="sxs-lookup"><span data-stu-id="6443e-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="6443e-1193">Feld ähnliche Ereignisse</span><span class="sxs-lookup"><span data-stu-id="6443e-1193">Field-like events</span></span>

<span data-ttu-id="6443e-1194">Im Programmtext der Klasse oder Struktur, die die Deklaration eines Ereignisses enthält, können bestimmte Ereignisse wie Felder verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="6443e-1195">Um auf diese Weise verwendet zu werden, darf ein Ereignis nicht `abstract` oder `extern` sein und darf *event_accessor_declarations*nicht explizit enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="6443e-1196">Ein derartiges Ereignis kann in jedem Kontext verwendet werden, der ein Feld zulässt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="6443e-1197">Das-Feld enthält einen Delegaten[(](delegates.md)Delegaten), der auf die Liste der Ereignishandler verweist, die dem-Ereignis hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="6443e-1198">Wenn keine Ereignishandler hinzugefügt wurden, enthält `null`das Feld.</span><span class="sxs-lookup"><span data-stu-id="6443e-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="6443e-1199">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1199">In the example</span></span>
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
<span data-ttu-id="6443e-1200">`Click`wird als Feld in der `Button` -Klasse verwendet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="6443e-1201">Wie das Beispiel zeigt, kann das-Feld überprüft, geändert und in Delegataufrufausdrücken verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="6443e-1202">Die `OnClick` -Methode in `Button` der-Klasse löst das `Click` -Ereignis aus.</span><span class="sxs-lookup"><span data-stu-id="6443e-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="6443e-1203">Das Auslösen eines Ereignisses entspricht exakt dem Aufrufen des Delegaten, der durch das Ereignis repräsentiert wird, es gibt deshalb keine besonderen Sprachkonstrukte zum Auslösen von Ereignissen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="6443e-1204">Beachten Sie, dass dem Delegataufruf eine Prüfung vorangestellt ist, die sicherstellt, dass der Delegat nicht NULL ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="6443e-1205">Außerhalb der Deklaration `Button` der `+=` -Klasse kann `Click` der Member nur auf der linken Seite der Operatoren und `-=` verwendet werden, wie in</span><span class="sxs-lookup"><span data-stu-id="6443e-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="6443e-1206">, der einen Delegaten an die Aufruf Liste des `Click` Ereignisses anfügt, und</span><span class="sxs-lookup"><span data-stu-id="6443e-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="6443e-1207">entfernt einen Delegaten aus der Aufruf Liste des `Click` Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="6443e-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="6443e-1208">Beim Kompilieren eines Feld ähnlichen Ereignisses erstellt der Compiler automatisch Speicher, um den Delegaten aufzunehmen, und erstellt Accessoren für das Ereignis, mit dem Ereignishandler zum Delegatfeld hinzugefügt oder daraus entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="6443e-1209">Die hinzu Füge-und Entfernungs Vorgänge sind Thread sicher und können (jedoch nicht erforderlich) ausgeführt werden, während die Sperre ([lock-Anweisung](statements.md#the-lock-statement)) für das enthaltende Objekt für ein Instanzereignis oder das Typobjekt ([Anonyme Objekt Erstellungs Ausdrücke](expressions.md#anonymous-object-creation-expressions)) aufrechterhalten werden. für ein statisches Ereignis.</span><span class="sxs-lookup"><span data-stu-id="6443e-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="6443e-1210">Daher ist eine Instanzereignisdeklaration der folgenden Form:</span><span class="sxs-lookup"><span data-stu-id="6443e-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="6443e-1211">wird in eine entsprechende kompiliert:</span><span class="sxs-lookup"><span data-stu-id="6443e-1211">will be compiled to something equivalent to:</span></span>
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
<span data-ttu-id="6443e-1212">In der `+=` - `X`Klasse bewirken Verweise `Ev` auf auf der linken Seite der Operatoren und `-=` , dass die Add-und remove-Accessoren aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="6443e-1213">Alle anderen Verweise auf `Ev` werden kompiliert, um stattdessen auf das `__Ev` ausgeblendete Feld zu verweisen ([Member Access](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="6443e-1214">Der Name "`__Ev`" ist willkürlich; das ausgeblendete Feld kann einen beliebigen Namen oder keinen Namen haben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="6443e-1215">Ereignisaccessoren</span><span class="sxs-lookup"><span data-stu-id="6443e-1215">Event accessors</span></span>

<span data-ttu-id="6443e-1216">Ereignis Deklarationen lassen *event_accessor_declarations*üblicherweise aus, wie im obigen Beispiel `Button`.</span><span class="sxs-lookup"><span data-stu-id="6443e-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="6443e-1217">Eine Situation hierfür ist der Fall, in dem die Speicherkosten eines Felds pro Ereignis nicht zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="6443e-1218">In solchen Fällen kann eine Klasse *event_accessor_declarations* einschließen und einen privaten Mechanismus zum Speichern der Liste von Ereignis Handlern verwenden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="6443e-1219">Der *event_accessor_declarations* eines Ereignisses gibt die ausführbaren Anweisungen an, die dem Hinzufügen und Entfernen von Ereignis Handlern zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="6443e-1220">Die Accessordeklarationen bestehen aus einem *add_accessor_declaration* -und einem *remove_accessor_declaration*-Operator.</span><span class="sxs-lookup"><span data-stu-id="6443e-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="6443e-1221">Jede Accessordeklaration besteht aus dem `add` Token `remove` oder gefolgt von einem- *Block*.</span><span class="sxs-lookup"><span data-stu-id="6443e-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="6443e-1222">Der einem *add_accessor_declaration* zugeordnete *Block* gibt die Anweisungen an, die beim Hinzufügen eines Ereignis Handlers ausgeführt werden sollen, und der mit einem *remove_accessor_declaration* -Block verknüpfte *Block* gibt die auszuführenden Anweisungen an. Wenn ein Ereignishandler entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="6443e-1223">Jede *add_accessor_declaration* und *remove_accessor_declaration* entspricht einer Methode mit einem einzelnen Wert Parameter des Ereignis Typs und einem `void`-Rückgabetyp.</span><span class="sxs-lookup"><span data-stu-id="6443e-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="6443e-1224">Der implizite Parameter eines Ereignis Accessors heißt `value`.</span><span class="sxs-lookup"><span data-stu-id="6443e-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="6443e-1225">Wenn ein Ereignis in einer Ereignis Zuweisung verwendet wird, wird der entsprechende Ereignis Accessor verwendet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="6443e-1226">Insbesondere, wenn der Zuweisungs Operator `+=` ist, wird der Add-Accessor verwendet, und wenn der Zuweisungs Operator ist `-=` , wird der remove-Accessor verwendet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="6443e-1227">In beiden Fällen wird der rechte Operand des Zuweisungs Operators als Argument für den Ereignis Accessor verwendet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="6443e-1228">Der-Block eines *add_accessor_declaration* -oder *remove_accessor_declaration* -muss den Regeln für `void`-Methoden entsprechen, die im [Methoden Text](classes.md#method-body)beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6443e-1229">Insbesondere `return` -Anweisungen in einem solchen Block dürfen keinen Ausdruck angeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="6443e-1230">Da ein Ereignis Accessor implizit einen Parameter mit dem `value`Namen aufweist, handelt es sich um einen Kompilierzeitfehler für eine lokale Variable oder Konstante, die in einem Ereignis Accessor deklariert wurde, um diesen Namen zu haben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="6443e-1231">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1231">In the example</span></span>
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
<span data-ttu-id="6443e-1232">die `Control` -Klasse implementiert einen internen Speichermechanismus für-Ereignisse.</span><span class="sxs-lookup"><span data-stu-id="6443e-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="6443e-1233">Die `AddEventHandler` -Methode ordnet einem Schlüssel einen Delegatwert zu `GetEventHandler` , die Methode gibt den derzeit einem Schlüssel zugeordneten Delegaten `RemoveEventHandler` zurück, und die Methode entfernt einen Delegaten als Ereignishandler für das angegebene Ereignis.</span><span class="sxs-lookup"><span data-stu-id="6443e-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="6443e-1234">Vermutlich ist der zugrunde liegende Speichermechanismus so konzipiert, dass es keine Kosten für das Zuordnen eines `null` Delegatwerts zu einem Schlüssel gibt. daher verbrauchen nicht behandelte Ereignisse keinen Speicherplatz.</span><span class="sxs-lookup"><span data-stu-id="6443e-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="6443e-1235">Statische Ereignisse und Instanzereignisse</span><span class="sxs-lookup"><span data-stu-id="6443e-1235">Static and instance events</span></span>

<span data-ttu-id="6443e-1236">Wenn eine Ereignis Deklaration einen `static` Modifizierer enthält, wird das Ereignis als ***statisches Ereignis***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="6443e-1237">Wenn kein `static` Modifizierer vorhanden ist, wird das Ereignis als ***Instanzereignis***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="6443e-1238">Ein statisches Ereignis ist nicht mit einer bestimmten Instanz verknüpft, und es ist ein Kompilierzeitfehler, auf `this` den in den Accessoren eines statischen Ereignisses verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="6443e-1239">Ein Instanzereignis ist einer bestimmten Instanz einer Klasse zugeordnet, und auf diese Instanz kann in den Accessoren dieses Ereignisses als `this` ([dieser Zugriff](expressions.md#this-access)) zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="6443e-1240">Wenn auf ein Ereignis in einem *member_access* -Element ([Member Access](expressions.md#member-access)) der Form `E.M` verwiesen wird, muss, wenn `M` ein statisches Ereignis ist, `E` einen Typ angeben, der `M` enthält, und wenn `M` ein Instanzereignis ist, muss E eine Instanz eines Typs angeben, der enthält. `M`.</span><span class="sxs-lookup"><span data-stu-id="6443e-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6443e-1241">Die Unterschiede zwischen statischen und Instanzmembern werden in [statischen und Instanzmembern](classes.md#static-and-instance-members)ausführlicher erläutert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="6443e-1242">Virtual-, sealed-, override-und Abstract-Ereignisaccessoren</span><span class="sxs-lookup"><span data-stu-id="6443e-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="6443e-1243">Eine `virtual` -Ereignis Deklaration gibt an, dass die Accessoren dieses Ereignisses virtuell sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="6443e-1244">Der `virtual` -Modifizierer gilt für beide Accessoren eines Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="6443e-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="6443e-1245">Eine `abstract` Ereignis Deklaration gibt an, dass die Accessoren des Ereignisses virtuell sind, aber keine tatsächliche Implementierung der Accessoren bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="6443e-1246">Stattdessen sind nicht abstrakte abgeleitete Klassen erforderlich, um eine eigene Implementierung für die Accessoren bereitzustellen, indem das-Ereignis überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="6443e-1247">Da eine abstrakte Ereignis Deklaration keine tatsächliche Implementierung bereitstellt, kann Sie keine durch Klammern getrennte *event_accessor_declarations*bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="6443e-1248">Eine Ereignis Deklaration, die den `abstract` - `override` Modifizierer und den-Modifizierer enthält, gibt an, dass das Ereignis abstrakt ist, und überschreibt ein</span><span class="sxs-lookup"><span data-stu-id="6443e-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="6443e-1249">Die Accessoren eines solchen Ereignisses sind ebenfalls abstrakt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="6443e-1250">Abstrakte Ereignis Deklarationen sind nur in abstrakten Klassen zulässig ([abstrakte Klassen](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="6443e-1251">Die Accessoren eines geerbten virtuellen Ereignisses können durch Einschließen einer Ereignis Deklaration, die einen `override` Modifizierer angibt, in einer abgeleiteten Klasse überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="6443e-1252">Dies wird als über schreibende ***Ereignis Deklaration***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="6443e-1253">Eine über schreibende Ereignis Deklaration deklariert kein neues Ereignis.</span><span class="sxs-lookup"><span data-stu-id="6443e-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="6443e-1254">Stattdessen werden lediglich die Implementierungen der Accessoren eines vorhandenen virtuellen Ereignisses spezialisiert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="6443e-1255">Eine über schreibende Ereignis Deklaration muss genau dieselben Zugriffsmodifizierer, denselben Typ und denselben Namen wie das überschriebene Ereignis angeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="6443e-1256">Eine über schreibende Ereignis Deklaration `sealed` kann den-Modifizierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="6443e-1257">Durch die Verwendung dieses Modifizierers wird verhindert, dass eine abgeleitete Klasse das Ereignis weiter überschreibt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="6443e-1258">Die Accessoren eines versiegelten Ereignisses werden ebenfalls versiegelt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="6443e-1259">Es ist ein Kompilierzeitfehler, wenn eine über schreibende Ereignis Deklaration einen `new` -Modifizierer einschließt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="6443e-1260">Mit Ausnahme der Unterschiede in der Deklaration und der Aufruf Syntax Verhalten sich virtuelle, versiegelte, Überschreibungs-und abstrakte Accessoren genauso wie virtuelle, versiegelte, Überschreibungs-und abstrakte Methoden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="6443e-1261">Insbesondere gelten die in [Virtual Methods](classes.md#virtual-methods), [override Methods](classes.md#override-methods), [sealed Methods](classes.md#sealed-methods)und [abstract Methods](classes.md#abstract-methods) beschriebenen Regeln so, als wären Accessoren Methoden einer entsprechenden Form.</span><span class="sxs-lookup"><span data-stu-id="6443e-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="6443e-1262">Jeder-Accessor entspricht einer Methode mit einem einzelnen value-Parameter des Ereignis Typs, einem `void` Rückgabetyp und denselben Modifiziererwerten wie das enthaltende Ereignis.</span><span class="sxs-lookup"><span data-stu-id="6443e-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="6443e-1263">Indexer</span><span class="sxs-lookup"><span data-stu-id="6443e-1263">Indexers</span></span>

<span data-ttu-id="6443e-1264">Ein ***Indexer*** ist ein Member, mit dem ein Objekt auf die gleiche Weise wie ein Array indiziert werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="6443e-1265">Indexer werden mithilfe von *indexer_declaration*s deklariert:</span><span class="sxs-lookup"><span data-stu-id="6443e-1265">Indexers are declared using *indexer_declaration*s:</span></span>

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

<span data-ttu-id="6443e-1266">Ein *indexer_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)), den `new` ([den neuen Modifizierer](classes.md#the-new-modifier)), `virtual` ([virtuelle Methoden) enthalten. ](classes.md#virtual-methods)), `override` ([Überschreibungs Methoden](classes.md#override-methods)), `sealed` ([versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakte Methoden](classes.md#abstract-methods)) und `extern` ([Externe Methoden](classes.md#external-methods))-Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="6443e-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6443e-1267">Indexer-Deklarationen unterliegen den gleichen Regeln wie Methoden Deklarationen ([Methoden](classes.md#methods)) in Bezug auf gültige Kombinationen von Modifizierern, wobei die einzige Ausnahme darin besteht, dass der statische Modifizierer in einer Indexer-Deklaration nicht zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="6443e-1268">Die `virtual`modifiziererer `override`, und `abstract` schließen sich gegenseitig aus, außer in einem Fall.</span><span class="sxs-lookup"><span data-stu-id="6443e-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="6443e-1269">Die `abstract` Modifizierer und `override` können zusammen verwendet werden, damit ein abstrakter Indexer einen virtuellen überschreiben kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="6443e-1270">Der *Typ* einer Indexer-Deklaration gibt den Elementtyp des Indexers an, der von der Deklaration eingeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="6443e-1271">Es *sei denn* , der Indexer ist eine explizite Schnittstellenmember-Implementierung, gefolgt vom `this`-Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="6443e-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="6443e-1272">Für eine explizite Implementierung des Schnittstellenmembers folgt der *Typ* *INTERFACE_TYPE*, a "`.`" und das Schlüsselwort `this`.</span><span class="sxs-lookup"><span data-stu-id="6443e-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="6443e-1273">Im Gegensatz zu anderen Membern haben Indexer keine benutzerdefinierten Namen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="6443e-1274">*Formal_parameter_list* gibt die Parameter des Indexers an.</span><span class="sxs-lookup"><span data-stu-id="6443e-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="6443e-1275">Die Liste formaler Parameter eines Indexers entspricht der einer Methode ([Methoden Parameter](classes.md#method-parameters)), mit dem Unterschied, dass mindestens ein Parameter angegeben werden muss und dass die `ref` - `out` und-Parametermodifizierer nicht zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="6443e-1276">Der *Typ* eines Indexers und alle Typen, auf die in *formal_parameter_list* verwiesen wird, müssen mindestens so zugänglich sein wie der Indexer selbst ([Barrierefreiheits Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6443e-1277">Ein *indexer_body* kann entweder aus einem ***Accessor-Text*** oder einem ***Ausdrucks Körper***bestehen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="6443e-1278">In einem Accessor-Text muss *accessor_declarations*, der in "`{`" und "`}`"-Token eingeschlossen werden muss, die Accessoren ([Accessoren](classes.md#accessors)) der Eigenschaft deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="6443e-1279">Die Accessoren geben die ausführbaren Anweisungen an, die dem Lesen und Schreiben der Eigenschaft zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="6443e-1280">Ein Ausdrucks Text, der aus`=>`"" gefolgt von einem `E` Ausdruck und einem Semikolon besteht, entspricht exakt dem `{ get { return E; } }`Anweisungs Text und kann daher nur zum Angeben von nur-Getter-indexatoren verwendet werden, bei denen das Ergebnis des Getters wird von einem einzelnen Ausdruck angegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="6443e-1281">Obwohl die Syntax für den Zugriff auf ein Indexer-Element mit der Syntax für ein Array Element identisch ist, wird ein Indexer-Element nicht als Variable klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="6443e-1282">Folglich ist es nicht möglich, ein Indexer-Element als `ref` -oder `out` -Argument zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="6443e-1283">Die Liste der formalen Parameter eines Indexers definiert die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) des Indexers.</span><span class="sxs-lookup"><span data-stu-id="6443e-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="6443e-1284">Insbesondere besteht die Signatur eines Indexers aus der Anzahl und den Typen der formalen Parameter.</span><span class="sxs-lookup"><span data-stu-id="6443e-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="6443e-1285">Der Elementtyp und die Namen der formalen Parameter sind nicht Teil der Signatur eines Indexers.</span><span class="sxs-lookup"><span data-stu-id="6443e-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="6443e-1286">Die Signatur eines Indexer muss sich von den Signaturen aller anderen Indexer unterscheiden, die in derselben Klasse deklariert sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="6443e-1287">Indexer und Eigenschaften sind in der Konzeption sehr ähnlich, unterscheiden sich jedoch wie folgt:</span><span class="sxs-lookup"><span data-stu-id="6443e-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="6443e-1288">Eine Eigenschaft wird anhand ihres Namens identifiziert, während ein Indexer durch die Signatur identifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="6443e-1289">Der Zugriff auf eine Eigenschaft erfolgt über einen *Simple_name* ([simple names](expressions.md#simple-names)) oder einen *member_access* ([Member Access](expressions.md#member-access)), während auf ein Indexer-Element über einen *element_access* ([Indexer-Zugriff](expressions.md#indexer-access)) zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="6443e-1290">Eine Eigenschaft kann ein `static` Member sein, während ein Indexer immer ein Instanzmember ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="6443e-1291">Ein `get` -Accessor einer Eigenschaft entspricht einer Methode ohne Parameter, während ein `get` -Accessor eines Indexers einer Methode mit derselben formalen Parameterliste wie der Indexer entspricht.</span><span class="sxs-lookup"><span data-stu-id="6443e-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="6443e-1292">Ein `set` -Accessor einer Eigenschaft entspricht einer Methode mit einem einzelnen Parameter mit dem `value`Namen, während `set` ein Accessor eines Indexers einer Methode mit derselben formalen Parameterliste wie der Indexer entspricht, zuzüglich eines zusätzlichen Parameters. mit `value`dem Namen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="6443e-1293">Es ist ein Kompilierzeitfehler für einen Indexer-Accessor, eine lokale Variable mit dem gleichen Namen wie ein Indexer-Parameter zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="6443e-1294">In einer über schreibenden Eigenschaften Deklaration wird auf die geerbte Eigenschaft `base.P`mithilfe der `P` -Syntax zugegriffen, wobei der Eigenschaftsname ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="6443e-1295">In einer über schreibenden Indexer-Deklaration wird auf den geerbten Indexer mithilfe `E` der-Syntax `base[E]`zugegriffen, wobei eine durch Kommas getrennte Liste von Ausdrücken ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="6443e-1296">Es gibt kein Konzept für einen "automatisch implementierten Indexer".</span><span class="sxs-lookup"><span data-stu-id="6443e-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="6443e-1297">Es ist ein Fehler, wenn ein nicht abstrakter, nicht externer Indexer mit Semikolon-Accessoren vorliegt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="6443e-1298">Abgesehen von diesen Unterschieden gelten alle in [Accessoren](classes.md#accessors) und [automatisch implementierten Eigenschaften](classes.md#automatically-implemented-properties) definierten Regeln sowohl für Indexer-Accessoren als auch für Eigenschaftenaccessoren.</span><span class="sxs-lookup"><span data-stu-id="6443e-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="6443e-1299">Wenn eine Indexer-Deklaration `extern` einen Modifizierer enthält, wird der Indexer als ***externer Indexer***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="6443e-1300">Da eine externe Indexer-Deklaration keine tatsächliche Implementierung bereitstellt, besteht jede Ihrer *accessor_declarations* aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="6443e-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="6443e-1301">Im folgenden Beispiel wird eine `BitArray` Klasse deklariert, die einen Indexer für den Zugriff auf die einzelnen Bits im BitArray implementiert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
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

<span data-ttu-id="6443e-1302">Eine Instanz der `BitArray` -Klasse beansprucht wesentlich weniger Arbeitsspeicher als eine `bool[]` entsprechende (da jeder Wert des ersten-Werts nur ein Bit und nicht das zweite Byte) beansprucht, aber die gleichen Vorgänge wie eine `bool[]`zulässt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="6443e-1303">Die folgende `CountPrimes` Klasse verwendet einen `BitArray` und den klassischen "Sieve"-Algorithmus, um die Anzahl von PRIMES zwischen 1 und einem angegebenen Maximum zu berechnen:</span><span class="sxs-lookup"><span data-stu-id="6443e-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
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

<span data-ttu-id="6443e-1304">Beachten Sie, dass die Syntax für den Zugriff `BitArray` auf Elemente von exakt der gleiche ist `bool[]`wie für ein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="6443e-1305">Im folgenden Beispiel wird eine 26 \* 10-Raster Klasse gezeigt, die über einen Indexer mit zwei Parametern verfügt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="6443e-1306">Der erste Parameter muss ein groß-oder Kleinbuchstabe im Bereich A-Z sein, und der zweite Parameter muss eine ganze Zahl im Bereich 0-9 sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

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

### <a name="indexer-overloading"></a><span data-ttu-id="6443e-1307">Überladen von Indexern</span><span class="sxs-lookup"><span data-stu-id="6443e-1307">Indexer overloading</span></span>

<span data-ttu-id="6443e-1308">Die Regeln der Indexer-Überladungs Auflösung werden unter [Typrückschluss](expressions.md#type-inference)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="6443e-1309">Operatoren</span><span class="sxs-lookup"><span data-stu-id="6443e-1309">Operators</span></span>

<span data-ttu-id="6443e-1310">Ein ***Operator*** ist ein Member, der die Bedeutung eines Ausdrucks Operators definiert, der auf Instanzen der Klasse angewendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="6443e-1311">Operatoren werden mit *operator_declaration*s deklariert:</span><span class="sxs-lookup"><span data-stu-id="6443e-1311">Operators are declared using *operator_declaration*s:</span></span>

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

<span data-ttu-id="6443e-1312">Es gibt drei Kategorien von über ladbaren Operatoren: Unäre Operatoren ([unäre Operatoren](classes.md#unary-operators)), binäre Operatoren ([binäre Operatoren](classes.md#binary-operators)) und Konvertierungs Operatoren ([Konvertierungs Operatoren](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="6443e-1313">Der *operator_body* ist entweder ein Semikolon, ein ***Anweisungs Text*** oder ein ***Ausdrucks Körper***.</span><span class="sxs-lookup"><span data-stu-id="6443e-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="6443e-1314">Ein Anweisungs Text besteht aus einem- *Block*, der die auszuführenden Anweisungen angibt, wenn der Operator aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="6443e-1315">Der- *Block* muss den Regeln für Rückgabe Methoden entsprechen, die im [Methoden Text](classes.md#method-body)beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6443e-1316">Ein Ausdrucks Körper besteht aus `=>` , gefolgt von einem Ausdruck und einem Semikolon und deutet auf einen einzelnen Ausdruck hin, der beim Aufrufen des Operators ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="6443e-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="6443e-1317">Bei `extern`-Operatoren besteht das *operator_body* einfach aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="6443e-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="6443e-1318">Für alle anderen Operatoren ist *operator_body* entweder ein Block Körper oder ein Ausdrucks Körper.</span><span class="sxs-lookup"><span data-stu-id="6443e-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="6443e-1319">Die folgenden Regeln gelten für alle Operator Deklarationen:</span><span class="sxs-lookup"><span data-stu-id="6443e-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="6443e-1320">Eine Operator Deklaration muss sowohl einen `public` -als `static` auch einen-Modifizierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="6443e-1321">Die Parameter eines Operators müssen Wert Parameter ([value](variables.md#value-parameters)-Parameter) sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="6443e-1322">Es handelt sich um einen Kompilierzeitfehler für eine Operator Deklaration `out` zum angeben `ref` von-oder-Parametern.</span><span class="sxs-lookup"><span data-stu-id="6443e-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="6443e-1323">Die Signatur eines Operators ([unäre Operatoren](classes.md#unary-operators), [binäre Operatoren](classes.md#binary-operators)und [Konvertierungs Operatoren](classes.md#conversion-operators)) muss sich von den Signaturen aller anderen Operatoren unterscheiden, die in derselben Klasse deklariert sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="6443e-1324">Alle Typen, auf die in einer Operator Deklaration verwiesen wird, müssen mindestens so zugänglich sein wie der Operator selbst ([Barrierefreiheits Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="6443e-1325">Es ist ein Fehler, dass derselbe Modifizierer mehrmals in einer Operator Deklaration angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="6443e-1326">Jede Operator Kategorie erzwingt zusätzliche Einschränkungen, wie in den folgenden Abschnitten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="6443e-1327">Wie bei anderen Membern werden Operatoren, die in einer Basisklasse deklariert werden, von abgeleiteten Klassen geerbt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="6443e-1328">Da Operator Deklarationen immer die Klasse oder Struktur erfordern, in der der Operator deklariert ist, um an der Signatur des Operators teilzunehmen, ist es nicht möglich, dass ein Operator, der in einer abgeleiteten Klasse deklariert ist, einen in einer Basisklasse deklarierten Operator ausblenden kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="6443e-1329">Daher ist der `new` -Modifizierer in einer Operator Deklaration nie erforderlich und daher nie zulässig.</span><span class="sxs-lookup"><span data-stu-id="6443e-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="6443e-1330">Weitere Informationen zu unären und binären Operatoren finden Sie unter [Operatoren](expressions.md#operators).</span><span class="sxs-lookup"><span data-stu-id="6443e-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="6443e-1331">Weitere Informationen zu Konvertierungs Operatoren finden Sie unter [benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions).</span><span class="sxs-lookup"><span data-stu-id="6443e-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="6443e-1332">Unäre Operatoren</span><span class="sxs-lookup"><span data-stu-id="6443e-1332">Unary operators</span></span>

<span data-ttu-id="6443e-1333">Die folgenden Regeln gelten für unäre Operator Deklarationen, `T` wobei den Instanztyp der Klasse oder Struktur bezeichnet, die die Operator Deklaration enthält:</span><span class="sxs-lookup"><span data-stu-id="6443e-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="6443e-1334">Ein unärer `+`Operator `-`, `!`, oder `~` muss einen einzelnen Parameter vom Typ `T` oder `T?` annehmen und kann jeden beliebigen Typ zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="6443e-1335">Ein unärer `++` or `--` -Operator muss einen einzelnen Parameter vom Typ `T` oder `T?` annehmen, und er muss denselben Typ oder einen von ihm abgeleiteten Typ zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="6443e-1336">Ein unärer `true` or `false` -Operator muss einen einzelnen Parameter vom Typ `T` oder `T?` annehmen und muss den `bool`Typ zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="6443e-1337">Die Signatur eines unären Operators besteht aus dem Operator Token (`+`, `-`, `!`, `~`, `++`, `--`, `true`oder `false`) und dem Typ des einzelnen formalen Parameters.</span><span class="sxs-lookup"><span data-stu-id="6443e-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="6443e-1338">Der Rückgabetyp ist weder Teil der Signatur eines unären Operators noch der Name des formalen Parameters.</span><span class="sxs-lookup"><span data-stu-id="6443e-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="6443e-1339">Die `true` unären Operatoren und `false` erfordern eine paarweise Deklaration.</span><span class="sxs-lookup"><span data-stu-id="6443e-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="6443e-1340">Ein Kompilierzeitfehler tritt auf, wenn eine Klasse einen dieser Operatoren deklariert, ohne auch den anderen zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="6443e-1341">Die `true` Operatoren und `false` werden weiter unten in [benutzerdefinierten bedingten logischen Operatoren](expressions.md#user-defined-conditional-logical-operators) und [booleschen Ausdrücken](expressions.md#boolean-expressions)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="6443e-1342">Das folgende Beispiel zeigt eine-Implementierung und die nachfolg `operator ++` Ende Verwendung von für eine ganzzahlige Vektor Klasse:</span><span class="sxs-lookup"><span data-stu-id="6443e-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
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

<span data-ttu-id="6443e-1343">Beachten Sie, dass die Operator-Methode den Wert zurückgibt, der durch das Hinzufügen von 1 zum Operanden erzeugt wird, genau wie die Postfix-Inkrement-und Dekrementoperatoren ([postfix-Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators)) und die Präfix-Inkrement-[ Inkrement-und Dekrementoperatoren](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="6443e-1344">Anders als C++in muss diese Methode den Wert des Operanden nicht direkt ändern.</span><span class="sxs-lookup"><span data-stu-id="6443e-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="6443e-1345">Tatsächlich würde das Ändern des Operanden-Werts gegen die Standard Semantik des Postfix-Inkrementoperators verstoßen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="6443e-1346">Binäre Operatoren</span><span class="sxs-lookup"><span data-stu-id="6443e-1346">Binary operators</span></span>

<span data-ttu-id="6443e-1347">Die folgenden Regeln gelten für binäre Operator Deklarationen, `T` wobei den Instanztyp der Klasse oder Struktur bezeichnet, die die Operator Deklaration enthält:</span><span class="sxs-lookup"><span data-stu-id="6443e-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="6443e-1348">Ein binärer nicht Verschiebungs Operator muss zwei Parameter annehmen, von denen mindestens eine den Typ `T` oder `T?`aufweisen muss und jeden beliebigen Typ zurückgeben kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="6443e-1349">Ein `<<` binärer `>>` or `T?` -Operator muss zwei Parameter annehmen. der erste muss den- `T` Typ aufweisen, und der zweite muss den- `int` Typ `int?`oder aufweisen und jeden beliebigen Typ zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="6443e-1350">Die Signatur`+`eines binären Operators besteht aus dem Operator Token (, `<<` `|` `*` `&` `/` `-`,,, `%`,,, `^`,, `>>`, `==`, ,,`>`, oder`>=`) unddieTypenderbeidenformalenParameter`<=`. `!=` `<`</span><span class="sxs-lookup"><span data-stu-id="6443e-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="6443e-1351">Der Rückgabetyp und die Namen der formalen Parameter sind nicht Teil der Signatur eines binären Operators.</span><span class="sxs-lookup"><span data-stu-id="6443e-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="6443e-1352">Bestimmte binäre Operatoren erfordern eine paarweise Deklaration.</span><span class="sxs-lookup"><span data-stu-id="6443e-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="6443e-1353">Für jede Deklaration eines der beiden Operatoren eines Paares muss eine entsprechende Deklaration des anderen Operators des Paars vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="6443e-1354">Zwei Operator Deklarationen stimmen überein, wenn Sie den gleichen Rückgabetyp und denselben Typ für jeden Parameter aufweisen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="6443e-1355">Die folgenden Operatoren erfordern eine paarweise Deklaration:</span><span class="sxs-lookup"><span data-stu-id="6443e-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="6443e-1356">`operator ==` und `operator !=`</span><span class="sxs-lookup"><span data-stu-id="6443e-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="6443e-1357">`operator >` und `operator <`</span><span class="sxs-lookup"><span data-stu-id="6443e-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="6443e-1358">`operator >=` und `operator <=`</span><span class="sxs-lookup"><span data-stu-id="6443e-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="6443e-1359">Konvertierungsoperatoren</span><span class="sxs-lookup"><span data-stu-id="6443e-1359">Conversion operators</span></span>

<span data-ttu-id="6443e-1360">Eine Konvertierungs Operator Deklaration führt eine ***benutzerdefinierte Konvertierung*** ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)) ein, mit der die vordefinierten und expliziten Konvertierungen erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="6443e-1361">Eine Konvertierungs Operator Deklaration, `implicit` die das-Schlüsselwort enthält, führt eine benutzerdefinierte implizite Konvertierung ein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="6443e-1362">Implizite Konvertierungen können in einer Vielzahl von Situationen auftreten, einschließlich Funktionsmember-Aufrufe, Umwandlungs Ausdrücke und Zuweisungen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="6443e-1363">Dies wird in [implizite Konvertierungen](conversions.md#implicit-conversions)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="6443e-1364">Eine Konvertierungs Operator Deklaration, `explicit` die das-Schlüsselwort enthält, führt eine benutzerdefinierte explizite Konvertierung ein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="6443e-1365">Explizite Konvertierungen können in Umwandlungs Ausdrücken auftreten und werden weiter unten in [expliziten Konvertierungen](conversions.md#explicit-conversions)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="6443e-1366">Ein Konvertierungs Operator konvertiert von einem Quelltyp, der durch den Parametertyp des Konvertierungs Operators angegeben ist, in einen Zieltyp, der durch den Rückgabetyp des Konvertierungs Operators angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="6443e-1367">Verwenden Sie für einen angegebenen `S` Quelltyp und `T`Zieltyp `S` , `T` wenn oder NULL-Werte zulassen `S0` `T0` , `T0` die zugrunde liegenden Typen, und verweisen `S0` Sie andernfalls auf. `S` gleich`T` bzw.</span><span class="sxs-lookup"><span data-stu-id="6443e-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="6443e-1368">Eine Klasse oder Struktur darf nur dann eine Konvertierung von einem Quelltyp `S` in einen Zieltyp `T` deklarieren, wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="6443e-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="6443e-1369">`S0`und `T0` sind unterschiedliche Typen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="6443e-1370">Entweder `S0` oder`T0` ist der Klassen-oder Strukturtyp, in dem die Operator Deklaration stattfindet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="6443e-1371">Weder `S0` noch `T0` ist ein *INTERFACE_TYPE*-Wert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="6443e-1372">Ohne Benutzer `S` definierte Konvertierungen ist eine Konvertierung von zu `T` oder von `T` zu `S`nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="6443e-1373">Bei diesen Regeln werden alle Typparameter, die mit `S` oder `T` verknüpft sind, als eindeutige Typen betrachtet, die keine Vererbungs Beziehung mit anderen Typen aufweisen, und Einschränkungen für diese Typparameter werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="6443e-1374">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="6443e-1375">die ersten beiden Operator Deklarationen sind zulässig, da für `T` [Indexer-Indexer](classes.md#indexers), und `int` `string` bzw. als eindeutige Typen ohne Beziehung angesehen werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="6443e-1376">Der dritte Operator ist jedoch ein Fehler, da `C<T>` die Basisklasse von `D<T>`ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="6443e-1377">Aus der zweiten Regel folgt, dass ein Konvertierungs Operator entweder in oder aus dem Klassen-oder Strukturtyp konvertieren muss, in dem der Operator deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="6443e-1378">Beispielsweise ist es möglich, dass ein Klassen- `C` oder Strukturtyp eine Konvertierung von `C` in `int` und von `int` in `C`, aber nicht von `int` in `bool`definiert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="6443e-1379">Es ist nicht möglich, eine vordefinierte Konvertierung direkt neu zu definieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="6443e-1380">Folglich ist es nicht zulässig, Konvertierungs Operatoren von oder `object` in zu konvertieren, da zwischen `object` und allen anderen Typen bereits implizite und explizite Konvertierungen vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="6443e-1381">Ebenso kann weder die Quelle noch die Zieltypen einer Konvertierung ein Basistyp der anderen sein, da eine Konvertierung dann bereits vorhanden wäre.</span><span class="sxs-lookup"><span data-stu-id="6443e-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="6443e-1382">Es ist jedoch möglich, Operatoren für generische Typen zu deklarieren, die für bestimmte Typargumente Konvertierungen angeben, die bereits als vordefinierte Konvertierungen vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="6443e-1383">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="6443e-1384">Wenn Type `object` als Typargument für `T`angegeben wird, deklariert der zweite Operator eine Konvertierung, die bereits vorhanden ist (ein implizites und somit auch eine explizite Konvertierung von einem beliebigen `object`Typ in den Typ).</span><span class="sxs-lookup"><span data-stu-id="6443e-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="6443e-1385">In Fällen, in denen eine vordefinierte Konvertierung zwischen zwei Typen vorhanden ist, werden alle benutzerdefinierten Konvertierungen zwischen diesen Typen ignoriert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="6443e-1386">Dies gilt insbesondere in folgenden Fällen:</span><span class="sxs-lookup"><span data-stu-id="6443e-1386">Specifically:</span></span>

*  <span data-ttu-id="6443e-1387">Wenn eine vordefinierte implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions) `S` ) vom Typ in den Typ `T`vorhanden ist, werden alle benutzerdefinierten Konvertierungen (implizit oder explizit `T` ) von `S` zu ignoriert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="6443e-1388">Wenn eine vordefinierte explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-conversions) `S` ) vom Typ in den Typ `T`vorhanden ist, werden alle benutzerdefinierten expliziten `T` Konvertierungen von `S` in ignoriert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="6443e-1389">Weiter</span><span class="sxs-lookup"><span data-stu-id="6443e-1389">Furthermore:</span></span>

<span data-ttu-id="6443e-1390">Wenn `T` ein Schnittstellentyp ist, werden benutzerdefinierte implizite Konvertierungen `T` von `S` in ignoriert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="6443e-1391">Andernfalls werden benutzerdefinierte implizite Konvertierungen von `S` in `T` immer noch berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="6443e-1392">Für alle Typen `object`, jedoch verursachen die `Convertible<T>` vom oben genannten Typ deklarierten Operatoren keinen Konflikt mit vordefinierten Konvertierungen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="6443e-1393">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6443e-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="6443e-1394">Für den Typ `object`verbergen vordefinierte Konvertierungen jedoch die benutzerdefinierten Konvertierungen in allen Fällen, aber eine:</span><span class="sxs-lookup"><span data-stu-id="6443e-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="6443e-1395">Benutzerdefinierte Konvertierungen dürfen nicht von oder in *INTERFACE_TYPE*s konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="6443e-1396">Diese Einschränkung stellt insbesondere sicher, dass keine benutzerdefinierten Transformationen beim Konvertieren in ein *INTERFACE_TYPE*auftreten und dass eine Konvertierung in ein *INTERFACE_TYPE* nur erfolgreich ist, wenn das konvertierte Objekt tatsächlich das angegebene *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="6443e-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="6443e-1397">Die Signatur eines Konvertierungs Operators besteht aus dem Quelltyp und dem Zieltyp.</span><span class="sxs-lookup"><span data-stu-id="6443e-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="6443e-1398">(Beachten Sie, dass dies die einzige Form der Member ist, für die der Rückgabetyp an der Signatur teilnimmt.) Die `implicit` - `explicit` oder-Klassifizierung eines Konvertierungs Operators ist nicht Teil der Signatur des Operators.</span><span class="sxs-lookup"><span data-stu-id="6443e-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="6443e-1399">Daher kann eine Klasse oder Struktur nicht sowohl einen `implicit` -als auch einen `explicit` -Konvertierungs Operator mit denselben Quell-und Zieltypen deklarieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="6443e-1400">Im Allgemeinen sollten benutzerdefinierte implizite Konvertierungen so entworfen werden, dass Sie niemals Ausnahmen auslösen und nie Informationen verlieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="6443e-1401">Wenn eine benutzerdefinierte Konvertierung Ausnahmen auslösen kann (z. b. weil das Quell Argument außerhalb des gültigen Bereichs liegt) oder Informationen verloren geht (z. b. das Verwerfen von großen Bits), sollte diese Konvertierung als explizite Konvertierung definiert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="6443e-1402">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1402">In the example</span></span>
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
<span data-ttu-id="6443e-1403">die Konvertierung von `Digit` in `byte` ist implizit, weil Sie niemals Ausnahmen auslöst oder Informationen verliert, aber die Konvertierung `byte` von `Digit` in ist explizit `Digit` , da nur eine Teilmenge der möglichen Werte einer `byte`.</span><span class="sxs-lookup"><span data-stu-id="6443e-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="6443e-1404">Instanzkonstruktoren</span><span class="sxs-lookup"><span data-stu-id="6443e-1404">Instance constructors</span></span>

<span data-ttu-id="6443e-1405">Ein ***Instanzkonstruktor*** ist ein Member, der die erforderlichen Aktionen zum Initialisieren einer Instanz einer Klasse implementiert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="6443e-1406">Instanzkonstruktoren werden mit *constructor_declaration*s deklariert:</span><span class="sxs-lookup"><span data-stu-id="6443e-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

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

<span data-ttu-id="6443e-1407">Ein *constructor_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)), eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)) und einen `extern`-Modifizierer ([Externe Methoden](classes.md#external-methods)) enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="6443e-1408">Eine Konstruktordeklaration darf nicht denselben Modifizierer mehrmals einschließen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="6443e-1409">Der *Bezeichner* eines *constructor_declarator* muss der Klasse, in der der Instanzkonstruktor deklariert ist, einen Namen benennen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="6443e-1410">Wenn ein anderer Name angegeben wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="6443e-1411">Der optionale *formal_parameter_list* eines Instanzkonstruktors unterliegt den gleichen Regeln wie das *formal_parameter_list* einer Methode ([Methoden](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="6443e-1412">Die Liste formaler Parameter definiert die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) eines Instanzkonstruktors und steuert den Prozess, bei dem die Überladungs Auflösung ([Typrückschluss](expressions.md#type-inference)) einen bestimmten Instanzkonstruktor in einem Aufruf auswählt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="6443e-1413">Auf jeden der Typen, auf die in der *formal_parameter_list* eines Instanzkonstruktors verwiesen wird, muss mindestens der Zugriff auf den Konstruktor selbst (Barrierefreiheits[Einschränkungen](basic-concepts.md#accessibility-constraints)) möglich sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6443e-1414">Der optionale *constructor_initializer* -Konstruktor gibt einen anderen Instanzkonstruktor an, der vor der Ausführung der Anweisungen im *constructor_body* dieses Instanzkonstruktors aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="6443e-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="6443e-1415">Dies wird in [konstruktorinitialisierern](classes.md#constructor-initializers)ausführlicher beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="6443e-1416">Wenn eine Konstruktordeklaration einen `extern` Modifizierer enthält, wird der Konstruktor als ***externer Konstruktor***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="6443e-1417">Da eine externe Konstruktordeklaration keine tatsächliche Implementierung bereitstellt, besteht deren *constructor_body* aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="6443e-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="6443e-1418">Für alle anderen Konstruktoren besteht der *constructor_body* aus einem- *Block* , der die Anweisungen zum Initialisieren einer neuen Instanz der-Klasse angibt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="6443e-1419">Dies entspricht exakt dem *Block* einer Instanzmethode mit einem `void` Rückgabetyp ([Methoden Text](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="6443e-1420">Instanzkonstruktoren werden nicht geerbt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="6443e-1421">Folglich hat eine Klasse keine Instanzkonstruktoren, die nicht tatsächlich in der Klasse deklariert sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="6443e-1422">Wenn eine Klasse keine Instanzkonstruktordeklarationen enthält, wird automatisch ein Standardinstanzkonstruktor bereitgestellt ([Standardkonstruktoren](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="6443e-1423">Instanzkonstruktoren werden von *object_creation_expression*s ([Objekt Erstellungs Ausdrücke](expressions.md#object-creation-expressions)) und bis *constructor_initializer*s aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="6443e-1424">Konstruktorinitialisierer</span><span class="sxs-lookup"><span data-stu-id="6443e-1424">Constructor initializers</span></span>

<span data-ttu-id="6443e-1425">Alle Instanzkonstruktoren (außer den Klassen `object`) enthalten implizit einen Aufruf eines anderen Instanzkonstruktors direkt vor dem *constructor_body*-Element.</span><span class="sxs-lookup"><span data-stu-id="6443e-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="6443e-1426">Der implizit aufzurufende Konstruktor wird durch die *constructor_initializer*bestimmt:</span><span class="sxs-lookup"><span data-stu-id="6443e-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="6443e-1427">Ein instanzkonstruktorinitialisierer des Formulars `base(argument_list)` oder `base()` bewirkt, dass ein Instanzkonstruktor von der direkten Basisklasse aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="6443e-1428">Dieser Konstruktor wird mit *argument_list* , sofern vorhanden, und den Regeln zur Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution)ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="6443e-1429">Der Satz von Kandidaten Instanzkonstruktoren besteht aus allen zugänglichen Instanzkonstruktoren, die in der direkten Basisklasse enthalten sind, oder dem Standardkonstruktor ([Standardkonstruktoren](classes.md#default-constructors)), wenn in der direkten Basisklasse keine Instanzkonstruktoren deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="6443e-1430">Wenn dieser Satz leer ist oder ein einzelner Konstruktor mit der besten Instanz nicht identifiziert werden kann, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="6443e-1431">Ein instanzkonstruktorinitialisierer des Formulars `this(argument-list)` oder `this()` bewirkt, dass ein Instanzkonstruktor von der Klasse selbst aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="6443e-1432">Der Konstruktor wird mit *argument_list* , sofern vorhanden, und den Regeln zur Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution)ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="6443e-1433">Der Satz von Kandidaten Instanzkonstruktoren besteht aus allen zugänglichen Instanzkonstruktoren, die in der Klasse selbst deklariert sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="6443e-1434">Wenn dieser Satz leer ist oder ein einzelner Konstruktor mit der besten Instanz nicht identifiziert werden kann, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="6443e-1435">Wenn eine Instanzkonstruktordeklaration einen Konstruktorinitialisierer enthält, der den Konstruktor selbst aufruft, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="6443e-1436">Wenn ein Instanzkonstruktor über keinen Konstruktorinitialisierer verfügt, wird ein Konstruktorinitialisierer `base()` des Formulars implizit bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="6443e-1437">Folglich ist eine Instanzkonstruktordeklaration der Form</span><span class="sxs-lookup"><span data-stu-id="6443e-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="6443e-1438">ist genau Äquivalent zu</span><span class="sxs-lookup"><span data-stu-id="6443e-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="6443e-1439">Der Gültigkeitsbereich der Parameter, die vom *formal_parameter_list* einer Instanzkonstruktordeklaration angegeben werden, beinhaltet den Konstruktorinitialisierer dieser Deklaration.</span><span class="sxs-lookup"><span data-stu-id="6443e-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="6443e-1440">Daher ist es einem Konstruktorinitialisierer gestattet, auf die Parameter des Konstruktors zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="6443e-1441">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6443e-1441">For example:</span></span>
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

<span data-ttu-id="6443e-1442">Ein instanzkonstruktorinitialisierer kann nicht auf die Instanz zugreifen, die erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="6443e-1443">Daher ist es ein Kompilierzeitfehler, der auf `this` in einem Argument Ausdruck des konstruktorinitialisierers verweist, ebenso wie es ein Kompilierzeitfehler für einen Argument Ausdruck ist, der auf ein Instanzmember über eine *Simple_name*verweist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="6443e-1444">Instanzvariableninitialisierer</span><span class="sxs-lookup"><span data-stu-id="6443e-1444">Instance variable initializers</span></span>

<span data-ttu-id="6443e-1445">Wenn ein Instanzkonstruktor über keinen Konstruktorinitialisierer oder einen Konstruktorinitialisierer der Form `base(...)` verfügt, führt dieser Konstruktor implizit die Initialisierungen aus, die von den *variable_initializer*s der Instanzfelder angegeben werden, die in deklariert werden. die Klasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="6443e-1446">Dies entspricht einer Sequenz von Zuweisungen, die unmittelbar nach dem Einstieg in den Konstruktor und vor dem impliziten Aufruf des direkten Basisklassenkonstruktors ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="6443e-1447">Die Variableninitialisierer werden in der Text Reihenfolge ausgeführt, in der Sie in der Klassen Deklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="6443e-1448">Konstruktorausführung</span><span class="sxs-lookup"><span data-stu-id="6443e-1448">Constructor execution</span></span>

<span data-ttu-id="6443e-1449">Variableninitialisierer werden in Zuweisungs Anweisungen transformiert, und diese Zuweisungs Anweisungen werden vor dem Aufruf des Basisklasseninstanzkonstruktors ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="6443e-1450">Diese Reihenfolge stellt sicher, dass alle Instanzfelder durch ihre Variableninitialisierer initialisiert werden, bevor Anweisungen ausgeführt werden, die auf diese Instanz zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="6443e-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="6443e-1451">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1451">Given the example</span></span>
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
<span data-ttu-id="6443e-1452">Wenn `new B()` verwendet wird, um eine Instanz von `B`zu erstellen, wird die folgende Ausgabe erzeugt:</span><span class="sxs-lookup"><span data-stu-id="6443e-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```console
x = 1, y = 0
```

<span data-ttu-id="6443e-1453">Der Wert von `x` ist 1, da der Variableninitialisierer ausgeführt wird, bevor der basisklasseninstanzkonstruktor aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="6443e-1454">Der Wert von `y` ist jedoch 0 (der Standardwert `int`von), da die Zuweisung zu `y` erst ausgeführt wird, nachdem der Basisklassenkonstruktor zurückgegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="6443e-1455">Es ist hilfreich, instanzvariableninitialisierer und Konstruktorinitialisierer als Anweisungen zu betrachten, die vor dem *constructor_body*automatisch eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="6443e-1456">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1456">The example</span></span>
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
<span data-ttu-id="6443e-1457">enthält mehrere Variableninitialisierer. Sie enthält auch Konstruktorinitialisierer von beiden Formularen (`base` und `this`).</span><span class="sxs-lookup"><span data-stu-id="6443e-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="6443e-1458">Das Beispiel entspricht dem unten gezeigten Code, wobei jeder Kommentar eine automatisch eingefügte Anweisung angibt (die Syntax, die für die automatisch eingefügten Konstruktoraufrufe verwendet wird, ist nicht gültig, dient lediglich zur Veranschaulichung des Mechanismus).</span><span class="sxs-lookup"><span data-stu-id="6443e-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

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

### <a name="default-constructors"></a><span data-ttu-id="6443e-1459">Standardkonstruktoren</span><span class="sxs-lookup"><span data-stu-id="6443e-1459">Default constructors</span></span>

<span data-ttu-id="6443e-1460">Wenn eine Klasse keine Instanzkonstruktordeklarationen enthält, wird automatisch ein Standardinstanzkonstruktor bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="6443e-1461">Dieser Standardkonstruktor ruft einfach den Parameter losen Konstruktor der direkten Basisklasse auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="6443e-1462">Wenn die Klasse abstrakt ist, wird die deklarierte Barrierefreiheit für den Standardkonstruktor geschützt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="6443e-1463">Andernfalls ist die deklarierte Barrierefreiheit für den Standardkonstruktor öffentlich.</span><span class="sxs-lookup"><span data-stu-id="6443e-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="6443e-1464">Daher ist der Standardkonstruktor immer das Formular.</span><span class="sxs-lookup"><span data-stu-id="6443e-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="6443e-1465">oder</span><span class="sxs-lookup"><span data-stu-id="6443e-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="6443e-1466">dabei `C` ist der Name der Klasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="6443e-1467">Wenn die Überladungs Auflösung keinen eindeutigen besten Kandidaten für den basisklassenkonstruktorinitialisierer ermitteln kann, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="6443e-1468">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="6443e-1469">ein Standardkonstruktor wird bereitgestellt, da die-Klasse keine Instanzkonstruktordeklarationen enthält.</span><span class="sxs-lookup"><span data-stu-id="6443e-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="6443e-1470">Folglich entspricht das Beispiel genau dem</span><span class="sxs-lookup"><span data-stu-id="6443e-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="6443e-1471">Private Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="6443e-1471">Private constructors</span></span>

<span data-ttu-id="6443e-1472">Wenn eine Klasse `T` nur private Instanzkonstruktoren deklariert, ist es nicht möglich, dass Klassen außerhalb des Programm `T` Texts von `T` oder direkt Instanzen von `T`erstellen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="6443e-1473">Wenn eine Klasse nur statische Member enthält und nicht instanziiert werden soll, wird durch das Hinzufügen eines leeren privaten Instanzkonstruktors eine Instanziierung verhindert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="6443e-1474">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6443e-1474">For example:</span></span>
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

<span data-ttu-id="6443e-1475">Die `Trig` -Klasse gruppiert verwandte Methoden und Konstanten, sollte jedoch nicht instanziiert werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="6443e-1476">Daher wird ein einzelner leerer privater Instanzkonstruktor deklariert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="6443e-1477">Mindestens ein Instanzkonstruktor muss deklariert werden, um die automatische Generierung eines Standardkonstruktors zu unterdrücken.</span><span class="sxs-lookup"><span data-stu-id="6443e-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="6443e-1478">Optionale instanzkonstruktorparameter</span><span class="sxs-lookup"><span data-stu-id="6443e-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="6443e-1479">Die `this(...)` Form des konstruktorinitialisierers wird häufig in Verbindung mit überladen verwendet, um optionale instanzkonstruktorparameter zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="6443e-1480">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1480">In the example</span></span>
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
<span data-ttu-id="6443e-1481">die ersten beiden Instanzkonstruktoren stellen lediglich die Standardwerte für die fehlenden Argumente bereit.</span><span class="sxs-lookup"><span data-stu-id="6443e-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="6443e-1482">Beide verwenden einen `this(...)` Konstruktorinitialisierer, um den dritten Instanzkonstruktor aufzurufen, der tatsächlich die Initialisierung der neuen Instanz bewirkt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="6443e-1483">Der Effekt besteht aus den optionalen Konstruktorparametern:</span><span class="sxs-lookup"><span data-stu-id="6443e-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="6443e-1484">Statische Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="6443e-1484">Static constructors</span></span>

<span data-ttu-id="6443e-1485">Ein ***statischer Konstruktor*** ist ein Member, der die erforderlichen Aktionen zum Initialisieren eines geschlossenen Klassen Typs implementiert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="6443e-1486">Statische Konstruktoren werden mit *static_constructor_declaration*s deklariert:</span><span class="sxs-lookup"><span data-stu-id="6443e-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

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

<span data-ttu-id="6443e-1487">Ein *static_constructor_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)) und einen `extern`-Modifizierer ([Externe Methoden](classes.md#external-methods)) enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="6443e-1488">Der *Bezeichner* eines *static_constructor_declaration* muss der Klasse, in der der statische Konstruktor deklariert ist, einen Namen benennen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="6443e-1489">Wenn ein anderer Name angegeben wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="6443e-1490">Wenn eine statische Konstruktordeklaration einen `extern` Modifizierer enthält, wird als statischer Konstruktor ein ***externer statischer Konstruktor***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="6443e-1491">Da eine externe statische Konstruktordeklaration keine tatsächliche Implementierung bereitstellt, besteht deren *static_constructor_body* aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="6443e-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="6443e-1492">Für alle anderen statischen Konstruktordeklarationen besteht der *static_constructor_body* aus einem- *Block* , der die auszuführenden Anweisungen angibt, um die-Klasse zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="6443e-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="6443e-1493">Dies entspricht exakt dem *method_body* einer statischen Methode mit einem `void`-Rückgabetyp ([Methoden Text](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="6443e-1494">Statische Konstruktoren werden nicht geerbt und können nicht direkt aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="6443e-1495">Der statische Konstruktor für einen geschlossenen Klassentyp wird höchstens einmal in einer bestimmten Anwendungsdomäne ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="6443e-1496">Die Ausführung eines statischen Konstruktors wird ausgelöst, wenn das erste der folgenden Ereignisse in einer Anwendungsdomäne auftritt:</span><span class="sxs-lookup"><span data-stu-id="6443e-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="6443e-1497">Eine Instanz des-Klassen Typs wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="6443e-1498">Auf alle statischen Member des Klassen Typs wird verwiesen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="6443e-1499">Wenn eine Klasse die `Main` Methode ([Anwendungsstart](basic-concepts.md#application-startup)) enthält, in der die Ausführung beginnt, wird der statische Konstruktor für diese Klasse `Main` ausgeführt, bevor die-Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="6443e-1500">Um einen neuen geschlossenen Klassentyp zu initialisieren, wird zuerst ein neuer Satz statischer Felder ([statische Felder und Instanzfelder](classes.md#static-and-instance-fields)) für diesen bestimmten geschlossenen Typ erstellt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="6443e-1501">Jedes der statischen Felder wird mit dem Standardwert initialisiert ([Standardwerte](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="6443e-1502">Als nächstes werden die statischen Feldinitialisierer ([statische Feld Initialisierung](classes.md#static-field-initialization)) für diese statischen Felder ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="6443e-1503">Schließlich wird der statische Konstruktor ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="6443e-1504">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1504">The example</span></span>
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
<span data-ttu-id="6443e-1505">die Ausgabe muss erzeugt werden:</span><span class="sxs-lookup"><span data-stu-id="6443e-1505">must produce the output:</span></span>
```console
Init A
A.F
Init B
B.F
```
<span data-ttu-id="6443e-1506">, da der statische `A`Konstruktor der Ausführung durch den `B`- `B.F`aufzurufenden aufgerufen wird und die Ausführung des statischen Konstruktors durch den-Befehl ausgelöst wird. `A.F`</span><span class="sxs-lookup"><span data-stu-id="6443e-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="6443e-1507">Es ist möglich, zirkuläre Abhängigkeiten zu erstellen, die es ermöglichen, dass statische Felder mit Variableninitialisierern im Standardwert Zustand beobachtet werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="6443e-1508">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1508">The example</span></span>
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
<span data-ttu-id="6443e-1509">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="6443e-1509">produces the output</span></span>
```console
X = 1, Y = 2
```

<span data-ttu-id="6443e-1510">Zum Ausführen der `Main` -Methode führt das System zuerst den Initialisierer für `B.Y`aus, bevor der `B`statische Konstruktor der Klasse ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="6443e-1511">`Y`der Initialisierer bewirkt `A`, dass der statische Konstruktor ausgeführt wird, da auf `A.X` den Wert von verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="6443e-1512">Der statische Konstruktor von `A` führt wiederum den Wert von `X`aus und ruft dabei den Standardwert von `Y`ab, der 0 (null) ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="6443e-1513">`A.X`wird daher mit 1 initialisiert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="6443e-1514">Der Prozess der Ausführung `A`der statischen Feldinitialisierer und des statischen Konstruktors wird dann abgeschlossen, wobei die Berechnung des Anfangs `Y`Werts von zurückgegeben wird. Dadurch wird das Ergebnis 2.</span><span class="sxs-lookup"><span data-stu-id="6443e-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="6443e-1515">Da der statische Konstruktor für jeden geschlossenen konstruierten Klassentyp genau einmal ausgeführt wird, können Sie Laufzeitüberprüfungen für den Typparameter erzwingen, der zur Kompilierzeit nicht über Einschränkungen ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) überprüft werden kann. .</span><span class="sxs-lookup"><span data-stu-id="6443e-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="6443e-1516">Der folgende Typ verwendet beispielsweise einen statischen Konstruktor, um zu erzwingen, dass das Typargument eine-Enum ist:</span><span class="sxs-lookup"><span data-stu-id="6443e-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
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

## <a name="destructors"></a><span data-ttu-id="6443e-1517">Destruktoren</span><span class="sxs-lookup"><span data-stu-id="6443e-1517">Destructors</span></span>

<span data-ttu-id="6443e-1518">Ein ***destrukturtor*** ist ein Member, der die erforderlichen Aktionen zum Zerstörung einer Instanz einer Klasse implementiert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="6443e-1519">Ein Dekonstruktor wird mit einem *destructor_declaration*deklariert:</span><span class="sxs-lookup"><span data-stu-id="6443e-1519">A destructor is declared using a *destructor_declaration*:</span></span>

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

<span data-ttu-id="6443e-1520">Ein *destructor_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)) enthalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="6443e-1521">Der *Bezeichner* eines *destructor_declaration* muss der Klasse, in der der Dekonstruktor deklariert ist, einen Namen benennen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="6443e-1522">Wenn ein anderer Name angegeben wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="6443e-1523">Wenn eine dekonstruktordeklaration `extern` einen Modifizierer enthält, wird der Dekonstruktor als ***externer Dekonstruktor***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="6443e-1524">Da eine externe dekonstruktordeklaration keine tatsächliche Implementierung bereitstellt, besteht deren *destructor_body* aus einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="6443e-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="6443e-1525">Für alle anderen destrukturtoren besteht der *destructor_body* aus einem- *Block* , der die auszuführenden Anweisungen angibt, um eine Instanz der-Klasse zu Zerstörung.</span><span class="sxs-lookup"><span data-stu-id="6443e-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="6443e-1526">Ein *destructor_body* entspricht exakt dem *method_body* einer Instanzmethode mit einem `void`-Rückgabetyp ([Methoden Text](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="6443e-1527">Deerdektoren werden nicht geerbt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1527">Destructors are not inherited.</span></span> <span data-ttu-id="6443e-1528">Folglich hat eine Klasse keine Dekonstruktoren, die nicht die Dekonstruktoren aufweisen, die in dieser Klasse deklariert werden können.</span><span class="sxs-lookup"><span data-stu-id="6443e-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="6443e-1529">Da für einen Dekonstruktor keine Parameter erforderlich sind, kann er nicht überladen werden, sodass eine Klasse höchstens einen Dekonstruktor aufweisen kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="6443e-1530">Dededektoren werden automatisch aufgerufen und können nicht explizit aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="6443e-1531">Eine Instanz ist für die Zerstörung infrage, wenn es für keinen Code mehr möglich ist, diese Instanz zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="6443e-1532">Die Ausführung des Dekonstruktors für die Instanz kann zu einem beliebigen Zeitpunkt erfolgen, nachdem die Instanz für eine Zerstörung berechtigt ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="6443e-1533">Wenn eine Instanz von zerstört wird, werden die Dekonstruktoren in der Vererbungs Kette dieser Instanz in der richtigen Reihenfolge von der am wenigsten abgeleiteten zum geringsten abgeleiteten aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="6443e-1534">Ein Dekonstruktor kann in jedem Thread ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="6443e-1535">Weitere Informationen zu den Regeln, die bestimmen, wann und wie ein Dekonstruktor ausgeführt wird, finden Sie unter [Automatische Speicherverwaltung](basic-concepts.md#automatic-memory-management).</span><span class="sxs-lookup"><span data-stu-id="6443e-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="6443e-1536">Die Ausgabe des Beispiels</span><span class="sxs-lookup"><span data-stu-id="6443e-1536">The output of the example</span></span>
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
<span data-ttu-id="6443e-1537">echt</span><span class="sxs-lookup"><span data-stu-id="6443e-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="6443e-1538">Da deerdektoren in einer Vererbungs Kette in der Reihenfolge von den meisten abgeleiteten zu den am wenigsten abgeleiteten</span><span class="sxs-lookup"><span data-stu-id="6443e-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="6443e-1539">Dededektoren werden implementiert, indem die `Finalize` virtuelle `System.Object`Methode für überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="6443e-1540">C#Es ist nicht zulässig, dass Programme diese Methode außer Kraft setzen oder Sie direkt (bzw. über schreibungen) direkt aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="6443e-1541">Beispielsweise wird das Programm</span><span class="sxs-lookup"><span data-stu-id="6443e-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="6443e-1542">enthält zwei Fehler.</span><span class="sxs-lookup"><span data-stu-id="6443e-1542">contains two errors.</span></span>

<span data-ttu-id="6443e-1543">Der Compiler verhält sich so, als ob diese Methode und über schreibungen davon überhaupt nicht vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="6443e-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="6443e-1544">Dieses Programm ist also:</span><span class="sxs-lookup"><span data-stu-id="6443e-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="6443e-1545">ist gültig, und die `System.Object`-Methode hat die- `Finalize` Methode ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="6443e-1546">Eine Erläuterung des Verhaltens, wenn eine Ausnahme von einem debugtor ausgelöst wird, finden Sie unter [so werden Ausnahmen behandelt](exceptions.md#how-exceptions-are-handled).</span><span class="sxs-lookup"><span data-stu-id="6443e-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="6443e-1547">Iterators</span><span class="sxs-lookup"><span data-stu-id="6443e-1547">Iterators</span></span>

<span data-ttu-id="6443e-1548">Ein Funktionsmember ([Funktionsmember](expressions.md#function-members)), der mit einem Iteratorblock ([Blocks](statements.md#blocks)) implementiert wird, wird als ***Iterator***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="6443e-1549">Ein Iteratorblock kann als Text eines Funktionsmembers verwendet werden, solange der Rückgabetyp des entsprechenden Funktionsmembers eine der Enumeratorschnittstellen ([Enumeratorschnittstellen](classes.md#enumerator-interfaces)) oder eine der Aufzähl Bare-Schnittstellen ([Aufzähl Bare Schnitt](classes.md#enumerable-interfaces)stellen) ist. .</span><span class="sxs-lookup"><span data-stu-id="6443e-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="6443e-1550">Sie kann als *method_body*, *operator_body* oder *accessor_body*auftreten, wohingegen Ereignisse, Instanzkonstruktoren, statische Konstruktoren und Dekonstruktoren nicht als Iteratoren implementiert werden können.</span><span class="sxs-lookup"><span data-stu-id="6443e-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="6443e-1551">Wenn ein Funktionsmember mit einem Iteratorblock implementiert wird, ist dies ein Kompilierzeitfehler für die Liste formaler Parameter des Funktionsmembers, um `ref` beliebige `out` -oder-Parameter anzugeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="6443e-1552">Enumeratorschnittstellen</span><span class="sxs-lookup"><span data-stu-id="6443e-1552">Enumerator interfaces</span></span>

<span data-ttu-id="6443e-1553">Die ***Enumeratorschnittstellen*** sind die nicht generische Schnittstelle `System.Collections.IEnumerator` und alle Instanziierungen der generischen- `System.Collections.Generic.IEnumerator<T>`Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="6443e-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="6443e-1554">Aus Gründen der Kürze werden diese Schnittstellen in diesem Kapitel als `IEnumerator` `IEnumerator<T>`bzw. referenziert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="6443e-1555">Enumerable-Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="6443e-1555">Enumerable interfaces</span></span>

<span data-ttu-id="6443e-1556">Die ***Aufzähl Bare-Schnittstellen*** sind die nicht generische `System.Collections.IEnumerable` Schnittstelle und alle Instanziierungen der generischen-Schnittstelle. `System.Collections.Generic.IEnumerable<T>`</span><span class="sxs-lookup"><span data-stu-id="6443e-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="6443e-1557">Aus Gründen der Kürze werden diese Schnittstellen in diesem Kapitel als `IEnumerable` `IEnumerable<T>`bzw. referenziert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="6443e-1558">Yield-Typ</span><span class="sxs-lookup"><span data-stu-id="6443e-1558">Yield type</span></span>

<span data-ttu-id="6443e-1559">Ein Iterator erzeugt eine Sequenz von Werten, die alle denselben Typ haben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="6443e-1560">Dieser Typ wird als ***Yield-Typ*** des Iterators bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="6443e-1561">Der Yield-Typ eines Iterators, der `IEnumerator` oder `IEnumerable` zurück `object`gibt, ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="6443e-1562">Der Yield-Typ eines Iterators, der `IEnumerator<T>` oder `IEnumerable<T>` zurück `T`gibt, ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="6443e-1563">Enumeratorobjekte</span><span class="sxs-lookup"><span data-stu-id="6443e-1563">Enumerator objects</span></span>

<span data-ttu-id="6443e-1564">Wenn ein Funktionsmember, der einen enumeratorschnittstellentyp zurückgibt, mit einem Iteratorblock implementiert wird, führt der Aufruf des Funktionsmembers den Code nicht sofort im Iteratorblock aus.</span><span class="sxs-lookup"><span data-stu-id="6443e-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="6443e-1565">Stattdessen wird ein ***Enumeratorobjekt*** erstellt und zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="6443e-1566">Dieses Objekt kapselt den im Iteratorblock angegebenen Code, und die Ausführung des Codes im Iteratorblock tritt auf, wenn die- `MoveNext` Methode des Enumeratorobjekts aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="6443e-1567">Ein Enumeratorobjekt weist die folgenden Eigenschaften auf:</span><span class="sxs-lookup"><span data-stu-id="6443e-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="6443e-1568">Es implementiert `IEnumerator` und `IEnumerator<T>`, wobei `T` der Yield-Typ des Iterators ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="6443e-1569">Sie implementiert `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="6443e-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="6443e-1570">Sie wird mit einer Kopie der Argument Werte (sofern vorhanden) und dem Instanzwert initialisiert, der an das Funktionsmember übermittelt wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="6443e-1571">Sie verfügt über vier mögliche Zustände: ***vor***, wird ***ausgeführt*** ***, angeh***alten und ***nach***, und befindet sich anfänglich im Zustand " ***vor*** ".</span><span class="sxs-lookup"><span data-stu-id="6443e-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="6443e-1572">Bei einem Enumeratorobjekt handelt es sich in der Regel um eine Instanz einer vom Compiler generierten Enumeratorklasse, die den Code im Iteratorblock kapselt und die Enumeratorschnittstellen implementiert, aber andere Implementierungs Methoden sind möglich.</span><span class="sxs-lookup"><span data-stu-id="6443e-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="6443e-1573">Wenn eine Enumeratorklasse vom Compiler generiert wird, wird diese Klasse direkt oder indirekt in der Klasse, die den Funktionsmember enthält, in die-Klasse eingefügt, Sie verfügt über private zugreif barkeit und hat einen Namen, der fürdie Verwendung durch den Compiler reserviert ist ([Identifier](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="6443e-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="6443e-1574">Ein Enumeratorobjekt kann mehr Schnittstellen implementieren, als die oben genannten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="6443e-1575">In den folgenden `MoveNext`Abschnitten wird das genaue Verhalten der `IEnumerable` - `Current`,- `Dispose` und-Member der `IEnumerable<T>` -und-Schnittstellen Implementierungen beschrieben, die von einem Enumeratorobjekt bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="6443e-1576">Beachten Sie, dass Enumeratorobjekte die `IEnumerator.Reset` -Methode nicht unterstützen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="6443e-1577">Das Aufrufen dieser Methode bewirkt `System.NotSupportedException` , dass eine ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="6443e-1578">Die Methode "Design ext"</span><span class="sxs-lookup"><span data-stu-id="6443e-1578">The MoveNext method</span></span>

<span data-ttu-id="6443e-1579">Die `MoveNext` -Methode eines Enumeratorobjekts kapselt den Code eines Iteratorblocks.</span><span class="sxs-lookup"><span data-stu-id="6443e-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="6443e-1580">Durch Aufrufen `MoveNext` der-Methode wird Code im Iteratorblock ausgeführt, `Current` und die-Eigenschaft des Enumeratorobjekts wird entsprechend festgelegt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="6443e-1581">Die genaue Aktion, die `MoveNext` von ausgeführt wird, hängt vom Status des Enumeratorobjekts ab, wenn `MoveNext` aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="6443e-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="6443e-1582">Wenn der Status des Enumeratorobjekts ***vor***ist, wird aufgerufen `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="6443e-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="6443e-1583">Ändert den Status in wird ***ausgeführt***.</span><span class="sxs-lookup"><span data-stu-id="6443e-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="6443e-1584">Initialisiert die Parameter (einschließlich `this`) des Iteratorblocks mit den Argument Werten und dem Instanzwert, die beim Initialisieren des Enumeratorobjekts gespeichert wurden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="6443e-1585">Führt den Iteratorblock von dem Anfang aus, bis die Ausführung unterbrochen wird (wie unten beschrieben).</span><span class="sxs-lookup"><span data-stu-id="6443e-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="6443e-1586">Wenn der Status des Enumeratorobjekts ***ausgeführt***wird, `MoveNext` ist das Ergebnis des Aufrufs nicht angegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="6443e-1587">Wenn der Status des Enumeratorobjekts angehalten ***wird,*** wird `MoveNext`aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="6443e-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="6443e-1588">Ändert den Status in wird ***ausgeführt***.</span><span class="sxs-lookup"><span data-stu-id="6443e-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="6443e-1589">Stellt die Werte aller lokalen Variablen und Parameter (einschließlich dieser) für die Werte wieder her, die bei der letzten Ausführung des Iteratorblocks gespeichert wurden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="6443e-1590">Beachten Sie, dass sich der Inhalt aller Objekte, auf die von diesen Variablen verwiesen wird, seit dem vorherigen-Befehl von "muvenext" geändert hat.</span><span class="sxs-lookup"><span data-stu-id="6443e-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="6443e-1591">Nimmt die Ausführung des Iteratorblocks unmittelbar nach der `yield return` Anweisung an, die die Unterbrechung der Ausführung verursacht hat, und wird fortgesetzt, bis die Ausführung unterbrochen wurde (wie unten beschrieben).</span><span class="sxs-lookup"><span data-stu-id="6443e-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="6443e-1592">Wenn der Zustand des Enumeratorobjekts ***nach***ist, wird der `MoveNext` Aufruf `false`von zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="6443e-1593">Wenn `MoveNext` den Iteratorblock ausführt, kann die Ausführung auf vier Arten unterbrochen werden: Durch eine `yield return` -Anweisung durch eine `yield break` -Anweisung, durch die das Ende des Iteratorblocks und durch eine Ausnahme ausgelöst und aus dem Iteratorblock weitergegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="6443e-1594">Wenn eine `yield return` -Anweisung gefunden wird ([die yield-Anweisung](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="6443e-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="6443e-1595">Der in der-Anweisung angegebene Ausdruck wird ausgewertet, implizit in den Yield-Typ konvertiert und der `Current` -Eigenschaft des Enumeratorobjekts zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="6443e-1596">Die Ausführung des iteratortexts wurde angehalten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="6443e-1597">Die Werte aller lokalen Variablen und Parameter (einschließlich `this`) werden gespeichert, ebenso wie der Speicherort dieser `yield return` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="6443e-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="6443e-1598">Wenn sich `yield return` die-Anweisung innerhalb eines oder `try` mehrerer Blöcke befindet, `finally` werden die zugeordneten Blöcke zurzeit nicht ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="6443e-1599">Der Status des Enumeratorobjekts ***wird in "*** angehalten" geändert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="6443e-1600">Die `MoveNext` -Methode `true` kehrt an ihren Aufrufer zurück und gibt an, dass die Iterationen erfolgreich auf den nächsten Wert erweitert wurden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="6443e-1601">Wenn eine `yield break` -Anweisung gefunden wird ([die yield-Anweisung](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="6443e-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="6443e-1602">Wenn die `yield break` Anweisung innerhalb eines oder mehrerer `try` Blöcke liegt, werden die `finally` zugeordneten Blöcke ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="6443e-1603">Der Status des Enumeratorobjekts wird in ***nach***geändert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="6443e-1604">Die `MoveNext` Methode kehrt `false` an ihren Aufrufer zurück und gibt an, dass die Iterations Funktion vollständig ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="6443e-1605">Wenn das Ende des iteratortexts erreicht ist:</span><span class="sxs-lookup"><span data-stu-id="6443e-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="6443e-1606">Der Status des Enumeratorobjekts wird in ***nach***geändert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="6443e-1607">Die `MoveNext` Methode kehrt `false` an ihren Aufrufer zurück und gibt an, dass die Iterations Funktion vollständig ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="6443e-1608">Wenn eine Ausnahme ausgelöst und aus dem Iteratorblock weitergegeben wird:</span><span class="sxs-lookup"><span data-stu-id="6443e-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="6443e-1609">Die `finally` entsprechenden Blöcke im iteratortext werden von der Ausnahme Weitergabe ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="6443e-1610">Der Status des Enumeratorobjekts wird in ***nach***geändert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="6443e-1611">Die Ausnahme Weitergabe wird an den Aufrufer der `MoveNext` -Methode weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="6443e-1612">Die aktuelle Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="6443e-1612">The Current property</span></span>

<span data-ttu-id="6443e-1613">Die-Eigenschaft eines Enumeratorobjekts wirkt sich `yield return` auf- `Current` Anweisungen im Iteratorblock aus.</span><span class="sxs-lookup"><span data-stu-id="6443e-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="6443e-1614">Wenn sich ein Enumeratorobjekt im angehaltenen Zustand befindet, ist `Current` der Wert von der Wert, der durch `MoveNext`den vorherigen-Befehl festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="6443e-1615">Wenn sich ein Enumeratorobjekt in den Zuständen " ***before***", " ***Running***" oder " ***after*** " `Current` befindet, ist das Ergebnis des Zugriffs nicht angegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="6443e-1616">Bei einem Iterator mit einem anderen Yield-Typ `object`als entspricht das Ergebnis des `Current` Zugriffs auf die- `IEnumerable` Implementierung des Enumeratorobjekts dem `Current` Zugriff über das- `IEnumerator<T>` Enumeratorobjekt. Implementierung und Umwandeln des Ergebnisses in `object`.</span><span class="sxs-lookup"><span data-stu-id="6443e-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="6443e-1617">Die verwerfen-Methode</span><span class="sxs-lookup"><span data-stu-id="6443e-1617">The Dispose method</span></span>

<span data-ttu-id="6443e-1618">Die `Dispose` -Methode wird verwendet, um die Iterationen zu bereinigen, indem das Enumeratorobjekt in den ***after*** -Zustand versetzt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="6443e-1619">Wenn der Status des Enumeratorobjekts ***vorher***ist, ändert der `Dispose` Aufruf von den Zustand in ***nach***.</span><span class="sxs-lookup"><span data-stu-id="6443e-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="6443e-1620">Wenn der Status des Enumeratorobjekts ***ausgeführt***wird, `Dispose` ist das Ergebnis des Aufrufs nicht angegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="6443e-1621">Wenn der Status des Enumeratorobjekts angehalten ***wird,*** wird `Dispose`aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="6443e-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="6443e-1622">Ändert den Status in wird ***ausgeführt***.</span><span class="sxs-lookup"><span data-stu-id="6443e-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="6443e-1623">Führt beliebige letzte Blöcke aus, als wäre die `yield return` Letzte ausgeführte `yield break` Anweisung eine-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="6443e-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="6443e-1624">Wenn dies bewirkt, dass eine Ausnahme ausgelöst und aus dem iteratortext weitergegeben wird, wird der Status des Enumeratorobjekts auf ***after*** festgelegt, und die Ausnahme wird an den Aufrufer der `Dispose` Methode weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="6443e-1625">Ändert den Zustand in ***nach***.</span><span class="sxs-lookup"><span data-stu-id="6443e-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="6443e-1626">Wenn der Zustand des Enumeratorobjekts ***nach***ist, hat der `Dispose` Aufruf von keine Auswirkungen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="6443e-1627">Enumerable-Objekte</span><span class="sxs-lookup"><span data-stu-id="6443e-1627">Enumerable objects</span></span>

<span data-ttu-id="6443e-1628">Wenn ein Funktionsmember, der einen Aufzähl baren Schnittstellentyp zurückgibt, mit einem Iteratorblock implementiert wird, führt der Aufruf des Funktionsmembers den Code nicht sofort im Iteratorblock aus.</span><span class="sxs-lookup"><span data-stu-id="6443e-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="6443e-1629">Stattdessen wird ein ***Aufzähl bares Objekt*** erstellt und zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="6443e-1630">Die- `GetEnumerator` Methode des Aufzähl Bare-Objekts gibt ein Enumeratorobjekt zurück, das den im Iteratorblock angegebenen Code kapselt, und die Ausführung des Codes im Iteratorblock tritt auf, wenn die- `MoveNext` Methode des Enumeratorobjekts aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="6443e-1631">Ein Aufzähl Bare-Objekt hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="6443e-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="6443e-1632">Es implementiert `IEnumerable` und `IEnumerable<T>`, wobei `T` der Yield-Typ des Iterators ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="6443e-1633">Sie wird mit einer Kopie der Argument Werte (sofern vorhanden) und dem Instanzwert initialisiert, der an das Funktionsmember übermittelt wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="6443e-1634">Ein Aufzähl Bare-Objekt ist in der Regel eine Instanz einer vom Compiler generierten Aufzähl Bare-Klasse, die den Code im Iteratorblock kapselt und die Aufzähl Bare-Schnittstellen implementiert, aber andere Implementierungs Methoden sind möglich.</span><span class="sxs-lookup"><span data-stu-id="6443e-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="6443e-1635">Wenn eine Aufzähl Bare-Klasse vom Compiler generiert wird, wird diese Klasse direkt oder indirekt in der-Klasse, die den Funktionsmember enthält, in eine private Barrierefreiheit eingefügt, und Sie erhält einen Namen, der für die Verwendung durch den[Compiler (](lexical-structure.md#identifiers)Bezeichner) reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="6443e-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="6443e-1636">Ein Aufzähl Bare-Objekt kann mehr Schnittstellen implementieren, als die oben genannten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="6443e-1637">Insbesondere kann ein Aufzähl Bare-Objekt auch und `IEnumerator` `IEnumerator<T>`implementieren, sodass es sowohl als Aufzähl Bare-als auch als Enumerator fungieren kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="6443e-1638">Bei diesem Implementierungstyp wird das Aufzähl Bare Objekt selbst zurückgegeben `GetEnumerator` , wenn die-Methode eines Aufzähl Bare-Objekts zum ersten Mal aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="6443e-1639">Nachfolgende Aufrufe des Aufzähl baren Objekts `GetEnumerator`geben, falls vorhanden, eine Kopie des Aufzähl Bare-Objekts zurück.</span><span class="sxs-lookup"><span data-stu-id="6443e-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="6443e-1640">Folglich hat jeder zurückgegebene Enumerator seinen eigenen Zustand, und Änderungen in einem Enumerator haben keine Auswirkung auf einen anderen Enumerator.</span><span class="sxs-lookup"><span data-stu-id="6443e-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="6443e-1641">Die getenreerator-Methode</span><span class="sxs-lookup"><span data-stu-id="6443e-1641">The GetEnumerator method</span></span>

<span data-ttu-id="6443e-1642">Ein Aufzähl Bare `IEnumerable` -Objekt stellt eine Implementierung der `GetEnumerator` Methoden der-Schnitt `IEnumerable<T>` Stelle und der-Schnittstelle bereit.</span><span class="sxs-lookup"><span data-stu-id="6443e-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="6443e-1643">Die beiden `GetEnumerator` Methoden verwenden eine gemeinsame-Implementierung, die ein verfügbares Enumeratorobjekt abruft und zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="6443e-1644">Das Enumeratorobjekt wird mit den Argument Werten und dem Instanzwert initialisiert, die gespeichert wurden, als das Aufzähl Bare Objekt initialisiert wurde. andernfalls ist das Enumeratorobjekt wie in [enumeratorobjekten](classes.md#enumerator-objects)beschrieben funktionsfähig.</span><span class="sxs-lookup"><span data-stu-id="6443e-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="6443e-1645">Implementierungs Beispiel</span><span class="sxs-lookup"><span data-stu-id="6443e-1645">Implementation example</span></span>

<span data-ttu-id="6443e-1646">In diesem Abschnitt wird eine mögliche Implementierung von Iteratoren in Bezug auf C# standardkonstrukte beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="6443e-1647">Die hier beschriebene Implementierung basiert auf denselben Prinzipien, die vom Microsoft C# -Compiler verwendet werden. Dies bedeutet jedoch nicht, dass es sich um eine vorgeschriebene Implementierung oder die einzige Möglichkeit handelt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="6443e-1648">Die folgende `Stack<T>` Klasse `GetEnumerator` implementiert die-Methode mit einem Iterator.</span><span class="sxs-lookup"><span data-stu-id="6443e-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="6443e-1649">Der Iterator listet die Elemente des Stapels in der Reihenfolge von oben nach unten auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

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

<span data-ttu-id="6443e-1650">Die `GetEnumerator` -Methode kann in eine Instanziierung einer vom Compiler generierten Enumeratorklasse übersetzt werden, die den Code im Iteratorblock kapselt, wie im folgenden gezeigt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="6443e-1651">In der obigen Übersetzung wird der Code im Iteratorblock in einen Zustands Automat umgewandelt und in die `MoveNext` -Methode der Enumeratorklasse eingefügt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="6443e-1652">Außerdem wird die lokale Variable `i` in ein Feld im Enumeratorobjekt umgewandelt, sodass Sie weiterhin über Aufrufe von `MoveNext`vorhanden sein kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="6443e-1653">Im folgenden Beispiel wird eine einfache Multiplikationstabelle der ganzen Zahlen 1 bis 10 gedruckt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="6443e-1654">Die `FromTo` -Methode im Beispiel gibt ein Aufzähl bares Objekt zurück und wird mit einem Iterator implementiert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

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

<span data-ttu-id="6443e-1655">Die `FromTo` -Methode kann in eine Instanziierung einer vom Compiler generierten Aufzähl Bare-Klasse übersetzt werden, die den Code im Iteratorblock kapselt, wie im folgenden gezeigt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="6443e-1656">Die Aufzähl Bare-Klasse implementiert sowohl die Aufzähl Bare-Schnittstelle als auch die Enumeratorschnittstellen, sodass Sie sowohl als Aufzähl Bare-als auch als Enumerator fungieren kann.</span><span class="sxs-lookup"><span data-stu-id="6443e-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="6443e-1657">Wenn die- `GetEnumerator` Methode zum ersten Mal aufgerufen wird, wird das Aufzähl Bare-Objekt selbst zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="6443e-1658">Nachfolgende Aufrufe des Aufzähl baren Objekts `GetEnumerator`geben, falls vorhanden, eine Kopie des Aufzähl Bare-Objekts zurück.</span><span class="sxs-lookup"><span data-stu-id="6443e-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="6443e-1659">Folglich hat jeder zurückgegebene Enumerator seinen eigenen Zustand, und Änderungen in einem Enumerator haben keine Auswirkung auf einen anderen Enumerator.</span><span class="sxs-lookup"><span data-stu-id="6443e-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="6443e-1660">Die `Interlocked.CompareExchange` -Methode wird verwendet, um einen Thread sicheren Vorgang sicherzustellen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="6443e-1661">Der `from` - `to` Parameter und der-Parameter werden in Felder in der Aufzähl Bare-Klasse umgewandelt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="6443e-1662">Da `from` im Iteratorblock geändert wird, wird ein zusätzliches `__from` Feld eingeführt, das den Anfangswert enthält, der `from` für jeden Enumerator angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="6443e-1663">Die `MoveNext` Methode löst eine `InvalidOperationException` aus, wenn `0`Sie aufgerufen `__state` wird, wenn den Wert hat.</span><span class="sxs-lookup"><span data-stu-id="6443e-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="6443e-1664">Dadurch wird verhindert, dass das Aufzähl Bare Objekt als Enumeratorobjekt verwendet wird, ohne `GetEnumerator`dass zuerst aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="6443e-1665">Das folgende Beispiel zeigt eine einfache Strukturklasse.</span><span class="sxs-lookup"><span data-stu-id="6443e-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="6443e-1666">Die `Tree<T>` -Klasse `GetEnumerator` implementiert die-Methode mit einem Iterator.</span><span class="sxs-lookup"><span data-stu-id="6443e-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="6443e-1667">Der Iterator listet die Elemente der Struktur in der Infix-Reihenfolge auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

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

<span data-ttu-id="6443e-1668">Die `GetEnumerator` -Methode kann in eine Instanziierung einer vom Compiler generierten Enumeratorklasse übersetzt werden, die den Code im Iteratorblock kapselt, wie im folgenden gezeigt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="6443e-1669">Die vom Compiler generierten temporare, die `foreach` in den-Anweisungen verwendet `__left` werden `__right` , werden in die Felder und des Enumeratorobjekts gehoben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="6443e-1670">Das `__state` -Feld des Enumeratorobjekts wird sorgfältig aktualisiert, sodass die richtige `Dispose()` Methode ordnungsgemäß aufgerufen wird, wenn eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="6443e-1671">Beachten Sie, dass es nicht möglich ist, den übersetzten Code `foreach` mit einfachen Anweisungen zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="6443e-1672">Async-Funktionen</span><span class="sxs-lookup"><span data-stu-id="6443e-1672">Async functions</span></span>

<span data-ttu-id="6443e-1673">Eine Methode ([Methoden](classes.md#methods)) oder eine anonyme Funktion ([Anonyme Funktions Ausdrücke](expressions.md#anonymous-function-expressions)) mit `async` dem-Modifizierer wird als ***Async-Funktion***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="6443e-1674">Im Allgemeinen wird der Begriff ***Async*** verwendet, um jede Art von Funktion zu beschreiben, die `async` über den-Modifizierer verfügt.</span><span class="sxs-lookup"><span data-stu-id="6443e-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="6443e-1675">Es handelt sich um einen Kompilierzeitfehler für die Liste formaler Parameter einer Async-Funktion, `ref` um `out` beliebige-oder-Parameter anzugeben.</span><span class="sxs-lookup"><span data-stu-id="6443e-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="6443e-1676">Der *return_type* einer Async-Methode muss entweder `void` oder ein ***Tasktyp***sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="6443e-1677">Die Aufgaben Typen sind `System.Threading.Tasks.Task` -und-Typen `System.Threading.Tasks.Task<T>`, die aus erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="6443e-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="6443e-1678">Der Kürze halber wird in diesem Kapitel auf diese Typen als `Task` `Task<T>`bzw. verwiesen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="6443e-1679">Eine Async-Methode, die einen Tasktyp zurückgibt, wird als Aufgaben Rückgabe bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="6443e-1680">Die genaue Definition der Aufgaben Typen ist implementiert, aber aus der Sicht der Sprache befindet sich ein Aufgabentyp in einem der Zustände unvollständig, erfolgreich oder fehlerhaft.</span><span class="sxs-lookup"><span data-stu-id="6443e-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="6443e-1681">Eine fehlerhafte Aufgabe zeichnet eine relevante Ausnahme auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="6443e-1682">Ein erfolgreicher Datensatz `T` zeichneteinErgebnis`Task<T>` des Typs auf.</span><span class="sxs-lookup"><span data-stu-id="6443e-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="6443e-1683">Aufgaben Typen sind möglich und können daher die Operanden von Erwartungs Ausdrücken (Erwartungs[Ausdrücke](expressions.md#await-expressions)) sein.</span><span class="sxs-lookup"><span data-stu-id="6443e-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="6443e-1684">Ein Async-Funktionsaufruf bietet die Möglichkeit zum Aussetzen der Auswertung mithilfe von Erwartungs Ausdrücken (Erwartungs[Ausdrücke](expressions.md#await-expressions)) im Textkörper.</span><span class="sxs-lookup"><span data-stu-id="6443e-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="6443e-1685">Die Auswertung kann später an dem Punkt fortgesetzt werden, an dem der aufrufende Ausdruck mithilfe eines ***Wiederaufnahme***Delegaten fortgesetzt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="6443e-1686">Der Wiederaufnahme Delegat ist `System.Action`vom Typ, und wenn er aufgerufen wird, wird die Auswertung des asynchronen Funktions aufrutens aus dem Erwartungs Ausdruck fortgesetzt, an dem er unterbrochen wurde.</span><span class="sxs-lookup"><span data-stu-id="6443e-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="6443e-1687">Der ***aktuelle*** Aufrufer eines Async-Funktions Aufrufers ist der ursprüngliche Aufrufer, wenn der Funktionsaufruf nie angehalten wurde, andernfalls der letzte Aufrufer des Wiederaufnahme Delegaten.</span><span class="sxs-lookup"><span data-stu-id="6443e-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="6443e-1688">Auswertung einer Aufgaben Rückgabe Async-Funktion</span><span class="sxs-lookup"><span data-stu-id="6443e-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="6443e-1689">Durch den Aufruf einer Aufgaben Rückgabe Async-Funktion wird eine Instanz des zurückgegebenen Aufgaben Typs generiert.</span><span class="sxs-lookup"><span data-stu-id="6443e-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="6443e-1690">Dies wird als ***Rückgabe Task*** der Async-Funktion bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6443e-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="6443e-1691">Der Task befindet sich anfänglich in einem unvollständigen Zustand.</span><span class="sxs-lookup"><span data-stu-id="6443e-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="6443e-1692">Der asynchrone Funktions Text wird dann ausgewertet, bis er entweder angehalten wird (indem ein Erwartungs Ausdruck erreicht wird) oder beendet wird, an dem die Punkt Steuerung zusammen mit der Rückgabe Aufgabe an den Aufrufer zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="6443e-1693">Wenn der Text der Async-Funktion beendet wird, wird die Rückgabe Aufgabe aus dem unvollständigen Zustand verschoben:</span><span class="sxs-lookup"><span data-stu-id="6443e-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="6443e-1694">Wenn der Funktions Rumpf durch das Erreichen einer Return-Anweisung oder des Endes des Texts beendet wird, werden alle Ergebnis Werte in der Rückgabe Aufgabe aufgezeichnet, die in den Status "erfolgreich" versetzt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="6443e-1695">Wenn der Funktions Text als Ergebnis einer nicht abgefangenen Ausnahme ([der throw-Anweisung](statements.md#the-throw-statement)) beendet wird, wird die Ausnahme in der Rückgabe Aufgabe aufgezeichnet, die in einen fehlerhaften Zustand versetzt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="6443e-1696">Auswertung einer "void"-Rückgabe Async-Funktion</span><span class="sxs-lookup"><span data-stu-id="6443e-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="6443e-1697">Wenn der Rückgabetyp der Async- `void`Funktion ist, weicht die Auswertung von der obigen auf folgende Weise ab: Da keine Aufgabe zurückgegeben wird, kommuniziert die Funktion stattdessen mit Abschluss und Ausnahmen mit dem ***Synchronisierungs Kontext***des aktuellen Threads.</span><span class="sxs-lookup"><span data-stu-id="6443e-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="6443e-1698">Die genaue Definition des Synchronisierungs Kontexts ist implementierungsabhängig, ist jedoch eine Darstellung von "Where", in der der aktuelle Thread ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="6443e-1699">Der Synchronisierungs Kontext wird benachrichtigt, wenn die Auswertung einer Async-Funktion mit void-Rückgabe beginnt, erfolgreich abgeschlossen wird oder eine nicht abgefangene Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="6443e-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="6443e-1700">Dies ermöglicht es dem Kontext nachzuverfolgen, wie viele void-zurückgegebene asynchrone Funktionen darunter ausgeführt werden, und um zu entscheiden, wie Ausnahmen weitergegeben werden sollen, die aus ihnen stammen.</span><span class="sxs-lookup"><span data-stu-id="6443e-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
