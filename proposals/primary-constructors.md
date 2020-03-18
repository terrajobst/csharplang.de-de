---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "79483901"
---
# <a name="primary-constructors"></a><span data-ttu-id="1aaad-101">Primäre Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="1aaad-101">Primary constructors</span></span>

* <span data-ttu-id="1aaad-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="1aaad-102">[x] Proposed</span></span>
* <span data-ttu-id="1aaad-103">[] Prototyp: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="1aaad-103">[ ] Prototype: Not started</span></span>
* <span data-ttu-id="1aaad-104">[] Implementierung: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="1aaad-104">[ ] Implementation: Not started</span></span>
* <span data-ttu-id="1aaad-105">[] Spezifikation: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="1aaad-105">[ ] Specification: Not started</span></span>

## <a name="summary"></a><span data-ttu-id="1aaad-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="1aaad-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="1aaad-107">Klassen können über eine Parameterliste verfügen, und wenn Sie dies tun, kann Ihre Basisklassen Spezifikation eine Argumentliste haben.</span><span class="sxs-lookup"><span data-stu-id="1aaad-107">Classes can have a parameter list, and when they do, their base class specification can have an argument list.</span></span>
<span data-ttu-id="1aaad-108">Primäre Konstruktorparameter befinden sich innerhalb der Klassen Deklaration im Gültigkeitsbereich. Wenn Sie von einem Funktionsmember oder einer anonymen Funktion aufgezeichnet werden, werden Sie als private Felder in der-Klasse gespeichert.</span><span class="sxs-lookup"><span data-stu-id="1aaad-108">Primary constructor parameters are in scope throughout the class declaration, and if they are captured by a function member or anonymous function, they are stored as private fields in the class.</span></span>

## <a name="motivation"></a><span data-ttu-id="1aaad-109">Motivation</span><span class="sxs-lookup"><span data-stu-id="1aaad-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="1aaad-110">Es kommt häufig vor, dass im Programm Initialisierungs Code viele Bausteine vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="1aaad-110">It is common to have a lot of boilerplate in program initialization code.</span></span> <span data-ttu-id="1aaad-111">Im Allgemeinen wird eine bestimmte Daten `x` mehrmals erwähnt:</span><span class="sxs-lookup"><span data-stu-id="1aaad-111">In the general case, a given piece of data `x` is mentioned many times:</span></span>

- <span data-ttu-id="1aaad-112">Als privates Feld `_x`</span><span class="sxs-lookup"><span data-stu-id="1aaad-112">As a private field `_x`</span></span>
- <span data-ttu-id="1aaad-113">Als Parameter `x` einem Konstruktor</span><span class="sxs-lookup"><span data-stu-id="1aaad-113">As a parameter `x` to a constructor</span></span>
- <span data-ttu-id="1aaad-114">In einer Zuweisungs `_x = x;` des Felds aus dem-Parameter im-Konstruktor</span><span class="sxs-lookup"><span data-stu-id="1aaad-114">In an assignment `_x = x;` of the field from the parameter in the constructor</span></span>
- <span data-ttu-id="1aaad-115">Als Eigenschaften `X`</span><span class="sxs-lookup"><span data-stu-id="1aaad-115">As a property `X`</span></span>
- <span data-ttu-id="1aaad-116">Im Eigenschaften Setter `x = value;`</span><span class="sxs-lookup"><span data-stu-id="1aaad-116">In the property setter `x = value;`</span></span>
- <span data-ttu-id="1aaad-117">Im Eigenschaften Getter `return x;`</span><span class="sxs-lookup"><span data-stu-id="1aaad-117">In the property getter `return x;`</span></span>

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

<span data-ttu-id="1aaad-118">Bei Eigenschaften, für die keine Validierung oder Berechnung erforderlich ist, kann das aufwändigen Schritte mithilfe von automatischen Eigenschaften reduziert werden. dadurch ist es nicht mehr erforderlich, ein explizites Unterstützungs Feld für die Eigenschaft zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="1aaad-118">For properties that don't require validation or computation, the tedium can be reduced using auto-properties, thus cutting out the need to declare an explicit backing field for the property.</span></span> <span data-ttu-id="1aaad-119">Wenn Ihre Eigenschaft jedoch eine beliebige Logik erfordert, die über das Bereitstellen einer Auto-Eigenschaft hinausgeht, ist das oben genannte das beste, was Sie tun.</span><span class="sxs-lookup"><span data-stu-id="1aaad-119">But if your property requires any sort of logic beyond what an auto-property provides, the above is the best you an do.</span></span>

<span data-ttu-id="1aaad-120">Primäre Konstruktoren verringern stattdessen den mehr Aufwand durch das direkte Einfügen von Konstruktorargumenten in den Gültigkeitsbereich der gesamten Klasse, wodurch die Notwendigkeit besteht, ein dahinter liegendes Feld explizit zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="1aaad-120">Primary constructors instead reduce the overhead by putting constructor arguments directly in scope throughout the class, again obviating the need to explicitly declare a backing field.</span></span> <span data-ttu-id="1aaad-121">Daher würde das obige Beispiel lauten:</span><span class="sxs-lookup"><span data-stu-id="1aaad-121">Thus, the above example would become:</span></span>

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

<span data-ttu-id="1aaad-122">In diesem Beispiel reduziert der primäre Konstruktor die Anzahl der benannten Entitäten für die `x` von drei auf zwei, wobei das `_x` dahinter liegende Feld vermieden wird.</span><span class="sxs-lookup"><span data-stu-id="1aaad-122">In this example, the primary constructor reduces the number of named entities for `x` from three to two, obviating the `_x` backing field.</span></span> <span data-ttu-id="1aaad-123">Es entfernt zwei aus drei Element Deklarationen (wobei nur die Eigenschafts Deklaration selbst beibehalten wird) und reduziert die Gesamtzahl der Erwähnungen von `x`/`_x`/`X` von 8 bis 5.</span><span class="sxs-lookup"><span data-stu-id="1aaad-123">It removes two out of three member declarations (keeping only the property declaration itself), and reduces the total number of mentions of `x`/`_x`/`X` from eight to five.</span></span>


## <a name="detailed-design"></a><span data-ttu-id="1aaad-124">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="1aaad-124">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="1aaad-125">Klassen können eine Parameterliste aufweisen:</span><span class="sxs-lookup"><span data-stu-id="1aaad-125">Classes can have a parameter list:</span></span>

``` c#
public class C(int i, string s)
{
    ...
}
```

<span data-ttu-id="1aaad-126">Die Parameterliste bewirkt, dass ein Konstruktor implizit für die Klasse deklariert wird, mit der gleichen Barrierefreiheit wie die Klasse selbst.</span><span class="sxs-lookup"><span data-stu-id="1aaad-126">The parameter list causes a constructor to be implicitly declared for the class, with the same accessibility as the class itself.</span></span>

``` c#
new C(5, "Hello");
```

<span data-ttu-id="1aaad-127">Primäre Konstruktorparameter befinden sich im Gültigkeitsbereich des gesamten Klassen Texts.</span><span class="sxs-lookup"><span data-stu-id="1aaad-127">Primary constructor parameters are in scope throughout the class body.</span></span> <span data-ttu-id="1aaad-128">Wenn Sie von einem Funktionsmember oder einer anonymen Funktion aufgezeichnet werden, werden Sie als private Felder in der-Klasse gespeichert.</span><span class="sxs-lookup"><span data-stu-id="1aaad-128">If they are captured by a function member or anonymous function, they become stored as private fields in the class.</span></span> <span data-ttu-id="1aaad-129">Wenn Sie nur während der Initialisierung verwendet werden, werden Sie nicht im-Objekt gespeichert.</span><span class="sxs-lookup"><span data-stu-id="1aaad-129">If they are only used during initialization they will not be stored in the object.</span></span>

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

<span data-ttu-id="1aaad-130">Wenn eine Klasse mit einem primären Konstruktor über eine Basisklassen Spezifikation verfügt, kann diese eine Argumentliste haben.</span><span class="sxs-lookup"><span data-stu-id="1aaad-130">If a class with a primary constructor has a base class specification, that one can have an argument list.</span></span> <span data-ttu-id="1aaad-131">Dies dient als Argumentliste für einen `base(...)` Initialisierer des implizit deklarierten Konstruktors.</span><span class="sxs-lookup"><span data-stu-id="1aaad-131">This serves as the argument list to a `base(...)` initializer of the implicitly declared constructor.</span></span> <span data-ttu-id="1aaad-132">Wenn keine Argumentliste bereitgestellt wird, wird ein leerer Wert angenommen.</span><span class="sxs-lookup"><span data-stu-id="1aaad-132">If no argument list is provided, an empty one is assumed.</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
<span data-ttu-id="1aaad-133">Die Klasse kann auch explizit definierte Konstruktoren aufweisen, aber alle müssen einen `this(...)` Initialisierer verwenden.</span><span class="sxs-lookup"><span data-stu-id="1aaad-133">The class can have explicitly defined constructors as well, but those all have to use a `this(...)` initializer.</span></span> <span data-ttu-id="1aaad-134">Dadurch wird sichergestellt, dass der primäre Konstruktor immer aufgerufen wird, wenn eine neue Instanz erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="1aaad-134">This ensures that the primary constructor is always called when a new instance is constructed.</span></span>

<span data-ttu-id="1aaad-135">Alle Initialisierer im Klassen Text werden zu Zuweisungen im generierten Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="1aaad-135">All initializers in the class body will become assignments in the generated constructor.</span></span> <span data-ttu-id="1aaad-136">Dies bedeutet, dass Initialisierer im Gegensatz zu anderen Klassen ausgeführt werden, *nachdem* der Basiskonstruktor aufgerufen wurde, nicht vor.</span><span class="sxs-lookup"><span data-stu-id="1aaad-136">This means that, unlike other classes, initializers will run *after* the base constructor has been invoked, not before.</span></span> <span data-ttu-id="1aaad-137">Außerdem enthält die generierte-Klasse Zuweisungen, um alle privaten Felder zu initialisieren, die generiert wurden, um primäre Konstruktorparameter zu speichern, die von Membern aufgezeichnet wurden.</span><span class="sxs-lookup"><span data-stu-id="1aaad-137">In addition, the generated class will contain assignments to initialize any private fields that were generated to store primary constructor parameters that were captured by members.</span></span> <span data-ttu-id="1aaad-138">Diese Member werden umgeschrieben, um das private-Feld anstelle des-Parameters auf eine Weise zu verwenden, die von Abschlüssen für Lambda-Ausdrücke ähnlich ist.</span><span class="sxs-lookup"><span data-stu-id="1aaad-138">Those members are rewritten to use the private field instead of the parameter in a manner similar to closures for lambda expressions.</span></span> <span data-ttu-id="1aaad-139">Die generierten primären Felder werden zuerst initialisiert. Anschließend werden die initialisierergenerierten Zuweisungen in der Reihenfolge der Darstellung in der-Klasse ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="1aaad-139">The generated primary fields are initialized first, and then the initializer-generated assignments are executed in the order of appearance in the class.</span></span>

<span data-ttu-id="1aaad-140">Im obigen Beispiel ist die Auswirkung der Klassen Deklaration wie folgt umgeschrieben:</span><span class="sxs-lookup"><span data-stu-id="1aaad-140">For the above example, the effect of the class declaration is as if rewritten like this:</span></span>

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

<span data-ttu-id="1aaad-141">Die Erfassung hat ähnliche Einschränkungen wie die Erfassung lokaler Variablen durch Lambda-Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="1aaad-141">The capture has similar restrictions to the capture of local variables by lambda expressions.</span></span> <span data-ttu-id="1aaad-142">Beispielsweise sind `ref`-und `out` Parameter in primären Konstruktoren zulässig, können aber keine Element Körper erfasst werden.</span><span class="sxs-lookup"><span data-stu-id="1aaad-142">For instance, `ref` and `out` parameters are allowed in primary constructors, but cannot be captured my member bodies.</span></span>


## <a name="drawbacks"></a><span data-ttu-id="1aaad-143">Nachteile</span><span class="sxs-lookup"><span data-stu-id="1aaad-143">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="1aaad-144">In groben Reihenfolge von Bedeutung.</span><span class="sxs-lookup"><span data-stu-id="1aaad-144">In rough order of significance.</span></span>

* <span data-ttu-id="1aaad-145">Der Vorschlag verwendet die Syntax, die auch für Datensätze mit Feldern fester Breite vorgeschlagen wurde.</span><span class="sxs-lookup"><span data-stu-id="1aaad-145">The proposal uses syntax that has also been proposed for positional records.</span></span> <span data-ttu-id="1aaad-146">Wenn wir beide Features wünschen, ist eine gewisse Unterbringung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="1aaad-146">If we desire both features, some accommodation is required.</span></span> <span data-ttu-id="1aaad-147">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1aaad-147">E.g.</span></span> <span data-ttu-id="1aaad-148">Es wurde ein `data` Modifizierer für Datensätze vorgeschlagen.</span><span class="sxs-lookup"><span data-stu-id="1aaad-148">a `data` modifier on records has been proposed.</span></span>
* <span data-ttu-id="1aaad-149">Die Zuordnungs Größe von konstruierten Objekten ist weniger offensichtlich, da der Compiler bestimmt, ob basierend auf dem vollständigen Text der Klasse ein Feld für einen primären Konstruktorparameter zugeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="1aaad-149">The allocation size of constructed objects is less obvious, as the compiler determines whether to allocate a field for a primary constructor parameter based on the full text of the class.</span></span> <span data-ttu-id="1aaad-150">Dieses Risiko ähnelt der impliziten Erfassung von Variablen durch Lambda-Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="1aaad-150">This risk is similar to the implicit capture of variables by lambda expressions.</span></span>
* <span data-ttu-id="1aaad-151">Eine gängige Versuchung (oder ein versehentliches Muster) könnte darin bestehen, den "gleichen" Parameter auf mehreren Vererbungs Ebenen zu erfassen, da er an die konstruktorkette übergeben wird, anstatt ihm explizit ein geschütztes Feld in der Basisklasse zuzuordnen, was zu doppelten Zuordnungen führt. für dieselben Daten in-Objekten.</span><span class="sxs-lookup"><span data-stu-id="1aaad-151">A common temptation (or accidental pattern) might be to capture the "same" parameter at multiple levels of inheritance as it is passed up the constructor chain instead of explicitly allotting it a protected field at the base class, leading to duplicated allocations for the same data in objects.</span></span> <span data-ttu-id="1aaad-152">Dies ähnelt dem heutigen Risiko, automatische Eigenschaften mit automatischen Eigenschaften zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="1aaad-152">This is very similar to today's risk of overriding auto-properties with auto-properties.</span></span> 
* <span data-ttu-id="1aaad-153">Wie oben bereits erwähnt, gibt es keinen Ort für zusätzliche Logik, die in der Regel in konstruktortexten ausgedrückt wird.</span><span class="sxs-lookup"><span data-stu-id="1aaad-153">As proposed above, there is no place for additional logic that might usually expressed in constructor bodies.</span></span> <span data-ttu-id="1aaad-154">Die Erweiterung "primäre konstruktortexte" unten behandelt die.</span><span class="sxs-lookup"><span data-stu-id="1aaad-154">The "Primary constructor bodies" extension below addresses that.</span></span>
* <span data-ttu-id="1aaad-155">Wie bereits vorgeschlagen, unterscheidet sich die Semantik der Ausführungsreihenfolge von den normalen Konstruktoren.</span><span class="sxs-lookup"><span data-stu-id="1aaad-155">As proposed, execution order semantics are subtly different than with ordinary constructors.</span></span> <span data-ttu-id="1aaad-156">Dies kann wahrscheinlich behoben werden, aber auf Kosten einiger Erweiterungs Vorschläge (insbesondere "primäre konstruktortexte").</span><span class="sxs-lookup"><span data-stu-id="1aaad-156">This could probably be remedied, but at the cost of some of the extension proposals (notably "Primary constructor bodies").</span></span>
* <span data-ttu-id="1aaad-157">Der Vorschlag funktioniert nur, wenn ein einzelner Konstruktor als primär festgelegt werden kann.</span><span class="sxs-lookup"><span data-stu-id="1aaad-157">The proposal only works when a single constructor can be designated primary.</span></span>
* <span data-ttu-id="1aaad-158">Es gibt keine Möglichkeit, unterschiedliche Zugriffsmöglichkeiten für die-Klasse und den primären Konstruktor zu haben.</span><span class="sxs-lookup"><span data-stu-id="1aaad-158">There is no way to have separate accessibility of the class and the primary constructor.</span></span> <span data-ttu-id="1aaad-159">Beispielsweise in Situationen, in denen öffentliche Konstruktoren alle an einen privaten Build-it-all-Konstruktor delegieren, der benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="1aaad-159">For instance, in situations where public constructors all delegate to one private "build-it-all" constructor that would be needed.</span></span> <span data-ttu-id="1aaad-160">Bei Bedarf könnte die Syntax dafür später vorgeschlagen werden.</span><span class="sxs-lookup"><span data-stu-id="1aaad-160">If necessary, syntax could be proposed for that later.</span></span>


## <a name="alternatives"></a><span data-ttu-id="1aaad-161">Alternativen</span><span class="sxs-lookup"><span data-stu-id="1aaad-161">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="1aaad-162">Vollständige Datensätze mit Feldern fester Breite können eine Alternative sein oder je nach Besonderheiten mit primären Konstruktoren koexistieren.</span><span class="sxs-lookup"><span data-stu-id="1aaad-162">Full-on positional records may be an alternative, or may coexist with primary constructors, depending on the specifics.</span></span> <span data-ttu-id="1aaad-163">Sie würden in einer *kleineren* Anzahl von Szenarien eine *genauere* Abkürzung ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="1aaad-163">They would allow for *more* abbreviation in a *smaller* number of scenarios.</span></span> <span data-ttu-id="1aaad-164">Beide sind möglicherweise nützlich, aber beide können außer Kraft gesetzt werden, es sei denn, Sie können in einander nahtlos integriert werden.</span><span class="sxs-lookup"><span data-stu-id="1aaad-164">So both are potentially useful, but having both may be overkill, unless they can be somewhat neatly integrated with each other.</span></span>


## <a name="possible-extensions"></a><span data-ttu-id="1aaad-165">Mögliche Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="1aaad-165">Possible extensions</span></span>
[extensions]: #possible-extensions

<span data-ttu-id="1aaad-166">Dabei handelt es sich um Variationen oder Ergänzungen des Kern Angebots, die möglicherweise in Verbindung mit der Anwendung oder zu einem späteren Zeitpunkt als nützlich angesehen werden.</span><span class="sxs-lookup"><span data-stu-id="1aaad-166">These are variations or additions to the core proposal that may be considered in conjunction with it, or at a later stage if deemed useful.</span></span>

### <a name="primary-constructor-bodies"></a><span data-ttu-id="1aaad-167">Primäre konstruktortexte</span><span class="sxs-lookup"><span data-stu-id="1aaad-167">Primary constructor bodies</span></span>

<span data-ttu-id="1aaad-168">Konstruktoren selbst enthalten häufig Parameter Validierungs Logik oder anderen nicht trivialen Initialisierungs Code, der nicht als Initialisierer ausgedrückt werden kann.</span><span class="sxs-lookup"><span data-stu-id="1aaad-168">Constructors themselves often contain parameter validation logic or other nontrivial initialization code that cannot be expressed as initializers.</span></span>

<span data-ttu-id="1aaad-169">Primäre Konstruktoren können so erweitert werden, dass Anweisungsblöcke direkt im Klassen Text angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="1aaad-169">Primary constructors could be extended to allow statement blocks to appear directly in the class body.</span></span> <span data-ttu-id="1aaad-170">Diese Anweisungen werden im generierten Konstruktor an dem Punkt eingefügt, an dem Sie zwischen den Initialisierungs Zuweisungen angezeigt werden, und werden daher mit Initialisierern zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="1aaad-170">Those statements would be inserted in the generated constructor at the point where they appear between initializing assignments, and would thus be executed interspersed with initializers.</span></span> <span data-ttu-id="1aaad-171">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1aaad-171">For instance:</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a><span data-ttu-id="1aaad-172">Initialisiererfelder und initialisiererfunktionen</span><span class="sxs-lookup"><span data-stu-id="1aaad-172">Initializer fields and initializer functions</span></span>

<span data-ttu-id="1aaad-173">In einer Klasse mit einem primären Konstruktor könnten wir Feld-und Methoden Deklarationen ohne Zugriffsmodifizierer als lokale Variablen und lokale Funktionen in Erwägung gezogen werden:</span><span class="sxs-lookup"><span data-stu-id="1aaad-173">In a class with a primary constructor we could consider field and method declarations without accessibility modifiers to be more like local variables and local functions:</span></span>

* <span data-ttu-id="1aaad-174">Ebenso wie primäre Konstruktorparameter werden die initialisiererfelder nur in einem tatsächlichen privaten Feld aufgezeichnet, wenn Sie in Funktionsmembern verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="1aaad-174">Just like primary constructor parameters the "initializer fields" would only be captured into an actual private field if they were used in function members.</span></span>
* <span data-ttu-id="1aaad-175">Die initialisiererfunktionen werden nur für die Erfassung primärer Konstruktorparameter und initialisiererfelder in Erwägung gezogen, wenn Sie selbst in anderen Funktionsmembern verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="1aaad-175">The "initializer functions" would only be considered to capture primary constructor parameters and initializer fields if they were themselves used in other function members.</span></span> <span data-ttu-id="1aaad-176">Wenn Sie nicht erfasst werden, können Sie in einer optimalen Weise generiert werden, wie z. b. lokale Funktionen.</span><span class="sxs-lookup"><span data-stu-id="1aaad-176">If not captured, they could be generated in a more optimal fashion, like local functions.</span></span>
* <span data-ttu-id="1aaad-177">Ebenso wie primäre Konstruktorparameter sind Sie nicht über den Member-Zugriff verfügbar, sondern nur als einfacher Name.</span><span class="sxs-lookup"><span data-stu-id="1aaad-177">Just like primary constructor parameters they would not be available via member access, but only as a simple name.</span></span>

<span data-ttu-id="1aaad-178">Dies kann für temporäre Variablen und Hilfsfunktionen verwendet werden, die nur für die Initialisierung relevant sind:</span><span class="sxs-lookup"><span data-stu-id="1aaad-178">This could be used for temporary variables and helper functions that are only relevant to initialization:</span></span>

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

<span data-ttu-id="1aaad-179">Dies ist möglicherweise zu gering, besonders weil das Fehlen von zugriffsmodifizierermodifiziererzugriffsmodi`private`gern</span><span class="sxs-lookup"><span data-stu-id="1aaad-179">This may be too subtle, especially since the absence of accessibility modifiers elsewhere simply means `private`.</span></span> 

### <a name="initializer-statements"></a><span data-ttu-id="1aaad-180">Initialisiereranweisungen</span><span class="sxs-lookup"><span data-stu-id="1aaad-180">Initializer statements</span></span>

<span data-ttu-id="1aaad-181">Eine radikale Kombination der obigen Erweiterungen zu Erweiterungen besteht darin,-Anweisungen einfach direkt im Klassen Text zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="1aaad-181">A radical combination of the above to extensions would be to simply allow statements directly in the class body.</span></span> <span data-ttu-id="1aaad-182">Diese Anweisungen sind genau wie die oben vorgeschlagenen überlappenden konstruktortexte, mit dem Unterschied, dass Sie nicht in `{ }`eingeschlossen werden müssen.</span><span class="sxs-lookup"><span data-stu-id="1aaad-182">Such statements are exactly as the interspersed constructor bodies proposed above, except they don't need to be enclosed in `{ }`.</span></span> <span data-ttu-id="1aaad-183">Damit dies ausreichend nützlich ist, müssen "local"-Variablen und Hilfsfunktionen auf der obersten Ebene der Klasse auf die gleiche Weise wie in der Erweiterungs Ansicht "Initialisierer-Felder und initialisiererfunktionen" untersucht werden.</span><span class="sxs-lookup"><span data-stu-id="1aaad-183">For this to be sufficiently useful, "local" variables and helper functions would need to also be expressible at the top level of the class, in the manner explored in the extension "Initializer fields and initializer functions" above.</span></span>


### <a name="member-access"></a><span data-ttu-id="1aaad-184">Memberzugriff</span><span class="sxs-lookup"><span data-stu-id="1aaad-184">Member access</span></span>

<span data-ttu-id="1aaad-185">Der Kernvorschlag behandelt primäre Konstruktorparameter als Parameter, die nur als einfache Namen bezeichnet werden können.</span><span class="sxs-lookup"><span data-stu-id="1aaad-185">The core proposal treats primary constructor parameters as parameters that can only be referred as simple names.</span></span> <span data-ttu-id="1aaad-186">Eine Alternative besteht darin, dass auf Sie verwiesen wird, als wären Sie private Felder, d. h. mit einem Element Zugriff, *auch* Wenn Sie manchmal nicht als Felder generiert werden.</span><span class="sxs-lookup"><span data-stu-id="1aaad-186">An alternative is to allow them to be referenced as if they were private fields, i.e. with a member access, *even* if they are sometimes not generated as fields.</span></span> <span data-ttu-id="1aaad-187">Auf diese Weise können Sie beim Shadowing durch lokale Variablen als `this.x` referenziert werden, und der Zugriff auf eine andere Instanz erfolgt als `other.x`.</span><span class="sxs-lookup"><span data-stu-id="1aaad-187">This would allow them to be referenced as `this.x` when shadowed by local variables, and accessed from a different instance as `other.x`.</span></span>

<span data-ttu-id="1aaad-188">Wenn Sie auf die Erweiterung "initialisiererfelder und initialisiererfunktionen" angewendet wird, würde dies auch dazu geführt, dass sich diese von normalen privaten Membern unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="1aaad-188">If applied to the "initializer fields and initializer functions" extension this would also reduce the degree to which those were different from ordinary private members.</span></span> <span data-ttu-id="1aaad-189">Der einzige Unterschied besteht darin, dass der Compiler Sie aus dem-Objekt entfernen kann, wenn es nur während der Initialisierung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="1aaad-189">The only difference would then be that the compiler is free to elide them from the object if only used during initialization.</span></span>

