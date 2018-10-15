# <a name="namespaces"></a>Namespaces

C#-Programme werden mithilfe von Namespaces organisiert. Namespaces werden verwendet, sowohl als "intern" Organisation System für ein Programm, und als "extern" Organisation System – eine Möglichkeit zum Darstellen der Programmelemente, die für andere Programme verfügbar gemacht werden.

Using-Direktiven ([Using-Direktiven](namespaces.md#using-directives)) werden bereitgestellt, um die Verwendung von Namespaces zu erleichtern.

## <a name="compilation-units"></a>Kompilierungseinheiten hinweg

Ein *Compilation_unit* die allgemeine Struktur einer Quelldatei definiert. Eine Kompilierungseinheit besteht aus 0 (null) oder mehreren *Using_directive*s, gefolgt von NULL oder mehr *Global_attributes* gefolgt von 0 (null) oder mehr *Namespace_member_declaration*s .

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

Ein C#-Programm besteht aus einen oder mehrere Kompilierungseinheiten hinweg, jeweils in einer separaten Quelldatei enthalten. Wenn ein C#-Programm kompiliert wird, werden alle von der Kompilierungseinheiten zusammen verarbeitet. Daher können Kompilierungseinheiten voneinander, möglicherweise in einer kreisförmigen Weise abhängig sind.

Die *Using_directive*s eine Kompilierung Einheit beeinflussen die *Global_attributes* und *Namespace_member_declaration*s Kompilationseinheit, haben jedoch keine Auswirkungen auf andere Kompilierungseinheiten hinweg.

Die *Global_attributes* ([Attribute](attributes.md)) von einer Kompilierungseinheit ermöglichen die Angabe von Attributen für die Ziel-Assembly und Modul. Fungieren als physische Container für Typen, Assemblys und Modulen. Eine Assembly bestehen aus mehreren physisch separaten Modulen.

Die *Namespace_member_declaration*s der einzelnen Kompilierungseinheiten eines Programms tragen Member zu einer einzelnen Deklarationsabschnitt den globalen Namespace bezeichnet. Zum Beispiel:

Datei `A.cs`:
```csharp
class A {}
```

Datei `B.cs`:
```csharp
class B {}
```

Die zwei Kompilierungseinheiten beitragen, auf die einzelnen globalen Namespace deklarieren zwei Klassen mit den vollqualifizierten Namen in diesem Fall `A` und `B`. Da die zwei Kompilierungseinheiten zum selben Deklarationsabschnitt beitragen möchten, wäre ein Fehler gewesen, wenn jeweils eine Deklaration eines Elements mit dem gleichen Namen enthalten.

## <a name="namespace-declarations"></a>Namespacedeklarationen

Ein *Namespace_declaration* besteht aus dem Schlüsselwort `namespace`, gefolgt von einem Namespace-Name und der Text, optional gefolgt von einem Semikolon.

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

Ein *Namespace_declaration* auftreten als Deklaration auf oberster Ebene in einer *Compilation_unit* oder als eine Memberdeklaration innerhalb einer anderen *Namespace_declaration*. Wenn eine *Namespace_declaration* tritt ein, wenn eine Deklaration mit der höchsten Ebene in eine *Compilation_unit*, aus dem Namespace wird ein Member des globalen Namespaces. Wenn eine *Namespace_declaration* tritt auf, in einer anderen *Namespace_declaration*, aus dem innere Namespace wird ein Mitglied der äußere Namespace. In beiden Fällen muss der Name eines Namespace innerhalb des enthaltenden Namespaces eindeutig sein.

Namespaces sind implizit `public` und die Deklaration eines Namespace kann keine Zugriffsmodifizierer enthalten.

Innerhalb einer *Namespace_body*, den optionalen *Using_directive*s importieren Sie die Namen der anderen Namespaces, Typen und Member, sodass sie direkt anstelle von über qualifizierte Namen verwiesen werden. Der optionale *Namespace_member_declaration*s tragen Member in den Deklarationsbereich des Namespaces. Beachten Sie, dass alle *Using_directive*s muss vor jeglichen Memberdeklarationen angezeigt werden.

Die *Qualified_identifier* von einem *Namespace_declaration* möglicherweise einen einzelnen Bezeichner oder eine Sequenz von Bezeichnern, getrennt durch "`.`" Token. Die letztgenannte Form lässt es sich um ein Programm, um einem geschachtelten Namespace definieren, ohne die lexikalische Schachtelung mehrere Namespacedeklarationen. Ein auf ein Objekt angewendeter

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
ist semantisch gleichwertig mit
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

Namespaces haben, und zwei Namespacedeklarationen mit den gleichen vollqualifizierten Namen tragen zum selben Deklarationsabschnitt ([Deklarationen](basic-concepts.md#declarations)). Im Beispiel
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
die beiden oben genannten Namespacedeklarationen beitragen zum selben Deklarationsabschnitt, deklarieren zwei Klassen mit den vollqualifizierten Namen in diesem Fall `N1.N2.A` und `N1.N2.B`. Da die beiden Deklarationen zum selben Deklarationsabschnitt beitragen möchten, wäre gewesen, dass ein Fehler, wenn jeder eine Deklaration eines Elements mit dem gleichen Namen enthalten.

## <a name="extern-aliases"></a>Extern-Aliase

Ein *Extern_alias_directive* führt einen Bezeichner, der als Alias für einen Namespace fungiert. Die Spezifikation des Aliasnamespace als für den Quellcode des Programms extern ist und gilt auch für geschachtelte Namespaces des Namespace mit einem Alias versehen.

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

Im Rahmen einer *Extern_alias_directive* erstreckt sich über die *Using_directive*s, *Global_attributes* und *Namespace_member_declaration*s des direkt enthaltenden Kompilierung oder Text.

Innerhalb eines Kompilierung Einheit oder Namespace, der enthält ein *Extern_alias_directive*, der Bezeichner wurde eingeführt, durch die *Extern_alias_directive* können verwendet werden, um den Aliasnamespace als zu verweisen. Es ist ein Fehler während der Kompilierung für die *Bezeichner* das Wort sein `global`.

Ein *Extern_alias_directive* stellt ein alias zur Verfügung, wenn es innerhalb eines bestimmten Kompilierung Einheit oder Namespace, aber es trägt nicht die neuen Member zu den zugrunde liegenden Deklarationsabschnitt. Das heißt, eine *Extern_alias_directive* ist nicht transitiv, sondern nur die Kompilierung Einheit oder Namespacetext in dem er auftritt, beeinflusst.

Das folgende Programm deklariert und verwendet zwei "extern" Aliase `X` und `Y`, die jeweils von den Stamm einer Namespacehierarchie unterschiedliche darstellt:
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

Das Programm wird das Vorhandensein der "extern" deklariert Aliase `X` und `Y`, aber die tatsächlichen Definitionen der Aliase für die Anwendung extern sind. Dem identisch benannten `N.B` Klassen können jetzt als verwiesen werden `X.N.B` und `Y.N.B`, oder Verwenden des Namespacealias-Qualifizierers, `X::N.B` und `Y::N.B`. Ein Fehler tritt auf, wenn ein Programm einen externen Alias deklariert, für den keine externe Definition bereitgestellt wird.

## <a name="using-directives"></a>using-Direktiven

***Using-Direktiven*** vereinfachen die Nutzung von Namespaces und Typen, die in anderen Namespaces definiert. Mithilfe von Direktiven Auswirkungen auf die namensauflösung von *Namespace_or_type_name*s ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)) und *Simple_name*s ([einfache Namen ](expressions.md#simple-names)), aber im Gegensatz zu Deklarationen, die using-Direktiven tragen nicht die neuen Elemente in der zugrunde liegenden Deklaration Leerzeichen der Kompilierungseinheiten oder Namespaces, die in dem sie verwendet werden.

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

Ein *Using_alias_directive* ([mithilfe von Direktiven für Namespacealiase](namespaces.md#using-alias-directives)) führt einen Alias für einen Namespace oder Typ.

Ein *Using_namespace_directive* ([Using namespacedirektiven](namespaces.md#using-namespace-directives)) Typmember von einem Namespace importiert.

Ein *Using_static_directive* ([Using static-Direktiven](namespaces.md#using-static-directives)) importiert die geschachtelten Typen und statische Member eines Typs.

Im Rahmen einer *Using_directive* erstreckt sich über die *Namespace_member_declaration*s des direkt enthaltenden Kompilierung oder Text. Im Rahmen einer *Using_directive* beinhalten insbesondere nicht die gleichgeordnete *Using_directive*s. Daher peer *Using_directive*s haben keinen Einfluss auf einander und spielt die Reihenfolge, in der sie geschrieben werden.

### <a name="using-alias-directives"></a>Using-Alias-Direktiven

Ein *Using_alias_directive* führt einen Bezeichner, der als Alias für einen Namespace oder Typ, in dem unmittelbar einschließenden Kompilierung Einheit oder Namespacetext dient.

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

Innerhalb der Memberdeklarationen in einer Kompilierung Einheit oder Namespacetext, die enthält eine *Using_alias_directive*, der Bezeichner wurde eingeführt, durch die *Using_alias_directive* kann verwendet werden, zu verweisen der angegebenen Namespace oder Typ. Zum Beispiel:
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

Oben in den Memberdeklarationen in der `N3` Namespace `A` ist ein Alias für `N1.N2.A`, und daher `N3.B` von Klasse abgeleitet ist `N1.N2.A`. Die gleiche Wirkung erhalten Sie durch Erstellen eines Alias `R` für `N1.N2` verweisen Sie einfach auf `R.A`:
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

Die *Bezeichner* von einem *Using_alias_directive* muss innerhalb des Deklarationsabschnitts der Kompilierungseinheit oder Namespace, der sofort enthält eindeutig sein der *Using_alias_directive* . Zum Beispiel:
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

Oben `N3` enthält bereits einen Member `A`, daher wird ein Fehler während der Kompilierung für eine *Using_alias_directive* dieses Bezeichners zu verwenden. Entsprechend ist es ein Fehler während der Kompilierung für zwei oder mehr *Using_alias_directive*s in der gleichen Kompilierung Einheit oder Namespacetext Aliase mit demselben Namen zu deklarieren.

Ein *Using_alias_directive* stellt ein alias zur Verfügung, wenn es innerhalb eines bestimmten Kompilierung Einheit oder Namespace, aber es trägt nicht die neuen Member zu den zugrunde liegenden Deklarationsabschnitt. Das heißt, eine *Using_alias_directive* ist nicht transitiv, sondern beeinflusst nur die Kompilierung Einheit oder Namespacetext in dem er auftritt. Im Beispiel
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
im Rahmen der *Using_alias_directive* eingeführt `R` erstreckt sich auch nur auf Deklarationen im Hauptteil Namespace, in dem es enthalten ist, damit `R` in die zweite Namespacedeklaration nicht bekannt ist. Platzieren Sie jedoch die *Using_alias_directive* in der enthaltenden Kompilierung führt Komponententests den Alias in den beiden Namespacedeklarationen verfügbar sind:
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

Genau wie reguläre Elemente Namen eingeführt, durch *Using_alias_directive*s durch ähnlich benannten Elemente in geschachtelte Bereiche ausgeblendet sind. Im Beispiel
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
der Verweis auf `R.A` in der Deklaration der `B` verursacht einen Kompilierungsfehler, da `R` bezieht sich auf `N3.R`, nicht `N1.N2`.

Die Reihenfolge, in der *Using_alias_directive*s werden geschrieben, verfügt über keine Bedeutung und Auflösung von der *Namespace_or_type_name* verwiesen wird, indem eine *Using_alias_directive*ist nicht betroffen von dem *Using_alias_directive* selbst oder von anderen *Using_directive*s im direkt enthaltenden Kompilierung oder Text. Das heißt, die *Namespace_or_type_name* von einer *Using_alias_directive* wird aufgelöst, als hätte der direkt enthaltende Kompilierung Einheit oder Namespacetext keine *Using_directive*s. Ein *Using_alias_directive* jedoch möglicherweise betroffen *Extern_alias_directive*s im direkt enthaltenden Kompilierung oder Text. Im Beispiel
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
die letzte *Using_alias_directive* führt ein Fehler während der Kompilierung, da es nicht, vom ersten betroffen ist *Using_alias_directive*. Die erste *Using_alias_directive* führt nicht zu einem Fehler seit den Rahmen der externe Alias `E` enthält die *Using_alias_directive*.

Ein *Using_alias_directive* erstellen einen Alias für jeden Namespace oder Typ, einschließlich des Namespace, in dem er angezeigt wird, und jeden Namespace oder Typ, die in diesem Namespace geschachtelt sind.

Zugreifen auf einen Namespace oder Typ über einen Alias führt genau dieselbe Ergebnisse wie beim Zugriff auf diesen Namespace oder Typ durch den deklarierten Namen. Angenommen,
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
die Namen `N1.N2.A`, `R1.N2.A`, und `R2.A` sind äquivalent und alle beziehen sich auf die Klasse ist, dessen vollqualifizierter Name `N1.N2.A`.

Aliase können Name ein geschlossener konstruierter Typ, sondern kann nicht der Name einer ungebundenen generischen Typdeklaration ohne Angabe von Typargumenten. Zum Beispiel:
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

### <a name="using-namespace-directives"></a>Using namespacedirektiven

Ein *Using_namespace_directive* importiert Typen in einem Namespace enthalten sind, in dem unmittelbar einschließenden Kompilierung Einheit oder Namespacetext, aktivieren den Bezeichner für jeden Typ ohne Qualifikation verwendet werden.

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

In den Memberdeklarationen in einer Kompilierung Einheit oder Namespacetext, die enthält eine *Using_namespace_directive*, die im angegebenen Namespace enthaltenen Typen direkt verwiesen werden können. Zum Beispiel:
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

Oben in den Memberdeklarationen in der `N3` -Namespace, den Membern des `N1.N2` direkt verfügbar sind, und daher Klasse `N3.B` von Klasse abgeleitet ist `N1.N2.A`.

Ein *Using_namespace_directive* im angegebenen Namespace enthaltenen Typen importiert, insbesondere, importiert jedoch nicht geschachtelten Namespaces. Im Beispiel
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
die *Using_namespace_directive* importiert die Typen, die in enthaltenen `N1`, aber nicht die Namespaces in geschachtelten `N1`. Daher den Verweis auf `N2.A` in der Deklaration der `B` führt zu einem Fehler während der Kompilierung, da keine Member mit dem Namen `N2` befinden sich im Gültigkeitsbereich.

Im Gegensatz zu einem *Using_alias_directive*, *Using_namespace_directive* kann, deren Bezeichner sind bereits in der einschließenden Kompilierung Einheit oder Namespacetext definierten, Typen zu importieren. Namen aktiviert ist, importiert, indem eine *Using_namespace_directive* durch ähnlich benannten Elemente in der einschließenden Kompilierung Einheit oder Namespacetext ausgeblendet sind. Zum Beispiel:
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

Hier sind die Memberdeklarationen in der `N3` Namespace `A` bezieht sich auf `N3.A` statt `N1.N2.A`.

Wenn mehr als ein Namespace oder Typ importiert werden können, indem *Using_namespace_directive*s oder *Using_static_directive*s in der gleichen Kompilierung Einheit oder Namespacetext enthalten Typen, mit dem gleichen Namen, Verweise auf Dieser Name als eine *Type_name* gelten als nicht eindeutig. Im Beispiel
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
beide `N1` und `N2` -Member enthalten `A`, und da `N3` importiert, verweisen auf `A` in `N3` ist ein Fehler während der Kompilierung. In diesem Fall der Konflikt kann aufgelöst werden, entweder über die Qualifikation von Verweisen auf `A`, oder durch die Einführung einer *Using_alias_directive* , die einen bestimmten wählt `A`. Zum Beispiel:
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

Darüber hinaus bei mehr als ein Namespace oder Typ importiert *Using_namespace_directive*s oder *Using_static_directive*s in der gleichen Kompilierung oder Text enthalten, Typen oder Member von der gleichnamige verweist auf diesen Namen als einen *Simple_name* gelten als nicht eindeutig. Im Beispiel
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
`N1` enthält einen Typmember `A`, und `C` enthält eine statische Methode `A`, und da `N2` importiert, verweisen auf `A` als eine *Simple_name* mehrdeutig und durch einen während der Kompilierung Fehler. 

Wie eine *Using_alias_directive*, *Using_namespace_directive* trägt sich nicht auf alle neuen Member zu der zugrunde liegenden Deklarationsabschnitt der Kompilierungseinheit oder Namespaces, sondern beeinflusst nur den Kompilierung Einheit oder Namespacetext in dem er angezeigt wird.

Die *Namespace_name* verwiesen wird, indem eine *Using_namespace_directive* wird aufgelöst, auf die gleiche Weise wie die *Namespace_or_type_name* verwiesen wird, indem eine  *Using_alias_directive*. Daher *Using_namespace_directive*s in der gleichen Kompilierung Einheit oder Namespacetext wirken sich gegenseitig nicht und können in beliebiger Reihenfolge geschrieben werden.

### <a name="using-static-directives"></a>Using static-Direktiven

Ein *Using_static_directive* importiert die geschachtelten Typen und statische Member direkt in einer Typdeklaration unmittelbar einschließenden Kompilierung-Einheit oder Namespace-Text enthalten, die Aktivieren des Bezeichners der jedes Element und einem Typ sein ohne Qualifizierung verwendet.

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

In den Memberdeklarationen in einer Kompilierung Einheit oder Namespacetext, die enthält eine *Using_static_directive*, die zugegriffen werden kann geschachtelte Typen und statische Member (mit Ausnahme von Erweiterungsmethoden) direkt in der Deklaration enthalten die angegebene Typ kann direkt verwiesen werden. Zum Beispiel:

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

Oben in Memberdeklarationen in der `N2` -Namespace, der statische Member und geschachtelte Typen von `N1.A` direkt verfügbar sind, und daher die Methode `N` kann sowohl auf die `B` und `M` Mitglied `N1.A`.

Ein *Using_static_directive* insbesondere Erweiterungsmethoden direkt als statische Methoden werden nicht importiert, aber stellt sie für den Aufruf der Erweiterungsmethode zur Verfügung ([Erweiterung Methodenaufrufe](expressions.md#extension-method-invocations)). Im Beispiel

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
die *Using_static_directive* importiert die Erweiterungsmethode `M` in enthaltenen `N1.A`, sondern nur als eine Erweiterungsmethode. Daher der erste Verweis auf `M` im Text der `B.N` führt zu einem Fehler während der Kompilierung, da keine Member mit dem Namen `M` befinden sich im Gültigkeitsbereich.

Ein *Using_static_directive* importiert nur Elemente und Typen, die direkt in den angegebenen Typ deklariert keine Elemente und Typen, die in Basisklassen deklariert.

TODO: Beispiel

Mehrdeutigkeiten zwischen mehreren *Using_namespace_directives* und *Using_static_directives* finden Sie im [Using namespacedirektiven](namespaces.md#using-namespace-directives).

## <a name="namespace-members"></a>Namespace-Elemente

Ein *Namespace_member_declaration* ist entweder ein *Namespace_declaration* ([Namespace-Deklarationen](namespaces.md#namespace-declarations)) oder ein *Type_declaration* () [Typdeklarationen](namespaces.md#type-declarations)).

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

Eine Kompilierungseinheit oder einen Namespacetext darf *Namespace_member_declaration*s und derartige Deklarationen tragen neue Mitglieder zu der zugrunde liegenden Deklarationsabschnitt des enthaltenden Kompilierung Einheit oder Namespace-Texts.

## <a name="type-declarations"></a>Deklarationen von Typen

Ein *Type_declaration* ist eine *Class_declaration* ([Klasse Deklarationen](classes.md#class-declarations)), ein *Struct_declaration* ([Struktur Deklarationen](structs.md#struct-declarations)), wird eine *Interface_declaration* ([Schnittstellendeklarationen](interfaces.md#interface-declarations)), wird eine *Enum_declaration* ([Enumeration Deklarationen](enums.md#enum-declarations)), oder ein *Delegate_declaration* ([delegieren Deklarationen](delegates.md#delegate-declarations)).

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

Ein *Type_declaration* kann auftreten, als eine Deklaration mit der höchsten Ebene in einer Kompilierungseinheit oder die Deklaration eines Elements in einem Namespace, Klasse oder Struktur.

Wenn eine Typdeklaration für einen Typ `T` tritt ein, wenn eine Deklaration mit der höchsten Ebene in einer Kompilierungseinheit, der vollqualifizierte Namen des neu deklarierten Typs ist einfach `T`. Wenn eine Typdeklaration für einen Typ `T` tritt auf, in einem Namespace, Klasse oder Struktur, die den vollqualifizierten Namen des neu deklarierten Typs ist `N.T`, wobei `N` der vollqualifizierte Namen der enthaltenden Namespace, Klasse oder Struktur ist.

Ein Typ deklariert wird, innerhalb einer Klasse oder Struktur wird als geschachtelter Typ bezeichnet ([geschachtelte Typen](classes.md#nested-types)).

Die Zugriffs-Modifizierer und der Standardzugriff für eine Typdeklaration abhängig sind, auf dem Kontext, in dem die Deklaration erfolgt ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)):

*  Typen in Namespaces oder einer Kompilierungseinheiten deklariert haben `public` oder `internal` Zugriff. Der Standardwert ist `internal` Zugriff.
*  Typen, die in Klassen deklariert haben `public`, `protected internal`, `protected`, `internal`, oder `private` Zugriff. Der Standardwert ist `private` Zugriff.
*  In Strukturen deklarierte Typen haben `public`, `internal`, oder `private` Zugriff. Der Standardwert ist `private` Zugriff.

## <a name="namespace-alias-qualifiers"></a>Namespace-Alias-Qualifizierer

Die ***Namespacealias-Qualifizierer*** `::` ermöglicht es, sicherzustellen, dass Suchvorgänge nach Typ durch die Einführung der neuen Typen und Member sind nicht betroffen sind. Der Namespacealias-Qualifizierer wird zwischen zwei Bezeichnern, die als die linken und rechten Bezeichner bezeichnet immer angezeigt. Im Gegensatz zu regulären `.` Qualifizierer, der linke Bezeichner, der die `::` Qualifizierer gesucht oben nur als eine Extern oder using-Alias.

Ein *Qualified_alias_member* ist wie folgt definiert:

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

Ein *Qualified_alias_member* kann verwendet werden, als eine *Namespace_or_type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)) oder als der linke Operand in einem *Member_access* ([Memberzugriff](expressions.md#member-access)).

Ein *Qualified_alias_member* verfügt über einen von zwei Formen:

*  `N::I<A1, ..., Ak>`, wobei `N` und `I` Bezeichner darstellen und `<A1, ..., Ak>` ist eine Liste der Typargumente. (`K` ist immer mindestens eine.)
*  `N::I`, wobei `N` und `I` Bezeichner darstellen. (In diesem Fall `K` gilt als 0 (null) sein.)

Diese Notation, die Bedeutung des einen *Qualified_alias_member* wird wie folgt bestimmt:

*  Wenn `N` ist der Bezeichner `global`, und klicken Sie dann der globale Namespace gesucht wird `I`:
   * Wenn der globale Namespace ein Namespace, der mit dem Namen enthält `I` und `K` ist 0 (null), und klicken Sie dann die *Qualified_alias_member* bezieht sich auf diesen Namespace.
   * Wenn der globale Namespace einen nicht generischen Typ mit dem Namen enthält, andernfalls `I` und `K` ist 0 (null), und klicken Sie dann die *Qualified_alias_member* bezieht sich auf diesen Typ.
   * Wenn der globale Namespace einen Typ, der mit dem Namen enthält, andernfalls `I` , bei dem `K` Typparameter, und klicken Sie dann die *Qualified_alias_member* bezieht sich auf diesen Typ mit den Argumenten angegebenen Typs erstellt.
   * Andernfalls die *Qualified_alias_member* ist nicht definiert und ein Fehler während der Kompilierung auftritt.

*  Dabei ist, andernfalls die Namespacedeklaration ([Namespace-Deklarationen](namespaces.md#namespace-declarations)) sofort mit der *Qualified_alias_member* (sofern vorhanden), ausgehend mit jeder einschließenden Namespacedeklaration (falls vorhanden), und endet mit der Kompilierungseinheit, enthält die *Qualified_alias_member*, die folgenden Schritte werden ausgewertet, bis eine Entität befindet:

   * Wenn die Namespace-Deklaration oder Kompilierung Einheit enthält eine *Using_alias_directive* zuordnet, die `N` mit einem Typ, die *Qualified_alias_member* ist nicht definiert und Kompilierzeit Fehler tritt auf.
   * Andernfalls, wenn die Namespace-Deklaration oder Kompilierung Einheit enthält eine *Extern_alias_directive* oder *Using_alias_directive* zuordnet, die `N` mit einem Namespace, dann:
     * Wenn der Namespace zugeordnete `N` enthält einen Namespace mit dem Namen `I` und `K` NULL ist, wird die *Qualified_alias_member* bezieht sich auf diesen Namespace.
     * Andernfalls, wenn der Namespace zugeordnete `N` enthält einen nicht generischen Typ mit dem Namen `I` und `K` ist 0 (null), und klicken Sie dann die *Qualified_alias_member* bezieht sich auf diesen Typ.
     * Andernfalls, wenn der Namespace zugeordnete `N` enthält einen Typ mit dem Namen `I` , bei dem `K` Typparameter, und klicken Sie dann die *Qualified_alias_member* bezieht sich auf, mit dem angegebenen Typ erstellter Typ Argumente.
     * Andernfalls die *Qualified_alias_member* ist nicht definiert und ein Fehler während der Kompilierung auftritt.
*  Andernfalls die *Qualified_alias_member* ist nicht definiert und ein Fehler während der Kompilierung auftritt.

Beachten Sie, dass bewirkt, einen Fehler während der Kompilierung mit der Namespacealias-Qualifizierer, die einen Alias, der auf einen Typ verweist dass. Beachten Sie, dass bei den Bezeichner `N` ist `global`, und klicken Sie dann in den globalen Namespace Suche durchgeführt wird, auch wenn es ein alias zuordnen mit `global` mit einem Typ oder Namespace.

### <a name="uniqueness-of-aliases"></a>Die Eindeutigkeit von Aliasen

Jeder Kompilierung Komponenten- und Namespace-Text enthält, einen separaten Deklarationsabschnitt für "extern" Aliase und using-Aliase. Daher zwar der Namen des einen externen Alias oder using-Alias innerhalb der Satz von "extern" Aliase eindeutig sein muss und using-Aliase in der direkt enthaltenden Kompilierung Einheit oder Namespacetext deklariert, ein Alias ist zulässig, haben den gleichen Namen wie eines Typs oder Namespaces solange ich t wird verwendet, nur mit der `::` Qualifizierer.

Im Beispiel
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
der Name `A` hat zwei mögliche Bedeutungen in der zweiten Namespace-Text, da sowohl die Klasse `A` und der Alias `A` befinden sich im Gültigkeitsbereich. Aus diesem Grund verwenden `A` im qualifizierten Namen `A.Stream` ist mehrdeutig und verursacht einen Fehler während der Kompilierung auftreten. Allerdings verwenden der `A` mit der `::` Qualifizierer ist kein Fehler, da `A` wird nur als ein Namespacealias nachgeschlagen.
