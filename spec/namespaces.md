# <a name="namespaces"></a><span data-ttu-id="97cde-101">Namespaces</span><span class="sxs-lookup"><span data-stu-id="97cde-101">Namespaces</span></span>

<span data-ttu-id="97cde-102">C#-Programme werden mithilfe von Namespaces organisiert.</span><span class="sxs-lookup"><span data-stu-id="97cde-102">C# programs are organized using namespaces.</span></span> <span data-ttu-id="97cde-103">Namespaces werden verwendet, sowohl als "intern" Organisation System für ein Programm, und als "extern" Organisation System – eine Möglichkeit zum Darstellen der Programmelemente, die für andere Programme verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="97cde-103">Namespaces are used both as an "internal" organization system for a program, and as an "external" organization system—a way of presenting program elements that are exposed to other programs.</span></span>

<span data-ttu-id="97cde-104">Using-Direktiven ([Using-Direktiven](namespaces.md#using-directives)) werden bereitgestellt, um die Verwendung von Namespaces zu erleichtern.</span><span class="sxs-lookup"><span data-stu-id="97cde-104">Using directives ([Using directives](namespaces.md#using-directives)) are provided to facilitate the use of namespaces.</span></span>

## <a name="compilation-units"></a><span data-ttu-id="97cde-105">Kompilierungseinheiten hinweg</span><span class="sxs-lookup"><span data-stu-id="97cde-105">Compilation units</span></span>

<span data-ttu-id="97cde-106">Ein *Compilation_unit* die allgemeine Struktur einer Quelldatei definiert.</span><span class="sxs-lookup"><span data-stu-id="97cde-106">A *compilation_unit* defines the overall structure of a source file.</span></span> <span data-ttu-id="97cde-107">Eine Kompilierungseinheit besteht aus 0 (null) oder mehreren *Using_directive*s, gefolgt von NULL oder mehr *Global_attributes* gefolgt von 0 (null) oder mehr *Namespace_member_declaration*s .</span><span class="sxs-lookup"><span data-stu-id="97cde-107">A compilation unit consists of zero or more *using_directive*s followed by zero or more *global_attributes* followed by zero or more *namespace_member_declaration*s.</span></span>

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

<span data-ttu-id="97cde-108">Ein C#-Programm besteht aus einen oder mehrere Kompilierungseinheiten hinweg, jeweils in einer separaten Quelldatei enthalten.</span><span class="sxs-lookup"><span data-stu-id="97cde-108">A C# program consists of one or more compilation units, each contained in a separate source file.</span></span> <span data-ttu-id="97cde-109">Wenn ein C#-Programm kompiliert wird, werden alle von der Kompilierungseinheiten zusammen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="97cde-109">When a C# program is compiled, all of the compilation units are processed together.</span></span> <span data-ttu-id="97cde-110">Daher können Kompilierungseinheiten voneinander, möglicherweise in einer kreisförmigen Weise abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="97cde-110">Thus, compilation units can depend on each other, possibly in a circular fashion.</span></span>

<span data-ttu-id="97cde-111">Die *Using_directive*s eine Kompilierung Einheit beeinflussen die *Global_attributes* und *Namespace_member_declaration*s Kompilationseinheit, haben jedoch keine Auswirkungen auf andere Kompilierungseinheiten hinweg.</span><span class="sxs-lookup"><span data-stu-id="97cde-111">The *using_directive*s of a compilation unit affect the *global_attributes* and *namespace_member_declaration*s of that compilation unit, but have no effect on other compilation units.</span></span>

<span data-ttu-id="97cde-112">Die *Global_attributes* ([Attribute](attributes.md)) von einer Kompilierungseinheit ermöglichen die Angabe von Attributen für die Ziel-Assembly und Modul.</span><span class="sxs-lookup"><span data-stu-id="97cde-112">The *global_attributes* ([Attributes](attributes.md)) of a compilation unit permit the specification of attributes for the target assembly and module.</span></span> <span data-ttu-id="97cde-113">Fungieren als physische Container für Typen, Assemblys und Modulen.</span><span class="sxs-lookup"><span data-stu-id="97cde-113">Assemblies and modules act as physical containers for types.</span></span> <span data-ttu-id="97cde-114">Eine Assembly bestehen aus mehreren physisch separaten Modulen.</span><span class="sxs-lookup"><span data-stu-id="97cde-114">An assembly may consist of several physically separate modules.</span></span>

<span data-ttu-id="97cde-115">Die *Namespace_member_declaration*s der einzelnen Kompilierungseinheiten eines Programms tragen Member zu einer einzelnen Deklarationsabschnitt den globalen Namespace bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="97cde-115">The *namespace_member_declaration*s of each compilation unit of a program contribute members to a single declaration space called the global namespace.</span></span> <span data-ttu-id="97cde-116">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="97cde-116">For example:</span></span>

<span data-ttu-id="97cde-117">Datei `A.cs`:</span><span class="sxs-lookup"><span data-stu-id="97cde-117">File `A.cs`:</span></span>
```csharp
class A {}
```

<span data-ttu-id="97cde-118">Datei `B.cs`:</span><span class="sxs-lookup"><span data-stu-id="97cde-118">File `B.cs`:</span></span>
```csharp
class B {}
```

<span data-ttu-id="97cde-119">Die zwei Kompilierungseinheiten beitragen, auf die einzelnen globalen Namespace deklarieren zwei Klassen mit den vollqualifizierten Namen in diesem Fall `A` und `B`.</span><span class="sxs-lookup"><span data-stu-id="97cde-119">The two compilation units contribute to the single global namespace, in this case declaring two classes with the fully qualified names `A` and `B`.</span></span> <span data-ttu-id="97cde-120">Da die zwei Kompilierungseinheiten zum selben Deklarationsabschnitt beitragen möchten, wäre ein Fehler gewesen, wenn jeweils eine Deklaration eines Elements mit dem gleichen Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="97cde-120">Because the two compilation units contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="namespace-declarations"></a><span data-ttu-id="97cde-121">Namespacedeklarationen</span><span class="sxs-lookup"><span data-stu-id="97cde-121">Namespace declarations</span></span>

<span data-ttu-id="97cde-122">Ein *Namespace_declaration* besteht aus dem Schlüsselwort `namespace`, gefolgt von einem Namespace-Name und der Text, optional gefolgt von einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="97cde-122">A *namespace_declaration* consists of the keyword `namespace`, followed by a namespace name and body, optionally followed by a semicolon.</span></span>

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

<span data-ttu-id="97cde-123">Ein *Namespace_declaration* auftreten als Deklaration auf oberster Ebene in einer *Compilation_unit* oder als eine Memberdeklaration innerhalb einer anderen *Namespace_declaration*.</span><span class="sxs-lookup"><span data-stu-id="97cde-123">A *namespace_declaration* may occur as a top-level declaration in a *compilation_unit* or as a member declaration within another *namespace_declaration*.</span></span> <span data-ttu-id="97cde-124">Wenn eine *Namespace_declaration* tritt ein, wenn eine Deklaration mit der höchsten Ebene in eine *Compilation_unit*, aus dem Namespace wird ein Member des globalen Namespaces.</span><span class="sxs-lookup"><span data-stu-id="97cde-124">When a *namespace_declaration* occurs as a top-level declaration in a *compilation_unit*, the namespace becomes a member of the global namespace.</span></span> <span data-ttu-id="97cde-125">Wenn eine *Namespace_declaration* tritt auf, in einer anderen *Namespace_declaration*, aus dem innere Namespace wird ein Mitglied der äußere Namespace.</span><span class="sxs-lookup"><span data-stu-id="97cde-125">When a *namespace_declaration* occurs within another *namespace_declaration*, the inner namespace becomes a member of the outer namespace.</span></span> <span data-ttu-id="97cde-126">In beiden Fällen muss der Name eines Namespace innerhalb des enthaltenden Namespaces eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="97cde-126">In either case, the name of a namespace must be unique within the containing namespace.</span></span>

<span data-ttu-id="97cde-127">Namespaces sind implizit `public` und die Deklaration eines Namespace kann keine Zugriffsmodifizierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="97cde-127">Namespaces are implicitly `public` and the declaration of a namespace cannot include any access modifiers.</span></span>

<span data-ttu-id="97cde-128">Innerhalb einer *Namespace_body*, den optionalen *Using_directive*s importieren Sie die Namen der anderen Namespaces, Typen und Member, sodass sie direkt anstelle von über qualifizierte Namen verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="97cde-128">Within a *namespace_body*, the optional *using_directive*s import the names of other namespaces, types and members, allowing them to be referenced directly instead of through qualified names.</span></span> <span data-ttu-id="97cde-129">Der optionale *Namespace_member_declaration*s tragen Member in den Deklarationsbereich des Namespaces.</span><span class="sxs-lookup"><span data-stu-id="97cde-129">The optional *namespace_member_declaration*s contribute members to the declaration space of the namespace.</span></span> <span data-ttu-id="97cde-130">Beachten Sie, dass alle *Using_directive*s muss vor jeglichen Memberdeklarationen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="97cde-130">Note that all *using_directive*s must appear before any member declarations.</span></span>

<span data-ttu-id="97cde-131">Die *Qualified_identifier* von einem *Namespace_declaration* möglicherweise einen einzelnen Bezeichner oder eine Sequenz von Bezeichnern, getrennt durch "`.`" Token.</span><span class="sxs-lookup"><span data-stu-id="97cde-131">The *qualified_identifier* of a *namespace_declaration* may be a single identifier or a sequence of identifiers separated by "`.`" tokens.</span></span> <span data-ttu-id="97cde-132">Die letztgenannte Form lässt es sich um ein Programm, um einem geschachtelten Namespace definieren, ohne die lexikalische Schachtelung mehrere Namespacedeklarationen.</span><span class="sxs-lookup"><span data-stu-id="97cde-132">The latter form permits a program to define a nested namespace without lexically nesting several namespace declarations.</span></span> <span data-ttu-id="97cde-133">Ein auf ein Objekt angewendeter</span><span class="sxs-lookup"><span data-stu-id="97cde-133">For example,</span></span>

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
<span data-ttu-id="97cde-134">ist semantisch gleichwertig mit</span><span class="sxs-lookup"><span data-stu-id="97cde-134">is semantically equivalent to</span></span>
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

<span data-ttu-id="97cde-135">Namespaces haben, und zwei Namespacedeklarationen mit den gleichen vollqualifizierten Namen tragen zum selben Deklarationsabschnitt ([Deklarationen](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="97cde-135">Namespaces are open-ended, and two namespace declarations with the same fully qualified name contribute to the same declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="97cde-136">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="97cde-136">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
<span data-ttu-id="97cde-137">die beiden oben genannten Namespacedeklarationen beitragen zum selben Deklarationsabschnitt, deklarieren zwei Klassen mit den vollqualifizierten Namen in diesem Fall `N1.N2.A` und `N1.N2.B`.</span><span class="sxs-lookup"><span data-stu-id="97cde-137">the two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `N1.N2.A` and `N1.N2.B`.</span></span> <span data-ttu-id="97cde-138">Da die beiden Deklarationen zum selben Deklarationsabschnitt beitragen möchten, wäre gewesen, dass ein Fehler, wenn jeder eine Deklaration eines Elements mit dem gleichen Namen enthalten.</span><span class="sxs-lookup"><span data-stu-id="97cde-138">Because the two declarations contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="extern-aliases"></a><span data-ttu-id="97cde-139">Extern-Aliase</span><span class="sxs-lookup"><span data-stu-id="97cde-139">Extern aliases</span></span>

<span data-ttu-id="97cde-140">Ein *Extern_alias_directive* führt einen Bezeichner, der als Alias für einen Namespace fungiert.</span><span class="sxs-lookup"><span data-stu-id="97cde-140">An *extern_alias_directive* introduces an identifier that serves as an alias for a namespace.</span></span> <span data-ttu-id="97cde-141">Die Spezifikation des Aliasnamespace als für den Quellcode des Programms extern ist und gilt auch für geschachtelte Namespaces des Namespace mit einem Alias versehen.</span><span class="sxs-lookup"><span data-stu-id="97cde-141">The specification of the aliased namespace is external to the source code of the program and applies also to nested namespaces of the aliased namespace.</span></span>

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

<span data-ttu-id="97cde-142">Im Rahmen einer *Extern_alias_directive* erstreckt sich über die *Using_directive*s, *Global_attributes* und *Namespace_member_declaration*s des direkt enthaltenden Kompilierung oder Text.</span><span class="sxs-lookup"><span data-stu-id="97cde-142">The scope of an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span>

<span data-ttu-id="97cde-143">Innerhalb eines Kompilierung Einheit oder Namespace, der enthält ein *Extern_alias_directive*, der Bezeichner wurde eingeführt, durch die *Extern_alias_directive* können verwendet werden, um den Aliasnamespace als zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="97cde-143">Within a compilation unit or namespace body that contains an *extern_alias_directive*, the identifier introduced by the *extern_alias_directive* can be used to reference the aliased namespace.</span></span> <span data-ttu-id="97cde-144">Es ist ein Fehler während der Kompilierung für die *Bezeichner* das Wort sein `global`.</span><span class="sxs-lookup"><span data-stu-id="97cde-144">It is a compile-time error for the *identifier* to be the word `global`.</span></span>

<span data-ttu-id="97cde-145">Ein *Extern_alias_directive* stellt ein alias zur Verfügung, wenn es innerhalb eines bestimmten Kompilierung Einheit oder Namespace, aber es trägt nicht die neuen Member zu den zugrunde liegenden Deklarationsabschnitt.</span><span class="sxs-lookup"><span data-stu-id="97cde-145">An *extern_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="97cde-146">Das heißt, eine *Extern_alias_directive* ist nicht transitiv, sondern nur die Kompilierung Einheit oder Namespacetext in dem er auftritt, beeinflusst.</span><span class="sxs-lookup"><span data-stu-id="97cde-146">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>

<span data-ttu-id="97cde-147">Das folgende Programm deklariert und verwendet zwei "extern" Aliase `X` und `Y`, die jeweils von den Stamm einer Namespacehierarchie unterschiedliche darstellt:</span><span class="sxs-lookup"><span data-stu-id="97cde-147">The following program declares and uses two extern aliases, `X` and `Y`, each of which represent the root of a distinct namespace hierarchy:</span></span>
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

<span data-ttu-id="97cde-148">Das Programm wird das Vorhandensein der "extern" deklariert Aliase `X` und `Y`, aber die tatsächlichen Definitionen der Aliase für die Anwendung extern sind.</span><span class="sxs-lookup"><span data-stu-id="97cde-148">The program declares the existence of the extern aliases `X` and `Y`, but the actual definitions of the aliases are external to the program.</span></span> <span data-ttu-id="97cde-149">Dem identisch benannten `N.B` Klassen können jetzt als verwiesen werden `X.N.B` und `Y.N.B`, oder Verwenden des Namespacealias-Qualifizierers, `X::N.B` und `Y::N.B`.</span><span class="sxs-lookup"><span data-stu-id="97cde-149">The identically named `N.B` classes can now be referenced as `X.N.B` and `Y.N.B`, or, using the namespace alias qualifier, `X::N.B` and `Y::N.B`.</span></span> <span data-ttu-id="97cde-150">Ein Fehler tritt auf, wenn ein Programm einen externen Alias deklariert, für den keine externe Definition bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="97cde-150">An error occurs if a program declares an extern alias for which no external definition is provided.</span></span>

## <a name="using-directives"></a><span data-ttu-id="97cde-151">using-Direktiven</span><span class="sxs-lookup"><span data-stu-id="97cde-151">Using directives</span></span>

<span data-ttu-id="97cde-152">***Using-Direktiven*** vereinfachen die Nutzung von Namespaces und Typen, die in anderen Namespaces definiert.</span><span class="sxs-lookup"><span data-stu-id="97cde-152">***Using directives*** facilitate the use of namespaces and types defined in other namespaces.</span></span> <span data-ttu-id="97cde-153">Mithilfe von Direktiven Auswirkungen auf die namensauflösung von *Namespace_or_type_name*s ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)) und *Simple_name*s ([einfache Namen ](expressions.md#simple-names)), aber im Gegensatz zu Deklarationen, die using-Direktiven tragen nicht die neuen Elemente in der zugrunde liegenden Deklaration Leerzeichen der Kompilierungseinheiten oder Namespaces, die in dem sie verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="97cde-153">Using directives impact the name resolution process of *namespace_or_type_name*s ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) and *simple_name*s ([Simple names](expressions.md#simple-names)), but unlike declarations, using directives do not contribute new members to the underlying declaration spaces of the compilation units or namespaces within which they are used.</span></span>

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

<span data-ttu-id="97cde-154">Ein *Using_alias_directive* ([mithilfe von Direktiven für Namespacealiase](namespaces.md#using-alias-directives)) führt einen Alias für einen Namespace oder Typ.</span><span class="sxs-lookup"><span data-stu-id="97cde-154">A *using_alias_directive* ([Using alias directives](namespaces.md#using-alias-directives)) introduces an alias for a namespace or type.</span></span>

<span data-ttu-id="97cde-155">Ein *Using_namespace_directive* ([Using namespacedirektiven](namespaces.md#using-namespace-directives)) Typmember von einem Namespace importiert.</span><span class="sxs-lookup"><span data-stu-id="97cde-155">A *using_namespace_directive* ([Using namespace directives](namespaces.md#using-namespace-directives)) imports the type members of a namespace.</span></span>

<span data-ttu-id="97cde-156">Ein *Using_static_directive* ([Using static-Direktiven](namespaces.md#using-static-directives)) importiert die geschachtelten Typen und statische Member eines Typs.</span><span class="sxs-lookup"><span data-stu-id="97cde-156">A *using_static_directive* ([Using static directives](namespaces.md#using-static-directives)) imports the nested types and static members of a type.</span></span>

<span data-ttu-id="97cde-157">Im Rahmen einer *Using_directive* erstreckt sich über die *Namespace_member_declaration*s des direkt enthaltenden Kompilierung oder Text.</span><span class="sxs-lookup"><span data-stu-id="97cde-157">The scope of a *using_directive* extends over the *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="97cde-158">Im Rahmen einer *Using_directive* beinhalten insbesondere nicht die gleichgeordnete *Using_directive*s.</span><span class="sxs-lookup"><span data-stu-id="97cde-158">The scope of a *using_directive* specifically does not include its peer *using_directive*s.</span></span> <span data-ttu-id="97cde-159">Daher peer *Using_directive*s haben keinen Einfluss auf einander und spielt die Reihenfolge, in der sie geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="97cde-159">Thus, peer *using_directive*s do not affect each other, and the order in which they are written is insignificant.</span></span>

### <a name="using-alias-directives"></a><span data-ttu-id="97cde-160">Using-Alias-Direktiven</span><span class="sxs-lookup"><span data-stu-id="97cde-160">Using alias directives</span></span>

<span data-ttu-id="97cde-161">Ein *Using_alias_directive* führt einen Bezeichner, der als Alias für einen Namespace oder Typ, in dem unmittelbar einschließenden Kompilierung Einheit oder Namespacetext dient.</span><span class="sxs-lookup"><span data-stu-id="97cde-161">A *using_alias_directive* introduces an identifier that serves as an alias for a namespace or type within the immediately enclosing compilation unit or namespace body.</span></span>

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

<span data-ttu-id="97cde-162">Innerhalb der Memberdeklarationen in einer Kompilierung Einheit oder Namespacetext, die enthält eine *Using_alias_directive*, der Bezeichner wurde eingeführt, durch die *Using_alias_directive* kann verwendet werden, zu verweisen der angegebenen Namespace oder Typ.</span><span class="sxs-lookup"><span data-stu-id="97cde-162">Within member declarations in a compilation unit or namespace body that contains a *using_alias_directive*, the identifier introduced by the *using_alias_directive* can be used to reference the given namespace or type.</span></span> <span data-ttu-id="97cde-163">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="97cde-163">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

<span data-ttu-id="97cde-164">Oben in den Memberdeklarationen in der `N3` Namespace `A` ist ein Alias für `N1.N2.A`, und daher `N3.B` von Klasse abgeleitet ist `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="97cde-164">Above, within member declarations in the `N3` namespace, `A` is an alias for `N1.N2.A`, and thus class `N3.B` derives from class `N1.N2.A`.</span></span> <span data-ttu-id="97cde-165">Die gleiche Wirkung erhalten Sie durch Erstellen eines Alias `R` für `N1.N2` verweisen Sie einfach auf `R.A`:</span><span class="sxs-lookup"><span data-stu-id="97cde-165">The same effect can be obtained by creating an alias `R` for `N1.N2` and then referencing `R.A`:</span></span>
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

<span data-ttu-id="97cde-166">Die *Bezeichner* von einem *Using_alias_directive* muss innerhalb des Deklarationsabschnitts der Kompilierungseinheit oder Namespace, der sofort enthält eindeutig sein der *Using_alias_directive* .</span><span class="sxs-lookup"><span data-stu-id="97cde-166">The *identifier* of a *using_alias_directive* must be unique within the declaration space of the compilation unit or namespace that immediately contains the *using_alias_directive*.</span></span> <span data-ttu-id="97cde-167">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="97cde-167">For example:</span></span>
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

<span data-ttu-id="97cde-168">Oben `N3` enthält bereits einen Member `A`, daher wird ein Fehler während der Kompilierung für eine *Using_alias_directive* dieses Bezeichners zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="97cde-168">Above, `N3` already contains a member `A`, so it is a compile-time error for a *using_alias_directive* to use that identifier.</span></span> <span data-ttu-id="97cde-169">Entsprechend ist es ein Fehler während der Kompilierung für zwei oder mehr *Using_alias_directive*s in der gleichen Kompilierung Einheit oder Namespacetext Aliase mit demselben Namen zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="97cde-169">Likewise, it is a compile-time error for two or more *using_alias_directive*s in the same compilation unit or namespace body to declare aliases by the same name.</span></span>

<span data-ttu-id="97cde-170">Ein *Using_alias_directive* stellt ein alias zur Verfügung, wenn es innerhalb eines bestimmten Kompilierung Einheit oder Namespace, aber es trägt nicht die neuen Member zu den zugrunde liegenden Deklarationsabschnitt.</span><span class="sxs-lookup"><span data-stu-id="97cde-170">A *using_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="97cde-171">Das heißt, eine *Using_alias_directive* ist nicht transitiv, sondern beeinflusst nur die Kompilierung Einheit oder Namespacetext in dem er auftritt.</span><span class="sxs-lookup"><span data-stu-id="97cde-171">In other words, a *using_alias_directive* is not transitive but rather affects only the compilation unit or namespace body in which it occurs.</span></span> <span data-ttu-id="97cde-172">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="97cde-172">In the example</span></span>
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
<span data-ttu-id="97cde-173">im Rahmen der *Using_alias_directive* eingeführt `R` erstreckt sich auch nur auf Deklarationen im Hauptteil Namespace, in dem es enthalten ist, damit `R` in die zweite Namespacedeklaration nicht bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="97cde-173">the scope of the *using_alias_directive* that introduces `R` only extends to member declarations in the namespace body in which it is contained, so `R` is unknown in the second namespace declaration.</span></span> <span data-ttu-id="97cde-174">Platzieren Sie jedoch die *Using_alias_directive* in der enthaltenden Kompilierung führt Komponententests den Alias in den beiden Namespacedeklarationen verfügbar sind:</span><span class="sxs-lookup"><span data-stu-id="97cde-174">However, placing the *using_alias_directive* in the containing compilation unit causes the alias to become available within both namespace declarations:</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

<span data-ttu-id="97cde-175">Genau wie reguläre Elemente Namen eingeführt, durch *Using_alias_directive*s durch ähnlich benannten Elemente in geschachtelte Bereiche ausgeblendet sind.</span><span class="sxs-lookup"><span data-stu-id="97cde-175">Just like regular members, names introduced by *using_alias_directive*s are hidden by similarly named members in nested scopes.</span></span> <span data-ttu-id="97cde-176">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="97cde-176">In the example</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
<span data-ttu-id="97cde-177">der Verweis auf `R.A` in der Deklaration der `B` verursacht einen Kompilierungsfehler, da `R` bezieht sich auf `N3.R`, nicht `N1.N2`.</span><span class="sxs-lookup"><span data-stu-id="97cde-177">the reference to `R.A` in the declaration of `B` causes a compile-time error because `R` refers to `N3.R`, not `N1.N2`.</span></span>

<span data-ttu-id="97cde-178">Die Reihenfolge, in der *Using_alias_directive*s werden geschrieben, verfügt über keine Bedeutung und Auflösung von der *Namespace_or_type_name* verwiesen wird, indem eine *Using_alias_directive*ist nicht betroffen von dem *Using_alias_directive* selbst oder von anderen *Using_directive*s im direkt enthaltenden Kompilierung oder Text.</span><span class="sxs-lookup"><span data-stu-id="97cde-178">The order in which *using_alias_directive*s are written has no significance, and resolution of the *namespace_or_type_name* referenced by a *using_alias_directive* is not affected by the *using_alias_directive* itself or by other *using_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="97cde-179">Das heißt, die *Namespace_or_type_name* von einer *Using_alias_directive* wird aufgelöst, als hätte der direkt enthaltende Kompilierung Einheit oder Namespacetext keine *Using_directive*s.</span><span class="sxs-lookup"><span data-stu-id="97cde-179">In other words, the *namespace_or_type_name* of a *using_alias_directive* is resolved as if the immediately containing compilation unit or namespace body had no *using_directive*s.</span></span> <span data-ttu-id="97cde-180">Ein *Using_alias_directive* jedoch möglicherweise betroffen *Extern_alias_directive*s im direkt enthaltenden Kompilierung oder Text.</span><span class="sxs-lookup"><span data-stu-id="97cde-180">A *using_alias_directive* may however be affected by *extern_alias_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="97cde-181">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="97cde-181">In the example</span></span>
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
<span data-ttu-id="97cde-182">die letzte *Using_alias_directive* führt ein Fehler während der Kompilierung, da es nicht, vom ersten betroffen ist *Using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="97cde-182">the last *using_alias_directive* results in a compile-time error because it is not affected by the first *using_alias_directive*.</span></span> <span data-ttu-id="97cde-183">Die erste *Using_alias_directive* führt nicht zu einem Fehler seit den Rahmen der externe Alias `E` enthält die *Using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="97cde-183">The first *using_alias_directive* does not result in an error since the scope of the extern alias `E` includes the *using_alias_directive*.</span></span>

<span data-ttu-id="97cde-184">Ein *Using_alias_directive* erstellen einen Alias für jeden Namespace oder Typ, einschließlich des Namespace, in dem er angezeigt wird, und jeden Namespace oder Typ, die in diesem Namespace geschachtelt sind.</span><span class="sxs-lookup"><span data-stu-id="97cde-184">A *using_alias_directive* can create an alias for any namespace or type, including the namespace within which it appears and any namespace or type nested within that namespace.</span></span>

<span data-ttu-id="97cde-185">Zugreifen auf einen Namespace oder Typ über einen Alias führt genau dieselbe Ergebnisse wie beim Zugriff auf diesen Namespace oder Typ durch den deklarierten Namen.</span><span class="sxs-lookup"><span data-stu-id="97cde-185">Accessing a namespace or type through an alias yields exactly the same result as accessing that namespace or type through its declared name.</span></span> <span data-ttu-id="97cde-186">Angenommen,</span><span class="sxs-lookup"><span data-stu-id="97cde-186">For example, given</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
<span data-ttu-id="97cde-187">die Namen `N1.N2.A`, `R1.N2.A`, und `R2.A` sind äquivalent und alle beziehen sich auf die Klasse ist, dessen vollqualifizierter Name `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="97cde-187">the names `N1.N2.A`, `R1.N2.A`, and `R2.A` are equivalent and all refer to the class whose fully qualified name is `N1.N2.A`.</span></span>

<span data-ttu-id="97cde-188">Aliase können Name ein geschlossener konstruierter Typ, sondern kann nicht der Name einer ungebundenen generischen Typdeklaration ohne Angabe von Typargumenten.</span><span class="sxs-lookup"><span data-stu-id="97cde-188">Using aliases can name a closed constructed type, but cannot name an unbound generic type declaration without supplying type arguments.</span></span> <span data-ttu-id="97cde-189">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="97cde-189">For example:</span></span>
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a><span data-ttu-id="97cde-190">Using namespacedirektiven</span><span class="sxs-lookup"><span data-stu-id="97cde-190">Using namespace directives</span></span>

<span data-ttu-id="97cde-191">Ein *Using_namespace_directive* importiert Typen in einem Namespace enthalten sind, in dem unmittelbar einschließenden Kompilierung Einheit oder Namespacetext, aktivieren den Bezeichner für jeden Typ ohne Qualifikation verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="97cde-191">A *using_namespace_directive* imports the types contained in a namespace into the immediately enclosing compilation unit or namespace body, enabling the identifier of each type to be used without qualification.</span></span>

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

<span data-ttu-id="97cde-192">In den Memberdeklarationen in einer Kompilierung Einheit oder Namespacetext, die enthält eine *Using_namespace_directive*, die im angegebenen Namespace enthaltenen Typen direkt verwiesen werden können.</span><span class="sxs-lookup"><span data-stu-id="97cde-192">Within member declarations in a compilation unit or namespace body that contains a *using_namespace_directive*, the types contained in the given namespace can be referenced directly.</span></span> <span data-ttu-id="97cde-193">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="97cde-193">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

<span data-ttu-id="97cde-194">Oben in den Memberdeklarationen in der `N3` -Namespace, den Membern des `N1.N2` direkt verfügbar sind, und daher Klasse `N3.B` von Klasse abgeleitet ist `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="97cde-194">Above, within member declarations in the `N3` namespace, the type members of `N1.N2` are directly available, and thus class `N3.B` derives from class `N1.N2.A`.</span></span>

<span data-ttu-id="97cde-195">Ein *Using_namespace_directive* im angegebenen Namespace enthaltenen Typen importiert, insbesondere, importiert jedoch nicht geschachtelten Namespaces.</span><span class="sxs-lookup"><span data-stu-id="97cde-195">A *using_namespace_directive* imports the types contained in the given namespace, but specifically does not import nested namespaces.</span></span> <span data-ttu-id="97cde-196">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="97cde-196">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
<span data-ttu-id="97cde-197">die *Using_namespace_directive* importiert die Typen, die in enthaltenen `N1`, aber nicht die Namespaces in geschachtelten `N1`.</span><span class="sxs-lookup"><span data-stu-id="97cde-197">the *using_namespace_directive* imports the types contained in `N1`, but not the namespaces nested in `N1`.</span></span> <span data-ttu-id="97cde-198">Daher den Verweis auf `N2.A` in der Deklaration der `B` führt zu einem Fehler während der Kompilierung, da keine Member mit dem Namen `N2` befinden sich im Gültigkeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="97cde-198">Thus, the reference to `N2.A` in the declaration of `B` results in a compile-time error because no members named `N2` are in scope.</span></span>

<span data-ttu-id="97cde-199">Im Gegensatz zu einem *Using_alias_directive*, *Using_namespace_directive* kann, deren Bezeichner sind bereits in der einschließenden Kompilierung Einheit oder Namespacetext definierten, Typen zu importieren.</span><span class="sxs-lookup"><span data-stu-id="97cde-199">Unlike a *using_alias_directive*, a *using_namespace_directive* may import types whose identifiers are already defined within the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="97cde-200">Namen aktiviert ist, importiert, indem eine *Using_namespace_directive* durch ähnlich benannten Elemente in der einschließenden Kompilierung Einheit oder Namespacetext ausgeblendet sind.</span><span class="sxs-lookup"><span data-stu-id="97cde-200">In effect, names imported by a *using_namespace_directive* are hidden by similarly named members in the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="97cde-201">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="97cde-201">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

<span data-ttu-id="97cde-202">Hier sind die Memberdeklarationen in der `N3` Namespace `A` bezieht sich auf `N3.A` statt `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="97cde-202">Here, within member declarations in the `N3` namespace, `A` refers to `N3.A` rather than `N1.N2.A`.</span></span>

<span data-ttu-id="97cde-203">Wenn mehr als ein Namespace oder Typ importiert werden können, indem *Using_namespace_directive*s oder *Using_static_directive*s in der gleichen Kompilierung Einheit oder Namespacetext enthalten Typen, mit dem gleichen Namen, Verweise auf Dieser Name als eine *Type_name* gelten als nicht eindeutig.</span><span class="sxs-lookup"><span data-stu-id="97cde-203">When more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types by the same name, references to that name as a *type_name* are considered ambiguous.</span></span> <span data-ttu-id="97cde-204">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="97cde-204">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
<span data-ttu-id="97cde-205">beide `N1` und `N2` -Member enthalten `A`, und da `N3` importiert, verweisen auf `A` in `N3` ist ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="97cde-205">both `N1` and `N2` contain a member `A`, and because `N3` imports both, referencing `A` in `N3` is a compile-time error.</span></span> <span data-ttu-id="97cde-206">In diesem Fall der Konflikt kann aufgelöst werden, entweder über die Qualifikation von Verweisen auf `A`, oder durch die Einführung einer *Using_alias_directive* , die einen bestimmten wählt `A`.</span><span class="sxs-lookup"><span data-stu-id="97cde-206">In this situation, the conflict can be resolved either through qualification of references to `A`, or by introducing a *using_alias_directive* that picks a particular `A`.</span></span> <span data-ttu-id="97cde-207">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="97cde-207">For example:</span></span>
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

<span data-ttu-id="97cde-208">Darüber hinaus bei mehr als ein Namespace oder Typ importiert *Using_namespace_directive*s oder *Using_static_directive*s in der gleichen Kompilierung oder Text enthalten, Typen oder Member von der gleichnamige verweist auf diesen Namen als einen *Simple_name* gelten als nicht eindeutig.</span><span class="sxs-lookup"><span data-stu-id="97cde-208">Furthermore, when more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types or members by the same name, references to that name as a *simple_name* are considered ambiguous.</span></span> <span data-ttu-id="97cde-209">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="97cde-209">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
<span data-ttu-id="97cde-210">`N1` enthält einen Typmember `A`, und `C` enthält eine statische Methode `A`, und da `N2` importiert, verweisen auf `A` als eine *Simple_name* mehrdeutig und durch einen während der Kompilierung Fehler.</span><span class="sxs-lookup"><span data-stu-id="97cde-210">`N1` contains a type member `A`, and `C` contains a static method `A`, and because `N2` imports both, referencing `A` as a *simple_name* is ambiguous and a compile-time error.</span></span> 

<span data-ttu-id="97cde-211">Wie eine *Using_alias_directive*, *Using_namespace_directive* trägt sich nicht auf alle neuen Member zu der zugrunde liegenden Deklarationsabschnitt der Kompilierungseinheit oder Namespaces, sondern beeinflusst nur den Kompilierung Einheit oder Namespacetext in dem er angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="97cde-211">Like a *using_alias_directive*, a *using_namespace_directive* does not contribute any new members to the underlying declaration space of the compilation unit or namespace, but rather affects only the compilation unit or namespace body in which it appears.</span></span>

<span data-ttu-id="97cde-212">Die *Namespace_name* verwiesen wird, indem eine *Using_namespace_directive* wird aufgelöst, auf die gleiche Weise wie die *Namespace_or_type_name* verwiesen wird, indem eine  *Using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="97cde-212">The *namespace_name* referenced by a *using_namespace_directive* is resolved in the same way as the *namespace_or_type_name* referenced by a *using_alias_directive*.</span></span> <span data-ttu-id="97cde-213">Daher *Using_namespace_directive*s in der gleichen Kompilierung Einheit oder Namespacetext wirken sich gegenseitig nicht und können in beliebiger Reihenfolge geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="97cde-213">Thus, *using_namespace_directive*s in the same compilation unit or namespace body do not affect each other and can be written in any order.</span></span>

### <a name="using-static-directives"></a><span data-ttu-id="97cde-214">Using static-Direktiven</span><span class="sxs-lookup"><span data-stu-id="97cde-214">Using static directives</span></span>

<span data-ttu-id="97cde-215">Ein *Using_static_directive* importiert die geschachtelten Typen und statische Member direkt in einer Typdeklaration unmittelbar einschließenden Kompilierung-Einheit oder Namespace-Text enthalten, die Aktivieren des Bezeichners der jedes Element und einem Typ sein ohne Qualifizierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="97cde-215">A *using_static_directive* imports the nested types and static members contained directly in a type declaration into the immediately enclosing compilation unit or namespace body, enabling the identifier of each member and type to be used without qualification.</span></span>

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

<span data-ttu-id="97cde-216">In den Memberdeklarationen in einer Kompilierung Einheit oder Namespacetext, die enthält eine *Using_static_directive*, die zugegriffen werden kann geschachtelte Typen und statische Member (mit Ausnahme von Erweiterungsmethoden) direkt in der Deklaration enthalten die angegebene Typ kann direkt verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="97cde-216">Within member declarations in a compilation unit or namespace body that contains a *using_static_directive*, the accessible nested types and static members (except extension methods) contained directly in the declaration of the given type can be referenced directly.</span></span> <span data-ttu-id="97cde-217">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="97cde-217">For example:</span></span>

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

<span data-ttu-id="97cde-218">Oben in Memberdeklarationen in der `N2` -Namespace, der statische Member und geschachtelte Typen von `N1.A` direkt verfügbar sind, und daher die Methode `N` kann sowohl auf die `B` und `M` Mitglied `N1.A`.</span><span class="sxs-lookup"><span data-stu-id="97cde-218">Above, within member declarations in the `N2` namespace, the static members and nested types of `N1.A` are directly available, and thus the method `N` is able to reference both the `B` and `M` members of `N1.A`.</span></span>

<span data-ttu-id="97cde-219">Ein *Using_static_directive* insbesondere Erweiterungsmethoden direkt als statische Methoden werden nicht importiert, aber stellt sie für den Aufruf der Erweiterungsmethode zur Verfügung ([Erweiterung Methodenaufrufe](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="97cde-219">A *using_static_directive* specifically does not import extension methods directly as static methods, but makes them available for extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="97cde-220">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="97cde-220">In the example</span></span>

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
<span data-ttu-id="97cde-221">die *Using_static_directive* importiert die Erweiterungsmethode `M` in enthaltenen `N1.A`, sondern nur als eine Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="97cde-221">the *using_static_directive* imports the extension method `M` contained in `N1.A`, but only as an extension method.</span></span> <span data-ttu-id="97cde-222">Daher der erste Verweis auf `M` im Text der `B.N` führt zu einem Fehler während der Kompilierung, da keine Member mit dem Namen `M` befinden sich im Gültigkeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="97cde-222">Thus, the first reference to `M` in the body of `B.N` results in a compile-time error because no members named `M` are in scope.</span></span>

<span data-ttu-id="97cde-223">Ein *Using_static_directive* importiert nur Elemente und Typen, die direkt in den angegebenen Typ deklariert keine Elemente und Typen, die in Basisklassen deklariert.</span><span class="sxs-lookup"><span data-stu-id="97cde-223">A *using_static_directive* only imports members and types declared directly in the given type, not members and types declared in base classes.</span></span>

<span data-ttu-id="97cde-224">TODO: Beispiel</span><span class="sxs-lookup"><span data-stu-id="97cde-224">TODO: Example</span></span>

<span data-ttu-id="97cde-225">Mehrdeutigkeiten zwischen mehreren *Using_namespace_directives* und *Using_static_directives* finden Sie im [Using namespacedirektiven](namespaces.md#using-namespace-directives).</span><span class="sxs-lookup"><span data-stu-id="97cde-225">Ambiguities between multiple *using_namespace_directives* and *using_static_directives* are discussed in [Using namespace directives](namespaces.md#using-namespace-directives).</span></span>

## <a name="namespace-members"></a><span data-ttu-id="97cde-226">Namespace-Elemente</span><span class="sxs-lookup"><span data-stu-id="97cde-226">Namespace members</span></span>

<span data-ttu-id="97cde-227">Ein *Namespace_member_declaration* ist entweder ein *Namespace_declaration* ([Namespace-Deklarationen](namespaces.md#namespace-declarations)) oder ein *Type_declaration* () [Typdeklarationen](namespaces.md#type-declarations)).</span><span class="sxs-lookup"><span data-stu-id="97cde-227">A *namespace_member_declaration* is either a *namespace_declaration* ([Namespace declarations](namespaces.md#namespace-declarations)) or a *type_declaration* ([Type declarations](namespaces.md#type-declarations)).</span></span>

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

<span data-ttu-id="97cde-228">Eine Kompilierungseinheit oder einen Namespacetext darf *Namespace_member_declaration*s und derartige Deklarationen tragen neue Mitglieder zu der zugrunde liegenden Deklarationsabschnitt des enthaltenden Kompilierung Einheit oder Namespace-Texts.</span><span class="sxs-lookup"><span data-stu-id="97cde-228">A compilation unit or a namespace body can contain *namespace_member_declaration*s, and such declarations contribute new members to the underlying declaration space of the containing compilation unit or namespace body.</span></span>

## <a name="type-declarations"></a><span data-ttu-id="97cde-229">Deklarationen von Typen</span><span class="sxs-lookup"><span data-stu-id="97cde-229">Type declarations</span></span>

<span data-ttu-id="97cde-230">Ein *Type_declaration* ist eine *Class_declaration* ([Klasse Deklarationen](classes.md#class-declarations)), ein *Struct_declaration* ([Struktur Deklarationen](structs.md#struct-declarations)), wird eine *Interface_declaration* ([Schnittstellendeklarationen](interfaces.md#interface-declarations)), wird eine *Enum_declaration* ([Enumeration Deklarationen](enums.md#enum-declarations)), oder ein *Delegate_declaration* ([delegieren Deklarationen](delegates.md#delegate-declarations)).</span><span class="sxs-lookup"><span data-stu-id="97cde-230">A *type_declaration* is a *class_declaration* ([Class declarations](classes.md#class-declarations)), a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)), an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)), an *enum_declaration* ([Enum declarations](enums.md#enum-declarations)), or a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)).</span></span>

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

<span data-ttu-id="97cde-231">Ein *Type_declaration* kann auftreten, als eine Deklaration mit der höchsten Ebene in einer Kompilierungseinheit oder die Deklaration eines Elements in einem Namespace, Klasse oder Struktur.</span><span class="sxs-lookup"><span data-stu-id="97cde-231">A *type_declaration* can occur as a top-level declaration in a compilation unit or as a member declaration within a namespace, class, or struct.</span></span>

<span data-ttu-id="97cde-232">Wenn eine Typdeklaration für einen Typ `T` tritt ein, wenn eine Deklaration mit der höchsten Ebene in einer Kompilierungseinheit, der vollqualifizierte Namen des neu deklarierten Typs ist einfach `T`.</span><span class="sxs-lookup"><span data-stu-id="97cde-232">When a type declaration for a type `T` occurs as a top-level declaration in a compilation unit, the fully qualified name of the newly declared type is simply `T`.</span></span> <span data-ttu-id="97cde-233">Wenn eine Typdeklaration für einen Typ `T` tritt auf, in einem Namespace, Klasse oder Struktur, die den vollqualifizierten Namen des neu deklarierten Typs ist `N.T`, wobei `N` der vollqualifizierte Namen der enthaltenden Namespace, Klasse oder Struktur ist.</span><span class="sxs-lookup"><span data-stu-id="97cde-233">When a type declaration for a type `T` occurs within a namespace, class, or struct, the fully qualified name of the newly declared type is `N.T`, where `N` is the fully qualified name of the containing namespace, class, or struct.</span></span>

<span data-ttu-id="97cde-234">Ein Typ deklariert wird, innerhalb einer Klasse oder Struktur wird als geschachtelter Typ bezeichnet ([geschachtelte Typen](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="97cde-234">A type declared within a class or struct is called a nested type ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="97cde-235">Die Zugriffs-Modifizierer und der Standardzugriff für eine Typdeklaration abhängig sind, auf dem Kontext, in dem die Deklaration erfolgt ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)):</span><span class="sxs-lookup"><span data-stu-id="97cde-235">The permitted access modifiers and the default access for a type declaration depend on the context in which the declaration takes place ([Declared accessibility](basic-concepts.md#declared-accessibility)):</span></span>

*  <span data-ttu-id="97cde-236">Typen in Namespaces oder einer Kompilierungseinheiten deklariert haben `public` oder `internal` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="97cde-236">Types declared in compilation units or namespaces can have `public` or `internal` access.</span></span> <span data-ttu-id="97cde-237">Der Standardwert ist `internal` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="97cde-237">The default is `internal` access.</span></span>
*  <span data-ttu-id="97cde-238">Typen, die in Klassen deklariert haben `public`, `protected internal`, `protected`, `internal`, oder `private` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="97cde-238">Types declared in classes can have `public`, `protected internal`, `protected`, `internal`, or `private` access.</span></span> <span data-ttu-id="97cde-239">Der Standardwert ist `private` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="97cde-239">The default is `private` access.</span></span>
*  <span data-ttu-id="97cde-240">In Strukturen deklarierte Typen haben `public`, `internal`, oder `private` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="97cde-240">Types declared in structs can have `public`, `internal`, or `private` access.</span></span> <span data-ttu-id="97cde-241">Der Standardwert ist `private` Zugriff.</span><span class="sxs-lookup"><span data-stu-id="97cde-241">The default is `private` access.</span></span>

## <a name="namespace-alias-qualifiers"></a><span data-ttu-id="97cde-242">Namespace-Alias-Qualifizierer</span><span class="sxs-lookup"><span data-stu-id="97cde-242">Namespace alias qualifiers</span></span>

<span data-ttu-id="97cde-243">Die ***Namespacealias-Qualifizierer*** `::` ermöglicht es, sicherzustellen, dass Suchvorgänge nach Typ durch die Einführung der neuen Typen und Member sind nicht betroffen sind.</span><span class="sxs-lookup"><span data-stu-id="97cde-243">The ***namespace alias qualifier*** `::` makes it possible to guarantee that type name lookups are unaffected by the introduction of new types and members.</span></span> <span data-ttu-id="97cde-244">Der Namespacealias-Qualifizierer wird zwischen zwei Bezeichnern, die als die linken und rechten Bezeichner bezeichnet immer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="97cde-244">The namespace alias qualifier always appears between two identifiers referred to as the left-hand and right-hand identifiers.</span></span> <span data-ttu-id="97cde-245">Im Gegensatz zu regulären `.` Qualifizierer, der linke Bezeichner, der die `::` Qualifizierer gesucht oben nur als eine Extern oder using-Alias.</span><span class="sxs-lookup"><span data-stu-id="97cde-245">Unlike the regular `.` qualifier, the left-hand identifier of the `::` qualifier is looked up only as an extern or using alias.</span></span>

<span data-ttu-id="97cde-246">Ein *Qualified_alias_member* ist wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="97cde-246">A *qualified_alias_member* is defined as follows:</span></span>

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

<span data-ttu-id="97cde-247">Ein *Qualified_alias_member* kann verwendet werden, als eine *Namespace_or_type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)) oder als der linke Operand in einem *Member_access* ([Memberzugriff](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="97cde-247">A *qualified_alias_member* can be used as a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) or as the left operand in a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="97cde-248">Ein *Qualified_alias_member* verfügt über einen von zwei Formen:</span><span class="sxs-lookup"><span data-stu-id="97cde-248">A *qualified_alias_member* has one of two forms:</span></span>

*  <span data-ttu-id="97cde-249">`N::I<A1, ..., Ak>`, wobei `N` und `I` Bezeichner darstellen und `<A1, ..., Ak>` ist eine Liste der Typargumente.</span><span class="sxs-lookup"><span data-stu-id="97cde-249">`N::I<A1, ..., Ak>`, where `N` and `I` represent identifiers, and `<A1, ..., Ak>` is a type argument list.</span></span> <span data-ttu-id="97cde-250">(`K` ist immer mindestens eine.)</span><span class="sxs-lookup"><span data-stu-id="97cde-250">(`K` is always at least one.)</span></span>
*  <span data-ttu-id="97cde-251">`N::I`, wobei `N` und `I` Bezeichner darstellen.</span><span class="sxs-lookup"><span data-stu-id="97cde-251">`N::I`, where `N` and `I` represent identifiers.</span></span> <span data-ttu-id="97cde-252">(In diesem Fall `K` gilt als 0 (null) sein.)</span><span class="sxs-lookup"><span data-stu-id="97cde-252">(In this case, `K` is considered to be zero.)</span></span>

<span data-ttu-id="97cde-253">Diese Notation, die Bedeutung des einen *Qualified_alias_member* wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="97cde-253">Using this notation, the meaning of a *qualified_alias_member* is determined as follows:</span></span>

*  <span data-ttu-id="97cde-254">Wenn `N` ist der Bezeichner `global`, und klicken Sie dann der globale Namespace gesucht wird `I`:</span><span class="sxs-lookup"><span data-stu-id="97cde-254">If `N` is the identifier `global`, then the global namespace is searched for `I`:</span></span>
   * <span data-ttu-id="97cde-255">Wenn der globale Namespace ein Namespace, der mit dem Namen enthält `I` und `K` ist 0 (null), und klicken Sie dann die *Qualified_alias_member* bezieht sich auf diesen Namespace.</span><span class="sxs-lookup"><span data-stu-id="97cde-255">If the global namespace contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
   * <span data-ttu-id="97cde-256">Wenn der globale Namespace einen nicht generischen Typ mit dem Namen enthält, andernfalls `I` und `K` ist 0 (null), und klicken Sie dann die *Qualified_alias_member* bezieht sich auf diesen Typ.</span><span class="sxs-lookup"><span data-stu-id="97cde-256">Otherwise, if the global namespace contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
   * <span data-ttu-id="97cde-257">Wenn der globale Namespace einen Typ, der mit dem Namen enthält, andernfalls `I` , bei dem `K` Typparameter, und klicken Sie dann die *Qualified_alias_member* bezieht sich auf diesen Typ mit den Argumenten angegebenen Typs erstellt.</span><span class="sxs-lookup"><span data-stu-id="97cde-257">Otherwise, if the global namespace contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
   * <span data-ttu-id="97cde-258">Andernfalls die *Qualified_alias_member* ist nicht definiert und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="97cde-258">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

*  <span data-ttu-id="97cde-259">Dabei ist, andernfalls die Namespacedeklaration ([Namespace-Deklarationen](namespaces.md#namespace-declarations)) sofort mit der *Qualified_alias_member* (sofern vorhanden), ausgehend mit jeder einschließenden Namespacedeklaration (falls vorhanden), und endet mit der Kompilierungseinheit, enthält die *Qualified_alias_member*, die folgenden Schritte werden ausgewertet, bis eine Entität befindet:</span><span class="sxs-lookup"><span data-stu-id="97cde-259">Otherwise, starting with the namespace declaration ([Namespace declarations](namespaces.md#namespace-declarations)) immediately containing the *qualified_alias_member* (if any), continuing with each enclosing namespace declaration (if any), and ending with the compilation unit containing the *qualified_alias_member*, the following steps are evaluated until an entity is located:</span></span>

   * <span data-ttu-id="97cde-260">Wenn die Namespace-Deklaration oder Kompilierung Einheit enthält eine *Using_alias_directive* zuordnet, die `N` mit einem Typ, die *Qualified_alias_member* ist nicht definiert und Kompilierzeit Fehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="97cde-260">If the namespace declaration or compilation unit contains a *using_alias_directive* that associates `N` with a type, then the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
   * <span data-ttu-id="97cde-261">Andernfalls, wenn die Namespace-Deklaration oder Kompilierung Einheit enthält eine *Extern_alias_directive* oder *Using_alias_directive* zuordnet, die `N` mit einem Namespace, dann:</span><span class="sxs-lookup"><span data-stu-id="97cde-261">Otherwise, if the namespace declaration or compilation unit contains an *extern_alias_directive* or *using_alias_directive* that associates `N` with a namespace, then:</span></span>
     * <span data-ttu-id="97cde-262">Wenn der Namespace zugeordnete `N` enthält einen Namespace mit dem Namen `I` und `K` NULL ist, wird die *Qualified_alias_member* bezieht sich auf diesen Namespace.</span><span class="sxs-lookup"><span data-stu-id="97cde-262">If the namespace associated with `N` contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
     * <span data-ttu-id="97cde-263">Andernfalls, wenn der Namespace zugeordnete `N` enthält einen nicht generischen Typ mit dem Namen `I` und `K` ist 0 (null), und klicken Sie dann die *Qualified_alias_member* bezieht sich auf diesen Typ.</span><span class="sxs-lookup"><span data-stu-id="97cde-263">Otherwise, if the namespace associated with `N` contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
     * <span data-ttu-id="97cde-264">Andernfalls, wenn der Namespace zugeordnete `N` enthält einen Typ mit dem Namen `I` , bei dem `K` Typparameter, und klicken Sie dann die *Qualified_alias_member* bezieht sich auf, mit dem angegebenen Typ erstellter Typ Argumente.</span><span class="sxs-lookup"><span data-stu-id="97cde-264">Otherwise, if the namespace associated with `N` contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
     * <span data-ttu-id="97cde-265">Andernfalls die *Qualified_alias_member* ist nicht definiert und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="97cde-265">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="97cde-266">Andernfalls die *Qualified_alias_member* ist nicht definiert und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="97cde-266">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="97cde-267">Beachten Sie, dass bewirkt, einen Fehler während der Kompilierung mit der Namespacealias-Qualifizierer, die einen Alias, der auf einen Typ verweist dass.</span><span class="sxs-lookup"><span data-stu-id="97cde-267">Note that using the namespace alias qualifier with an alias that references a type causes a compile-time error.</span></span> <span data-ttu-id="97cde-268">Beachten Sie, dass bei den Bezeichner `N` ist `global`, und klicken Sie dann in den globalen Namespace Suche durchgeführt wird, auch wenn es ein alias zuordnen mit `global` mit einem Typ oder Namespace.</span><span class="sxs-lookup"><span data-stu-id="97cde-268">Also note that if the identifier `N` is `global`, then lookup is performed in the global namespace, even if there is a using alias associating `global` with a type or namespace.</span></span>

### <a name="uniqueness-of-aliases"></a><span data-ttu-id="97cde-269">Die Eindeutigkeit von Aliasen</span><span class="sxs-lookup"><span data-stu-id="97cde-269">Uniqueness of aliases</span></span>

<span data-ttu-id="97cde-270">Jeder Kompilierung Komponenten- und Namespace-Text enthält, einen separaten Deklarationsabschnitt für "extern" Aliase und using-Aliase.</span><span class="sxs-lookup"><span data-stu-id="97cde-270">Each compilation unit and namespace body has a separate declaration space for extern aliases and using aliases.</span></span> <span data-ttu-id="97cde-271">Daher zwar der Namen des einen externen Alias oder using-Alias innerhalb der Satz von "extern" Aliase eindeutig sein muss und using-Aliase in der direkt enthaltenden Kompilierung Einheit oder Namespacetext deklariert, ein Alias ist zulässig, haben den gleichen Namen wie eines Typs oder Namespaces solange ich t wird verwendet, nur mit der `::` Qualifizierer.</span><span class="sxs-lookup"><span data-stu-id="97cde-271">Thus, while the name of an extern alias or using alias must be unique within the set of extern aliases and using aliases declared in the immediately containing compilation unit or namespace body, an alias is permitted to have the same name as a type or namespace as long as it is used only with the `::` qualifier.</span></span>

<span data-ttu-id="97cde-272">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="97cde-272">In the example</span></span>
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
<span data-ttu-id="97cde-273">der Name `A` hat zwei mögliche Bedeutungen in der zweiten Namespace-Text, da sowohl die Klasse `A` und der Alias `A` befinden sich im Gültigkeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="97cde-273">the name `A` has two possible meanings in the second namespace body because both the class `A` and the using alias `A` are in scope.</span></span> <span data-ttu-id="97cde-274">Aus diesem Grund verwenden `A` im qualifizierten Namen `A.Stream` ist mehrdeutig und verursacht einen Fehler während der Kompilierung auftreten.</span><span class="sxs-lookup"><span data-stu-id="97cde-274">For this reason, use of `A` in the qualified name `A.Stream` is ambiguous and causes a compile-time error to occur.</span></span> <span data-ttu-id="97cde-275">Allerdings verwenden der `A` mit der `::` Qualifizierer ist kein Fehler, da `A` wird nur als ein Namespacealias nachgeschlagen.</span><span class="sxs-lookup"><span data-stu-id="97cde-275">However, use of `A` with the `::` qualifier is not an error because `A` is looked up only as a namespace alias.</span></span>
