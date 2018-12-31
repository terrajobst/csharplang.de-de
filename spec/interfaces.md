# <a name="interfaces"></a><span data-ttu-id="1cff5-101">Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="1cff5-101">Interfaces</span></span>

<span data-ttu-id="1cff5-102">Eine Schnittstelle definiert einen Vertrag.</span><span class="sxs-lookup"><span data-stu-id="1cff5-102">An interface defines a contract.</span></span> <span data-ttu-id="1cff5-103">Eine Klasse oder Struktur, die eine Schnittstelle implementiert, muss ihren Vertrag einhalten.</span><span class="sxs-lookup"><span data-stu-id="1cff5-103">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="1cff5-104">Eine Schnittstelle kann von mehreren Basisschnittstellen erben, und eine Klasse oder Struktur kann mehrere Schnittstellen implementieren.</span><span class="sxs-lookup"><span data-stu-id="1cff5-104">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="1cff5-105">Schnittstellen können Methoden, Eigenschaften, Ereignisse und Indexer enthalten.</span><span class="sxs-lookup"><span data-stu-id="1cff5-105">Interfaces can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="1cff5-106">Die Schnittstelle selbst stellt keine Implementierungen für die Member bereit, das sie definiert.</span><span class="sxs-lookup"><span data-stu-id="1cff5-106">The interface itself does not provide implementations for the members that it defines.</span></span> <span data-ttu-id="1cff5-107">Die Schnittstelle gibt nur die Elemente, die von Klassen oder Strukturen, die die Schnittstelle implementieren bereitgestellt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-107">The interface merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

## <a name="interface-declarations"></a><span data-ttu-id="1cff5-108">Deklarationen von Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="1cff5-108">Interface declarations</span></span>

<span data-ttu-id="1cff5-109">Ein *Interface_declaration* ist eine *Type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), die eine neue Art von Schnittstelle deklariert.</span><span class="sxs-lookup"><span data-stu-id="1cff5-109">An *interface_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new interface type.</span></span>

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

<span data-ttu-id="1cff5-110">Ein *Interface_declaration* besteht aus einer optionalen Gruppe von *Attribute* ([Attribute](attributes.md)), gefolgt von einer optionalen Gruppe von *Interface_modifier*s ([Schnittstelle Modifizierer](interfaces.md#interface-modifiers)), gefolgt von einem optionalen `partial` Modifizierer, gefolgt vom Schlüsselwort `interface` und *Bezeichner* mit dem Namen der Schnittstelle gefolgt von einem optionalen *Variant_type_parameter_list* Spezifikation ([Variante Typparameterlisten](interfaces.md#variant-type-parameter-lists)), gefolgt von einem optionalen *Interface_base* Spezifikation ([Basisschnittstellen](interfaces.md#base-interfaces)), gefolgt von einem optionalen *Type_parameter_constraints_clause*s-Spezifikation ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)) , gefolgt von einem *Interface_body* ([Schnittstelle-Text](interfaces.md#interface-body)), optional gefolgt durch ein Semikolon.</span><span class="sxs-lookup"><span data-stu-id="1cff5-110">An *interface_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *interface_modifier*s ([Interface modifiers](interfaces.md#interface-modifiers)), followed by an optional `partial` modifier, followed by the keyword `interface` and an *identifier* that names the interface, followed by an optional *variant_type_parameter_list* specification ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)), followed by an optional *interface_base* specification ([Base interfaces](interfaces.md#base-interfaces)), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by an *interface_body* ([Interface body](interfaces.md#interface-body)), optionally followed by a semicolon.</span></span>

### <a name="interface-modifiers"></a><span data-ttu-id="1cff5-111">Interface-Modifizierer</span><span class="sxs-lookup"><span data-stu-id="1cff5-111">Interface modifiers</span></span>

<span data-ttu-id="1cff5-112">Ein *Interface_declaration* kann optional eine Sequenz von Schnittstelle Modifizierer enthalten:</span><span class="sxs-lookup"><span data-stu-id="1cff5-112">An *interface_declaration* may optionally include a sequence of interface modifiers:</span></span>

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

<span data-ttu-id="1cff5-113">Es ist ein Fehler während der Kompilierung für den gleichen Modifizierer für mehrere Male in einer Schnittstellendeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="1cff5-113">It is a compile-time error for the same modifier to appear multiple times in an interface declaration.</span></span>

<span data-ttu-id="1cff5-114">Die `new` Modifizierer ist nur zulässig, auf die Schnittstellen, die innerhalb einer Klasse definiert.</span><span class="sxs-lookup"><span data-stu-id="1cff5-114">The `new` modifier is only permitted on interfaces defined within a class.</span></span> <span data-ttu-id="1cff5-115">Es gibt an, dass die Schnittstelle mit dem gleichen Namen, Blendet einen geerbten Member aus wie in beschrieben [der new-Modifizierer](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="1cff5-115">It specifies that the interface hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="1cff5-116">Die `public`, `protected`, `internal`, und `private` Modifizierern steuern den Zugriff auf die Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="1cff5-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the interface.</span></span> <span data-ttu-id="1cff5-117">Je nach Kontext, in dem die Schnittstellendeklaration auftritt, nur einige dieser Modifizierer gestattet werden ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="1cff5-117">Depending on the context in which the interface declaration occurs, only some of these modifiers may be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="1cff5-118">Ein partial-Modifizierer</span><span class="sxs-lookup"><span data-stu-id="1cff5-118">Partial modifier</span></span>

<span data-ttu-id="1cff5-119">Die `partial` Modifizierer gibt an, dass dies *Interface_declaration* ist eine Deklaration der partiellen Typ.</span><span class="sxs-lookup"><span data-stu-id="1cff5-119">The `partial` modifier indicates that this *interface_declaration* is a partial type declaration.</span></span> <span data-ttu-id="1cff5-120">Mehrere partielle Schnittstellendeklarationen mit demselben Namen in einer einschließenden Namespace oder Typ Deklaration kombiniert werden, um das Formular eine Schnittstellendeklaration, im angegebenen gemäß den Regeln [partielle Typen](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="1cff5-120">Multiple partial interface declarations with the same name within an enclosing namespace or type declaration combine to form one interface declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="variant-type-parameter-lists"></a><span data-ttu-id="1cff5-121">Variant-Typ-Parameterlisten</span><span class="sxs-lookup"><span data-stu-id="1cff5-121">Variant type parameter lists</span></span>

<span data-ttu-id="1cff5-122">Die Listen der Variant-Typ-Parameter können nur auf Schnittstellen-und Delegattypen auftreten.</span><span class="sxs-lookup"><span data-stu-id="1cff5-122">Variant type parameter lists can only occur on interface and delegate types.</span></span> <span data-ttu-id="1cff5-123">Der Unterschied zu normalen *Type_parameter_list*s ist der optionale *Variance_annotation* für jeden Typparameter.</span><span class="sxs-lookup"><span data-stu-id="1cff5-123">The difference from ordinary *type_parameter_list*s is the optional *variance_annotation* on each type parameter.</span></span>

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

<span data-ttu-id="1cff5-124">Wenn die varianzanmerkung ist `out`, der Type-Parameter gilt als ***kovariant***.</span><span class="sxs-lookup"><span data-stu-id="1cff5-124">If the variance annotation is `out`, the type parameter is said to be ***covariant***.</span></span> <span data-ttu-id="1cff5-125">Wenn die varianzanmerkung ist `in`, der Type-Parameter gilt als ***kontravariant***.</span><span class="sxs-lookup"><span data-stu-id="1cff5-125">If the variance annotation is `in`, the type parameter is said to be ***contravariant***.</span></span> <span data-ttu-id="1cff5-126">Wenn keine varianzanmerkung vorhanden ist, der Type-Parameter gilt ***invariante***.</span><span class="sxs-lookup"><span data-stu-id="1cff5-126">If there is no variance annotation, the type parameter is said to be ***invariant***.</span></span>

<span data-ttu-id="1cff5-127">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="1cff5-127">In the example</span></span>
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
<span data-ttu-id="1cff5-128">`X` kovariant ist, `Y` kontravariant ist, und `Z` unveränderlich ist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-128">`X` is covariant, `Y` is contravariant and `Z` is invariant.</span></span>

#### <a name="variance-safety"></a><span data-ttu-id="1cff5-129">Varianz-Sicherheit</span><span class="sxs-lookup"><span data-stu-id="1cff5-129">Variance safety</span></span>

<span data-ttu-id="1cff5-130">Das Vorhandensein Varianzkennzeichnungen in der Typparameterliste eines Typs schränkt die Orte, wo die Typen in der Typdeklaration auftreten können.</span><span class="sxs-lookup"><span data-stu-id="1cff5-130">The occurrence of variance annotations in the type parameter list of a type restricts the places where types can occur within the type declaration.</span></span>

<span data-ttu-id="1cff5-131">Ein Typ `T` ist ***Ausgabe / unsafe*** , wenn eine der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="1cff5-131">A type `T` is ***output-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="1cff5-132">`T` Ein kontravarianter Typparameter wird</span><span class="sxs-lookup"><span data-stu-id="1cff5-132">`T` is a contravariant type parameter</span></span>
*  <span data-ttu-id="1cff5-133">`T` ist ein Arraytyp mit einer Ausgabe / unsafe-Elementtyp</span><span class="sxs-lookup"><span data-stu-id="1cff5-133">`T` is an array type with an output-unsafe element type</span></span>
*  <span data-ttu-id="1cff5-134">`T` ist ein Typ von Schnittstellen oder Delegate `S<A1,...,Ak>` aus einem generischen Typ erstellter `S<X1,...,Xk>` Where für mindestens eine `Ai` eine der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="1cff5-134">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="1cff5-135">`Xi` ist kovariant oder Invarianten und `Ai` Ausgabe-unsicher ist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-135">`Xi` is covariant or invariant and `Ai` is output-unsafe.</span></span>
   * <span data-ttu-id="1cff5-136">`Xi` ist kontravariant oder Invarianten und `Ai` Eingabe-sicher ist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-136">`Xi` is contravariant or invariant and `Ai` is input-safe.</span></span>
   
<span data-ttu-id="1cff5-137">Ein Typ `T` ist ***Eingabe-/ unsafe*** , wenn eine der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="1cff5-137">A type `T` is ***input-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="1cff5-138">`T` ein kovarianter Typparameter wird</span><span class="sxs-lookup"><span data-stu-id="1cff5-138">`T` is a covariant type parameter</span></span>
*  <span data-ttu-id="1cff5-139">`T` ist ein Arraytyp mit einem Eingabe-/ unsafe-Elementtyp</span><span class="sxs-lookup"><span data-stu-id="1cff5-139">`T` is an array type with an input-unsafe element type</span></span>
*  <span data-ttu-id="1cff5-140">`T` ist ein Typ von Schnittstellen oder Delegate `S<A1,...,Ak>` aus einem generischen Typ erstellter `S<X1,...,Xk>` Where für mindestens eine `Ai` eine der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="1cff5-140">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="1cff5-141">`Xi` ist kovariant oder Invarianten und `Ai` Eingabe-unsicher ist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-141">`Xi` is covariant or invariant and `Ai` is input-unsafe.</span></span>
   * <span data-ttu-id="1cff5-142">`Xi` ist kontravariant oder Invarianten und `Ai` Ausgabe-unsicher ist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-142">`Xi` is contravariant or invariant and `Ai` is output-unsafe.</span></span>

<span data-ttu-id="1cff5-143">Intuitiv Ausgabe-unsicherem Typ ist nicht zulässig, in einer Ausgabeposition und Eingabe-unsicherem Typ ist nicht zulässig, in die Position einer Eingabe.</span><span class="sxs-lookup"><span data-stu-id="1cff5-143">Intuitively, an output-unsafe type is prohibited in an output position, and an input-unsafe type is prohibited in an input position.</span></span>

<span data-ttu-id="1cff5-144">Ein Typ ist ***Ausgabe-sichere*** ist dies nicht Ausgabe unsicher, und ***Eingabe-sichere*** ist dies nicht Eingabe-/ unsafe.</span><span class="sxs-lookup"><span data-stu-id="1cff5-144">A type is ***output-safe*** if it is not output-unsafe, and ***input-safe*** if it is not input-unsafe.</span></span>

#### <a name="variance-conversion"></a><span data-ttu-id="1cff5-145">Varianzkonvertierungen</span><span class="sxs-lookup"><span data-stu-id="1cff5-145">Variance conversion</span></span>

<span data-ttu-id="1cff5-146">Der Zweck der Varianzkennzeichnungen ist weniger strenge (aber immer noch typsicher) Konvertierungen in Schnittstellen-und Delegattypen bereit.</span><span class="sxs-lookup"><span data-stu-id="1cff5-146">The purpose of variance annotations is to provide for more lenient (but still type safe) conversions to interface and delegate types.</span></span> <span data-ttu-id="1cff5-147">So beenden Sie die Definitionen der implizite ([implizite Konvertierungen](conversions.md#implicit-conversions)) und explizite Konvertierungen ([explizite Konvertierungen](conversions.md#explicit-conversions)) stellen verwenden das Konzept der varianzkonvertierbarkeit, die wie folgt definiert ist:</span><span class="sxs-lookup"><span data-stu-id="1cff5-147">To this end the definitions of implicit ([Implicit conversions](conversions.md#implicit-conversions)) and explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) make use of the notion of variance-convertibility, which is defined as follows:</span></span>

<span data-ttu-id="1cff5-148">Ein Typ `T<A1,...,An>` ist varianzkonvertierbar in einen Typ `T<B1,...,Bn>` Wenn `T` ist entweder eine Schnittstelle oder ein Delegattyp deklariert mit den Parametern der variant-Typ `T<X1,...,Xn>`, und für jeden Variante Typparameter `Xi` eine der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="1cff5-148">A type `T<A1,...,An>` is variance-convertible to a type `T<B1,...,Bn>` if `T` is either an interface or a delegate type declared with the variant type parameters `T<X1,...,Xn>`, and for each variant type parameter `Xi` one of the following holds:</span></span>

*  <span data-ttu-id="1cff5-149">`Xi` ist "covariant" und eine implizite Konvertierung von Verweis- oder Identität vorhanden ist, von `Ai` auf `Bi`</span><span class="sxs-lookup"><span data-stu-id="1cff5-149">`Xi` is covariant and an implicit reference or identity conversion exists from `Ai` to `Bi`</span></span>
*  <span data-ttu-id="1cff5-150">`Xi` kontravariant ist und einen impliziten Verweis oder identitätskonvertierung vorhanden ist, von `Bi` auf `Ai`</span><span class="sxs-lookup"><span data-stu-id="1cff5-150">`Xi` is contravariant and an implicit reference or identity conversion exists from `Bi` to `Ai`</span></span>
*  <span data-ttu-id="1cff5-151">`Xi` ist die invariante und eine Identität Konvertierungen von `Ai` zu `Bi`</span><span class="sxs-lookup"><span data-stu-id="1cff5-151">`Xi` is invariant and an identity conversion exists from `Ai` to `Bi`</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="1cff5-152">Basisschnittstellen</span><span class="sxs-lookup"><span data-stu-id="1cff5-152">Base interfaces</span></span>

<span data-ttu-id="1cff5-153">Eine Schnittstelle kann von NULL oder mehr Schnittstellentypen, die aufgerufen werden, erben die ***explizite Basisschnittstelle*** der Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="1cff5-153">An interface can inherit from zero or more interface types, which are called the ***explicit base interfaces*** of the interface.</span></span> <span data-ttu-id="1cff5-154">Wenn eine Schnittstelle ein oder mehrere explizite Basisschnittstelle verfügt, in der Deklaration dieser Schnittstelle, der Schnittstellenbezeichner wird anschließend von einem Doppelpunkt und einer durch Trennzeichen getrennte Liste der Basisschnittstelle-Typen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-154">When an interface has one or more explicit base interfaces, then in the declaration of that interface, the interface identifier is followed by a colon and a comma separated list of base interface types.</span></span>

```antlr
interface_base
    : ':' interface_type_list
    ;
```

<span data-ttu-id="1cff5-155">Für einen Typ konstruierte Schnittstelle werden die explizite Basisschnittstelle gebildet, indem die explizite Basisschnittstelle-Deklarationen in der Deklaration des generischen Typs und ersetzt werden, für die einzelnen *Type_parameter* in der Basisschnittstelle Deklaration der entsprechenden *Type_argument* des konstruierten Typs.</span><span class="sxs-lookup"><span data-stu-id="1cff5-155">For a constructed interface type, the explicit base interfaces are formed by taking the explicit base interface declarations on the generic type declaration, and substituting, for each *type_parameter* in the base interface declaration, the corresponding *type_argument* of the constructed type.</span></span>

<span data-ttu-id="1cff5-156">Die explizite Basisschnittstelle einer Schnittstelle muss mindestens dieselben zugriffsmöglichkeiten wie der Schnittstelle selbst ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="1cff5-156">The explicit base interfaces of an interface must be at least as accessible as the interface itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span> <span data-ttu-id="1cff5-157">Ist beispielsweise ein Fehler während der Kompilierung an eine `private` oder `internal` -Schnittstelle in der *Interface_base* von einer `public` Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="1cff5-157">For example, it is a compile-time error to specify a `private` or `internal` interface in the *interface_base* of a `public` interface.</span></span>

<span data-ttu-id="1cff5-158">Es ist ein Fehler während der Kompilierung für eine Schnittstelle, die direkt oder indirekt von sich selbst erben.</span><span class="sxs-lookup"><span data-stu-id="1cff5-158">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>

<span data-ttu-id="1cff5-159">Die ***Basisschnittstellen*** von einer Schnittstelle sind die explizite Basisschnittstelle und ihre Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-159">The ***base interfaces*** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="1cff5-160">Der Satz von Schnittstellen ist also die vollständige transitiven Abschluss von die explizite Basisschnittstelle, die explizite Basisschnittstelle und So weiter.</span><span class="sxs-lookup"><span data-stu-id="1cff5-160">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="1cff5-161">Eine Schnittstelle erbt alle Member der Basisschnittstellen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-161">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="1cff5-162">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="1cff5-162">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="1cff5-163">die Basisschnittstelle `IComboBox` sind `IControl`, `ITextBox`, und `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-163">the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="1cff5-164">Das heißt, die `IComboBox` obige Schnittstelle erbt Member `SetText` und `SetItems` sowie `Paint`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-164">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="1cff5-165">Jeder Basisschnittstelle einer Schnittstelle muss die Ausgabe-Safe ([Varianz Sicherheit](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="1cff5-165">Every base interface of an interface must be output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span> <span data-ttu-id="1cff5-166">Eine Klasse oder Struktur, die eine Schnittstelle, auch implizit implementiert implementiert alle die Basisschnittstellen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-166">A class or struct that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

### <a name="interface-body"></a><span data-ttu-id="1cff5-167">Schnittstelle-Text</span><span class="sxs-lookup"><span data-stu-id="1cff5-167">Interface body</span></span>

<span data-ttu-id="1cff5-168">Die *Interface_body* einer Schnittstelle definiert den Member der Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="1cff5-168">The *interface_body* of an interface defines the members of the interface.</span></span>

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a><span data-ttu-id="1cff5-169">Schnittstellenmember</span><span class="sxs-lookup"><span data-stu-id="1cff5-169">Interface members</span></span>

<span data-ttu-id="1cff5-170">Die Member einer Schnittstelle sind die von der Basisschnittstellen geerbten Member aus, und die Member der Schnittstelle selbst deklariert.</span><span class="sxs-lookup"><span data-stu-id="1cff5-170">The members of an interface are the members inherited from the base interfaces and the members declared by the interface itself.</span></span>

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

<span data-ttu-id="1cff5-171">Eine Schnittstellendeklaration kann NULL oder mehr Member deklarieren.</span><span class="sxs-lookup"><span data-stu-id="1cff5-171">An interface declaration may declare zero or more members.</span></span> <span data-ttu-id="1cff5-172">Die Member einer Schnittstelle müssen es sich um Methoden, Eigenschaften, Ereignisse und Indexer sein.</span><span class="sxs-lookup"><span data-stu-id="1cff5-172">The members of an interface must be methods, properties, events, or indexers.</span></span> <span data-ttu-id="1cff5-173">Eine Schnittstelle darf keine Konstanten, Felder, Operatoren, Instanzkonstruktoren, Destruktoren oder Typen enthalten, noch kann eine Schnittstelle enthalten statische Member irgendeiner Art.</span><span class="sxs-lookup"><span data-stu-id="1cff5-173">An interface cannot contain constants, fields, operators, instance constructors, destructors, or types, nor can an interface contain static members of any kind.</span></span>

<span data-ttu-id="1cff5-174">Alle Schnittstellenmember sind implizit über öffentlichen Zugriff.</span><span class="sxs-lookup"><span data-stu-id="1cff5-174">All interface members implicitly have public access.</span></span> <span data-ttu-id="1cff5-175">Es ist ein Fehler während der Kompilierung für Schnittstellenmember-Deklarationen Modifizierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="1cff5-175">It is a compile-time error for interface member declarations to include any modifiers.</span></span> <span data-ttu-id="1cff5-176">Insbesondere können nicht Schnittstellenmember deklariert werden, mit den Modifizierern `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, oder `static`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-176">In particular, interfaces members cannot be declared with the modifiers `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="1cff5-177">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="1cff5-177">The example</span></span>
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
<span data-ttu-id="1cff5-178">deklariert eine Schnittstelle, die jeweils eine der möglichen Arten von Member enthält: Eine Methode, eine Eigenschaft, ein Ereignis und einen Indexer.</span><span class="sxs-lookup"><span data-stu-id="1cff5-178">declares an interface that contains one each of the possible kinds of members: A method, a property, an event, and an indexer.</span></span>

<span data-ttu-id="1cff5-179">Ein *Interface_declaration* erstellt einen neuen Deklarationsabschnitt ([Deklarationen](basic-concepts.md#declarations)), und die *Interface_member_declaration*s sofort die enthaltenen*Interface_declaration* neue Elemente in diesen Deklarationsabschnitt einführen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-179">An *interface_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *interface_member_declaration*s immediately contained by the *interface_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="1cff5-180">Die folgenden Regeln gelten für *Interface_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="1cff5-180">The following rules apply to *interface_member_declaration*s:</span></span>

*  <span data-ttu-id="1cff5-181">Der Name einer Methode muss die Namen aller Eigenschaften und Ereignisse, die in der gleichen Schnittstelle deklariert unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="1cff5-181">The name of a method must differ from the names of all properties and events declared in the same interface.</span></span> <span data-ttu-id="1cff5-182">Außerdem wird die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) von eine Methode muss die Signaturen aller anderen in derselben Schnittstelle deklarierten Methoden unterscheiden, und zwei Methoden, die in der gleichen Schnittstelle deklarierten dürfen keine Signaturen, unterscheiden sich ausschließlich vom `ref` und `out`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-182">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same interface, and two methods declared in the same interface may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="1cff5-183">Der Name der Eigenschaft bzw. das Ereignis muss die Namen aller anderen in derselben Schnittstelle deklarierten Member unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="1cff5-183">The name of a property or event must differ from the names of all other members declared in the same interface.</span></span>
*  <span data-ttu-id="1cff5-184">Die Signatur eines Indexers muss sich von den Signaturen aller anderen in derselben Schnittstelle deklarierten Indexer unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="1cff5-184">The signature of an indexer must differ from the signatures of all other indexers declared in the same interface.</span></span>

<span data-ttu-id="1cff5-185">Die geerbten Member einer Schnittstelle sind speziell nicht Teil des Deklarationsabschnitts der Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="1cff5-185">The inherited members of an interface are specifically not part of the declaration space of the interface.</span></span> <span data-ttu-id="1cff5-186">Daher ist eine Schnittstelle zulässig, um einen Member mit demselben Namen bzw. derselben Signatur als einen geerbten Member deklarieren.</span><span class="sxs-lookup"><span data-stu-id="1cff5-186">Thus, an interface is allowed to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="1cff5-187">In diesem Fall wird der Member der abgeleiteten Schnittstelle als die Member der Basisschnittstelle ausblenden möchten.</span><span class="sxs-lookup"><span data-stu-id="1cff5-187">When this occurs, the derived interface member is said to hide the base interface member.</span></span> <span data-ttu-id="1cff5-188">Durch das Ausblenden eines geerbten Members ist kein Fehler betrachtet, aber es führt dazu, dass des Compilers eine Warnung ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="1cff5-188">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="1cff5-189">Um die Warnung zu unterdrücken, muss die Deklaration der abgeleiteten Schnittstellenmember enthalten eine `new` Modifizierer, um anzugeben, dass der abgeleitete Member die Member den Basisklassen ausblenden vorgesehen ist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-189">To suppress the warning, the declaration of the derived interface member must include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="1cff5-190">In diesem Thema wird erläutert. im weiteren [ausblenden, die durch Vererbung von](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="1cff5-190">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="1cff5-191">Wenn eine `new` Modifizierer ist in einer Deklaration, die einen geerbten Member auszublenden, nicht enthalten, die zu diesem Zweck wird eine Warnung ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="1cff5-191">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning is issued to that effect.</span></span> <span data-ttu-id="1cff5-192">Diese Warnung unterdrückt wird, durch das Entfernen der `new` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="1cff5-192">This warning is suppressed by removing the `new` modifier.</span></span>

<span data-ttu-id="1cff5-193">Beachten Sie, dass die Elemente in der Klasse `object` sind nicht streng genommen, Mitglied einer beliebigen Schnittstelle ([Schnittstellenmember](interfaces.md#interface-members)).</span><span class="sxs-lookup"><span data-stu-id="1cff5-193">Note that the members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="1cff5-194">Allerdings die Elemente in der Klasse `object` stehen über die Suche nach Membern in einen Schnittstellentyp ([Membersuche](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="1cff5-194">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="interface-methods"></a><span data-ttu-id="1cff5-195">Schnittstellenmethoden</span><span class="sxs-lookup"><span data-stu-id="1cff5-195">Interface methods</span></span>

<span data-ttu-id="1cff5-196">Schnittstellenmethoden deklariert werden, mithilfe von *Interface_method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="1cff5-196">Interface methods are declared using *interface_method_declaration*s:</span></span>

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

<span data-ttu-id="1cff5-197">Die *Attribute*, *Return_type*, *Bezeichner*, und *Formal_parameter_list* einer Methodendeklaration Schnittstelle weist den gleichen Dies bedeutet, wie die Deklaration einer Methode in einer Klasse ([Methoden](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="1cff5-197">The *attributes*, *return_type*, *identifier*, and *formal_parameter_list* of an interface method declaration have the same meaning as those of a method declaration in a class ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="1cff5-198">Eine Schnittstellen-Methodendeklaration ist nicht zulässig, einen Methodentext an, und die Deklaration ist daher immer mit einem Semikolon endet.</span><span class="sxs-lookup"><span data-stu-id="1cff5-198">An interface method declaration is not permitted to specify a method body, and the declaration therefore always ends with a semicolon.</span></span>

<span data-ttu-id="1cff5-199">Jeder formalen Parametertyp einer Schnittstellenmethode muss die Eingabe-Safe ([Varianz Sicherheit](interfaces.md#variance-safety)), und der Rückgabetyp muss entweder `void` oder Ausgabe-sicher.</span><span class="sxs-lookup"><span data-stu-id="1cff5-199">Each formal parameter type of an interface method must be input-safe ([Variance safety](interfaces.md#variance-safety)), and the return type must be either `void` or output-safe.</span></span> <span data-ttu-id="1cff5-200">Darüber hinaus muss jeder klassentypeinschränkung, die Schnittstelle eine Einschränkung und die Einschränkung eines Typparameters auf einen Typparameter der Methode Eingabe threadsicher sein.</span><span class="sxs-lookup"><span data-stu-id="1cff5-200">Furthermore, each class type constraint, interface type constraint and type parameter constraint on any type parameter of the method must be input-safe.</span></span>

<span data-ttu-id="1cff5-201">Diese Regeln stellen sicher, dass jede Kovariante oder die kontravariant Nutzung der Schnittstelle bleibt typsicher.</span><span class="sxs-lookup"><span data-stu-id="1cff5-201">These rules ensure that any covariant or contravariant usage of the interface remains type-safe.</span></span> <span data-ttu-id="1cff5-202">Ein auf ein Objekt angewendeter</span><span class="sxs-lookup"><span data-stu-id="1cff5-202">For example,</span></span>
```csharp
interface I<out T> { void M<U>() where U : T; }
```
<span data-ttu-id="1cff5-203">ist nicht zulässig da die Verwendung von `T` als Einschränkung für einen Parameter auf `U` ist nicht Eingabe-sicher.</span><span class="sxs-lookup"><span data-stu-id="1cff5-203">is illegal because the usage of `T` as a type parameter constraint on `U` is not input-safe.</span></span>

<span data-ttu-id="1cff5-204">Diese Einschränkung nicht vorhanden waren wäre, typsicherheit auf folgende Weise zu verletzen möglich es:</span><span class="sxs-lookup"><span data-stu-id="1cff5-204">Were this restriction not in place it would be possible to violate type safety in the following manner:</span></span>
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
<span data-ttu-id="1cff5-205">Dies ist tatsächlich ein Aufruf zum `C.M<E>`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-205">This is actually a call to `C.M<E>`.</span></span> <span data-ttu-id="1cff5-206">Aber dieser Aufruf, erfordert `E` abgeleitet `D`, sodass die typsicherheit hier ggf. verletzt wird.</span><span class="sxs-lookup"><span data-stu-id="1cff5-206">But that call requires that `E` derive from `D`, so type safety would be violated here.</span></span>

### <a name="interface-properties"></a><span data-ttu-id="1cff5-207">Schnittstelleneigenschaften</span><span class="sxs-lookup"><span data-stu-id="1cff5-207">Interface properties</span></span>

<span data-ttu-id="1cff5-208">Schnittstelleneigenschaften deklariert werden, mithilfe von *Interface_property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="1cff5-208">Interface properties are declared using *interface_property_declaration*s:</span></span>

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

<span data-ttu-id="1cff5-209">Die *Attribute*, *Typ*, und *Bezeichner* eine Schnittstellen-Eigenschaftendeklaration müssen die gleiche Bedeutung wie eine Eigenschaftendeklaration in einer Klasse ([ Eigenschaften](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="1cff5-209">The *attributes*, *type*, and *identifier* of an interface property declaration have the same meaning as those of a property declaration in a class ([Properties](classes.md#properties)).</span></span>

<span data-ttu-id="1cff5-210">Die Accessoren eine Schnittstellen-Eigenschaftendeklaration entsprechen den Accessoren einer Deklaration einer Klasseneigenschaft ([Accessoren](classes.md#accessors)), außer dass der Accessortext hinausgehen immer ein Semikolon sein muss.</span><span class="sxs-lookup"><span data-stu-id="1cff5-210">The accessors of an interface property declaration correspond to the accessors of a class property declaration ([Accessors](classes.md#accessors)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="1cff5-211">Daher Accessors wird angegeben, ob die Eigenschaft Lese-/ Schreibzugriff, schreibgeschützt oder lesegeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-211">Thus, the accessors simply indicate whether the property is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="1cff5-212">Der Typ einer Schnittstelleneigenschaft muss die Ausgabe-Safe, falls ein Get-Accessor vorhanden ist und sein Eingabe-Safe, wenn ein Set-Accessor vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-212">The type of an interface property must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-events"></a><span data-ttu-id="1cff5-213">Ereignisse der Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="1cff5-213">Interface events</span></span>

<span data-ttu-id="1cff5-214">Ereignisse der Benutzeroberfläche werden mit deklariert *Interface_event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="1cff5-214">Interface events are declared using *interface_event_declaration*s:</span></span>

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

<span data-ttu-id="1cff5-215">Die *Attribute*, *Typ*, und *Bezeichner* einer Schnittstellen-Ereignisdeklaration müssen die gleiche Bedeutung wie die Deklaration für einen Ereignis in einer Klasse ([Ereignisse ](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="1cff5-215">The *attributes*, *type*, and *identifier* of an interface event declaration have the same meaning as those of an event declaration in a class ([Events](classes.md#events)).</span></span>

<span data-ttu-id="1cff5-216">Der Typ eines Ereignisses für die Schnittstelle muss Eingabe threadsicher sein.</span><span class="sxs-lookup"><span data-stu-id="1cff5-216">The type of an interface event must be input-safe.</span></span>

### <a name="interface-indexers"></a><span data-ttu-id="1cff5-217">Indexer in Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="1cff5-217">Interface indexers</span></span>

<span data-ttu-id="1cff5-218">Schnittstellenindexer deklariert werden, mithilfe von *Interface_indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="1cff5-218">Interface indexers are declared using *interface_indexer_declaration*s:</span></span>

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

<span data-ttu-id="1cff5-219">Die *Attribute*, *Typ*, und *Formal_parameter_list* eine Schnittstellendeklaration für Indexer müssen die gleiche Bedeutung wie die Deklaration für einen Indexer in einer Klasse ([ Indexer](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="1cff5-219">The *attributes*, *type*, and *formal_parameter_list* of an interface indexer declaration have the same meaning as those of an indexer declaration in a class ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="1cff5-220">Die Accessoren des Indexers Schnittstellendeklarationen entsprechen den Accessoren einer Klassendeklaration für Indexer ([Indexer](classes.md#indexers)), außer dass der Accessortext hinausgehen immer ein Semikolon sein muss.</span><span class="sxs-lookup"><span data-stu-id="1cff5-220">The accessors of an interface indexer declaration correspond to the accessors of a class indexer declaration ([Indexers](classes.md#indexers)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="1cff5-221">Daher Accessors wird angegeben, ob der Indexer Lese-/ Schreibzugriff, schreibgeschützt oder lesegeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-221">Thus, the accessors simply indicate whether the indexer is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="1cff5-222">Alle Typen der formalen Parameter eines Indexers Schnittstelle müssen die Eingabe threadsicher sein.</span><span class="sxs-lookup"><span data-stu-id="1cff5-222">All the formal parameter types of an interface indexer must be input-safe .</span></span> <span data-ttu-id="1cff5-223">Darüber hinaus alle `out` oder `ref` Typen der formalen Parameter müssen auch Ausgabe threadsicher sein.</span><span class="sxs-lookup"><span data-stu-id="1cff5-223">In addition, any `out` or `ref` formal parameter types must also be output-safe.</span></span> <span data-ttu-id="1cff5-224">Beachten Sie, dass sogar `out` Parameter sind erforderlich, um die Eingabe-Safe, aufgrund einer Einschränkung von der zugrunde liegenden Ausführungsplattform zu werden.</span><span class="sxs-lookup"><span data-stu-id="1cff5-224">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="1cff5-225">Der Typ eines Indexers Schnittstelle muss die Ausgabe-Safe, falls ein Get-Accessor vorhanden ist und sein Eingabe-Safe, wenn ein Set-Accessor vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-225">The type of an interface indexer must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-member-access"></a><span data-ttu-id="1cff5-226">Schnittstelle Memberzugriff</span><span class="sxs-lookup"><span data-stu-id="1cff5-226">Interface member access</span></span>

<span data-ttu-id="1cff5-227">Schnittstellenmember erfolgt über den Memberzugriff ([Memberzugriff](expressions.md#member-access)) und Indexerzugriff ([Indexerzugriff](expressions.md#indexer-access)) Ausdrücken der Form `I.M` und `I[A]`, wobei `I` ist ein Schnittstellentyp `M` ist eine Methode, Eigenschaft oder das Ereignis, der diesen Schnittstellentyp und `A` ist eine Liste der Indexer-Argument.</span><span class="sxs-lookup"><span data-stu-id="1cff5-227">Interface members are accessed through member access ([Member access](expressions.md#member-access)) and indexer access ([Indexer access](expressions.md#indexer-access)) expressions of the form `I.M` and `I[A]`, where `I` is an interface type, `M` is a method, property, or event of that interface type, and `A` is an indexer argument list.</span></span>

<span data-ttu-id="1cff5-228">Für Schnittstellen, die streng sind einzelne Vererbung (jede Schnittstelle in der Vererbungskette hat genau 0 (null) oder eine direkte Basisschnittstelle), die Auswirkungen der Suche nach Membern ([Membersuche](expressions.md#member-lookup)), Methodenaufruf ([ Methodenaufrufe](expressions.md#method-invocations)), und Indexerzugriff ([Indexerzugriff](expressions.md#indexer-access)) Regeln sind genau die gleichen wie für Klassen und Strukturen: Stärker abgeleiteten Elemente ausblenden weniger abgeleiteten Elemente mit demselben Namen oder derselben Signatur.</span><span class="sxs-lookup"><span data-stu-id="1cff5-228">For interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effects of the member lookup ([Member lookup](expressions.md#member-lookup)), method invocation ([Method invocations](expressions.md#method-invocations)), and indexer access ([Indexer access](expressions.md#indexer-access)) rules are exactly the same as for classes and structs: More derived members hide less derived members with the same name or signature.</span></span> <span data-ttu-id="1cff5-229">Für mehrfache Vererbung Schnittstellen jedoch Mehrdeutigkeiten auftreten, wenn zwei oder mehr nicht verknüpfte Basisschnittstellen deklariert Member mit demselben Namen bzw. derselben Signatur.</span><span class="sxs-lookup"><span data-stu-id="1cff5-229">However, for multiple-inheritance interfaces, ambiguities can occur when two or more unrelated base interfaces declare members with the same name or signature.</span></span> <span data-ttu-id="1cff5-230">Dieser Abschnitt zeigt einige Beispiele für solche Situationen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-230">This section shows several examples of such situations.</span></span> <span data-ttu-id="1cff5-231">In allen Fällen können explizite Umwandlungen verwendet werden, um die Mehrdeutigkeiten aufzulösen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-231">In all cases, explicit casts can be used to resolve the ambiguities.</span></span>

<span data-ttu-id="1cff5-232">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="1cff5-232">In the example</span></span>
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
<span data-ttu-id="1cff5-233">die ersten beiden Anweisungen verursachen Kompilierzeitfehler, da die Membersuche ([Membersuche](expressions.md#member-lookup)) der `Count` in `IListCounter` ist mehrdeutig.</span><span class="sxs-lookup"><span data-stu-id="1cff5-233">the first two statements cause compile-time errors because the member lookup ([Member lookup](expressions.md#member-lookup)) of `Count` in `IListCounter` is ambiguous.</span></span> <span data-ttu-id="1cff5-234">Wie im Beispiel gezeigt wird, wird die Mehrdeutigkeit aufgelöst, indem Sie eine Umwandlung `x` in den entsprechenden Basisschnittstelle-Typ.</span><span class="sxs-lookup"><span data-stu-id="1cff5-234">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="1cff5-235">Solche Umwandlungen haben keine Laufzeit — sie bestehen lediglich zum Anzeigen der Instanz als weniger stark abgeleiteten Typ zum Zeitpunkt der Kompilierung aus.</span><span class="sxs-lookup"><span data-stu-id="1cff5-235">Such casts have no run-time costs—they merely consist of viewing the instance as a less derived type at compile-time.</span></span>

<span data-ttu-id="1cff5-236">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="1cff5-236">In the example</span></span>
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
<span data-ttu-id="1cff5-237">der Aufruf `n.Add(1)` wählt `IInteger.Add` durch Anwenden der Regeln der überladungsauflösung des [Überladungsauflösung](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="1cff5-237">the invocation `n.Add(1)` selects `IInteger.Add` by applying the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="1cff5-238">Analog dazu den Aufruf `n.Add(1.0)` wählt `IDouble.Add`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-238">Similarly the invocation `n.Add(1.0)` selects `IDouble.Add`.</span></span> <span data-ttu-id="1cff5-239">Wenn explizite Umwandlungen eingefügt werden, besteht nur ein Kandidat-Methode, und daher keine Mehrdeutigkeit.</span><span class="sxs-lookup"><span data-stu-id="1cff5-239">When explicit casts are inserted, there is only one candidate method, and thus no ambiguity.</span></span>

<span data-ttu-id="1cff5-240">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="1cff5-240">In the example</span></span>
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
<span data-ttu-id="1cff5-241">die `IBase.F` Element wird ausgeblendet, indem die `ILeft.F` Member.</span><span class="sxs-lookup"><span data-stu-id="1cff5-241">the `IBase.F` member is hidden by the `ILeft.F` member.</span></span> <span data-ttu-id="1cff5-242">Der Aufruf `d.F(1)` daher wählt `ILeft.F`, auch wenn `IBase.F` angezeigt wird, nicht in den Zugriffspfad ausgeblendet werden, das durch führt `IRight`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-242">The invocation `d.F(1)` thus selects `ILeft.F`, even though `IBase.F` appears to not be hidden in the access path that leads through `IRight`.</span></span>

<span data-ttu-id="1cff5-243">Die Faustregel für das Ausblenden in Schnittstellen für mehrfache Vererbung ist folgende: Wenn ein Element in einem Zugriffspfad ausgeblendet ist, wird es in alle Zugriffspfade ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="1cff5-243">The intuitive rule for hiding in multiple-inheritance interfaces is simply this: If a member is hidden in any access path, it is hidden in all access paths.</span></span> <span data-ttu-id="1cff5-244">Da Zugriffspfad aus `IDerived` zu `ILeft` zu `IBase` blendet `IBase.F`, das Element wird auch in den Zugriffspfad aus ausgeblendet `IDerived` zu `IRight` zu `IBase`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-244">Because the access path from `IDerived` to `ILeft` to `IBase` hides `IBase.F`, the member is also hidden in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

## <a name="fully-qualified-interface-member-names"></a><span data-ttu-id="1cff5-245">Den vollqualifizierten Namen von Schnittstellenmembern</span><span class="sxs-lookup"><span data-stu-id="1cff5-245">Fully qualified interface member names</span></span>

<span data-ttu-id="1cff5-246">Ein Schnittstellenmember wird mitunter die ***voll gekennzeichneten Namen***.</span><span class="sxs-lookup"><span data-stu-id="1cff5-246">An interface member is sometimes referred to by its ***fully qualified name***.</span></span> <span data-ttu-id="1cff5-247">Der vollqualifizierte Name eines Schnittstellenmembers besteht aus den Namen der Schnittstelle in der der Member ist deklariert, gefolgt von einem Punkt, gefolgt vom Namen des Members.</span><span class="sxs-lookup"><span data-stu-id="1cff5-247">The fully qualified name of an interface member consists of the name of the interface in which the member is declared, followed by a dot, followed by the name of the member.</span></span> <span data-ttu-id="1cff5-248">Der vollqualifizierte Name eines Elements verweist auf die Schnittstelle, die in der das Element deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="1cff5-248">The fully qualified name of a member references the interface in which the member is declared.</span></span> <span data-ttu-id="1cff5-249">Angenommen, die Deklarationen</span><span class="sxs-lookup"><span data-stu-id="1cff5-249">For example, given the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
<span data-ttu-id="1cff5-250">der vollqualifizierte Name des `Paint` ist `IControl.Paint` und den vollqualifizierten Namen der `SetText` ist `ITextBox.SetText`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-250">the fully qualified name of `Paint` is `IControl.Paint` and the fully qualified name of `SetText` is `ITextBox.SetText`.</span></span>

<span data-ttu-id="1cff5-251">Im obigen Beispiel ist es nicht möglich, verweisen auf `Paint` als `ITextBox.Paint`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-251">In the example above, it is not possible to refer to `Paint` as `ITextBox.Paint`.</span></span>

<span data-ttu-id="1cff5-252">Wenn eine Schnittstelle Teil eines Namespace ist, enthält der vollqualifizierte Name eines Schnittstellenmembers Name des Namespaces an.</span><span class="sxs-lookup"><span data-stu-id="1cff5-252">When an interface is part of a namespace, the fully qualified name of an interface member includes the namespace name.</span></span> <span data-ttu-id="1cff5-253">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1cff5-253">For example</span></span>
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

<span data-ttu-id="1cff5-254">Hier wird der vollqualifizierte Name des der `Clone` Methode `System.ICloneable.Clone`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-254">Here, the fully qualified name of the `Clone` method is `System.ICloneable.Clone`.</span></span>

## <a name="interface-implementations"></a><span data-ttu-id="1cff5-255">Schnittstellenimplementierungen</span><span class="sxs-lookup"><span data-stu-id="1cff5-255">Interface implementations</span></span>

<span data-ttu-id="1cff5-256">Schnittstellen können von Klassen und Strukturen implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="1cff5-256">Interfaces may be implemented by classes and structs.</span></span> <span data-ttu-id="1cff5-257">Um anzugeben, dass eine Klasse oder Struktur direkt eine Schnittstelle implementiert, ist der Schnittstellenbezeichner in der Basisklassenliste der Klasse oder Struktur enthalten.</span><span class="sxs-lookup"><span data-stu-id="1cff5-257">To indicate that a class or struct directly implements an interface, the interface identifier is included in the base class list of the class or struct.</span></span> <span data-ttu-id="1cff5-258">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1cff5-258">For example:</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

<span data-ttu-id="1cff5-259">Eine Klasse oder Struktur, die direkt für die eine Schnittstelle auch direkt implementiert alle Schnittstellen der Schnittstelle implizit implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="1cff5-259">A class or struct that directly implements an interface also directly implements all of the interface's base interfaces implicitly.</span></span> <span data-ttu-id="1cff5-260">Dies gilt auch, wenn die Klasse oder Struktur ausdrücklich alle Basisschnittstellen in der Basisklassenliste nicht.</span><span class="sxs-lookup"><span data-stu-id="1cff5-260">This is true even if the class or struct doesn't explicitly list all base interfaces in the base class list.</span></span> <span data-ttu-id="1cff5-261">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1cff5-261">For example:</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

<span data-ttu-id="1cff5-262">Hier Klasse `TextBox` implementiert beide `IControl` und `ITextBox`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-262">Here, class `TextBox` implements both `IControl` and `ITextBox`.</span></span>

<span data-ttu-id="1cff5-263">Wenn eine Klasse `C` direkt implementiert eine Schnittstelle, die alle von C abgeleitete Klassen auch implizit die Schnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="1cff5-263">When a class `C` directly implements an interface, all classes derived from C also implement the interface implicitly.</span></span> <span data-ttu-id="1cff5-264">Die Basisschnittstellen, die in einer Klassendeklaration angegeben können es sich um konstruierte Schnittstelle-Typen ([Typen konstruiert](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="1cff5-264">The base interfaces specified in a class declaration can be constructed interface types ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="1cff5-265">Eine Basisschnittstelle darf nicht Typparameter in ihren eigenen sein, obwohl er die Typparameter umfassen kann, die im Gültigkeitsbereich befinden.</span><span class="sxs-lookup"><span data-stu-id="1cff5-265">A base interface cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span> <span data-ttu-id="1cff5-266">Der folgende Code zeigt, wie eine Klasse implementieren und konstruierte Typen erweitern kann:</span><span class="sxs-lookup"><span data-stu-id="1cff5-266">The following code illustrates how a class can implement and extend constructed types:</span></span>
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

<span data-ttu-id="1cff5-267">Die Basisschnittstellen der Deklaration einer generischen Klasse erfüllen müssen, die Eindeutigkeit-Regel, die in beschriebenen [Eindeutigkeit der implementierten Schnittstellen](interfaces.md#uniqueness-of-implemented-interfaces).</span><span class="sxs-lookup"><span data-stu-id="1cff5-267">The base interfaces of a generic class declaration must satisfy the uniqueness rule described in [Uniqueness of implemented interfaces](interfaces.md#uniqueness-of-implemented-interfaces).</span></span>

### <a name="explicit-interface-member-implementations"></a><span data-ttu-id="1cff5-268">Explizite schnittstellenimplementierungen-Element</span><span class="sxs-lookup"><span data-stu-id="1cff5-268">Explicit interface member implementations</span></span>

<span data-ttu-id="1cff5-269">Für die Zwecke der Implementierung von Schnittstellen, eine Klasse oder Struktur deklarieren kann ***explizite Implementierungen eines Schnittstellenmembers***.</span><span class="sxs-lookup"><span data-stu-id="1cff5-269">For purposes of implementing interfaces, a class or struct may declare ***explicit interface member implementations***.</span></span> <span data-ttu-id="1cff5-270">Eine explizite Schnittstellenmember-Implementierung ist eine Methode, Eigenschaft, Ereignis, oder der Indexer-Deklaration, die einen vollständig qualifizierten Schnittstelle Membernamen verweist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-270">An explicit interface member implementation is a method, property, event, or indexer declaration that references a fully qualified interface member name.</span></span> <span data-ttu-id="1cff5-271">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1cff5-271">For example</span></span>
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

<span data-ttu-id="1cff5-272">Hier `IDictionary<int,T>.this` und `IDictionary<int,T>.Add` sind explizite Implementierungen eines Schnittstellenmembers.</span><span class="sxs-lookup"><span data-stu-id="1cff5-272">Here `IDictionary<int,T>.this` and `IDictionary<int,T>.Add` are explicit interface member implementations.</span></span>

<span data-ttu-id="1cff5-273">In einigen Fällen kann der Name eines Schnittstellenmembers für die implementierende Klasse, nicht in dem Fall den Schnittstellenmember implementiert werden kann, verwenden explizite Schnittstellenmember-Implementierung.</span><span class="sxs-lookup"><span data-stu-id="1cff5-273">In some cases, the name of an interface member may not be appropriate for the implementing class, in which case the interface member may be implemented using explicit interface member implementation.</span></span> <span data-ttu-id="1cff5-274">Implementieren einer Klasse, die eine Dateiabstraktion, die z. B. implementieren würde wahrscheinlich eine `Close` Memberfunktion, die die Auswirkungen der Freigabe der Ressource "File", und Implementieren der `Dispose` -Methode der der `IDisposable` Schnittstelle. dabei wird der expliziten Implementierung des Schnittstellenmembers:</span><span class="sxs-lookup"><span data-stu-id="1cff5-274">A class implementing a file abstraction, for example, would likely implement a `Close` member function that has the effect of releasing the file resource, and implement the `Dispose` method of the `IDisposable` interface using explicit interface member implementation:</span></span>
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

<span data-ttu-id="1cff5-275">Es ist nicht möglich, eine explizite Schnittstellenmember-Implementierung über dessen vollqualifizierten Namen in einen Methodenaufruf, der Zugriff auf Eigenschaften oder Indexerzugriff zugreifen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-275">It is not possible to access an explicit interface member implementation through its fully qualified name in a method invocation, property access, or indexer access.</span></span> <span data-ttu-id="1cff5-276">Eine explizite Schnittstellenmember-Implementierung kann nur über eine Schnittstelleninstanz zugegriffen werden, und wird in diesem Fall einfach durch den Membernamen verwiesen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-276">An explicit interface member implementation can only be accessed through an interface instance, and is in that case referenced simply by its member name.</span></span>

<span data-ttu-id="1cff5-277">Ist ein Fehler während der Kompilierung für eine explizite Schnittstellenmember-Implementierung Zugriffsmodifizierer eingeschlossen, und es ist ein Fehler während der Kompilierung, um die Modifizierer sind `abstract`, `virtual`, `override`, oder `static`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-277">It is a compile-time error for an explicit interface member implementation to include access modifiers, and it is a compile-time error to include the modifiers `abstract`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="1cff5-278">Explizite Implementierungen eines Schnittstellenmembers weisen verschiedene Eingabehilfen Merkmale als andere Elemente auf.</span><span class="sxs-lookup"><span data-stu-id="1cff5-278">Explicit interface member implementations have different accessibility characteristics than other members.</span></span> <span data-ttu-id="1cff5-279">Da explizite schnittstellenimplementierungen-Member nie über deren vollqualifizierter Name im Aufruf einer Methode oder ein Eigenschaftenzugriff zugänglich sind, sind sie in einem privaten Sinne.</span><span class="sxs-lookup"><span data-stu-id="1cff5-279">Because explicit interface member implementations are never accessible through their fully qualified name in a method invocation or a property access, they are in a sense private.</span></span> <span data-ttu-id="1cff5-280">Da sie über eine Schnittstelleninstanz zugegriffen werden kann, Sie sind jedoch in gewisser Hinsicht auch öffentliche.</span><span class="sxs-lookup"><span data-stu-id="1cff5-280">However, since they can be accessed through an interface instance, they are in a sense also public.</span></span>

<span data-ttu-id="1cff5-281">Explizite Implementierungen eines Schnittstellenmembers dienen zwei Hauptaufgaben:</span><span class="sxs-lookup"><span data-stu-id="1cff5-281">Explicit interface member implementations serve two primary purposes:</span></span>

*  <span data-ttu-id="1cff5-282">Da explizite Implementierungen eines Schnittstellenmembers nicht über die Klasse oder Struktur Instanzen zugänglich sind, können sie schnittstellenimplementierungen aus der öffentlichen Schnittstelle einer Klasse oder Struktur ausgeschlossen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-282">Because explicit interface member implementations are not accessible through class or struct instances, they allow interface implementations to be excluded from the public interface of a class or struct.</span></span> <span data-ttu-id="1cff5-283">Dies ist besonders nützlich, wenn eine Klasse oder Struktur implementiert eine interne Schnittstelle, die nicht von Interesse an einen Consumer diese Klasse oder Struktur ist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-283">This is particularly useful when a class or struct implements an internal interface that is of no interest to a consumer of that class or struct.</span></span>
*  <span data-ttu-id="1cff5-284">Explizite Implementierungen eines Schnittstellenmembers können zur Klärung von Schnittstellenmembern mit derselben Signatur.</span><span class="sxs-lookup"><span data-stu-id="1cff5-284">Explicit interface member implementations allow disambiguation of interface members with the same signature.</span></span> <span data-ttu-id="1cff5-285">Ohne explizite schnittstellenimplementierungen für Member werden einer Klasse oder Struktur nicht haben unterschiedliche Implementierungen der Schnittstellenmember mit derselben Signatur und den Rückgabetyp, als es unmöglich, dass eine Klasse oder Struktur wäre, haben eine beliebige Implementierung Geben Sie im Vorfeld für alle Schnittstellenmember mit derselben Signatur jedoch mit unterschiedlichen Typen zurück.</span><span class="sxs-lookup"><span data-stu-id="1cff5-285">Without explicit interface member implementations it would be impossible for a class or struct to have different implementations of interface members with the same signature and return type, as would it be impossible for a class or struct to have any implementation at all of interface members with the same signature but with different return types.</span></span>

<span data-ttu-id="1cff5-286">Für eine explizite Schnittstellenmember-Implementierung gültig ist muss die Klasse oder Struktur eine Schnittstelle in der Basisklassenliste Namen, die ein Element enthält, deren vollqualifizierten Namen, Typ und Parametertypen genau den explizite Schnittstellenmember übereinstimmen -Implementierung.</span><span class="sxs-lookup"><span data-stu-id="1cff5-286">For an explicit interface member implementation to be valid, the class or struct must name an interface in its base class list that contains a member whose fully qualified name, type, and parameter types exactly match those of the explicit interface member implementation.</span></span> <span data-ttu-id="1cff5-287">Daher in der folgenden Klasse</span><span class="sxs-lookup"><span data-stu-id="1cff5-287">Thus, in the following class</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
<span data-ttu-id="1cff5-288">die Deklaration von `IComparable.CompareTo` führt zu einem Kompilierzeitfehler, da `IComparable` ist nicht aufgeführt, in der Basisklassenliste der `Shape` und keine Basisschnittstelle `ICloneable`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-288">the declaration of `IComparable.CompareTo` results in a compile-time error because `IComparable` is not listed in the base class list of `Shape` and is not a base interface of `ICloneable`.</span></span> <span data-ttu-id="1cff5-289">Ebenso in den Deklarationen</span><span class="sxs-lookup"><span data-stu-id="1cff5-289">Likewise, in the declarations</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
<span data-ttu-id="1cff5-290">die Deklaration von `ICloneable.Clone` in `Ellipse` führt zu einem Kompilierzeitfehler, da `ICloneable` ist nicht in der Basisklassenliste der explizit aufgeführt `Ellipse`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-290">the declaration of `ICloneable.Clone` in `Ellipse` results in a compile-time error because `ICloneable` is not explicitly listed in the base class list of `Ellipse`.</span></span>

<span data-ttu-id="1cff5-291">Der vollqualifizierte Name eines Schnittstellenmembers muss die Schnittstelle referenzieren, in der der Member deklariert wurde.</span><span class="sxs-lookup"><span data-stu-id="1cff5-291">The fully qualified name of an interface member must reference the interface in which the member was declared.</span></span> <span data-ttu-id="1cff5-292">Daher in den Deklarationen</span><span class="sxs-lookup"><span data-stu-id="1cff5-292">Thus, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
<span data-ttu-id="1cff5-293">die Implementierung der explizite Schnittstellenmember `Paint` muss geschrieben werden, als `IControl.Paint`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-293">the explicit interface member implementation of `Paint` must be written as `IControl.Paint`.</span></span>

### <a name="uniqueness-of-implemented-interfaces"></a><span data-ttu-id="1cff5-294">Die Eindeutigkeit der implementierten Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="1cff5-294">Uniqueness of implemented interfaces</span></span>

<span data-ttu-id="1cff5-295">Die von der Deklaration eines generischen Typs implementierten Schnittstellen müssen eindeutig für alle möglichen konstruierte Typen bleiben.</span><span class="sxs-lookup"><span data-stu-id="1cff5-295">The interfaces implemented by a generic type declaration must remain unique for all possible constructed types.</span></span> <span data-ttu-id="1cff5-296">Ohne diese Regel wäre es nicht möglich, um zu bestimmen, die richtige Methode für bestimmte konstruierten Typen aufrufen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-296">Without this rule, it would be impossible to determine the correct method to call for certain constructed types.</span></span> <span data-ttu-id="1cff5-297">Nehmen wir beispielsweise an die Deklaration einer generischen Klasse darf wurden wie folgt geschrieben werden:</span><span class="sxs-lookup"><span data-stu-id="1cff5-297">For example, suppose a generic class declaration were permitted to be written as follows:</span></span>
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

<span data-ttu-id="1cff5-298">Dies zulässig wäre, wäre es nicht möglich, um zu bestimmen, welcher Code führen Sie im folgenden Fall:</span><span class="sxs-lookup"><span data-stu-id="1cff5-298">Were this permitted, it would be impossible to determine which code to execute in the following case:</span></span>
```csharp
I<int> x = new X<int,int>();
x.F();
```

<span data-ttu-id="1cff5-299">Um festzustellen, ob die Liste der Deklaration eines generischen Typs Interface gültig ist, werden die folgenden Schritte ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="1cff5-299">To determine if the interface list of a generic type declaration is valid, the following steps are performed:</span></span>

*  <span data-ttu-id="1cff5-300">Lassen Sie `L` werden die Liste der Schnittstellen, die direkt in eine generische Klasse, Struktur oder Schnittstellendeklaration angegebenen `C`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-300">Let `L` be the list of interfaces directly specified in a generic class, struct, or interface declaration `C`.</span></span>
*  <span data-ttu-id="1cff5-301">Hinzufügen zu `L` alle Basisschnittstellen die Schnittstellen, die bereits in `L`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-301">Add to `L` any base interfaces of the interfaces already in `L`.</span></span>
*  <span data-ttu-id="1cff5-302">Entfernen Sie Duplikate von `L`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-302">Remove any duplicates from `L`.</span></span>
*  <span data-ttu-id="1cff5-303">Eine mögliche von erstellten Typ erstellt `C` würde, nachdem die Typargumente ersetzt werden `L`, dazu führen, dass zwei Schnittstellen in `L` identisch ist, klicken Sie dann die Deklaration von `C` ist ungültig.</span><span class="sxs-lookup"><span data-stu-id="1cff5-303">If any possible constructed type created from `C` would, after type arguments are substituted into `L`, cause two interfaces in `L` to be identical, then the declaration of `C` is invalid.</span></span> <span data-ttu-id="1cff5-304">Einschränkung Deklarationen sind nicht berücksichtigt, wenn es sich bei allen mögliche konstruierte Typen zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="1cff5-304">Constraint declarations are not considered when determining all possible constructed types.</span></span>

<span data-ttu-id="1cff5-305">In der Klassendeklaration `X` oben die Schnittstellenliste `L` besteht aus `I<U>` und `I<V>`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-305">In the class declaration `X` above, the interface list `L` consists of `I<U>` and `I<V>`.</span></span> <span data-ttu-id="1cff5-306">Die Deklaration ist ungültig, da alle Typen mit erstellt `U` und `V` wird der gleiche Typ führt dazu, dass diese beiden Schnittstellen identische Typen sein.</span><span class="sxs-lookup"><span data-stu-id="1cff5-306">The declaration is invalid because any constructed type with `U` and `V` being the same type would cause these two interfaces to be identical types.</span></span>

<span data-ttu-id="1cff5-307">Es ist möglich, für die Schnittstellen, die auf verschiedenen Vererbungsebenen zur Vereinheitlichung angegeben:</span><span class="sxs-lookup"><span data-stu-id="1cff5-307">It is possible for interfaces specified at different inheritance levels to unify:</span></span>
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

<span data-ttu-id="1cff5-308">Dieser Code ist gültig, obwohl `Derived<U,V>` implementiert beide `I<U>` und `I<V>`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-308">This code is valid even though `Derived<U,V>` implements both `I<U>` and `I<V>`.</span></span> <span data-ttu-id="1cff5-309">Der code</span><span class="sxs-lookup"><span data-stu-id="1cff5-309">The code</span></span>
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
<span data-ttu-id="1cff5-310">Ruft die Methode in `Derived`, da `Derived<int,int>` effektiv neu implementiert `I<int>` ([erneute Schnittstellenimplementierung](interfaces.md#interface-re-implementation)).</span><span class="sxs-lookup"><span data-stu-id="1cff5-310">invokes the method in `Derived`, since `Derived<int,int>` effectively re-implements `I<int>` ([Interface re-implementation](interfaces.md#interface-re-implementation)).</span></span>

### <a name="implementation-of-generic-methods"></a><span data-ttu-id="1cff5-311">Implementierung der generischen Methoden</span><span class="sxs-lookup"><span data-stu-id="1cff5-311">Implementation of generic methods</span></span>

<span data-ttu-id="1cff5-312">Wenn eine generische Methode implizit implementiert eine Schnittstellenmethode erhalten, für jeden Typparameter der Methode muss in beiden Deklarationen entspricht (nach einem Schnittstellentyp Parameter werden durch die entsprechenden Typargumente ersetzt), in denen Einschränkungen Methode Typparameter werden durch die Ordnungspositionen, identifiziert, von links nach rechts.</span><span class="sxs-lookup"><span data-stu-id="1cff5-312">When a generic method implicitly implements an interface method, the constraints given for each method type parameter must be equivalent in both declarations (after any interface type parameters are replaced with the appropriate type arguments), where method type parameters are identified by ordinal positions, left to right.</span></span>

<span data-ttu-id="1cff5-313">Wenn eine generische Methode explizit eine Schnittstellenmethode implementiert, sind jedoch keine Einschränkungen für die implementierende Methode zulässig.</span><span class="sxs-lookup"><span data-stu-id="1cff5-313">When a generic method explicitly implements an interface method, however, no constraints are allowed on the implementing method.</span></span> <span data-ttu-id="1cff5-314">Die Einschränkungen werden stattdessen über die Schnittstellenmethode geerbt.</span><span class="sxs-lookup"><span data-stu-id="1cff5-314">Instead, the constraints are inherited from the interface method</span></span>

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

<span data-ttu-id="1cff5-315">Die Methode `C.F<T>` implizit implementiert `I<object,C,string>.F<T>`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-315">The method `C.F<T>` implicitly implements `I<object,C,string>.F<T>`.</span></span> <span data-ttu-id="1cff5-316">In diesem Fall `C.F<T>` ist nicht erforderlich (noch zulässig), geben Sie die Einschränkung `T:object` da `object` eine implizite Einschränkung für alle Typparameter spezifiziert ist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-316">In this case, `C.F<T>` is not required (nor permitted) to specify the constraint `T:object` since `object` is an implicit constraint on all type parameters.</span></span> <span data-ttu-id="1cff5-317">Die Methode `C.G<T>` implizit implementiert `I<object,C,string>.G<T>` , da die Einschränkungen mit den in der Schnittstelle übereinstimmen, nachdem die Typparameter der Schnittstelle durch die entsprechenden Typargumente ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="1cff5-317">The method `C.G<T>` implicitly implements `I<object,C,string>.G<T>` because the constraints match those in the interface, after the interface type parameters are replaced with the corresponding type arguments.</span></span> <span data-ttu-id="1cff5-318">Die Einschränkung für die Methode `C.H<T>` ist ein Fehler, da die Typen versiegelt (`string` in diesem Fall) kann nicht als Einschränkungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1cff5-318">The constraint for method `C.H<T>` is an error because sealed types (`string` in this case) cannot be used as constraints.</span></span> <span data-ttu-id="1cff5-319">Das Auslassen der Einschränkung wäre ein Fehler auch da Einschränkungen der Schnittstelle implizit methodenimplementierungen entsprechend erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="1cff5-319">Omitting the constraint would also be an error since constraints of implicit interface method implementations are required to match.</span></span> <span data-ttu-id="1cff5-320">Daher ist es nicht möglich, implizit implementieren `I<object,C,string>.H<T>`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-320">Thus, it is impossible to implicitly implement `I<object,C,string>.H<T>`.</span></span> <span data-ttu-id="1cff5-321">Dieser Schnittstellenmethode kann nur über eine explizite schnittstellenimplementierung für den Member implementiert werden:</span><span class="sxs-lookup"><span data-stu-id="1cff5-321">This interface method can only be implemented using an explicit interface member implementation:</span></span>
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

<span data-ttu-id="1cff5-322">In diesem Beispiel ruft die explizite Schnittstellenmember-Implementierung eine öffentliche Methode, die mit streng geringere Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-322">In this example, the explicit interface member implementation invokes a public method having strictly weaker constraints.</span></span> <span data-ttu-id="1cff5-323">Beachten Sie, dass der Zuweisung von `t` zu `s` gilt seit `T` erbt eine Einschränkung des `T:string`, auch wenn diese Einschränkung nicht ausgedrückt werden kann, im Quellcode verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="1cff5-323">Note that the assignment from `t` to `s` is valid since `T` inherits a constraint of `T:string`, even though this constraint is not expressible in source code.</span></span>

### <a name="interface-mapping"></a><span data-ttu-id="1cff5-324">Schnittstellenzuordnung</span><span class="sxs-lookup"><span data-stu-id="1cff5-324">Interface mapping</span></span>

<span data-ttu-id="1cff5-325">Eine Klasse oder Struktur muss Implementierungen aller Elemente der Schnittstellen bieten, die in der Basisklassenliste der Klasse oder Struktur aufgeführt sind.</span><span class="sxs-lookup"><span data-stu-id="1cff5-325">A class or struct must provide implementations of all members of the interfaces that are listed in the base class list of the class or struct.</span></span> <span data-ttu-id="1cff5-326">Der Prozess zum Auffinden von Implementierungen von Schnittstellenmembern in eine implementierende Klasse oder Struktur wird als bezeichnet ***schnittstellenzuordnung***.</span><span class="sxs-lookup"><span data-stu-id="1cff5-326">The process of locating implementations of interface members in an implementing class or struct is known as ***interface mapping***.</span></span>

<span data-ttu-id="1cff5-327">Schnittstellenzuordnung für eine Klasse oder Struktur `C` sucht eine Implementierung für jedes Element der einzelnen Schnittstellen, die in der Basisklassenliste des angegebenen `C`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-327">Interface mapping for a class or struct `C` locates an implementation for each member of each interface specified in the base class list of `C`.</span></span> <span data-ttu-id="1cff5-328">Die Implementierung der Member einer bestimmten Schnittstelle `I.M`, wobei `I` ist die Schnittstelle in dem das Element `M` deklariert wird, richtet sich nach der Untersuchung von jeder Klasse oder Struktur `S`, beginnend mit `C` und Wiederholen für jeder nachfolgenden Basisklasse von `C`, bis eine Übereinstimmung gefunden wird:</span><span class="sxs-lookup"><span data-stu-id="1cff5-328">The implementation of a particular interface member `I.M`, where `I` is the interface in which the member `M` is declared, is determined by examining each class or struct `S`, starting with `C` and repeating for each successive base class of `C`, until a match is located:</span></span>

*  <span data-ttu-id="1cff5-329">Wenn `S` enthält eine Deklaration des eine explizite Schnittstellenmember-Implementierung, die entspricht `I` und `M`, dann ist dieser Member die Implementierung der `I.M`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-329">If `S` contains a declaration of an explicit interface member implementation that matches `I` and `M`, then this member is the implementation of `I.M`.</span></span>
*  <span data-ttu-id="1cff5-330">Andernfalls gilt: Wenn `S` enthält eine Deklaration eines öffentlichen nicht statischen Members, der entspricht `M`, dann ist dieser Member die Implementierung der `I.M`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-330">Otherwise, if `S` contains a declaration of a non-static public member that matches `M`, then this member is the implementation of `I.M`.</span></span> <span data-ttu-id="1cff5-331">Falls mehr als ein Element entspricht, es nicht angegeben wird, ist welches Element die Implementierung der `I.M`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-331">If more than one member matches, it is unspecified which member is the implementation of `I.M`.</span></span> <span data-ttu-id="1cff5-332">Diese Situation kann nur auftreten, wenn `S` ein konstruierten Typs, wenn die beiden Elemente, wie in den generischen Typ deklariert verschiedene Signaturen aufweisen, aber die Typargumente stellen ihren Signaturen identisch, ist.</span><span class="sxs-lookup"><span data-stu-id="1cff5-332">This situation can only occur if `S` is a constructed type where the two members as declared in the generic type have different signatures, but the type arguments make their signatures identical.</span></span>

<span data-ttu-id="1cff5-333">Ein Fehler während der Kompilierung tritt auf, wenn Sie Implementierungen für alle Elemente aller Schnittstellen, die in der Basisklassenliste des angegebenen nicht `C`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-333">A compile-time error occurs if implementations cannot be located for all members of all interfaces specified in the base class list of `C`.</span></span> <span data-ttu-id="1cff5-334">Beachten Sie, dass die Member einer Schnittstelle die Elemente enthalten, die von Basisschnittstellen geerbt werden.</span><span class="sxs-lookup"><span data-stu-id="1cff5-334">Note that the members of an interface include those members that are inherited from base interfaces.</span></span>

<span data-ttu-id="1cff5-335">Zum Zweck der schnittstellenzuordnung, einen Klassenmember `A` ein Schnittstellenmembers entspricht `B` bei:</span><span class="sxs-lookup"><span data-stu-id="1cff5-335">For purposes of interface mapping, a class member `A` matches an interface member `B` when:</span></span>

*  <span data-ttu-id="1cff5-336">`A` und `B` sind Methoden, und der Name, Typ, und die Liste der formalen Parameter der `A` und `B` identisch sind.</span><span class="sxs-lookup"><span data-stu-id="1cff5-336">`A` and `B` are methods, and the name, type, and formal parameter lists of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="1cff5-337">`A` und `B` sind Eigenschaften, die Namen und Typ des `A` und `B` sind nahezu identisch, und `A` weist dieselben Accessoren als `B` (`A` ist zusätzliche Accessoren haben, wenn es sich nicht um eine explizite Schnittstelle ist zulässig. Memberimplementierung).</span><span class="sxs-lookup"><span data-stu-id="1cff5-337">`A` and `B` are properties, the name and type of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>
*  <span data-ttu-id="1cff5-338">`A` und `B` sind Ereignisse, Name und Typ des `A` und `B` identisch sind.</span><span class="sxs-lookup"><span data-stu-id="1cff5-338">`A` and `B` are events, and the name and type of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="1cff5-339">`A` und `B` werden Indexer, der Typ und die Listen der formalen Parameter `A` und `B` sind nahezu identisch, und `A` weist dieselben Accessoren als `B` (`A` zusätzliche Accessoren haben, ist dies nicht zulässig ist eine explizite Schnittstellenmember-Implementierung).</span><span class="sxs-lookup"><span data-stu-id="1cff5-339">`A` and `B` are indexers, the type and formal parameter lists of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>

<span data-ttu-id="1cff5-340">Wesentliche Auswirkungen auf den Mappingalgorithmus Schnittstelle sind:</span><span class="sxs-lookup"><span data-stu-id="1cff5-340">Notable implications of the interface mapping algorithm are:</span></span>

*  <span data-ttu-id="1cff5-341">Explizite schnittstellenimplementierungen-Element haben Vorrang gegenüber anderen Elementen in der gleichen Klasse oder Struktur, wenn Member der Klasse oder Struktur bestimmt wird, das einen Schnittstellenmember implementiert.</span><span class="sxs-lookup"><span data-stu-id="1cff5-341">Explicit interface member implementations take precedence over other members in the same class or struct when determining the class or struct member that implements an interface member.</span></span>
*  <span data-ttu-id="1cff5-342">Nicht öffentliche weder statische Elemente beteiligt schnittstellenzuordnung ab.</span><span class="sxs-lookup"><span data-stu-id="1cff5-342">Neither non-public nor static members participate in interface mapping.</span></span>

<span data-ttu-id="1cff5-343">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="1cff5-343">In the example</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
<span data-ttu-id="1cff5-344">die `ICloneable.Clone` Mitglied `C` wird die Implementierung der `Clone` in `ICloneable` da explizite Implementierungen eines Schnittstellenmembers gegenüber anderen Mitgliedern Vorrang.</span><span class="sxs-lookup"><span data-stu-id="1cff5-344">the `ICloneable.Clone` member of `C` becomes the implementation of `Clone` in `ICloneable` because explicit interface member implementations take precedence over other members.</span></span>

<span data-ttu-id="1cff5-345">Wenn eine Klasse oder Struktur zwei implementiert oder mehr Schnittstellen, die mit einem Element mit dem gleichen Namen, den Typ und die Parametertypen, ist es möglich, jede dieser Schnittstellenmember einen einzelnen Member der Klasse oder Struktur zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-345">If a class or struct implements two or more interfaces containing a member with the same name, type, and parameter types, it is possible to map each of those interface members onto a single class or struct member.</span></span> <span data-ttu-id="1cff5-346">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1cff5-346">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

<span data-ttu-id="1cff5-347">Hier ist die `Paint` Methoden von `IControl` und `IForm` zugeordnet sind, auf die `Paint` -Methode in der `Page`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-347">Here, the `Paint` methods of both `IControl` and `IForm` are mapped onto the `Paint` method in `Page`.</span></span> <span data-ttu-id="1cff5-348">Es ist natürlich auch möglich, separate, explizite schnittstellenimplementierungen von Membern für die zwei Methoden haben.</span><span class="sxs-lookup"><span data-stu-id="1cff5-348">It is of course also possible to have separate explicit interface member implementations for the two methods.</span></span>

<span data-ttu-id="1cff5-349">Wenn eine Klasse oder Struktur eine Schnittstelle, die ausgeblendete Elemente enthält implementiert, müssen unbedingt einige Member über explizite Implementierungen eines Schnittstellenmembers implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="1cff5-349">If a class or struct implements an interface that contains hidden members, then some members must necessarily be implemented through explicit interface member implementations.</span></span> <span data-ttu-id="1cff5-350">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1cff5-350">For example</span></span>
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

<span data-ttu-id="1cff5-351">Eine Implementierung dieser Schnittstelle benötigt mindestens eine explizite Schnittstellenmember-Implementierung, und würde einen der folgenden Formen annehmen</span><span class="sxs-lookup"><span data-stu-id="1cff5-351">An implementation of this interface would require at least one explicit interface member implementation, and would take one of the following forms</span></span>
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

<span data-ttu-id="1cff5-352">Wenn eine Klasse mehrere Schnittstellen, die die gleiche Basis-Schnittstelle verfügen implementiert, können nur eine Implementierung der Basisschnittstelle vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="1cff5-352">When a class implements multiple interfaces that have the same base interface, there can be only one implementation of the base interface.</span></span> <span data-ttu-id="1cff5-353">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="1cff5-353">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
<span data-ttu-id="1cff5-354">Es ist nicht möglich, separate Implementierungen für die `IControl` mit dem Namen in der Basisklassenliste der `IControl` geerbt `ITextBox`, und die `IControl` geerbt `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-354">it is not possible to have separate implementations for the `IControl` named in the base class list, the `IControl` inherited by `ITextBox`, and the `IControl` inherited by `IListBox`.</span></span> <span data-ttu-id="1cff5-355">Es gibt tatsächlich keiner separaten Identität für diese Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-355">Indeed, there is no notion of a separate identity for these interfaces.</span></span> <span data-ttu-id="1cff5-356">Vielmehr die Implementierungen der `ITextBox` und `IListBox` Teilen zusammen dieselbe Implementierung von `IControl`, und `ComboBox` gilt lediglich drei Schnittstellen implementieren `IControl`, `ITextBox`, und `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-356">Rather, the implementations of `ITextBox` and `IListBox` share the same implementation of `IControl`, and `ComboBox` is simply considered to implement three interfaces, `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="1cff5-357">Die Member einer Basisklasse beteiligt schnittstellenzuordnung ab.</span><span class="sxs-lookup"><span data-stu-id="1cff5-357">The members of a base class participate in interface mapping.</span></span> <span data-ttu-id="1cff5-358">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="1cff5-358">In the example</span></span>
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
<span data-ttu-id="1cff5-359">die Methode `F` in `Class1` werden in `Class2`Implementierung von `Interface1`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-359">the method `F` in `Class1` is used in `Class2`'s implementation of `Interface1`.</span></span>

### <a name="interface-implementation-inheritance"></a><span data-ttu-id="1cff5-360">Schnittstellenvererbung-Implementierung</span><span class="sxs-lookup"><span data-stu-id="1cff5-360">Interface implementation inheritance</span></span>

<span data-ttu-id="1cff5-361">Eine Klasse erbt alle schnittstellenimplementierungen, die von ihrer Basisklassen bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="1cff5-361">A class inherits all interface implementations provided by its base classes.</span></span>

<span data-ttu-id="1cff5-362">Ohne explizit ***erneut zu implementieren*** eine Schnittstelle, eine abgeleitete Klasse kann nicht in keiner Weise geändert werden die Mappings der Schnittstelle von der zugehörigen Basisklassen erbt.</span><span class="sxs-lookup"><span data-stu-id="1cff5-362">Without explicitly ***re-implementing*** an interface, a derived class cannot in any way alter the interface mappings it inherits from its base classes.</span></span> <span data-ttu-id="1cff5-363">Z. B. in den Deklarationen</span><span class="sxs-lookup"><span data-stu-id="1cff5-363">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
<span data-ttu-id="1cff5-364">die `Paint` -Methode in der `TextBox` Blendet Sie aus der `Paint` -Methode in der `Control`, es er ändert jedoch nicht die Zuordnung von `Control.Paint` auf `IControl.Paint`, und Aufrufe von `Paint` über Klasse Instanzen sowie Schnittstelleninstanzen werden die folgenden Auswirkungen haben</span><span class="sxs-lookup"><span data-stu-id="1cff5-364">the `Paint` method in `TextBox` hides the `Paint` method in `Control`, but it does not alter the mapping of `Control.Paint` onto `IControl.Paint`, and calls to `Paint` through class instances and interface instances will have the following effects</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

<span data-ttu-id="1cff5-365">Wenn eine Schnittstellenmethode auf eine virtuelle Methode in einer Klasse zugeordnet ist, ist es jedoch möglich, dass abgeleitete Klassen überschreiben die virtuelle Methode, und ändern Sie die Implementierung der Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="1cff5-365">However, when an interface method is mapped onto a virtual method in a class, it is possible for derived classes to override the virtual method and alter the implementation of the interface.</span></span> <span data-ttu-id="1cff5-366">Umschreiben z. B. die Deklarationen oben hinzu</span><span class="sxs-lookup"><span data-stu-id="1cff5-366">For example, rewriting the declarations above to</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
<span data-ttu-id="1cff5-367">die folgenden Auswirkungen werden jetzt beachtet werden soll</span><span class="sxs-lookup"><span data-stu-id="1cff5-367">the following effects will now be observed</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

<span data-ttu-id="1cff5-368">Da explizite Implementierungen eines Schnittstellenmembers virtuellen können nicht deklariert werden, ist es nicht möglich, eine explizite Schnittstellenmember-Implementierung zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="1cff5-368">Since explicit interface member implementations cannot be declared virtual, it is not possible to override an explicit interface member implementation.</span></span> <span data-ttu-id="1cff5-369">Allerdings uneingeschränkt für eine explizite Schnittstellenmember-Implementierung eine andere Methode aufruft, und, dass andere Methode virtuell deklariert werden kann abgeleitete Klassen, um sie zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="1cff5-369">However, it is perfectly valid for an explicit interface member implementation to call another method, and that other method can be declared virtual to allow derived classes to override it.</span></span> <span data-ttu-id="1cff5-370">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1cff5-370">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

<span data-ttu-id="1cff5-371">Hier von abgeleiteten Klassen `Control` kann die Implementierung der "specialize" `IControl.Paint` durch Überschreiben der `PaintControl` Methode.</span><span class="sxs-lookup"><span data-stu-id="1cff5-371">Here, classes derived from `Control` can specialize the implementation of `IControl.Paint` by overriding the `PaintControl` method.</span></span>

### <a name="interface-re-implementation"></a><span data-ttu-id="1cff5-372">Die erneute schnittstellenimplementierung</span><span class="sxs-lookup"><span data-stu-id="1cff5-372">Interface re-implementation</span></span>

<span data-ttu-id="1cff5-373">Eine Klasse, eine schnittstellenimplementierung erbt, ist zulässig, ***erneut implementieren*** der Schnittstelle durch Integrieren der Codekomponente in der Basisklassenliste.</span><span class="sxs-lookup"><span data-stu-id="1cff5-373">A class that inherits an interface implementation is permitted to ***re-implement*** the interface by including it in the base class list.</span></span>

<span data-ttu-id="1cff5-374">Eine erneute Implementierung einer Schnittstelle folgt genau die gleiche Schnittstelle Zuordnungsregeln an wie die erste Implementierung einer Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="1cff5-374">A re-implementation of an interface follows exactly the same interface mapping rules as an initial implementation of an interface.</span></span> <span data-ttu-id="1cff5-375">Daher wirkt sich die geerbte schnittstellenzuordnung nicht überhaupt auf die schnittstellenzuordnung für die erneute Implementierung der Schnittstelle hergestellt.</span><span class="sxs-lookup"><span data-stu-id="1cff5-375">Thus, the inherited interface mapping has no effect whatsoever on the interface mapping established for the re-implementation of the interface.</span></span> <span data-ttu-id="1cff5-376">Z. B. in den Deklarationen</span><span class="sxs-lookup"><span data-stu-id="1cff5-376">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
<span data-ttu-id="1cff5-377">die Tatsache, `Control` ordnet `IControl.Paint` auf `Control.IControl.Paint` wirkt sich nicht auf die erneute Implementierung von `MyControl`, welche Karten `IControl.Paint` auf `MyControl.Paint`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-377">the fact that `Control` maps `IControl.Paint` onto `Control.IControl.Paint` doesn't affect the re-implementation in `MyControl`, which maps `IControl.Paint` onto `MyControl.Paint`.</span></span>

<span data-ttu-id="1cff5-378">Geerbte öffentliche Memberdeklarationen und geerbten explizite Schnittstellenmember, die Deklarationen Teilnahme an der Schnittstelle Zuordnungsprozess für neu implementierten Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-378">Inherited public member declarations and inherited explicit interface member declarations participate in the interface mapping process for re-implemented interfaces.</span></span> <span data-ttu-id="1cff5-379">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1cff5-379">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

<span data-ttu-id="1cff5-380">Hier wird die Implementierung der `IMethods` in `Derived` ordnet die Schnittstellenmethoden `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, und `Base.I`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-380">Here, the implementation of `IMethods` in `Derived` maps the interface methods onto `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, and `Base.I`.</span></span>

<span data-ttu-id="1cff5-381">Wenn eine Klasse eine Schnittstelle, implementiert er implizit implementiert auch alle diese Basisschnittstellen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-381">When a class implements an interface, it implicitly also implements all of that interface's base interfaces.</span></span> <span data-ttu-id="1cff5-382">Ebenso ist eine erneute Implementierung einer Schnittstelle auch implizit eine erneute Implementierung der Basisschnittstellen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-382">Likewise, a re-implementation of an interface is also implicitly a re-implementation of all of the interface's base interfaces.</span></span> <span data-ttu-id="1cff5-383">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1cff5-383">For example</span></span>
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

<span data-ttu-id="1cff5-384">Hier wird die erneute Implementierung von `IDerived` erneut implementiert auch `IBase`und ordnet `IBase.F` auf `D.F`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-384">Here, the re-implementation of `IDerived` also re-implements `IBase`, mapping `IBase.F` onto `D.F`.</span></span>

### <a name="abstract-classes-and-interfaces"></a><span data-ttu-id="1cff5-385">Abstrakte Klassen und Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="1cff5-385">Abstract classes and interfaces</span></span>

<span data-ttu-id="1cff5-386">Wie einer nicht abstrakten Klasse muss eine abstrakte Klasse Implementierungen von allen Membern der Schnittstellen bereitstellen, die in der Basisklassenliste der Klasse aufgeführt sind.</span><span class="sxs-lookup"><span data-stu-id="1cff5-386">Like a non-abstract class, an abstract class must provide implementations of all members of the interfaces that are listed in the base class list of the class.</span></span> <span data-ttu-id="1cff5-387">Allerdings ist eine abstrakte Klasse zulässig, Schnittstellenmethoden abstrakte Methoden zuordnen.</span><span class="sxs-lookup"><span data-stu-id="1cff5-387">However, an abstract class is permitted to map interface methods onto abstract methods.</span></span> <span data-ttu-id="1cff5-388">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1cff5-388">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

<span data-ttu-id="1cff5-389">Hier wird die Implementierung der `IMethods` ordnet `F` und `G` auf abstrakte Methoden, die in der nicht abstrakte abgeleitete Klassen überschrieben werden muss `C`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-389">Here, the implementation of `IMethods` maps `F` and `G` onto abstract methods, which must be overridden in non-abstract classes that derive from `C`.</span></span>

<span data-ttu-id="1cff5-390">Beachten Sie, dass explizite schnittstellenimplementierungen-Element darf nicht abstrakt sein, aber die explizite Implementierungen eines Schnittstellenmembers sind natürlich abstrakte Methoden aufrufen darf.</span><span class="sxs-lookup"><span data-stu-id="1cff5-390">Note that explicit interface member implementations cannot be abstract, but explicit interface member implementations are of course permitted to call abstract methods.</span></span> <span data-ttu-id="1cff5-391">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1cff5-391">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

<span data-ttu-id="1cff5-392">Hier wird von abgeleiteten nicht abstrakten Klassen `C` wäre erforderlich, außer Kraft setzen `FF` und `GG`, dadurch wird die tatsächliche Implementierung der `IMethods`.</span><span class="sxs-lookup"><span data-stu-id="1cff5-392">Here, non-abstract classes that derive from `C` would be required to override `FF` and `GG`, thus providing the actual implementation of `IMethods`.</span></span>
