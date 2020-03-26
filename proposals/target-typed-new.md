---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281969"
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

## <a name="specification"></a>Spezifikation
[design]: #detailed-design

Eine neue syntaktische Form, *target_typed_new* des *object_creation_expression* wird akzeptiert, in der der *Typ* optional ist.

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

Ein *target_typed_new* Ausdruck weist keinen Typ auf. Es gibt jedoch eine neue *Konvertierung der Objekt Erstellung* , bei der es sich um eine implizite Konvertierung von Expression handelt, die von einer *target_typed_new* zu jedem Typ vorhanden ist.

Wenn ein Zieltyp `T`ist, ist der Typ `T0` `T`zugrunde liegende Typ, wenn `T` eine Instanz von `System.Nullable`ist. Andernfalls wird `T0` `T`. Die Bedeutung eines *target_typed_new* Ausdrucks, der in den-Typ konvertiert wird `T` entspricht der Bedeutung einer entsprechenden *object_creation_expression* , die `T0` als Typ angibt.

Es handelt sich um einen Kompilierzeitfehler, wenn eine *target_typed_new* als Operand eines unären oder binären Operators verwendet wird, oder wenn Sie verwendet wird, wenn Sie nicht einer *Objekt Erstellungs Konvertierung*unterliegt.

> **Open Issue:** sollten wir Delegaten und Tupel als Zieltyp zulassen?

Die oben genannten Regeln enthalten Delegaten (einen Verweistyp) und Tupel (einen Strukturtyp). Obwohl beide Typen konstruiert werden können, kann die Verwendung einer anonymen Funktion oder eines tupelliterals bereits verwendet werden, wenn der Typ abgeleitet werden kann.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a>Sonstiges

Die Spezifikation hat folgende Konsequenzen:

- `throw new()` ist zulässig (der Zieltyp ist `System.Exception`).
- Mit dem Ziel typisierte `new` ist bei binären Operatoren nicht zulässig.
- Es ist nicht zulässig, wenn kein Typ zum Ziel vorhanden ist: unäre Operatoren, Auflistung eines `foreach`in einer `using`in einer Dekonstruktion in einem `await` Ausdruck als anonyme Typeigenschaft (`new { Prop = new() }`) in einer `lock`-Anweisung in einer `sizeof`in einer `fixed`-Anweisung in einem Element Zugriff (`new().field`) in einem dynamisch verteilten Vorgang (`someDynamic.Method(new())`) in einer LINQ-Abfrage als Operand des `is` Operators, als Linker Operand des `??`-Operators. ,  ...
- Dies ist auch als `ref`unzulässig.
- Die folgenden Arten von Typen sind nicht als Ziele der Konvertierung zulässig.
  - **Enumerationstypen:** `new()` funktioniert (wie `new Enum()`, um den Standardwert zu verwenden), aber `new(1)` funktioniert nicht, weil Enumerationstypen keinen Konstruktor aufweisen.
  - **Schnittstellentypen:** Dies funktioniert genauso wie der entsprechende Erstellungs Ausdruck für COM-Typen.
  - **Array Typen:** Arrays benötigen eine spezielle Syntax, um die Länge bereitzustellen.    
  - **dynamisch:** wir lassen `new dynamic()`nicht zu, daher lassen wir `new()` mit `dynamic` nicht als Zieltyp zu.
  - **Tupel:** Diese haben dieselbe Bedeutung wie eine Objekt Erstellung mit dem zugrunde liegenden Typ.
  - Alle anderen Typen, die in der *object_creation_expression* nicht zulässig sind, werden ebenfalls ausgeschlossen, z.b. Zeiger Typen.   

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
- [LDM-2020-03-25](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
