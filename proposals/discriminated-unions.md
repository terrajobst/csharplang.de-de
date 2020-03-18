---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "79484033"
---

# <a name="discriminated-unions--enum-class"></a>Unterscheidungs-Unions/`enum class`

`enum class`es handelt sich um eine neue Art von Typdeklaration, die manchmal auch als Unterscheidungs-Unions bezeichnet wird, in denen jede mögliche Instanz, in der der Typ aufgeführt ist, und jede Instanz nicht überlappend ist.

Eine `enum class` wird mit der folgenden Syntax definiert:

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

Beispielsyntax:

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a>Semantik

Eine `enum class` Definition definiert einen Stammtyp, bei dem es sich um eine abstrakte Klasse mit dem gleichen Namen wie die `enum class` Deklaration handelt, sowie eine Menge von Membern, von denen jede einen Typ aufweist, der ein Untertyp des Stammtyps ist. Wenn mehrere `partial enum class` Definitionen vorhanden sind, werden alle Member als Member der enumerationsklassendefinition betrachtet. Anders als bei einer benutzerdefinierten abstrakten Klassendefinition ist der `enum class` Stammtyp standardmäßig partiell und so definiert, dass er einen standardmäßigen *privaten* Parameter losen Konstruktor hat.

Beachten Sie, dass, da der Stammtyp als partielle abstrakte Klasse definiert ist, partielle Definitionen des *Stammtyps* ebenfalls hinzugefügt werden können, wobei die standardmäßigen Syntax Formulare für einen Klassen Text zulässig sind.
Allerdings können keine Typen direkt vom Stammtyp in einer Deklaration erben, abgesehen von denjenigen, die als `enum class` Member angegeben werden. Außerdem sind für den Stammtyp keine benutzerdefinierten Konstruktoren zulässig.

Es gibt vier Arten von `enum class`-Element Deklarationen:

* Einfache Klassenmember

* komplexe Klassenmember

* Mitglieder `enum class`

* Wertemember.

### <a name="simple-class-members"></a>Einfache Klassenmember

Eine einfache Klassenmember-Deklaration definiert eine neue, in diesem Dokument absichtlich nicht definierte "Record"-Klasse mit demselben Namen. Die von der-Klasse erfasste Klasse erbt vom Stammtyp.

Im obigen Beispielcode:

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

die `enum class` Deklaration hat eine Semantik, die der folgenden Deklaration entspricht.

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a>Komplexe Klassenmember

Sie können auch eine vollständige Klassen Deklaration unterhalb einer `enum class` Deklaration Schachteln. Sie wird als eine Klasse vom Typ "fsted" behandelt. Die Syntax lässt eine beliebige Klassen Deklaration zu, Sie ist jedoch erforderlich, damit der komplexe Klassenmember von der direkt enthaltenden `enum class` Deklaration erben kann. 

### <a name="enum-class-members"></a>Mitglieder `enum class`

`enum classes` können untereinander (z. b.

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

Dies ist nahezu identisch mit der Semantik einer `enum class`der obersten Ebene, mit der Ausnahme, dass die eingefügte Enumerationsklasse einen Typ vom Typ "nsted root" definiert, und alles unterhalb der untergeordneten Enumerationstypen ist ein Untertyp des nicht dem Stammtyp der obersten Ebene.

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a>Wertemember

`enum classes` können auch Wertmember enthalten. Wertemember definieren öffentliche statische Get-only-Eigenschaften für den Stammtyp, die ebenfalls den Stammtyp zurückgeben, z. b.

```C#
enum class Color
{
    Red,
    Green
}
```

verfügt über Eigenschaften, die entsprechen

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

Die gesamte Semantik wird als Implementierungsdetail betrachtet, es wird jedoch sichergestellt, dass für jede Eigenschaft eine eindeutige Instanz zurückgegeben wird und dass bei wiederholten Aufrufen dieselbe Instanz zurückgegeben wird.


### <a name="switch-expression-and-patterns"></a>Wechseln von Ausdruck und Mustern

Es gibt einige Vorschläge für den Musterabgleich und den Switch-Ausdruck, der `enum classes`behandelt. Switch-Ausdrücke können bereits über das Variablen Muster mit Typen übereinstimmen. für Verweis Typen werden jedoch keine Satz-Zeichen im Switch-Ausdruck als "Complete" betrachtet, außer für Vergleiche mit dem statischen Argumenttyp oder einen Untertyp.

Switch-Ausdrücke würden geändert, wenn der Stammtyp eines `enum class` der statische Typ des Arguments für den Switch-Ausdruck ist und es eine Reihe von Mustern gibt, die allen Membern der Enumeration entsprechen, wird der Schalter als vollständig betrachtet.

Da Wertemember keine Konstanten sind und keine neuen statischen Typen definieren, können Sie derzeit nicht anhand eines Musters abgeglichen werden. Um dies zu ermöglichen, wird ein neues Muster mit der konstantenmustersyntax hinzugefügt, um eine Entsprechung mit `enum class`-wertmembern zuzulassen. Die Übereinstimmung ist so definiert, dass Sie nur erfolgreich ist, wenn das Argument für das Muster übereinstimmt und der Wert, der vom `enum class`-Wertmember zurückgegeben wird, gleich ist, auch wenn die Implementierung diese Überprüfung nicht ausführen muss.


## <a name="open-questions"></a>Offene Fragen

- [] Was sagt der allgemeine Typalgorithmus über `enum class` Member? Handelt es sich um einen gültigen Code?
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- [] Das Hinzufügen eines neuen Musters nur für Wertemember scheint stark. Gibt es eine allgemeinere Versions Erstellung, die sinnvoll ist?
    - [] Wertemember können nicht ordnungsgemäß einer parallelen Struktur der Klassen Erstellung zugeordnet werden.

- [] Wechselt zu einem Argument, bei dem ein `enum class` statischer Typ garantiert konstant ist.

- [] Sollte es eine Möglichkeit geben, `enum class`es im Switch-Ausdruck nicht als "Fertig" angesehen wird? Präfix mit `virtual`?

- [] Welche modifiziererer sollten bei `enum class`zulässig sein?