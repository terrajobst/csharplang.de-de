---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484081"
---
# <a name="default-interface-methods"></a>Standardschnittstellen Methoden

* [x] vorgeschlagen
* [] Prototyp: [in](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md) Bearbeitung
* [] Implementierung: keine
* [] Spezifikation: in Bearbeitung

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Hinzufügen von Unterstützung für _virtuelle Erweiterungs Methoden_ -Methoden in Schnittstellen mit konkreten Implementierungen. Eine Klasse oder Struktur, die eine solche Schnittstelle implementiert, muss über eine einzige _spezifischere_ Implementierung für die Schnittstellen Methode verfügen, die entweder von der Klasse oder Struktur implementiert wird oder von ihren Basisklassen oder Schnittstellen geerbt wird. Mithilfe von virtuellen Erweiterungs Methoden kann ein API-Autor Methoden zu einer Schnittstelle in zukünftigen Versionen hinzufügen, ohne dass die Quell-oder Binärkompatibilität mit vorhandenen Implementierungen dieser Schnittstelle unterbrochen wird.

Diese ähneln den ["Standardmethoden"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)von Java.

(Basierend auf dem wahrscheinlichen Implementierungs Verfahren) erfordert dieses Feature entsprechende Unterstützung in der CLI/CLR. Programme, die dieses Feature nutzen, können in früheren Versionen der Plattform nicht ausgeführt werden.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Die Hauptgründe für dieses Feature sind

- Mithilfe von Standardschnittstellen Methoden kann ein API-Autor in zukünftigen Versionen Methoden zu einer Schnittstelle hinzufügen, ohne dass die Quell-oder Binärkompatibilität mit vorhandenen Implementierungen dieser Schnittstelle unterbrochen wird.
- Mit der- C# Funktion können Sie mit APIs für [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) und [IOS (SWIFT)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267)interagieren, die ähnliche Funktionen unterstützen.
- Es stellt sich heraus, dass das Hinzufügen von Standardschnittstellen Implementierungen die Elemente der Sprachfunktion "Merkmale" (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>) bereitstellt. Merkmale haben sich als leistungsstarkes Programmierverfahren (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>) bewährt.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Die Syntax für eine Schnittstelle wird auf zulassen erweitert.

- Member-Deklarationen, die Konstanten, Operatoren, statische Konstruktoren und die Typen von Typen deklarieren.
- ein *Text* für eine Methode oder einen Indexer, eine Eigenschaft oder einen Ereignis Accessor (d. h. eine "Default"-Implementierung);
- Member-Deklarationen, die statische Felder, Methoden, Eigenschaften, Indexer und Ereignisse deklarieren.
- Member-Deklarationen mit der expliziten Schnittstellen Implementierungs Syntax; immer
- Explizite Zugriffsmodifizierer (der Standard Zugriff ist `public`).

Member mit Text ermöglichen der-Schnittstelle, eine "Default"-Implementierung für die Methode in Klassen und Strukturen bereitzustellen, die keine über schreibende Implementierung bereitstellen.

Schnittstellen dürfen keinen Instanzstatus enthalten. Obwohl statische Felder jetzt zulässig sind, sind Instanzfelder in Schnittstellen nicht zulässig. Automatische Instanzeigenschaften werden in Schnittstellen nicht unterstützt, da Sie implizit ein ausgeblendetes Feld deklarieren würden.

Statische und private Methoden ermöglichen ein nützliches Refactoring und eine Organisation des Codes, der zum Implementieren der öffentlichen API der Schnittstelle verwendet wird.

Eine Methoden Überschreibung in einer Schnittstelle muss die explizite Schnittstellen Implementierungs Syntax verwenden.

Es ist ein Fehler, einen Klassentyp, Strukturtyp oder Enumerationstyp innerhalb des Gültigkeits Bereichs eines Typparameters zu deklarieren, der mit einem *variance_annotation*deklariert wurde.  Beispielsweise ist die Deklaration von `C` unten ein Fehler.

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a>Konkrete Methoden in Schnittstellen

Die einfachste Form dieses Features ist die Möglichkeit, eine *konkrete Methode* in einer Schnittstelle zu deklarieren, bei der es sich um eine Methode mit einem Text handelt.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

Eine Klasse, die diese Schnittstelle implementiert, muss Ihre konkrete Methode nicht implementieren.

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

Die abschließende außer Kraft setzung für `IA.M` in Class `C` ist die konkrete Methode `M` die in `IA`deklariert wurde. Beachten Sie, dass eine Klasse keine Member von ihren Schnittstellen erbt. Dies wird von dieser Funktion nicht geändert:

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

In einem Instanzmember einer Schnittstelle hat `this` den Typ der einschließenden Schnittstelle.

### <a name="modifiers-in-interfaces"></a>Modifiziererer in Schnittstellen

Die Syntax für eine Schnittstelle wird gelockert, um modifiziererer auf ihren Membern zuzulassen. Folgendes ist zulässig: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`und `partial`.

> ***TODO***: Überprüfen Sie, welche anderen modifiziererer vorhanden sind.

Ein Schnittstellenmember, dessen Deklaration einen Text enthält, ist ein `virtual` Member, es sei denn, der `sealed` oder `private` Modifizierer wird verwendet Der `virtual` Modifizierer kann auf einem Funktionsmember verwendet werden, der andernfalls implizit `virtual`wäre. Auch wenn `abstract` der Standardwert für Schnittstellenmember ohne Text ist, kann dieser Modifizierer explizit angegeben werden. Ein nicht virtuelles Member kann mit dem `sealed`-Schlüsselwort deklariert werden.

Es ist ein Fehler bei einem `private` oder `sealed` Funktionsmember einer Schnittstelle, dass kein Text vorhanden ist. Ein `private` Funktionsmember verfügt möglicherweise nicht über den-Modifizierer `sealed`.

Zugriffsmodifizierer können für Schnittstellenmember aller zulässigen Member verwendet werden. Die Zugriffsebene `public` ist die Standardeinstellung, kann aber explizit angegeben werden.

> ***Problem öffnen:*** Wir müssen die genaue Bedeutung der Zugriffsmodifizierer angeben, z. b. `protected` und `internal`. Außerdem müssen Sie angeben, welche Deklarationen Sie (in einer abgeleiteten Schnittstelle) überschreiben oder implementieren (in einer Klasse, die die-Schnittstelle implementiert).

Schnittstellen können `static` Member deklarieren, einschließlich der Typen, Methoden, Indexer, Eigenschaften, Ereignisse und statischen Konstruktoren. Die Standard Zugriffsebene für alle Schnittstellenmember ist `public`.

Schnittstellen können keine Instanzkonstruktoren, Dekonstruktoren oder Felder deklarieren.

> ***Geschlossene Probleme:*** Sollten Operator Deklarationen in einer Schnittstelle zulässig sein? Wahrscheinlich keine Konvertierungs Operatoren, aber was ist mit anderen? ***Entscheidung***: Operatoren sind *mit Ausnahme* von Konvertierungs-, Gleichheits-und Ungleichheits Operatoren zulässig.

> ***Geschlossene Probleme:*** Sollten `new` für Schnittstellenmember-Deklarationen zulässig sein, die Member von Basis Schnittstellen ausblenden? ***Entscheidung***: Ja.

> ***Geschlossene Probleme:*** Wir gestatten derzeit keine `partial` auf einer Schnittstelle oder ihren Membern. Dies erfordert einen separaten Vorschlag. ***Entscheidung***: Ja. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a>Über Schreibungen in Schnittstellen

Überschreibungs Deklarationen (d. h. solche, die den `override` Modifizierer enthalten) ermöglichen es dem Programmierer, eine spezifischere Implementierung eines virtuellen Members in einer Schnittstelle bereitzustellen, auf der der Compiler oder die Laufzeit andernfalls keinen finden würde. Außerdem ermöglicht es die Umwandlung eines abstrakten Members aus einer Super Schnittstelle in ein Standardelement in einer abgeleiteten Schnittstelle. Eine Überschreibungs Deklaration ist berechtigt, eine bestimmte Basis Schnittstellen Methode *explizit* zu überschreiben, indem Sie die Deklaration mit dem Schnittstellennamen qualifiziert (in diesem Fall ist kein Zugriffsmodifizierer zulässig). Implizite über schreibungen sind nicht zulässig.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

Überschreibungs Deklarationen in Schnittstellen können nicht als `sealed`deklariert werden.

Öffentliche `virtual` Funktionsmember in einer Schnittstelle können in einer abgeleiteten Schnittstelle explizit überschrieben werden (indem Sie den Namen in der Überschreibungs Deklaration mit dem Schnittstellentyp qualifizieren, der die Methode ursprünglich deklariert hat, und einen Zugriffsmodifizierer weggelassen).

`virtual` Funktionsmember in einer Schnittstelle können nur explizit (nicht implizit) in abgeleiteten Schnittstellen überschrieben werden, und Member, die nicht `public` sind, können nur in einer Klasse oder Struktur explizit (nicht implizit) implementiert werden. In beiden Fällen muss auf den überschriebenen oder implementierten Member zugegriffen werden können *, wo er* überschrieben wurde.

### <a name="reabstraction"></a>Neuabstraktion

Eine in einer Schnittstelle deklarierte virtuelle (konkrete) Methode kann in einer abgeleiteten Schnittstelle als abstrakt überschrieben werden.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

Der `abstract` Modifizierer ist in der Deklaration von `IB.M` nicht erforderlich (Dies ist die Standardeinstellung in-Schnittstellen), aber es ist wahrscheinlich empfehlenswert, explizit in einer Überschreibungs Deklaration zu sein.

Dies ist in abgeleiteten Schnittstellen nützlich, bei denen die Standard Implementierung einer Methode ungeeignet ist und eine geeignetere Implementierung durch Implementieren von Klassen bereitgestellt werden sollte.

> ***Problem öffnen:*** Sollte eine erneute Abstraktion zulässig sein?

### <a name="the-most-specific-override-rule"></a>Die spezifischsten Überschreibungs Regel

Wir verlangen, dass jede Schnittstelle und Klasse über eine *spezifischere außer Kraft* setzung für jedes virtuelle Element unter den außer Kraft setzungen verfügen, die im Typ oder seinen direkten und indirekten Schnittstellen angezeigt werden. Die *spezifischere außer Kraft* Setzung ist eine eindeutige außer Kraft setzung, die spezifischer ist als jede andere außer Kraft Setzung. Wenn keine außer Kraft Setzung vorhanden ist, wird der Member selbst als die spezifischsten außer Kraft Setzung betrachtet.

Eine außer Kraft Setzung `M1` gilt als *spezifischere* als eine andere außer Kraft Setzung `M2` wenn `M1` für den Typ `T1`deklariert ist, `M2` für den Typ `T2`deklariert ist, und entweder

1. `T1` enthält `T2` zu den direkten oder indirekten Schnittstellen.
2. `T2` ist ein Schnittstellentyp, aber `T1` ist kein Schnittstellentyp.

Beispiel:

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

Die spezifischere Überschreibungs Regel stellt sicher, dass ein Konflikt (d. h. eine Mehrdeutigkeit durch die rautenvererbung) explizit vom Programmierer an dem Punkt aufgelöst wird, an dem der Konflikt auftritt.

Da wir explizite abstrakte über Schreibungen in Schnittstellen unterstützen, können wir dies auch in Klassen tun.

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> ***Open Issue***: sollten explizite Schnittstellen abstrakte über Schreibungen in Klassen unterstützt werden?

Außerdem ist es ein Fehler, wenn in einer Klassen Deklaration die spezifischere außer Kraft Setzung einer Schnittstellen Methode eine abstrakte außer Kraft Setzung ist, die in einer Schnittstelle deklariert wurde. Dies ist eine vorhandene Regel, die mithilfe der neuen Terminologie neu angegeben wurde.

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

Eine in einer Schnittstelle deklarierte virtuelle Eigenschaft kann eine spezifischere außer Kraft setzung für den `get` Accessor in einer Schnittstelle und eine spezifischere außer Kraft setzung für den `set` Accessor in einer anderen Schnittstelle aufweisen. Dies wird als Verstoß gegen die *spezifischsten Überschreibungs* Regel angesehen.

### <a name="static-and-private-methods"></a>Die Methoden `static` und `private`

Da Schnittstellen nun ausführbaren Code enthalten können, ist es hilfreich, allgemeinen Code in private und statische Methoden zu abstrahieren. Diese werden jetzt in Schnittstellen zugelassen.

> ***Geschlossenes Problem***: sollen private Methoden unterstützt werden? Sollten statische Methoden unterstützt werden? **Entscheidung: Ja**

> ***Open Issue***: sollen Schnittstellen Methoden `protected` oder `internal` oder andere Zugriffsberechtigungen zugelassen werden? Wenn dies der Fall ist, wie lautet die Semantik? Sind Sie standardmäßig `virtual`? Wenn dies der Fall ist, gibt es eine Möglichkeit, Sie als nicht virtuell zu gestalten?

> ***Open Issue***: Wenn statische Methoden unterstützt werden, sollten (statische) Operatoren unterstützt werden?

### <a name="base-interface-invocations"></a>Basis Schnittstellen Aufrufe

Code in einem Typ, der von einer Schnittstelle mit einer Standardmethode abgeleitet ist, kann die "Base"-Implementierung dieser Schnittstelle explizit aufrufen.

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

Eine Instanzmethode (nicht statisch) kann die Implementierung einer zugänglichen Instanzmethode in einer direkten Basisschnittstelle nicht virtuell aufrufen, indem Sie Sie mit der Syntax `base(Type).M`benennt. Dies ist hilfreich, wenn eine außer Kraft setzung, die aufgrund der rautenvererbung bereitgestellt werden muss, durch Delegieren an eine bestimmte Basis Implementierung aufgelöst wird.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

Wenn auf eine `virtual` oder `abstract` Member mithilfe der Syntax `base(Type).M`zugegriffen wird, ist es erforderlich, dass `Type` eine eindeutige *außer Kraft* setzung für `M`enthält.

### <a name="binding-base-clauses"></a>Bindungs Basis Klauseln

Schnittstellen enthalten nun Typen.  Diese Typen können in der Base-Klausel als Basis Schnittstellen verwendet werden.  Beim Binden einer Basis Klausel müssen wir möglicherweise den Satz von Basis Schnittstellen kennen, um diese Typen zu binden (z. b. um Nachrichten zu suchen und geschützten Zugriff aufzulösen).  Die Bedeutung der Basis Klausel einer Schnittstelle wird daher zirkulär definiert.  Um den Kreis zu unterbrechen, fügen wir eine neue Sprachregel hinzu, die einer ähnlichen Regel entspricht, die für Klassen bereits vorhanden ist.

Beim Ermitteln der Bedeutung des *interface_base* einer Schnittstelle wird davon ausgegangen, dass die Basis Schnittstellen temporär leer sind. Intuitiv wird dadurch sichergestellt, dass die Bedeutung einer Basis Klausel nicht rekursiv von sich selbst abhängig ist. 

**Wir haben die folgenden Regeln verwendet:**

"Wenn eine Klasse B von einer Klasse a abgeleitet ist, ist dies ein Kompilierzeitfehler für eine, die von B abhängig ist. Eine Klasse **hängt direkt** von ihrer direkten Basisklasse (sofern vorhanden) ab und **hängt direkt** von der ~~**Klasse**~~ ab, in der Sie sofort geschachtelt ist (sofern vorhanden). Bei dieser Definition ist der gesamte Satz von ~~**Klassen**~~ , von dem eine Klasse abhängt, die reflexive und transitiv Schließung der **direkt** von der Beziehung abhängig. "

Es ist ein Kompilierzeitfehler für eine Schnittstelle, die direkt oder indirekt von sich selbst erbt.
Die **Basis Schnittstellen** einer Schnittstelle sind die expliziten Basis Schnittstellen und deren Basis Schnittstellen. Mit anderen Worten: der Satz von Basis Schnittstellen ist die komplette transitiv Schließung der expliziten Basis Schnittstellen, ihrer expliziten Basis Schnittstellen usw.

**Wir passen Sie wie folgt an:**

Wenn eine Klasse B von einer Klasse a abgeleitet ist, ist dies ein Kompilierzeitfehler für eine, die von B abhängig ist. Eine Klasse **hängt direkt** von ihrer direkten Basisklasse (sofern vorhanden) ab und **hängt direkt** von dem _**Typ**_ ab, in dem Sie sofort geschachtelt ist (sofern vorhanden).

Wenn ein Interface-IB eine Interface IA erweitert, handelt es sich um einen Kompilierzeitfehler, damit IA von IB abhängig ist. Eine Schnittstelle **hängt direkt** von ihren direkten Basis Schnittstellen (sofern vorhanden) ab und **hängt direkt** von dem Typ ab, in dem Sie sofort geschachtelt ist (sofern vorhanden).

Bei diesen Definitionen ist der gesamte Satz von **Typen** , von dem ein Typ abhängt, der reflexive und transitiv Abschluss der **direkt** von Beziehung abhängig.

### <a name="effect-on-existing-programs"></a>Auswirkung auf vorhandene Programme

Die hier vorgestellten Regeln sollen keine Auswirkung auf die Bedeutung vorhandener Programme haben.

Beispiel 1:

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

Beispiel 2:

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

Die gleichen Regeln führen zu ähnlichen Ergebnissen wie bei den Standardschnittstellen Methoden:

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> ***Geschlossenes Problem***: Vergewissern Sie sich, dass dies eine beabsichtigte Folge der Spezifikation ist. **Entscheidung: Ja**

### <a name="runtime-method-resolution"></a>Auflösung der Lauf Zeit Methode

> ***Geschlossene Probleme:*** Die Spezifikation sollte den Algorithmus zur Auflösung der Lauf Zeit Methode im Gesicht der Standardmethoden der Schnittstelle beschreiben. Wir müssen sicherstellen, dass die Semantik mit der sprach Semantik konsistent ist, z. b. welche deklarierten Methoden dies tun und keine `internal` Methode überschreiben oder implementieren.

### <a name="clr-support-api"></a>CLR-Support-API

Damit Compiler erkennen können, dass Sie für eine Laufzeit kompiliert werden, die diese Funktion unterstützt, werden Bibliotheken für diese Laufzeiten so geändert, dass diese Fakten über die in <https://github.com/dotnet/corefx/issues/17116>erörterte API angekündigt werden. Wir fügen

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> ***Open Issue***: ist der beste Name für die *CLR* -Funktion? Die CLR-Funktion ist viel mehr als nur das (z. b. lockern von Schutz Einschränkungen, unterstützt über Schreibungen in Schnittstellen usw.). Vielleicht sollte Sie z. b. "konkrete Methoden in Schnittstellen" oder "Merkmale" genannt werden?

### <a name="further-areas-to-be-specified"></a>Weitere Bereiche, die angegeben werden sollen

- [] Es ist hilfreich, die Arten von Quell-und Binär Kompatibilitäts Effekten zu katalogisieren, die durch das Hinzufügen von Standardschnittstellen Methoden und über schreibungen zu vorhandenen Schnittstellen verursacht werden.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Dieser Vorschlag erfordert ein koordiniertes Update der CLR-Spezifikation (zur Unterstützung konkreter Methoden in Schnittstellen und Methoden Auflösung). Es ist daher ziemlich "teuer", und es kann sinnvoll sein, in Kombination mit anderen Features, die wir erwarten, CLR-Änderungen zu erfordern.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

None.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

- Offene Fragen werden im obigen Vorschlag genannt.
- Eine Liste der offenen Fragen finden Sie unter <https://github.com/dotnet/csharplang/issues/406>.
- Die ausführliche Spezifikation muss den Auflösungs Mechanismus beschreiben, der zur Laufzeit verwendet wird, um die genaue Methode auszuwählen, die aufgerufen werden soll.
- Die Interaktion von Metadaten, die von neuen Compilern erzeugt und von älteren Compilern genutzt werden, muss ausführlich verarbeitet werden. Beispielsweise müssen wir sicherstellen, dass die von uns verwendete Metadatendarstellung nicht bewirkt, dass eine Standard Implementierung in einer Schnittstelle hinzugefügt wird, um eine vorhandene Klasse zu unterbrechen, die diese Schnittstelle implementiert, wenn Sie von einem älteren Compiler kompiliert wird. Dies kann sich auf die Metadatendarstellung auswirken, die wir verwenden können.
- Der Entwurf muss die Interoperation mit anderen Sprachen und vorhandenen Compilern für andere Sprachen in Erwägung gezogen werden.

## <a name="resolved-questions"></a>Aufgelöste Fragen

### <a name="abstract-override"></a>Abstrakte außer Kraft Setzung

Die vorherige Entwurfs Spezifikation enthielt die Möglichkeit, eine geerbte Methode "reabstract" zu erstellen:

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

Meine Notizen für 2017-03-20 haben ergeben, dass wir dies nicht zulassen. Hierfür gibt es jedoch mindestens zwei Anwendungsfälle:

1. Die Java-APIs, mit denen einige Benutzer dieses Features zusammenarbeiten, sind von dieser Funktion abhängig.
2. Die Programmierung mit *Merkmalen* profitiert davon. Reabstraktion ist eines der Elemente der Sprachfunktion "Merkmale" (https://en.wikipedia.org/wiki/Trait_(computer_programming)). Die folgenden Klassen sind zulässig:

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

Leider kann dieser Code nicht als Satz von Schnittstellen (Merkmalen) umgestaltet werden, es sei denn, dies ist zulässig. Das *Jared-Prinzip der Gier*sollte zugelassen werden.

> ***Geschlossene Probleme:*** Sollte eine erneute Abstraktion zulässig sein? Zwar Meine Notizen waren falsch. Die [LDM bemerkt](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) , dass eine erneute Abstraktion in einer Schnittstelle zulässig ist. Nicht in einer Klasse.

### <a name="virtual-modifier-vs-sealed-modifier"></a>Virtueller Modifizierer vs sealed-Modifizierer

Aus [Aleksey tsingauz](https://github.com/AlekseyTs):

> Es wurde beschlossen, Modifizierer explizit für Schnittstellenmember anzugeben, es sei denn, es gibt einen Grund dafür, einige davon zu unterbinden. Dies bringt eine interessante Frage in Bezug auf den virtuellen Modifizierer. Sollte es für Member mit Standard Implementierung erforderlich sein?
>
> Wir könnten Folgendes sagen:
>
> - Wenn keine Implementierung vorhanden ist und weder Virtual noch Sealed angegeben sind, wird davon ausgegangen, dass der Member abstrakt ist.
> - Wenn eine Implementierung vorhanden ist und weder abstract noch Sealed angegeben sind, wird davon ausgegangen, dass der Member virtuell ist.
> - der sealed-Modifizierer ist erforderlich, um eine Methode weder virtuell noch abstrakt zu machen.
>
> Wir könnten auch sagen, dass der virtuelle Modifizierer für ein virtuelles Element erforderlich ist. Wenn also ein Member vorhanden ist, dessen Implementierung nicht explizit mit dem virtuellen Modifizierer markiert ist, ist es weder virtuell noch abstrakt. Diese Vorgehensweise bietet möglicherweise eine bessere Leistung, wenn eine Methode von einer Klasse zu einer Schnittstelle verschoben wird:
>
> - eine abstrakte Methode bleibt abstrakt.
> - eine virtuelle Methode bleibt virtuell.
> - eine Methode ohne einen Modifizierer bleibt weder virtuell noch abstrakt.
> - der sealed-Modifizierer kann nicht auf eine Methode angewendet werden, die keine außer Kraft Setzung ist.
>
> Was denkst du?

> ***Geschlossene Probleme:*** Sollte eine konkrete Methode (mit Implementierung) implizit `virtual`werden? Zwar

***Entscheidungen:*** Erstellt in LDM 2017-04-05:

1. nicht virtuell sollte explizit durch `sealed` oder `private`ausgedrückt werden.
2. `sealed` ist das Schlüsselwort, um schnittstelleninstanzmember mit nicht virtuellen Text Schnittstellen zu erstellen.
3. Wir möchten alle modifiziererer in Schnittstellen zulassen.  
4. Der Standard Zugriff für Schnittstellenmember ist öffentlich, einschließlich der Untertypen
5. Private Funktionsmember in Schnittstellen sind implizit versiegelt, und `sealed` ist für Sie nicht zulässig.
6. Private Klassen (in Schnittstellen) sind zulässig und können versiegelt werden, und das bedeutet versiegelt in der Klassen Sense Sealed.
7. Wenn kein gutes Angebot vorhanden ist, ist partiell bei Schnittstellen oder ihren Membern weiterhin nicht zulässig.

### <a name="binary-compatibility-1"></a>Binäre Kompatibilität 1

Wenn eine Bibliothek eine Standard Implementierung bereitstellt

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

Wir wissen, dass die Implementierung von `I1.M` in `C` `I1.M`ist. Was geschieht, wenn die Assembly, die `I2` enthält, wie folgt geändert und neu kompiliert wird.

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

`C` wird jedoch nicht neu kompiliert. Was geschieht, wenn das Programm ausgeführt wird? Ein Aufruf von `(C as I1).M()`

1. Führt `I1.M`
2. Führt `I2.M`
3. Löst einen Laufzeitfehler aus.

***Entscheidung:*** 2017-04-11: führt `I2.M`aus, bei dem es sich um die eindeutig spezifischere außer Kraft Setzung zur Laufzeit handelt.

### <a name="event-accessors-closed"></a>Ereignisaccessoren (geschlossen)

> ***Geschlossene Probleme:*** Kann ein Ereignis "schrittweise" überschrieben werden?

Beachten Sie diesen Fall:

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

Diese "partielle" Implementierung des Ereignisses ist unzulässig, da die Syntax für eine Ereignis Deklaration, wie in einer Klasse, nicht nur einen Accessor zulässt. Beide (oder keine) müssen angegeben werden. Sie können das gleiche erreichen, indem Sie zulassen, dass der abstrakte remove-Accessor in der Syntax implizit abstrakt ist, wenn kein Text vorhanden ist:

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

Beachten Sie, dass *es sich hierbei um eine neue (vorgeschlagene) Syntax handelt*. In der aktuellen Grammatik haben Ereignisaccessoren einen obligatorischen Text.

> ***Geschlossene Probleme:*** Kann ein Ereignis Accessor (implizit) durch das Weglassen eines Texts abstrahiert werden, ähnlich der Art, wie Methoden in Schnittstellen und Eigenschaftenaccessoren (implizit) durch das Weglassen eines Texts abstrakt sind?

***Entscheidung:*** (2017-04-18) Nein, Ereignis Deklarationen erfordern sowohl konkrete Accessoren (oder keines von beiden).

### <a name="reabstraction-in-a-class-closed"></a>Neuabstraktion in einer Klasse (geschlossen)

***Geschlossene Probleme:*** Wir sollten bestätigen, dass dies zulässig ist (Andernfalls wäre das Hinzufügen einer Standard Implementierung eine Breaking Change):

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

***Entscheidung:*** (2017-04-18) ja, das Hinzufügen eines Texts zu einer Schnittstellenmember-Deklaration sollte C nicht unterbrechen.

### <a name="sealed-override-closed"></a>Versiegelte außer Kraft Setzung (geschlossen)

Die vorherige Frage geht implizit davon aus, dass der `sealed` Modifizierer auf eine `override` in einer Schnittstelle angewendet werden kann. Dies widerspricht der Entwurfs Spezifikation. Möchten wir das Versiegeln einer außer Kraft Setzung zulassen? Die Auswirkungen auf die Quelle und die binäre Kompatibilität der Versiegelung sollten berücksichtigt werden.

> ***Geschlossene Probleme:*** Sollten wir das Versiegeln einer außer Kraft Setzung zulassen?

***Entscheidung:*** (2017-04-18) erlauben Sie keine `sealed` für außer Kraft setzungen in Schnittstellen. Die einzige Verwendung von `sealed` für Schnittstellenmember ist, dass Sie in ihrer ursprünglichen Deklaration nicht virtuell sind.

### <a name="diamond-inheritance-and-classes-closed"></a>Rautenvererbung und-Klassen (geschlossen)

Der Entwurf des Angebots bevorzugt die Außerkraftsetzungs Überschreibungen von Klassen in rautenvererbungs Szenarien:

> Wir verlangen, dass jede Schnittstelle und Klasse eine *spezifischere außer Kraft* setzung für jede Schnittstellen Methode unter den außer Kraft setzungen aufweisen, die im Typ oder den direkten und indirekten Schnittstellen angezeigt werden. Die *spezifischere außer Kraft* Setzung ist eine eindeutige außer Kraft setzung, die spezifischer ist als jede andere außer Kraft Setzung. Wenn keine außer Kraft Setzung vorhanden ist, wird die Methode selbst als die spezifischere außer Kraft Setzung betrachtet.
>
> Eine außer Kraft Setzung `M1` gilt als *spezifischere* als eine andere außer Kraft Setzung `M2` wenn `M1` für den Typ `T1`deklariert ist, `M2` für den Typ `T2`deklariert ist, und entweder
>
> 1. `T1` enthält `T2` zu den direkten oder indirekten Schnittstellen.
> 2. `T2` ist ein Schnittstellentyp, aber `T1` ist kein Schnittstellentyp.

Das Szenario ist

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

Wir sollten dieses Verhalten bestätigen (oder andernfalls festlegen).

> ***Geschlossene Probleme:*** Bestätigen Sie die oben beschriebene Entwurfs Spezifikation für *die spezifischsten außer Kraft* setzung, da Sie auf gemischte Klassen und Schnittstellen angewendet wird (eine Klasse hat Vorrang vor einer Schnittstelle). Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.

### <a name="interface-methods-vs-structs-closed"></a>Schnittstellen Methoden im Vergleich zu Strukturen (geschlossen)

Es gibt einige unglückliche Interaktionen zwischen Standardschnittstellen Methoden und Strukturen.

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

Beachten Sie, dass Schnittstellenmember nicht geerbt werden:

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

Folglich muss der Client die Struktur zum Aufrufen von Schnittstellen Methoden in Box Box

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

Wenn Sie auf diese Weise Boxing, werden die Hauptvorteile eines `struct` Typs zunichte. Außerdem haben alle mutations Methoden keinen offensichtlichen Effekt, da Sie auf einer geachtelten *Kopie* der Struktur ausgeführt werden:

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> ***Geschlossene Probleme:*** Dazu können Sie Folgendes tun:
>
> 1. Verbieten, dass eine `struct` eine Standard Implementierung erbt. Alle Schnittstellen Methoden werden in einem `struct`als abstrakt behandelt. Dann können wir später einige Zeit in Anspruch nehmen, um die Arbeit zu verbessern.
> 2. Entwickeln Sie eine Art Code Generierungs Strategie, die Boxing vermeidet. Innerhalb einer Methode wie `IB.Increment`wäre der Typ des `this` vielleicht vergleichbar mit einem Typparameter, der auf `IB`eingeschränkt ist. Um das Boxing im Aufrufer zu vermeiden, werden nicht abstrakte Methoden in Verbindung mit den Schnittstellen geerbt. Dadurch kann der Compiler und die CLR-Implementierung erheblich zunehmen.
> 3. Machen Sie sich keine Gedanken um die IT, und lassen Sie Sie einfach als wart.
> 4. Weitere Ideen?

***Entscheidung:*** Machen Sie sich keine Gedanken um die IT, und lassen Sie Sie einfach als wart. Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.

### <a name="base-interface-invocations-closed"></a>Basis Schnittstellen Aufrufe (geschlossen)

Die Entwurfs Spezifikation schlägt eine Syntax für Basis Schnittstellen Aufrufe vor, die von Java inspiriert sind: `Interface.base.M()`. Wir müssen eine Syntax auswählen, zumindest für den anfänglichen Prototyp. Mein Favorit ist `base<Interface>.M()`.

> ***Geschlossene Probleme:*** Was ist die Syntax für einen Basismember-Aufruf?

***Entscheidung:*** Die Syntax ist `base(Interface).M()`. Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>. Die Schnittstelle, die so benannt ist, muss eine Basisschnittstelle sein, es muss sich jedoch nicht um eine direkte Basisschnittstelle handeln.

> ***Problem öffnen:*** Sollten Basis Schnittstellen Aufrufe in Klassenmembern zulässig sein?

***Entscheidung***: Ja. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a>Überschreiben von nicht öffentlichen Schnittstellenmembern (geschlossen)

In einer Schnittstelle werden nicht öffentliche Member von Basis Schnittstellen mithilfe des `override` Modifizierers überschrieben. Wenn es sich um eine explizite außer Kraft Setzung handelt, die die Schnittstelle mit dem Member benennt, wird der Zugriffsmodifizierer ausgelassen.

> ***Geschlossene Probleme:*** Muss der Zugriffsmodifizierer abgeglichen werden, wenn es sich um eine "implizite" außer Kraft Setzung handelt, die der Schnittstelle nicht entspricht?

***Entscheidung:*** Nur öffentliche Member können implizit überschrieben werden, und der Zugriff muss entsprechend erfolgen. Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.

> ***Problem öffnen:*** Ist der Zugriffsmodifizierer für eine explizite außer Kraft Setzung erforderlich, optional oder ausgelassen, z. b. `override void IB.M() {}`?

> ***Problem öffnen:*** Ist `override` erforderlich, optional oder bei einer expliziten außer Kraft setzung, z. b. `void IB.M() {}`, ausgelassen?

Wie implementiert ein nicht öffentliches Schnittstellenmember in einer Klasse? Sie müssen möglicherweise explizit ausgeführt werden?

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> ***Geschlossene Probleme:*** Wie implementiert ein nicht öffentliches Schnittstellenmember in einer Klasse?

***Entscheidung:*** Nicht öffentliche Schnittstellenmember können nur explizit implementiert werden. Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.

***Entscheidung***: für Schnittstellenmember ist kein `override` Schlüsselwort zulässig. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a>Binäre Kompatibilität 2 (geschlossen)

Beachten Sie den folgenden Code, in dem sich die einzelnen Typen in einer separaten Assembly befinden.

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

Wir wissen, dass die Implementierung von `I1.M` in `C` `I2.M`ist. Was geschieht, wenn die Assembly, die `I3` enthält, wie folgt geändert und neu kompiliert wird.

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

`C` wird jedoch nicht neu kompiliert. Was geschieht, wenn das Programm ausgeführt wird? Ein Aufruf von `(C as I1).M()`

1. Führt `I1.M`
2. Führt `I2.M`
3. Führt `I3.M`
4. Entweder 2 oder 3, deterministisch
5. Löst eine Art von Lauf Zeit Ausnahme aus.

***Entscheidung***: lösen Sie eine Ausnahme aus (5). Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.

### <a name="permit-partial-in-interface-closed"></a>`partial` in der Schnittstelle zulassen? geschlossen

Da Schnittstellen in ähnlicher Weise wie abstrakte Klassen verwendet werden können, kann es sinnvoll sein, Sie `partial`zu deklarieren. Dies wäre besonders nützlich bei Generatoren.

> ***Angebot:*** Entfernen Sie die sprach Einschränkung, dass Schnittstellen und Member von Schnittstellen möglicherweise nicht als `partial`deklariert werden.

***Entscheidung***: Ja. Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.

### <a name="main-in-an-interface-closed"></a>`Main` in einer Schnittstelle? geschlossen

> ***Problem öffnen:*** Ist eine `static Main` Methode in einer Schnittstelle ein Kandidat für den Einstiegspunkt des Programms?

***Entscheidung***: Ja. Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a>Bestätigen, dass öffentliche, nicht virtuelle Methoden unterstützt werden sollen (geschlossen)

Können wir unsere Entscheidung bestätigen (oder umkehren), um nicht virtuelle öffentliche Methoden in einer Schnittstelle zuzulassen?

```csharp
interface IA
{
    public sealed void M() { }
}
```

> ***Semiclosed-Problem:*** (2017-04-18) Wir sind davon überzeugt, dass es nützlich sein wird, es wird jedoch zurückgegeben. Dabei handelt es sich um einen Block für das Muster des geistigen Modells.

***Entscheidung***: Ja. [https://login.microsoftonline.com/consumers/](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>).

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a>Führt ein `override` in einer Schnittstelle ein neues Mitglied ein? geschlossen

Es gibt mehrere Möglichkeiten, um zu überprüfen, ob eine Überschreibungs Deklaration einen neuen Member einführt.

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> ***Problem öffnen:*** Führt eine Überschreibungs Deklaration in einer Schnittstelle einen neuen Member ein? geschlossen

In einer Klasse ist eine über schreibende Methode in gewisser Hinsicht "sichtbar". Die Namen der Parameter haben beispielsweise Vorrang vor den Namen von Parametern in der überschriebenen Methode. Möglicherweise ist es möglich, dieses Verhalten in Schnittstellen zu duplizieren, da immer eine spezifischere außer Kraft Setzung vorliegt. Möchten Sie dieses Verhalten jedoch duplizieren?

Außerdem ist es möglich, eine Überschreibungs Methode außer Kraft zu setzen? Fraglich

***Entscheidung***: für Schnittstellenmember ist kein `override` Schlüsselwort zulässig. [https://login.microsoftonline.com/consumers/](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>).

### <a name="properties-with-a-private-accessor-closed"></a>Eigenschaften mit einem privaten Accessor (geschlossen)

Wir sagen, dass private Member nicht virtuell sind und die Kombination aus Virtual und private nicht zulässig ist. Aber wie sieht es mit einer Eigenschaft mit einem privaten Accessor aus?

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

Ist dies zulässig? `virtual` der `set` Accessor oder nicht? Kann sie überschrieben werden, wo Sie zugänglich ist? Implementiert Folgendes implizit nur den `get` Accessor?

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

Der folgende Fehler ist vermutlich ein Fehler, weil IA vorliegt. P. Set ist nicht virtuell und auch weil nicht darauf zugegriffen werden kann?

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

***Entscheidung***: das erste Beispiel ist gültig, während das letzte nicht. Dies wird analog zur Funktionsweise in C#gelöst. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a>Basis Schnittstellen Aufrufe, Round 2 (geschlossen)

Unsere vorherige "Lösung" zur Behandlung von grundlegenden aufrufen bietet keine ausreichende Ausdruckskraft. Es stellt sich heraus, C# dass Sie in und der CLR im Gegensatz zu Java sowohl die-Schnittstelle, die die Methoden Deklaration enthält, als auch den Speicherort der Implementierung angeben müssen, die Sie aufrufen möchten.

Ich schlage die folgende Syntax für Basis Aufrufe in Schnittstellen vor. Ich bin nicht in der Liebe, aber es wird veranschaulicht, welche Syntax in der Lage sein muss, folgendes auszudrücken:

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

Wenn keine Mehrdeutigkeit vorliegt, können Sie Sie einfacher schreiben.

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

Oder

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

Oder

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

***Entscheidung***: entscheiden Sie sich für `base(N.I1<T>).M(s)`, und wenn wir eine Aufruf Bindung haben, können Sie später möglicherweise Probleme finden. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a>Warnung für die Struktur, die die Standardmethode nicht implementiert? geschlossen

@vancem bestätigt, dass wir ernsthaft eine Warnung erstellen sollten, wenn eine Werttyp Deklaration eine Schnittstellen Methode nicht außer Kraft setzt, auch wenn Sie eine Implementierung dieser Methode von einer Schnittstelle erben würde. Da dies zum Boxing und zum untergraben von eingeschränkten aufrufen bewirkt.

***Entscheidung***: Dies scheint etwas, das für einen Analyzer besser geeignet ist. Es scheint auch, dass diese Warnung nicht mehr angezeigt wird, da Sie auch dann ausgelöst würde, wenn die Standardschnittstellen Methode niemals aufgerufen wird und nie Boxing auftritt. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a>Statische Schnittstellen Konstruktoren (geschlossen)

Wann werden statische Konstruktoren für die Schnittstelle ausgeführt?  Der aktuelle CLI-Entwurf schlägt vor, dass er auftritt, wenn auf die erste statische Methode oder das erste Feld zugegriffen wird. Wenn keines davon vorhanden ist, wird es möglicherweise nie ausgeführt?

[2018-10-09 das CLR-Team schlägt vor, was wir für Werttypen tun (cctor Check on Access to each instance method)]

***Entscheidung***: statische Konstruktoren werden auch für den Eintrag in Instanzmethoden ausgeführt, wenn der statische Konstruktor nicht `beforefieldinit`wurde. in diesem Fall werden statische Konstruktoren vor dem Zugriff auf das erste statische Feld ausgeführt. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a>Treffen von Besprechungen

[2017-03-08 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 Meeting "CLR Behavior for Default Interface Methods"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md) Meeting [Notes
2017-04-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) LDM Meeting Notes
[2017-04-19 LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md) Meeting Notes
2017-05-17 [LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md) Meeting [Notes
2017-05-31](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) LDM Meeting Notes
2017-06-14 LDM [ Besprechungs Hinweise](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)
