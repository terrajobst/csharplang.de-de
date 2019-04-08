---
ms.openlocfilehash: 72d17175dfb8ef284dce6cf7e5837420fa06f16a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229608"
---
# <a name="structs"></a>Strukturen

Strukturen sind ähnlich wie Klassen, Datenstrukturen dar, die Datenmember und Funktionsmember enthalten können. Im Gegensatz zu Klassen, Strukturen sind allerdings Werttypen und erfordern keine Heapzuordnung. Eine Variable eines Strukturtyps enthält die Daten der Struktur direkt, während eine Variable eines Klassentyps einen Verweis auf die Daten der letzten bekannten als Objekt enthält.

Strukturen sind besonders nützlich für kleine Datenstrukturen, die über Wertsemantik verfügen. Komplexe Zahlen, Punkte in einem Koordinatensystem oder Schlüssel-Wert-Paare im Wörterbuch sind gute Beispiele für Strukturen. Schlüssel für diese Datenstrukturen ist, dass sie einige Datenmember, das, dass keine Verwendung von Vererbung und referenzieller Identität erforderlich ist und sie problemlos implementiert werden können über Wertsemantik, in der Zuweisung des Werts anstelle des Verweises kopiert haben.

Siehe [einfache Typen](types.md#simple-types), die einfachen Typen wie z. B. von C# bereitgestellten `int`, `double`, und `bool`, tatsächlich alle Strukturtypen sind. Genau wie diese vordefinierten Typen Strukturen sind, ist es auch möglich, Strukturen und Überladen von Operatoren zum Implementieren der neuer "Grundtypen" in der C#-Sprache verwenden. Zwei Beispiele für solche Typen werden am Ende dieses Kapitels erhalten ([Struct Examples](structs.md#struct-examples)).

## <a name="struct-declarations"></a>Strukturdeklarationen

Ein *Struct_declaration* ist eine *Type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), die eine neue Struktur deklariert:

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

Ein *Struct_declaration* besteht aus einer optionalen Gruppe von *Attribute* ([Attribute](attributes.md)), gefolgt von einer optionalen Gruppe von *Struct_modifier*s ([Struktur Modifizierer](structs.md#struct-modifiers)), gefolgt von einem optionalen `partial` Modifizierer, gefolgt vom Schlüsselwort `struct` und *Bezeichner* mit dem Namen der Struktur, gefolgt von einer optionale *Type_parameter_list* Spezifikation ([Typparameter](classes.md#type-parameters)), gefolgt von einem optionalen *Struct_interfaces* Spezifikation ([Partial-Modifizierer](structs.md#partial-modifier))), gefolgt von einem optionalen *Type_parameter_constraints_clause*s-Spezifikation ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)), gefolgt von einem *Struct_body* ([strukturtext](structs.md#struct-body)), optional gefolgt durch ein Semikolon.

### <a name="struct-modifiers"></a>Struktur-Modifizierer

Ein *Struct_declaration* kann optional eine Sequenz von Struct-Modifizierer enthalten:

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

Es ist ein Fehler während der Kompilierung für den gleichen Modifizierer für mehrere Male in einer Strukturdeklaration angezeigt werden.

Die Modifizierer der einer Strukturdeklaration haben dieselbe Bedeutung wie die einer Klassendeklaration ([Klasse Deklarationen](classes.md#class-declarations)).

### <a name="partial-modifier"></a>Ein partial-Modifizierer

Die `partial` Modifizierer gibt an, dass dies *Struct_declaration* ist eine Deklaration der partiellen Typ. Mehreren partiellen Strukturdeklarationen mit demselben Namen in einer einschließenden Namespace oder Typ Deklaration kombinieren, um eine Strukturdeklaration zu bilden, im angegebenen gemäß den Regeln [partielle Typen](classes.md#partial-types).

### <a name="struct-interfaces"></a>Struktur-Schnittstellen

Herausgeberkontos ausgewiesenen Form eine Strukturdeklaration eine *Struct_interfaces* -Spezifikation, in dem Fall der Struktur wird als die angegebene Schnittstelle-Typen direkt zu implementieren.

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

Schnittstellenimplementierungen finden Sie weiter unten in [Schnittstellenimplementierungen](interfaces.md#interface-implementations).

### <a name="struct-body"></a>Strukturtext

Die *Struct_body* einer Struktur definiert die Elemente der Struktur.

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>Strukturmember

Die Member einer Struktur besteht aus der Elemente eingeführt, durch die *Struct_member_declaration*s und den Membern geerbt vom Typ `System.ValueType`.

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

Mit Ausnahme der Unterschiede in notiert [Klassen- und Strukturmember Unterschiede](structs.md#class-and-struct-differences), die Beschreibungen der Klassenmember, die im bereitgestellten [Klassenmember](classes.md#class-members) über [Iteratoren](classes.md#iterators) gelten für Struktur auch Elemente.

## <a name="class-and-struct-differences"></a>Unterschiede für Klassen- und Strukturmember

Strukturen unterscheiden sich von Klassen in mehreren wichtigen Punkten:

*  Strukturen sind Werttypen ([Wertsemantik](structs.md#value-semantics)).
*  Alle Strukturtypen erben implizit von der Klasse `System.ValueType` ([Vererbung](structs.md#inheritance)).
*  Zuweisung zu einer Variable eines Strukturtyps erstellt eine Kopie des Werts zugewiesen wird ([Zuweisung](structs.md#assignment)).
*  Der Standardwert einer Struktur ist der Wert, der erstellt werden, indem alle Werttypfelder auf ihre Standardwerte und alle Verweise auf Felder festlegen. `null` ([Standardwerte](structs.md#default-values)).
*  Boxing und unboxing-Vorgänge werden verwendet, um zwischen einem Strukturtyp zu konvertieren und `object` ([Boxing und unboxing](structs.md#boxing-and-unboxing)).
*  Die Bedeutung der `this` unterscheidet sich für Strukturen ([diesen Zugriff](expressions.md#this-access)).
*  Instanz Steuerelementfeld-Deklarationen für eine Struktur dürfen nicht Variableninitialisierern enthalten ([Feld Initialisierer](structs.md#field-initializers)).
*  Eine Struktur ist nicht zulässig, einen parameterlosen Instanzenkonstruktor deklarieren ([Konstruktoren](structs.md#constructors)).
*  Eine Struktur ist nicht zulässig, einen Destruktor deklarieren ([Destruktoren](structs.md#destructors)).

### <a name="value-semantics"></a>Wertsemantik

Strukturen sind Werttypen ([Werttypen](types.md#value-types)) und Sie über Wertsemantik verfügen. Auf der anderen Seite sind Klassen, Verweistypen ([Verweistypen](types.md#reference-types)) und bewirken, haben Verweissemantik.

Eine Variable eines Strukturtyps enthält die Daten der Struktur direkt, während eine Variable eines Klassentyps einen Verweis auf die Daten der letzten bekannten als Objekt enthält. Wenn eine Struktur `B` enthält ein Instanzenfeld eines Typs `A` und `A` ist ein Strukturtyp, es ist ein Fehler während der Kompilierung für `A` hängt von `B` oder ein Typ erstellt, von `B`. Eine Struktur `X` ***hängt direkt von*** eine Struktur `Y` Wenn `X` enthält ein Instanzenfeld eines Typs `Y`. Dank dieser der vollständige Satz von Strukturen, die von der eine Struktur abhängig ist, wird den transitiven Abschluss von der ***hängt direkt von*** Beziehung.  Beispiel:
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
ist ein Fehler, da `Node` ein Instanzenfeld des eigenen Typs enthält.  Ein weiteres Beispiel
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
ist ein Fehler, da alle Typen `A`, `B`, und `C` voneinander abhängig.

Mit Klassen ist es möglich, dass zwei Variablen auf dasselbe Objekt verweisen, und so können Vorgänge auf eine Variable auf das Objekt, das die andere Variable verweist auswirken. Mit Strukturen besitzt jede Variable eine eigene Kopie der Daten (außer im Fall von `ref` und `out` Parametervariablen), und es ist nicht möglich, für Vorgänge auf einem anderen zu beeinflussen. Darüber hinaus da Strukturen keine Verweistypen sind, ist es nicht möglich, dass die Werte eines Strukturtyps sein `null`.

Im Falle folgender Deklaration
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
das Codefragment
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
Gibt den Wert `10`. Die Zuweisung von `a` zu `b` erstellt eine Kopie des Werts und `b` ist daher nicht betroffen, durch die Zuweisung zu `a.x`. Hatte `Point` wurde stattdessen als eine Klasse deklariert, ist die Ausgabe wäre `100` da `a` und `b` würde das gleiche Objekt verweisen.

### <a name="inheritance"></a>Vererbung

Alle Strukturtypen erben implizit von der Klasse `System.ValueType`, das wiederum erbt von Klasse `object`. Eine Strukturdeklaration kann eine Liste der implementierten Schnittstellen angeben, aber es ist nicht möglich, für die Strukturdeklaration einer, die eine Basisklasse angeben.

Strukturtypen sind nicht abstrakt und sind immer implizit versiegelt. Die `abstract` und `sealed` Modifizierer sind in einer Strukturdeklaration daher nicht zulässig.

Da Vererbung für Strukturen nicht unterstützt wird, nicht die deklarierte Zugriffsart eines Members Struktur `protected` oder `protected internal`.

Funktionsmember in einer Struktur nicht `abstract` oder `virtual`, und die `override` Modifizierer kann nur für das Überschreiben von geerbten Methoden `System.ValueType`.

### <a name="assignment"></a>Zuweisung

Zuweisung zu einer Variable eines Strukturtyps erstellt eine Kopie der Wert zugewiesen wird. Dies unterscheidet sich von Zuweisung einer Variable eines Klassentyps, die den Verweis aber nicht das Objekt identifiziert, die durch den Verweis kopiert.

Ähnlich wie bei einer Zuweisung ist, wenn eine Struktur als ein Value-Parameter übergeben oder als Ergebnis ein Funktionsmember zurückgegeben wird, wird eine Kopie der Struktur erstellt. Eine Struktur kann übergeben werden, durch Verweis auf ein Funktionsmember mithilfe einer `ref` oder `out` Parameter.

Wenn eine Eigenschaft oder der Indexer einer Struktur das Ziel einer Zuweisung ist, muss der Instanzausdruck, der mit der Eigenschaft oder der Indexer verknüpft als Variable klassifiziert werden. Wenn der Instanzausdruck als Wert klassifiziert ist, tritt ein Fehler während der Kompilierung. Dies wird ausführlicher im beschrieben [einfache Zuweisung](expressions.md#simple-assignment).

### <a name="default-values"></a>Standardwerte

Siehe [Standardwerte](variables.md#default-values), verschiedene Arten von Variablen werden automatisch auf ihren Standardwert initialisiert, wenn sie erstellt werden. Für die Variablen der Klasse und anderen Verweistypen, der Standardwert ist `null`. Aber da Strukturen Werttypen sind, die nicht möglich `null`, der Standardwert einer Struktur ist der Wert, der erstellt werden, indem alle Werttypfelder auf ihre Standardwerte und alle Verweise auf Felder festlegen. `null`.

Verweisen auf die `Point` Struktur deklariert werden, im Beispiel oben
```csharp
Point[] a = new Point[100];
```
Initialisiert alle `Point` in das Array, das den Wert ergibt, wenn die `x` und `y` Felder auf 0 (null).

Der Standardwert einer Struktur entspricht dem Wert, der vom Standardkonstruktor der Struktur zurückgegeben ([Standardkonstruktoren](types.md#default-constructors)). Im Gegensatz zu einer Klasse ist eine Struktur nicht zulässig, einen parameterlosen Instanzenkonstruktor deklarieren. Stattdessen enthält jede Struktur implizit einen parameterlosen Instanzenkonstruktor die immer den Wert, die zurückgibt aus alle Werttypfelder auf ihre Standardwerte und alle Verweise auf Felder festlegen. `null`.

Strukturen sollten so entworfen werden, die Standard-Initialisierungszustand mit einem gültigen Zustand berücksichtigen. Im Beispiel
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
der Instanzkonstruktor für eine benutzerdefinierte schützt vor null-Werte nur, wenn es explizit aufgerufen wird. In Fällen, in denen eine `KeyValuePair` Variable unterliegt standardinitialisierung-Wert, der `key` und `value` Felder ist null, und die Struktur muss vorbereitet werden, um diesen Zustand zu behandeln.

### <a name="boxing-and-unboxing"></a>Boxing und Unboxing

Der Wert ein Klassentyp konvertiert werden kann, Typ `object` oder in einen Schnittstellentyp, die von der Klasse implementiert wird, indem Sie einfach zum Behandeln des Verweis als einen anderen Typ zum Zeitpunkt der Kompilierung. Ebenso wird ein Wert vom Typ `object` oder ein Wert eines Schnittstellentyps wieder in einen Klassentyp konvertiert werden kann, ohne den Verweis (aber natürlich eine Laufzeittyp-Überprüfung ist erforderlich, in diesem Fall).

Da Strukturen keine Verweistypen sind, werden diese Vorgänge für Strukturtypen anders implementiert. Wenn der Wert einen Strukturtyp in den Typ konvertiert wird `object` oder in einen Schnittstellentyp, der durch die Struktur implementiert wird, ein Boxing-Vorgang stattfindet. Ebenso, wenn ein Wert vom Typ `object` oder ein Wert eines Schnittstellentyps wieder in einen Strukturtyp konvertiert wird, ein unboxing-Vorgang stattfindet. Ein wichtiger Unterschied zu der gleichen Vorgänge für Klassentypen ist, dass das boxing und unboxing den Strukturwert entweder in oder aus der geschachtelten Instanz kopiert. Daher werden nach einem Boxing- oder unboxing-Vorgang an die nicht geschachtelten Struktur vorgenommenen Änderungen nicht in der geschachtelten Struktur wiedergegeben.

Wenn ein Strukturtyp überschreibt eine virtuelle Methode geerbt von `System.Object` (wie z. B. `Equals`, `GetHashCode`, oder `ToString`), Aufruf der virtuellen Methode durch eine Instanz des Strukturtyps verursacht keine Boxing auftreten. Dies gilt auch, wenn die Struktur, wie ein Typparameter verwendet wird und der Aufruf durch eine Instanz des Type-Parameter erfolgt. Zum Beispiel:
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

Die Ausgabe des Programms lautet:
```
1
2
3
```

Ungültiges Format für zwar `ToString` um Nebeneffekte haben, im Beispiel wird veranschaulicht, dass keine Boxing-Konvertierung für die drei Aufrufe von aufgetreten `x.ToString()`.

Auf ähnliche Weise beim boxing nie implizit ein Element in einem eingeschränkten Typparameter zugreifen auf. Nehmen wir beispielsweise an eine Schnittstelle `ICounter` enthält eine Methode `Increment` die können verwendet werden, um einen Wert zu ändern. Wenn `ICounter` dient als eine Einschränkung, die Implementierung der `Increment` Methode wird aufgerufen, mit einem Verweis auf die Variable, `Increment` für noch nie eine geschachtelte Kopie wurde aufgerufen.

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

Der erste Aufruf `Increment` ändert den Wert in der Variablen `x`. Dies entspricht nicht der zweite Aufruf von `Increment`, die geändert, dass des Wert in eine geschachtelte Kopie von `x`. Daher ist die Ausgabe des Programms:
```
0
1
1
```

Weitere Informationen zu Boxing und unboxing zu erhalten, finden Sie unter [Boxing und unboxing](types.md#boxing-and-unboxing).

### <a name="meaning-of-this"></a>Bedeutung

Innerhalb eines Instanzkonstruktors oder Funktionsmember der Member einer Klasse, Instanz `this` wird als Wert klassifiziert. Daher zwar `this` kann verwendet werden, verweisen Sie auf der Instanz für das das Funktionselement aufgerufen wurde, ist es nicht möglich, Zuweisen `this` in ein Funktionsmember der Member einer Klasse.

In einem Instanzenkonstruktor der eine Struktur `this` entspricht einer `out` Parameter des Strukturtyps und innerhalb einer Instanz Funktionsmember der Member einer Struktur, `this` entspricht einem `ref` Parameter des Strukturtyps. In beiden Fällen `this` wird als Variable klassifiziert, und es ist möglich, die gesamte Struktur ändern, für die das Funktionselement aufgerufen wurde, indem zuweisen `this` oder durch Übergabe als eine `ref` oder `out` Parameter.

### <a name="field-initializers"></a>Feldinitialisierer

Siehe [Standardwerte](structs.md#default-values), der Wert, der ergibt, wenn alle Werttypfelder auf ihre Standardwerte und alle Verweise auf Felder festlegen. der Standardwert einer Struktur besteht aus `null`. Aus diesem Grund lässt eine Struktur nicht Felddeklarationen Variableninitialisierern eingeschlossen wird. Diese Einschränkung gilt nur für Instanzfelder. Statische Felder einer Struktur Variable Initialisierer enthalten dürfen.

Im Beispiel
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
Fehler ist, da die Instanz Felddeklarationen Variableninitialisierern enthalten.

### <a name="constructors"></a>Konstruktoren

Im Gegensatz zu einer Klasse ist eine Struktur nicht zulässig, einen parameterlosen Instanzenkonstruktor deklarieren. Stattdessen enthält jede Struktur implizit einen parameterlosen Instanzenkonstruktor die immer den Wert, die zurückgibt aus alle Werttypfelder auf ihre Standardwerte und alle Verweise auf null-Feldern festlegen ([Standardkonstruktoren](types.md#default-constructors)). Eine Struktur kann Instanzkonstruktoren Parametern deklarieren. Beispiel:
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

Wenn die obige Deklaration, die Anweisungen
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
Erstellen einer `Point` mit `x` und `y` auf 0 (null) initialisiert.

Ein Strukturkonstruktor-Instanz ist nicht zulässig, einem Konstruktorinitialisierer des Formulars einschließen `base(...)`.

Wenn die Struktur Instanzkonstruktor einem Konstruktorinitialisierer angeben, nicht der `this` Variable entspricht ein `out` -Parameter des Strukturtyps und ähnelt ein `out` Parameter `this` müssen (definitiv zugewiesen werden [Definitive Zuweisung](variables.md#definite-assignment)) an jedem Standort, in den Konstruktor zurück. Wenn der Struktur-Instanzkonstruktor einem Konstruktorinitialisierer gibt an, die `this` Variable entspricht eine `ref` -Parameter des Strukturtyps und ähnelt ein `ref` -Parameter `this` gilt als definitiv zugewiesen, auf der Eintrag in den Text des Konstruktors. Beachten Sie die Instanz Konstruktorimplementierung unten:
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

Keine Instanz-Member-Funktion (einschließlich der Set-Accessoren für die Eigenschaften `X` und `Y`) aufgerufen werden kann, bis alle Felder der Struktur, die erstellt wird definitiv zugewiesen wurden. Die einzige Ausnahme umfasst automatisch implementierte Eigenschaften ([automatisch implementierten Eigenschaften](classes.md#automatically-implemented-properties)). Die Regeln für definitive Zuweisungen ([Ausdrücke für einfache Zuweisung](variables.md#simple-assignment-expressions)) schließen Sie die Zuweisung zu einer Auto-Eigenschaft eines Strukturtyps innerhalb eines Instanzkonstruktors dieses Strukturtyps speziell: eine solche Zuweisung gilt eine definitive das ausgeblendete dahinter liegende Feld der Auto-Eigenschaft zugeordnet. Daher ist Folgendes zulässig:

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a>Destruktoren

Eine Struktur ist nicht zulässig, einen Destruktor deklarieren.

### <a name="static-constructors"></a>Statische Konstruktoren

Statische Konstruktoren für Strukturen führen Sie die meisten hier dieselben Regeln wie bei Klassen. Die Ausführung eines statischen Konstruktors für einen Strukturtyp wird durch das erste der folgenden Ereignisse innerhalb einer Anwendungsdomäne ausgelöst:

*  Ein statischer Member des Strukturtyps ist auf die verwiesen wird.
*  Ein Konstruktor explizit deklarierter des Strukturtyps wird aufgerufen.

Die Erstellung von Standardwerten ([Standardwerte](structs.md#default-values)) der Struktur löst Typen keinen statischen Konstruktor. (Ein Beispiel hierfür ist der Anfangswert der Elemente in einem Array.)

## <a name="struct-examples"></a>Beispiele für Strukturen

Das folgende Beispiel zeigt zwei wichtige Beispiele für die Verwendung `struct` Typen zum Erstellen von Typen, die auf ähnliche Weise auf die vordefinierten Typen der Sprache, aber mit der geänderten Semantik verwendet werden können.

### <a name="database-integer-type"></a>Datenbank-Integer-Datentyp

Die `DBInt` -Struktur implementiert, einen ganzzahliger Typ, der den vollständigen Satz von Werten darstellen, kann die `int` -Typ sowie einen zusätzlichen Zustand, der einen unbekannten Wert angibt. Ein Typ mit folgenden Merkmalen wird häufig in Datenbanken verwendet.

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a>Datenbank-boolean-Typ

Die `DBBool` folgende Struktur implementiert einen logischen dreiwertige-Typ. Die möglichen Werte dieses Typs sind `DBBool.True`, `DBBool.False`, und `DBBool.Null`, wobei die `Null` Element gibt einen unbekannten Wert an. Diese logischen dreiwertige-Typen werden häufig in Datenbanken verwendet.

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
