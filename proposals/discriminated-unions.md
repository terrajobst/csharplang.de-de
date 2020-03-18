---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "79484033"
---

# <a name="discriminated-unions--enum-class"></a><span data-ttu-id="6256f-101">Unterscheidungs-Unions/`enum class`</span><span class="sxs-lookup"><span data-stu-id="6256f-101">Discriminated unions / `enum class`</span></span>

<span data-ttu-id="6256f-102">`enum class`es handelt sich um eine neue Art von Typdeklaration, die manchmal auch als Unterscheidungs-Unions bezeichnet wird, in denen jede mögliche Instanz, in der der Typ aufgeführt ist, und jede Instanz nicht überlappend ist.</span><span class="sxs-lookup"><span data-stu-id="6256f-102">`enum class`es are a new kind of type declaration, sometimes referred to as discriminated unions, where each every possible instance the type is listed, and each instance is non-overlapping.</span></span>

<span data-ttu-id="6256f-103">Eine `enum class` wird mit der folgenden Syntax definiert:</span><span class="sxs-lookup"><span data-stu-id="6256f-103">An `enum class` is defined using the following syntax:</span></span>

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

<span data-ttu-id="6256f-104">Beispielsyntax:</span><span class="sxs-lookup"><span data-stu-id="6256f-104">Sample syntax:</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a><span data-ttu-id="6256f-105">Semantik</span><span class="sxs-lookup"><span data-stu-id="6256f-105">Semantics</span></span>

<span data-ttu-id="6256f-106">Eine `enum class` Definition definiert einen Stammtyp, bei dem es sich um eine abstrakte Klasse mit dem gleichen Namen wie die `enum class` Deklaration handelt, sowie eine Menge von Membern, von denen jede einen Typ aufweist, der ein Untertyp des Stammtyps ist.</span><span class="sxs-lookup"><span data-stu-id="6256f-106">An `enum class` definition defines a root type, which is an abstract class of the same name as the `enum class` declaration, and a set of members, each of which has a type which is a subtype of the root type.</span></span> <span data-ttu-id="6256f-107">Wenn mehrere `partial enum class` Definitionen vorhanden sind, werden alle Member als Member der enumerationsklassendefinition betrachtet.</span><span class="sxs-lookup"><span data-stu-id="6256f-107">If there are multiple `partial enum class` definitions, all members will be considered members of the enum class definition.</span></span> <span data-ttu-id="6256f-108">Anders als bei einer benutzerdefinierten abstrakten Klassendefinition ist der `enum class` Stammtyp standardmäßig partiell und so definiert, dass er einen standardmäßigen *privaten* Parameter losen Konstruktor hat.</span><span class="sxs-lookup"><span data-stu-id="6256f-108">Unlike a user-defined abstract class definition, the `enum class` root type is partial by default and defined to have a default *private* parameter-less constructor.</span></span>

<span data-ttu-id="6256f-109">Beachten Sie, dass, da der Stammtyp als partielle abstrakte Klasse definiert ist, partielle Definitionen des *Stammtyps* ebenfalls hinzugefügt werden können, wobei die standardmäßigen Syntax Formulare für einen Klassen Text zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="6256f-109">Note that, since the root type is defined to be a partial abstract class, partial definitions of the *root type* may also be added, where standard syntax forms for a class body are allowed.</span></span>
<span data-ttu-id="6256f-110">Allerdings können keine Typen direkt vom Stammtyp in einer Deklaration erben, abgesehen von denjenigen, die als `enum class` Member angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="6256f-110">However, no types may directly inherit from the root type in any declaration, aside from those specified as `enum class` members.</span></span> <span data-ttu-id="6256f-111">Außerdem sind für den Stammtyp keine benutzerdefinierten Konstruktoren zulässig.</span><span class="sxs-lookup"><span data-stu-id="6256f-111">In addition, no user-defined constructors are permitted for the root type.</span></span>

<span data-ttu-id="6256f-112">Es gibt vier Arten von `enum class`-Element Deklarationen:</span><span class="sxs-lookup"><span data-stu-id="6256f-112">There are four kinds of `enum class` member declarations:</span></span>

* <span data-ttu-id="6256f-113">Einfache Klassenmember</span><span class="sxs-lookup"><span data-stu-id="6256f-113">simple class members</span></span>

* <span data-ttu-id="6256f-114">komplexe Klassenmember</span><span class="sxs-lookup"><span data-stu-id="6256f-114">complex class members</span></span>

* <span data-ttu-id="6256f-115">Mitglieder `enum class`</span><span class="sxs-lookup"><span data-stu-id="6256f-115">`enum class` members</span></span>

* <span data-ttu-id="6256f-116">Wertemember.</span><span class="sxs-lookup"><span data-stu-id="6256f-116">value members.</span></span>

### <a name="simple-class-members"></a><span data-ttu-id="6256f-117">Einfache Klassenmember</span><span class="sxs-lookup"><span data-stu-id="6256f-117">Simple class members</span></span>

<span data-ttu-id="6256f-118">Eine einfache Klassenmember-Deklaration definiert eine neue, in diesem Dokument absichtlich nicht definierte "Record"-Klasse mit demselben Namen.</span><span class="sxs-lookup"><span data-stu-id="6256f-118">A simple class member declaration defines a new nested "record" class (intentionally left undefined in this document) with the same name.</span></span> <span data-ttu-id="6256f-119">Die von der-Klasse erfasste Klasse erbt vom Stammtyp.</span><span class="sxs-lookup"><span data-stu-id="6256f-119">The nested class inherits from the root type.</span></span>

<span data-ttu-id="6256f-120">Im obigen Beispielcode:</span><span class="sxs-lookup"><span data-stu-id="6256f-120">Given the sample code above,</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

<span data-ttu-id="6256f-121">die `enum class` Deklaration hat eine Semantik, die der folgenden Deklaration entspricht.</span><span class="sxs-lookup"><span data-stu-id="6256f-121">the `enum class` declaration has semantics equivalent to the following declaration</span></span>

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a><span data-ttu-id="6256f-122">Komplexe Klassenmember</span><span class="sxs-lookup"><span data-stu-id="6256f-122">Complex class members</span></span>

<span data-ttu-id="6256f-123">Sie können auch eine vollständige Klassen Deklaration unterhalb einer `enum class` Deklaration Schachteln.</span><span class="sxs-lookup"><span data-stu-id="6256f-123">You can also nest an entire class declaration below an `enum class` declaration.</span></span> <span data-ttu-id="6256f-124">Sie wird als eine Klasse vom Typ "fsted" behandelt.</span><span class="sxs-lookup"><span data-stu-id="6256f-124">It will be treated as a nested class of the root type.</span></span> <span data-ttu-id="6256f-125">Die Syntax lässt eine beliebige Klassen Deklaration zu, Sie ist jedoch erforderlich, damit der komplexe Klassenmember von der direkt enthaltenden `enum class` Deklaration erben kann.</span><span class="sxs-lookup"><span data-stu-id="6256f-125">The syntax allows any class declaration, but it is required for the complex class member to inherit from the direct containing `enum class` declaration.</span></span> 

### <a name="enum-class-members"></a><span data-ttu-id="6256f-126">Mitglieder `enum class`</span><span class="sxs-lookup"><span data-stu-id="6256f-126">`enum class` members</span></span>

<span data-ttu-id="6256f-127">`enum classes` können untereinander (z. b.</span><span class="sxs-lookup"><span data-stu-id="6256f-127">`enum classes` can be nested under each other, e.g.</span></span>

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

<span data-ttu-id="6256f-128">Dies ist nahezu identisch mit der Semantik einer `enum class`der obersten Ebene, mit der Ausnahme, dass die eingefügte Enumerationsklasse einen Typ vom Typ "nsted root" definiert, und alles unterhalb der untergeordneten Enumerationstypen ist ein Untertyp des nicht dem Stammtyp der obersten Ebene.</span><span class="sxs-lookup"><span data-stu-id="6256f-128">This is almost identical to the semantics of a top-level `enum class`, except that the nested enum class defines a nested root type, and everything below the nested enum class is a subtype of the nested root type, instead of the top-level root type.</span></span>

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a><span data-ttu-id="6256f-129">Wertemember</span><span class="sxs-lookup"><span data-stu-id="6256f-129">Value members</span></span>

<span data-ttu-id="6256f-130">`enum classes` können auch Wertmember enthalten.</span><span class="sxs-lookup"><span data-stu-id="6256f-130">`enum classes` can also contain value members.</span></span> <span data-ttu-id="6256f-131">Wertemember definieren öffentliche statische Get-only-Eigenschaften für den Stammtyp, die ebenfalls den Stammtyp zurückgeben, z. b.</span><span class="sxs-lookup"><span data-stu-id="6256f-131">Value members define public get-only static properties on the root type that also return the root type, e.g.</span></span>

```C#
enum class Color
{
    Red,
    Green
}
```

<span data-ttu-id="6256f-132">verfügt über Eigenschaften, die entsprechen</span><span class="sxs-lookup"><span data-stu-id="6256f-132">has properties equivalent to</span></span>

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

<span data-ttu-id="6256f-133">Die gesamte Semantik wird als Implementierungsdetail betrachtet, es wird jedoch sichergestellt, dass für jede Eigenschaft eine eindeutige Instanz zurückgegeben wird und dass bei wiederholten Aufrufen dieselbe Instanz zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="6256f-133">The complete semantics are considered an implementation detail, but it is guaranteed that one unique instance will be returned for each property, and the same instance will be returned on repeated invocations.</span></span>


### <a name="switch-expression-and-patterns"></a><span data-ttu-id="6256f-134">Wechseln von Ausdruck und Mustern</span><span class="sxs-lookup"><span data-stu-id="6256f-134">Switch expression and patterns</span></span>

<span data-ttu-id="6256f-135">Es gibt einige Vorschläge für den Musterabgleich und den Switch-Ausdruck, der `enum classes`behandelt.</span><span class="sxs-lookup"><span data-stu-id="6256f-135">There are some proposed adjustments to pattern matching and the switch expression to handle `enum classes`.</span></span> <span data-ttu-id="6256f-136">Switch-Ausdrücke können bereits über das Variablen Muster mit Typen übereinstimmen. für Verweis Typen werden jedoch keine Satz-Zeichen im Switch-Ausdruck als "Complete" betrachtet, außer für Vergleiche mit dem statischen Argumenttyp oder einen Untertyp.</span><span class="sxs-lookup"><span data-stu-id="6256f-136">Switch expressions can already match types through the variable pattern, but for currently for reference types, no set of switch arms in the switch expression are considered complete, except for matching against the static type of the argument, or a subtype.</span></span>

<span data-ttu-id="6256f-137">Switch-Ausdrücke würden geändert, wenn der Stammtyp eines `enum class` der statische Typ des Arguments für den Switch-Ausdruck ist und es eine Reihe von Mustern gibt, die allen Membern der Enumeration entsprechen, wird der Schalter als vollständig betrachtet.</span><span class="sxs-lookup"><span data-stu-id="6256f-137">Switch expressions would be changed such that, if the root type of an `enum class` is the static type of the argument to the switch expression, and there is a set of patterns matching all members of the enum, then the switch will be considered exhaustive.</span></span>

<span data-ttu-id="6256f-138">Da Wertemember keine Konstanten sind und keine neuen statischen Typen definieren, können Sie derzeit nicht anhand eines Musters abgeglichen werden.</span><span class="sxs-lookup"><span data-stu-id="6256f-138">Since value members are not constants and do not define new static types, they currently cannot be matched by pattern.</span></span> <span data-ttu-id="6256f-139">Um dies zu ermöglichen, wird ein neues Muster mit der konstantenmustersyntax hinzugefügt, um eine Entsprechung mit `enum class`-wertmembern zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="6256f-139">To make this possible, a new pattern using the constant pattern syntax will be added to allow match against `enum class` value members.</span></span> <span data-ttu-id="6256f-140">Die Übereinstimmung ist so definiert, dass Sie nur erfolgreich ist, wenn das Argument für das Muster übereinstimmt und der Wert, der vom `enum class`-Wertmember zurückgegeben wird, gleich ist, auch wenn die Implementierung diese Überprüfung nicht ausführen muss.</span><span class="sxs-lookup"><span data-stu-id="6256f-140">The match is defined to succeed if and only if the argument to the pattern match and the value returned by the `enum class` value member would be reference equal, although the implementation is not required to perform this check.</span></span>


## <a name="open-questions"></a><span data-ttu-id="6256f-141">Offene Fragen</span><span class="sxs-lookup"><span data-stu-id="6256f-141">Open questions</span></span>

- <span data-ttu-id="6256f-142">[] Was sagt der allgemeine Typalgorithmus über `enum class` Member?</span><span class="sxs-lookup"><span data-stu-id="6256f-142">[ ] What does the common type algorithm say about `enum class` members?</span></span> <span data-ttu-id="6256f-143">Handelt es sich um einen gültigen Code?</span><span class="sxs-lookup"><span data-stu-id="6256f-143">Is this valid code?</span></span>
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- <span data-ttu-id="6256f-144">[] Das Hinzufügen eines neuen Musters nur für Wertemember scheint stark.</span><span class="sxs-lookup"><span data-stu-id="6256f-144">[ ] Adding a new pattern just for value members seems heavy handed.</span></span> <span data-ttu-id="6256f-145">Gibt es eine allgemeinere Versions Erstellung, die sinnvoll ist?</span><span class="sxs-lookup"><span data-stu-id="6256f-145">Is there a more general version construction that makes sense?</span></span>
    - <span data-ttu-id="6256f-146">[] Wertemember können nicht ordnungsgemäß einer parallelen Struktur der Klassen Erstellung zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="6256f-146">[ ] Value members also do not map well to a parallel nested class construction because of this</span></span>

- <span data-ttu-id="6256f-147">[] Wechselt zu einem Argument, bei dem ein `enum class` statischer Typ garantiert konstant ist.</span><span class="sxs-lookup"><span data-stu-id="6256f-147">[ ] Is switching against an argument with an `enum class` static type guaranteed to be constant-time?</span></span>

- <span data-ttu-id="6256f-148">[] Sollte es eine Möglichkeit geben, `enum class`es im Switch-Ausdruck nicht als "Fertig" angesehen wird?</span><span class="sxs-lookup"><span data-stu-id="6256f-148">[ ] Should there be a way to make `enum class`es not be considered complete in the switch expression?</span></span> <span data-ttu-id="6256f-149">Präfix mit `virtual`?</span><span class="sxs-lookup"><span data-stu-id="6256f-149">Prefix with `virtual`?</span></span>

- <span data-ttu-id="6256f-150">[] Welche modifiziererer sollten bei `enum class`zulässig sein?</span><span class="sxs-lookup"><span data-stu-id="6256f-150">[ ] What modifiers should be permitted on `enum class`?</span></span>