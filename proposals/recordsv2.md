---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "79483925"
---

# <a name="records-v2"></a><span data-ttu-id="165ab-101">Datensätze v2</span><span class="sxs-lookup"><span data-stu-id="165ab-101">Records v2</span></span>

<span data-ttu-id="165ab-102">In der Vergangenheit haben wir uns für Datensätze als Feature angesehen, um die Arbeit mit Daten zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="165ab-102">In the past we've thought about records as a feature to enable working with data.</span></span>

<span data-ttu-id="165ab-103">"Arbeiten mit Daten" ist eine große Gruppe mit einer Reihe von Facetten, daher kann es interessant sein, jede einzelne isoliert zu betrachten.</span><span class="sxs-lookup"><span data-stu-id="165ab-103">"Working with data" is a big group with a number of facets, so it may be interesting to look at each in isolation.</span></span> <span data-ttu-id="165ab-104">Betrachten wir nun ein Beispiel für Datensätze heute und einige Nachteile.</span><span class="sxs-lookup"><span data-stu-id="165ab-104">Let's start by looking at an example of records today and some of its drawbacks.</span></span>

<span data-ttu-id="165ab-105">Beispielsweise wird ein einfacher Datensatz heute wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="165ab-105">For instance, a simple record would be defined today as follows</span></span>

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

<span data-ttu-id="165ab-106">und die Verwendung würde lesen</span><span class="sxs-lookup"><span data-stu-id="165ab-106">and the usage would read</span></span>

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

<span data-ttu-id="165ab-107">In diesem Code gibt es bedeutende Vorteile:</span><span class="sxs-lookup"><span data-stu-id="165ab-107">There are significant advantages in this code:</span></span>

1. <span data-ttu-id="165ab-108">Die Definition ist Versions robust, Eigenschaften können einfach hinzugefügt oder verschoben werden.</span><span class="sxs-lookup"><span data-stu-id="165ab-108">The definition is version resilient, properties can easily be added or moved</span></span>
2. <span data-ttu-id="165ab-109">Eigenschaften können in beliebiger Reihenfolge festgelegt werden, und die Namen in der Initialisierung entsprechen immer den Accessoren.</span><span class="sxs-lookup"><span data-stu-id="165ab-109">Properties can be set in any order, and the names in the initialization always match the accessors</span></span>
3. <span data-ttu-id="165ab-110">Eigenschaften mit Standardwerten können einfach übersprungen werden.</span><span class="sxs-lookup"><span data-stu-id="165ab-110">Properties with default values can simply be skipped</span></span>

<span data-ttu-id="165ab-111">Der erste Fehler besteht darin, dass die Eigenschaften nun änderbar sein müssen.</span><span class="sxs-lookup"><span data-stu-id="165ab-111">The first flaw is that the properties must now be mutable.</span></span>

# <a name="mutability"></a><span data-ttu-id="165ab-112">Veränderlichkeit</span><span class="sxs-lookup"><span data-stu-id="165ab-112">Mutability</span></span>

<span data-ttu-id="165ab-113">Wir möchten C# eine Möglichkeit bereitstellen, um einen `readonly` Member in Objektinitialisierern festzulegen.</span><span class="sxs-lookup"><span data-stu-id="165ab-113">What we'd like is for C# to provide a way to set a `readonly` member in object initializers.</span></span>
<span data-ttu-id="165ab-114">Da einige Typen möglicherweise nicht mit dieser Initialisierung entworfen wurden, würden wir Ihnen auch gerne zustimmen.</span><span class="sxs-lookup"><span data-stu-id="165ab-114">Since some types may not have been designed with this initialization in mind, we'd also like it to be opt-in.</span></span>

<span data-ttu-id="165ab-115">Die vorgeschlagene Lösung ist ein neuer Modifizierer, `initonly`, der auf Eigenschaften und Felder angewendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="165ab-115">The proposed solution is a new modifier, `initonly`, that can be applied to properties and fields:</span></span>

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="165ab-116">Die CodeGen hierfür ist erstaunlich einfach: Wir legen nur das schreibgeschützte Feld fest.</span><span class="sxs-lookup"><span data-stu-id="165ab-116">The codegen for this is surprisingly straight forward: we just set the readonly field.</span></span>
<span data-ttu-id="165ab-117">Insbesondere würden die abgesenkten Eigenschaften wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="165ab-117">Specifically, the lowered properties would look like:</span></span>

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

<span data-ttu-id="165ab-118">Die CLR berücksichtigt das Festlegen von schreibgeschützten Feldern als nicht überprüfbar, aber nicht unsicher.</span><span class="sxs-lookup"><span data-stu-id="165ab-118">The CLR considers setting readonly fields to be unverifiable, but not unsafe.</span></span> <span data-ttu-id="165ab-119">Zur Unterstützung eines erweiterten verifizierers wird die folgende Regel vorgeschlagen: ein Schreib geschütztes Feld kann nur in `initonly` Methoden oder in einem neuen Objekt geändert werden, das sich auf dem CLR-Stapel befindet und nicht über einen Speicher-oder Methodenaufrufe veröffentlicht wurde.</span><span class="sxs-lookup"><span data-stu-id="165ab-119">To support a more advanced verifier, the following rule is proposed: a readonly field can be modified only inside `initonly` methods, or on a new object that is on the CLR stack and has not been published via a store or method call.</span></span>

<span data-ttu-id="165ab-120">Dies sollte viele der Probleme mit der Veränderlichkeit im `UserInfo` Beispiel lösen, ohne komplizierte oder nicht komplizierte Ausgabe Strategien zu erfordern.</span><span class="sxs-lookup"><span data-stu-id="165ab-120">This should solve many of the problems with mutability in the `UserInfo` example, while not requiring complicated or brittle emit strategies.</span></span> <span data-ttu-id="165ab-121">Die Unveränderlichkeit stellt jedoch ein neues Problem dar: einfaches Erstellen eines Objekts mit Änderungen.</span><span class="sxs-lookup"><span data-stu-id="165ab-121">However, immutability does present a new problem: easily constructing an object with changes.</span></span>

# <a name="with-ing"></a><span data-ttu-id="165ab-122">Mit-ung</span><span class="sxs-lookup"><span data-stu-id="165ab-122">With-ing</span></span>

<span data-ttu-id="165ab-123">Beim Programmieren mit Unveränderlichkeit wird das vornehmen von Änderungen an einem Objekt durch das Erstellen einer Kopie mit Änderungen vorgenommen, anstatt die Änderungen direkt auf dem Objekt vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="165ab-123">When programming with immutability, making changes to an object is done by constructing a copy with changes instead of making the changes directly on the object.</span></span> <span data-ttu-id="165ab-124">Leider gibt es keine bequeme Methode, dies auch mit Daten C#Sätzen im aktuellen Stil in durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="165ab-124">Unfortunately, there's no convenient way to do this in C#, even with current-style records.</span></span> <span data-ttu-id="165ab-125">Es wurde bereits zuvor vorgeschlagen, dass für Datensätze, die diese Funktionalität implementieren, eine Art automatisch generiertes "with"-Verfahren bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="165ab-125">It's been previously proposed that some kind of autogenerated "With" method be provided for records that implements that functionality.</span></span> <span data-ttu-id="165ab-126">Wenn wir einen solchen Mechanismus haben, ist es wichtig, dass er mit `initonly` Mitgliedern arbeitet.</span><span class="sxs-lookup"><span data-stu-id="165ab-126">If we have such a mechanism, it's important that it work with `initonly` members.</span></span> <span data-ttu-id="165ab-127">Um dies zu erreichen, wird vorgeschlagen, dass wir einen `with` Ausdruck analog zu einem Objektinitialisierer hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="165ab-127">To achieve this, it's proposed that we add a `with` expression, analogous to an object initializer.</span></span> <span data-ttu-id="165ab-128">Die Beispiel Verwendung sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="165ab-128">Sample usage would be as follows:</span></span>

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

<span data-ttu-id="165ab-129">Das resultierende `newUserName` Objekt wäre eine Kopie von `userInfo`, wobei `Username` auf "angocke" festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="165ab-129">The resulting `newUserName` object would be a copy of `userInfo`, with `Username` set to "angocke".</span></span>
<span data-ttu-id="165ab-130">Die CodeGen im `with` Ausdruck ähnelt auch dem Objektinitialisierer: ein neues-Objekt wird erstellt, und anschließend wird der `initonly` `Username` Setter im Methoden Text aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="165ab-130">The codegen on the `with` expression would also be similar to the object initializer: a new object is constructed, and then the `initonly` `Username` setter would be called in the method body.</span></span>

<span data-ttu-id="165ab-131">Der Unterschied besteht hierbei darin, dass das neu erstellte Objekt keine einfache Erstellung eines neuen Objekts ist, sondern ein Duplikat des ursprünglichen Objekts.</span><span class="sxs-lookup"><span data-stu-id="165ab-131">Of course, the difference here is that the new object being constructed is not a simple new object creation, it is a duplicate of the original object.</span></span> <span data-ttu-id="165ab-132">Um diese Funktionalität bereitzustellen, ist es erforderlich, dass das-Objekt einen "with-Konstruktor" bereitstellt, der ein doppeltes Objekt bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="165ab-132">To provide this functionality, we require that the object provide a "With constructor" that provides a duplicate object.</span></span> <span data-ttu-id="165ab-133">Ein Beispiel `With` Konstruktors sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="165ab-133">A sample `With` constructor would look like:</span></span>

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

<span data-ttu-id="165ab-134">Der `with` Ausdruck legt `initonly` Member genau wie der Objektinitialisierer fest. um die Überprüfung zu unterstützen, müssen Sie daher sicherstellen, dass das Objekt nicht veröffentlicht werden kann, bevor die `initonly` Elemente festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="165ab-134">Notably, the `with` expression will set `initonly` members, just like the object initializer, so to support verification we must ensure that the object cannot have been published before the `initonly` members are set.</span></span> <span data-ttu-id="165ab-135">Um dies zu erzwingen, erzwingt das `WithConstructor`-Attribut (oder die entsprechende Syntax) eine neue Regel für die-Methode: alle Return-Anweisungen müssen direkt einen Objekt Erstellungs Ausdruck enthalten, möglicherweise mit einem Objektinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="165ab-135">To enforce this, the `WithConstructor` attribute (or equivalent syntax) will enforce a new rule for the method: all return statements must directly contain an object creation expression, possibly with an object initializer.</span></span>

<span data-ttu-id="165ab-136">Wenn der `With`-Konstruktor eine Validierung erfordert, kann der Benutzer einen Konstruktor für diese Validierung einführen, z. b.</span><span class="sxs-lookup"><span data-stu-id="165ab-136">If the `With` constructor requires validation, the user may introduce a constructor to do that validation, e.g.</span></span>

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

<span data-ttu-id="165ab-137">Die letzte Komplexität, die `With` zugeordnet ist, ist Vererbung.</span><span class="sxs-lookup"><span data-stu-id="165ab-137">The last piece of complexity associated with `With` is inheritance.</span></span> <span data-ttu-id="165ab-138">Wenn Ihr Datensatz erweiterbar ist, müssen Sie eine neue `With` für die Unterklasse bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="165ab-138">If your record is extensible, you will need to provide a new `With` for the subclass.</span></span> <span data-ttu-id="165ab-139">Dies kann wie folgt erreicht werden:</span><span class="sxs-lookup"><span data-stu-id="165ab-139">This can be achieved as follows:</span></span>

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

<span data-ttu-id="165ab-140">Beachten Sie hier eine weitere Komplexität: um den `With`-Konstruktor mit dem abgeleiteten Typ zu überschreiben, muss die Sprache auch kovariante Rückgabe Typen in außer Kraft setzungen unterstützen.</span><span class="sxs-lookup"><span data-stu-id="165ab-140">Note one additional piece of complexity here: in order to override the `With` constructor with the derived type the language will also need to support covariant return types in overrides.</span></span>
<span data-ttu-id="165ab-141">Es gibt bereits einen separaten Vorschlag für [dieses Feature.](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)</span><span class="sxs-lookup"><span data-stu-id="165ab-141">There is already a separate proposal for this feature [here](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md).</span></span>

<span data-ttu-id="165ab-142">**Nachteil**</span><span class="sxs-lookup"><span data-stu-id="165ab-142">**Drawbacks**</span></span>

- <span data-ttu-id="165ab-143">Das Festlegen aller Return-Anweisungen in `WithConstructor`s, die neue Objekt Ausdrücke enthalten, ist restriktiv.</span><span class="sxs-lookup"><span data-stu-id="165ab-143">Making all return statements in `WithConstructor`s contain new object expressions is restrictive.</span></span>
  <span data-ttu-id="165ab-144">Dies kann möglicherweise durch die Fluss Analyse verringert werden, um sicherzustellen, dass das neue Objekt nicht mit der Methode versehen wird.</span><span class="sxs-lookup"><span data-stu-id="165ab-144">This could be possibly be mitigated by flow analysis that ensures the new object doesn't escape the method</span></span>
- <span data-ttu-id="165ab-145">Die Unterstützung von Varianz in außer Kraft setzungen durch compilertricks erfordert Stub-Methoden, die mit der Vererbungs Tiefe quadratisch vergrößert werden.</span><span class="sxs-lookup"><span data-stu-id="165ab-145">Supporting variance in overrides through compiler tricks will require stub methods, which will grow quadratically with the inheritance depth.</span></span> <span data-ttu-id="165ab-146">Die Notwendigkeit einer Stubmethode ist auf eine Lauf Zeit Anforderung zurückzuführen, bei der Signaturen überschrieben werden, die exakt übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="165ab-146">The need for a stub method is due to a runtime requirement that override signatures match exactly.</span></span> <span data-ttu-id="165ab-147">Wenn die Lauf Zeit Anforderung gelockert wurde, sind die Stub-Methoden überhaupt nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="165ab-147">If the runtime requirement were loosened, the stub methods would not be required at all.</span></span>
- <span data-ttu-id="165ab-148">Durch die Verwendung von verketteten Konstruktoren im Formular `Type(Type original)` wird dieser Konstruktor effektiv für die Verwendung des Musters reserviert.</span><span class="sxs-lookup"><span data-stu-id="165ab-148">Using chained constructors of the form `Type(Type original)` effectively reserves that constructor for the use of the pattern.</span></span> <span data-ttu-id="165ab-149">Da Konstruktoren über eine eindeutige Semantik verfügen und nicht neu benannt werden können, kann dies eine Einschränkung sein.</span><span class="sxs-lookup"><span data-stu-id="165ab-149">Since constructors have unique semantics and cannot be re-named this could be limiting.</span></span>


## <a name="wrapping-it-all-up-records"></a><span data-ttu-id="165ab-150">Alles packen: Datensätze</span><span class="sxs-lookup"><span data-stu-id="165ab-150">Wrapping it all up: Records</span></span>

<span data-ttu-id="165ab-151">Die oben genannten Features ermöglichen die Programmierung, die vor sehr schwierig war.</span><span class="sxs-lookup"><span data-stu-id="165ab-151">The above features enable a style of programming that was very difficult before.</span></span> <span data-ttu-id="165ab-152">Aber selbst bei den neuen Features könnte es recht ausführlich und fehleranfällig sein, alles selbst zu kommentieren.</span><span class="sxs-lookup"><span data-stu-id="165ab-152">But even with the new features it could be quite verbose and error prone to annotate everything yourself.</span></span> <span data-ttu-id="165ab-153">Es gibt auch einige Elemente, wie z. b. "gleich" und "GetHashCode", die heute bereits geschrieben werden können.</span><span class="sxs-lookup"><span data-stu-id="165ab-153">There are also a few items, like Equals and GetHashCode, which can already be written today, it's just laborious.</span></span>
<span data-ttu-id="165ab-154">Außerdem besteht ein bedeutender Nachteil bei der Implementierung von Gleichheit über diese neuen primitiven darin, dass die Struktur Gleichheit etwas ist, das sich mit dem Datentyp ändern soll, wenn neue Daten hinzugefügt werden. Wenn Sie es jedoch manuell verarbeiten, ist es wahrscheinlich, dass diese Dinge nicht synchronisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="165ab-154">Moreover, a significant flaw in implementing equality on top of these new primitives is that structural equality is something that should change with your data type as new data is added, but when handling it manually it is likely that these things can get out of sync.</span></span>

<span data-ttu-id="165ab-155">Daher wird vorgeschlagen, dass neue C# Syntax für Datensätze unterstützt, nicht für die Bereitstellung neuer Features, sondern für das Festlegen von Standardwerten und das Erstellen von Code, der für die Verwendung in Datensätzen</span><span class="sxs-lookup"><span data-stu-id="165ab-155">Therefore, it is proposed that C# support new syntax for records, not for providing new features, but for setting defaults and generating code designed for use in records.</span></span> <span data-ttu-id="165ab-156">Die Beispiel Syntax sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="165ab-156">Example syntax would look like</span></span>

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="165ab-157">Der generierte Code für diese Klasse würde alle öffentlichen Felder und automatischen Eigenschaften als strukturelle Elemente des Datensatzes berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="165ab-157">The generated code for this class would regard all public fields and auto-properties as structural members of the record.</span></span> <span data-ttu-id="165ab-158">Daten Satz Mitglieder können mithilfe eines neuen `RecordMember(bool)` Attributs angepasst werden, das zum einschließen oder Ausschließen von Membern verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="165ab-158">Record members could be customized using a new `RecordMember(bool)` attribute that could be used to either include or exclude members.</span></span> <span data-ttu-id="165ab-159">Daten Satz Mitglieder werden standardmäßig `initonly`, und für die Klasse wird auf der Grundlage der Datensatz-Member die Gleichheit automatisch generiert.</span><span class="sxs-lookup"><span data-stu-id="165ab-159">Record members would be `initonly` by default and equality would be autogenerated for the class based on the record members.</span></span> <span data-ttu-id="165ab-160">An jedem Punkt kann das Verhalten dieser Member einfach durch Deklarieren der Elemente in der Quelle angepasst werden.</span><span class="sxs-lookup"><span data-stu-id="165ab-160">At any point the behavior of these members could be customized simply by declaring them in source.</span></span> <span data-ttu-id="165ab-161">Die Benutzer geschriebene Implementierung ersetzt die Standard Implementierung in der gesamten Muster Verwendung.</span><span class="sxs-lookup"><span data-stu-id="165ab-161">The user-written implementation would replace the default implementation in all pattern usage.</span></span>

<span data-ttu-id="165ab-162">Beachten Sie, dass die Gleichheit im Gesichts der Vererbung Komplex ist, aber anscheinend in den [anderen Daten Satz Vorschläge](records.md)angemessen gelöst wurde.</span><span class="sxs-lookup"><span data-stu-id="165ab-162">Note that equality in the face of inheritance is complex, but seems to have been adequately solved in the [other records proposal](records.md).</span></span>

## <a name="primary-constructors"></a><span data-ttu-id="165ab-163">Primäre Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="165ab-163">Primary constructors</span></span>

<span data-ttu-id="165ab-164">Der vorherige Datensatz-Vorschlag enthält auch eine neue Syntax für eine Parameterliste für den Typ selbst, z. b.</span><span class="sxs-lookup"><span data-stu-id="165ab-164">Previous record proposal have also included a new syntax for a parameter list on the type itself, e.g.</span></span>

```C#
class Point(int X, int Y);
```

<span data-ttu-id="165ab-165">Beim neuen Design wäre die Parameterliste ein orthogonales C# Feature, das in Datensätzen ordnungsgemäß integriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="165ab-165">In the new design, the parameter list would be an orthogonal C# feature, which could be cleanly integrated with records.</span></span> <span data-ttu-id="165ab-166">Wenn ein primärer Konstruktor in einem Datensatz enthalten ist, verfügt er wie öffentliche Felder und automatische Eigenschaften über neue Standardwerte: die Parameter im primären Konstruktor werden verwendet, um öffentliche Daten Satz Element Eigenschaften mit demselben Namen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="165ab-166">If a primary constructor is included in a record, it would have new defaults, just like public fields and auto-properties: the parameters in the primary constructor would be used to generate public record-member properties with the same name.</span></span> <span data-ttu-id="165ab-167">Außerdem kann der primäre Konstruktor jetzt zum automatischen Generieren eines Dekonstruktors verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="165ab-167">In addition, the primary constructor could now be used to auto-generate a deconstructor.</span></span>

<span data-ttu-id="165ab-168">Beispielsweise der folgende Datensatz mit einem primären Konstruktor</span><span class="sxs-lookup"><span data-stu-id="165ab-168">For example, the following record with a primary constructor</span></span>

```C#
data class Point(int X, int Y);
```

<span data-ttu-id="165ab-169">wäre äquivalent zu</span><span class="sxs-lookup"><span data-stu-id="165ab-169">would be equivalent to</span></span>

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

<span data-ttu-id="165ab-170">und die endgültige Generierung des obigen Beispiels wäre</span><span class="sxs-lookup"><span data-stu-id="165ab-170">and the final generation of the above would be</span></span>

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

<span data-ttu-id="165ab-171">Beachten Sie, dass wir ein anderes Informationselement für eine Datenklasse mit einem primären Konstruktor übernommen haben: statt die primären Felder im generierten geschützten Konstruktor festzulegen, delegieren wir an den primären Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="165ab-171">Note that we've taken one other piece of information into account for a data class with a primary constructor: instead of setting the primary fields inside the generated protected constructor, we delegate to the primary constructor.</span></span> <span data-ttu-id="165ab-172">, Wenn die Point-Klasse einen anderen nicht primären Datensatz-Member besaß, z. b.</span><span class="sxs-lookup"><span data-stu-id="165ab-172">If the Point class had another non-primary record member, e.g.</span></span>

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

<span data-ttu-id="165ab-173">Dadurch würde der generierte geschützte Konstruktor wie folgt geändert werden:</span><span class="sxs-lookup"><span data-stu-id="165ab-173">then that would change the generated protected constructor as follows:</span></span>

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

<span data-ttu-id="165ab-174">Dies hat insbesondere keine Antwort auf die Vererbung von Datensätzen mit primären Konstruktoren.</span><span class="sxs-lookup"><span data-stu-id="165ab-174">Notably, this doesn't answer what to do about inheritance of records with primary constructors.</span></span> <span data-ttu-id="165ab-175">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="165ab-175">For instance,</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

<span data-ttu-id="165ab-176">Anstatt auf willkürliche Weise aufzulösen, könnte ein expliziter Ansatz erfordern, dass eine Parameterliste mit der Basisliste bereitgestellt wird, z. b.</span><span class="sxs-lookup"><span data-stu-id="165ab-176">Rather than resolving in an arbitrary manner, a more explicit approach could require that a parameter list be provided with the base list, e.g.</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

<span data-ttu-id="165ab-177">Die Parameterliste in der Basisliste wird dann auf einen `base` aufrufen im generierten primären Konstruktor angewendet:</span><span class="sxs-lookup"><span data-stu-id="165ab-177">The parameter list in the base list would then be applied to a `base` call in the generated primary constructor:</span></span>

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

<span data-ttu-id="165ab-178">Wie bei einem primären Konstruktor, der außerhalb eines Datensatzes gemeint sein könnte, ist dies weiterhin für weitere Vorschläge offen.</span><span class="sxs-lookup"><span data-stu-id="165ab-178">As for what a primary constructor could mean outside of a record, that is still open to further proposal.</span></span>
