---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704102"
---
# <a name="delegates"></a>Delegaten

Delegaten aktivieren Szenarien, die von anderen Sprachen C++– wie, Pascal und Modula, mit Funktions Zeigern adressiert wurden. Im C++ Gegensatz zu Funktions Zeigern sind Delegaten jedoch vollständig objektorientiert C++ , und im Gegensatz zu Zeigern auf Element Funktionen Kapseln Delegaten sowohl eine Objektinstanz als auch eine-Methode.

Eine Delegatdeklaration definiert eine Klasse, die von der-Klasse `System.Delegate` abgeleitet ist. Eine Delegatinstanz kapselt eine Aufruf Liste, bei der es sich um eine Liste von mindestens einer Methode handelt, von denen jede als Aufruf Bare Entität bezeichnet wird. Bei Instanzmethoden besteht eine Aufruf Bare Entität aus einer-Instanz und einer-Methode für diese Instanz. Bei statischen Methoden besteht eine Aufruf Bare Entität nur aus einer-Methode. Das Aufrufen einer Delegatinstanz mit einem geeigneten Satz von Argumenten bewirkt, dass alle Aufruf baren Entitäten des Delegaten mit dem angegebenen Satz von Argumenten aufgerufen werden.

Eine interessante und nützliche Eigenschaft einer Delegatinstanz besteht darin, dass die Klassen der gekapselten Methoden nicht bekannt sind. alles, was wichtig ist, ist, dass diese Methoden mit dem Typ des Delegaten kompatibel sind ([Delegatdeklarationen](delegates.md#delegate-declarations)). Dadurch eignen sich Delegaten hervorragend für den "anonymen" Aufruf.

## <a name="delegate-declarations"></a>Delegatdeklarationen

Ein *delegate_declaration* ist ein *type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), der einen neuen Delegattyp deklariert.

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

Es ist ein Kompilierzeitfehler, damit derselbe Modifizierer mehrmals in einer Delegatdeklaration angezeigt wird.

Der `new`-Modifizierer ist nur für Delegaten zulässig, die in einem anderen Typ deklariert wurden. in diesem Fall gibt er an, dass ein solcher Delegat einen geerbten Member mit demselben Namen verbirgt, wie im [neuen Modifizierer](classes.md#the-new-modifier)beschrieben.

Die Modifizierer `public`, `protected`, `internal` und `private` steuern den Zugriff auf den Delegattyp. Abhängig vom Kontext, in dem die Delegatdeklaration auftritt, sind einige dieser Modifizierer möglicherweise nicht zulässig (als[Barrierefreiheit deklariert](basic-concepts.md#declared-accessibility)).

Der Typname des Delegaten ist ein *Bezeichner*.

Der optionale *formal_parameter_list* gibt die Parameter des Delegaten an, und *return_type* gibt den Rückgabetyp des Delegaten an.

Die optionalen *variant_type_parameter_list* ([Variant-Typparameter Listen](interfaces.md#variant-type-parameter-lists)) gibt die Typparameter für den Delegaten an.

Der Rückgabetyp eines Delegattyps muss entweder `void` oder Output-Safe ([Varianz Sicherheit](interfaces.md#variance-safety)) sein.

Alle formalen Parametertypen eines Delegattyps müssen Eingabe sicher sein. Außerdem müssen alle Parametertypen von `out` oder `ref` ebenfalls Ausgabe sicher sein. Beachten Sie, dass auch `out`-Parameter aufgrund einer Einschränkung der zugrunde liegenden Ausführungsplattform Eingabe sicher sein müssen.

Delegattypen C# in sind namens äquivalent, nicht strukturell äquivalent. Insbesondere werden zwei unterschiedliche Delegattypen, die über die gleichen Parameterlisten und Rückgabe Typen verfügen, als andere Delegattypen betrachtet. Instanzen von zwei eindeutigen, aber strukturell äquivalenten Delegattypen können als gleich (Delegat-[Gleichheits Operatoren](expressions.md#delegate-equality-operators)) verglichen werden.

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

Die Methoden `A.M1` und `B.M1` sind mit den Delegattypen `D1` und `D2` kompatibel, da Sie den gleichen Rückgabetyp und die gleiche Parameterliste aufweisen. Diese Delegattypen sind jedoch zwei verschiedene Typen, sodass Sie nicht austauschbar sind. Die Methoden `B.M2`, `B.M3` und `B.M4` sind nicht mit den Delegattypen `D1` und `D2` kompatibel, da Sie unterschiedliche Rückgabe Typen oder Parameterlisten aufweisen.

Wie andere generische Typdeklarationen müssen Typargumente angegeben werden, um einen konstruierten Delegattyp zu erstellen. Die Parametertypen und der Rückgabetyp eines konstruierten Delegattyps werden erstellt, indem für jeden Typparameter in der Delegatdeklaration das entsprechende Typargument des konstruierten Delegattyps ersetzt wird. Der resultierende Rückgabetyp und die Parametertypen werden verwendet, um zu bestimmen, welche Methoden mit einem konstruierten Delegattyp kompatibel sind. Zum Beispiel:

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

Die-Methode `X.F` ist mit dem Delegattyp `Predicate<int>` kompatibel, und die-Methode `X.G` ist mit dem Delegattyp `Predicate<string>` kompatibel.

Die einzige Möglichkeit, einen Delegattyp zu deklarieren, ist über eine *delegate_declaration*. Ein Delegattyp ist ein Klassentyp, der von `System.Delegate` abgeleitet ist. Delegattypen sind implizit `sealed`, daher ist es nicht zulässig, einen beliebigen Typ von einem Delegattyp abzuleiten. Es ist auch nicht zulässig, einen nicht-delegatklassentyp von `System.Delegate` abzuleiten. Beachten Sie, dass "`System.Delegate`" nicht selbst ein Delegattyp ist. Dabei handelt es sich um einen Klassentyp, von dem alle Delegattypen abgeleitet werden.

C#bietet eine spezielle Syntax für die Instanziierung und den Aufruf von Delegaten. Mit Ausnahme der Instanziierung kann jeder Vorgang, der auf eine Klasse oder eine Klasseninstanz angewendet werden kann, auch auf eine Delegatklasse bzw.-Instanz angewendet werden. Insbesondere ist es möglich, über die übliche Syntax des Element Zugriffs auf Member des `System.Delegate`-Typs zuzugreifen.

Der Satz von Methoden, die von einer Delegatinstanz gekapselt werden, wird als Aufruf Liste bezeichnet. Wenn eine Delegatinstanz ([delegatkompatibilität](delegates.md#delegate-compatibility)) aus einer einzelnen Methode erstellt wird, kapselt Sie diese Methode, und die Aufruf Liste enthält nur einen Eintrag. Wenn jedoch zwei nicht-NULL-Delegatinstanzen kombiniert werden, werden die Aufruf Listen verkettet, und zwar im Order Left-Operand, Right Operand--, um eine neue Aufruf Liste zu erstellen, die zwei oder mehr Einträge enthält.

Delegaten werden mithilfe der binären `+`-Operatoren (Additions[Operator](expressions.md#addition-operator)) und `+=`-Operatoren ([Verbund Zuweisung](expressions.md#compound-assignment)) kombiniert. Ein Delegat kann aus einer Kombination von Delegaten entfernt werden, wobei der binäre `-`-[Operator (Subtraktions Operator](expressions.md#subtraction-operator)) und `-=`-Operatoren ([Verbund Zuweisung](expressions.md#compound-assignment)) verwendet wird. Delegaten können auf Gleichheit verglichen werden ([delegatgleichheits-Operatoren](expressions.md#delegate-equality-operators)).

Im folgenden Beispiel wird die Instanziierung einer Reihe von Delegaten und ihre entsprechenden Aufruf Listen veranschaulicht:

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

Wenn `cd1` und `cd2` instanziiert werden, Kapseln Sie jeweils eine Methode. Wenn `cd3` instanziiert wird, verfügt sie über eine Aufruf Liste mit zwei Methoden `M1` und `M2` in dieser Reihenfolge. die Aufruf Liste von `cd4` enthält `M1`, `M2` und `M1` in dieser Reihenfolge. Zum Schluss enthält die Aufruf Liste von `cd5` `M1`, `M2`, `M1`, `M1` und `M2` in dieser Reihenfolge. Weitere Beispiele für das kombinieren (und entfernen) von Delegaten finden Sie unter [Delegataufruf](delegates.md#delegate-invocation).

## <a name="delegate-compatibility"></a>Delegatkompatibilität

Eine Methode oder ein Delegat `M` ist mit einem Delegattyp ***kompatibel*** `D`, wenn Folgendes zutrifft:

*  `D` und `M` haben die gleiche Anzahl von Parametern, und jeder Parameter in `D` hat dieselben `ref`-oder `out`-Modifizierer wie der entsprechende Parameter in `M`.
*  Für jeden value-Parameter (ein Parameter ohne `ref`-oder `out`-Modifizierer) ist eine Identitäts Konvertierung ([Identitäts Konvertierung](conversions.md#identity-conversion)) oder eine implizite Verweis Konvertierung ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions)) vom Parametertyp in `D` zum entsprechender Parametertyp in `M`.
*  Für jeden `ref`-oder `out`-Parameter ist der Parametertyp in `D` mit dem Parametertyp in `M` identisch.
*  Eine Identität oder implizite Verweis Konvertierung ist vom Rückgabetyp `M` bis zum Rückgabetyp `D` vorhanden.

## <a name="delegate-instantiation"></a>Delegatinstanziierung

Eine Instanz eines Delegaten wird durch einen *delegate_creation_expression* ([delegaterstellungs-Ausdruck](expressions.md#delegate-creation-expressions)) oder eine Konvertierung in einen Delegattyp erstellt. Die neu erstellte Delegatinstanz verweist dann auf eine der folgenden Optionen:

*  Die statische Methode, auf die in *delegate_creation_expression*verwiesen wird, oder
*  Das Zielobjekt (das nicht `null` sein kann) und die Instanzmethode, auf die in *delegate_creation_expression*verwiesen wird, oder
*  Ein weiterer Delegat.

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

Nach der instanziierten werden Delegatinstanzen immer auf das gleiche Zielobjekt und die gleiche Methode verwiesen. Beachten Sie Folgendes: Wenn zwei Delegaten kombiniert werden oder eine aus einer anderen entfernt wird, ergibt sich ein neuer Delegat mit einer eigenen Aufruf Liste. die Aufruf Listen der zusammengesetzten oder entfernten Delegaten bleiben unverändert.

## <a name="delegate-invocation"></a>Delegataufruf

C#bietet eine spezielle Syntax zum Aufrufen eines Delegaten. Wenn eine Delegatinstanz ungleich NULL, deren Aufruf Liste einen Eintrag enthält, aufgerufen wird, ruft Sie die eine Methode mit denselben Argumenten auf, die Sie angegeben hat, und gibt denselben Wert zurück wie die Methode, auf die verwiesen wird. (Weitere Informationen zum Delegataufruf finden Sie unter [delegieren](expressions.md#delegate-invocations) von aufrufen.) Wenn während des Aufrufs eines solchen Delegaten eine Ausnahme auftritt und diese Ausnahme in der aufgerufenen Methode nicht abgefangen wird, wird die Suche nach einer Ausnahme catch-Klausel in der Methode fortgesetzt, die den Delegaten aufgerufen hat, als wäre diese Methode direkt als Methode, auf die dieser Delegat verwiesen hat.

Der Aufruf einer Delegatinstanz, deren Aufruf Liste mehrere Einträge enthält, verläuft durch das synchrone Aufrufen der Methoden in der Aufruf Liste. Jede Methode, die so aufgerufen wird, wird der gleiche Satz von Argumenten übergeben, die an die Delegatinstanz übergeben wurden. Wenn ein solcher Delegataufruf Verweis Parameter ([Verweis Parameter](classes.md#reference-parameters)) enthält, erfolgt jeder Methodenaufruf mit einem Verweis auf die gleiche Variable. Änderungen an dieser Variablen durch eine Methode in der Aufruf Liste werden für Methoden weiter unten in der Aufruf Liste angezeigt. Wenn der Delegataufruf Ausgabeparameter oder einen Rückgabewert enthält, erfolgt der endgültige Wert aus dem Aufruf des letzten Delegaten in der Liste.

Wenn bei der Verarbeitung des Aufrufs eines solchen Delegaten eine Ausnahme auftritt und diese Ausnahme innerhalb der aufgerufenen Methode nicht abgefangen wird, wird die Suche nach einer Ausnahme catch-Klausel in der Methode fortgesetzt, die den Delegaten aufgerufen hat, und alle Methoden weiter unten die Aufruf Liste wird nicht aufgerufen.

Der Versuch, eine Delegatinstanz aufzurufen, deren Wert NULL ist, führt zu einer Ausnahme vom Typ `System.NullReferenceException`.

Im folgenden Beispiel wird gezeigt, wie Delegaten instanziiert, kombiniert, entfernt und aufgerufen werden:

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

Wie in der-Anweisung `cd3 += cd1;` gezeigt, kann ein Delegat mehrmals in einer Aufruf Liste vorhanden sein. In diesem Fall wird Sie nur einmal pro vorkommen aufgerufen. In einer Aufruf Liste wie diesem wird beim Entfernen dieses Delegaten das letzte Vorkommen in der Aufruf Liste tatsächlich entfernt.

Unmittelbar vor der Ausführung der abschließenden Anweisung `cd3 -= cd1;` verweist der Delegat `cd3` auf eine leere Aufruf Liste. Der Versuch, einen Delegaten aus einer leeren Liste zu entfernen (oder um einen nicht vorhandenen Delegaten aus einer nicht leeren Liste zu entfernen), ist kein Fehler.

Die erstellte Ausgabe lautet wie folgt:

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
