# <a name="enums"></a>Enumerationen

Ein ***Enumerationstyp*** ist ein unverwechselbarer Werttyp ([Werttypen](types.md#value-types)), die einen Satz benannter Konstanten deklariert.

Im Beispiel

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

deklariert einen Enum-Typ, der mit dem Namen `Color` mit Mitgliedern `Red`, `Green`, und `Blue`.

## <a name="enum-declarations"></a>Enum-Deklarationen

Eine Enum-Deklaration deklariert einen neue Enum-Typ. Eine Enum-Deklaration beginnt mit dem Schlüsselwort `enum`, und definiert den Namen, die Barrierefreiheit, die zugrunde liegenden Typ und die Member der Enumeration.

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

Jeder Enumerationstyp hat einen entsprechenden ganzzahligen Typ, der Namen der ***zugrunde liegender Typ*** des Enum-Typs. Diese zugrunde liegenden Typ muss alle in der Enumeration definierten Enumeratorwerte darstellen können. Eine Enum-Deklaration kann explizit einen zugrunde liegenden Typ deklarieren `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` oder `ulong`. Beachten Sie, dass `char` nicht als zugrunde liegender Typ verwendet werden. Eine Enum-Deklaration, der nicht explizit einen zugrunde liegenden Typ deklariert, hat einen zugrunde liegenden Typ `int`.

Im Beispiel

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

deklariert eine Enumeration mit zugrunde liegender Typ `long`. Ein Entwickler kann auswählen, einen zugrunde liegenden Typ verwenden `long`, wie im Beispiel verwenden, um die Verwendung von Werten zu ermöglichen, die im Bereich von `long` , aber nicht in den Bereich der `int`, oder klicken Sie auf diese Option für die Zukunft zu erhalten.

## <a name="enum-modifiers"></a>Enum-Modifizierer

Ein *Enum_declaration* kann optional eine Sequenz von Enum-Modifizierer enthalten:

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

Es ist ein Fehler während der Kompilierung für den gleichen Modifizierer für mehrere Male in einer Enum-Deklaration angezeigt werden.

Der Modifizierer, der eine Enum-Deklaration haben dieselbe Bedeutung wie die einer Klassendeklaration ([Klasse Modifizierer](classes.md#class-modifiers)). Beachten Sie jedoch, die die `abstract` und `sealed` Modifizierer dürfen nicht in eine Enum-Deklaration. Enumerationen darf nicht abstrakt sein und lassen sich nicht auf die Ableitung.

## <a name="enum-members"></a>Mitglieder aufzählen

Der Hauptteil der Deklaration eines Enum-Typs definiert NULL oder mehr Enum-Member, die benannten Konstanten des Enum-Typs sind. Enumerationsmember nicht, können die gleichen Namen haben.

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

Jedem Enumerationsmember ist einen konstanten Wert zugeordneten. Der Typ dieses Werts wird der zugrunde liegende Typ der entsprechenden Enumeration. Der Konstante Wert für jede Enum-Element muss im Bereich von den zugrunde liegenden Typ der Enumeration. Im Beispiel

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

führt zu einem Kompilierzeitfehler, da die Konstanten Werte `-1`, `-2`, und `-3` sind nicht im Bereich des zugrunde liegenden ganzzahligen Typs `uint`.

Mehrere Enumerationsmember darf es sich um den gleichen zugeordneten Wert weitergeben. Im Beispiel

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

Zeigt eine Enumeration in die zwei Enumerationsmember-- `Blue` und `Max` – der gleiche Wert verknüpft haben.

Der zugeordnete Wert eines Enumerationsmembers wird entweder implizit oder explizit zugewiesen. Wenn die Deklaration des Enumerationsmembers eine *Constant_expression* Initialisierer, der Wert dieses konstanten Ausdrucks, in der zugrunde liegende Typ der Enumeration implizit konvertiert wird, den zugeordneten Wert, der dem Enumerationsmember. Wenn die Deklaration des Enumerationsmembers keinen Initialisierer aufweist, wird der zugehörige Wert festgelegt, implizit, wie folgt:

*  Wenn der Enum-Element der ersten Enum-Element, das in Enum-Typs deklariert ist, ist der zugehörige Wert 0 (null).
*  Andernfalls wird der zugeordnete Wert des Enumerationsmembers abgerufen, durch den zugeordneten Wert des textlich vorausgehenden Enumerationsmembers um eins erhöhen. Dieser höhere Wert muss innerhalb des Bereichs von Werten, die durch den zugrunde liegenden Typ dargestellt werden kann, andernfalls tritt ein Fehler während der Kompilierung auf.

Im Beispiel

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

Gibt den Elementnamen Enumeration und die zugehörigen Werte. Ausgabe:

```
Red = 0
Green = 10
Blue = 11
```

aus den folgenden Gründen:

*  der Enumerationsmember `Red` erhält automatisch den Wert 0 (da es keinen Initialisierer aufweist und das erste Enum-Element ist);
*  der Enumerationsmember `Green` erhält den Wert explizit `10`;
*  und der Enumerationsmember `Blue` wird automatisch der Wert eins größer ist als das Element, das textlich vorausgehenden zugewiesen.

Der zugeordnete Wert eines Enumerationsmembers möglicherweise nicht der Fall, direkt oder indirekt, verwenden Sie den Wert der eigenen zugehörigen Enum-Element. Außer dieser Einschränkung Zirkularität können die Enum-Standardmember-Initialisierer frei auf andere Enumeration Standardmember-Initialisierer, unabhängig von deren Text Position verweisen. Innerhalb einer enumerationsmemberinitialisierer werden Werte anderer Enumerationsmember immer als müssen den Typ des ihre zugrunde liegenden Typ und behandelt, sodass Umwandlungen nicht erforderlich, beim Verweisen auf andere Enumerationsmember sind.

Im Beispiel

```csharp
enum Circular
{
    A = B,
    B
}
```

führt zu einem Kompilierzeitfehler, da die Deklarationen der `A` und `B` sind zirkulär. `A` hängt von `B` explizit und `B` hängt `A` implizit.

Enumerationsmember sind mit dem Namen und in einer Weise, die analog zu Feldern in Klassen beschränkt. Der Bereich eines Enumerationsmembers ist der Text, den enthaltenden Typ der Enumeration. Innerhalb des Bereichs können die Enum-Elementen, mit ihrem einfachen Namen verwiesen werden. Von anderem Code muss der Name des Enum-Element mit dem Namen der Enum-Typs qualifiziert werden. Enum-Mitglieder haben keiner deklarierten Zugriffsebenen – ein Enum-Element zugegriffen werden kann, wenn die enthaltende Enum-Typ zugegriffen werden kann.

## <a name="the-systemenum-type"></a>Der System.Enum-Typ

Der Typ `System.Enum` ist die abstrakte Basisklasse aller Enum-Typen (Dies ist distinct und unterscheidet sich von den zugrunde liegenden Typ des Enum-Typs), und die Mitglieder von geerbt `System.Enum` stehen in jeder Enumerationstyp. Eine Boxingkonvertierung ([Boxing-Konvertierung](types.md#boxing-conversions)) vorhanden ist, aus jeder Enumerationstyp, `System.Enum`, und einer unboxing-Konvertierung ([Unboxing-Konvertierungen](types.md#unboxing-conversions)) vorhanden ist, von `System.Enum` auf einen beliebigen Enumerationstyp.

Beachten Sie, dass `System.Enum` ist nicht selbst eine *Enum_type*. Es handelt sich vielmehr eine *Class_type* von der alle *Enum_type*s abgeleitet werden. Der Typ `System.Enum` erbt vom Typ `System.ValueType` ([die System.ValueType-Typ](types.md#the-systemvaluetype-type)), das wiederum vom Typ erbt `object`. Zur Laufzeit, ein Wert vom Typ `System.Enum` kann `null` oder ein Verweis auf einen geschachtelten Wert eines beliebigen Enumerationstyps.

## <a name="enum-values-and-operations"></a>Enum-Werte und Vorgänge

Jeder Enumerationstyp definiert einen gesonderter Typ ist. eine Enumeration der expliziten Konvertierung ([explizite Enumeration Konvertierungen](conversions.md#explicit-enumeration-conversions)) ist erforderlich, um zwischen einer Enum-Typ und einen ganzzahligen Typ oder zwischen zwei Enum-Typen zu konvertieren. Der Satz von Werten, die ein Enum-Typ übernehmen kann, ist nicht durch die Enumerationsmember beschränkt. Insbesondere ein Wert für den zugrunde liegenden Typ einer Enumeration in Enum-Typs umgewandelt werden kann, und ist ein eindeutiger gültiger Wert dieses Enum-Typs.

Enum-Member haben den Typ ihres enthaltenden Enum-Typs (mit Ausnahme der in anderen Enum-Standardmember-Initialisierer: finden Sie unter [Enumerationsmember](enums.md#enum-members)). Der Wert eines Enumerationsmembers in Enum-Typs deklariert `E` mit zugeordneten Wert `v` ist `(E)v`.

Die folgenden Operatoren verwendet werden können, auf den Werten der Enum-Typen: `==`, `!=`, `<`, `>`, `<=`, `>=`  ([Enumeration Vergleichsoperatoren](expressions.md#enumeration-comparison-operators)) , binäre `+`  ([Additionsoperator](expressions.md#addition-operator)), binäre `-`  ([Subtraktionsoperator](expressions.md#subtraction-operator)), `^`, `&` , `|`  ([Enumeration logische Operatoren](expressions.md#enumeration-logical-operators)), `~`  ([bitweiser Komplementoperator](expressions.md#bitwise-complement-operator)), `++` und `--`  ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators) und [Präfix-Inkrement und Dekrement-Operatoren](expressions.md#prefix-increment-and-decrement-operators)).

Jeder Enumerationstyp wird automatisch von der Klasse `System.Enum` (abgeleitet, die wiederum `System.ValueType` und `object`). Daher können für die Werte eines Enumerationstyps geerbter Methoden und Eigenschaften dieser Klasse verwendet werden.
