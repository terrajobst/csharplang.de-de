---
ms.openlocfilehash: 088c4a77cecde490c556c44c239a3496f896582e
ms.sourcegitcommit: 4ddf18d000734c1b6d0a48127bf338086fc3f2c3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616141"
---
# <a name="types"></a>Typen

Die Typen der C# Sprache sind in zwei Hauptkategorien unterteilt: ***Werttypen*** und ***Verweis Typen***. Sowohl Werttypen als auch Verweis Typen können ***generische Typen***sein, die einen oder mehrere ***Typparameter***annehmen. Typparameter können sowohl Werttypen als auch Verweis Typen bestimmen.

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

Die letzte Kategorie von Typen, Zeiger, ist nur in unsicherem Code verfügbar. Dies wird in [Zeiger Typen](unsafe-code.md#pointer-types)ausführlicher erläutert.

Werttypen unterscheiden sich von Verweis Typen in den Variablen der Werttypen, die Ihre Daten direkt enthalten, wohingegen Variablen der Verweis Typen ***Verweise*** auf Ihre Daten speichern. Letztere werden als ***Objekte***bezeichnet. Bei Verweis Typen können zwei Variablen auf das gleiche Objekt verweisen, und so können Vorgänge in einer Variablen das Objekt beeinflussen, auf das von der anderen Variablen verwiesen wird. Bei Werttypen verfügen die Variablen jeweils über eine eigene Kopie der Daten, und es ist nicht möglich, dass sich der Vorgang auf einen anderen auswirkt.

C#das Typsystem ist einheitlich, sodass ein Wert eines beliebigen Typs als Objekt behandelt werden kann. Jeder Typ in C# ist direkt oder indirekt vom `object`-Klassentyp abgeleitet, und `object` ist die ultimative Basisklasse aller Typen. Werte von Verweistypen werden als Objekte behandelt, indem die Werte einfach als Typ `object` angezeigt werden. Werte von Werttypen werden als Objekte behandelt, indem Boxing-und Unboxing-Vorgänge ([Boxing und Unboxing](types.md#boxing-and-unboxing)) durchgeführt werden.

## <a name="value-types"></a>Werttypen

Ein Werttyp ist entweder ein Strukturtyp oder ein Enumerationstyp. C#stellt einen Satz vordefinierter Strukturtypen bereit, die als ***einfache Typen***bezeichnet werden. Die einfachen Typen werden mithilfe von reservierten Wörtern identifiziert.

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

Anders als bei einer Variablen eines Verweis Typs kann eine Variable eines Werttyps den Wert `null` nur enthalten, wenn der Werttyp ein Typ ist, der NULL-Werte zulässt.  Für jeden Werttyp, der nicht auf NULL festgelegt werden kann, gibt es einen entsprechenden Werte zulässt-Werttyp, der denselben Satz von Werten sowie den Wert `null`bezeichnet.

Durch die Zuweisung zu einer Variablen eines Werttyps wird eine Kopie des zugewiesenen Werts erstellt. Dies unterscheidet sich von der Zuweisung zu einer Variablen eines Verweis Typs, die den Verweis, aber nicht das durch den Verweis identifizierte Objekt kopiert.

### <a name="the-systemvaluetype-type"></a>Der Typ "System. ValueType"

Alle Werttypen erben implizit von der-Klasse `System.ValueType`, die wiederum von der-Klasse `object`erbt. Es ist nicht möglich, dass ein Typ von einem Werttyp abgeleitet wird. Werttypen sind daher implizit versiegelt ([versiegelte Klassen](classes.md#sealed-classes)).

Beachten Sie, dass `System.ValueType` nicht selbst ein *value_type*ist. Vielmehr handelt es sich um eine *class_type* , von der alle *value_type*s automatisch abgeleitet werden.

### <a name="default-constructors"></a>Standardkonstruktoren

Alle Werttypen deklarieren implizit einen öffentlichen Parameter losen Instanzenkonstruktor, der als ***Standardkonstruktor***bezeichnet wird. Der Standardkonstruktor gibt eine NULL initialisierte Instanz zurück, die als ***Standardwert*** für den Werttyp bezeichnet wird:

*  Der Standardwert für alle *Simple_Type*s ist der Wert, der von einem Bitmuster aller Nullen erzeugt wird:
    * Für `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`und `ulong`ist der Standardwert `0`.
    * Der Standardwert für `char`ist `'\x0000'`.
    * Der Standardwert für `float`ist `0.0f`.
    * Der Standardwert für `double`ist `0.0d`.
    * Der Standardwert für `decimal`ist `0.0m`.
    * Der Standardwert für `bool`ist `false`.
*  Bei einem *enum_type* `E`wird der Standardwert `0`in den Typ `E`konvertiert.
*  Bei einem *struct_type*ist der Standardwert der Wert, der erzeugt wird, indem alle Werttyp Felder auf ihren Standardwert und alle Verweistyp Felder auf `null`festgelegt werden.
*  Bei einem- *nullable_type* ist der Standardwert eine-Instanz, für die die `HasValue`-Eigenschaft false ist und die `Value`-Eigenschaft nicht definiert ist. Der Standardwert wird auch als NULL- ***Wert*** des Typs bezeichnet, der NULL-Werte zulässt.

Wie jeder andere Instanzkonstruktor wird der Standardkonstruktor eines Werttyps mit dem `new`-Operator aufgerufen. Aus Effizienzgründen ist diese Anforderung nicht dafür vorgesehen, dass die Implementierung einen konstruktorbefehl generiert. Im folgenden Beispiel werden die Variablen `i` und `j` beide mit 0 (null) initialisiert.

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

Da jeder Werttyp implizit über einen öffentlichen Parameter losen Instanzenkonstruktor verfügt, ist es nicht möglich, dass ein Strukturtyp eine explizite Deklaration eines Parameter losen Konstruktors enthält. Ein Strukturtyp ist jedoch zulässig, um parametrisierte Instanzkonstruktoren ([Konstruktoren](structs.md#constructors)) zu deklarieren.

### <a name="struct-types"></a>Strukturtypen

Ein Strukturtyp ist ein Werttyp, der Konstanten, Felder, Methoden, Eigenschaften, Indexer, Operatoren, Instanzkonstruktoren, statische Konstruktoren und Strukturtypen deklarieren kann. Die Deklaration von Strukturtypen wird in [Struktur Deklarationen](structs.md#struct-declarations)beschrieben.

### <a name="simple-types"></a>Einfache Typen

C#stellt einen Satz vordefinierter Strukturtypen bereit, die als ***einfache Typen***bezeichnet werden. Die einfachen Typen werden mithilfe von reservierten Wörtern identifiziert. diese reservierten Wörter sind jedoch einfach Aliase für vordefinierte Strukturtypen im `System` Namespace, wie in der folgenden Tabelle beschrieben.


| __Reserviertes Wort__ | __Alias-Typ__ |
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

Da ein einfacher Typ einen Strukturtyp Aliase, verfügt jeder einfache Typ über Member. Beispielsweise `int` die in `System.Int32` deklarierten Member und die Member, die von `System.Object`geerbt werden, und die folgenden Anweisungen sind zulässig:

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

Die einfachen Typen unterscheiden sich von anderen Strukturtypen dadurch, dass sie bestimmte zusätzliche Vorgänge ermöglichen:

*  Bei den meisten einfachen Typen können Werte erstellt werden, indem *Literale* ([Literale](lexical-structure.md#literals)) geschrieben werden. `123` ist z. b. ein Literaltyp `int`, und `'a'` ist ein Literaltyp `char`. C#stellt im Allgemeinen keine Bereitstellung von literalen von Strukturtypen bereit, und nicht standardmäßige Werte anderer Strukturtypen werden letztendlich immer durch Instanzkonstruktoren dieser Strukturtypen erstellt.
*  Wenn es sich bei den Operanden eines Ausdrucks um einfache Typkonstanten handelt, kann der Compiler den Ausdruck zur Kompilierzeit auswerten. Ein solcher Ausdruck wird als *constant_expression* ([Konstante Ausdrücke](expressions.md#constant-expressions)) bezeichnet. Ausdrücke mit Operatoren, die von anderen Strukturtypen definiert werden, werden nicht als Konstante Ausdrücke betrachtet.
*  Durch `const` Deklarationen ist es möglich, Konstanten der einfachen Typen ([Konstanten](classes.md#constants)) zu deklarieren. Es ist nicht möglich, Konstanten anderer Strukturtypen zu haben, aber ein ähnlicher Effekt wird durch `static readonly` Felder bereitgestellt.
*  Konvertierungen, die einfache Typen umfassen, können an der Auswertung von Konvertierungs Operatoren teilnehmen, die von anderen Strukturtypen definiert werden, aber ein benutzerdefinierter Konvertierungs Operator kann nie an der Auswertung eines anderen benutzerdefinierten Operators teilnehmen ([Auswertung benutzerdefinierter Konvertierungen](conversions.md#evaluation-of-user-defined-conversions)).

### <a name="integral-types"></a>Ganzzahlige Typen

C#unterstützt neun ganzzahlige Typen: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`und `char`. Die ganzzahligen Typen weisen die folgenden Größen und Wertebereiche auf:

*  Der `sbyte`-Typ stellt signierte 8-Bit-Ganzzahlen mit Werten zwischen-128 und 127 dar.
*  Der `byte`-Typ stellt ganze 8-Bit-Ganzzahlen ohne Vorzeichen mit Werten zwischen 0 und 255 dar.
*  Der `short` Typ stellt signierte 16-Bit-Ganzzahlen mit Werten zwischen-32768 und 32767 dar.
*  Der `ushort`-Typ stellt ganze 16-Bit-Ganzzahlen ohne Vorzeichen mit Werten zwischen 0 und 65535 dar.
*  Der `int`-Typ stellt signierte 32-Bit-Ganzzahlen mit Werten zwischen-2147483648 und 2147483647 dar.
*  Der `uint`-Typ stellt ganzzahlige 32-Bit-Ganzzahlen ohne Vorzeichen mit Werten zwischen 0 und 4294967295 dar.
*  Der `long`-Typ stellt signierte 64-Bit-Ganzzahlen mit Werten zwischen-9.223.372.036.854.775.808 und 9223372036854775807 dar.
*  Der `ulong`-Typ stellt ganzzahlige 64-Bit-Ganzzahlen ohne Vorzeichen mit Werten zwischen 0 und 18446744073709551615 dar.
*  Der `char`-Typ stellt ganze 16-Bit-Ganzzahlen ohne Vorzeichen mit Werten zwischen 0 und 65535 dar. Der Satz möglicher Werte für den `char` Typ entspricht dem Unicode-Zeichensatz. Obwohl `char` die gleiche Darstellung wie `ushort`hat, sind nicht alle Vorgänge zulässig, die für einen Typ zulässig sind.

Der unäre und binäre Operator des ganzzahligen Typs funktionieren immer mit der signierten 32-Bit-Genauigkeit, der 32-Bit-Genauigkeit ohne Vorzeichen, der 64-Bit-Genauigkeit mit Vorzeichen oder der 64-Bit-Genauigkeit ohne Vorzeichen:

*  Für die unären `+` und `~` Operatoren wird der Operand in den Typ `T`konvertiert, wobei `T` der erste von `int`, `uint`, `long`und `ulong` ist, die alle möglichen Werte des Operanden vollständig darstellen können. Der Vorgang wird dann mit der Genauigkeit des Typs `T`ausgeführt, und der Ergebnistyp ist `T`.
*  Für den unären `-` Operator wird der Operand in den Typ `T`konvertiert, wobei `T` der erste von `int` und `long` ist, der alle möglichen Werte des Operanden vollständig darstellen kann. Der Vorgang wird dann mit der Genauigkeit des Typs `T`ausgeführt, und der Ergebnistyp ist `T`. Der unäre `-`-Operator kann nicht auf Operanden vom Typ "`ulong`" angewendet werden.
*  Die Operatoren Binary `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`und `<=` werden die Operanden in den Typ `T`konvertiert, wobei `T` der erste von `int`, `uint`, `long`und `ulong` ist, die alle möglichen Werte beider Operanden vollständig darstellen können. Der Vorgang wird dann mit der Genauigkeit des Typs `T`ausgeführt, und der Ergebnistyp ist `T` (oder `bool` für die relationalen Operatoren). Es ist nicht zulässig, dass ein Operand vom Typ `long` und der andere vom Typ ist, der mit den binären Operatoren `ulong` ist.
*  Für die binären `<<`-und `>>` Operatoren wird der linke Operand in den Typ `T`konvertiert, wobei `T` der erste von `int`, `uint`, `long`und `ulong` ist, die alle möglichen Werte des Operanden vollständig darstellen können. Der Vorgang wird dann mit der Genauigkeit des Typs `T`ausgeführt, und der Ergebnistyp ist `T`.

Der `char` Typ wird als ganzzahliger Typ klassifiziert, aber er unterscheidet sich von den anderen ganzzahligen Typen auf zwei Arten:

*  Es gibt keine impliziten Konvertierungen anderen Typen in Typ `char`. Insbesondere wenn die Typen `sbyte`, `byte`und `ushort` Wertebereiche aufweisen, die mithilfe des `char` Typs vollständig Darstell Bar sind, sind implizite Konvertierungen von `sbyte`, `byte`oder `ushort` `char` nicht vorhanden.
*  Konstanten des `char` Typs müssen als *character_literal*s oder als *integer_literal*en in Kombination mit einer Umwandlung in den Typ `char`geschrieben werden. `(char)10` entspricht beispielsweise `'\x000A'`.

Mithilfe der Operatoren "`checked`" und "`unchecked`" werden Überlauf Prüfungen bei arithmetischen Operationen und Konvertierungen von ganzzahligen Typen gesteuert (die aktivierten und deaktivierten[Operatoren](expressions.md#the-checked-and-unchecked-operators)). In einem `checked` Kontext erzeugt ein Überlauf einen Kompilierzeitfehler oder bewirkt, dass eine `System.OverflowException` ausgelöst wird. In einem `unchecked` Kontext werden Überläufe ignoriert, und alle höherwertigen Bits, die nicht in den Zieltyp passen, werden verworfen.

### <a name="floating-point-types"></a>Gleit Komma Typen

C#unterstützt zwei Gleit Komma Typen: `float` und `double`. Die `float`-und `double` Typen werden mithilfe der 32-Bit-Formate für die einfache Genauigkeit und 64 Bit mit doppelter Genauigkeit mit doppelter 754 Genauigkeit dargestellt, die die folgenden Werte Sätze bereitstellen:

*  Positive NULL und negative Null. In den meisten Fällen verhalten sich positiv NULL und negatives NULL identisch mit dem einfachen Wert 0 (null), aber bestimmte Vorgänge unterscheiden zwischen den beiden ([Divisions Operator](expressions.md#division-operator)).
*  Positiv unendlich und minus unendlich. Infinities werden von solchen Vorgängen erzeugt, die eine Zahl ungleich 0 (null) durch Null aufteilen. `1.0 / 0.0` ergibt beispielsweise positive unendlich, und `-1.0 / 0.0` ergibt negative unendlich.
*  Der ***not-a-Number-*** Wert, häufig als NaN abgekürzt. Nane werden durch ungültige Gleit Komma Vorgänge erstellt, z. b. durch die Division von NULL durch Null.
*  Der endliche Satz von Werten ungleich 0 (null) `s * m * 2^e`, wobei `s` 1 oder-1 ist, und `m` und `e` durch den jeweiligen Gleit kommatyp bestimmt werden: für `float`, `0 < m < 2^24` und `-149 <= e <= 104`sowie `double`und `0 < m < 2^53`.`-1075 <= e <= 970` Denormalisierte Gleit Komma Zahlen gelten als gültige Werte ungleich 0 (null).

Der `float`-Typ kann Werte darstellen, die von ungefähr `1.5 * 10^-45` bis `3.4 * 10^38` mit einer Genauigkeit von 7 Ziffern reichen.

Der `double`-Typ kann Werte darstellen, die von ungefähr `5.0 * 10^-324` bis `1.7 × 10^308` mit einer Genauigkeit von 15-16 Ziffern reichen.

Wenn einer der Operanden eines binären Operators ein Gleit kommatyp ist, muss der andere Operand ein ganzzahliger Typ oder ein Gleit kommatyp sein, und der Vorgang wird wie folgt ausgewertet:

*  Wenn einer der Operanden ein ganzzahliger Typ ist, wird dieser Operand in den Gleit kommatyp des anderen Operanden konvertiert.
*  Wenn einer der Operanden vom Typ `double`ist, wird der andere Operand in `double`konvertiert, der Vorgang wird mit mindestens `double` Bereich und Genauigkeit durchgeführt, und der Ergebnistyp wird `double` (oder `bool` für die relationalen Operatoren).
*  Andernfalls wird der Vorgang mit mindestens `float` Bereich und Genauigkeit durchgeführt, und der Ergebnistyp wird `float` (oder für die relationalen Operatoren `bool`).

Die Gleit Komma Operatoren, einschließlich der Zuweisungs Operatoren, führen niemals zu Ausnahmen. In Ausnahmefällen wird von Gleit Komma Vorgängen, wie unten beschrieben, NULL, unendlich oder NaN erzeugt:

*  Wenn das Ergebnis einer Gleit Komma Operation für das Zielformat zu klein ist, wird das Ergebnis des Vorgangs positiv 0 (null) oder negativ 0 (null).
*  Wenn das Ergebnis einer Gleit Komma Operation für das Zielformat zu groß ist, wird das Ergebnis des Vorgangs positiv unendlich oder negativ unendlich.
*  Wenn ein Gleit Komma Vorgang ungültig ist, wird das Ergebnis des Vorgangs "NaN".
*  Wenn ein oder beide Operanden einer Gleit Komma Operation NaN sind, wird das Ergebnis des Vorgangs Nan.

Gleit Komma Operationen können mit höherer Genauigkeit ausgeführt werden als der Ergebnistyp des Vorgangs. Beispielsweise unterstützen einige Hardwarearchitekturen einen "Extended"-oder "long Double"-Gleit kommatyp mit größerem Bereich und präziser als der `double`-Typ und führen implizit alle Gleit Komma Vorgänge mit diesem Typ höherer Genauigkeit aus. Nur zu hohen Leistungseinbußen können solche Hardwarearchitekturen zum Ausführen von Gleit Komma Vorgängen mit geringerer Genauigkeit gemacht werden, und C# es ist nicht erforderlich, dass eine Implementierung sowohl die Leistung als auch die Genauigkeit beeinträchtigt. , der für alle Gleit Komma Operationen verwendet werden soll. Abgesehen von der Bereitstellung präziseren Ergebnisse hat dies nur selten messbare Auswirkungen. In Ausdrücken der Form `x * y / z`, wobei die Multiplikation ein Ergebnis erzeugt, das außerhalb des `double` Bereichs liegt, aber die nachfolgende Division das temporäre Ergebnis wieder in den `double` Bereich bringt, wird die Tatsache, dass der Ausdruck ausgewertet wird, in einem ein höheres Bereichs Format kann bewirken, dass ein endliches Ergebnis anstelle von unendlich erzeugt wird.

### <a name="the-decimal-type"></a>Der Decimal-Typ.

Der `decimal`-Typ ist ein für Finanz-und Währungsberechnungen geeigneter 128-Bit-Datentyp. Der `decimal`-Typ kann Werte zwischen `1.0 * 10^-28` und ungefähr `7.9 * 10^28` mit 28-29 signifikanten Ziffern darstellen.

Der endliche Satz von Werten vom Typ `decimal` hat die Form `(-1)^s * c * 10^-e`, wobei das Vorzeichen `s` 0 oder 1 ist, der Koeffizienten `c` von `0 <= *c* < 2^96`angegeben wird und die Skalierungs `e` `0 <= e <= 28`. Der `decimal`-Typ unterstützt keine signierten Nullen, Infinities oder NaN-. Ein `decimal` wird als eine ganze Zahl mit einer Länge von 96 dargestellt, die durch eine Potenz von zehn skaliert wird. Bei `decimal`s mit einem absoluten Wert, der kleiner als `1.0m`ist, entspricht der Wert exakt dem 28. Dezimaltrennzeichen, aber nicht weiter. Bei `decimal`s mit einem absoluten Wert, der größer oder gleich `1.0m`ist, entspricht der Wert exakt 28 oder 29 Ziffern. Im Gegensatz zu den Datentypen `float` und `double` können dezimale Bruchzahlen wie 0,1 genau in der `decimal` Darstellung dargestellt werden. In den `float`-und `double` Darstellungen sind solche Zahlen häufig unendliche Bruchzahlen, sodass diese Darstellungen anfälliger für Fehler sind.

Wenn einer der Operanden eines binären Operators vom Typ `decimal`ist, muss der andere Operand ein ganzzahliger Typ oder vom Typ `decimal`sein. Wenn ein ganzzahliger Typoperand vorhanden ist, wird er in `decimal` konvertiert, bevor der Vorgang durchgeführt wird.

Das Ergebnis eines Vorgangs für Werte des Typs `decimal` ist, dass die Berechnung eines exakten Ergebnisses (wie für jeden Operator definiert) zu einem Ergebnis führt und dann an die Darstellung angepasst wird. Die Ergebnisse werden auf den nächstgelegenen darstellbaren Wert gerundet und, wenn ein Ergebnis gleich nah bei zwei darstellbaren Werten ist, bis zu dem Wert, der eine gerade Zahl in der am wenigsten wichtigen Ziffern Position aufweist (Dies wird als "Banker srundung" bezeichnet). Ein NULL-Ergebnis hat immer ein Vorzeichen von 0 und eine Skala von 0.

Wenn eine arithmetische decimal-Operation einen Wert erzeugt, der kleiner als oder gleich `5 * 10^-29` im absoluten Wert ist, wird das Ergebnis des Vorgangs 0 (null). Wenn ein `decimal` arithmetischer Vorgang ein Ergebnis erzeugt, das für das `decimal` Format zu groß ist, wird ein `System.OverflowException` ausgelöst.

Der `decimal` Typ hat eine höhere Genauigkeit, aber einen kleineren Bereich als die Gleit Komma Typen. Folglich können Konvertierungen von Gleit Komma Typen in `decimal` Überlauf Ausnahmen erzeugen, und Konvertierungen aus `decimal` in die Gleit Komma Typen können zu Genauigkeits Verlusten führen. Aus diesen Gründen sind keine impliziten Konvertierungen zwischen Gleit Komma Typen und `decimal`vorhanden, und ohne explizite Umwandlungen ist es nicht möglich, Gleit Komma-und `decimal` Operanden im gleichen Ausdruck zu mischen.

### <a name="the-bool-type"></a>Der boolesche Typ

Der `bool` Typ stellt boolesche logische Mengen dar. Die möglichen Werte vom Typ `bool` sind `true` und `false`.

Zwischen `bool` und anderen Typen sind keine Standard Konvertierungen vorhanden. Der `bool` Typ ist insbesondere eindeutig und getrennt von den ganzzahligen Typen, und ein `bool` Wert kann nicht anstelle eines ganzzahligen Werts verwendet werden und umgekehrt.

In den Programmiersprachen C++ C und kann ein ganzzahliger Wert von 0 (null) oder ein Gleit Komma Wert oder ein NULL-Zeiger in den booleschen Wert `false`konvertiert werden, und ein ganzzahliger oder Gleit Komma Wert ungleich NULL oder ein nicht-NULL-Zeiger kann in den booleschen Wert `true`konvertiert werden. In C#werden solche Konvertierungen durch explizites Vergleichen eines ganzzahligen oder Gleit Komma Werts mit 0 (null) oder durch explizites Vergleichen eines Objekt Verweises mit `null`erreicht.

### <a name="enumeration-types"></a>Enumerationstypen

Ein Enumerationstyp ist ein eindeutiger Typ mit benannten Konstanten. Jeder Enumerationstyp verfügt über einen zugrunde liegenden Typ, der `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` oder `ulong`sein muss. Der Satz von Werten des Enumerationstyps ist mit dem Satz von Werten des zugrunde liegenden Typs identisch. Werte des Enumerationstyps sind nicht auf die Werte der benannten Konstanten beschränkt. Enumerationstypen werden durch Enumerationsdeklarationen (Enumerationsdeklarationen[)](enums.md#enum-declarations)definiert.

### <a name="nullable-types"></a>Auf NULL festlegbare Typen

Ein Typ, der NULL-Werte zulässt, kann alle Werte seines ***zugrunde liegenden Typs*** und einen zusätzlichen NULL-Wert darstellen. Ein Typ, der NULL-Werte zulässt, wird `T?`geschrieben, wobei `T` der zugrunde liegende Typ ist. Diese Syntax ist eine Kurzform für `System.Nullable<T>`, und die beiden Formen können austauschbar verwendet werden.

Ein ***Werttyp*** , der nicht auf NULL festgelegt werden kann, ist umgekehrt ein beliebiger Werttyp als `System.Nullable<T>` und seine Kurzform `T?` (für alle `T`) sowie alle Typparameter, die auf einen Werttyp beschränkt sind, der keine NULL-Werte zulässt (d. h. alle Typparameter mit einer `struct`-Einschränkung). Der `System.Nullable<T>` Typ gibt die Werttyp Einschränkung für `T` ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) an. Dies bedeutet, dass der zugrunde liegende Typ eines Typs, der NULL-Werte zulässt, jeder Werttyp sein kann, der keine NULL-Werte zulässt. Der zugrunde liegende Typ eines Typs, der NULL-Werte zulässt, kann kein Typ oder Verweistyp sein, der NULL-Werte zulässt. `int??` und `string?` sind z. b. ungültige Typen.

Eine Instanz eines Typs, der NULL-Werte zulässt `T?` hat zwei öffentliche schreibgeschützte Eigenschaften:

*  Eine `HasValue`-Eigenschaft des Typs `bool`
*  Eine `Value`-Eigenschaft des Typs `T`

Eine-Instanz, für die `HasValue` true ist, wird als ungleich NULL bezeichnet. Eine Instanz, die nicht NULL ist, enthält einen bekannten Wert, und `Value` gibt diesen Wert zurück.

Eine-Instanz, für die `HasValue` false ist, wird als NULL bezeichnet. Eine NULL-Instanz hat einen nicht definierten Wert. Der Versuch, den `Value` einer NULL-Instanz zu lesen, bewirkt, dass eine `System.InvalidOperationException` ausgelöst wird. Der Prozess des Zugriffs auf die `Value`-Eigenschaft einer Instanz, die NULL-Werte zulässt, wird als ***zum Entpacken***bezeichnet.

Zusätzlich zum Standardkonstruktor hat jeder Typ, der NULL-Werte zulässt, `T?` einen öffentlichen Konstruktor, der ein einzelnes Argument vom Typ "`T`" annimmt. Wenn ein Wert `x` vom Typ `T`ist, wird ein Konstruktoraufruf des Formulars ausgeführt.

```csharp
new T?(x)
```
erstellt eine Instanz von, die keine NULL-`T?` ist, für die die `Value` Eigenschaft `x`ist. Das Erstellen einer nicht-NULL-Instanz eines Typs, der NULL-Werte zulässt, wird als ***Wrapping***bezeichnet.

Implizite Konvertierungen sind vom `null` literalen für `T?` ([null-literalkonvertierungen](conversions.md#null-literal-conversions)) und von `T` bis `T?` ([implizite Konvertierungen](conversions.md#implicit-nullable-conversions), die NULL-Werte zulassen) verfügbar.

## <a name="reference-types"></a>Verweistypen

Bei einem Verweistyp handelt es sich um einen Klassentyp, einen Schnittstellentyp, einen Arraytyp oder einen Delegattyp.

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

Ein Verweistyp Wert ist ein Verweis auf eine ***Instanz*** des Typs, wobei es sich um einen Verweis handelt, der als ***Objekt***bezeichnet wird. Der spezielle Wert `null` ist mit allen Verweis Typen kompatibel und gibt an, dass keine Instanz vorhanden ist.

### <a name="class-types"></a>Klassentypen

Ein Klassentyp definiert eine Datenstruktur, die Datenmember (Konstanten und Felder), Funktionsmember (Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren, Instanzkonstruktoren, destrukturatoren und statische Konstruktoren) und die in der Struktur enthaltenen Typen enthält. Klassentypen unterstützen Vererbung, einen Mechanismus, bei dem abgeleitete Klassen die Basisklassen erweitern und spezialisieren können. Instanzen von Klassentypen werden mithilfe von *object_creation_expression*s erstellt ([Objekt Erstellungs Ausdrücke](expressions.md#object-creation-expressions)).

Klassentypen werden in [Klassen](classes.md)beschrieben.

Bestimmte vordefinierte Klassentypen haben in der C# Sprache eine besondere Bedeutung, wie in der folgenden Tabelle beschrieben.


| __Klassentyp__     | __Beschreibung__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | Die ultimative Basisklasse aller anderen Typen. Siehe [Objekttyp](types.md#the-object-type). | 
| `System.String`    | Der Zeichen Folgentyp C# der Sprache. Siehe [den String-Typ](types.md#the-string-type).         |
| `System.ValueType` | Die Basisklasse aller Werttypen. Siehe [den Typ System. ValueType](types.md#the-systemvaluetype-type).          |
| `System.Enum`      | Die Basisklasse aller Enumerationstypen. Siehe [-](enums.md)Auffinden.              |
| `System.Array`     | Die Basisklasse aller Array Typen. Siehe [Arrays](arrays.md).             |
| `System.Delegate`  | Die Basisklasse aller Delegattypen. Siehe [Delegaten](delegates.md).          |
| `System.Exception` | Die Basisklasse aller Ausnahme Typen. Siehe [Ausnahmen](exceptions.md).         |

### <a name="the-object-type"></a>Der Objekttyp

Der `object`-Klassentyp ist die ultimative Basisklasse aller anderen Typen. Jeder Typ C# direkt oder indirekt wird vom `object`-Klassentyp abgeleitet.

Das Schlüsselwort `object` ist einfach ein Alias für die vordefinierte Klasse `System.Object`.

### <a name="the-dynamic-type"></a>Der dynamische Typ

Der `dynamic` Typ kann, wie `object`, auf jedes beliebige Objekt verweisen. Wenn Operatoren auf Ausdrücke vom Typ `dynamic`angewendet werden, wird ihre Auflösung verzögert, bis das Programm ausgeführt wird. Wenn der Operator daher nicht auf das Objekt angewendet werden kann, auf das verwiesen wird, wird während der Kompilierung kein Fehler angegeben. Stattdessen wird eine Ausnahme ausgelöst, wenn die Auflösung des Operators zur Laufzeit fehlschlägt.

Der Zweck besteht darin, dynamische Bindungen zuzulassen, die im Detail unter [dynamische Bindung](expressions.md#dynamic-binding)beschrieben werden.

`dynamic` gilt als identisch mit `object`, außer in den folgenden Punkten:

*  Vorgänge für Ausdrücke vom Typ `dynamic` können dynamisch gebunden werden ([dynamische Bindung](expressions.md#dynamic-binding)).
*  Beim Typrückschluss ([Typrückschluss](expressions.md#type-inference)) wird die `dynamic` über `object` bevorzugt, wenn beide Kandidaten sind.

Aufgrund dieser Äquivalenz enthält Folgendes:

*  Es gibt eine implizite Identitäts Konvertierung zwischen `object` und `dynamic`und zwischen konstruierten Typen, die beim Ersetzen von `dynamic` durch `object`
*  Implizite und explizite Konvertierungen in und aus `object` gelten auch für und von `dynamic`.
*  Methoden Signaturen, die beim Ersetzen von `dynamic` durch `object` identisch sind, gelten als dieselbe Signatur.
*  Der Typ `dynamic` kann zur Laufzeit nicht von `object` unterschieden werden.
*  Ein Ausdruck des Typs `dynamic` wird als ***dynamischer Ausdruck***bezeichnet.

### <a name="the-string-type"></a>Der Zeichenfolgentyp

Der `string` Typ ist ein versiegelter Klassentyp, der direkt von `object`erbt. Instanzen der `string`-Klasse stellen Unicode-Zeichen folgen dar.

Werte des `string` Typs können als Zeichen folgen Literale ([Zeichenfolgenliterale](lexical-structure.md#string-literals)) geschrieben werden.

Das Schlüsselwort `string` ist einfach ein Alias für die vordefinierte Klasse `System.String`.

### <a name="interface-types"></a>Schnittstellentypen

Eine Schnittstelle definiert einen Vertrag. Eine Klasse oder Struktur, die eine Schnittstelle implementiert, muss ihren Vertrag einhalten. Eine Schnittstelle kann von mehreren Basis Schnittstellen erben, und eine Klasse oder Struktur kann mehrere Schnittstellen implementieren.

Schnittstellentypen werden unter [Schnittstellen](interfaces.md)beschrieben.

### <a name="array-types"></a>Arraytypen

Ein Array ist eine Datenstruktur, die NULL oder mehr Variablen enthält, auf die über berechnete Indizes zugegriffen wird. Die Variablen, die in einem Array enthalten sind, auch als Elemente des Arrays bezeichnet, sind vom selben Typ, und dieser Typ wird als Elementtyp des Arrays bezeichnet.

Array Typen werden in [Arrays](arrays.md)beschrieben.

### <a name="delegate-types"></a>Delegattypen

Bei einem Delegaten handelt es sich um eine Datenstruktur, die auf eine oder mehrere Methoden verweist. Bei Instanzmethoden bezieht sie sich auch auf ihre entsprechenden Objektinstanzen.

Das nächstliegende Äquivalent eines Delegaten in C C++ oder ist ein Funktionszeiger, während ein Funktionszeiger nur auf statische Funktionen verweisen kann, kann ein Delegat sowohl auf statische Methoden als auch auf Instanzmethoden verweisen. Im letzteren Fall speichert der Delegat nicht nur einen Verweis auf den Einstiegspunkt der Methode, sondern auch einen Verweis auf die Objektinstanz, für die die Methode aufgerufen werden soll.

Delegattypen [werden in](delegates.md)Delegaten beschrieben.

## <a name="boxing-and-unboxing"></a>Boxing und Unboxing

Das Konzept von Boxing und Unboxing ist für C#das Typsystem von zentraler Bedeutung. Sie bietet eine Brücke zwischen *value_type*s und *reference_type*s, indem es ermöglicht wird, dass jeder Wert eines *value_type* in und aus dem Typ `object`konvertiert werden kann. Boxing und Unboxing ermöglichen eine einheitliche Ansicht des Typsystems, wobei ein Wert eines beliebigen Typs letztendlich als Objekt behandelt werden kann.

### <a name="boxing-conversions"></a>Boxing-Konvertierungen

Eine Boxing-Konvertierung ermöglicht eine implizite Konvertierung einer *value_type* in eine *reference_type*. Die folgenden boxkonvertierungen sind vorhanden:

*  Von allen *value_type* bis zum Typ `object`.
*  Von allen *value_type* bis zum Typ `System.ValueType`.
*  Von allen *non_nullable_value_type* bis *INTERFACE_TYPE* , die vom *value_type*implementiert werden.
*  Von allen *nullable_type* bis *INTERFACE_TYPE* , die vom zugrunde liegenden Typ des *nullable_type*implementiert werden.
*  Von allen *enum_type* bis zum Typ `System.Enum`.
*  Von allen *nullable_type* mit einem zugrunde liegenden *enum_type* bis zum Typ `System.Enum`.
*  Beachten Sie, dass eine implizite Konvertierung von einem Typparameter als Boxing-Konvertierung ausgeführt wird, wenn Sie zur Laufzeit von einem Werttyp in einen Verweistyp konvertiert wird ([implizite Konvertierungen mit Typparametern](conversions.md#implicit-conversions-involving-type-parameters)).

Das Boxing eines Werts einer *non_nullable_value_type* besteht aus der Zuordnung einer Objektinstanz und dem Kopieren des *non_nullable_value_type* Werts in diese Instanz.

Das Boxing eines Werts einer *nullable_type* erzeugt einen NULL-Verweis, wenn es sich um den `null`-Wert (`HasValue` `false`) oder das Entpacken und Boxing des zugrunde liegenden Werts andernfalls handelt.

Der eigentliche Prozess des Boxens eines Werts eines *non_nullable_value_type* wird am besten erläutert, indem das vorhanden sein einer generischen ***Boxing-Klasse***dargestellt wird, die sich so verhält, als wäre sie wie folgt deklariert:

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

Boxing eines Werts `v` vom Typ "`T`" besteht jetzt aus der Ausführung des Ausdrucks `new Box<T>(v)`und der Rückgabe der resultierenden Instanz als Wert des Typs `object`. Folglich werden die Anweisungen
```csharp
int i = 123;
object box = i;
```
konzeptionell entsprechen
```csharp
int i = 123;
object box = new Box<int>(i);
```

Eine Boxing-Klasse wie `Box<T>` oben ist nicht vorhanden, und der dynamische Typ eines geschachtelten Werts ist nicht tatsächlich ein Klassentyp. Stattdessen kann ein geachtelter Wert vom Typ `T` den dynamischen Typ `T`haben, und eine dynamische Typüberprüfung mit dem `is`-Operator kann einfach auf den Typ `T`verweisen. Ein auf ein Objekt angewendeter
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
die Zeichenfolge "`Box contains an int`" wird in der Konsole ausgegeben.

Eine Boxing-Konvertierung impliziert das Erstellen einer Kopie des Werts, der gekapselt wird. Dies unterscheidet sich von der Konvertierung eines *reference_type* in den Typ `object`, bei dem der Wert weiterhin auf dieselbe Instanz verweist und einfach als weniger abgeleiteter Typ `object`angesehen wird. Beispielsweise mit der Deklaration
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
Gibt den Wert 10 in der Konsole aus, da der implizite Boxing-Vorgang, der bei der Zuweisung von `p` zu `box` auftritt, bewirkt, dass der Wert von `p` kopiert wird. Wenn `Point` stattdessen als `class` deklariert worden wäre, würde der Wert 20 ausgegeben werden, da `p` und `box` auf dieselbe Instanz verweisen würden.

### <a name="unboxing-conversions"></a>Unboxing-Konvertierungen

Eine Unboxing-Konvertierung ermöglicht das explizite Konvertieren eines *reference_type* in eine *value_type*. Die folgenden Unboxing-Konvertierungen sind vorhanden:

*  Vom Typ `object` bis *value_type*.
*  Vom Typ `System.ValueType` bis *value_type*.
*  Von allen *INTERFACE_TYPE* bis zu *non_nullable_value_type* , die die *INTERFACE_TYPE*implementiert.
*  Von allen *INTERFACE_TYPE* zu beliebigen *nullable_type* , deren zugrunde liegender Typ den *INTERFACE_TYPE*implementiert.
*  Vom Typ `System.Enum` bis *enum_type*.
*  Vom Typ `System.Enum` bis *nullable_type* mit einem zugrunde liegenden *enum_type*.
*  Beachten Sie, dass eine explizite Konvertierung in einen Typparameter als Unboxing-Konvertierung ausgeführt wird, wenn Sie zur Laufzeit von einem Verweistyp in einen Werttyp ([explizite dynamische Konvertierungen](conversions.md#explicit-dynamic-conversions)) konvertiert wird.

Ein Unboxing-Vorgang für eine *non_nullable_value_type* besteht darin, zuerst zu überprüfen, ob die Objektinstanz ein geachtelter Wert der angegebenen *non_nullable_value_type*ist, und dann den Wert aus der-Instanz zu kopieren.

Beim Unboxing in eine *nullable_type* wird der NULL-Wert des *nullable_type* erzeugt, wenn der Quell Operand `null`ist, oder das umschließende Ergebnis des Unboxing der Objektinstanz in den zugrunde liegenden Typ des *nullable_type* andernfalls.

Bei der im vorherigen Abschnitt beschriebenen imaginären Boxingklasse wird eine Unboxing-Konvertierung eines Objekts `box` zu einem *value_type* `T` aus der Ausführung des Ausdrucks `((Box<T>)box).value`. Folglich werden die Anweisungen
```csharp
object box = 123;
int i = (int)box;
```
konzeptionell entsprechen
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

Damit eine Unboxing-Konvertierung in eine angegebene *non_nullable_value_type* zur Laufzeit erfolgreich ausgeführt werden kann, muss der Wert des Quell Operanden ein Verweis auf einen geachtelten Wert dieses *non_nullable_value_type*sein. Wenn der Quell Operand `null`ist, wird eine `System.NullReferenceException` ausgelöst. Wenn der Quell Operand ein Verweis auf ein inkompatibles Objekt ist, wird ein `System.InvalidCastException` ausgelöst.

Damit eine Unboxing-Konvertierung in eine angegebene *nullable_type* zur Laufzeit erfolgreich ausgeführt werden kann, muss der Wert des Quell Operanden entweder `null` oder ein Verweis auf einen geachtelten Wert der zugrunde liegenden *non_nullable_value_type* der *nullable_type*sein. Wenn der Quell Operand ein Verweis auf ein inkompatibles Objekt ist, wird ein `System.InvalidCastException` ausgelöst.

## <a name="constructed-types"></a>Konstruierte Typen

Eine generische Typdeklaration bezeichnet allein einen ***ungebundenen generischen Typ*** , der als "Blueprint" verwendet wird, um viele verschiedene Typen mithilfe von ***Typargumenten***zu bilden. Die Typargumente werden in spitzen Klammern (`<` und `>`) direkt nach dem Namen des generischen Typs geschrieben. Ein Typ, der mindestens ein Typargument enthält, wird als ***konstruierter Typ***bezeichnet. Ein konstruierter Typ kann an den meisten Stellen in der Sprache verwendet werden, in der ein Typname angezeigt werden kann. Ein ungebundener generischer Typ kann nur innerhalb eines *typeof_expression* ([der typeof-Operator](expressions.md#the-typeof-operator)) verwendet werden.

Konstruierte Typen können auch in Ausdrücken als einfache Namen ([einfache Namen](expressions.md#simple-names)) oder beim Zugriff auf einen Member ([Member Access](expressions.md#member-access)) verwendet werden.

Wenn ein *namespace_or_type_name* ausgewertet wird, werden nur generische Typen mit der richtigen Anzahl von Typparametern berücksichtigt. Daher ist es möglich, denselben Bezeichner zu verwenden, um unterschiedliche Typen zu identifizieren, sofern die Typen eine unterschiedliche Anzahl von Typparametern aufweisen. Dies ist nützlich, wenn generische und nicht generische Klassen in demselben Programm gemischt werden:

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

Ein *TYPE_NAME* kann einen konstruierten Typ identifizieren, obwohl er keine Typparameter direkt angibt. Dies kann vorkommen, wenn ein Typ in einer generischen Klassen Deklaration geschachtelt ist und der Instanztyp der enthaltenden Deklaration implizit für die Namenssuche (geschachtelte[Typen in generischen Klassen](classes.md#nested-types-in-generic-classes)) verwendet wird:

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

In unsicherem Code kann ein konstruierter Typ nicht als *unmanaged_type* ([Zeiger Typen](unsafe-code.md#pointer-types)) verwendet werden.

### <a name="type-arguments"></a>Typargumente

Jedes Argument in einer Typargument Liste ist einfach ein *Typ*.

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

In unsicherem Code ([unsicherer Code](unsafe-code.md)) ist ein *type_argument* möglicherweise kein Zeigertyp. Jedes Typargument muss alle Einschränkungen für den entsprechenden Typparameter ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) erfüllen.

### <a name="open-and-closed-types"></a>Open-und Closed-Typen

Alle Typen können als ***offene*** oder ***geschlossene Typen***klassifiziert werden. Ein offener Typ ist ein Typ, der Typparameter umfasst. Genauer gesagt:

*  Ein Typparameter definiert einen geöffneten Typ.
*  Ein Arraytyp ist nur dann ein offener Typ, wenn sein Elementtyp ein offener Typ ist.
*  Ein konstruierter Typ ist ein offener Typ, wenn es sich bei mindestens einem Typargument um einen geöffneten Typ handelt. Ein konstruierter, von einem Typ erstellter Typ ist ein offener Typ, wenn es sich bei mindestens einem Typargument oder den Typargumenten der enthaltenden Typen um einen geöffneten Typ handelt.

Ein geschlossener Typ ist ein Typ, bei dem es sich nicht um einen geöffneten Typ handelt.

Zur Laufzeit wird der gesamte Code in einer generischen Typdeklaration im Kontext eines geschlossenen konstruierten Typs ausgeführt, der durch Anwenden von Typargumenten auf die generische Deklaration erstellt wurde. Jeder Typparameter innerhalb des generischen Typs ist an einen bestimmten Lauf Zeittyp gebunden. Die Lauf Zeit Verarbeitung aller Anweisungen und Ausdrücke tritt immer bei geschlossenen Typen auf, und offene Typen werden nur während der Kompilierungszeit verarbeitet.

Jeder geschlossene konstruierte Typ verfügt über einen eigenen Satz statischer Variablen, die nicht gemeinsam mit anderen geschlossenen konstruierten Typen verwendet werden. Da ein offener Typ zur Laufzeit nicht vorhanden ist, sind keine statischen Variablen mit einem geöffneten Typ verknüpft. Zwei geschlossene konstruierte Typen weisen denselben Typ auf, wenn Sie aus demselben ungebundenen generischen Typ erstellt werden und die entsprechenden Typargumente denselben Typ haben.

### <a name="bound-and-unbound-types"></a>Gebundene und ungebundene Typen

Der Begriff " ***ungebundener Typ*** " verweist auf einen nicht generischen Typ oder einen ungebundenen generischen Typ. Der Begriff ***gebundene Typ*** verweist auf einen nicht generischen Typ oder einen konstruierten Typ.

Ein ungebundener Typ verweist auf die durch eine Typdeklaration deklarierte Entität. Ein ungebundener generischer Typ ist nicht selbst ein Typ und kann nicht als Typ einer Variablen, eines Arguments oder eines Rückgabewerts oder als Basistyp verwendet werden. Das einzige Konstrukt, in dem auf einen ungebundenen generischen Typ verwiesen werden kann, ist der `typeof` Ausdruck ([der typeof-Operator](expressions.md#the-typeof-operator)).

### <a name="satisfying-constraints"></a>Erfüllen von Einschränkungen

Wenn auf einen konstruierten Typ oder eine generische Methode verwiesen wird, werden die angegebenen Typargumente mit den Typparameter Einschränkungen überprüft, die für den generischen Typ oder die generische Methode deklariert sind ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)). Für jede `where`-Klausel wird das Typargument `A`, das dem benannten Typparameter entspricht, wie folgt gegen jede Einschränkung überprüft:

*  Wenn es sich bei der Einschränkung um einen Klassentyp, einen Schnittstellentyp oder einen Typparameter handelt, können `C` diese Einschränkung mit den bereitgestellten Typargumenten darstellen, die für Typparameter ersetzt werden, die in der Einschränkung angezeigt werden. Um die Einschränkung zu erfüllen, muss der Typ `A` in den Typ konvertiert werden können, der von einem der folgenden Typen `C` kann:
    * Eine Identitäts Konvertierung ([Identitäts Konvertierung](conversions.md#identity-conversion))
    * Implizite Verweis Konvertierung ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions))
    * Eine Boxing-Konvertierung ([boxkonvertierungen](conversions.md#boxing-conversions)), vorausgesetzt, dass TYPE a ein Werttyp ist, der keine NULL-Werte zulässt.
    * Eine implizite Verweis-, Boxing-oder Typparameter Konvertierung von einem Typparameter `A` in `C`.
*  Wenn es sich bei der Einschränkung um die Verweistyp Einschränkung (`class`) handelt, muss der Typ `A` einen der folgenden Bedingungen erfüllen:
    * `A` ist ein Schnittstellentyp, Klassentyp, Delegattyp oder Arraytyp. Beachten Sie, dass `System.ValueType` und `System.Enum` Verweis Typen sind, die diese Einschränkung erfüllen.
    * `A` ist ein Typparameter, bei dem es sich um einen Verweistyp ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) handelt.
*  Wenn es sich bei der Einschränkung um die Werttyp Einschränkung (`struct`) handelt, muss der Typ `A` einen der folgenden Bedingungen erfüllen:
    * `A` ist ein Strukturtyp oder ein Aufzählungs Typ, aber kein Typ, der NULL-Werte zulässt. Beachten Sie, dass `System.ValueType` und `System.Enum` Verweis Typen sind, die diese Einschränkung nicht erfüllen.
    * `A` ist ein Typparameter mit der Werttyp Einschränkung ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)).
*  Wenn die Einschränkung die konstruktoreinschränkungs `new()`ist, darf der Typ `A` nicht `abstract` sein und muss über einen öffentlichen Parameter losen Konstruktor verfügen. Dies ist erfüllt, wenn eine der folgenden Bedingungen zutrifft:
    * `A` ist ein Werttyp, da alle Werttypen über einen öffentlichen Standardkonstruktor ([Standardkonstruktoren](types.md#default-constructors)) verfügen.
    * `A` ist ein Typparameter mit der Konstruktoreinschränkung ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)).
    * `A` ist ein Typparameter mit der Werttyp Einschränkung ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)).
    * `A` ist eine Klasse, die nicht `abstract` ist und einen explizit deklarierten `public`-Konstruktor ohne Parameter enthält.
    * `A` ist nicht `abstract` und verfügt über einen Standardkonstruktor ([Standardkonstruktoren](classes.md#default-constructors)).

Ein Kompilierzeitfehler tritt auf, wenn eine oder mehrere der Einschränkungen eines Typparameters nicht durch die angegebenen Typargumente erfüllt werden.

Da Typparameter nicht vererbt werden, werden Einschränkungen nie geerbt. Im folgenden Beispiel muss `D` die-Einschränkung für den Typparameter `T` angeben, damit `T` die von der Basisklasse `B<T>`erzwungene Einschränkung erfüllt. Im Gegensatz dazu muss Class `E` keine Einschränkung angeben, da `List<T>` `IEnumerable` für jede `T`implementiert.

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>Typparameter

Ein Typparameter ist ein Bezeichner, der einen Werttyp oder Verweistyp festlegt, an den der Parameter zur Laufzeit gebunden ist.

```antlr
type_parameter
    : identifier
    ;
```

Da ein Typparameter mit vielen verschiedenen tatsächlichen Typargumenten instanziiert werden kann, haben Typparameter etwas andere Vorgänge und Einschränkungen als andere Typen. Dazu gehören:

*  Ein Typparameter kann nicht direkt verwendet werden, um eine Basisklasse ([Basisklasse](classes.md#base-class)) oder eine Schnittstelle ([Variant-Typparameter Listen](interfaces.md#variant-type-parameter-lists)) zu deklarieren.
*  Die Regeln für die Element Suche für Typparameter hängen von den Einschränkungen ab, die ggf. auf den Typparameter angewendet werden. Sie werden in der [Mitglieder Suche](expressions.md#member-lookup)ausführlich erläutert.
*  Die verfügbaren Konvertierungen für einen Typparameter hängen von den Einschränkungen ab, die ggf. auf den Typparameter angewendet werden. Sie werden in [impliziten Konvertierungen mit Typparametern](conversions.md#implicit-conversions-involving-type-parameters) und [expliziten dynamischen Konvertierungen](conversions.md#explicit-dynamic-conversions)ausführlich erläutert.
*  Der Literal`null` kann nicht in einen Typ konvertiert werden, der durch einen Typparameter angegeben wird, mit dem Unterschied, dass der Typparameter ein Verweistyp ist ([implizite Konvertierungen mit Typparametern](conversions.md#implicit-conversions-involving-type-parameters)). Stattdessen kann jedoch ein `default` Ausdruck ([Standardwert Ausdrücke](expressions.md#default-value-expressions)) verwendet werden. Außerdem kann ein Wert mit einem Typ, der durch einen Typparameter angegeben wird, mit `null` mithilfe von `==` und `!=` ([Verweistyp-Gleichheits Operatoren](expressions.md#reference-type-equality-operators)) verglichen werden, es sei denn, der Typparameter weist die Werttyp Einschränkung auf.
*  Ein `new` Ausdruck ([Objekt Erstellungs Ausdrücke](expressions.md#object-creation-expressions)) kann nur mit einem Typparameter verwendet werden, wenn der Typparameter durch eine *constructor_constraint* oder die Werttyp Einschränkung ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) eingeschränkt wird.
*  Ein Typparameter kann nicht an einer beliebigen Stelle innerhalb eines Attributs verwendet werden.
*  Ein Typparameter kann nicht in einem Element Zugriff ([Member Access](expressions.md#member-access)) oder Typname ([Namespace-und Typnamen](basic-concepts.md#namespace-and-type-names)) verwendet werden, um einen statischen Member oder einen schsted Typ zu identifizieren.
*  In unsicherem Code kann ein Typparameter nicht als *unmanaged_type* ([Zeiger Typen](unsafe-code.md#pointer-types)) verwendet werden.

Typparameter sind ein reines Kompilierzeit Konstrukt. Zur Laufzeit wird jeder Typparameter an einen Lauf Zeittyp gebunden, der durch Bereitstellen eines Typarguments an die generische Typdeklaration angegeben wurde. Daher ist der Typ einer Variablen, die mit einem Typparameter deklariert wird, zur Laufzeit ein geschlossener konstruierter Typ ([Open-und Closed-Typen](types.md#open-and-closed-types)). Die Lauf Zeit Ausführung aller Anweisungen und Ausdrücke, die Typparameter betreffen, verwendet den eigentlichen Typ, der als Typargument für diesen Parameter angegeben wurde.

## <a name="expression-tree-types"></a>Ausdrucks Baumstruktur Typen

***Ausdrucks Baum*** Strukturen erlauben, dass Lambda-Ausdrücke als Datenstrukturen anstelle von ausführbarem Code dargestellt werden. Ausdrucks Baumstrukturen sind Werte von ***Ausdrucks Baumstruktur Typen*** der Form `System.Linq.Expressions.Expression<D>`, wobei `D` ein beliebiger Delegattyp ist. Für den Rest dieser Spezifikation verweisen wir auf diese Typen mit der kurzzeile `Expression<D>`.

Wenn eine Konvertierung von einem Lambda-Ausdruck in einen Delegattyp `D`vorhanden ist, ist auch eine Konvertierung für den Ausdrucks bauentyp `Expression<D>`vorhanden. Während die Konvertierung eines Lambda-Ausdrucks in einen Delegattyp einen Delegaten generiert, der auf den ausführbaren Code für den Lambda-Ausdruck verweist, erstellt die Konvertierung in einen Ausdrucks Strukturtyp eine Ausdrucks Baumstruktur-Darstellung des Lambda Ausdrucks.

Ausdrucks Baumstrukturen sind effiziente in-Memory-Daten Darstellungen von Lambda-Ausdrücken und machen die Struktur des Lambda Ausdrucks transparent und explizit.

Wie bei einem Delegattyp `D`werden `Expression<D>` über Parameter-und Rückgabe Typen verfügen, die mit denen von `D`identisch sind.

Im folgenden Beispiel wird ein Lambda-Ausdruck sowohl als ausführbarer Code als auch als Ausdrucks Baumstruktur dargestellt. Da eine Konvertierung in `Func<int,int>`vorhanden ist, ist auch eine Konvertierung zum `Expression<Func<int,int>>`vorhanden:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

Nach diesen Zuweisungen verweist der Delegat `del` auf eine Methode, die `x + 1`zurückgibt, und der Ausdrucks Baum `exp` verweist auf eine Datenstruktur, die den Ausdrucks `x => x + 1`beschreibt.

Die genaue Definition des generischen Typs `Expression<D>` sowie die präzisen Regeln zum Erstellen einer Ausdrucks Baumstruktur, wenn ein Lambda Ausdruck in einen Ausdrucks Strukturtyp konvertiert wird, liegen beide außerhalb des Gültigkeits Bereichs dieser Spezifikation.

Zwei Dinge sind wichtig, um explizit zu machen:

*  Nicht alle Lambda-Ausdrücke können in Ausdrucks Baumstrukturen konvertiert werden. Beispielsweise können Lambda-Ausdrücke mit Anweisungs Text und Lambda-Ausdrücke, die Zuweisungs Ausdrücke enthalten, nicht dargestellt werden. In diesen Fällen ist noch eine Konvertierung vorhanden, schlägt jedoch zur Kompilierzeit fehl. Diese Ausnahmen werden in [anonymen Funktions Konvertierungen](conversions.md#anonymous-function-conversions)ausführlich erläutert.
*   `Expression<D>` bietet eine Instanzmethode `Compile` die einen Delegaten vom Typ `D`erzeugt:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    Das Aufrufen dieses Delegaten bewirkt, dass der durch die Ausdrucks Baumstruktur dargestellte Code ausgeführt wird. Folglich sind die oben aufgeführten Definitionen gleichwertig, und die folgenden zwei Anweisungen haben die gleiche Wirkung:

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    Nach dem Ausführen dieses Codes haben `i1` und `i2` den Wert `2`.

