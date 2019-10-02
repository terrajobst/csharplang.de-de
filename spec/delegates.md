---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704102"
---
# <a name="delegates"></a><span data-ttu-id="78bab-101">Delegaten</span><span class="sxs-lookup"><span data-stu-id="78bab-101">Delegates</span></span>

<span data-ttu-id="78bab-102">Delegaten aktivieren Szenarien, die von anderen Sprachen C++– wie, Pascal und Modula, mit Funktions Zeigern adressiert wurden.</span><span class="sxs-lookup"><span data-stu-id="78bab-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="78bab-103">Im C++ Gegensatz zu Funktions Zeigern sind Delegaten jedoch vollständig objektorientiert C++ , und im Gegensatz zu Zeigern auf Element Funktionen Kapseln Delegaten sowohl eine Objektinstanz als auch eine-Methode.</span><span class="sxs-lookup"><span data-stu-id="78bab-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="78bab-104">Eine Delegatdeklaration definiert eine Klasse, die von der-Klasse `System.Delegate` abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="78bab-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="78bab-105">Eine Delegatinstanz kapselt eine Aufruf Liste, bei der es sich um eine Liste von mindestens einer Methode handelt, von denen jede als Aufruf Bare Entität bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="78bab-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="78bab-106">Bei Instanzmethoden besteht eine Aufruf Bare Entität aus einer-Instanz und einer-Methode für diese Instanz.</span><span class="sxs-lookup"><span data-stu-id="78bab-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="78bab-107">Bei statischen Methoden besteht eine Aufruf Bare Entität nur aus einer-Methode.</span><span class="sxs-lookup"><span data-stu-id="78bab-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="78bab-108">Das Aufrufen einer Delegatinstanz mit einem geeigneten Satz von Argumenten bewirkt, dass alle Aufruf baren Entitäten des Delegaten mit dem angegebenen Satz von Argumenten aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="78bab-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="78bab-109">Eine interessante und nützliche Eigenschaft einer Delegatinstanz besteht darin, dass die Klassen der gekapselten Methoden nicht bekannt sind. alles, was wichtig ist, ist, dass diese Methoden mit dem Typ des Delegaten kompatibel sind ([Delegatdeklarationen](delegates.md#delegate-declarations)).</span><span class="sxs-lookup"><span data-stu-id="78bab-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="78bab-110">Dadurch eignen sich Delegaten hervorragend für den "anonymen" Aufruf.</span><span class="sxs-lookup"><span data-stu-id="78bab-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="78bab-111">Delegatdeklarationen</span><span class="sxs-lookup"><span data-stu-id="78bab-111">Delegate declarations</span></span>

<span data-ttu-id="78bab-112">Ein *delegate_declaration* ist ein *type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), der einen neuen Delegattyp deklariert.</span><span class="sxs-lookup"><span data-stu-id="78bab-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

<span data-ttu-id="78bab-113">Es ist ein Kompilierzeitfehler, damit derselbe Modifizierer mehrmals in einer Delegatdeklaration angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="78bab-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="78bab-114">Der `new`-Modifizierer ist nur für Delegaten zulässig, die in einem anderen Typ deklariert wurden. in diesem Fall gibt er an, dass ein solcher Delegat einen geerbten Member mit demselben Namen verbirgt, wie im [neuen Modifizierer](classes.md#the-new-modifier)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="78bab-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="78bab-115">Die Modifizierer `public`, `protected`, `internal` und `private` steuern den Zugriff auf den Delegattyp.</span><span class="sxs-lookup"><span data-stu-id="78bab-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="78bab-116">Abhängig vom Kontext, in dem die Delegatdeklaration auftritt, sind einige dieser Modifizierer möglicherweise nicht zulässig (als[Barrierefreiheit deklariert](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="78bab-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="78bab-117">Der Typname des Delegaten ist ein *Bezeichner*.</span><span class="sxs-lookup"><span data-stu-id="78bab-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="78bab-118">Der optionale *formal_parameter_list* gibt die Parameter des Delegaten an, und *return_type* gibt den Rückgabetyp des Delegaten an.</span><span class="sxs-lookup"><span data-stu-id="78bab-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="78bab-119">Die optionalen *variant_type_parameter_list* ([Variant-Typparameter Listen](interfaces.md#variant-type-parameter-lists)) gibt die Typparameter für den Delegaten an.</span><span class="sxs-lookup"><span data-stu-id="78bab-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="78bab-120">Der Rückgabetyp eines Delegattyps muss entweder `void` oder Output-Safe ([Varianz Sicherheit](interfaces.md#variance-safety)) sein.</span><span class="sxs-lookup"><span data-stu-id="78bab-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="78bab-121">Alle formalen Parametertypen eines Delegattyps müssen Eingabe sicher sein.</span><span class="sxs-lookup"><span data-stu-id="78bab-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="78bab-122">Außerdem müssen alle Parametertypen von `out` oder `ref` ebenfalls Ausgabe sicher sein.</span><span class="sxs-lookup"><span data-stu-id="78bab-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="78bab-123">Beachten Sie, dass auch `out`-Parameter aufgrund einer Einschränkung der zugrunde liegenden Ausführungsplattform Eingabe sicher sein müssen.</span><span class="sxs-lookup"><span data-stu-id="78bab-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="78bab-124">Delegattypen C# in sind namens äquivalent, nicht strukturell äquivalent.</span><span class="sxs-lookup"><span data-stu-id="78bab-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="78bab-125">Insbesondere werden zwei unterschiedliche Delegattypen, die über die gleichen Parameterlisten und Rückgabe Typen verfügen, als andere Delegattypen betrachtet.</span><span class="sxs-lookup"><span data-stu-id="78bab-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="78bab-126">Instanzen von zwei eindeutigen, aber strukturell äquivalenten Delegattypen können als gleich (Delegat-[Gleichheits Operatoren](expressions.md#delegate-equality-operators)) verglichen werden.</span><span class="sxs-lookup"><span data-stu-id="78bab-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="78bab-127">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78bab-127">For example:</span></span>

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

<span data-ttu-id="78bab-128">Die Methoden `A.M1` und `B.M1` sind mit den Delegattypen `D1` und `D2` kompatibel, da Sie den gleichen Rückgabetyp und die gleiche Parameterliste aufweisen. Diese Delegattypen sind jedoch zwei verschiedene Typen, sodass Sie nicht austauschbar sind.</span><span class="sxs-lookup"><span data-stu-id="78bab-128">The methods `A.M1` and `B.M1` are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="78bab-129">Die Methoden `B.M2`, `B.M3` und `B.M4` sind nicht mit den Delegattypen `D1` und `D2` kompatibel, da Sie unterschiedliche Rückgabe Typen oder Parameterlisten aufweisen.</span><span class="sxs-lookup"><span data-stu-id="78bab-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="78bab-130">Wie andere generische Typdeklarationen müssen Typargumente angegeben werden, um einen konstruierten Delegattyp zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="78bab-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="78bab-131">Die Parametertypen und der Rückgabetyp eines konstruierten Delegattyps werden erstellt, indem für jeden Typparameter in der Delegatdeklaration das entsprechende Typargument des konstruierten Delegattyps ersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="78bab-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="78bab-132">Der resultierende Rückgabetyp und die Parametertypen werden verwendet, um zu bestimmen, welche Methoden mit einem konstruierten Delegattyp kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="78bab-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="78bab-133">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78bab-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="78bab-134">Die-Methode `X.F` ist mit dem Delegattyp `Predicate<int>` kompatibel, und die-Methode `X.G` ist mit dem Delegattyp `Predicate<string>` kompatibel.</span><span class="sxs-lookup"><span data-stu-id="78bab-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="78bab-135">Die einzige Möglichkeit, einen Delegattyp zu deklarieren, ist über eine *delegate_declaration*.</span><span class="sxs-lookup"><span data-stu-id="78bab-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="78bab-136">Ein Delegattyp ist ein Klassentyp, der von `System.Delegate` abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="78bab-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="78bab-137">Delegattypen sind implizit `sealed`, daher ist es nicht zulässig, einen beliebigen Typ von einem Delegattyp abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="78bab-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="78bab-138">Es ist auch nicht zulässig, einen nicht-delegatklassentyp von `System.Delegate` abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="78bab-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="78bab-139">Beachten Sie, dass "`System.Delegate`" nicht selbst ein Delegattyp ist. Dabei handelt es sich um einen Klassentyp, von dem alle Delegattypen abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="78bab-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="78bab-140">C#bietet eine spezielle Syntax für die Instanziierung und den Aufruf von Delegaten.</span><span class="sxs-lookup"><span data-stu-id="78bab-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="78bab-141">Mit Ausnahme der Instanziierung kann jeder Vorgang, der auf eine Klasse oder eine Klasseninstanz angewendet werden kann, auch auf eine Delegatklasse bzw.-Instanz angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="78bab-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="78bab-142">Insbesondere ist es möglich, über die übliche Syntax des Element Zugriffs auf Member des `System.Delegate`-Typs zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="78bab-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="78bab-143">Der Satz von Methoden, die von einer Delegatinstanz gekapselt werden, wird als Aufruf Liste bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="78bab-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="78bab-144">Wenn eine Delegatinstanz ([delegatkompatibilität](delegates.md#delegate-compatibility)) aus einer einzelnen Methode erstellt wird, kapselt Sie diese Methode, und die Aufruf Liste enthält nur einen Eintrag.</span><span class="sxs-lookup"><span data-stu-id="78bab-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="78bab-145">Wenn jedoch zwei nicht-NULL-Delegatinstanzen kombiniert werden, werden die Aufruf Listen verkettet, und zwar im Order Left-Operand, Right Operand--, um eine neue Aufruf Liste zu erstellen, die zwei oder mehr Einträge enthält.</span><span class="sxs-lookup"><span data-stu-id="78bab-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="78bab-146">Delegaten werden mithilfe der binären `+`-Operatoren (Additions[Operator](expressions.md#addition-operator)) und `+=`-Operatoren ([Verbund Zuweisung](expressions.md#compound-assignment)) kombiniert.</span><span class="sxs-lookup"><span data-stu-id="78bab-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="78bab-147">Ein Delegat kann aus einer Kombination von Delegaten entfernt werden, wobei der binäre `-`-[Operator (Subtraktions Operator](expressions.md#subtraction-operator)) und `-=`-Operatoren ([Verbund Zuweisung](expressions.md#compound-assignment)) verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="78bab-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="78bab-148">Delegaten können auf Gleichheit verglichen werden ([delegatgleichheits-Operatoren](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="78bab-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="78bab-149">Im folgenden Beispiel wird die Instanziierung einer Reihe von Delegaten und ihre entsprechenden Aufruf Listen veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="78bab-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

<span data-ttu-id="78bab-150">Wenn `cd1` und `cd2` instanziiert werden, Kapseln Sie jeweils eine Methode.</span><span class="sxs-lookup"><span data-stu-id="78bab-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="78bab-151">Wenn `cd3` instanziiert wird, verfügt sie über eine Aufruf Liste mit zwei Methoden `M1` und `M2` in dieser Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="78bab-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="78bab-152">die Aufruf Liste von `cd4` enthält `M1`, `M2` und `M1` in dieser Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="78bab-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="78bab-153">Zum Schluss enthält die Aufruf Liste von `cd5` `M1`, `M2`, `M1`, `M1` und `M2` in dieser Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="78bab-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="78bab-154">Weitere Beispiele für das kombinieren (und entfernen) von Delegaten finden Sie unter [Delegataufruf](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="78bab-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="78bab-155">Delegatkompatibilität</span><span class="sxs-lookup"><span data-stu-id="78bab-155">Delegate compatibility</span></span>

<span data-ttu-id="78bab-156">Eine Methode oder ein Delegat `M` ist mit einem Delegattyp ***kompatibel*** `D`, wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="78bab-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="78bab-157">`D` und `M` haben die gleiche Anzahl von Parametern, und jeder Parameter in `D` hat dieselben `ref`-oder `out`-Modifizierer wie der entsprechende Parameter in `M`.</span><span class="sxs-lookup"><span data-stu-id="78bab-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="78bab-158">Für jeden value-Parameter (ein Parameter ohne `ref`-oder `out`-Modifizierer) ist eine Identitäts Konvertierung ([Identitäts Konvertierung](conversions.md#identity-conversion)) oder eine implizite Verweis Konvertierung ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions)) vom Parametertyp in `D` zum entsprechender Parametertyp in `M`.</span><span class="sxs-lookup"><span data-stu-id="78bab-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="78bab-159">Für jeden `ref`-oder `out`-Parameter ist der Parametertyp in `D` mit dem Parametertyp in `M` identisch.</span><span class="sxs-lookup"><span data-stu-id="78bab-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="78bab-160">Eine Identität oder implizite Verweis Konvertierung ist vom Rückgabetyp `M` bis zum Rückgabetyp `D` vorhanden.</span><span class="sxs-lookup"><span data-stu-id="78bab-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="78bab-161">Delegatinstanziierung</span><span class="sxs-lookup"><span data-stu-id="78bab-161">Delegate instantiation</span></span>

<span data-ttu-id="78bab-162">Eine Instanz eines Delegaten wird durch einen *delegate_creation_expression* ([delegaterstellungs-Ausdruck](expressions.md#delegate-creation-expressions)) oder eine Konvertierung in einen Delegattyp erstellt.</span><span class="sxs-lookup"><span data-stu-id="78bab-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="78bab-163">Die neu erstellte Delegatinstanz verweist dann auf eine der folgenden Optionen:</span><span class="sxs-lookup"><span data-stu-id="78bab-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="78bab-164">Die statische Methode, auf die in *delegate_creation_expression*verwiesen wird, oder</span><span class="sxs-lookup"><span data-stu-id="78bab-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="78bab-165">Das Zielobjekt (das nicht `null` sein kann) und die Instanzmethode, auf die in *delegate_creation_expression*verwiesen wird, oder</span><span class="sxs-lookup"><span data-stu-id="78bab-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="78bab-166">Ein weiterer Delegat.</span><span class="sxs-lookup"><span data-stu-id="78bab-166">Another delegate.</span></span>

<span data-ttu-id="78bab-167">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78bab-167">For example:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

<span data-ttu-id="78bab-168">Nach der instanziierten werden Delegatinstanzen immer auf das gleiche Zielobjekt und die gleiche Methode verwiesen.</span><span class="sxs-lookup"><span data-stu-id="78bab-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="78bab-169">Beachten Sie Folgendes: Wenn zwei Delegaten kombiniert werden oder eine aus einer anderen entfernt wird, ergibt sich ein neuer Delegat mit einer eigenen Aufruf Liste. die Aufruf Listen der zusammengesetzten oder entfernten Delegaten bleiben unverändert.</span><span class="sxs-lookup"><span data-stu-id="78bab-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="78bab-170">Delegataufruf</span><span class="sxs-lookup"><span data-stu-id="78bab-170">Delegate invocation</span></span>

<span data-ttu-id="78bab-171">C#bietet eine spezielle Syntax zum Aufrufen eines Delegaten.</span><span class="sxs-lookup"><span data-stu-id="78bab-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="78bab-172">Wenn eine Delegatinstanz ungleich NULL, deren Aufruf Liste einen Eintrag enthält, aufgerufen wird, ruft Sie die eine Methode mit denselben Argumenten auf, die Sie angegeben hat, und gibt denselben Wert zurück wie die Methode, auf die verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="78bab-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="78bab-173">(Weitere Informationen zum Delegataufruf finden Sie unter [delegieren](expressions.md#delegate-invocations) von aufrufen.) Wenn während des Aufrufs eines solchen Delegaten eine Ausnahme auftritt und diese Ausnahme in der aufgerufenen Methode nicht abgefangen wird, wird die Suche nach einer Ausnahme catch-Klausel in der Methode fortgesetzt, die den Delegaten aufgerufen hat, als wäre diese Methode direkt als Methode, auf die dieser Delegat verwiesen hat.</span><span class="sxs-lookup"><span data-stu-id="78bab-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="78bab-174">Der Aufruf einer Delegatinstanz, deren Aufruf Liste mehrere Einträge enthält, verläuft durch das synchrone Aufrufen der Methoden in der Aufruf Liste.</span><span class="sxs-lookup"><span data-stu-id="78bab-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="78bab-175">Jede Methode, die so aufgerufen wird, wird der gleiche Satz von Argumenten übergeben, die an die Delegatinstanz übergeben wurden.</span><span class="sxs-lookup"><span data-stu-id="78bab-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="78bab-176">Wenn ein solcher Delegataufruf Verweis Parameter ([Verweis Parameter](classes.md#reference-parameters)) enthält, erfolgt jeder Methodenaufruf mit einem Verweis auf die gleiche Variable. Änderungen an dieser Variablen durch eine Methode in der Aufruf Liste werden für Methoden weiter unten in der Aufruf Liste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="78bab-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="78bab-177">Wenn der Delegataufruf Ausgabeparameter oder einen Rückgabewert enthält, erfolgt der endgültige Wert aus dem Aufruf des letzten Delegaten in der Liste.</span><span class="sxs-lookup"><span data-stu-id="78bab-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="78bab-178">Wenn bei der Verarbeitung des Aufrufs eines solchen Delegaten eine Ausnahme auftritt und diese Ausnahme innerhalb der aufgerufenen Methode nicht abgefangen wird, wird die Suche nach einer Ausnahme catch-Klausel in der Methode fortgesetzt, die den Delegaten aufgerufen hat, und alle Methoden weiter unten die Aufruf Liste wird nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="78bab-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="78bab-179">Der Versuch, eine Delegatinstanz aufzurufen, deren Wert NULL ist, führt zu einer Ausnahme vom Typ `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="78bab-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="78bab-180">Im folgenden Beispiel wird gezeigt, wie Delegaten instanziiert, kombiniert, entfernt und aufgerufen werden:</span><span class="sxs-lookup"><span data-stu-id="78bab-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

<span data-ttu-id="78bab-181">Wie in der-Anweisung `cd3 += cd1;` gezeigt, kann ein Delegat mehrmals in einer Aufruf Liste vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="78bab-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="78bab-182">In diesem Fall wird Sie nur einmal pro vorkommen aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="78bab-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="78bab-183">In einer Aufruf Liste wie diesem wird beim Entfernen dieses Delegaten das letzte Vorkommen in der Aufruf Liste tatsächlich entfernt.</span><span class="sxs-lookup"><span data-stu-id="78bab-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="78bab-184">Unmittelbar vor der Ausführung der abschließenden Anweisung `cd3 -= cd1;` verweist der Delegat `cd3` auf eine leere Aufruf Liste.</span><span class="sxs-lookup"><span data-stu-id="78bab-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="78bab-185">Der Versuch, einen Delegaten aus einer leeren Liste zu entfernen (oder um einen nicht vorhandenen Delegaten aus einer nicht leeren Liste zu entfernen), ist kein Fehler.</span><span class="sxs-lookup"><span data-stu-id="78bab-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="78bab-186">Die erstellte Ausgabe lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="78bab-186">The output produced is:</span></span>

```console
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
