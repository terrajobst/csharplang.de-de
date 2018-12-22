# <a name="delegates"></a><span data-ttu-id="32849-101">Delegaten</span><span class="sxs-lookup"><span data-stu-id="32849-101">Delegates</span></span>

<span data-ttu-id="32849-102">Delegaten ermöglichen von Szenarien, die von anderen Sprachen, z. B. C++, Pascal-Schreibweise und Modula – mit Funktionszeigern behoben haben.</span><span class="sxs-lookup"><span data-stu-id="32849-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="32849-103">Im Gegensatz zu C++-Funktionszeigern allerdings Delegaten sind vollständig objektorientiert, und im Gegensatz zu C++-Zeiger auf Member-Funktionen Delegaten sowohl eine Objektinstanz und eine Methode kapseln.</span><span class="sxs-lookup"><span data-stu-id="32849-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="32849-104">Definiert eine Klasse, die von der Klasse abgeleitet ist, eine Delegatdeklaration `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="32849-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="32849-105">Eine Delegatinstanz kapselt eine Aufrufliste, wird eine Liste von ein oder mehrere Methoden, von die jedes als eine aufrufbare Entität bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="32849-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="32849-106">Beispielsweise besteht eine aufrufbare Entität aus einer Instanz und eine Methode für diese Instanz.</span><span class="sxs-lookup"><span data-stu-id="32849-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="32849-107">Bei statischen Methoden besteht aus nur eine Methode eine aufrufbare Entität.</span><span class="sxs-lookup"><span data-stu-id="32849-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="32849-108">Die Instanz eines Delegaten mit einer entsprechenden Satz von Argumenten aufrufen bewirkt, dass jede aufrufbare Entitäten des Delegaten ab, mit dem angegebenen Satz von Argumenten aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="32849-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="32849-109">Eine interessante und nützliche Eigenschaft einer Delegatinstanz ist, dass es nicht kennt oder Sie die Klassen der Methoden, die diese kapselt kümmern. wichtig ist, dass diese Methoden kompatibel ([delegieren Deklarationen](delegates.md#delegate-declarations)) mit dem Typ des Delegaten.</span><span class="sxs-lookup"><span data-stu-id="32849-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="32849-110">Auf diese Weise Delegaten bestens geeignet für den Aufruf von "Anonym".</span><span class="sxs-lookup"><span data-stu-id="32849-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="32849-111">Delegatdeklarationen</span><span class="sxs-lookup"><span data-stu-id="32849-111">Delegate declarations</span></span>

<span data-ttu-id="32849-112">Ein *Delegate_declaration* ist eine *Type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), einen neuer Delegattyp deklariert.</span><span class="sxs-lookup"><span data-stu-id="32849-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

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

<span data-ttu-id="32849-113">Es ist ein Fehler während der Kompilierung für den gleichen Modifizierer für mehrere Male in einer Delegatdeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="32849-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="32849-114">Die `new` Modifizierer ist nur zulässig, auf den Delegaten in einem anderen Typ deklariert, in diesem Fall wird, einen solchen Delegaten Blendet einen geerbten Member mit demselben Namen, wie in beschrieben [der new-Modifizierer](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="32849-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="32849-115">Die `public`, `protected`, `internal`, und `private` Modifizierern steuern den Zugriff der Delegattyp.</span><span class="sxs-lookup"><span data-stu-id="32849-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="32849-116">Je nach Kontext, in dem die Delegatdeklaration auftritt, einige dieser Modifizierer kann nicht gestattet ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="32849-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="32849-117">Ist der Name des Delegattyps *Bezeichner*.</span><span class="sxs-lookup"><span data-stu-id="32849-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="32849-118">Der optionale *Formal_parameter_list* gibt die Parameter des Delegaten und *Return_type* den Rückgabetyp des Delegaten angibt.</span><span class="sxs-lookup"><span data-stu-id="32849-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="32849-119">Der optionale *Variant_type_parameter_list* ([Variante Typparameterlisten](interfaces.md#variant-type-parameter-lists)) gibt an, die Typparameter an den Delegaten selbst.</span><span class="sxs-lookup"><span data-stu-id="32849-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="32849-120">Der Rückgabetyp eines Delegattyps muss entweder `void`, oder Ausgabe-Safe ([Varianz Sicherheit](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="32849-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="32849-121">Alle Typen der formalen Parameter einen Delegattyp aufweisen müssen Eingabe threadsicher sein.</span><span class="sxs-lookup"><span data-stu-id="32849-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="32849-122">Darüber hinaus eine `out` oder `ref` Parametertypen müssen auch Ausgabe threadsicher sein.</span><span class="sxs-lookup"><span data-stu-id="32849-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="32849-123">Beachten Sie, dass sogar `out` Parameter sind erforderlich, um die Eingabe-Safe, aufgrund einer Einschränkung von der zugrunde liegenden Ausführungsplattform zu werden.</span><span class="sxs-lookup"><span data-stu-id="32849-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="32849-124">Delegattypen in C# geschrieben sind nennen, entspricht, nicht strukturell äquivalent.</span><span class="sxs-lookup"><span data-stu-id="32849-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="32849-125">Insbesondere zwei unterschiedliche Delegattypen, die den gleichen Parameter enthält und Zurückgeben des Typs gelten unterschiedliche Delegattypen.</span><span class="sxs-lookup"><span data-stu-id="32849-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="32849-126">Allerdings können Instanzen von zwei unterschiedliche, jedoch strukturell Äquivalent Delegattypen als gleichwertig zu vergleichen ([delegieren Gleichheitsoperatoren](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="32849-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="32849-127">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="32849-127">For example:</span></span>

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

<span data-ttu-id="32849-128">Die Methoden `A.M1` und `B.M1 `sind kompatibel mit sowohl die Delegattypen `D1` und `D2` , da sie haben dasselbe zurückzugeben und die Parameterliste; jedoch diese Delegattypen zwei unterschiedliche Typen sind, sodass sie nicht sind austauschbar.</span><span class="sxs-lookup"><span data-stu-id="32849-128">The methods `A.M1` and `B.M1 `are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="32849-129">Die Methoden `B.M2`, `B.M3`, und `B.M4` sind nicht kompatibel mit Delegattypen `D1` und `D2`, da sie unterschiedliche Rückgabetypen oder Parameterlisten aufweisen.</span><span class="sxs-lookup"><span data-stu-id="32849-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="32849-130">Wie andere generische Typdeklarationen müssen Typargumente angegeben werden, um einen Typ des erstellten Delegaten zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="32849-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="32849-131">Ersetzen der für jeden Typparameter in der Delegatdeklaration überein, das entsprechende Typargument der Typ des erstellten Delegaten werden die Parameter und Rückgabetypen eines Typs des erstellten Delegaten erstellt.</span><span class="sxs-lookup"><span data-stu-id="32849-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="32849-132">Die resultierende Rückgabe- und Parametertypen werden verwendet, bei der Bestimmung, welche Methoden mit einem Typ des erstellten Delegaten kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="32849-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="32849-133">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="32849-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="32849-134">Die Methode `X.F` ist kompatibel mit dem Delegattyp `Predicate<int>` und die Methode `X.G` ist kompatibel mit dem Delegattyp `Predicate<string>` .</span><span class="sxs-lookup"><span data-stu-id="32849-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="32849-135">Die einzige Möglichkeit, einen Delegattyp deklariert wird, über eine *Delegate_declaration*.</span><span class="sxs-lookup"><span data-stu-id="32849-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="32849-136">Ein Delegattyp ist ein Klassentyp, das von abgeleitet ist `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="32849-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="32849-137">Delegattypen sind implizit `sealed`, sodass es nicht zulässig, einen Delegattyp einen beliebigen Typ abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="32849-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="32849-138">Es ist auch nicht zulässig, einen Typ "nicht-Delegate"-Klasse aus abzuleiten `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="32849-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="32849-139">Beachten Sie, dass `System.Delegate` ist selbst ein Delegattyp, sondern ein Klassentyp, der von der alle Delegattypen abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="32849-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="32849-140">C# und bietet Instanziierung und den Aufruf spezielle Syntax für Delegaten.</span><span class="sxs-lookup"><span data-stu-id="32849-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="32849-141">Mit Ausnahme der Instanziierung kann alle Vorgänge, die auf eine Klasse oder Instanz der Klasse angewendet werden kann auch auf eine "Delegate"-Klasse oder die Instanz ist, bzw. angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="32849-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="32849-142">Insbesondere kann den Zugriff auf Member, der die `System.Delegate` Typ über die üblichen Member-Access-Syntax.</span><span class="sxs-lookup"><span data-stu-id="32849-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="32849-143">Der Satz von Methoden, die von einer Delegatinstanz gekapselt wird eine Aufrufliste aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="32849-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="32849-144">Wenn eine Delegatinstanz erstellt wird ([delegieren Kompatibilität](delegates.md#delegate-compatibility)) aus einer einzelnen Methode, diese Methode kapselt, und die Aufrufliste nur einen Eintrag enthält.</span><span class="sxs-lookup"><span data-stu-id="32849-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="32849-145">Jedoch, wenn zwei Delegatinstanzen von nicht-Null-kombiniert werden, deren Aufruflisten verkettet werden in der Reihenfolge, Linker Operand rechten Operanden –, um eine neue Aufrufliste zu bilden, die zwei oder mehr Einträge enthält:.</span><span class="sxs-lookup"><span data-stu-id="32849-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="32849-146">Delegaten kombiniert werden, verwenden die Binärdatei `+` ([Additionsoperator](expressions.md#addition-operator)) und `+=` Operatoren ([Verbundzuweisung](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="32849-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="32849-147">Ein Delegat kann entfernt werden, aus einer Kombination von Delegaten, mit der Binärdatei `-` ([Subtraktionsoperator](expressions.md#subtraction-operator)) und `-=` Operatoren ([Verbundzuweisung](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="32849-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="32849-148">Delegaten, die auf Gleichheit verglichen werden können ([delegieren Gleichheitsoperatoren](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="32849-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="32849-149">Das folgende Beispiel zeigt die Instanziierung einer Reihe von Delegaten, und ihre entsprechenden Aufruf werden aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="32849-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

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

<span data-ttu-id="32849-150">Wenn `cd1` und `cd2` werden instanziiert, jeweils eine Methode kapseln.</span><span class="sxs-lookup"><span data-stu-id="32849-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="32849-151">Wenn `cd3` wird instanziiert, er verfügt über eine Aufrufliste der beiden Methoden, `M1` und `M2`in dieser Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="32849-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="32849-152">`cd4`die Aufrufliste enthält `M1`, `M2`, und `M1`in dieser Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="32849-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="32849-153">Zum Schluss `cd5`Aufrufliste enthält `M1`, `M2`, `M1`, `M1`, und `M2`in dieser Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="32849-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="32849-154">Weitere Beispiele für die Kombination von (auch als entfernen) Delegaten, finden Sie unter [Delegataufruf](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="32849-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="32849-155">Delegieren der Kompatibilität</span><span class="sxs-lookup"><span data-stu-id="32849-155">Delegate compatibility</span></span>

<span data-ttu-id="32849-156">Eine Methode oder Delegat `M` ist ***kompatibel*** mit einem Delegattypen `D` , wenn alle der folgenden Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="32849-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="32849-157">`D` und `M` haben die gleiche Anzahl von Parametern und jeden Parameter in `D` hat die gleiche `ref` oder `out` Modifizierer wie der entsprechende Parameter im `M`.</span><span class="sxs-lookup"><span data-stu-id="32849-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="32849-158">Für jeden Wertparameter (einen Parameter ohne `ref` oder `out` Modifizierer), eine identitätskonvertierung ([identitätskonvertierung](conversions.md#identity-conversion)) oder implizite verweiskonvertierung ([implizite Verweis-](conversions.md#implicit-reference-conversions)) vorhanden ist, aus dem Parametertyp in `D` auf den entsprechenden Parametertyp in `M`.</span><span class="sxs-lookup"><span data-stu-id="32849-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="32849-159">Für jede `ref` oder `out` Typparameter, der Parameter in `D` ist identisch mit dem Parametertyp in `M`.</span><span class="sxs-lookup"><span data-stu-id="32849-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="32849-160">Eine Identität oder implizite verweiskonvertierung vorhanden ist, aus dem Rückgabetyp der `M` in den Rückgabetyp der `D`.</span><span class="sxs-lookup"><span data-stu-id="32849-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="32849-161">Instanziierung von Delegaten</span><span class="sxs-lookup"><span data-stu-id="32849-161">Delegate instantiation</span></span>

<span data-ttu-id="32849-162">Eine Instanz eines Delegaten wird erstellt, indem eine *Delegate_creation_expression* ([delegieren erstellen Ausdrücke](expressions.md#delegate-creation-expressions)) oder eine Konvertierung in einen Delegattyp aufweisen.</span><span class="sxs-lookup"><span data-stu-id="32849-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="32849-163">Die neu erstellte Delegatinstanz verweist dann entweder auf:</span><span class="sxs-lookup"><span data-stu-id="32849-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="32849-164">Die statische Methode, die auf die verwiesen wird der *Delegate_creation_expression*, oder</span><span class="sxs-lookup"><span data-stu-id="32849-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="32849-165">Das Zielobjekt (nicht die `null`) und der Instanz-Methode, die auf die verwiesen wird der *Delegate_creation_expression*, oder</span><span class="sxs-lookup"><span data-stu-id="32849-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="32849-166">Ein anderer Delegat.</span><span class="sxs-lookup"><span data-stu-id="32849-166">Another delegate.</span></span>

<span data-ttu-id="32849-167">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="32849-167">For example:</span></span>

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

<span data-ttu-id="32849-168">Nach der Instanziierung finden Sie Delegatinstanzen immer die gleichen Zielobjekt und die Methode.</span><span class="sxs-lookup"><span data-stu-id="32849-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="32849-169">Beachten Sie, wenn zwei Delegaten kombiniert werden, oder eine von einem anderen, einen neuen Delegaten Ergebnisse mit eigenen Aufrufliste entfernt wird. die Aufruflisten der Delegaten kombiniert oder entfernt bleiben unverändert.</span><span class="sxs-lookup"><span data-stu-id="32849-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="32849-170">Delegataufruf</span><span class="sxs-lookup"><span data-stu-id="32849-170">Delegate invocation</span></span>

<span data-ttu-id="32849-171">C# bietet speziellen Syntax zum Aufrufen eines Delegaten an.</span><span class="sxs-lookup"><span data-stu-id="32849-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="32849-172">Wenn eine nicht-Null-Delegatinstanz, dessen Aufrufliste einen Eintrag enthält, aufgerufen wird, ruft die Methode mit den gleichen Argumenten, es wurde angegeben, und gibt den gleichen Wert wie bezeichnet-Methode.</span><span class="sxs-lookup"><span data-stu-id="32849-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="32849-173">(Finden Sie unter [delegieren Aufrufe](expressions.md#delegate-invocations) ausführliche Informationen zu Delegaten aufrufen.) Wenn eine Ausnahme tritt auf, während des Aufrufs eines solchen Delegaten, und diese Ausnahme nicht abgefangen wird, innerhalb der Methode, der aufgerufen wurde, werden die Suche nach einer Ausnahme-Catch-Klausel in der Methode, die den Delegaten aufgerufen fortgesetzt, als wäre diese Methode direkt aufgerufen hätte die Methode, die delegieren, bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="32849-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="32849-174">Aufruf einer Delegatinstanz, dessen Aufrufliste mehrere Einträge enthält, die durch den Aufruf der Methoden in der Aufrufliste synchron, nacheinander wird fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="32849-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="32849-175">An jede Methode wird den gleiche Satz von Argumenten, wie die Delegatinstanz zugewiesen wurde übergeben.</span><span class="sxs-lookup"><span data-stu-id="32849-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="32849-176">Wenn ein Delegataufruf Verweisparameter enthält ([Verweisparameter](classes.md#reference-parameters)), jeden Methodenaufruf erfolgt durch einen Verweis auf dieselbe Variable; Änderungen an die Variable einer Methode in der Aufrufliste für die folgenden Methoden der Aufrufliste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="32849-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="32849-177">Wenn der Delegataufruf Output-Parameter oder Rückgabewerte enthält, wird der endgültige Wert über den Aufruf des letzten Delegaten in der Liste stammen.</span><span class="sxs-lookup"><span data-stu-id="32849-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="32849-178">Wenn eine Ausnahme tritt auf, während der Verarbeitung des Aufrufs eines solchen Delegaten, und diese Ausnahme wird nicht innerhalb der Methode, die aufgerufen wurde abgefangen, die Suche nach einer Ausnahme-Catch-Klausel wird weiterhin in der Methode, die der Delegat aufgerufen, und alle Methoden weiter unten die Liste der Aufrufe werden nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="32849-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="32849-179">Es wird versucht, eine Delegatinstanz aufrufen, deren Wert null führt zu einer Ausnahme vom Typ ist `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="32849-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="32849-180">Das folgende Beispiel zeigt, wie Sie instanziieren, zu kombinieren, entfernen und Aufrufen von Delegaten:</span><span class="sxs-lookup"><span data-stu-id="32849-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

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

<span data-ttu-id="32849-181">Wie in der Anweisung `cd3 += cd1;`, ein Delegat kann vorhanden sein in einer Aufrufliste mehrere Male.</span><span class="sxs-lookup"><span data-stu-id="32849-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="32849-182">In diesem Fall ist es einfach einmal pro Vorkommen aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="32849-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="32849-183">In einer Aufrufliste, wie diese Wenn dieses Delegaten entfernt wird, wird das letzte Vorkommen der Aufrufliste entfernt.</span><span class="sxs-lookup"><span data-stu-id="32849-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="32849-184">Unmittelbar vor der Ausführung der letzten Anweisung `cd3 -= cd1;`, den Delegaten `cd3` bezieht sich auf eine leere Aufrufliste.</span><span class="sxs-lookup"><span data-stu-id="32849-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="32849-185">Es wird versucht, einen Delegaten aus einer leeren Liste zu entfernen (oder um einen nicht vorhandenen Delegaten aus eine nicht leere Liste zu entfernen) ist kein Fehler.</span><span class="sxs-lookup"><span data-stu-id="32849-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="32849-186">Die Ausgabe erzeugt wird:</span><span class="sxs-lookup"><span data-stu-id="32849-186">The output produced is:</span></span>

```
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
