---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703965"
---
# <a name="enums"></a>Enumerationen

Ein ***Enumerationstyp*** ist ein eindeutiger Werttyp ([Werttypen](types.md#value-types)), der einen Satz benannter Konstanten deklariert.

Das Beispiel

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

deklariert einen Enumerationstyp mit dem Namen "`Color`" mit Membern `Red`, `Green` und `Blue`.

## <a name="enum-declarations"></a>Enumerationsdeklarationen

Eine Aufzählungs Deklaration deklariert einen neuen Aufzählungstyp. Eine Enumerationsdeklaration beginnt mit dem-Schlüsselwort `enum` und definiert den Namen, die Barrierefreiheit, den zugrunde liegenden Typ und Member der Enumeration.

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

Jeder Aufzählungstyp verfügt über einen entsprechenden ganzzahligen Typ, der als ***zugrunde liegenden Typ*** des Aufzählungs Typs bezeichnet wird. Dieser zugrunde liegende Typ muss in der Lage sein, alle Enumeratorwerte darzustellen, die in der-Enumeration definiert sind. Eine Aufzählungs Deklaration kann einen zugrunde liegenden Typ von "`byte`", "`sbyte`", "`short`", "`ushort`", "`int`", "`uint`", "`long`" oder "@no__t Beachten Sie, dass "`char`" nicht als zugrunde liegender Typ verwendet werden kann. Eine Aufzählungs Deklaration, die einen zugrunde liegenden Typ nicht explizit deklariert, hat einen zugrunde liegenden Typ `int`.

Das Beispiel

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

deklariert eine-Aufzählung mit dem zugrunde liegenden Typ `long`. Ein Entwickler kann einen zugrunde liegenden Typ von "`long`" verwenden, wie z. b., um die Verwendung von Werten zu ermöglichen, die im Bereich von `long` liegen, jedoch nicht im Bereich von `int`, oder um diese Option für die Zukunft beizubehalten.

## <a name="enum-modifiers"></a>Aufzählungs modifiziererer

Ein *enum_declaration* kann optional eine Sequenz von enumerationsmodifiziererwerten enthalten:

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

Es ist ein Kompilierzeitfehler, damit derselbe Modifizierer mehrmals in einer Enumeration-Deklaration angezeigt wird.

Die Modifizierer einer Enumerationsdeklaration haben dieselbe Bedeutung wie die einer Klassen Deklaration ([Klassenmodifizierer](classes.md#class-modifiers)). Beachten Sie jedoch, dass die Modifizierer "`abstract`" und "`sealed`" in einer Aufzählungs Deklaration nicht zulässig sind. Auffüge Zeichen können nicht abstrakt sein und dürfen keine Ableitung zulassen.

## <a name="enum-members"></a>Enumerationsmember

Der Text einer Enumerationstypdeklaration definiert NULL oder mehr Enumerationsmember, bei denen es sich um die benannten Konstanten des Enumerationstyps handelt. Es können nicht zwei Enumerationsmember denselben Namen haben.

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

Jedem Enumerationsmember ist ein konstanter Wert zugeordnet. Der Typ dieses Werts ist der zugrunde liegende Typ für die enthaltende Enumeration. Der Konstante Wert für jedes Enumerationsmember muss im Bereich des zugrunde liegenden Typs für die Enumeration liegen. Das Beispiel

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

führt zu einem Kompilierzeitfehler, da die Konstanten Werte `-1`, `-2` und `-3` nicht im Bereich des zugrunde liegenden integralen Typs `uint` liegen.

Mehrere Enumerationsmember können denselben zugeordneten Wert gemeinsam verwenden. Das Beispiel

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

zeigt eine Enumeration an, in der zwei Enumerationsmember--`Blue` und `Max`-den gleichen zugeordneten Wert aufweisen.

Der zugeordnete Wert eines Enumerationsmembers wird entweder implizit oder explizit zugewiesen. Wenn die Deklaration des Enumerationsmembers über einen *constant_expression* -Initialisierer verfügt, ist der Wert dieses konstanten Ausdrucks, der implizit in den zugrunde liegenden Typ der Enumeration konvertiert wird, der zugeordnete Wert des Enumeration-Members. Wenn die Deklaration des Enumerationsmembers keinen Initialisierer aufweist, wird der zugehörige Wert implizit wie folgt festgelegt:

*  Wenn das Enumerationsmember das erste Enumerationsmember ist, das im Enumerationstyp deklariert wurde, ist der zugeordnete Wert 0 (null).
*  Andernfalls wird der zugeordnete Wert des Enumerationsmembers abgerufen, indem der zugeordnete Wert des texthin vorangehenden Enumerationsmembers um eins erhöht wird. Dieser erweiterte Wert muss innerhalb des Bereichs von Werten liegen, der durch den zugrunde liegenden Typ dargestellt werden kann. andernfalls tritt ein Kompilierzeitfehler auf.

Das Beispiel

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

druckt die enumerationselementnamen und die zugehörigen Werte aus. Ausgabe:

```console
Red = 0
Green = 10
Blue = 11
```

aus den folgenden Gründen:

*  dem Enumerationsmember `Red` wird automatisch der Wert 0 (null) zugewiesen (da er über keinen Initialisierer verfügt und der erste Enumerationsmember ist);
*  der Enumerationsmember `Green` erhält explizit den Wert `10`;
*  und dem Enumerationsmember `Blue` wird automatisch der Wert zugewiesen, der größer ist als der Member, der ihm texthal vorangestellt ist.

Der zugeordnete Wert eines Enumerationsmembers darf nicht direkt oder indirekt den Wert seines eigenen zugeordneten Enumerationsmembers verwenden. Abgesehen von dieser zirkulareinschränkungs Einschränkung verweisen Enumerationsmember-Initialisierer möglicherweise unabhängig von ihrer Textposition auf andere Initialisierer von Enumerationsmembern. In einem enumerationselementinitialisierer werden Werte anderer Enumerationsmember immer so behandelt, als hätten Sie den Typ des zugrunde liegenden Typs, sodass Umwandlungen beim Verweis auf andere Enumerationsmember nicht erforderlich sind.

Das Beispiel

```csharp
enum Circular
{
    A = B,
    B
}
```

führt zu einem Kompilierzeitfehler, da die Deklarationen von `A` und `B` zirkulär sind. `A` ist explizit von `B` abhängig, und `B` hängt implizit von `A` ab.

Enumerationsmember werden genau analog zu den Feldern innerhalb von Klassen benannt und auf eine Weise festgelegt. Der Gültigkeitsbereich eines Enumerationsmembers ist der Text seines enthaltenden Enumerationstyps. Innerhalb dieses Bereichs kann mit dem einfachen Namen auf Enumerationsmember verwiesen werden. Aus dem gesamten anderen Code muss der Name eines Enumerationsmembers mit dem Namen seines Enumerationstyps qualifiziert werden. Enumerationsmember haben keine deklarierten Barrierefreiheits Elemente. ein Enumerationsmember ist verfügbar, wenn auf den enthaltenden Enumerationstyp zugegriffen werden kann.

## <a name="the-systemenum-type"></a>Der System. Aufzählungs Typen

Der Typ `System.Enum` ist die abstrakte Basisklasse aller Enumerationstypen (Dies unterscheidet sich von dem zugrunde liegenden Typ des Enumerationstyps), und die von `System.Enum` geerbten Member sind in jedem Enumerationstyp verfügbar. Eine Boxing-Konvertierung ([Boxing-Konvertierungen](types.md#boxing-conversions)) ist von einem beliebigen Aufzählungstyp zu `System.Enum` vorhanden, und eine Unboxing-Konvertierung ([Unboxing-Konvertierungen](types.md#unboxing-conversions)) ist von `System.Enum` in einen beliebigen Aufzählungstyp vorhanden.

Beachten Sie, dass "`System.Enum`" nicht selbst ein *enum_type*ist. Vielmehr handelt es sich um eine *class_type* , von der alle *enum_type*s abgeleitet werden. Der Typ `System.Enum` erbt vom Typ `System.ValueType` ([System. ValueType](types.md#the-systemvaluetype-type)), der wiederum vom Typ `object` erbt. Zur Laufzeit kann ein Wert vom Typ "`System.Enum`" `null` oder ein Verweis auf einen geboxten Wert eines beliebigen Enumerationstyps sein.

## <a name="enum-values-and-operations"></a>Enumerationswerte und-Vorgänge

Jeder Aufzählungstyp definiert einen eindeutigen Typ. eine explizite Enumerationskonvertierung ([Explizite Enumerationskonvertierungen](conversions.md#explicit-enumeration-conversions)) ist erforderlich, um zwischen einem Enumerationstyp und einem ganzzahligen Typ oder zwischen zwei Enumerationstypen zu konvertieren. Der Satz von Werten, der von einem Enumerationstyp übernommen werden kann, wird nicht durch seine Enumerationsmember eingeschränkt. Insbesondere kann jeder Wert des zugrunde liegenden Typs einer Enumeration in den Enumerationstyp umgewandelt werden und ist ein eindeutiger gültiger Wert dieses Enumerationstyps.

Enumerationsmember haben den Typ Ihres enthaltenden Enumerationstyps (außer innerhalb anderer Enumerationmember-Initialisierer: siehe [Enumeration](enums.md#enum-members)-Member). Der Wert eines Enumerationsmembers, der im Enumerationstyp deklariert wurde, `E` mit dem zugeordneten Wert `v` ist `(E)v`.

Die folgenden Operatoren können für Werte von Enumerationstypen verwendet werden: `==`, `!=`, `<`, `>`, `<=`, `>=` @ no__t-6 ([enumerationsvergleichs-Operatoren](expressions.md#enumeration-comparison-operators)), Binary `+` @ no__t-9 (Additions[Operator](expressions.md#addition-operator)), Binary 1 @ no__ t-12 ([Subtraktions Operator](expressions.md#subtraction-operator)), 4, 5, 6 @ no__t-17 ([logische enumerationsoperatoren](expressions.md#enumeration-logical-operators)), 9 @ no__t-20 ([bitweiser Komplement Operator](expressions.md#bitwise-complement-operator)), 2 und 3 @ no__t-24 ([postfix-Inkrement und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators) und [Präfix Inkrement-und Dekrementoperatoren](expressions.md#prefix-increment-and-decrement-operators)).

Jeder Enumerationstyp wird automatisch von der-Klasse `System.Enum` abgeleitet (wiederum abgeleitet von `System.ValueType` und `object`). Daher können geerbte Methoden und Eigenschaften dieser Klasse für Werte eines Enumerationstyps verwendet werden.
