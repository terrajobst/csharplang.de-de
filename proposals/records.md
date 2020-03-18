---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "79483877"
---
# <a name="records"></a>Datensätze

* [x] vorgeschlagen
* [] Prototyp: [Fertig](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME) stellen
* [] Implementierung: [in](https://github.com/dotnet/roslyn/BRANCH_NAME) Bearbeitung
* [] Spezifikation: umschlossene Spezifikation

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Datensätze sind ein neues, vereinfachtes deklarationsformular für C# Klassen-und Strukturtypen, die die Vorteile einer Reihe einfacherer Features kombinieren. Wir beschreiben die neuen Features (Aufrufer-Empfänger Parameter und *with-Expression*s), geben die Syntax und die Semantik für Daten Satz Deklarationen an und geben dann einige Beispiele an.


## <a name="motivation"></a>Motivation
[motivation]: #motivation

Eine große Anzahl von Typdeklarationen C# in ist wenig mehr als Aggregat Auflistungen typisierter Daten. Leider erfordert das Deklarieren solcher Typen einen großen Teil des Code Bausteine. *Datensätze* bieten einen Mechanismus zum Deklarieren eines Datentyps, indem die Elemente des Aggregats zusammen mit zusätzlichem Code oder Abweichungen von der üblichen Bausteine, sofern vorhanden, beschrieben werden.

Siehe [Beispiele](#examples)weiter unten.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a>Parameter des Aufrufers-Empfängers

Derzeit muss das *default-Argument* eines Methoden Parameters lauten. 
- ein *konstanter Ausdruck*. noch
- ein Ausdruck der Form `new S()`, wobei `S` ein Werttyp ist. noch
- ein Ausdruck der Form `default(S)`, wobei `S` ein Werttyp ist.

Dies wird erweitert, um Folgendes hinzuzufügen:
- ein Ausdruck der Form `this.Identifier`

Dieses neue Formular wird als ' *Caller-Receiver-Standardargument*' bezeichnet und ist nur zulässig, wenn alle der folgenden Bedingungen erfüllt sind.
- Die Methode, in der Sie angezeigt wird, ist eine Instanzmethode. immer
- Der Ausdruck `this.Identifier` an einen Instanzmember des einschließenden Typs gebunden, bei dem es sich entweder um ein Feld oder eine Eigenschaft handeln muss. immer
- Der Member, an den er gebunden wird (und der `get` Accessor, wenn er eine Eigenschaft ist), ist zumindest so verfügbar wie die-Methode. immer
- Der `this.Identifier` Typ kann implizit durch eine Identität oder eine Konvertierung in den Typ des Parameters konvertiert werden (Dies ist eine vorhandene Einschränkung für *default-Argument*).

Wenn ein Argument bei einem Aufruf eines Funktionsmembers für einen entsprechenden optionalen Parameter mit einem *Caller-Receiver-default-Argument*ausgelassen wird, wird der Wert des Empfänger Members implizit übergeben. 

> **Entwurfs Hinweise**: der Hauptgrund für den Parameter "Caller-Receiver" ist die Unterstützung von *with-Expression*. Die Idee ist, dass Sie eine Methode wie diese deklarieren können.
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> und dann wie folgt verwenden
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> Zum Erstellen einer neuen `Point` wie eine vorhandene `Point`, aber mit dem Wert `X` geändert.
> 
> Es ist eine offene Frage, ob die syntaktische Form des *with-Ausdrucks* hinzugefügt werden soll, sobald wir die Unterstützung für Aufrufer-Empfänger Parameter haben. Daher ist es möglich, dies anstelle von *with-Expression* *anstelle von* *zu* verwenden.

- [] **Problem öffnen**: wie hoch ist die Reihenfolge, in der das *Standardargument des Aufrufers-Empfängers* in Bezug auf andere Argumente ausgewertet wird? Sollten wir sagen, dass es nicht angegeben ist?

### <a name="with-expressions"></a>with-Ausdrücke

Ein neues Ausdrucks Formular wird vorgeschlagen:

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

Der Token-`with` ist ein neues Kontext abhängiger Schlüsselwort.

Jeder *Bezeichner* auf der linken Seite eines *with_initializer* muss an ein barrierefreies Instanzfeld oder eine Eigenschaft des Typs der *primary_expression* des *with_expression*gebunden werden. Es gibt möglicherweise keinen doppelten Namen zwischen diesen bezeichlern einer angegebenen *with_expression*.

Eine *with_expression* des Formulars.

> *E1* `with` `{` *Bezeichner* = *E2*... `}`

wird als Aufruf des Formulars behandelt.

> *E1*`.With(`*Bezeichner2*`:` *E2*,...`)`

Dabei wird für jede Methode mit dem Namen `With`, die ein Barrierefreiheits-Instanzmember von *E1*ist, *Bezeichner2* als Name des ersten Parameters in dieser Methode ausgewählt, der über einen Caller-Receiver-Parameter verfügt *, bei dem es sich um denselben*Member wie das Instanzfeld oder die Eigenschaft handelt Wenn kein solcher Parameter gefunden werden kann, wird die Methode nicht berücksichtigt. Die aufzurufende Methode wird bei der Überladungs Auflösung aus den verbleibenden Kandidaten ausgewählt.

> **Entwurfs Hinweise**: bei den Parametern für den aufrufenden Empfänger sind viele der Vorteile von *with-Expression* ohne dieses spezielle Syntax Formular verfügbar. Daher wird in Erwägung gezogen, ob es erforderlich ist. Der Hauptvorteil besteht darin, dass ein Programm in Bezug auf die Namen von Feldern und Eigenschaften und nicht in Bezug auf die Namen von Parametern programmiert werden kann. Auf diese Weise verbessern wir sowohl die Lesbarkeit als auch die Qualität der Tools (z. b. Gehe zu Definition für den Bezeichner einer *with_expression* die anstelle eines Methoden Parameters zur-Eigenschaft navigieren würde).

- [] **Problem öffnen**: Diese Beschreibung muss geändert werden, um Erweiterungs Methoden zu unterstützen.

### <a name="pattern-matching"></a>Mustervergleich

Eine Angabe `Deconstruct` und der zugehörigen Beziehung zum Muster Vergleich finden Sie in der [Muster Vergleichs Spezifikation](csharp-8.0/patterns.md#positional-pattern) .

> **Entwurfs Hinweise**: durch die vom Compiler generierte `Deconstruct` wie hier angegeben, und die Spezifikation für den Musterabgleich, eine Daten Satz Deklaration
> ```cs
> public class Point(int X, int Y);
> ```
> unterstützt die Zuordnung von Positions Mustern wie folgt:
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a>Daten Satz Typen Deklarationen

Die Syntax für eine `class`-oder `struct` Deklaration wird erweitert, um Wert Parameter zu unterstützen. die Parameter werden zu Eigenschaften des Typs:

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> **Entwurfs Hinweise**: Da Daten Satz Typen häufig nützlich sind, ohne dass explizit in einem Klassen Körper deklarierte Elemente erforderlich sind, ändern wir die Syntax der Deklaration, damit ein Text einfach ein Semikolon sein kann.

Eine Klasse (Struktur), die mit den *Datensatz-Parametern* deklariert wurde, wird als *Daten Satz Klasse* (Daten Satz*Struktur*) bezeichnet, von denen jeder ein *Daten Satz Typ*ist.

- [] **Problem öffnen**: Wir müssen *primary_constructor_body* in die Grammatik einschließen, damit es in einer Daten Satz-Typdeklaration vorkommen kann.
- [] **Problem öffnen**: welche namens Konfliktregeln gelten für die Parameternamen? Vermutlich ist ein Konflikt mit einem Typparameter oder einem anderen *Datensatz-Parameter*nicht zulässig.
- [] **Problem öffnen**: der Bereich der Datensatz-Parameter muss angegeben werden. Wo können Sie verwendet werden? Vermutlich innerhalb von instanzfeldinitialisierern und *primary_constructor_body* mindestens.
- [] **Problem öffnen**: kann eine Daten Satz-Typdeklaration partiell sein? Wenn dies der Fall ist, müssen die Parameter für jeden Teil wiederholt werden?

#### <a name="members-of-a-record-type"></a>Mitglieder eines Daten Satz Typs

Zusätzlich zu den Membern, die im *Class-Body*deklariert sind, verfügt ein Daten Satz Typen über die folgenden zusätzlichen Member:

##### <a name="primary-constructor"></a>Primärer Konstruktor

Ein Daten Recordtyp verfügt über einen `public` Konstruktor, dessen Signatur den Wert Parametern der Typdeklaration entspricht. Dies wird als *primärer Konstruktor* für den Typ bezeichnet und bewirkt, dass der implizit deklarierte *Standardkonstruktor* unterdrückt wird.

Zur Laufzeit der primäre Konstruktor

* Initialisiert vom Compiler generierte Unterstützungs Felder für die Eigenschaften, die den Wert Parametern entsprechen (wenn diese Eigenschaften vom Compiler bereitgestellt werden). [Siehe 1.1.2](#1.1.2)); Seitdem
* führt die instanzfeldinitialisierer aus, die in der *Klasse Body*angezeigt werden. Und dann
* Ruft einen Basisklassenkonstruktor auf:
    * Wenn im *record_base_arguments*Argumente vorhanden sind, wird ein Basiskonstruktor aufgerufen, der durch die Überladungs Auflösung mit diesen Argumenten ausgewählt wird.
    * Andernfalls wird ein Basiskonstruktor ohne Argumente aufgerufen.
* führt den Text der einzelnen *primary_constructor_body*(sofern vorhanden) in der Quell Reihenfolge aus.

- [] **Problem öffnen**: Wir müssen diese Reihenfolge angeben, insbesondere in Kompilierungs Einheiten für partiale.
- [] **Problem öffnen**: Wir müssen angeben, dass jeder explizit deklarierte Konstruktor mit dem primären Konstruktor verkettet werden muss.
- [] **Problem öffnen**: sollte es zulässig sein, den Zugriffsmodifizierer für den primären Konstruktor zu ändern?
- [] **Problem öffnen**: in einer Daten Satzstruktur ist es ein Fehler, dass keine Daten Satz Parameter vorhanden sind?

##### <a name="primary-constructor-body"></a>Primärer Konstruktortext

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

Eine *primary_constructor_body* darf nur innerhalb einer Daten Satz Typen Deklaration verwendet werden. Der *Bezeichner* eines *primary_constructor_body* muss den Daten Satz Typen benennen, in dem er deklariert ist.

Der *primary_constructor_body* deklariert keinen Member eigenständig, stellt jedoch eine Möglichkeit für den Programmierer dar, Attribute für bereitzustellen und den Zugriff auf den primären Konstruktor eines Daten Satz Typs anzugeben. Außerdem kann der Programmierer zusätzlichen Code bereitstellen, der ausgeführt wird, wenn eine Instanz des Daten Satz Typs erstellt wird.

- [] **Problem öffnen**: Beachten Sie, dass ein strukturstandardkonstruktor Dies umgeht.
- [] **Problem öffnen**: Wir sollten die Ausführungsreihenfolge der Initialisierung angeben.
- [] **Problem öffnen**: sollte etwas wie ein *primary_constructor_body* (vermutlich ohne Attribute und Modifizierer) in einer nicht-Datensatz-Typdeklaration zulässig sein, und es sollte wie der Code eines instanzfeldinitialisierers behandelt werden?

##### <a name="properties"></a>Eigenschaften

Für jeden Datensatz-Parameter einer Daten Satz-Typdeklaration gibt es einen entsprechenden `public` Eigenschaftenmember, dessen Name und Typ aus der Deklaration des Wert Parameters entnommen werden. Der Name ist der *Bezeichner* der *record_property_name*, sofern vorhanden, oder der *Bezeichner* des *record_parameter* andernfalls. Wenn keine konkrete (d. h. nicht abstrakte) öffentliche Eigenschaft mit einem `get`-Accessor und mit diesem Namen und Typ explizit deklariert oder geerbt wird, wird der Compiler wie folgt erstellt:

* Für eine *Daten Satzstruktur* oder eine `sealed` *Record-Klasse*:
 * Ein `private` `readonly` Feld wird als dahinter liegendes Feld für eine `readonly` Eigenschaft erstellt. Der Wert wird während der Erstellung mit dem Wert des entsprechenden primären Konstruktorparameters initialisiert.
 * Der `get` Accessor der Eigenschaft wird implementiert, um den Wert des dahinter liegenden Felds zurückzugeben.
 * Jede "übereinstimmende" vererbte virtuelle Eigenschaft `get` Accessor wird überschrieben.

> **Entwurfs Hinweise**: anders ausgedrückt: Wenn Sie eine Basisklasse erweitern oder eine Schnittstelle implementieren, die eine öffentliche abstrakte Eigenschaft mit demselben Namen und Typ wie ein Datensatz-Parameter deklariert, wird diese Eigenschaft überschrieben oder implementiert.

- [] **Problem öffnen**: sollte es möglich sein, den Zugriffsmodifizierer für eine Eigenschaft zu ändern, wenn er explizit deklariert wird?
- [] **Problem öffnen**: sollte es möglich sein, ein Feld für eine Eigenschaft zu ersetzen?

##### <a name="object-methods"></a>Objektmethoden

Bei einer *Daten Satzstruktur* oder einer `sealed`- *Daten Satz Klasse*werden Implementierungen der Methoden `object.GetHashCode()` und `object.Equals(object)` vom Compiler erstellt, es sei denn, Sie werden vom Benutzer bereitgestellt.

- [] **Problem öffnen**: Wir sollten die Implementierung genau angeben.
- [] **Problem öffnen**: Wir sollten auch die-Schnittstelle `IEquatable<T>` für den Daten Satz Typ hinzufügen und angeben, dass Implementierungen bereitgestellt werden.
- [] **Problem öffnen**: Wir sollten auch angeben, dass alle `IEquatable<T>.Equals`implementiert werden.
- [] **Problem öffnen**: Wir sollten genau angeben, wie das Problem von gleich bei der Daten Satz Vererbung gelöst wird: insbesondere, wie Gleichheits Methoden generiert werden, sodass Sie symmetrisch, transitiv, reflexiv usw. sind.
- [] **Problem öffnen**: Es wurde vorgeschlagen, dass wir `operator ==` und `operator !=` für Daten Satz Typen implementieren.
- [] **Problem öffnen**: sollte eine Implementierung von `object.ToString`automatisch generiert werden?

##### `Deconstruct`

Ein Daten Recordtyp verfügt über eine vom Compiler generierte `public` Methode `void Deconstruct`, es sei denn, eine mit einer Signatur wird vom Benutzer bereitgestellt. Jeder Parameter ist ein `out` Parameter desselben Namens und Typs wie der entsprechende Parameter des Daten Satz Typs. Die vom Compiler bereitgestellte Implementierung dieser Methode muss jedem `out`-Parameter den Wert der entsprechenden-Eigenschaft zuweisen.

Die [Muster Vergleichs Spezifikation](csharp-8.0/patterns.md#positional-pattern) für die Semantik von `Deconstruct`finden Sie unter.

##### <a name="with-method"></a>`With` -Methode

Es sei denn, es gibt einen vom Benutzer deklarierten Member mit dem Namen `With` deklariert wurde. ein Daten Recordtyp verfügt über eine vom Compiler bereitgestellte Methode mit dem Namen `With`, deren Rückgabetyp der Datensatz selbst ist und einen Wert Parameter enthält, der den einzelnen *Datensatz-Para* Metern in derselben Reihenfolge entspricht, in der diese Parameter in der Deklaration Jeder Parameter muss über ein *Caller-Receiver-default-Argument* der entsprechenden Eigenschaft verfügen.

In einer `abstract` Record-Klasse ist die vom Compiler angegebene `With` Methode abstrakt. In einer Daten Satzstruktur oder einer versiegelten Daten Satz Klasse wird die vom Compiler bereitgestellte `With` Methode `sealed`. Andernfalls ist die vom Compiler bereitgestellte `With` Methode "virtuell", und ihre Implementierung gibt eine neue-Instanz zurück, die durch den Aufruf des primären Konstruktors mit den Parametern als Argumente erzeugt wird, um eine neue Instanz aus den Parametern zu erstellen und diese neue Instanz zurückzugeben.

- [] **Problem öffnen**: Wir sollten auch unter den Bedingungen angeben, die von uns überschrieben werden, oder die geerbten virtuellen `With` Methoden oder `With` Methoden von implementierten Schnittstellen implementieren
- [] **Problem öffnen**: Wir sollten sagen, was geschieht, wenn wir eine nicht virtuelle `With` Methode erben.

> **Entwurfs Hinweise**: Da Daten Satz Typen standardmäßig unveränderlich sind, bietet die `With`-Methode eine Möglichkeit, eine neue-Instanz zu erstellen, die mit einer vorhandenen Instanz identisch ist, jedoch mit den ausgewählten Eigenschaften mit neuen Werten. Beispiel:
> ```cs
> public class Point(int X, int Y);
> ```
> ein vom Compiler bereitgestellter Member
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> Ermöglicht eine Variable des Daten Satz Typs.
> ```cs
> var p = new Point(3, 4);
> ```
> zum Ersetzen durch eine Instanz, die eine oder mehrere Eigenschaften unterscheidet
> ```cs
>     p = p.With(X: 5);
> ```
> Dies kann auch mithilfe des *with_expression*ausgedrückt werden:
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a>Beispiele

#### <a name="compatibility-of-record-types"></a>Kompatibilität von Daten Satz Typen

Da der Programmierer der Deklaration eines Daten Satz Typs Mitglieder hinzufügen kann, ist es oft möglich, den Satz von Daten Satz Elementen zu ändern, ohne dass sich dies auf vorhandene Clients auswirkt. Beispielsweise bei einer ersten Version eines Daten Satz Typs.

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

Ein neues Element des Daten Satz Typs kann in der nächsten Revision des Typs kompatibel hinzugefügt werden, ohne die Binär-oder Quell Kompatibilität zu beeinträchtigen:

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a>Beispiel für Daten Satzstruktur

Diese Daten Satzstruktur

```cs
public struct Pair(object First, object Second);
```

wird in diesen Code übersetzt

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- [] **Problem öffnen**: sollte die Implementierung von Gleichheits (paar anderen) ein öffentliches Member des Paars sein?
- [] **Problem öffnen**: Diese Implementierung von `Equals` ist bei Vererbung nicht symmetrisch.
- [] **Problem öffnen**: sollte ein Datensatz `operator ==` und `operator !=`deklarieren?

> **Entwurfs Hinweise**: da ein Daten Recordtyp von einem anderen Typ erben kann und diese Implementierung von `Equals` in diesem Fall nicht symmetrisch wäre, ist er nicht richtig. Es wird vorgeschlagen, Gleichheit auf diese Weise zu implementieren:
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> Abgeleitete Datensätze würden `override EqualityContract`. Die weniger attraktive Alternative besteht darin, die Vererbung einzuschränken.

#### <a name="sealed-record-example"></a>Beispiel für versiegelten Datensatz

Diese versiegelte Daten Satz Klasse

```cs
public sealed class Student(string Name, decimal Gpa);
```

wird in diesen Code übersetzt

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a>Beispiel für eine abstrakte Daten Satz Klasse

Diese abstrakte Daten Satz Klasse

```cs
public abstract class Person(string Name);
```

wird in diesen Code übersetzt

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a>Kombinieren von abstrakten und versiegelten Datensätzen

Bei Angabe der abstrakten Daten Satz Klasse `Person` oberhalb dieser versiegelten Daten Satz Klasse

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

wird in diesen Code übersetzt

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Wie bei allen Sprach Features müssen wir Fragen, ob die zusätzliche Komplexität der Sprache in der zusätzlichen Klarheit für den Text der C# Programme, die von der Funktion profitieren würden, zurückgegeben wird.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Wir haben in C# 6 als *primäre Konstruktoren* hinzugefügt. Obwohl Sie die gleiche syntaktische Oberfläche wie dieser Vorschlag belegen, haben wir festgestellt, dass Sie die Vorteile der Datensätze nicht nur kurz hatten.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

Offene Fragen werden im Hauptteil des Angebots angezeigt.

## <a name="design-meetings"></a>Treffen von Besprechungen

TBD
