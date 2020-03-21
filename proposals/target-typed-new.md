---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108368"
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

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
