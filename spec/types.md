---
ms.openlocfilehash: a28397b1ce97dbead6d5014e2b20e108a1018502
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229641"
---
# <a name="types"></a>Typen

Die Typen von c#-Sprache sind in zwei Hauptkategorien unterteilt: ***Werttypen*** und ***Verweistypen***. Werttypen und Verweistypen möglicherweise ***generische Typen***, dies dauern mindestens ***Typparameter***. Typparameter können bestimmen sowohl Werttypen und Verweistypen.

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

Die letzten Kategorie von Typen, Zeigern, steht nur in unsicheren Code zur Verfügung. Informationen finden Sie weiter unten in [Zeigertypen](unsafe-code.md#pointer-types).

Werttypen unterscheiden sich von Verweistypen, Variablen von Werttypen ihre Daten direkt enthalten, während Variablen des Verweises Typen Store ***Verweise*** auf ihre Daten, die letztere wird bezeichnet als ***Objekte***. Mit Verweistypen ist es möglich, dass zwei Variablen auf dasselbe Objekt verweisen, und so können Vorgänge auf eine Variable auf das Objekt, das die andere Variable verweist auswirken. Bei Werttypen besitzt jede Variable eine eigene Kopie der Daten, und es ist nicht möglich, für Vorgänge auf einem anderen zu beeinflussen.

# Typsystem ist dahingehend vereinheitlicht, dass ein Wert eines beliebigen Typs als Objekt behandelt werden kann. Jeder Typ in C# ist direkt oder indirekt vom `object`-Klassentyp abgeleitet, und `object` ist die ultimative Basisklasse aller Typen. Werte von Verweistypen werden als Objekte behandelt, indem die Werte einfach als Typ `object` angezeigt werden. Werte von Werttypen werden als Objekte behandelt, durch Ausführen von Vorgängen für Boxing und unboxing ([Boxing und unboxing](types.md#boxing-and-unboxing)).

## <a name="value-types"></a>Werttypen

Ein Werttyp ist, einen Strukturtyp oder ein Enumerationstyp. C# bietet eine Reihe von vordefinierten Strukturtypen, die Namen der ***einfache Typen***. Die einfachen Typen werden über reservierte Wörter identifiziert.

```antlr
value_type
    : struct_type
    | enum_type
    ;

struct_type
    : type_name
    | simple_type
    | nullable_type
    ;

simple_type
    : numeric_type
    | 'bool'
    ;

numeric_type
    : integral_type
    | floating_point_type
    | 'decimal'
    ;

integral_type
    : 'sbyte'
    | 'byte'
    | 'short'
    | 'ushort'
    | 'int'
    | 'uint'
    | 'long'
    | 'ulong'
    | 'char'
    ;

floating_point_type
    : 'float'
    | 'double'
    ;

nullable_type
    : non_nullable_value_type '?'
    ;

non_nullable_value_type
    : type
    ;

enum_type
    : type_name
    ;
```

Im Gegensatz zu einer Variable eines Referenztyps eine Variable eines Werttyps kann den Wert enthalten `null` nur dann, wenn der Werttyp ein nullable-Typ ist.  Für jeden NULL-Werte besteht ein entsprechende Werttyp, der den gleichen Satz von Werten und der Wert angibt `null`.

Zuweisung zu einer Variable eines Werttyps erstellt eine Kopie der Wert zugewiesen wird. Dies unterscheidet sich von Zuweisung einer Variable eines Referenztyps, die den Verweis aber nicht das Objekt identifiziert, die durch den Verweis kopiert.

### <a name="the-systemvaluetype-type"></a>Die System.ValueType-Typ

Alle Werttypen werden implizit von der Klasse erben `System.ValueType`, das wiederum erbt von Klasse `object`. Es ist nicht möglich, für jeden Typ ein Werttyp abgeleitet und Werttypen sind daher implizit versiegelt ([versiegelte Klassen](classes.md#sealed-classes)).

Beachten Sie, dass `System.ValueType` ist nicht selbst eine *Value_type*. Es handelt sich vielmehr eine *Class_type* von der alle *Value_type*s automatisch abgeleitet werden.

### <a name="default-constructors"></a>Standardkonstruktoren

Alle Werttypen werden implizit einen öffentlichen, parameterlosen Konstruktor wird aufgerufen, deklarieren die ***Standardkonstruktor***. Der Standardkonstruktor gibt eine 0 (null) initialisierten Instanz genannt die ***Standardwert*** für den Werttyp:

*  Für alle *Simple_type*s, der Standardwert ist der Wert, der durch ein Bitmuster mit ausschließlich Nullen generiert:
    * Für `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, und `ulong`, der Standardwert ist `0`.
    * Für `char`, der Standardwert ist `'\x0000'`.
    * Für `float`, der Standardwert ist `0.0f`.
    * Für `double`, der Standardwert ist `0.0d`.
    * Für `decimal`, der Standardwert ist `0.0m`.
    * Für `bool`, der Standardwert ist `false`.
*  Für eine *Enum_type* `E`, der Standardwert ist `0`, in den Typ konvertiert `E`.
*  Für eine *Struct_type*, der Standardwert ist der Wert, der durch Festlegen von alle Werttypfelder auf ihre Standardwerte und alle Verweise auf Felder generiert `null`.
*  Für eine *Nullable_type* der Standardwert ist eine Instanz für die die `HasValue` Eigenschaft ist "false" und die `Value` Eigenschaft ist nicht definiert. Der Standardwert ist auch bekannt als die ***null-Wert*** von nullable-Typ.

Wie alle anderen Instanzkonstruktor, wird der Standardkonstruktor eines Werttyps aufgerufen, mit der `new` Operator. Aus Effizienzgründen der Fall ist diese Anforderung nicht vorgesehen, um die Implementierung, die einen Konstruktoraufruf generiert haben. Im folgenden Beispiel wird Variablen `i` und `j` sind beide auf 0 (null) initialisiert.

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

Da jeder Werttyp implizit einen öffentlichen parameterlosen Instanzenkonstruktor verfügt, ist es nicht möglich, für einen Strukturtyp in eine explizite Deklaration von parameterlosen Konstruktoren enthalten. Ein Strukturtyp ist jedoch zulässig, parametrisierte Instanzkonstruktoren deklarieren ([Konstruktoren](structs.md#constructors)).

### <a name="struct-types"></a>Strukturtypen

Ein Strukturtyp ist ein Werttyp, der Konstanten, Felder, Methoden, Eigenschaften, Indexer, Operatoren, Instanzkonstruktoren, statische Konstruktoren und geschachtelte Typen deklarieren können. Die Deklaration von Strukturtypen finden Sie im [Strukturdeklarationen](structs.md#struct-declarations).

### <a name="simple-types"></a>Einfache Typen

C# bietet eine Reihe von vordefinierten Strukturtypen, die Namen der ***einfache Typen***. Die einfachen Typen über reservierte Wörter identifiziert werden, aber diese reservierte Wörter sind einfach Aliase für vordefinierte Strukturtypen in die `System` Namespace, wie in der folgenden Tabelle beschrieben.


| __Reserviertes Wort__ | __Aliastyp__ |
|-------------------|------------------|
| `sbyte`           | `System.SByte`   | 
| `byte`            | `System.Byte`    | 
| `short`           | `System.Int16`   | 
| `ushort`          | `System.UInt16`  | 
| `int`             | `System.Int32`   | 
| `uint`            | `System.UInt32`  | 
| `long`            | `System.Int64`   | 
| `ulong`           | `System.UInt64`  | 
| `char`            | `System.Char`    | 
| `float`           | `System.Single`  | 
| `double`          | `System.Double`  | 
| `bool`            | `System.Boolean` | 
| `decimal`         | `System.Decimal` | 

Da der einfache Typ einen Alias ein Strukturtyps, verfügt über alle einfachen Typen auf Member. Z. B. `int` wurde im deklarierten Member `System.Int32` und die Mitglieder von geerbt `System.Object`, und die folgenden Anweisungen sind zulässig:

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

Die einfachen Typen unterscheiden sich von anderen Strukturtypen dadurch, dass sie bestimmte zusätzliche Vorgänge ermöglichen:

*  Die meisten einfachen Typen ermöglichen, Werte für das Schreiben von zu erstellenden *Literale* ([Literale](lexical-structure.md#literals)). Z. B. `123` ist ein Literal vom Typ `int` und `'a'` ist ein Literal vom Typ `char`. In c# keine Bereitstellung für Literale der Strukturtypen im Allgemeinen wird, und nicht standardmäßigen Werten anderer Strukturtypen sind letztendlich immer über Instanzkonstruktoren dieser Struct-Typen erstellt.
*  Wenn die Operanden eines Ausdrucks auf alle einfache Typkonstanten sind, ist es möglich, dass der Compiler zum Auswerten des Ausdrucks während der Kompilierung. Ein solcher Ausdruck wird als bezeichnet ein *Constant_expression* ([Konstante Ausdrücke](expressions.md#constant-expressions)). Ausdrücke, die im Zusammenhang mit anderen Strukturtypen definierten Operatoren sind nicht als Konstante Ausdrücke sein.
*  Über `const` Deklarationen kann zum Deklarieren von Konstanten der einfachen Typen ([Konstanten](classes.md#constants)). Es ist nicht möglich, die Konstanten, die von anderen Strukturtypen haben, aber ein ähnliches Ergebnis erfolgt über `static readonly` Felder.
*  Konvertierungen, die im Zusammenhang mit einfachen Typen Auswertung von Konvertierungsoperatoren, die von anderen Strukturtypen definiert teilnehmen können, aber ein benutzerdefinierten Konvertierungsoperator kann nie teilnehmen Auswertung von einem anderen benutzerdefinierten Operator ([Auswertung Benutzerdefinierte Konvertierungen](conversions.md#evaluation-of-user-defined-conversions)).

### <a name="integral-types"></a>Ganzzahlige Typen

C# unterstützt neun ganzzahlige Typen: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, und `char`. Die ganzzahligen Typen haben die folgenden Größen und Wertebereiche:

*  Die `sbyte` Typ stellt signierten 8-Bit-Ganzzahlen mit Werten zwischen-128 und 127.
*  Die `byte` Typ darstellt, 8-Bit-Ganzzahlen ohne Vorzeichen mit Werten zwischen 0 und 255.
*  Die `short` Typ stellt signiert 16-Bit-Ganzzahlen mit Werten zwischen-32768 und 32767.
*  Die `ushort` Typ darstellt, 16-Bit-Ganzzahlen ohne Vorzeichen mit Werten zwischen 0 und 65535 liegen.
*  Die `int` Typ stellt signiert, 32-Bit-Ganzzahlen mit Werten zwischen-2147483648 und 2147483647.
*  Die `uint` Typ darstellt, 32-Bit-Ganzzahlen ohne Vorzeichen mit Werten zwischen 0 und 4294967295.
*  Die `long` Typ stellt signiert, 64-Bit-Ganzzahlen mit Werten zwischen – 9223372036854775808 und 9223372036854775807.
*  Die `ulong` Typ darstellt, 64-Bit-Ganzzahlen ohne Vorzeichen mit Werten zwischen 0 und 18446744073709551615.
*  Die `char` Typ darstellt, 16-Bit-Ganzzahlen ohne Vorzeichen mit Werten zwischen 0 und 65535 liegen. Der Satz möglicher Werte für die `char` Typ entspricht dem Unicode-Zeichensatz. Obwohl `char` hat die gleiche Darstellung wie `ushort`, nicht alle Vorgänge, die für einen Typ zulässig sind zulässig, auf dem anderen.

Der vom integralen Typ unären und binären Operatoren setzen immer mit 32-Bit-Präzision, ohne Vorzeichen 32-Bit-präziser, signierten 64-Bit-Präzision oder ohne Vorzeichen 64-Bit-präziser:

*  Für den unären `+` und `~` Operatoren, die den Operanden wird in den Typ konvertiert `T`, wobei `T` ist der erste Teil `int`, `uint`, `long`, und `ulong` , können alle vollständig darstellen Mögliche Werte des Operanden. Der Vorgang erfolgt dann mit der Genauigkeit des Datentyps `T`, und der Typ des Ergebnisses ist `T`.
*  Für den unären `-` -Operator, der Operand den Typ konvertiert wird `T`, wobei `T` ist der erste Teil `int` und `long` , die alle möglichen Werte des Operanden vollständig darstellen können. Der Vorgang erfolgt dann mit der Genauigkeit des Datentyps `T`, und der Typ des Ergebnisses ist `T`. Der unäre `-` Operator kann nicht auf Operanden des Typs angewendet werden `ulong`.
*  Für die Binärdatei `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, und `<=` Operatoren, die Operanden werden in den Typ konvertiert `T`, wobei `T` ist der erste Teil `int`, `uint`, `long`, und `ulong` , die alle vollständig darstellen können die Werte der beiden Operanden. Der Vorgang erfolgt dann mit der Genauigkeit des Datentyps `T`, und der Typ des Ergebnisses ist `T` (oder `bool` für die relationalen Operatoren). Es ist nicht zulässig, dass ein Operand vom Typ `long` und der andere Typ `ulong` mit binären Operatoren.
*  Für die Binärdatei `<<` und `>>` Operatoren, der linke Operand wird in den Typ konvertiert `T`, wobei `T` ist der erste Teil `int`, `uint`, `long`, und `ulong` , können alle vollständig darstellen Mögliche Werte des Operanden. Der Vorgang erfolgt dann mit der Genauigkeit des Datentyps `T`, und der Typ des Ergebnisses ist `T`.

Die `char` Typ wird als ein ganzzahliger Typ klassifiziert, aber es unterscheidet sich von anderen ganzzahligen Typen gibt es zwei Möglichkeiten:

*  Es gibt keine impliziten Konvertierungen anderen Typen in der `char` Typ. Insbesondere, obwohl die `sbyte`, `byte`, und `ushort` aufweisen Wertebereiche, die vollständig auf darstellbar mithilfe der `char` eingeben, implizite Konvertierungen von `sbyte`, `byte`, oder `ushort` auf `char` nicht vorhanden sind.
*  Die Konstanten der `char` Typ geschrieben werden muss, als *Character_literal*s oder als *Integer_literal*s in Kombination mit einer Umwandlung in den Typ `char`. Beispielsweise hat `(char)10` die gleiche Wirkung wie `'\x000A'`.

Die `checked` und `unchecked` Operatoren und Anweisungen werden verwendet, um überlaufprüfung für arithmetische Operationen für ganzzahlige Typen und Konvertierungen zu steuern ([checked und unchecked Operatoren](expressions.md#the-checked-and-unchecked-operators)). In einem `checked` Kontext ist ein Überlauf erzeugt einen Fehler während der Kompilierung oder bewirkt, dass eine `System.OverflowException` ausgelöst wird. In einer `unchecked` Kontext Überläufe ignoriert, und alle höherwertigen Bits, die nicht in den Zieltyp passen werden verworfen.

### <a name="floating-point-types"></a>Gleitkommatypen

C# unterstützt zwei Gleitkommatypen: `float` und `double`. Die `float` und `double` Typen werden mit den 32-Bit mit einfacher Genauigkeit und 64-Bit-Gleitkommazahl mit doppelter Genauigkeit IEEE 754-Formaten, die die folgenden Sätze von Werten bereitstellen dargestellt:

*  Positive Nullen und negative 0 (null). In den meisten Fällen, positive und Negative 0 (null) Verhalten sich identisch, der einfachen Wert 0 (null), aber bestimmte Vorgänge, die zwischen den beiden unterscheiden ([Divisionsoperator](expressions.md#division-operator)).
*  Plus unendlich und negativ unendlich. Unendliche Werte werden durch Vorgänge wie dividiert eine Zahl ungleich NULL durch 0 (null) erstellt. Z. B. `1.0 / 0.0` plus unendlich ergibt und `-1.0 / 0.0` negative Unendlichkeit.
*  Die ***Not-a-Number*** Wert, häufig abgekürzt als NaN. NaN-Werte werden durch ungültige Gleitkommaoperationen, z. B. Division von 0 (null) von 0 (null) erstellt.
*  Die begrenzte Menge von Werten ungleich Null des Formulars `s * m * 2^e`, wobei `s` 1 oder-1 ist, und `m` und `e` hängen von den entsprechenden Gleitkommatyp: Für `float`, `0 < m < 2^24` und `-149 <= e <= 104`, und für `double`, `0 < m < 2^53` und `1075 <= e <= 970`. Denormalisierte Gleitkommazahlen gelten als gültige nicht-NULL-Werte.

Die `float` Typ kann Werte im Bereich von ungefähr darstellen `1.5 * 10^-45` zu `3.4 * 10^38` mit einer Genauigkeit von 7 Stellen.

Die `double` Typ kann Werte im Bereich von ungefähr darstellen `5.0 * 10^-324` zu `1.7 × 10^308` mit einer Genauigkeit von 15 – 16 Stellen.

Wenn einer der Operanden des binären Operators ein Gleitkommatyp ist, dann muss der andere Operand ein ganzzahliger Typ oder Gleitkommatyp sein, und der Vorgang wird wie folgt ausgewertet:

*  Wenn einer der Operanden ein ganzzahliger Typ ist, wird dieser Operand, den der Gleitkommatyp des anderen Operanden konvertiert.
*  Klicken Sie dann, wenn einer der Operanden des Typs `double`, wird der andere Operand in konvertiert `double`, der Vorgang ausgeführt wird, mit mindestens `double` Bereich und Genauigkeit, und der Typ des Ergebnisses `double` (oder `bool` für die Relationale Operatoren).
*  Andernfalls ist der Vorgang mit mindestens ausgeführt `float` Bereich und Genauigkeit, und der Typ des Ergebnisses `float` (oder `bool` für die relationalen Operatoren).

Der Gleitkomma-Operatoren, einschließlich der Zuweisungsoperatoren sind erzeugen niemals Ausnahmen aus. Stattdessen erzeugt in Ausnahmefällen, Gleitkommaoperationen 0 (null), unendlich oder NaN ist, wie im folgenden beschrieben:

*  Wenn das Ergebnis einer Gleitkommaoperation zu klein für das Zielformat ist, wird das Ergebnis des Vorgangs positiv oder negative 0 (null).
*  Wenn das Ergebnis einer Gleitkommaoperation für das Zielformat zu groß ist, wird das Ergebnis des Vorgangs positive oder negative Unendlichkeit.
*  Wenn eine Gleitkommaoperation ungültig ist, wird das Ergebnis des Vorgangs NaN.
*  Wenn eine oder beide der Operanden einer Gleitkommaoperation ist NaN, wird das Ergebnis des Vorgangs NaN.

Gleitkommaoperationen können mit einer höheren Genauigkeit als der Ergebnistyp des Vorgangs ausgeführt werden. Beispielsweise unterstützen einige Hardwarearchitekturen einen "erweiterten" oder "long double" vom Typ Gleitkommazahlen mit größeren Bereich und Genauigkeit als den `double` geben, und führen Sie alle Operationen mit Gleitkommazahlen mit dem folgenden höhere Genauigkeit Typ implizit. Nur auf eine übermäßige Abstriche bei der Leistung können solche Hardwarearchitekturen zum Ausführen von Operations mit Gleitkommazahlen mit geringerer Genauigkeit und anstelle eine Implementierung, die in Anspruch genommenen Leistung und Genauigkeit erfordern, lässt c# einen höheren Genauigkeit Typ für alle Operationen mit Gleitkommazahlen verwendet. Als die Bereitstellung eine genauere Ergebnisse, hat dies nur selten keine messbaren Auswirkungen. In Ausdrücken des Formulars `x * y / z`, in denen die Multiplikation für ein Ergebnis erzeugt, die außerhalb der `double` Bereich, aber die nachfolgende Division wird das temporäre Ergebnis wieder in die `double` liegen, die Tatsache, dass der Ausdruck ausgewertet, in einem höheren Bereich Format kann dazu führen, dass eine endliche Ergebnis unendlich erstellt werden.

### <a name="the-decimal-type"></a>Der decimal-Typ

Der `decimal`-Typ ist ein für Finanz-und Währungsberechnungen geeigneter 128-Bit-Datentyp. Die `decimal` Typ kann im Bereich von Werten darstellen `1.0 * 10^-28` auf ungefähr `7.9 * 10^28` mit 28 bis 29 signifikanten Stellen.

Die begrenzte Menge von Werten des Typs `decimal` weisen folgendes Format `(-1)^s * c * 10^-e`, wobei das Vorzeichen `s` gleich 0 oder 1, den Koeffizienten `c` , angegeben durch `0 <= *c* < 2^96`, und der Skala `e` ist so, dass `0 <= e <= 28`. Die `decimal` Typ unterstützt nicht mit Nullen, unendliche und NaN. Ein `decimal` wird als eine 96-Bit-Ganzzahl, die durch eine Potenz von 10 skaliert dargestellt. Für `decimal`s mit einem absoluten Wert kleiner als `1.0m`, der Wert ist genau auf die 28. Dezimalstelle, aber nicht weiter. Für `decimal`s mit einem absoluten Wert größer als oder gleich `1.0m`, der Wert ist auf 28 oder 29 Stellen genau. Gattungsbezeichnung der `float` und `double` -Datentypen, wie z. B. 0,1 Dezimalzahlen dargestellt werden können in genau der `decimal` Darstellung. In der `float` und `double` Darstellungen, diese Zahlen sind häufig unendliche Brüche angegeben, wodurch die Darstellung anfälliger für runden Fehler.

Wenn einer der Operanden des binären Operators vom Typ `decimal`, muss der andere Operand ein ganzzahliger Typ oder vom Typ werden `decimal`. Wenn ein Operand integralen Typ vorhanden ist, wird eine Konvertierung in `decimal` , bevor der Vorgang ausgeführt wird.

Das Ergebnis eines Vorgangs für Werte vom Typ `decimal` besteht darin, dass die Berechnung eines genauen Ergebnis (Staffelung beibehalten, wie für die einzelnen Operatoren definiert), und klicken Sie dann entsprechend die Darstellung runden entstehen würden. Ergebnisse werden gerundet, um den nächsten darstellbaren Wert und, wenn ein Ergebnis gleichmäßig in der Nähe zwei Werte dargestellt werden kann, auf den Wert, der eine gerade Anzahl an der Position der am wenigsten signifikanten Ziffern verfügt (Dies ist bekannt als "unverzerrte Rundung der"). Ein NULL-Ergebnis hat immer ein Vorzeichen 0 und einer Skala von 0.

Wenn eine dezimale arithmetische Operation auf einen Wert kleiner als oder gleich erzeugt `5 * 10^-29` in absoluten Wert wird das Ergebnis des Vorgangs 0 (null). Wenn eine `decimal` arithmetische Operation führt zu einem Ergebnis, das für zu groß ist die `decimal` -Format eine `System.OverflowException` ausgelöst.

Die `decimal` Typ verfügt über höhere Genauigkeit aber kleineren Bereich als das Gleitkomma-Datentypen. Daher Konvertierungen von Gleitkommatypen auf `decimal` Stapelüberlauf-Ausnahmen und Konvertierungen von erzeugen möglicherweise `decimal` auf die Gleitkomma-Datentypen möglicherweise zu Genauigkeitsverlust führen. Aus diesen Gründen werden keine impliziten Konvertierungen zwischen Gleitkommatypen vorhanden und `decimal`, und ohne explizite Umwandlungen ist es nicht möglich, Gleitkomma mischen und `decimal` Operanden in dem gleichen Ausdruck.

### <a name="the-bool-type"></a>Der Typ "bool"

Die `bool` Boolesche logische Mengen darstellt. Die möglichen Werte des Typs `bool` sind `true` und `false`.

Keine standardkonvertierungen bestehen zwischen `bool` und anderen Typen. Insbesondere die `bool` verschieden und getrennt von ganzzahligen Typen ist und ein `bool` Wert kann nicht verwendet werden, statt einen ganzzahligen Wert (und umgekehrt).

In den Sprachen C und C++ einen ganzzahligen oder Gleitkomma Nullwert oder ein null-Zeiger auf den booleschen Wert konvertiert werden kann `false`, und einen Wert ungleich NULL, ganzzahligen oder Gleitkomma oder ein nicht-Null-Zeiger auf den booleschen Wert konvertiert werden kann `true`. Solche Konvertierungen werden in c# erreicht, durch die explizite Vergleich einen ganzzahligen oder Gleitkomma-Wert 0 (null) oder durch die explizite Vergleich einen Objektverweis auf `null`.

### <a name="enumeration-types"></a>Enumerationstypen

Ein Enumerationstyp ist ein eigenständiger Typ mit benannter Konstanten. Jeder Enumerationstyp hat einen zugrunde liegenden Typ, der sein muss `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` oder `ulong`. Der Satz von Werten des Enumerationstyps ist der Satz von Werten des zugrunde liegenden Typs identisch. Werte des Enumerationstyps sind nicht auf die Werte der benannten Konstanten beschränkt. Enumerationstypen über Enumerationsdeklarationen definiert ([Enumerationsdeklarationen](enums.md#enum-declarations)).

### <a name="nullable-types"></a>Auf NULL festlegbare Typen

Ein nullable-Typ kann alle Werte der darstellen der ***zugrunde liegender Typ*** plus eine zusätzliche null-Wert. Ein nullable-Typ geschrieben `T?`, wobei `T` ist der zugrunde liegenden Typ. Diese Syntax ist die Kurzform für `System.Nullable<T>`, und die beiden Formen sind austauschbar.

Ein ***nicht auf NULL festlegbarer Werttyp*** ist im Gegensatz dazu jeder Werttyp außer `System.Nullable<T>` und die Kurzschreibweise `T?` (für alle `T`), sowie einen Typparameter, der ein NULL-Werttyp (d. h. alle ist beschränkt ist Geben Sie Parameter mit einem `struct` Einschränkung). Die `System.Nullable<T>` Typ gibt an, der werttypeinschränkung für `T` ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)), was bedeutet, dass der zugrunde liegende Typ einen nullable-Typ NULL-Werttyp werden kann. Der zugrunde liegende Typ eines nullable-Typs darf keinen nullable-Typ oder ein Verweistyp sein. Z. B. `int??` und `string?` ist ein ungültiger Typ.

Eine Instanz von einem nullable-Typ `T?` hat zwei öffentliche schreibgeschützte Eigenschaften:

*  Ein `HasValue` Eigenschaft vom Typ `bool`
*  Ein `Value` Eigenschaft vom Typ `T`

Eine Instanz für die `HasValue` ist "true" gilt als nicht Null sein. Eine nicht-Null-Instanz enthält, einen bekannten Wert und `Value` gibt diesen Wert zurück.

Eine Instanz für die `HasValue` ist "false" gilt als null sein. Eine null-Instanz verfügt über einen nicht definierten Wert. Beim Lesen der `Value` führt dazu, dass eine null-Instanz eine `System.InvalidOperationException` ausgelöst wird. Der Prozess für den Zugriff auf die `Value` Eigenschaft einer Instanz mit NULL-Werte zulässt, die Verfügbarkeitsklasse ***zum Entpacken***.

Zusätzlich zu den Standardkonstruktor, der alle nullable-Typ `T?` verfügt über einen öffentlichen Konstruktor, der ein einzelnes vom Typ Argument `T`. Ein Wert zugewiesen `x` des Typs `T`, einen Konstruktoraufruf des Formulars

```csharp
new T?(x)
```
erstellt eine Instanz ungleich Null der `T?` für die die `Value` Eigenschaft `x`. Erstellen Sie eine nicht-Null-Instanz, der einen nullable-Typ für ein bestimmten Wert als bezeichnet ***wrapping***.

Implizite Konvertierungen aus verfügbar sind die `null` literal `T?` ([Null-Literale Konvertierungen](conversions.md#null-literal-conversions)) und von `T` zu `T?` ([implizite NULL-Werte zulassen Konvertierungen](conversions.md#implicit-nullable-conversions)).

## <a name="reference-types"></a>Verweistypen

Ein Verweistyp ist ein Klassentyp, einen Schnittstellentyp, einen Arraytyp oder einen Delegattyp aufweisen.

```antlr
reference_type
    : class_type
    | interface_type
    | array_type
    | delegate_type
    ;

class_type
    : type_name
    | 'object'
    | 'dynamic'
    | 'string'
    ;

interface_type
    : type_name
    ;

array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;

delegate_type
    : type_name
    ;
```

Ein verweistypwert ist ein Verweis auf ein ***Instanz*** des Typs, der letzten bekannten als ein ***Objekt***. Der spezielle Wert `null` ist kompatibel mit alle Verweistypen zulässig, und gibt das Fehlen einer Instanz.

### <a name="class-types"></a>Klassentypen

Ein Klassentyp definiert eine Datenstruktur, die Daten Mitglieder (Konstanten und Feldern), Funktionsmember (Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren, Instanzkonstruktoren, Destruktoren und statische Konstruktoren) und geschachtelte Typen enthält. Klassentypen unterstützen die Vererbung, einen Mechanismus, bei dem abgeleitete Klassen können erweitert und Basisklassen spezialisiert. Instanzen von Klassentypen werden mit erstellt *Object_creation_expression*s ([Erstellung Objektausdrücke](expressions.md#object-creation-expressions)).

Klassentypen werden in beschrieben [Klassen](classes.md).

Bestimmte vordefinierte Klassentypen haben eine besondere Bedeutung in der C#-Sprache, wie in der folgenden Tabelle beschrieben.


| __Klassentyp__     | __Beschreibung__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | Die ultimative Basisklasse aller anderen Typen. Finden Sie unter [den Objekttyp](types.md#the-object-type). | 
| `System.String`    | Der Zeichenfolgentyp der C#-Sprache. Finden Sie unter [Zeichenfolgentyps](types.md#the-string-type).         |
| `System.ValueType` | Die Basisklasse aller Typen von Wert. Finden Sie unter [die System.ValueType-Typ](types.md#the-systemvaluetype-type).          |
| `System.Enum`      | Die Basisklasse aller Enum-Typen. Finden Sie unter [Enumerationen](enums.md).              |
| `System.Array`     | Die Basisklasse für alle Arraytypen. Siehe [Arrays](arrays.md).             |
| `System.Delegate`  | Die Basisklasse aller Delegattypen. Finden Sie unter [Delegaten](delegates.md).          |
| `System.Exception` | Die Basisklasse aller Art der Ausnahme. Finden Sie unter [Ausnahmen](exceptions.md).         |

### <a name="the-object-type"></a>Der Objekttyp

Die `object` Klassentyp ist die ultimative Basisklasse aller anderen Typen. Jeder Typ in C# geschrieben, direkt oder indirekt, abgeleitet aus den `object` Klassentyp.

Das Schlüsselwort `object` ist einfach ein Alias für die vordefinierten Klasse `System.Object`.

### <a name="the-dynamic-type"></a>Der dynamische Typ

Die `dynamic` eingeben, z. B. `object`, kann jedes beliebige Objekt verweisen. Wenn Operatoren angewendet werden auf Ausdrücke vom Typ `dynamic`, deren Auflösung wird verzögert, bis das Programm ausgeführt wird. Daher dass der Operator kann nicht gesetzlich für das referenzierte Objekt angewendet werden, kein Fehler während der Kompilierung erhält. Stattdessen wird eine Ausnahme ausgelöst werden, wenn die Auflösung des Operators zur Laufzeit fehlschlägt.

Ihr Zweck ist, können dynamische Bindung, die ausführlich beschrieben wird [dynamische Bindung](expressions.md#dynamic-binding).

`dynamic` wird als identisch betrachtet `object` mit Ausnahme der in folgender Hinsicht:

*  Vorgänge für Ausdrücke vom Typ `dynamic` dynamisch gebunden werden kann ([dynamische Bindung](expressions.md#dynamic-binding)).
*  Typrückschluss ([Typrückschluss](expressions.md#type-inference)) am liebsten `dynamic` über `object` Wenn beide in Frage kommen.

Aufgrund dieser Äquivalenz enthält die folgenden Schritte aus:

*  Es gibt eine implizite identitätsänderung Konvertierung zwischen `object` und `dynamic`, und zwischen konstruierte Typen, die gleich, beim Ersetzen von sind `dynamic` mit `object`
*  Implizite und explizite Konvertierungen in und aus `object` gelten auch in den und aus `dynamic`.
*  Methodensignaturen, die gleich, beim Ersetzen von sind `dynamic` mit `object` gelten die gleiche Signatur
*  Der Typ `dynamic` wird nicht von Unterschieden `object` zur Laufzeit.
*  Ein Ausdruck vom Typ `dynamic` wird als bezeichnet ein ***dynamischen Ausdrucks***.

### <a name="the-string-type"></a>Der String-Datentyp

Die `string` Typ ist ein Typ von versiegelten Klasse, die direkt von erbt `object`. Instanzen der `string` Klasse dar, Unicode-Zeichenfolgen.

Werte von der `string` Typ geschrieben werden kann, als Zeichenfolgenliterale ([Zeichenfolgenliterale](lexical-structure.md#string-literals)).

Das Schlüsselwort `string` ist einfach ein Alias für die vordefinierten Klasse `System.String`.

### <a name="interface-types"></a>Schnittstellentypen

Eine Schnittstelle definiert einen Vertrag. Eine Klasse oder Struktur, die eine Schnittstelle implementiert, muss ihren Vertrag einhalten. Eine Schnittstelle kann von mehreren Basisschnittstellen erben, und eine Klasse oder Struktur kann mehrere Schnittstellen implementieren.

Schnittstellentypen werden in beschrieben [Schnittstellen](interfaces.md).

### <a name="array-types"></a>Arraytypen

Ein Array ist eine Datenstruktur, die NULL oder mehr Variablen enthält, die über berechnete Indizes zugegriffen wird. In einem Array, das die Elemente des Arrays, so genannte enthaltenen Variablen sind alle vom selben Typ, und diese Art wird den Elementtyp des Arrays bezeichnet.

Arraytypen sind in beschriebenen [Arrays](arrays.md).

### <a name="delegate-types"></a>Delegattypen

Ein Delegat ist eine Datenstruktur, die auf mindesten eine Methode verweist. Z. B. Methoden, bezieht er sich auch auf ihre entsprechenden Objektinstanzen.

Am nächsten eines Delegaten in C oder C++ entspricht ein Funktionszeiger, aber während ein Funktionszeiger nur statische Funktionen verweisen kann, ein Delegaten verweisen sowohl statische und Instanzenmethoden kann. Im letzteren Fall speichert der Delegaten nicht nur einen Verweis auf den Einstiegspunkt der Methode, sondern auch einen Verweis auf die Objektinstanz, auf dem zum Aufrufen der Methode.

Delegattypen werden in beschrieben [Delegaten](delegates.md).

## <a name="boxing-and-unboxing"></a>Boxing und Unboxing

Das Konzept von Boxing und unboxing ist für die # Typsystem. Es stellt eine Brücke zwischen *Value_type*s und *Reference_type*s Anwesenheitsebene einen Wert für die ein *Value_type* zum und vom Typ konvertiert werden `object`. Boxing und unboxing ermöglicht eine einheitliche Ansicht des Typsystems, bei dem ein Wert eines beliebigen Typs letztendlich als Objekt behandelt werden kann.

### <a name="boxing-conversions"></a>Boxing-Konvertierung

Ermöglicht eine Boxingkonvertierung einer *Value_type* implizit zu konvertierenden eine *Reference_type*. Die folgenden Boxing-Konvertierung sind vorhanden:

*  Von jedem *Value_type* in den Typ `object`.
*  Von jedem *Value_type* in den Typ `System.ValueType`.
*  Von jedem *Non_nullable_value_type* auf *Interface_type* implementiert die *Value_type*.
*  Von jedem *Nullable_type* auf *Interface_type* von den zugrunde liegenden Typ implementiert die *Nullable_type*.
*  Von jedem *Enum_type* in den Typ `System.Enum`.
*  Von jedem *Nullable_type* mit einem zugrunde liegenden *Enum_type* in den Typ `System.Enum`.
*  Beachten Sie, dass eine implizite Konvertierung von einem Typparameter als eine Boxingkonvertierung ausgeführt wird, wenn zur Laufzeit das Konvertieren von Werttypen auf einen Referenztyp führt ([implizite Konvertierungen, die im Zusammenhang mit Typparametern](conversions.md#implicit-conversions-involving-type-parameters)).

Boxing-Wert eine *Non_nullable_value_type* besteht aus eine Objektinstanz zuordnen und das Kopieren der *Non_nullable_value_type* Wert in dieser Instanz.

Boxing-Wert eine *Nullable_type* erzeugt einen null-Verweis, ist dies die `null` Wert (`HasValue` ist `false`), oder das Ergebnis von entpacken und den zugrunde liegenden Wert andernfalls boxing.

Der eigentliche Prozess der boxing-Wert eine *Non_nullable_value_type* am besten lässt sich vorstellen, das Vorhandensein eines generischen ***Boxingklasse***, die verhält, als ob es wie folgt deklariert wurden:

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

Boxing eines Werts `v` des Typs `T` besteht nun der Ausführung des Ausdrucks `new Box<T>(v)`, und die daraus resultierende Instanz zurückgegeben, als ein Wert vom Typ `object`. Daher die Anweisungen
```csharp
int i = 123;
object box = i;
```
im Prinzip entsprechen
```csharp
int i = 123;
object box = new Box<int>(i);
```

Eine Boxingklasse wie `Box<T>` höher ist nicht tatsächlich vorhanden und der dynamische Typ der einen geschachtelten Wert ist nicht tatsächlich einen Klassentyp. Stattdessen einen geschachtelten Wert vom Typ `T` weist den Typ der dynamischen `T`, und einem Typ "dynamic" Überprüfen Sie mithilfe der `is` Operator kann einfach auf den Typ verweisen `T`. Ein auf ein Objekt angewendeter
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
Gibt die Zeichenfolge "`Box contains an int`" in der Konsole.

Eine Boxingkonvertierung impliziert eine Kopie des Werts geschachtelt werden. Dies unterscheidet sich von der Konvertierung einer *Reference_type* eingeben `object`, in denen der Wert weiterhin dieselbe Instanz verweisen, und wird einfach als weniger stark abgeleiteten Typ betrachtet `object`. Z. B. im Falle folgender Deklaration
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
die folgenden Anweisungen
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
Gibt den Wert 10 in der Konsole aus, da der implizites Boxing-Vorgang, der bei der Zuweisung von auftritt, `p` zu `box` wird der Wert des `p` kopiert werden soll. Hatte `Point` wurde deklariert eine `class` stattdessen wäre der Wert 20 ausgegeben werden, da `p` und `box` würde die gleiche Instanz zu verweisen.

### <a name="unboxing-conversions"></a>Unboxing-Konvertierungen

Ermöglicht eine unboxing-Konvertierung einer *Reference_type* explizit zu konvertierenden eine *Value_type*. Die folgenden unboxing-Konvertierungen sind vorhanden:

*  Vom Typ `object` auf *Value_type*.
*  Vom Typ `System.ValueType` auf *Value_type*.
*  Von jedem *Interface_type* auf *Non_nullable_value_type* , implementiert die *Interface_type*.
*  Von jedem *Interface_type* auf *Nullable_type* , dessen zugrunde liegenden Typ implementiert die *Interface_type*.
*  Vom Typ `System.Enum` auf *Enum_type*.
*  Vom Typ `System.Enum` auf *Nullable_type* mit einem zugrunde liegenden *Enum_type*.
*  Beachten Sie, dass eine explizite Konvertierung für einen Typparameter als einer unboxing-Konvertierung ausgeführt wird, wenn zur Laufzeit das Konvertieren von einem Referenztyp in einen Werttyp führt ([explizite dynamische Konvertierungen](conversions.md#explicit-dynamic-conversions)).

Ein unboxing-Vorgang um eine *Non_nullable_value_type* zunächst geprüft wird, die die Objektinstanz einen geschachtelten Wert besteht aus der angegebenen *Non_nullable_value_type*, und kopieren Sie den Wert von der -Instanz.

Unboxing zu einer *Nullable_type* erzeugt den null-Wert von der *Nullable_type* ist Quelloperanden `null`, oder das umschlossene Ergebnis von unboxing die Objektinstanz, die den zugrunde liegenden Typ der *Nullable_type* andernfalls.

Um der imaginären Boxingklasse, die im vorherigen Abschnitt beschrieben, einer unboxing-Konvertierung eines Objekts verweisen `box` zu einem *Value_type* `T` besteht aus der Ausführung des Ausdrucks `((Box<T>)box).value`. Daher die Anweisungen
```csharp
object box = 123;
int i = (int)box;
```
im Prinzip entsprechen
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

Für ein unboxing-Konvertierung in einen angegebenen *Non_nullable_value_type* um zur Laufzeit erfolgreich ausgeführt werden, muss der Wert des Quelloperanden einen Verweis auf einen geschachtelten Wert, der *Non_nullable_value_type*. Wenn der Quelloperand ist `null`, `System.NullReferenceException` ausgelöst. Wenn Quelloperanden ein Verweis auf ein Objekt nicht kompatibel ist eine `System.InvalidCastException` ausgelöst.

Für ein unboxing-Konvertierung in einen angegebenen *Nullable_type* um zur Laufzeit erfolgreich ausgeführt werden, muss der Wert des Quelloperanden werden entweder `null` oder ein Verweis auf einen geschachtelten Wert des zugrunde liegenden *Non_nullable_value_type* von der *Nullable_type*. Wenn Quelloperanden ein Verweis auf ein Objekt nicht kompatibel ist eine `System.InvalidCastException` ausgelöst.

## <a name="constructed-types"></a>Konstruierte Typen

Gibt die Deklaration eines generischen Typs selbst eine ***ungebundene generischen Typ*** , der als "Blaupause" verwendet wird, um viele verschiedene Arten, über die Anwendung bilden ***Typargumente***. Die Typargumente in spitzen Klammern geschrieben werden (`<` und `>`) unmittelbar nach dem Namen des generischen Typs. Ein Typ, der mindestens ein Typargument enthält heißt eine ***konstruierter Typ***. Ein konstruierter Typ kann in den meisten stellen in der Sprache verwendet werden, in dem ein Typnamen angezeigt werden kann. Ein ungebundener generischer Typ kann nur verwendet werden, in einem *Typeof_expression* ([der Typeof-Operator](expressions.md#the-typeof-operator)).

Konstruierte Typen können auch als einfache Namen in Ausdrücken verwendet werden ([einfache Namen](expressions.md#simple-names)) oder auf einen Member zugreifen ([Memberzugriff](expressions.md#member-access)).

Wenn eine *Namespace_or_type_name* ist die richtige Anzahl von Parametern gelten als Typ ausgewertete, die nur generische Typen. Daher ist es möglich, die den gleichen Bezeichner zu verwenden, um unterschiedliche Typen aufweisen, ermitteln, solange die Typen eine unterschiedliche Anzahl von Typparametern haben. Dies ist hilfreich, wenn generische und nicht generischen Klassen im selben Programm zu kombinieren:

```csharp
namespace Widgets
{
    class Queue {...}
    class Queue<TElement> {...}
}

namespace MyApplication
{
    using Widgets;

    class X
    {
        Queue q1;            // Non-generic Widgets.Queue
        Queue<int> q2;       // Generic Widgets.Queue
    }
}
```

Ein *Type_name* möglicherweise einen konstruierten Typ zu identifizieren, obwohl sie direkt Typparameter angeben nicht. Dies kann auftreten, in denen ein Typ in der Deklaration einer generischen Klasse geschachtelt ist und der Instanztyp der enthaltenden Deklaration wird implizit für die Namenssuche verwendet ([geschachtelte Typen in generischen Klassen](classes.md#nested-types-in-generic-classes)):

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

In unsicherem Code nicht als ein konstruierter Typ verwendet werden eine *Unmanaged_type* ([Zeigertypen](unsafe-code.md#pointer-types)).

### <a name="type-arguments"></a>Typargumente

Jedes Argument in einer Liste der Typargumente ist einfach eine *Typ*.

```antlr
type_argument_list
    : '<' type_arguments '>'
    ;

type_arguments
    : type_argument (',' type_argument)*
    ;

type_argument
    : type
    ;
```

In unsicherem Code ([unsicheren Code](unsafe-code.md)), ein *Type_argument* möglicherweise kein Zeigertyp. Jedes Typargument erfüllen muss, alle Einschränkungen für den entsprechenden Typparameter ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)).

### <a name="open-and-closed-types"></a>Offene und geschlossene Typen

Alle Typen klassifiziert werden können, entweder als ***offene Typen*** oder ***geschlossene Typen***. Ein offener Typ ist ein Typ, der Parameter vom Typ umfasst. Genauer gesagt:

*  Ein Typparameter definiert einen offenen Typ.
*  Array-Typ ist ein offener Typ, wenn der Elementtyp ein offener Typ ist.
*  Ein konstruierter Typ ist ein offener Typ, wenn mindestens eines seiner Typargumente einen offenen Typ handelt. Ein geschachtelten konstruierter Typ ist ein offener Typ, wenn mindestens eines seiner Typargumente oder die Typargumente der enthaltenden Typs einen offenen Typ handelt.

Ein geschlossener Typ ist ein Typ, der nicht auf einen offenen Typ handelt.

Zur Laufzeit der gesamte Code in der Deklaration eines generischen Typs wird ausgeführt im Kontext eines geschlossenen konstruierten Typs, die durch Anwenden von Typargumenten auf die generische Deklaration erstellt wurde. Jeden von Typparameter in der generischen Typ ist für eine bestimmte Art von zur Laufzeit gebunden. Immer die Laufzeit die Verarbeitung aller Anweisungen und Ausdrücke geschieht geschlossene Typen, und offene Typen auftreten nur während der Kompilierzeit-Verarbeitung.

Jede geschlossener konstruierter Typ verfügt über einen eigenen Satz von statischen Variablen, die nicht gemeinsam mit anderen geschlossenen konstruierten Typen verwendet werden. Da ein offener Typ zur Laufzeit nicht vorhanden ist, sind keine statischen Variablen, die ein offener Typ zugeordnet. Zwei geschlossene konstruierte Typen weisen denselben Typ auf, wenn sie über den gleichen ungebundenen generischen Typ erstellt werden, und ihre entsprechenden Typargumente desselben Typs.

### <a name="bound-and-unbound-types"></a>Gebunden ist, und nicht gebundene Typen

Der Begriff ***nicht gebundenen Typ*** bezieht sich auf einen nicht generischen Typ oder einen ungebundenen generischen Typ. Der Begriff ***Typ gebunden*** bezieht sich auf einen nicht generischen Typ oder einen konstruierten Typ.

Ein ungebundener Typ bezieht sich auf die Entität deklariert, indem eine Typdeklaration. Ein ungebundener generischer Typ ist selbst ein Typ und kann nicht als Typ einer Variablen, Argument oder Rückgabewert oder als Basistyp verwendet werden. Ist das einzige Konstrukt in der ein ungebundener generischer Typ verwiesen werden kann die `typeof` Ausdruck ([der Typeof-Operator](expressions.md#the-typeof-operator)).

### <a name="satisfying-constraints"></a>Zufriedenstellendes Einschränkungen

Wenn es sich bei einem konstruierten Typ oder eine generische Methode verwiesen wird, werden die angegebenen Typargumente mit Einschränkungen für Typparameter für den generischen Typ oder Methode deklariert verglichen ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)). Für jede `where` -Klausel, die das Typargument `A` , entspricht die benannte Parameter vom Typ überprüft wird jede Einschränkung wie folgt:

*  Wenn die Einschränkung eines Klassentyps, einen Schnittstellentyp aufweisen oder ein Typparameter ist, können `C` stellen dar, dass die Einschränkung mit der angegebenen Typargumente für Typparameter ersetzt, die in der Einschränkung angezeigt werden. Um die Einschränkung zu erfüllen, muss es der Fall, die eingeben `A` konvertiert werden kann, geben Sie `C` durch einen der folgenden:
    * Eine identitätskonvertierung ([identitätskonvertierung](conversions.md#identity-conversion))
    * Implizite verweiskonvertierung ([implizite Verweis-](conversions.md#implicit-reference-conversions))
    * Eine Boxingkonvertierung ([Boxingkonvertierungen](conversions.md#boxing-conversions)), vorausgesetzt, dass Typ A ein NULL-Werte ist.
    * Eine implizite Konvertierung eines Verweis "," Boxing "oder" Typ-Parameter von einem Typparameter `A` zu `C`.
*  Wenn die Einschränkung der verweistypeinschränkung ist (`class`), den Typ `A` erfüllen müssen, eine der folgenden:
    * `A` ist ein Schnittstellentyp, der Klassentyp, der Delegattyp oder Arraytyp. Beachten Sie, dass `System.ValueType` und `System.Enum` sind Verweistypen, die diese Einschränkung zu erfüllen.
    * `A` Ein Typparameter, der bekannt ist, dass ein Verweistyp sein ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)).
*  Wenn die Einschränkung der werttypeinschränkung ist (`struct`), den Typ `A` erfüllen müssen, eine der folgenden:
    * `A` ist ein Strukturtyp oder Enum-Typ, aber keinen nullable-Typ. Beachten Sie, dass `System.ValueType` und `System.Enum` sind Verweistypen, die diese Einschränkung nicht erfüllen.
    * `A` Ein Typparameter mit der werttypeinschränkung ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)).
*  Wenn die Einschränkung die Konstruktoreinschränkung ist `new()`, den Typ `A` darf nicht sein `abstract` und muss über einen öffentlichen parameterlosen Konstruktor verfügen. Dies erfüllt wird, wenn eine der folgenden Aussagen zutrifft:
    * `A` ein Werttyp ist, da alle Werttypen einen öffentlichen Standardkonstruktor verfügen ([Standardkonstruktoren](types.md#default-constructors)).
    * `A` Ein Typparameter mit die Konstruktoreinschränkung ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)).
    * `A` Ein Typparameter mit der werttypeinschränkung ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)).
    * `A` ist eine Klasse, die nicht `abstract` und enthält eine explizit deklarierten `public` Konstruktor ohne Parameter.
    * `A` ist kein `abstract` und verfügt über einen Standardkonstruktor ([Standardkonstruktoren](classes.md#default-constructors)).

Ein Fehler während der Kompilierung tritt auf, wenn eine oder mehrere der Einschränkungen für einen Typparameter nicht durch die angegebenen Typargumente erfüllt werden.

Da der Typparameter nicht geerbt werden, sind niemals Einschränkungen entweder geerbt. Im folgenden Beispiel wird `D` muss die Einschränkung für den Typparameter angeben `T` , damit `T` erfüllt die Einschränkung auferlegt, die von der Basisklasse `B<T>`. Im Gegensatz dazu Klasse `E` eine Einschränkung müssen nicht angeben, da `List<T>` implementiert `IEnumerable` für alle `T`.

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>Typparameter

Ein Typparameter ist ein Bezeichner, die festlegen, ein Werttyp oder Verweistyp, der die Parameter zur Laufzeit gebunden wird.

```antlr
type_parameter
    : identifier
    ;
```

Da ein Typparameter mit vielen verschiedenen tatsächliche Typargumenten instanziiert werden kann, über Typparameter verfügen, leicht unterschiedliche Vorgänge und Einschränkungen als die anderen Typen. Dazu gehören:

*  Ein Typparameter kann nicht verwendet werden, direkt auf eine Basisklasse deklariert ([Basisklasse](classes.md#base-class)) oder Schnittstelle ([Variante Typparameterlisten](interfaces.md#variant-type-parameter-lists)).
*  Die Regeln für die Suche nach Membern auf, dass die Parameter der Einschränkungen, sofern vorhanden, abhängig, die auf den Typparameter angewendet werden. Sie werden ausführlich unter [Membersuche](expressions.md#member-lookup).
*  Die verfügbaren Konvertierungen für ein Typparameter hängen von den Einschränkungen, die ggf. auf den Typparameter angewendet. Sie werden ausführlich unter [implizite Konvertierungen, die im Zusammenhang mit Typparametern](conversions.md#implicit-conversions-involving-type-parameters) und [explizite dynamische Konvertierungen](conversions.md#explicit-dynamic-conversions).
*  Das Literal `null` kann nicht konvertiert werden, auf einen Typ, der durch einen Typparameter, mit Ausnahme angegeben wird, wenn der Typparameter bekannt ist, dass ein Verweistyp sein ([implizite Konvertierungen, die im Zusammenhang mit Typparametern](conversions.md#implicit-conversions-involving-type-parameters)). Allerdings eine `default` Ausdruck ([Standardwertausdrücke](expressions.md#default-value-expressions)) kann stattdessen verwendet werden. Darüber hinaus kann ein Wert mit einem Typ einen Typparameter verglichen werden, mit `null` mit `==` und `!=` ([Gleichheitsoperatoren](expressions.md#reference-type-equality-operators)), wenn der Typparameter der werttypeinschränkung aufweist.
*  Ein `new` Ausdruck ([Erstellung Objektausdrücke](expressions.md#object-creation-expressions)) kann nur mit einem Typparameter verwendet werden, wenn der Typparameter von eingeschränkt wird eine *Constructor_constraint* oder der Wert (Einschränkungstyp[ Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)).
*  Ein Typparameter kann nicht an einer beliebigen Stelle in einem Attribut verwendet werden.
*  Ein Typparameter kann nicht in einen Elementzugriff verwendet werden ([Memberzugriff](expressions.md#member-access)) oder Typname ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)) zum Identifizieren eines statischen Members oder eines geschachtelten Typs.
*  In unsicherem Code nicht als Typparameter verwendet werden eine *Unmanaged_type* ([Zeigertypen](unsafe-code.md#pointer-types)).

Als Typ sind Typparameter ausschließlich ein Kompilierzeit-Konstrukt. Zur Laufzeit ist jeden von Typparameter auf einen Typ zur Laufzeit gebunden, die durch Angabe von Typargument für die Deklaration des generischen Typs angegeben wurde. Daher der Typ einer Variablen, die mit einer Typ-Parameter wird zur Laufzeit, deklariert ein geschlossener konstruierter Typ sein ([offene und geschlossene Typen](types.md#open-and-closed-types)). Die Ausführung zur Laufzeit alle Anweisungen und Ausdrücke, die im Zusammenhang mit Typparametern verwendet den tatsächlichen Typ, der übergeben wurde, als Typargument für diesen Parameter.

## <a name="expression-tree-types"></a>Ausdrucksbaumstrukturtypen

***Ausdrucksbaumstrukturen*** zulassen von Lambda-Ausdrücke als Datenstrukturen statt ausführbaren Codes dargestellt werden. Ausdrucksbaumstrukturen sind die Werte der ***ausdrucksbaumstrukturtypen*** des Formulars `System.Linq.Expressions.Expression<D>`, wobei `D` jeden Delegattyp ist. Für den Rest dieser Spezifikation bezeichnen wir diese Typen mit der Kurzform `Expression<D>`.

Falls eine Konvertierung aus einem Lambdaausdruck vorhanden, auf einen Delegattyp ist `D`, eine Konvertierung auch vorhanden ist, um den Typ für die Ausdrucksbaumstruktur `Expression<D>`. Während die Konvertierung in einen Delegattyp einen Lambda-Ausdruck ein Delegat, die ausführbaren Code für den Lambda-Ausdruck verweist generiert, wird die Konvertierung in einen Typ für die Ausdrucksbaumstruktur ein ausdrucksbaumstrukturdarstellung des Lambda-Ausdrucks erstellt.

Ausdrucksbaumstrukturen sind effiziente Nutzung von Daten in-Memory-Darstellungen von Lambda-Ausdrücke, und stellen die Struktur des Lambda-Ausdrucks, transparent und explizit.

Genau wie einen Delegattyp `D`, `Expression<D>` Parameter- und Rückgabetypen, hat die sind identisch mit denen der `D`.

Das folgende Beispiel zeigt einen Lambda-Ausdruck als ausführbarer Code sowohl als eine Ausdrucksbaumstruktur. Da eine Konvertierung vorhanden ist, auf `Func<int,int>`, eine Konvertierung auch vorhanden ist, um `Expression<Func<int,int>>`:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

Befolgen diese Zuweisungen, die der Delegat `del` verweist auf eine Methode, die zurückgibt `x + 1`, und die Ausdrucksbaumstruktur `exp` verweist auf eine Datenstruktur, die den Ausdruck beschreibt `x => x + 1`.

Die genaue Definition des generischen Typs `Expression<D>` sowie die genauen Regeln für die Erstellung einer Ausdrucksbaumstruktur, wenn ein Lambda-Ausdruck in einen Typ für die Ausdrucksbaumstruktur konvertiert wird, sind außerhalb des Bereichs dieser Spezifikation.

Zwei Dinge sind wichtig, deutlich zu machen:

*  Nicht alle Lambda-Ausdrücke können in Ausdrucksbaumstrukturen konvertiert werden. Lambda-Ausdrücken mit Anweisung und Lambda-Ausdrücke mit Zuweisungsausdrücken, die können z. B. nicht dargestellt werden. In diesen Fällen ist eine Konvertierung weiterhin vorhanden, aber zum Zeitpunkt der Kompilierung fehl. Diese Ausnahmen werden ausführlich unter [anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions).
*   `Expression<D>` bietet eine Instanzmethode `Compile` erzeugt einen Delegaten vom Typ `D`:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    Aufrufen dieses Delegaten bewirkt, dass den Code, der von der Ausdrucksbaumstruktur dargestellt wird, ausgeführt werden. Daher erhalten die oben genannten Definitionen, del und del2 sind äquivalent, und die folgenden beiden Anweisungen müssen die gleiche Auswirkung:

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    Nach der Ausführung des Codes, `i1` und `i2` müssen beide den Wert `2`.

