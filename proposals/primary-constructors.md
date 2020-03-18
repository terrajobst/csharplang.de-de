---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "79483901"
---
# <a name="primary-constructors"></a>Primäre Konstruktoren

* [x] vorgeschlagen
* [] Prototyp: nicht gestartet
* [] Implementierung: nicht gestartet
* [] Spezifikation: nicht gestartet

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Klassen können über eine Parameterliste verfügen, und wenn Sie dies tun, kann Ihre Basisklassen Spezifikation eine Argumentliste haben.
Primäre Konstruktorparameter befinden sich innerhalb der Klassen Deklaration im Gültigkeitsbereich. Wenn Sie von einem Funktionsmember oder einer anonymen Funktion aufgezeichnet werden, werden Sie als private Felder in der-Klasse gespeichert.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Es kommt häufig vor, dass im Programm Initialisierungs Code viele Bausteine vorhanden sind. Im Allgemeinen wird eine bestimmte Daten `x` mehrmals erwähnt:

- Als privates Feld `_x`
- Als Parameter `x` einem Konstruktor
- In einer Zuweisungs `_x = x;` des Felds aus dem-Parameter im-Konstruktor
- Als Eigenschaften `X`
- Im Eigenschaften Setter `x = value;`
- Im Eigenschaften Getter `return x;`

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

Bei Eigenschaften, für die keine Validierung oder Berechnung erforderlich ist, kann das aufwändigen Schritte mithilfe von automatischen Eigenschaften reduziert werden. dadurch ist es nicht mehr erforderlich, ein explizites Unterstützungs Feld für die Eigenschaft zu deklarieren. Wenn Ihre Eigenschaft jedoch eine beliebige Logik erfordert, die über das Bereitstellen einer Auto-Eigenschaft hinausgeht, ist das oben genannte das beste, was Sie tun.

Primäre Konstruktoren verringern stattdessen den mehr Aufwand durch das direkte Einfügen von Konstruktorargumenten in den Gültigkeitsbereich der gesamten Klasse, wodurch die Notwendigkeit besteht, ein dahinter liegendes Feld explizit zu deklarieren. Daher würde das obige Beispiel lauten:

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

In diesem Beispiel reduziert der primäre Konstruktor die Anzahl der benannten Entitäten für die `x` von drei auf zwei, wobei das `_x` dahinter liegende Feld vermieden wird. Es entfernt zwei aus drei Element Deklarationen (wobei nur die Eigenschafts Deklaration selbst beibehalten wird) und reduziert die Gesamtzahl der Erwähnungen von `x`/`_x`/`X` von 8 bis 5.


## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Klassen können eine Parameterliste aufweisen:

``` c#
public class C(int i, string s)
{
    ...
}
```

Die Parameterliste bewirkt, dass ein Konstruktor implizit für die Klasse deklariert wird, mit der gleichen Barrierefreiheit wie die Klasse selbst.

``` c#
new C(5, "Hello");
```

Primäre Konstruktorparameter befinden sich im Gültigkeitsbereich des gesamten Klassen Texts. Wenn Sie von einem Funktionsmember oder einer anonymen Funktion aufgezeichnet werden, werden Sie als private Felder in der-Klasse gespeichert. Wenn Sie nur während der Initialisierung verwendet werden, werden Sie nicht im-Objekt gespeichert.

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

Wenn eine Klasse mit einem primären Konstruktor über eine Basisklassen Spezifikation verfügt, kann diese eine Argumentliste haben. Dies dient als Argumentliste für einen `base(...)` Initialisierer des implizit deklarierten Konstruktors. Wenn keine Argumentliste bereitgestellt wird, wird ein leerer Wert angenommen.

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
Die Klasse kann auch explizit definierte Konstruktoren aufweisen, aber alle müssen einen `this(...)` Initialisierer verwenden. Dadurch wird sichergestellt, dass der primäre Konstruktor immer aufgerufen wird, wenn eine neue Instanz erstellt wird.

Alle Initialisierer im Klassen Text werden zu Zuweisungen im generierten Konstruktor. Dies bedeutet, dass Initialisierer im Gegensatz zu anderen Klassen ausgeführt werden, *nachdem* der Basiskonstruktor aufgerufen wurde, nicht vor. Außerdem enthält die generierte-Klasse Zuweisungen, um alle privaten Felder zu initialisieren, die generiert wurden, um primäre Konstruktorparameter zu speichern, die von Membern aufgezeichnet wurden. Diese Member werden umgeschrieben, um das private-Feld anstelle des-Parameters auf eine Weise zu verwenden, die von Abschlüssen für Lambda-Ausdrücke ähnlich ist. Die generierten primären Felder werden zuerst initialisiert. Anschließend werden die initialisierergenerierten Zuweisungen in der Reihenfolge der Darstellung in der-Klasse ausgeführt.

Im obigen Beispiel ist die Auswirkung der Klassen Deklaration wie folgt umgeschrieben:

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

Die Erfassung hat ähnliche Einschränkungen wie die Erfassung lokaler Variablen durch Lambda-Ausdrücke. Beispielsweise sind `ref`-und `out` Parameter in primären Konstruktoren zulässig, können aber keine Element Körper erfasst werden.


## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

In groben Reihenfolge von Bedeutung.

* Der Vorschlag verwendet die Syntax, die auch für Datensätze mit Feldern fester Breite vorgeschlagen wurde. Wenn wir beide Features wünschen, ist eine gewisse Unterbringung erforderlich. Beispiel: Es wurde ein `data` Modifizierer für Datensätze vorgeschlagen.
* Die Zuordnungs Größe von konstruierten Objekten ist weniger offensichtlich, da der Compiler bestimmt, ob basierend auf dem vollständigen Text der Klasse ein Feld für einen primären Konstruktorparameter zugeordnet werden soll. Dieses Risiko ähnelt der impliziten Erfassung von Variablen durch Lambda-Ausdrücke.
* Eine gängige Versuchung (oder ein versehentliches Muster) könnte darin bestehen, den "gleichen" Parameter auf mehreren Vererbungs Ebenen zu erfassen, da er an die konstruktorkette übergeben wird, anstatt ihm explizit ein geschütztes Feld in der Basisklasse zuzuordnen, was zu doppelten Zuordnungen führt. für dieselben Daten in-Objekten. Dies ähnelt dem heutigen Risiko, automatische Eigenschaften mit automatischen Eigenschaften zu überschreiben. 
* Wie oben bereits erwähnt, gibt es keinen Ort für zusätzliche Logik, die in der Regel in konstruktortexten ausgedrückt wird. Die Erweiterung "primäre konstruktortexte" unten behandelt die.
* Wie bereits vorgeschlagen, unterscheidet sich die Semantik der Ausführungsreihenfolge von den normalen Konstruktoren. Dies kann wahrscheinlich behoben werden, aber auf Kosten einiger Erweiterungs Vorschläge (insbesondere "primäre konstruktortexte").
* Der Vorschlag funktioniert nur, wenn ein einzelner Konstruktor als primär festgelegt werden kann.
* Es gibt keine Möglichkeit, unterschiedliche Zugriffsmöglichkeiten für die-Klasse und den primären Konstruktor zu haben. Beispielsweise in Situationen, in denen öffentliche Konstruktoren alle an einen privaten Build-it-all-Konstruktor delegieren, der benötigt wird. Bei Bedarf könnte die Syntax dafür später vorgeschlagen werden.


## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Vollständige Datensätze mit Feldern fester Breite können eine Alternative sein oder je nach Besonderheiten mit primären Konstruktoren koexistieren. Sie würden in einer *kleineren* Anzahl von Szenarien eine *genauere* Abkürzung ermöglichen. Beide sind möglicherweise nützlich, aber beide können außer Kraft gesetzt werden, es sei denn, Sie können in einander nahtlos integriert werden.


## <a name="possible-extensions"></a>Mögliche Erweiterungen
[extensions]: #possible-extensions

Dabei handelt es sich um Variationen oder Ergänzungen des Kern Angebots, die möglicherweise in Verbindung mit der Anwendung oder zu einem späteren Zeitpunkt als nützlich angesehen werden.

### <a name="primary-constructor-bodies"></a>Primäre konstruktortexte

Konstruktoren selbst enthalten häufig Parameter Validierungs Logik oder anderen nicht trivialen Initialisierungs Code, der nicht als Initialisierer ausgedrückt werden kann.

Primäre Konstruktoren können so erweitert werden, dass Anweisungsblöcke direkt im Klassen Text angezeigt werden. Diese Anweisungen werden im generierten Konstruktor an dem Punkt eingefügt, an dem Sie zwischen den Initialisierungs Zuweisungen angezeigt werden, und werden daher mit Initialisierern zusammengeführt. Beispiel:

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

### <a name="initializer-fields-and-initializer-functions"></a>Initialisiererfelder und initialisiererfunktionen

In einer Klasse mit einem primären Konstruktor könnten wir Feld-und Methoden Deklarationen ohne Zugriffsmodifizierer als lokale Variablen und lokale Funktionen in Erwägung gezogen werden:

* Ebenso wie primäre Konstruktorparameter werden die initialisiererfelder nur in einem tatsächlichen privaten Feld aufgezeichnet, wenn Sie in Funktionsmembern verwendet wurden.
* Die initialisiererfunktionen werden nur für die Erfassung primärer Konstruktorparameter und initialisiererfelder in Erwägung gezogen, wenn Sie selbst in anderen Funktionsmembern verwendet wurden. Wenn Sie nicht erfasst werden, können Sie in einer optimalen Weise generiert werden, wie z. b. lokale Funktionen.
* Ebenso wie primäre Konstruktorparameter sind Sie nicht über den Member-Zugriff verfügbar, sondern nur als einfacher Name.

Dies kann für temporäre Variablen und Hilfsfunktionen verwendet werden, die nur für die Initialisierung relevant sind:

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

Dies ist möglicherweise zu gering, besonders weil das Fehlen von zugriffsmodifizierermodifiziererzugriffsmodi`private`gern 

### <a name="initializer-statements"></a>Initialisiereranweisungen

Eine radikale Kombination der obigen Erweiterungen zu Erweiterungen besteht darin,-Anweisungen einfach direkt im Klassen Text zuzulassen. Diese Anweisungen sind genau wie die oben vorgeschlagenen überlappenden konstruktortexte, mit dem Unterschied, dass Sie nicht in `{ }`eingeschlossen werden müssen. Damit dies ausreichend nützlich ist, müssen "local"-Variablen und Hilfsfunktionen auf der obersten Ebene der Klasse auf die gleiche Weise wie in der Erweiterungs Ansicht "Initialisierer-Felder und initialisiererfunktionen" untersucht werden.


### <a name="member-access"></a>Memberzugriff

Der Kernvorschlag behandelt primäre Konstruktorparameter als Parameter, die nur als einfache Namen bezeichnet werden können. Eine Alternative besteht darin, dass auf Sie verwiesen wird, als wären Sie private Felder, d. h. mit einem Element Zugriff, *auch* Wenn Sie manchmal nicht als Felder generiert werden. Auf diese Weise können Sie beim Shadowing durch lokale Variablen als `this.x` referenziert werden, und der Zugriff auf eine andere Instanz erfolgt als `other.x`.

Wenn Sie auf die Erweiterung "initialisiererfelder und initialisiererfunktionen" angewendet wird, würde dies auch dazu geführt, dass sich diese von normalen privaten Membern unterscheiden. Der einzige Unterschied besteht darin, dass der Compiler Sie aus dem-Objekt entfernen kann, wenn es nur während der Initialisierung verwendet wird.

