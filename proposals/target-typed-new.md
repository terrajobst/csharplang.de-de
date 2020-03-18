---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483529"
---

# <a name="target-typed-new-expressions"></a>`new` Ausdrücke mit Ziel Typisierung

* [x] vorgeschlagen
* [x] [Prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)
* []-Implementierung
* []-Spezifikation

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Sie benötigen keine Typspezifikation für Konstruktoren, wenn der Typ bekannt ist. 

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Ermöglicht die Feld Initialisierung ohne Duplizieren des Typs.
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
Lassen Sie den Typ weglassen aus, wenn er von der Verwendung abgeleitet werden kann.
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
Instanziieren Sie ein-Objekt, ohne den Typ zu benennen.
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Die *object_creation_expression* Syntax wird so geändert, dass der *Typ* optional ist, wenn Klammern vorhanden sind. Dies ist erforderlich, um die Mehrdeutigkeit mit *anonymous_object_creation_expression*zu beheben.
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
Ein mit dem Ziel typisiertes `new` kann in einen beliebigen Typ konvertiert werden. Dies hat zur Folge, dass Sie nicht zur Überladungs Auflösung beiträgt. Dies dient vor allem dazu, unvorhersehbare wichtige Änderungen zu vermeiden.

Die Argumentliste und die initialisiererausdrücke werden gebunden, nachdem der Typ bestimmt wurde.

Der Typ des Ausdrucks wird vom Zieltyp abgeleitet, der einen der folgenden Werte benötigen würde:

- **Beliebiger Strukturtyp**
- **Beliebiger Verweistyp**
- Ein **beliebiger Typparameter** mit einem Konstruktor oder einer `struct`-Einschränkung

mit den folgenden Ausnahmen:

- **Enumerationstypen:** nicht alle Enumerationstypen enthalten die Konstante NULL. Daher sollte es wünschenswert sein, den expliziten Enumerationsmember zu verwenden.
- **Schnittstellentypen:** Dies ist eine Nische Funktion, und es sollte besser sein, den Typ explizit zu erwähnen.
- **Array Typen:** Arrays benötigen eine spezielle Syntax, um die Länge bereitzustellen.
- **Strukturstandardkonstruktor**: Hierdurch werden alle primitiven Typen und die meisten Werttypen Regeln. Wenn Sie den Standardwert dieser Typen verwenden möchten, können Sie stattdessen `default` schreiben.

Alle anderen Typen, die in der *object_creation_expression* nicht zulässig sind, werden ebenfalls ausgeschlossen, z.b. Zeiger Typen.

> **Open Issue:** sollten wir Delegaten und Tupel als Zieltyp zulassen?

Die oben genannten Regeln enthalten Delegaten (einen Verweistyp) und Tupel (einen Strukturtyp). Obwohl beide Typen konstruiert werden können, kann die Verwendung einer anonymen Funktion oder eines tupelliterals bereits verwendet werden, wenn der Typ abgeleitet werden kann.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> **Open Issue:** sollten wir `throw new()` mit `Exception` als Zieltyp zulassen?

Wir haben heute `throw null`, aber nicht `throw default` (obwohl dies die gleiche Wirkung haben würde). Auf der anderen Seite könnten `throw new()` als Kurzform für `throw new Exception(...)`nützlich sein. Beachten Sie, dass Sie bereits von der aktuellen Spezifikation zugelassen wird. `Exception` ist ein Referenztyp, und die Spezifikation für die throw-Anweisung besagt, dass der Ausdruck in `Exception`konvertiert wird.

> **Open Issue:** sollten wir die Verwendung einer Ziel typisierten `new` mit benutzerdefinierten Vergleichs-und arithmetischen Operatoren zulassen?

Zum Vergleich unterstützt `default` nur Gleichheits Operatoren (benutzerdefinierte und integrierte). Wäre es sinnvoll, auch andere Operatoren für die `new()` zu unterstützen?

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

None.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Die meisten Beschwerden über Typen, die zu lang sind, um bei der Feld Initialisierung duplizieren zu können, sind *Typargumente* , die nicht den Typ selbst aufweisen. wir könnten nur Typargumente wie `new Dictionary(...)` (oder ähnliches) ableiten und Typargumente lokal von Argumenten oder dem auflistungsiniti

## <a name="questions"></a>Fragen
[questions]: #questions

- Sollten wir die Verwendung von Ausdrücken in Ausdrucks Baumstrukturen verbieten? gar
- Wie wird das Feature mit `dynamic` Argumenten interagiert? (keine besondere Behandlung)
- Wie funktioniert IntelliSense mit `new()`? (nur bei einem einzelnen Zieltyp)
## <a name="design-meetings"></a>Treffen von Besprechungen

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
