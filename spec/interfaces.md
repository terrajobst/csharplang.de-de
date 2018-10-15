# <a name="interfaces"></a>Schnittstellen

Eine Schnittstelle definiert einen Vertrag. Eine Klasse oder Struktur, die eine Schnittstelle implementiert, muss ihren Vertrag einhalten. Eine Schnittstelle kann von mehreren Basisschnittstellen erben, und eine Klasse oder Struktur kann mehrere Schnittstellen implementieren.

Schnittstellen können Methoden, Eigenschaften, Ereignisse und Indexer enthalten. Die Schnittstelle selbst stellt keine Implementierungen für die Member bereit, das sie definiert. Die Schnittstelle gibt nur die Elemente, die von Klassen oder Strukturen, die die Schnittstelle implementieren bereitgestellt werden müssen.

## <a name="interface-declarations"></a>Deklarationen von Schnittstellen

Ein *Interface_declaration* ist eine *Type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), die eine neue Art von Schnittstelle deklariert.

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

Ein *Interface_declaration* besteht aus einer optionalen Gruppe von *Attribute* ([Attribute](attributes.md)), gefolgt von einer optionalen Gruppe von *Interface_modifier*s ([Schnittstelle Modifizierer](interfaces.md#interface-modifiers)), gefolgt von einem optionalen `partial` Modifizierer, gefolgt vom Schlüsselwort `interface` und *Bezeichner* mit dem Namen der Schnittstelle gefolgt von einem optionalen *Variant_type_parameter_list* Spezifikation ([Variante Typparameterlisten](interfaces.md#variant-type-parameter-lists)), gefolgt von einem optionalen *Interface_base* Spezifikation ([Basisschnittstellen](interfaces.md#base-interfaces)), gefolgt von einem optionalen *Type_parameter_constraints_clause*s-Spezifikation ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)) , gefolgt von einem *Interface_body* ([Schnittstelle-Text](interfaces.md#interface-body)), optional gefolgt durch ein Semikolon.

### <a name="interface-modifiers"></a>Interface-Modifizierer

Ein *Interface_declaration* kann optional eine Sequenz von Schnittstelle Modifizierer enthalten:

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

Es ist ein Fehler während der Kompilierung für den gleichen Modifizierer für mehrere Male in einer Schnittstellendeklaration angezeigt werden.

Die `new` Modifizierer ist nur zulässig, auf die Schnittstellen, die innerhalb einer Klasse definiert. Es gibt an, dass die Schnittstelle mit dem gleichen Namen, Blendet einen geerbten Member aus wie in beschrieben [der new-Modifizierer](classes.md#the-new-modifier).

Die `public`, `protected`, `internal`, und `private` Modifizierern steuern den Zugriff auf die Schnittstelle. Je nach Kontext, in dem die Schnittstellendeklaration auftritt, nur einige dieser Modifizierer gestattet werden ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)).

### <a name="partial-modifier"></a>Ein partial-Modifizierer

Die `partial` Modifizierer gibt an, dass dies *Interface_declaration* ist eine Deklaration der partiellen Typ. Mehrere partielle Schnittstellendeklarationen mit demselben Namen in einer einschließenden Namespace oder Typ Deklaration kombiniert werden, um das Formular eine Schnittstellendeklaration, im angegebenen gemäß den Regeln [partielle Typen](classes.md#partial-types).

### <a name="variant-type-parameter-lists"></a>Variant-Typ-Parameterlisten

Die Listen der Variant-Typ-Parameter können nur auf Schnittstellen-und Delegattypen auftreten. Der Unterschied zu normalen *Type_parameter_list*s ist der optionale *Variance_annotation* für jeden Typparameter.

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

Wenn die varianzanmerkung ist `out`, der Type-Parameter gilt als ***kovariant***. Wenn die varianzanmerkung ist `in`, der Type-Parameter gilt als ***kontravariant***. Wenn keine varianzanmerkung vorhanden ist, der Type-Parameter gilt ***invariante***.

Im Beispiel
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
`X` kovariant ist, `Y` kontravariant ist, und `Z` unveränderlich ist.

#### <a name="variance-safety"></a>Varianz-Sicherheit

Das Vorhandensein Varianzkennzeichnungen in der Typparameterliste eines Typs schränkt die Orte, wo die Typen in der Typdeklaration auftreten können.

Ein Typ `T` ist ***Ausgabe / unsafe*** , wenn eine der folgenden enthält:

*  `T` Ein kontravarianter Typparameter wird
*  `T` ist ein Arraytyp mit einer Ausgabe / unsafe-Elementtyp
*  `T` ist ein Typ von Schnittstellen oder Delegate `S<A1,...,Ak>` aus einem generischen Typ erstellter `S<X1,...,Xk>` Where für mindestens eine `Ai` eine der folgenden enthält:
   * `Xi` ist kovariant oder Invarianten und `Ai` Ausgabe-unsicher ist.
   * `Xi` ist kontravariant oder Invarianten und `Ai` Eingabe-sicher ist.
   
Ein Typ `T` ist ***Eingabe-/ unsafe*** , wenn eine der folgenden enthält:

*  `T` ein kovarianter Typparameter wird
*  `T` ist ein Arraytyp mit einem Eingabe-/ unsafe-Elementtyp
*  `T` ist ein Typ von Schnittstellen oder Delegate `S<A1,...,Ak>` aus einem generischen Typ erstellter `S<X1,...,Xk>` Where für mindestens eine `Ai` eine der folgenden enthält:
   * `Xi` ist kovariant oder Invarianten und `Ai` Eingabe-unsicher ist.
   * `Xi` ist kontravariant oder Invarianten und `Ai` Ausgabe-unsicher ist.

Intuitiv Ausgabe-unsicherem Typ ist nicht zulässig, in einer Ausgabeposition und Eingabe-unsicherem Typ ist nicht zulässig, in die Position einer Eingabe.

Ein Typ ist ***Ausgabe-sichere*** ist dies nicht Ausgabe unsicher, und ***Eingabe-sichere*** ist dies nicht Eingabe-/ unsafe.

#### <a name="variance-conversion"></a>Varianzkonvertierungen

Der Zweck der Varianzkennzeichnungen ist weniger strenge (aber immer noch typsicher) Konvertierungen in Schnittstellen-und Delegattypen bereit. So beenden Sie die Definitionen der implizite ([implizite Konvertierungen](conversions.md#implicit-conversions)) und explizite Konvertierungen ([explizite Konvertierungen](conversions.md#explicit-conversions)) stellen verwenden das Konzept der varianzkonvertierbarkeit, die wie folgt definiert ist:

Ein Typ `T<A1,...,An>` ist varianzkonvertierbar in einen Typ `T<B1,...,Bn>` Wenn `T` ist entweder eine Schnittstelle oder ein Delegattyp deklariert mit den Parametern der variant-Typ `T<X1,...,Xn>`, und für jeden Variante Typparameter `Xi` eine der folgenden enthält:

*  `Xi` ist "covariant" und eine implizite Konvertierung von Verweis- oder Identität vorhanden ist, von `Ai` auf `Bi`
*  `Xi` kontravariant ist und einen impliziten Verweis oder identitätskonvertierung vorhanden ist, von `Bi` auf `Ai`
*  `Xi` ist die invariante und eine Identität Konvertierungen von `Ai` zu `Bi`

### <a name="base-interfaces"></a>Basisschnittstellen

Eine Schnittstelle kann von NULL oder mehr Schnittstellentypen, die aufgerufen werden, erben die ***explizite Basisschnittstelle*** der Schnittstelle. Wenn eine Schnittstelle ein oder mehrere explizite Basisschnittstelle verfügt, in der Deklaration dieser Schnittstelle, der Schnittstellenbezeichner wird anschließend von einem Doppelpunkt und einer durch Trennzeichen getrennte Liste der Basisschnittstelle-Typen.

```antlr
interface_base
    : ':' interface_type_list
    ;
```

Für einen Typ konstruierte Schnittstelle werden die explizite Basisschnittstelle gebildet, indem die explizite Basisschnittstelle-Deklarationen in der Deklaration des generischen Typs und ersetzt werden, für die einzelnen *Type_parameter* in der Basisschnittstelle Deklaration der entsprechenden *Type_argument* des konstruierten Typs.

Die explizite Basisschnittstelle einer Schnittstelle muss mindestens dieselben zugriffsmöglichkeiten wie der Schnittstelle selbst ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)). Ist beispielsweise ein Fehler während der Kompilierung an eine `private` oder `internal` -Schnittstelle in der *Interface_base* von einer `public` Schnittstelle.

Es ist ein Fehler während der Kompilierung für eine Schnittstelle, die direkt oder indirekt von sich selbst erben.

Die ***Basisschnittstellen*** von einer Schnittstelle sind die explizite Basisschnittstelle und ihre Schnittstellen. Der Satz von Schnittstellen ist also die vollständige transitiven Abschluss von die explizite Basisschnittstelle, die explizite Basisschnittstelle und So weiter. Eine Schnittstelle erbt alle Member der Basisschnittstellen. Im Beispiel
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
die Basisschnittstelle `IComboBox` sind `IControl`, `ITextBox`, und `IListBox`.

Das heißt, die `IComboBox` obige Schnittstelle erbt Member `SetText` und `SetItems` sowie `Paint`.

Jeder Basisschnittstelle einer Schnittstelle muss die Ausgabe-Safe ([Varianz Sicherheit](interfaces.md#variance-safety)). Eine Klasse oder Struktur, die eine Schnittstelle, auch implizit implementiert implementiert alle die Basisschnittstellen.

### <a name="interface-body"></a>Schnittstelle-Text

Die *Interface_body* einer Schnittstelle definiert den Member der Schnittstelle.

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a>Schnittstellenmember

Die Member einer Schnittstelle sind die von der Basisschnittstellen geerbten Member aus, und die Member der Schnittstelle selbst deklariert.

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

Eine Schnittstellendeklaration kann NULL oder mehr Member deklarieren. Die Member einer Schnittstelle müssen es sich um Methoden, Eigenschaften, Ereignisse und Indexer sein. Eine Schnittstelle darf keine Konstanten, Felder, Operatoren, Instanzkonstruktoren, Destruktoren oder Typen enthalten, noch kann eine Schnittstelle enthalten statische Member irgendeiner Art.

Alle Schnittstellenmember sind implizit über öffentlichen Zugriff. Es ist ein Fehler während der Kompilierung für Schnittstellenmember-Deklarationen Modifizierer enthalten. Insbesondere können nicht Schnittstellenmember deklariert werden, mit den Modifizierern `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, oder `static`.

Im Beispiel
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
deklariert eine Schnittstelle, die jeweils eine der möglichen Arten von Member enthält: eine Methode, eine Eigenschaft, ein Ereignis und einen Indexer.

Ein *Interface_declaration* erstellt einen neuen Deklarationsabschnitt ([Deklarationen](basic-concepts.md#declarations)), und die *Interface_member_declaration*s sofort die enthaltenen*Interface_declaration* neue Elemente in diesen Deklarationsabschnitt einführen. Die folgenden Regeln gelten für *Interface_member_declaration*s:

*  Der Name einer Methode muss die Namen aller Eigenschaften und Ereignisse, die in der gleichen Schnittstelle deklariert unterscheiden. Außerdem wird die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) von eine Methode muss die Signaturen aller anderen in derselben Schnittstelle deklarierten Methoden unterscheiden, und zwei Methoden, die in der gleichen Schnittstelle deklarierten dürfen keine Signaturen, unterscheiden sich ausschließlich vom `ref` und `out`.
*  Der Name der Eigenschaft bzw. das Ereignis muss die Namen aller anderen in derselben Schnittstelle deklarierten Member unterscheiden.
*  Die Signatur eines Indexers muss sich von den Signaturen aller anderen in derselben Schnittstelle deklarierten Indexer unterscheiden.

Die geerbten Member einer Schnittstelle sind speziell nicht Teil des Deklarationsabschnitts der Schnittstelle. Daher ist eine Schnittstelle zulässig, um einen Member mit demselben Namen bzw. derselben Signatur als einen geerbten Member deklarieren. In diesem Fall wird der Member der abgeleiteten Schnittstelle als die Member der Basisschnittstelle ausblenden möchten. Durch das Ausblenden eines geerbten Members ist kein Fehler betrachtet, aber es führt dazu, dass des Compilers eine Warnung ausgegeben. Um die Warnung zu unterdrücken, muss die Deklaration der abgeleiteten Schnittstellenmember enthalten eine `new` Modifizierer, um anzugeben, dass der abgeleitete Member die Member den Basisklassen ausblenden vorgesehen ist. In diesem Thema wird erläutert. im weiteren [ausblenden, die durch Vererbung von](basic-concepts.md#hiding-through-inheritance).

Wenn eine `new` Modifizierer ist in einer Deklaration, die einen geerbten Member auszublenden, nicht enthalten, die zu diesem Zweck wird eine Warnung ausgegeben. Diese Warnung unterdrückt wird, durch das Entfernen der `new` Modifizierer.

Beachten Sie, dass die Elemente in der Klasse `object` sind nicht streng genommen, Mitglied einer beliebigen Schnittstelle ([Schnittstellenmember](interfaces.md#interface-members)). Allerdings die Elemente in der Klasse `object` stehen über die Suche nach Membern in einen Schnittstellentyp ([Membersuche](expressions.md#member-lookup)).

### <a name="interface-methods"></a>Schnittstellenmethoden

Schnittstellenmethoden deklariert werden, mithilfe von *Interface_method_declaration*s:

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

Die *Attribute*, *Return_type*, *Bezeichner*, und *Formal_parameter_list* einer Methodendeklaration Schnittstelle weist den gleichen Dies bedeutet, wie die Deklaration einer Methode in einer Klasse ([Methoden](classes.md#methods)). Eine Schnittstellen-Methodendeklaration ist nicht zulässig, einen Methodentext an, und die Deklaration ist daher immer mit einem Semikolon endet.

Jeder formalen Parametertyp einer Schnittstellenmethode muss die Eingabe-Safe ([Varianz Sicherheit](interfaces.md#variance-safety)), und der Rückgabetyp muss entweder `void` oder Ausgabe-sicher. Darüber hinaus muss jeder klassentypeinschränkung, die Schnittstelle eine Einschränkung und die Einschränkung eines Typparameters auf einen Typparameter der Methode Eingabe threadsicher sein.

Diese Regeln stellen sicher, dass jede Kovariante oder die kontravariant Nutzung der Schnittstelle bleibt typsicher. Ein auf ein Objekt angewendeter
```csharp
interface I<out T> { void M<U>() where U : T; }
```
ist nicht zulässig da die Verwendung von `T` als Einschränkung für einen Parameter auf `U` ist nicht Eingabe-sicher.

Diese Einschränkung nicht vorhanden waren wäre, typsicherheit auf folgende Weise zu verletzen möglich es:
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
Dies ist tatsächlich ein Aufruf zum `C.M<E>`. Aber dieser Aufruf, erfordert `E` abgeleitet `D`, sodass die typsicherheit hier ggf. verletzt wird.

### <a name="interface-properties"></a>Schnittstelleneigenschaften

Schnittstelleneigenschaften deklariert werden, mithilfe von *Interface_property_declaration*s:

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

Die *Attribute*, *Typ*, und *Bezeichner* eine Schnittstellen-Eigenschaftendeklaration müssen die gleiche Bedeutung wie eine Eigenschaftendeklaration in einer Klasse ([ Eigenschaften](classes.md#properties)).

Die Accessoren eine Schnittstellen-Eigenschaftendeklaration entsprechen den Accessoren einer Deklaration einer Klasseneigenschaft ([Accessoren](classes.md#accessors)), außer dass der Accessortext hinausgehen immer ein Semikolon sein muss. Daher Accessors wird angegeben, ob die Eigenschaft Lese-/ Schreibzugriff, schreibgeschützt oder lesegeschützt ist.

Der Typ einer Schnittstelleneigenschaft muss die Ausgabe-Safe, falls ein Get-Accessor vorhanden ist und sein Eingabe-Safe, wenn ein Set-Accessor vorhanden ist.

### <a name="interface-events"></a>Ereignisse der Benutzeroberfläche

Ereignisse der Benutzeroberfläche werden mit deklariert *Interface_event_declaration*s:

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

Die *Attribute*, *Typ*, und *Bezeichner* einer Schnittstellen-Ereignisdeklaration müssen die gleiche Bedeutung wie die Deklaration für einen Ereignis in einer Klasse ([Ereignisse ](classes.md#events)).

Der Typ eines Ereignisses für die Schnittstelle muss Eingabe threadsicher sein.

### <a name="interface-indexers"></a>Indexer in Schnittstellen

Schnittstellenindexer deklariert werden, mithilfe von *Interface_indexer_declaration*s:

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

Die *Attribute*, *Typ*, und *Formal_parameter_list* eine Schnittstellendeklaration für Indexer müssen die gleiche Bedeutung wie die Deklaration für einen Indexer in einer Klasse ([ Indexer](classes.md#indexers)).

Die Accessoren des Indexers Schnittstellendeklarationen entsprechen den Accessoren einer Klassendeklaration für Indexer ([Indexer](classes.md#indexers)), außer dass der Accessortext hinausgehen immer ein Semikolon sein muss. Daher Accessors wird angegeben, ob der Indexer Lese-/ Schreibzugriff, schreibgeschützt oder lesegeschützt ist.

Alle Typen der formalen Parameter eines Indexers Schnittstelle müssen die Eingabe threadsicher sein. Darüber hinaus alle `out` oder `ref` Typen der formalen Parameter müssen auch Ausgabe threadsicher sein. Beachten Sie, dass sogar `out` Parameter sind erforderlich, um die Eingabe-Safe, aufgrund einer Einschränkung von der zugrunde liegenden Ausführungsplattform zu werden.

Der Typ eines Indexers Schnittstelle muss die Ausgabe-Safe, falls ein Get-Accessor vorhanden ist und sein Eingabe-Safe, wenn ein Set-Accessor vorhanden ist.

### <a name="interface-member-access"></a>Schnittstelle Memberzugriff

Schnittstellenmember erfolgt über den Memberzugriff ([Memberzugriff](expressions.md#member-access)) und Indexerzugriff ([Indexerzugriff](expressions.md#indexer-access)) Ausdrücken der Form `I.M` und `I[A]`, wobei `I` ist ein Schnittstellentyp `M` ist eine Methode, Eigenschaft oder das Ereignis, der diesen Schnittstellentyp und `A` ist eine Liste der Indexer-Argument.

Für Schnittstellen, die streng sind einzelne Vererbung (jede Schnittstelle in der Vererbungskette hat genau 0 (null) oder eine direkte Basisschnittstelle), die Auswirkungen der Suche nach Membern ([Membersuche](expressions.md#member-lookup)), Methodenaufruf ([ Methodenaufrufe](expressions.md#method-invocations)), und Indexerzugriff ([Indexerzugriff](expressions.md#indexer-access)) Regeln sind genau die gleichen wie für Klassen und Strukturen: stärker abgeleiteten Elemente ausblenden weniger abgeleiteten Elemente mit demselben Namen bzw. derselben Signatur. Für mehrfache Vererbung Schnittstellen jedoch Mehrdeutigkeiten auftreten, wenn zwei oder mehr nicht verknüpfte Basisschnittstellen deklariert Member mit demselben Namen bzw. derselben Signatur. Dieser Abschnitt zeigt einige Beispiele für solche Situationen. In allen Fällen können explizite Umwandlungen verwendet werden, um die Mehrdeutigkeiten aufzulösen.

Im Beispiel
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
die ersten beiden Anweisungen verursachen Kompilierzeitfehler, da die Membersuche ([Membersuche](expressions.md#member-lookup)) der `Count` in `IListCounter` ist mehrdeutig. Wie im Beispiel gezeigt wird, wird die Mehrdeutigkeit aufgelöst, indem Sie eine Umwandlung `x` in den entsprechenden Basisschnittstelle-Typ. Solche Umwandlungen haben keine Laufzeit — sie bestehen lediglich zum Anzeigen der Instanz als weniger stark abgeleiteten Typ zum Zeitpunkt der Kompilierung aus.

Im Beispiel
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
der Aufruf `n.Add(1)` wählt `IInteger.Add` durch Anwenden der Regeln der überladungsauflösung des [Überladungsauflösung](expressions.md#overload-resolution). Analog dazu den Aufruf `n.Add(1.0)` wählt `IDouble.Add`. Wenn explizite Umwandlungen eingefügt werden, besteht nur ein Kandidat-Methode, und daher keine Mehrdeutigkeit.

Im Beispiel
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
die `IBase.F` Element wird ausgeblendet, indem die `ILeft.F` Member. Der Aufruf `d.F(1)` daher wählt `ILeft.F`, auch wenn `IBase.F` angezeigt wird, nicht in den Zugriffspfad ausgeblendet werden, das durch führt `IRight`.

Die Faustregel für das Ausblenden in mehrere Vererbungen Schnittstellen ist dies: Wenn ein Element in einem Zugriffspfad ausgeblendet ist, wird Sie in der alle Zugriffspfade ausgeblendet. Da Zugriffspfad aus `IDerived` zu `ILeft` zu `IBase` blendet `IBase.F`, das Element wird auch in den Zugriffspfad aus ausgeblendet `IDerived` zu `IRight` zu `IBase`.

## <a name="fully-qualified-interface-member-names"></a>Den vollqualifizierten Namen von Schnittstellenmembern

Ein Schnittstellenmember wird mitunter die ***voll gekennzeichneten Namen***. Der vollqualifizierte Name eines Schnittstellenmembers besteht aus den Namen der Schnittstelle in der der Member ist deklariert, gefolgt von einem Punkt, gefolgt vom Namen des Members. Der vollqualifizierte Name eines Elements verweist auf die Schnittstelle, die in der das Element deklariert wird. Angenommen, die Deklarationen
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
der vollqualifizierte Name des `Paint` ist `IControl.Paint` und den vollqualifizierten Namen der `SetText` ist `ITextBox.SetText`.

Im obigen Beispiel ist es nicht möglich, verweisen auf `Paint` als `ITextBox.Paint`.

Wenn eine Schnittstelle Teil eines Namespace ist, enthält der vollqualifizierte Name eines Schnittstellenmembers Name des Namespaces an. Beispiel:
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

Hier wird der vollqualifizierte Name des der `Clone` Methode `System.ICloneable.Clone`.

## <a name="interface-implementations"></a>Schnittstellenimplementierungen

Schnittstellen können von Klassen und Strukturen implementiert werden. Um anzugeben, dass eine Klasse oder Struktur direkt eine Schnittstelle implementiert, ist der Schnittstellenbezeichner in der Basisklassenliste der Klasse oder Struktur enthalten. Zum Beispiel:
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

Eine Klasse oder Struktur, die direkt für die eine Schnittstelle auch direkt implementiert alle Schnittstellen der Schnittstelle implizit implementiert werden. Dies gilt auch, wenn die Klasse oder Struktur ausdrücklich alle Basisschnittstellen in der Basisklassenliste nicht. Zum Beispiel:
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

Hier Klasse `TextBox` implementiert beide `IControl` und `ITextBox`.

Wenn eine Klasse `C` direkt implementiert eine Schnittstelle, die alle von C abgeleitete Klassen auch implizit die Schnittstelle implementieren. Die Basisschnittstellen, die in einer Klassendeklaration angegeben können es sich um konstruierte Schnittstelle-Typen ([Typen konstruiert](types.md#constructed-types)). Eine Basisschnittstelle darf nicht Typparameter in ihren eigenen sein, obwohl er die Typparameter umfassen kann, die im Gültigkeitsbereich befinden. Der folgende Code zeigt, wie eine Klasse implementieren und konstruierte Typen erweitern kann:
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

Die Basisschnittstellen der Deklaration einer generischen Klasse erfüllen müssen, die Eindeutigkeit-Regel, die in beschriebenen [Eindeutigkeit der implementierten Schnittstellen](interfaces.md#uniqueness-of-implemented-interfaces).

### <a name="explicit-interface-member-implementations"></a>Explizite schnittstellenimplementierungen-Element

Für die Zwecke der Implementierung von Schnittstellen, eine Klasse oder Struktur deklarieren kann ***explizite Implementierungen eines Schnittstellenmembers***. Eine explizite Schnittstellenmember-Implementierung ist eine Methode, Eigenschaft, Ereignis, oder der Indexer-Deklaration, die einen vollständig qualifizierten Schnittstelle Membernamen verweist. Beispiel:
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

Hier `IDictionary<int,T>.this` und `IDictionary<int,T>.Add` sind explizite Implementierungen eines Schnittstellenmembers.

In einigen Fällen kann der Name eines Schnittstellenmembers für die implementierende Klasse, nicht in dem Fall den Schnittstellenmember implementiert werden kann, verwenden explizite Schnittstellenmember-Implementierung. Implementieren einer Klasse, die eine Dateiabstraktion, die z. B. implementieren würde wahrscheinlich eine `Close` Memberfunktion, die die Auswirkungen der Freigabe der Ressource "File", und Implementieren der `Dispose` -Methode der der `IDisposable` Schnittstelle. dabei wird der expliziten Implementierung des Schnittstellenmembers:
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

Es ist nicht möglich, eine explizite Schnittstellenmember-Implementierung über dessen vollqualifizierten Namen in einen Methodenaufruf, der Zugriff auf Eigenschaften oder Indexerzugriff zugreifen. Eine explizite Schnittstellenmember-Implementierung kann nur über eine Schnittstelleninstanz zugegriffen werden, und wird in diesem Fall einfach durch den Membernamen verwiesen.

Ist ein Fehler während der Kompilierung für eine explizite Schnittstellenmember-Implementierung Zugriffsmodifizierer eingeschlossen, und es ist ein Fehler während der Kompilierung, um die Modifizierer sind `abstract`, `virtual`, `override`, oder `static`.

Explizite Implementierungen eines Schnittstellenmembers weisen verschiedene Eingabehilfen Merkmale als andere Elemente auf. Da explizite schnittstellenimplementierungen-Member nie über deren vollqualifizierter Name im Aufruf einer Methode oder ein Eigenschaftenzugriff zugänglich sind, sind sie in einem privaten Sinne. Da sie über eine Schnittstelleninstanz zugegriffen werden kann, Sie sind jedoch in gewisser Hinsicht auch öffentliche.

Explizite Implementierungen eines Schnittstellenmembers dienen zwei Hauptaufgaben:

*  Da explizite Implementierungen eines Schnittstellenmembers nicht über die Klasse oder Struktur Instanzen zugänglich sind, können sie schnittstellenimplementierungen aus der öffentlichen Schnittstelle einer Klasse oder Struktur ausgeschlossen werden sollen. Dies ist besonders nützlich, wenn eine Klasse oder Struktur implementiert eine interne Schnittstelle, die nicht von Interesse an einen Consumer diese Klasse oder Struktur ist.
*  Explizite Implementierungen eines Schnittstellenmembers können zur Klärung von Schnittstellenmembern mit derselben Signatur. Ohne explizite schnittstellenimplementierungen für Member werden einer Klasse oder Struktur nicht haben unterschiedliche Implementierungen der Schnittstellenmember mit derselben Signatur und den Rückgabetyp, als es unmöglich, dass eine Klasse oder Struktur wäre, haben eine beliebige Implementierung Geben Sie im Vorfeld für alle Schnittstellenmember mit derselben Signatur jedoch mit unterschiedlichen Typen zurück.

Für eine explizite Schnittstellenmember-Implementierung gültig ist muss die Klasse oder Struktur eine Schnittstelle in der Basisklassenliste Namen, die ein Element enthält, deren vollqualifizierten Namen, Typ und Parametertypen genau den explizite Schnittstellenmember übereinstimmen -Implementierung. Daher in der folgenden Klasse
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
die Deklaration von `IComparable.CompareTo` führt zu einem Kompilierzeitfehler, da `IComparable` ist nicht aufgeführt, in der Basisklassenliste der `Shape` und keine Basisschnittstelle `ICloneable`. Ebenso in den Deklarationen
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
die Deklaration von `ICloneable.Clone` in `Ellipse` führt zu einem Kompilierzeitfehler, da `ICloneable` ist nicht in der Basisklassenliste der explizit aufgeführt `Ellipse`.

Der vollqualifizierte Name eines Schnittstellenmembers muss die Schnittstelle referenzieren, in der der Member deklariert wurde. Daher in den Deklarationen
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
die Implementierung der explizite Schnittstellenmember `Paint` muss geschrieben werden, als `IControl.Paint`.

### <a name="uniqueness-of-implemented-interfaces"></a>Die Eindeutigkeit der implementierten Schnittstellen

Die von der Deklaration eines generischen Typs implementierten Schnittstellen müssen eindeutig für alle möglichen konstruierte Typen bleiben. Ohne diese Regel wäre es nicht möglich, um zu bestimmen, die richtige Methode für bestimmte konstruierten Typen aufrufen. Nehmen wir beispielsweise an die Deklaration einer generischen Klasse darf wurden wie folgt geschrieben werden:
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

Dies zulässig wäre, wäre es nicht möglich, um zu bestimmen, welcher Code führen Sie im folgenden Fall:
```csharp
I<int> x = new X<int,int>();
x.F();
```

Um festzustellen, ob die Liste der Deklaration eines generischen Typs Interface gültig ist, werden die folgenden Schritte ausgeführt:

*  Lassen Sie `L` werden die Liste der Schnittstellen, die direkt in eine generische Klasse, Struktur oder Schnittstellendeklaration angegebenen `C`.
*  Hinzufügen zu `L` alle Basisschnittstellen die Schnittstellen, die bereits in `L`.
*  Entfernen Sie Duplikate von `L`.
*  Eine mögliche von erstellten Typ erstellt `C` würde, nachdem die Typargumente ersetzt werden `L`, dazu führen, dass zwei Schnittstellen in `L` identisch ist, klicken Sie dann die Deklaration von `C` ist ungültig. Einschränkung Deklarationen sind nicht berücksichtigt, wenn es sich bei allen mögliche konstruierte Typen zu ermitteln.

In der Klassendeklaration `X` oben die Schnittstellenliste `L` besteht aus `I<U>` und `I<V>`. Die Deklaration ist ungültig, da alle Typen mit erstellt `U` und `V` wird der gleiche Typ führt dazu, dass diese beiden Schnittstellen identische Typen sein.

Es ist möglich, für die Schnittstellen, die auf verschiedenen Vererbungsebenen zur Vereinheitlichung angegeben:
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

Dieser Code ist gültig, obwohl `Derived<U,V>` implementiert beide `I<U>` und `I<V>`. Der code
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
Ruft die Methode in `Derived`, da `Derived<int,int>` effektiv neu implementiert `I<int>` ([erneute Schnittstellenimplementierung](interfaces.md#interface-re-implementation)).

### <a name="implementation-of-generic-methods"></a>Implementierung der generischen Methoden

Wenn eine generische Methode implizit implementiert eine Schnittstellenmethode erhalten, für jeden Typparameter der Methode muss in beiden Deklarationen entspricht (nach einem Schnittstellentyp Parameter werden durch die entsprechenden Typargumente ersetzt), in denen Einschränkungen Methode Typparameter werden durch die Ordnungspositionen, identifiziert, von links nach rechts.

Wenn eine generische Methode explizit eine Schnittstellenmethode implementiert, sind jedoch keine Einschränkungen für die implementierende Methode zulässig. Die Einschränkungen werden stattdessen über die Schnittstellenmethode geerbt.

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

Die Methode `C.F<T>` implizit implementiert `I<object,C,string>.F<T>`. In diesem Fall `C.F<T>` ist nicht erforderlich (noch zulässig), geben Sie die Einschränkung `T:object` da `object` eine implizite Einschränkung für alle Typparameter spezifiziert ist. Die Methode `C.G<T>` implizit implementiert `I<object,C,string>.G<T>` , da die Einschränkungen mit den in der Schnittstelle übereinstimmen, nachdem die Typparameter der Schnittstelle durch die entsprechenden Typargumente ersetzt werden. Die Einschränkung für die Methode `C.H<T>` ist ein Fehler, da die Typen versiegelt (`string` in diesem Fall) kann nicht als Einschränkungen verwendet werden. Das Auslassen der Einschränkung wäre ein Fehler auch da Einschränkungen der Schnittstelle implizit methodenimplementierungen entsprechend erforderlich sind. Daher ist es nicht möglich, implizit implementieren `I<object,C,string>.H<T>`. Dieser Schnittstellenmethode kann nur über eine explizite schnittstellenimplementierung für den Member implementiert werden:
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

In diesem Beispiel ruft die explizite Schnittstellenmember-Implementierung eine öffentliche Methode, die mit streng geringere Einschränkungen. Beachten Sie, dass der Zuweisung von `t` zu `s` gilt seit `T` erbt eine Einschränkung des `T:string`, auch wenn diese Einschränkung nicht ausgedrückt werden kann, im Quellcode verwendet wird.

### <a name="interface-mapping"></a>Schnittstellenzuordnung

Eine Klasse oder Struktur muss Implementierungen aller Elemente der Schnittstellen bieten, die in der Basisklassenliste der Klasse oder Struktur aufgeführt sind. Der Prozess zum Auffinden von Implementierungen von Schnittstellenmembern in eine implementierende Klasse oder Struktur wird als bezeichnet ***schnittstellenzuordnung***.

Schnittstellenzuordnung für eine Klasse oder Struktur `C` sucht eine Implementierung für jedes Element der einzelnen Schnittstellen, die in der Basisklassenliste des angegebenen `C`. Die Implementierung der Member einer bestimmten Schnittstelle `I.M`, wobei `I` ist die Schnittstelle in dem das Element `M` deklariert wird, richtet sich nach der Untersuchung von jeder Klasse oder Struktur `S`, beginnend mit `C` und Wiederholen für jeder nachfolgenden Basisklasse von `C`, bis eine Übereinstimmung gefunden wird:

*  Wenn `S` enthält eine Deklaration des eine explizite Schnittstellenmember-Implementierung, die entspricht `I` und `M`, dann ist dieser Member die Implementierung der `I.M`.
*  Andernfalls gilt: Wenn `S` enthält eine Deklaration eines öffentlichen nicht statischen Members, der entspricht `M`, dann ist dieser Member die Implementierung der `I.M`. Falls mehr als ein Element entspricht, es nicht angegeben wird, ist welches Element die Implementierung der `I.M`. Diese Situation kann nur auftreten, wenn `S` ein konstruierten Typs, wenn die beiden Elemente, wie in den generischen Typ deklariert verschiedene Signaturen aufweisen, aber die Typargumente stellen ihren Signaturen identisch, ist.

Ein Fehler während der Kompilierung tritt auf, wenn Sie Implementierungen für alle Elemente aller Schnittstellen, die in der Basisklassenliste des angegebenen nicht `C`. Beachten Sie, dass die Member einer Schnittstelle die Elemente enthalten, die von Basisschnittstellen geerbt werden.

Zum Zweck der schnittstellenzuordnung, einen Klassenmember `A` ein Schnittstellenmembers entspricht `B` bei:

*  `A` und `B` sind Methoden, und der Name, Typ, und die Liste der formalen Parameter der `A` und `B` identisch sind.
*  `A` und `B` sind Eigenschaften, die Namen und Typ des `A` und `B` sind nahezu identisch, und `A` weist dieselben Accessoren als `B` (`A` ist zusätzliche Accessoren haben, wenn es sich nicht um eine explizite Schnittstelle ist zulässig. Memberimplementierung).
*  `A` und `B` sind Ereignisse, Name und Typ des `A` und `B` identisch sind.
*  `A` und `B` werden Indexer, der Typ und die Listen der formalen Parameter `A` und `B` sind nahezu identisch, und `A` weist dieselben Accessoren als `B` (`A` zusätzliche Accessoren haben, ist dies nicht zulässig ist eine explizite Schnittstellenmember-Implementierung).

Wesentliche Auswirkungen auf den Mappingalgorithmus Schnittstelle sind:

*  Explizite schnittstellenimplementierungen-Element haben Vorrang gegenüber anderen Elementen in der gleichen Klasse oder Struktur, wenn Member der Klasse oder Struktur bestimmt wird, das einen Schnittstellenmember implementiert.
*  Nicht öffentliche weder statische Elemente beteiligt schnittstellenzuordnung ab.

Im Beispiel
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
die `ICloneable.Clone` Mitglied `C` wird die Implementierung der `Clone` in `ICloneable` da explizite Implementierungen eines Schnittstellenmembers gegenüber anderen Mitgliedern Vorrang.

Wenn eine Klasse oder Struktur zwei implementiert oder mehr Schnittstellen, die mit einem Element mit dem gleichen Namen, den Typ und die Parametertypen, ist es möglich, jede dieser Schnittstellenmember einen einzelnen Member der Klasse oder Struktur zuzuordnen. Beispiel:
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

Hier ist die `Paint` Methoden von `IControl` und `IForm` zugeordnet sind, auf die `Paint` -Methode in der `Page`. Es ist natürlich auch möglich, separate, explizite schnittstellenimplementierungen von Membern für die zwei Methoden haben.

Wenn eine Klasse oder Struktur eine Schnittstelle, die ausgeblendete Elemente enthält implementiert, müssen unbedingt einige Member über explizite Implementierungen eines Schnittstellenmembers implementiert werden. Beispiel:
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

Eine Implementierung dieser Schnittstelle benötigt mindestens eine explizite Schnittstellenmember-Implementierung, und würde einen der folgenden Formen annehmen
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

Wenn eine Klasse mehrere Schnittstellen, die die gleiche Basis-Schnittstelle verfügen implementiert, können nur eine Implementierung der Basisschnittstelle vorhanden sein. Im Beispiel
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
Es ist nicht möglich, separate Implementierungen für die `IControl` mit dem Namen in der Basisklassenliste der `IControl` geerbt `ITextBox`, und die `IControl` geerbt `IListBox`. Es gibt tatsächlich keiner separaten Identität für diese Schnittstellen. Vielmehr die Implementierungen der `ITextBox` und `IListBox` Teilen zusammen dieselbe Implementierung von `IControl`, und `ComboBox` gilt lediglich drei Schnittstellen implementieren `IControl`, `ITextBox`, und `IListBox`.

Die Member einer Basisklasse beteiligt schnittstellenzuordnung ab. Im Beispiel
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
die Methode `F` in `Class1` werden in `Class2`Implementierung von `Interface1`.

### <a name="interface-implementation-inheritance"></a>Schnittstellenvererbung-Implementierung

Eine Klasse erbt alle schnittstellenimplementierungen, die von ihrer Basisklassen bereitgestellt.

Ohne explizit ***erneut zu implementieren*** eine Schnittstelle, eine abgeleitete Klasse kann nicht in keiner Weise geändert werden die Mappings der Schnittstelle von der zugehörigen Basisklassen erbt. Z. B. in den Deklarationen
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
die `Paint` -Methode in der `TextBox` Blendet Sie aus der `Paint` -Methode in der `Control`, es er ändert jedoch nicht die Zuordnung von `Control.Paint` auf `IControl.Paint`, und Aufrufe von `Paint` über Klasse Instanzen sowie Schnittstelleninstanzen werden die folgenden Auswirkungen haben
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

Wenn eine Schnittstellenmethode auf eine virtuelle Methode in einer Klasse zugeordnet ist, ist es jedoch möglich, dass abgeleitete Klassen überschreiben die virtuelle Methode, und ändern Sie die Implementierung der Schnittstelle. Umschreiben z. B. die Deklarationen oben hinzu
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
die folgenden Auswirkungen werden jetzt beachtet werden soll
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

Da explizite Implementierungen eines Schnittstellenmembers virtuellen können nicht deklariert werden, ist es nicht möglich, eine explizite Schnittstellenmember-Implementierung zu überschreiben. Allerdings uneingeschränkt für eine explizite Schnittstellenmember-Implementierung eine andere Methode aufruft, und, dass andere Methode virtuell deklariert werden kann abgeleitete Klassen, um sie zu überschreiben. Beispiel:
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

Hier von abgeleiteten Klassen `Control` kann die Implementierung der "specialize" `IControl.Paint` durch Überschreiben der `PaintControl` Methode.

### <a name="interface-re-implementation"></a>Die erneute schnittstellenimplementierung

Eine Klasse, eine schnittstellenimplementierung erbt, ist zulässig, ***erneut implementieren*** der Schnittstelle durch Integrieren der Codekomponente in der Basisklassenliste.

Eine erneute Implementierung einer Schnittstelle folgt genau die gleiche Schnittstelle Zuordnungsregeln an wie die erste Implementierung einer Schnittstelle. Daher wirkt sich die geerbte schnittstellenzuordnung nicht überhaupt auf die schnittstellenzuordnung für die erneute Implementierung der Schnittstelle hergestellt. Z. B. in den Deklarationen
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
die Tatsache, `Control` ordnet `IControl.Paint` auf `Control.IControl.Paint` wirkt sich nicht auf die erneute Implementierung von `MyControl`, welche Karten `IControl.Paint` auf `MyControl.Paint`.

Geerbte öffentliche Memberdeklarationen und geerbten explizite Schnittstellenmember, die Deklarationen Teilnahme an der Schnittstelle Zuordnungsprozess für neu implementierten Schnittstellen. Beispiel:
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

Hier wird die Implementierung der `IMethods` in `Derived` ordnet die Schnittstellenmethoden `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, und `Base.I`.

Wenn eine Klasse eine Schnittstelle, implementiert er implizit implementiert auch alle diese Basisschnittstellen. Ebenso ist eine erneute Implementierung einer Schnittstelle auch implizit eine erneute Implementierung der Basisschnittstellen. Beispiel:
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

Hier wird die erneute Implementierung von `IDerived` erneut implementiert auch `IBase`und ordnet `IBase.F` auf `D.F`.

### <a name="abstract-classes-and-interfaces"></a>Abstrakte Klassen und Schnittstellen

Wie einer nicht abstrakten Klasse muss eine abstrakte Klasse Implementierungen von allen Membern der Schnittstellen bereitstellen, die in der Basisklassenliste der Klasse aufgeführt sind. Allerdings ist eine abstrakte Klasse zulässig, Schnittstellenmethoden abstrakte Methoden zuordnen. Beispiel:
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

Hier wird die Implementierung der `IMethods` ordnet `F` und `G` auf abstrakte Methoden, die in der nicht abstrakte abgeleitete Klassen überschrieben werden muss `C`.

Beachten Sie, dass explizite schnittstellenimplementierungen-Element darf nicht abstrakt sein, aber die explizite Implementierungen eines Schnittstellenmembers sind natürlich abstrakte Methoden aufrufen darf. Beispiel:
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

Hier wird von abgeleiteten nicht abstrakten Klassen `C` wäre erforderlich, außer Kraft setzen `FF` und `GG`, dadurch wird die tatsächliche Implementierung der `IMethods`.
