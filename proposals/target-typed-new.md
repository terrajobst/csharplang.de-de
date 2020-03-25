---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217202"
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

- **Beliebiger Strukturtyp** (einschließlich Tupeltypen)
- **Beliebiger Verweistyp** (einschließlich Delegattypen)
- Ein **beliebiger Typparameter** mit einem Konstruktor oder einer `struct`-Einschränkung

mit den folgenden Ausnahmen:

- **Enumerationstypen:** nicht alle Enumerationstypen enthalten die Konstante NULL. Daher sollte es wünschenswert sein, den expliziten Enumerationsmember zu verwenden.
- **Schnittstellentypen:** Dies ist eine Nische Funktion, und es sollte besser sein, den Typ explizit zu erwähnen.
- **Array Typen:** Arrays benötigen eine spezielle Syntax, um die Länge bereitzustellen.
- **dynamisch:** wir lassen `new dynamic()`nicht zu, daher lassen wir `new()` mit `dynamic` nicht als Zieltyp zu.

Alle anderen Typen, die in der *object_creation_expression* nicht zulässig sind, werden ebenfalls ausgeschlossen, z.b. Zeiger Typen.

Wenn der Zieltyp ein auf NULL festleg barer Werttyp ist, wird der `new` vom Typ Target in den zugrunde liegenden Typ und nicht in den Typ konvertiert, der NULL-Werte zulässt.

> **Open Issue:** sollten wir Delegaten und Tupel als Zieltyp zulassen?

Die oben genannten Regeln enthalten Delegaten (einen Verweistyp) und Tupel (einen Strukturtyp). Obwohl beide Typen konstruiert werden können, kann die Verwendung einer anonymen Funktion oder eines tupelliterals bereits verwendet werden, wenn der Typ abgeleitet werden kann.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a>Verschiedenes

`throw new()` ist nicht zulässig.

Mit dem Ziel typisierte `new` ist bei binären Operatoren nicht zulässig.

Es ist nicht zulässig, wenn kein Typ zum Ziel vorhanden ist: unäre Operatoren, Auflistung eines `foreach`in einer `using`in einer Dekonstruktion in einem `await` Ausdruck als anonyme Typeigenschaft (`new { Prop = new() }`) in einer `lock`-Anweisung in einer `sizeof`in einer `fixed`-Anweisung in einem Element Zugriff (`new().field`) in einem dynamisch verteilten Vorgang (`someDynamic.Method(new())`) in einer LINQ-Abfrage als Operand des `is` Operators, als Linker Operand des `??`-Operators. ,  ...

Dies ist auch als `ref`unzulässig.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Es gab einige Probleme mit Ziel typisierten `new` das Erstellen neuer Kategorien von wichtigen Änderungen, aber wir haben dies bereits mit `null` und `default`, und das ist kein erhebliches Problem.

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
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
