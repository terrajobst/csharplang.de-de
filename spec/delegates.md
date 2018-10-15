# <a name="delegates"></a>Delegaten

Delegaten ermöglichen von Szenarien, die von anderen Sprachen, z. B. C++, Pascal-Schreibweise und Modula – mit Funktionszeigern behoben haben. Im Gegensatz zu C++-Funktionszeigern allerdings Delegaten sind vollständig objektorientiert, und im Gegensatz zu C++-Zeiger auf Member-Funktionen Delegaten sowohl eine Objektinstanz und eine Methode kapseln.

Definiert eine Klasse, die von der Klasse abgeleitet ist, eine Delegatdeklaration `System.Delegate`. Eine Delegatinstanz kapselt eine Aufrufliste, wird eine Liste von ein oder mehrere Methoden, von die jedes als eine aufrufbare Entität bezeichnet wird. Beispielsweise besteht eine aufrufbare Entität aus einer Instanz und eine Methode für diese Instanz. Bei statischen Methoden besteht aus nur eine Methode eine aufrufbare Entität. Die Instanz eines Delegaten mit einer entsprechenden Satz von Argumenten aufrufen bewirkt, dass jede aufrufbare Entitäten des Delegaten ab, mit dem angegebenen Satz von Argumenten aufgerufen werden.

Eine interessante und nützliche Eigenschaft einer Delegatinstanz ist, dass es nicht kennt oder Sie die Klassen der Methoden, die diese kapselt kümmern. wichtig ist, dass diese Methoden kompatibel ([delegieren Deklarationen](delegates.md#delegate-declarations)) mit dem Typ des Delegaten. Auf diese Weise Delegaten bestens geeignet für den Aufruf von "Anonym".

## <a name="delegate-declarations"></a>Delegatdeklarationen

Ein *Delegate_declaration* ist eine *Type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), einen neuer Delegattyp deklariert.

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

Es ist ein Fehler während der Kompilierung für den gleichen Modifizierer für mehrere Male in einer Delegatdeklaration angezeigt werden.

Die `new` Modifizierer ist nur zulässig, auf den Delegaten in einem anderen Typ deklariert, in diesem Fall wird, einen solchen Delegaten Blendet einen geerbten Member mit demselben Namen, wie in beschrieben [der new-Modifizierer](classes.md#the-new-modifier).

Die `public`, `protected`, `internal`, und `private` Modifizierern steuern den Zugriff der Delegattyp. Je nach Kontext, in dem die Delegatdeklaration auftritt, einige dieser Modifizierer kann nicht gestattet ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)).

Ist der Name des Delegattyps *Bezeichner*.

Der optionale *Formal_parameter_list* gibt die Parameter des Delegaten und *Return_type* den Rückgabetyp des Delegaten angibt.

Der optionale *Variant_type_parameter_list* ([Variante Typparameterlisten](interfaces.md#variant-type-parameter-lists)) gibt an, die Typparameter an den Delegaten selbst.

Der Rückgabetyp eines Delegattyps muss entweder `void`, oder Ausgabe-Safe ([Varianz Sicherheit](interfaces.md#variance-safety)).

Alle Typen der formalen Parameter einen Delegattyp aufweisen müssen Eingabe threadsicher sein. Darüber hinaus eine `out` oder `ref` Parametertypen müssen auch Ausgabe threadsicher sein. Beachten Sie, dass sogar `out` Parameter sind erforderlich, um die Eingabe-Safe, aufgrund einer Einschränkung von der zugrunde liegenden Ausführungsplattform zu werden.

Delegattypen in C# geschrieben sind nennen, entspricht, nicht strukturell äquivalent. Insbesondere zwei unterschiedliche Delegattypen, die den gleichen Parameter enthält und Zurückgeben des Typs gelten unterschiedliche Delegattypen. Allerdings können Instanzen von zwei unterschiedliche, jedoch strukturell Äquivalent Delegattypen als gleichwertig zu vergleichen ([delegieren Gleichheitsoperatoren](expressions.md#delegate-equality-operators)).

Zum Beispiel:

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

Die Methoden `A.M1` und `B.M1 `sind kompatibel mit sowohl die Delegattypen `D1` und `D2` , da sie haben dasselbe zurückzugeben und die Parameterliste; jedoch diese Delegattypen zwei unterschiedliche Typen sind, sodass sie nicht sind austauschbar. Die Methoden `B.M2`, `B.M3`, und `B.M4` sind nicht kompatibel mit Delegattypen `D1` und `D2`, da sie unterschiedliche Rückgabetypen oder Parameterlisten aufweisen.

Wie andere generische Typdeklarationen müssen Typargumente angegeben werden, um einen Typ des erstellten Delegaten zu erstellen. Ersetzen der für jeden Typparameter in der Delegatdeklaration überein, das entsprechende Typargument der Typ des erstellten Delegaten werden die Parameter und Rückgabetypen eines Typs des erstellten Delegaten erstellt. Die resultierende Rückgabe- und Parametertypen werden verwendet, bei der Bestimmung, welche Methoden mit einem Typ des erstellten Delegaten kompatibel sind. Zum Beispiel:

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

Die Methode `X.F` ist kompatibel mit dem Delegattyp `Predicate<int>` und die Methode `X.G` ist kompatibel mit dem Delegattyp `Predicate<string>` .

Die einzige Möglichkeit, einen Delegattyp deklariert wird, über eine *Delegate_declaration*. Ein Delegattyp ist ein Klassentyp, das von abgeleitet ist `System.Delegate`. Delegattypen sind implizit `sealed`, sodass es nicht zulässig, einen Delegattyp einen beliebigen Typ abgeleitet ist. Es ist auch nicht zulässig, einen Typ "nicht-Delegate"-Klasse aus abzuleiten `System.Delegate`. Beachten Sie, dass `System.Delegate` ist selbst ein Delegattyp, sondern ein Klassentyp, der von der alle Delegattypen abgeleitet werden.

C# und bietet Instanziierung und den Aufruf spezielle Syntax für Delegaten. Mit Ausnahme der Instanziierung kann alle Vorgänge, die auf eine Klasse oder Instanz der Klasse angewendet werden kann auch auf eine "Delegate"-Klasse oder die Instanz ist, bzw. angewendet werden. Insbesondere kann den Zugriff auf Member, der die `System.Delegate` Typ über die üblichen Member-Access-Syntax.

Der Satz von Methoden, die von einer Delegatinstanz gekapselt wird eine Aufrufliste aufgerufen. Wenn eine Delegatinstanz erstellt wird ([delegieren Kompatibilität](delegates.md#delegate-compatibility)) aus einer einzelnen Methode, diese Methode kapselt, und die Aufrufliste nur einen Eintrag enthält. Jedoch, wenn zwei Delegatinstanzen von nicht-Null-kombiniert werden, deren Aufruflisten verkettet werden in der Reihenfolge, Linker Operand rechten Operanden –, um eine neue Aufrufliste zu bilden, die zwei oder mehr Einträge enthält:.

Delegaten kombiniert werden, verwenden die Binärdatei `+` ([Additionsoperator](expressions.md#addition-operator)) und `+=` Operatoren ([Verbundzuweisung](expressions.md#compound-assignment)). Ein Delegat kann entfernt werden, aus einer Kombination von Delegaten, mit der Binärdatei `-` ([Subtraktionsoperator](expressions.md#subtraction-operator)) und `-=` Operatoren ([Verbundzuweisung](expressions.md#compound-assignment)). Delegaten, die auf Gleichheit verglichen werden können ([delegieren Gleichheitsoperatoren](expressions.md#delegate-equality-operators)).

Das folgende Beispiel zeigt die Instanziierung einer Reihe von Delegaten, und ihre entsprechenden Aufruf werden aufgeführt:

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

Wenn `cd1` und `cd2` werden instanziiert, jeweils eine Methode kapseln. Wenn `cd3` wird instanziiert, er verfügt über eine Aufrufliste der beiden Methoden, `M1` und `M2`in dieser Reihenfolge. `cd4`die Aufrufliste enthält `M1`, `M2`, und `M1`in dieser Reihenfolge. Zum Schluss `cd5`Aufrufliste enthält `M1`, `M2`, `M1`, `M1`, und `M2`in dieser Reihenfolge. Weitere Beispiele für die Kombination von (auch als entfernen) Delegaten, finden Sie unter [Delegataufruf](delegates.md#delegate-invocation).

## <a name="delegate-compatibility"></a>Delegieren der Kompatibilität

Eine Methode oder Delegat `M` ist ***kompatibel*** mit einem Delegattypen `D` , wenn alle der folgenden Bedingungen erfüllt sind:

*  `D` und `M` haben die gleiche Anzahl von Parametern und jeden Parameter in `D` hat die gleiche `ref` oder `out` Modifizierer wie der entsprechende Parameter im `M`.
*  Für jeden Wertparameter (einen Parameter ohne `ref` oder `out` Modifizierer), eine identitätskonvertierung ([identitätskonvertierung](conversions.md#identity-conversion)) oder implizite verweiskonvertierung ([implizite Verweis-](conversions.md#implicit-reference-conversions)) vorhanden ist, aus dem Parametertyp in `D` auf den entsprechenden Parametertyp in `M`.
*  Für jede `ref` oder `out` Typparameter, der Parameter in `D` ist identisch mit dem Parametertyp in `M`.
*  Eine Identität oder implizite verweiskonvertierung vorhanden ist, aus dem Rückgabetyp der `M` in den Rückgabetyp der `D`.

## <a name="delegate-instantiation"></a>Instanziierung von Delegaten

Eine Instanz eines Delegaten wird erstellt, indem eine *Delegate_creation_expression* ([delegieren erstellen Ausdrücke](expressions.md#delegate-creation-expressions)) oder eine Konvertierung in einen Delegattyp aufweisen. Die neu erstellte Delegatinstanz verweist dann entweder auf:

*  Die statische Methode, die auf die verwiesen wird der *Delegate_creation_expression*, oder
*  Das Zielobjekt (nicht die `null`) und der Instanz-Methode, die auf die verwiesen wird der *Delegate_creation_expression*, oder
*  Ein anderer Delegat.

Zum Beispiel:

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

Nach der Instanziierung finden Sie Delegatinstanzen immer die gleichen Zielobjekt und die Methode. Beachten Sie, wenn zwei Delegaten kombiniert werden, oder eine von einem anderen, einen neuen Delegaten Ergebnisse mit eigenen Aufrufliste entfernt wird. die Aufruflisten der Delegaten kombiniert oder entfernt bleiben unverändert.

## <a name="delegate-invocation"></a>Delegataufruf

C# bietet speziellen Syntax zum Aufrufen eines Delegaten an. Wenn eine nicht-Null-Delegatinstanz, dessen Aufrufliste einen Eintrag enthält, aufgerufen wird, ruft die Methode mit den gleichen Argumenten, es wurde angegeben, und gibt den gleichen Wert wie bezeichnet-Methode. (Finden Sie unter [delegieren Aufrufe](expressions.md#delegate-invocations) ausführliche Informationen zu Delegaten aufrufen.) Wenn eine Ausnahme tritt auf, während des Aufrufs eines solchen Delegaten, und diese Ausnahme nicht abgefangen wird, innerhalb der Methode, der aufgerufen wurde, werden die Suche nach einer Ausnahme-Catch-Klausel in der Methode, die den Delegaten aufgerufen fortgesetzt, als wäre diese Methode direkt aufgerufen hätte die Methode, die delegieren, bezeichnet.

Aufruf einer Delegatinstanz, dessen Aufrufliste mehrere Einträge enthält, die durch den Aufruf der Methoden in der Aufrufliste synchron, nacheinander wird fortgesetzt. An jede Methode wird den gleiche Satz von Argumenten, wie die Delegatinstanz zugewiesen wurde übergeben. Wenn ein Delegataufruf Verweisparameter enthält ([Verweisparameter](classes.md#reference-parameters)), jeden Methodenaufruf erfolgt durch einen Verweis auf dieselbe Variable; Änderungen an die Variable einer Methode in der Aufrufliste für die folgenden Methoden der Aufrufliste angezeigt. Wenn der Delegataufruf Output-Parameter oder Rückgabewerte enthält, wird der endgültige Wert über den Aufruf des letzten Delegaten in der Liste stammen.

Wenn eine Ausnahme tritt auf, während der Verarbeitung des Aufrufs eines solchen Delegaten, und diese Ausnahme wird nicht innerhalb der Methode, die aufgerufen wurde abgefangen, die Suche nach einer Ausnahme-Catch-Klausel wird weiterhin in der Methode, die der Delegat aufgerufen, und alle Methoden weiter unten die Liste der Aufrufe werden nicht aufgerufen.

Es wird versucht, eine Delegatinstanz aufrufen, deren Wert null führt zu einer Ausnahme vom Typ ist `System.NullReferenceException`.

Das folgende Beispiel zeigt, wie Sie instanziieren, zu kombinieren, entfernen und Aufrufen von Delegaten:

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

Wie in der Anweisung `cd3 += cd1;`, ein Delegat kann vorhanden sein in einer Aufrufliste mehrere Male. In diesem Fall ist es einfach einmal pro Vorkommen aufgerufen. In einer Aufrufliste, wie diese Wenn dieses Delegaten entfernt wird, wird das letzte Vorkommen der Aufrufliste entfernt.

Unmittelbar vor der Ausführung der letzten Anweisung `cd3 -= cd1;`, den Delegaten `cd3` bezieht sich auf eine leere Aufrufliste. Es wird versucht, einen Delegaten aus einer leeren Liste zu entfernen (oder um einen nicht vorhandenen Delegaten aus eine nicht leere Liste zu entfernen) ist kein Fehler.

Die Ausgabe erzeugt wird:

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
