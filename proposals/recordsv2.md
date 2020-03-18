---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "79483925"
---

# <a name="records-v2"></a>Datensätze v2

In der Vergangenheit haben wir uns für Datensätze als Feature angesehen, um die Arbeit mit Daten zu ermöglichen.

"Arbeiten mit Daten" ist eine große Gruppe mit einer Reihe von Facetten, daher kann es interessant sein, jede einzelne isoliert zu betrachten. Betrachten wir nun ein Beispiel für Datensätze heute und einige Nachteile.

Beispielsweise wird ein einfacher Datensatz heute wie folgt definiert:

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

und die Verwendung würde lesen

```C#
void M()
{
    var userInfo = new UserInfo() 
    {
        Username = "andy",
        Email = "angocke@microsoft.com",
        IsAdmin = true
    };
}
```

In diesem Code gibt es bedeutende Vorteile:

1. Die Definition ist Versions robust, Eigenschaften können einfach hinzugefügt oder verschoben werden.
2. Eigenschaften können in beliebiger Reihenfolge festgelegt werden, und die Namen in der Initialisierung entsprechen immer den Accessoren.
3. Eigenschaften mit Standardwerten können einfach übersprungen werden.

Der erste Fehler besteht darin, dass die Eigenschaften nun änderbar sein müssen.

# <a name="mutability"></a>Veränderlichkeit

Wir möchten C# eine Möglichkeit bereitstellen, um einen `readonly` Member in Objektinitialisierern festzulegen.
Da einige Typen möglicherweise nicht mit dieser Initialisierung entworfen wurden, würden wir Ihnen auch gerne zustimmen.

Die vorgeschlagene Lösung ist ein neuer Modifizierer, `initonly`, der auf Eigenschaften und Felder angewendet werden kann:

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

Die CodeGen hierfür ist erstaunlich einfach: Wir legen nur das schreibgeschützte Feld fest.
Insbesondere würden die abgesenkten Eigenschaften wie folgt aussehen:

```C#
public class UserInfo
{
    private readonly string <Backing>_username;
    public string get_Username() => <Backing>_username;
    [return: modreq(initonly)]
    public void set_Username(string value) { <Backing>_username = value; }
    ...
}
```

Die CLR berücksichtigt das Festlegen von schreibgeschützten Feldern als nicht überprüfbar, aber nicht unsicher. Zur Unterstützung eines erweiterten verifizierers wird die folgende Regel vorgeschlagen: ein Schreib geschütztes Feld kann nur in `initonly` Methoden oder in einem neuen Objekt geändert werden, das sich auf dem CLR-Stapel befindet und nicht über einen Speicher-oder Methodenaufrufe veröffentlicht wurde.

Dies sollte viele der Probleme mit der Veränderlichkeit im `UserInfo` Beispiel lösen, ohne komplizierte oder nicht komplizierte Ausgabe Strategien zu erfordern. Die Unveränderlichkeit stellt jedoch ein neues Problem dar: einfaches Erstellen eines Objekts mit Änderungen.

# <a name="with-ing"></a>Mit-ung

Beim Programmieren mit Unveränderlichkeit wird das vornehmen von Änderungen an einem Objekt durch das Erstellen einer Kopie mit Änderungen vorgenommen, anstatt die Änderungen direkt auf dem Objekt vorzunehmen. Leider gibt es keine bequeme Methode, dies auch mit Daten C#Sätzen im aktuellen Stil in durchzuführen. Es wurde bereits zuvor vorgeschlagen, dass für Datensätze, die diese Funktionalität implementieren, eine Art automatisch generiertes "with"-Verfahren bereitgestellt wird. Wenn wir einen solchen Mechanismus haben, ist es wichtig, dass er mit `initonly` Mitgliedern arbeitet. Um dies zu erreichen, wird vorgeschlagen, dass wir einen `with` Ausdruck analog zu einem Objektinitialisierer hinzufügen. Die Beispiel Verwendung sieht wie folgt aus:

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

Das resultierende `newUserName` Objekt wäre eine Kopie von `userInfo`, wobei `Username` auf "angocke" festgelegt ist.
Die CodeGen im `with` Ausdruck ähnelt auch dem Objektinitialisierer: ein neues-Objekt wird erstellt, und anschließend wird der `initonly` `Username` Setter im Methoden Text aufgerufen.

Der Unterschied besteht hierbei darin, dass das neu erstellte Objekt keine einfache Erstellung eines neuen Objekts ist, sondern ein Duplikat des ursprünglichen Objekts. Um diese Funktionalität bereitzustellen, ist es erforderlich, dass das-Objekt einen "with-Konstruktor" bereitstellt, der ein doppeltes Objekt bereitstellt. Ein Beispiel `With` Konstruktors sieht wie folgt aus:

```C#
class UserInfo
{
    ...
    [WithConstructor] // placeholder syntax, up for debate
    public UserInfo With()
    {
        return new UserInfo() { Username = this.Username, Email = this.Email, IsAdmin = this.IsAdmin };
    }
}
```

Der `with` Ausdruck legt `initonly` Member genau wie der Objektinitialisierer fest. um die Überprüfung zu unterstützen, müssen Sie daher sicherstellen, dass das Objekt nicht veröffentlicht werden kann, bevor die `initonly` Elemente festgelegt werden. Um dies zu erzwingen, erzwingt das `WithConstructor`-Attribut (oder die entsprechende Syntax) eine neue Regel für die-Methode: alle Return-Anweisungen müssen direkt einen Objekt Erstellungs Ausdruck enthalten, möglicherweise mit einem Objektinitialisierer.

Wenn der `With`-Konstruktor eine Validierung erfordert, kann der Benutzer einen Konstruktor für diese Validierung einführen, z. b.

```C#
class UserInfo
{
    ...
    private UserInfo(UserInfo original)
    {
        // validation code
    }
    [WithConstructor]
    public UserInfo With() => new UserInfo(this);
}
```

Die letzte Komplexität, die `With` zugeordnet ist, ist Vererbung. Wenn Ihr Datensatz erweiterbar ist, müssen Sie eine neue `With` für die Unterklasse bereitstellen. Dies kann wie folgt erreicht werden:

```C#
class Base
{
    ...
    protected Base(Base original)
    {
        // validation
    }
    [WithConstructor]
    public virtual Base With() => new Base(this);
}
class Derived : Base
{
    ...
    protected Derived(Derived original)
    : base(original)
    {
        // validation
    }
    [WithConstructor]
    public override Derived With() => new Derived(this);
}
```

Beachten Sie hier eine weitere Komplexität: um den `With`-Konstruktor mit dem abgeleiteten Typ zu überschreiben, muss die Sprache auch kovariante Rückgabe Typen in außer Kraft setzungen unterstützen.
Es gibt bereits einen separaten Vorschlag für [dieses Feature.](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)

**Nachteil**

- Das Festlegen aller Return-Anweisungen in `WithConstructor`s, die neue Objekt Ausdrücke enthalten, ist restriktiv.
  Dies kann möglicherweise durch die Fluss Analyse verringert werden, um sicherzustellen, dass das neue Objekt nicht mit der Methode versehen wird.
- Die Unterstützung von Varianz in außer Kraft setzungen durch compilertricks erfordert Stub-Methoden, die mit der Vererbungs Tiefe quadratisch vergrößert werden. Die Notwendigkeit einer Stubmethode ist auf eine Lauf Zeit Anforderung zurückzuführen, bei der Signaturen überschrieben werden, die exakt übereinstimmen. Wenn die Lauf Zeit Anforderung gelockert wurde, sind die Stub-Methoden überhaupt nicht erforderlich.
- Durch die Verwendung von verketteten Konstruktoren im Formular `Type(Type original)` wird dieser Konstruktor effektiv für die Verwendung des Musters reserviert. Da Konstruktoren über eine eindeutige Semantik verfügen und nicht neu benannt werden können, kann dies eine Einschränkung sein.


## <a name="wrapping-it-all-up-records"></a>Alles packen: Datensätze

Die oben genannten Features ermöglichen die Programmierung, die vor sehr schwierig war. Aber selbst bei den neuen Features könnte es recht ausführlich und fehleranfällig sein, alles selbst zu kommentieren. Es gibt auch einige Elemente, wie z. b. "gleich" und "GetHashCode", die heute bereits geschrieben werden können.
Außerdem besteht ein bedeutender Nachteil bei der Implementierung von Gleichheit über diese neuen primitiven darin, dass die Struktur Gleichheit etwas ist, das sich mit dem Datentyp ändern soll, wenn neue Daten hinzugefügt werden. Wenn Sie es jedoch manuell verarbeiten, ist es wahrscheinlich, dass diese Dinge nicht synchronisiert werden können.

Daher wird vorgeschlagen, dass neue C# Syntax für Datensätze unterstützt, nicht für die Bereitstellung neuer Features, sondern für das Festlegen von Standardwerten und das Erstellen von Code, der für die Verwendung in Datensätzen Die Beispiel Syntax sieht wie folgt aus:

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

Der generierte Code für diese Klasse würde alle öffentlichen Felder und automatischen Eigenschaften als strukturelle Elemente des Datensatzes berücksichtigen. Daten Satz Mitglieder können mithilfe eines neuen `RecordMember(bool)` Attributs angepasst werden, das zum einschließen oder Ausschließen von Membern verwendet werden kann. Daten Satz Mitglieder werden standardmäßig `initonly`, und für die Klasse wird auf der Grundlage der Datensatz-Member die Gleichheit automatisch generiert. An jedem Punkt kann das Verhalten dieser Member einfach durch Deklarieren der Elemente in der Quelle angepasst werden. Die Benutzer geschriebene Implementierung ersetzt die Standard Implementierung in der gesamten Muster Verwendung.

Beachten Sie, dass die Gleichheit im Gesichts der Vererbung Komplex ist, aber anscheinend in den [anderen Daten Satz Vorschläge](records.md)angemessen gelöst wurde.

## <a name="primary-constructors"></a>Primäre Konstruktoren

Der vorherige Datensatz-Vorschlag enthält auch eine neue Syntax für eine Parameterliste für den Typ selbst, z. b.

```C#
class Point(int X, int Y);
```

Beim neuen Design wäre die Parameterliste ein orthogonales C# Feature, das in Datensätzen ordnungsgemäß integriert werden kann. Wenn ein primärer Konstruktor in einem Datensatz enthalten ist, verfügt er wie öffentliche Felder und automatische Eigenschaften über neue Standardwerte: die Parameter im primären Konstruktor werden verwendet, um öffentliche Daten Satz Element Eigenschaften mit demselben Namen zu generieren. Außerdem kann der primäre Konstruktor jetzt zum automatischen Generieren eines Dekonstruktors verwendet werden.

Beispielsweise der folgende Datensatz mit einem primären Konstruktor

```C#
data class Point(int X, int Y);
```

wäre äquivalent zu

```C#
data class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }
}
```

und die endgültige Generierung des obigen Beispiels wäre

```C#
class Point
{
    public initonly int X { get; }
    public initonly int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected Point(Point other)
    : this(other.X, other.Y)
    { }

    [WithConstructor]
    public virtual Point With() => new Point(this);

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }

    // Generated equality
}
```

Beachten Sie, dass wir ein anderes Informationselement für eine Datenklasse mit einem primären Konstruktor übernommen haben: statt die primären Felder im generierten geschützten Konstruktor festzulegen, delegieren wir an den primären Konstruktor. , Wenn die Point-Klasse einen anderen nicht primären Datensatz-Member besaß, z. b.

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

Dadurch würde der generierte geschützte Konstruktor wie folgt geändert werden:

```C#
class Point
{
    // ...
    protected Point(Point other)
    : this(other.X, other.Y)
    {
        Z = other.Z;
    }
    // ...
}
```

Dies hat insbesondere keine Antwort auf die Vererbung von Datensätzen mit primären Konstruktoren. Beispiel:

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

Anstatt auf willkürliche Weise aufzulösen, könnte ein expliziter Ansatz erfordern, dass eine Parameterliste mit der Basisliste bereitgestellt wird, z. b.

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

Die Parameterliste in der Basisliste wird dann auf einen `base` aufrufen im generierten primären Konstruktor angewendet:

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

Wie bei einem primären Konstruktor, der außerhalb eines Datensatzes gemeint sein könnte, ist dies weiterhin für weitere Vorschläge offen.
