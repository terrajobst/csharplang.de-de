---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704028"
---
# <a name="structs"></a>Strukturen

Strukturen ähneln Klassen darin, dass Sie Datenstrukturen darstellen, die Datenmember und Funktionsmember enthalten können. Im Unterschied zu Klassen sind Strukturen jedoch Werttypen und erfordern keine Heap Zuordnung. Eine Variable eines Struktur Typs enthält direkt die Daten der Struktur, während eine Variable eines Klassen Typs einen Verweis auf die Daten enthält, wobei es sich um ein Objekt handelt, das als Objekt bezeichnet wird.

Strukturen sind besonders nützlich für kleine Datenstrukturen, die über Wertsemantik verfügen. Komplexe Zahlen, Punkte in einem Koordinatensystem oder Schlüssel-Wert-Paare im Wörterbuch sind gute Beispiele für Strukturen. Der Schlüssel für diese Datenstrukturen besteht darin, dass Sie nur wenige Datenmember haben, dass Sie keine Vererbung oder referenzielle Identität benötigen, und dass Sie mithilfe der Wert Semantik, bei der die Zuweisung den Wert anstelle des Verweises kopiert, bequem implementiert werden können.

Wie in [einfachen Typen](types.md#simple-types)beschrieben, sind die einfachen Typen, C#die von bereitgestellt werden, z. b. `int`, `double` und `bool`, tatsächlich alle Strukturtypen. Ebenso wie diese vordefinierten Typen Strukturen sind, ist es auch möglich, Strukturen und Operator Überladung zu verwenden, um neue "primitive" Typen in der C# Sprache zu implementieren. Am Ende dieses Kapitels ([Struktur Beispiele](structs.md#struct-examples)) werden zwei Beispiele für solche Typen angegeben.

## <a name="struct-declarations"></a>Struktur Deklarationen

Ein *struct_declaration* ist ein *type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), der eine neue Struktur deklariert:

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

Ein *struct_declaration* besteht aus einem optionalen Satz von *Attributen* ([Attributen](attributes.md)), gefolgt von einem optionalen Satz von *struct_modifier*s ([Strukturmodifizierer](structs.md#struct-modifiers)), gefolgt von einem optionalen `partial`-Modifizierer, gefolgt vom Schlüsselwort `struct` und ein *Bezeichner* , der die Struktur benennt, gefolgt von einer optionalen *type_parameter_list* -Spezifikation ([Typparameter](classes.md#type-parameters)), gefolgt von einer optionalen *struct_interfaces* -Spezifikation ([partiell -Modifizierer](structs.md#partial-modifier)), gefolgt von einer optionalen *type_parameter_constraints_clause*s-Spezifikation ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)), gefolgt von einem *struct_body* ([Struktur Text](structs.md#struct-body)), optional gefolgt von einem Semikolon.

### <a name="struct-modifiers"></a>Strukturmodifizierer

Ein *struct_declaration* kann optional eine Sequenz von strukturmodifizierermodifizierer einschließen:

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

Es ist ein Kompilierzeitfehler, damit derselbe Modifizierer mehrmals in einer Struktur Deklaration angezeigt wird.

Die Modifizierer einer Struktur Deklaration haben dieselbe Bedeutung wie die einer Klassen Deklaration ([Klassen Deklarationen](classes.md#class-declarations)).

### <a name="partial-modifier"></a>Partieller Modifizierer

Der `partial`-Modifizierer gibt an, dass dieses *struct_declaration* eine partielle Typdeklaration ist. Mehrere partielle Struktur Deklarationen mit demselben Namen innerhalb eines einschließenden Namespace oder einer Typdeklaration kombinieren eine Struktur Deklaration, die den in [partiellen Typen](classes.md#partial-types)angegebenen Regeln folgt.

### <a name="struct-interfaces"></a>Struktur Schnittstellen

Eine Struktur Deklaration kann eine *struct_interfaces* -Spezifikation enthalten. in diesem Fall wird die Struktur so bezeichnet, dass die angegebenen Schnittstellentypen direkt implementiert werden.

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

Schnittstellen Implementierungen werden in [Schnittstellen Implementierungen](interfaces.md#interface-implementations)ausführlicher erläutert.

### <a name="struct-body"></a>Struktur Text

Der *struct_body* einer Struktur definiert die Member der Struktur.

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>Strukturmember

Die Member einer Struktur bestehen aus den Membern, die von den *struct_member_declaration*s eingeführt wurden, und den Membern, die vom Typ `System.ValueType` geerbt wurden.

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

Mit Ausnahme der Unterschiede in den [Klassen-und Struktur unterschieden](structs.md#class-and-struct-differences)gelten die Beschreibungen von Klassenmembern, die in [Klassenmembern](classes.md#class-members) durch [Iteratoren](classes.md#iterators) bereitgestellt werden, auch für Strukturmember.

## <a name="class-and-struct-differences"></a>Klassen-und Strukturunterschiede

Strukturen unterscheiden sich in mehreren wichtigen Punkten von Klassen:

*  Strukturen sind Werttypen ([Wert Semantik](structs.md#value-semantics)).
*  Alle Strukturtypen erben implizit von der-Klasse `System.ValueType` ([Vererbung](structs.md#inheritance)).
*  Durch die Zuweisung zu einer Variablen eines Struktur Typs wird eine Kopie des zugewiesenen Werts ([Zuweisung](structs.md#assignment)) erstellt.
*  Der Standardwert einer Struktur ist der Wert, der erzeugt wird, indem alle Werttyp Felder auf ihren Standardwert und alle Verweistyp Felder auf `null` ([Standardwerte](structs.md#default-values)) festgelegt werden.
*  Boxing-und Unboxing-Vorgänge werden verwendet, um zwischen einem Strukturtyp und `object` ([Boxing und Unboxing](structs.md#boxing-and-unboxing)) zu konvertieren.
*  Die Bedeutung von "`this`" unterscheidet sich für Strukturen ([dieser Zugriff](expressions.md#this-access)).
*  Instanzfelddeklarationen für eine Struktur dürfen keine Variableninitialisierer ([Feldinitialisierer](structs.md#field-initializers)) enthalten.
*  Eine Struktur darf keinen Parameter losen Instanzenkonstruktor ([Konstruktoren](structs.md#constructors)) deklarieren.
*  Es ist nicht zulässig, einen destrukturtor ([destrukturtoren](structs.md#destructors)) zu deklarieren.

### <a name="value-semantics"></a>Wert Semantik

Strukturen sind Werttypen ([Werttypen](types.md#value-types)) und werden als Wert Semantik bezeichnet. Klassen hingegen sind Verweis Typen ([Verweis Typen](types.md#reference-types)) und werden als Verweis Semantik bezeichnet.

Eine Variable eines Struktur Typs enthält direkt die Daten der Struktur, während eine Variable eines Klassen Typs einen Verweis auf die Daten enthält, wobei es sich um ein Objekt handelt, das als Objekt bezeichnet wird. Wenn eine Struktur `B` ein Instanzfeld vom Typ `A` und `A` ein Strukturtyp ist, ist dies ein Kompilierzeitfehler, damit `A` von `B` oder einem von `B` erstellten Typ abhängig ist. Eine Struktur `X` ist ***direkt von*** einer Struktur abhängig `Y`, wenn `X` ein Instanzfeld vom Typ `Y` enthält. Bei dieser Definition ist der gesamte Satz von Strukturen, von denen eine Struktur abhängt, das transitiv Schließen der direkt von der Beziehung ***abhängig*** .  Beispiel:
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
ist ein Fehler, da `Node` ein Instanzfeld seines eigenen Typs enthält.  Ein weiteres Beispiel
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
ist ein Fehler, da jeder der Typen `A`, `B` und `C` voneinander abhängig ist.

Mit-Klassen können zwei Variablen auf das gleiche Objekt verweisen, und so können Vorgänge in einer Variablen das Objekt beeinflussen, auf das von der anderen Variablen verwiesen wird. Bei Strukturen verfügen die Variablen jeweils über eine eigene Kopie der Daten (außer im Fall der Parameter Variablen "`ref`" und "`out`"), und es ist nicht möglich, dass Vorgänge auf einem anderen die anderen beeinflussen. Da Strukturen keine Verweis Typen sind, ist es nicht möglich, dass die Werte eines Struktur Typs `null` sind.

Bei Angabe der Deklaration
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
das Code Fragment
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
Gibt den Wert `10` aus. Durch die Zuweisung von `a` zu `b` wird eine Kopie des Werts erstellt, und `b` ist von der Zuweisung zu `a.x` nicht betroffen. Hätte `Point` stattdessen als Klasse deklariert, würde die Ausgabe `100` lauten, da `a` und `b` auf das gleiche Objekt verweisen würden.

### <a name="inheritance"></a>Vererbung

Alle Strukturtypen erben implizit von der-Klasse `System.ValueType`, die wiederum von der-Klasse `object` erbt. Eine Struktur Deklaration kann eine Liste der implementierten Schnittstellen angeben, aber es ist nicht möglich, dass eine Struktur Deklaration eine Basisklasse angibt.

Strukturtypen sind niemals abstrakt und sind immer implizit versiegelt. Die Modifizierer "`abstract`" und "`sealed`" sind daher in einer Struktur Deklaration nicht zulässig.

Da die Vererbung für Strukturen nicht unterstützt wird, kann die deklarierte Barrierefreiheit eines Strukturmembers nicht `protected` oder `protected internal` sein.

Funktionsmember in einer Struktur können nicht `abstract` oder `virtual` sein, und der `override`-Modifizierer darf nur Methoden überschreiben, die von `System.ValueType` geerbt wurden.

### <a name="assignment"></a>Zuweisung

Durch die Zuweisung zu einer Variablen eines Struktur Typs wird eine Kopie des zugewiesenen Werts erstellt. Dies unterscheidet sich von der Zuweisung zu einer Variablen eines Klassen Typs, die den Verweis, aber nicht das durch den Verweis identifizierte Objekt kopiert.

Ähnlich wie bei einer Zuweisung wird eine Kopie der Struktur erstellt, wenn eine Struktur als Wert Parameter übergeben oder als Ergebnis eines Funktionsmembers zurückgegeben wird. Eine Struktur kann mithilfe eines `ref`-oder `out`-Parameters als Verweis an einen Funktionsmember übergeben werden.

Wenn eine Eigenschaft oder ein Indexer einer Struktur das Ziel einer Zuweisung ist, muss der Instanzausdruck, der der Eigenschaft oder dem Indexer-Zugriff zugeordnet ist, als Variable klassifiziert werden. Wenn der Instanzausdruck als Wert klassifiziert wird, tritt ein Kompilierzeitfehler auf. Dies wird in " [einfache Zuweisung](expressions.md#simple-assignment)" ausführlicher beschrieben.

### <a name="default-values"></a>Standardwerte

Wie in [Standardwerte](variables.md#default-values)beschrieben, werden bei der Erstellung automatisch verschiedene Arten von Variablen auf ihren Standardwert initialisiert. Für Variablen von Klassentypen und anderen Verweis Typen ist dieser Standardwert `null`. Da Strukturen jedoch Werttypen sind, die nicht `null` sein können, ist der Standardwert einer Struktur der Wert, der erzeugt wird, indem alle Werttyp Felder auf ihren Standardwert und alle Verweistyp Felder auf `null` festgelegt werden.

Im Beispiel wird auf die oben deklarierte `Point`-Struktur verwiesen.
```csharp
Point[] a = new Point[100];
```
Initialisiert jede `Point` im-Array mit dem Wert, der erstellt wird, indem die Felder `x` und `y` auf NULL festgelegt werden.

Der Standardwert einer Struktur entspricht dem Wert, der vom Standardkonstruktor der Struktur zurückgegeben wird ([Standardkonstruktoren](types.md#default-constructors)). Anders als bei einer Klasse ist es nicht zulässig, einen Parameter losen Instanzkonstruktor zu deklarieren. Stattdessen verfügt jede Struktur implizit über einen Parameter losen Instanzenkonstruktor, der immer den Wert zurückgibt, der sich aus dem Festlegen aller Werttyp Felder auf ihren Standardwert und alle Verweistyp Felder auf `null` ergibt.

Strukturen sollten so entworfen werden, dass der standardmäßige Initialisierungs Zustand einen gültigen Status aufweist. Im Beispiel
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
der benutzerdefinierte Instanzkonstruktor schützt nur bei NULL-Werten, wenn er explizit aufgerufen wird. In Fällen, in denen eine `KeyValuePair`-Variable der Standardwert Initialisierung unterliegt, sind die Felder `key` und `value` NULL, und die Struktur muss darauf vorbereitet sein, diesen Zustand zu verarbeiten.

### <a name="boxing-and-unboxing"></a>Boxing und Unboxing

Ein Wert eines Klassen Typs kann in den Typ `object` oder in einen Schnittstellentyp konvertiert werden, der von der-Klasse implementiert wird, indem der Verweis zum Zeitpunkt der Kompilierung als anderer Typ behandelt wird. Ebenso kann ein Wert vom Typ "`object`" oder ein Wert eines Schnittstellen Typs wieder in einen Klassentyp konvertiert werden, ohne den Verweis zu ändern (in diesem Fall ist jedoch eine Lauf Zeittyp Überprüfung erforderlich).

Da Strukturen keine Verweis Typen sind, werden diese Vorgänge für Strukturtypen unterschiedlich implementiert. Wenn ein Wert eines Strukturtyps in den Typ `object` oder in einen Schnittstellentyp konvertiert wird, der von der Struktur implementiert wird, wird ein Boxing-Vorgang durchgeführt. Wenn ein Wert des Typs `object` oder ein Wert eines Schnittstellen Typs zurück in einen Strukturtyp konvertiert wird, erfolgt auch ein Unboxing-Vorgang. Ein wichtiger Unterschied von denselben Vorgängen für Klassentypen besteht darin, dass Boxing und Unboxing den Struktur Wert entweder in die oder aus der geachtelten Instanz kopieren. Folglich werden Änderungen an der Unboxing-Struktur, die an der Unboxing-Struktur vorgenommen werden, nicht in der Struktur der Struktur spiegele widergespiegelt.

Wenn ein Strukturtyp eine virtuelle Methode überschreibt, die von `System.Object` geerbt wurde (z. b. `Equals`, `GetHashCode` oder `ToString`), bewirkt der Aufruf der virtuellen Methode über eine Instanz des Struktur Typs nicht, dass Boxing erfolgt. Dies gilt auch, wenn die Struktur als Typparameter verwendet wird und der Aufruf durch eine Instanz des Typparameter Typs erfolgt. Zum Beispiel:
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

Die Ausgabe des Programms lautet wie folgt:
```console
1
2
3
```

Obwohl es für `ToString` nicht möglich ist, Nebeneffekte zu haben, zeigt das Beispiel, dass kein Boxing für die drei Aufrufe von `x.ToString()` aufgetreten ist.

Auf ähnliche Weise tritt beim Zugriff auf ein Element in einem eingeschränkten Typparameter nie implizit ein Boxing auf. Angenommen, eine Schnittstelle `ICounter` enthält eine Methode `Increment`, die zum Ändern eines Werts verwendet werden kann. Wenn `ICounter` als Einschränkung verwendet wird, wird die Implementierung der `Increment`-Methode mit einem Verweis auf die Variable aufgerufen, für die `Increment` aufgerufen wurde, nie eine gepackte Kopie.

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

Beim ersten `Increment`-Aufrufwert wird der Wert in der Variablen `x` geändert. Dies entspricht nicht dem zweiten `Increment`-Aufrufwert, durch den der Wert in einer geboxten Kopie von `x` geändert wird. Folglich lautet die Ausgabe des Programms wie folgt:
```console
0
1
1
```

Weitere Informationen zu Boxing und Unboxing finden Sie unter [Boxing und Unboxing](types.md#boxing-and-unboxing).

### <a name="meaning-of-this"></a>Bedeutung dieses

Innerhalb eines Instanzkonstruktors oder Instanzfunktionsmembers einer Klasse wird `this` als Wert klassifiziert. Wenn `this` verwendet werden kann, um auf die Instanz zu verweisen, für die das Funktionsmember aufgerufen wurde, ist es daher nicht möglich, `this` in einem Funktionsmember einer Klasse zuzuweisen.

Innerhalb eines Instanzkonstruktors einer Struktur entspricht `this` einem `out`-Parameter des Struktur Typs, und innerhalb eines Instanzfunktionsmembers einer Struktur entspricht `this` einem `ref`-Parameter des Struktur Typs. In beiden Fällen wird `this` als Variable klassifiziert, und es ist möglich, die gesamte Struktur zu ändern, für die der Funktionsmember aufgerufen wurde, indem er `this` zugewiesen wurde oder indem er als `ref`-oder `out`-Parameter übergeben wird.

### <a name="field-initializers"></a>Feldinitialisierer

Wie in [Standardwerte](structs.md#default-values)beschrieben, besteht der Standardwert einer Struktur aus dem Wert, der sich aus dem Festlegen aller Werttyp Felder auf den Standardwert und alle Verweistyp Felder auf `null` ergibt. Aus diesem Grund lässt eine Struktur nicht zu, dass Instanzfelddeklarationen Variableninitialisierer einschließen. Diese Einschränkung gilt nur für Instanzfelder. Statische Felder einer Struktur dürfen Variableninitialisierer einschließen.

Das Beispiel
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
ist fehlerhaft, da die Instanzen Feld Deklarationen Variableninitialisierer enthalten.

### <a name="constructors"></a>Konstruktoren

Anders als bei einer Klasse ist es nicht zulässig, einen Parameter losen Instanzkonstruktor zu deklarieren. Stattdessen verfügt jede Struktur implizit über einen Parameter losen Instanzenkonstruktor, der immer den Wert zurückgibt, der sich aus dem Festlegen aller Werttyp Felder auf ihren Standardwert und alle Verweistyp Felder auf NULL ([Standardkonstruktoren](types.md#default-constructors)) ergibt. Eine Struktur kann Instanzkonstruktoren mit Parametern deklarieren. Beispiel:
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

Bei der obigen Deklaration werden die Anweisungen
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
Beide erstellen eine `Point` mit `x` und `y` mit 0 (null) initialisiert.

Ein Strukturinstanzkonstruktor darf keinen Konstruktorinitialisierer in der Form `base(...)` einschließen.

Wenn der Strukturinstanzkonstruktor keinen Konstruktorinitialisierer angibt, entspricht die `this`-Variable einem `out`-Parameter des Struktur Typs. ähnlich wie bei einem `out`-Parameter muss `this` definitiv zugewiesen werden ([definitive Zuweisung). ](variables.md#definite-assignment)) an jedem Speicherort, an dem der Konstruktor zurückgibt. Wenn der Strukturinstanzkonstruktor einen Konstruktorinitialisierer angibt, entspricht die `this`-Variable einem `ref`-Parameter des Struktur Typs. ähnlich wie bei einem `ref`-Parameter wird `this` als definitiv beim Eintrag dem Konstruktortext zugewiesen. . Beachten Sie die folgende instanzkonstruktorimplementierung:
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

Es kann keine Instanzmember-Funktion (einschließlich der Set-Accessoren für die Eigenschaften `X` und `Y`) aufgerufen werden, bis alle Felder der Struktur, die erstellt wird, definitiv zugewiesen wurden. Die einzige Ausnahme betrifft automatisch implementierte Eigenschaften ([automatisch implementierte Eigenschaften](classes.md#automatically-implemented-properties)). Die konkreten Zuweisungs Regeln ([einfache Zuweisungs Ausdrücke](variables.md#simple-assignment-expressions)) geben explizit die Zuweisung zu einer automatischen Eigenschaft eines Struktur Typs innerhalb eines Instanzkonstruktors dieses Struktur Typs aus: eine solche Zuweisung wird als eindeutige Zuweisung der ausgeblendeten dahinter liegendes Feld der Auto-Eigenschaft. Daher ist Folgendes zulässig:

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

Es ist nicht zulässig, einen Dekonstruktor zu deklarieren.

### <a name="static-constructors"></a>Statische Konstruktoren

Statische Konstruktoren für Strukturen folgen den meisten der gleichen Regeln wie für-Klassen. Die Ausführung eines statischen Konstruktors für einen Strukturtyp wird durch das erste der folgenden Ereignisse ausgelöst, die innerhalb einer Anwendungsdomäne auftreten:

*  Es wird auf einen statischen Member des Struktur Typs verwiesen.
*  Ein explizit deklarierter Konstruktor des Struktur Typs wird aufgerufen.

Das Erstellen von Standardwerten ([Standardwerte](structs.md#default-values)) von Strukturtypen löst nicht den statischen Konstruktor aus. (Ein Beispiel hierfür ist der Anfangswert von Elementen in einem Array.)

## <a name="struct-examples"></a>Struktur Beispiele

Das folgende Beispiel zeigt zwei bedeutende Beispiele für die Verwendung von `struct`-Typen, um Typen zu erstellen, die ähnlich wie die vordefinierten Typen der Sprache verwendet werden können, jedoch mit einer geänderten Semantik.

### <a name="database-integer-type"></a>Daten Bank Integertyp

Die folgende `DBInt`-Struktur implementiert einen ganzzahligen Typ, der den gesamten Satz von Werten des `int`-Typs darstellen kann, zuzüglich eines zusätzlichen Zustands, der einen unbekannten Wert angibt. Ein Typ mit diesen Merkmalen wird häufig in Datenbanken verwendet.

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

### <a name="database-boolean-type"></a>Boolescher Dateityp

In der folgenden `DBBool`-Struktur ist ein logischer Typ mit drei Werten implementiert. Mögliche Werte dieses Typs sind `DBBool.True`, `DBBool.False` und `DBBool.Null`, wobei der `Null`-Member einen unbekannten Wert angibt. Solche logischen Typen mit drei Werten werden häufig in Datenbanken verwendet.

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
