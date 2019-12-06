---
ms.openlocfilehash: 4d6d28a3127bc701867afe157aa5496377a06f69
ms.sourcegitcommit: 63d276488c9770a565fd787020783ffc1d2af9d6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "74868003"
---
# <a name="conversions"></a>Konvertierungen

Eine ***Konvertierung*** ermöglicht es, einen Ausdruck als einen bestimmten Typ zu behandeln. Eine Konvertierung kann bewirken, dass ein Ausdruck eines bestimmten Typs als einen anderen Typ behandelt wird, oder es kann dazu führen, dass ein Ausdruck ohne einen Typ einen Typ erhält. Konvertierungen können ***implizit*** oder ***explizit***sein. Dadurch wird bestimmt, ob eine explizite Umwandlung erforderlich ist. Beispielsweise ist die Konvertierung von Typ `int` in Typ `long` implizit, sodass Ausdrücke vom Typ `int` implizit als Typ `long`behandelt werden können. Die umgekehrte Konvertierung vom Typ `long` in den Typ `int`ist explizit, sodass eine explizite Umwandlung erforderlich ist.

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

Einige Konvertierungen werden von der Sprache definiert. Programme können auch Ihre eigenen Konvertierungen ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)) definieren.

## <a name="implicit-conversions"></a>Implizite Konvertierungen

Die folgenden Konvertierungen werden als implizite Konvertierungen klassifiziert:

*  Identitäts Konvertierungen
*  Implizite numerische Konvertierungen
*  Implizite Enumerationskonvertierungen
*  Implizite interinterpolierte Zeichen folgen Konvertierungen
*  Implizite Konvertierungen, die NULL zulassen
*  NULL Literale Konvertierungen
*  Implizite Verweis Konvertierungen
*  Boxing-Konvertierungen
*  Implizite dynamische Konvertierungen
*  Implizite Konstante Ausdrucks Konvertierungen
*  Benutzerdefinierte implizite Konvertierungen
*  Anonyme Funktions Konvertierungen
*  Methoden Gruppen Konvertierungen

Implizite Konvertierungen können in einer Vielzahl von Situationen auftreten, einschließlich Funktionsmember-Aufrufe ([Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), Umwandlungs Ausdrücke (Umwandlungs[Ausdrücke](expressions.md#cast-expressions)) und Zuweisungen ([Zuweisungs Operatoren](expressions.md#assignment-operators)).

Die vordefinierten impliziten Konvertierungen sind immer erfolgreich und bewirken nie, dass Ausnahmen ausgelöst werden. Ordnungsgemäß entworfene benutzerdefinierte implizite Konvertierungen sollten auch diese Merkmale aufweisen.

Zum Zweck der Konvertierung werden die Typen `object` und `dynamic` als gleichwertig betrachtet.

Dynamische Konvertierungen ([implizite dynamische Konvertierungen](conversions.md#implicit-dynamic-conversions) und [explizite dynamische Konvertierungen](conversions.md#explicit-dynamic-conversions)) gelten jedoch nur für Ausdrücke vom Typ `dynamic` ([der dynamische Typ](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>Identitäts Konvertierung

Eine Identitäts Konvertierung konvertiert von einem beliebigen Typ in denselben Typ. Diese Konvertierung besteht darin, dass eine Entität, die bereits über einen erforderlichen Typ verfügt, in diesen Typ konvertiert werden kann.

*  Da `object` und `dynamic` als gleichwertig betrachtet werden, gibt es eine Identitäts Konvertierung zwischen `object` und `dynamic`sowie zwischen konstruierten Typen, die beim Ersetzen aller Vorkommen von `dynamic` durch `object`identisch sind.

### <a name="implicit-numeric-conversions"></a>Implizite numerische Konvertierungen

Die impliziten numerischen Konvertierungen lauten:

*  Von `sbyte` bis `short`, `int`, `long`, `float`, `double`oder `decimal`.
*  Von `byte` bis `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`oder `decimal`.
*  Von `short` bis `int`, `long`, `float`, `double`oder `decimal`.
*  Von `ushort` bis `int`, `uint`, `long`, `ulong`, `float`, `double`oder `decimal`.
*  Von `int` bis `long`, `float`, `double`oder `decimal`.
*  Von `uint` bis `long`, `ulong`, `float`, `double`oder `decimal`.
*  Von `long` bis `float`, `double`oder `decimal`.
*  Von `ulong` bis `float`, `double`oder `decimal`.
*  Von `char` bis `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`oder `decimal`.
*  Von `float` in `double`.

Konvertierungen von `int`, `uint`, `long`oder `ulong` in `float` und aus `long` oder `ulong` zu `double` können zu einem Genauigkeits Verlust führen, aber nie zu einem Verlust der Größe führen. Die anderen impliziten numerischen Konvertierungen verlieren nie Informationen.

Es gibt keine impliziten Konvertierungen in den `char` Typ, sodass Werte der anderen ganzzahligen Typen nicht automatisch in den `char` Typ konvertiert werden.

### <a name="implicit-enumeration-conversions"></a>Implizite Enumerationskonvertierungen

Eine implizite Enumerationskonvertierung ermöglicht das Konvertieren des *decimal_integer_literal* `0` in beliebige *enum_type* und in beliebige *nullable_type* , deren zugrunde liegender Typ ein *enum_type*ist. Im letzteren Fall wird die Konvertierung ausgewertet, indem Sie in die zugrunde liegende *enum_type* konvertiert und das Ergebnis umwickelt ([Nullable-Typen](types.md#nullable-types)).

### <a name="implicit-interpolated-string-conversions"></a>Implizite interinterpolierte Zeichen folgen Konvertierungen

Eine implizite interpolierte Zeichen folgen Konvertierung ermöglicht das Konvertieren eines *interpolated_string_expression* ([interpoliert](expressions.md#interpolated-strings)Zeichen folgen) in `System.IFormattable` oder `System.FormattableString` (das `System.IFormattable`implementiert).

Wenn diese Konvertierung angewendet wird, wird ein Zeichen folgen Wert nicht aus der interinterpolierten Zeichenfolge zusammengesetzt. Stattdessen wird eine Instanz von `System.FormattableString` erstellt, wie in [interpoliert](expressions.md#interpolated-strings)-Zeichen folgen beschrieben.

### <a name="implicit-nullable-conversions"></a>Implizite Konvertierungen, die NULL zulassen

Vordefinierte implizite Konvertierungen, die nicht auf NULL festleg Bare Werttypen angewendet werden, können auch mit null-fähigen Formularen dieser Typen verwendet werden. Für jede der vordefinierten impliziten Identitäts-und numerischen Konvertierungen, die von einem Werttyp, der keine NULL-Werte zulässt, `S` in einen Werttyp, der nicht auf NULL festgelegt werden kann `T`, sind die folgenden impliziten Konvertierungen vorhanden:

*  Eine implizite Konvertierung von `S?` in `T?`.
*  Eine implizite Konvertierung von `S` in `T?`.

Die Auswertung einer impliziten Konvertierung auf NULL-Werte, die auf einer zugrunde liegenden Konvertierung von `S` in `T` basiert, verläuft wie folgt:

*  Wenn die Konvertierung, die NULL-Werte zulässt, von `S?` in `T?`erfolgt:
    * Wenn der Quellwert NULL ist (`HasValue`-Eigenschaft ist false), ist das Ergebnis der NULL-Wert des Typs `T?`.
    * Andernfalls wird die Konvertierung als zum Entpacken von `S?` in `S`ausgewertet, gefolgt von der zugrunde liegenden Konvertierung von `S` in `T`, gefolgt von einem Wrapping ([Nullable-Typen](types.md#nullable-types)) von `T` zu `T?`.

*  Wenn die Konvertierung, die NULL-Werte zulässt, von `S` auf `T?`erfolgt, wird die Konvertierung als die zugrunde liegende Konvertierung von `S` in `T` gefolgt von einem Wrapping von `T` zu `T?`ausgewertet.

### <a name="null-literal-conversions"></a>NULL Literale Konvertierungen

Eine implizite Konvertierung ist vom `null` Literaltyp zu einem beliebigen Typ, der NULL-Werte zulässt. Diese Konvertierung erzeugt den NULL-Wert ([Werte zulässt-Typen](types.md#nullable-types)) des angegebenen Typs, der NULL-Werte zulässt.

### <a name="implicit-reference-conversions"></a>Implizite Verweis Konvertierungen

Die impliziten Verweis Konvertierungen sind:

*  Von allen *reference_type* bis `object` und `dynamic`.
*  Von allen *class_type* `S` zu beliebigen *class_type* `T`wird die angegebene `S` von `T`abgeleitet.
*  Von allen *class_type* `S` bis hin zu beliebigen *INTERFACE_TYPE* `T`implementiert `S` `T`implementiert.
*  Von allen *INTERFACE_TYPE* `S` zu beliebigen *INTERFACE_TYPE* `T`wird die angegebene `S` von `T`abgeleitet.
*  Aus einer *array_type* `S` mit einem Elementtyp `SE` zu einer *array_type* `T` mit einem Elementtyp `TE`, wenn alle folgenden Punkte zutreffen:
    * `S` und `T` unterscheiden sich nur in Elementtyp. Anders ausgedrückt: `S` und `T` haben die gleiche Anzahl von Dimensionen.
    * Sowohl `SE` als auch `TE` sind *reference_type*s.
    * Eine implizite Verweis Konvertierung ist von `SE` in `TE`vorhanden.
*  Von allen *array_type* bis `System.Array` und den Schnittstellen, die es implementiert.
*  Von einem eindimensionalen Arraytyp `S[]` zu `System.Collections.Generic.IList<T>` und dessen Basis Schnittstellen, vorausgesetzt, dass es eine implizite Identität oder Verweis Konvertierung von `S` in `T`gibt.
*  Von allen *delegate_type* bis `System.Delegate` und den Schnittstellen, die es implementiert.
*  Von der NULL-Literale zu beliebigen *reference_type*.
*  Von allen *reference_type* zu einem *reference_type* `T`, wenn es eine implizite Identität oder Verweis Konvertierung in einen *reference_type* hat `T0` und `T0` über eine Identitäts Konvertierung in `T`verfügt.
*  Von einer beliebigen *reference_type* zu einer Schnittstelle oder einem Delegattyp `T`, wenn Sie eine implizite Identität oder Verweis Konvertierung in eine Schnittstelle oder einen Delegattyp aufweist `T0` und `T0` die Varianz konvertierbar ([Varianz Konvertierung](interfaces.md#variance-conversion)) zum `T`ist.
*  Implizite Konvertierungen mit Typparametern, die als Verweis Typen bekannt sind. Weitere Informationen zu impliziten Konvertierungen, die Typparameter einschließen, finden Sie unter [implizite Konvertierungen mit Typparametern](conversions.md#implicit-conversions-involving-type-parameters) .

Die impliziten Verweis Konvertierungen sind Konvertierungen zwischen *reference_type*en, die sich als immer erfolgreich erweisen können und daher keine Überprüfungen zur Laufzeit erfordern.

Verweis Konvertierungen, implizit oder explizit, ändern niemals die referenzielle Identität des Objekts, das konvertiert wird. Anders ausgedrückt: während eine Verweis Konvertierung den Typ des Verweises ändern kann, ändert Sie niemals den Typ oder Wert des Objekts, auf das verwiesen wird.

### <a name="boxing-conversions"></a>Boxing-Konvertierungen

Eine Boxing-Konvertierung ermöglicht das implizite Konvertieren eines *value_type* in einen Verweistyp. Eine Boxing-Konvertierung ist von allen *non_nullable_value_type* zu `object` und `dynamic`, `System.ValueType` und in alle *INTERFACE_TYPE* , die vom *non_nullable_value_type*implementiert werden, vorhanden. Außerdem kann eine *enum_type* in den Typ `System.Enum`konvertiert werden.

Eine Boxing-Konvertierung ist aus einer *nullable_type* in einen Verweistyp vorhanden, und zwar nur dann, wenn eine Boxing-Konvertierung von der zugrunde liegenden *non_nullable_value_type* in den Verweistyp vorhanden ist.

Ein Werttyp verfügt über eine Boxing-Konvertierung in einen Schnittstellentyp `I` wenn er eine Boxing-Konvertierung in einen Schnittstellentyp `I0` und `I0` über eine Identitäts Konvertierung in `I`verfügt.

Ein Werttyp verfügt über eine Boxing-Konvertierung in einen Schnittstellentyp `I` wenn er eine Boxing-Konvertierung in eine Schnittstelle oder einen Delegattyp aufweist `I0` und `I0` Varianz konvertierbar ([Varianz Konvertierung](interfaces.md#variance-conversion)) zum `I`ist.

Das Boxing eines Werts einer *non_nullable_value_type* besteht aus der Zuordnung einer Objektinstanz und dem Kopieren des *value_type* Werts in diese Instanz. Eine Struktur kann in den Typ `System.ValueType`gekapselt werden, da dies eine Basisklasse für alle Strukturen ([Vererbung](structs.md#inheritance)) ist.

Das Boxing eines Werts eines *nullable_type* verläuft wie folgt:

*  Wenn der Quellwert NULL ist (`HasValue`-Eigenschaft ist false), ist das Ergebnis ein NULL-Verweis des Zieltyps.
*  Andernfalls ist das Ergebnis ein Verweis auf eine `T`, die durch das Entpacken und Boxing des Quell Werts erzeugt wird.

Boxing-Konvertierungen werden weiter unten in [Boxing-Konvertierungen](types.md#boxing-conversions)beschrieben.

### <a name="implicit-dynamic-conversions"></a>Implizite dynamische Konvertierungen

Eine implizite dynamische Konvertierung ist von einem Ausdruck vom Typ `dynamic` in einen beliebigen Typ `T`vorhanden. Die Konvertierung ist dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)). Dies bedeutet, dass eine implizite Konvertierung zur Laufzeit vom Lauf Zeittyp des Ausdrucks zum `T`durchgeführt wird. Wenn keine Konvertierung gefunden wird, wird eine Lauf Zeit Ausnahme ausgelöst.

Beachten Sie, dass diese implizite Konvertierung scheinbar gegen den Rat am Anfang von [impliziten Konvertierungen](conversions.md#implicit-conversions) verstößt, dass eine implizite Konvertierung nie eine Ausnahme auslösen sollte. Dabei handelt es sich jedoch nicht um die Konvertierung, sondern um die *Suche nach* der Konvertierung, die die Ausnahme verursacht. Das Risiko von Lauf Zeit Ausnahmen ist die Verwendung der dynamischen Bindung. Wenn die dynamische Bindung der Konvertierung nicht erwünscht ist, kann der Ausdruck zuerst in `object`und dann in den gewünschten Typ konvertiert werden.

Das folgende Beispiel veranschaulicht implizite dynamische Konvertierungen:

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

Die Zuweisungen zu `s2` und `i` beide verwenden implizite dynamische Konvertierungen, bei denen die Bindung der Vorgänge bis zur Laufzeit angehalten wird. Zur Laufzeit werden implizite Konvertierungen vom Lauf Zeittyp `d` -- `string`-bis zum Zieltyp durchgeführt. Es wurde eine Konvertierung gefunden, um `string`, aber nicht `int`.

### <a name="implicit-constant-expression-conversions"></a>Implizite Konstante Ausdrucks Konvertierungen

Eine implizite Konstante Ausdrucks Konvertierung ermöglicht die folgenden Konvertierungen:

*  Eine *constant_expression* ([Konstante Ausdrücke](expressions.md#constant-expressions)) vom Typ `int` kann in den Typ `sbyte`, `byte`, `short`, `ushort`, `uint`oder `ulong`konvertiert werden, sofern der Wert des *constant_expression* innerhalb des Bereichs des Zieltyps liegt.
*  Eine *constant_expression* vom Typ `long` kann in den Typ `ulong`konvertiert werden, sofern der Wert des *constant_expression* nicht negativ ist.

### <a name="implicit-conversions-involving-type-parameters"></a>Implizite Konvertierungen mit Typparametern

Die folgenden impliziten Konvertierungen sind für einen angegebenen Typparameter `T`vorhanden:

*  Von `T` bis zur effektiven Basisklasse `C`, von `T` zu einer beliebigen Basisklasse `C`und von `T` bis zu einer beliebigen Schnittstelle, die von `C`implementiert wird. Wenn `T` ein Werttyp ist, wird die Konvertierung zur Laufzeit als Boxing-Konvertierung ausgeführt. Andernfalls wird die Konvertierung als implizite Verweis Konvertierung oder Identitäts Konvertierung ausgeführt.
*  Von `T` bis zu einem Schnittstellentyp `I` in den effektiven Schnittstellen Satz `T`und von `T` auf eine beliebige Basisschnittstelle `I`. Wenn `T` ein Werttyp ist, wird die Konvertierung zur Laufzeit als Boxing-Konvertierung ausgeführt. Andernfalls wird die Konvertierung als implizite Verweis Konvertierung oder Identitäts Konvertierung ausgeführt.
*  Von `T` bis zu einem Typparameter `U`ist `T` von `U` abhängig ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)). Wenn `U` ein Werttyp ist, sind `T` und `U` bei der Laufzeit notwendigerweise denselben Typ, und es wird keine Konvertierung durchgeführt. Andernfalls, wenn `T` ein Werttyp ist, wird die Konvertierung als Boxing-Konvertierung ausgeführt. Andernfalls wird die Konvertierung als implizite Verweis Konvertierung oder Identitäts Konvertierung ausgeführt.
*  Von der NULL-Literale bis zu `T`, wird `T` als Verweistyp bezeichnet.
*  Von `T` bis zu einem Verweistyp `I`, wenn eine implizite Konvertierung in einen Verweistyp `S0` und `S0` über eine Identitäts Konvertierung in `S`verfügt. Zur Laufzeit wird die Konvertierung auf die gleiche Weise ausgeführt wie die Konvertierung in `S0`.
*  Von `T` bis zu einem Schnittstellentyp `I`, wenn eine implizite Konvertierung in eine Schnittstelle oder einen Delegattyp erfolgt `I0` und `I0` in `I` Varianz konvertierbar ist ([Varianz Konvertierung](interfaces.md#variance-conversion)). Wenn `T` ein Werttyp ist, wird die Konvertierung zur Laufzeit als Boxing-Konvertierung ausgeführt. Andernfalls wird die Konvertierung als implizite Verweis Konvertierung oder Identitäts Konvertierung ausgeführt.

Wenn `T` bekannt ist, dass es sich um einen Verweistyp ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) handelt, werden die obigen Konvertierungen als implizite Verweis Konvertierungen ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions)) klassifiziert. Wenn `T` nicht bekannt ist, dass es sich um einen Verweistyp handelt, werden die obigen Konvertierungen als boxkonvertierungen ([Boxing-Konvertierungen](conversions.md#boxing-conversions)) klassifiziert.

### <a name="user-defined-implicit-conversions"></a>Benutzerdefinierte implizite Konvertierungen

Eine benutzerdefinierte implizite Konvertierung besteht aus einer optionalen standardmäßigen impliziten Konvertierung, gefolgt von der Ausführung eines benutzerdefinierten impliziten Konvertierungs Operators, gefolgt von einer anderen optionalen standardmäßigen impliziten Konvertierung. Die genauen Regeln für das Auswerten benutzerdefinierter impliziter Konvertierungen werden bei der [Verarbeitung von benutzerdefinierten impliziten Konvertierungen](conversions.md#processing-of-user-defined-implicit-conversions)beschrieben.

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>Anonyme Funktions Konvertierungen und Methoden Gruppen Konvertierungen

Anonyme Funktionen und Methoden Gruppen weisen keine Typen in und von sich selbst auf, können aber implizit in Delegattypen oder Ausdrucks Baumstruktur konvertiert werden. Anonyme Funktions Konvertierungen werden ausführlicher in [Anonyme Funktions Konvertierungen](conversions.md#anonymous-function-conversions) und Methoden Gruppen Konvertierungen in [Methoden Gruppen Konvertierungen](conversions.md#method-group-conversions)beschrieben.

## <a name="explicit-conversions"></a>Explizite Konvertierungen

Die folgenden Konvertierungen werden als explizite Konvertierungen klassifiziert:

*  Alle impliziten Konvertierungen.
*  Explizite numerische Konvertierungen.
*  Explizite Enumerationskonvertierungen.
*  Explizite Konvertierungen, die NULL zulassen.
*  Explizite Verweis Konvertierungen.
*  Explizite Schnittstellen Konvertierungen.
*  Unboxing-Konvertierungen.
*  Explizite dynamische Konvertierungen
*  Benutzerdefinierte explizite Konvertierungen.

Explizite Konvertierungen können in Umwandlungs Ausdrücken (Umwandlungs[Ausdrücke](expressions.md#cast-expressions)) auftreten.

Der Satz expliziter Konvertierungen umfasst alle impliziten Konvertierungen. Dies bedeutet, dass redundante Umwandlungs Ausdrücke zulässig sind.

Die expliziten Konvertierungen, bei denen es sich nicht um implizite Konvertierungen handelt, sind Konvertierungen, die sich nicht immer als erfolgreich erweisen können, Konvertierungen, die bekanntermaßen Informationen verlieren, und Konvertierungen zwischen Domänen von Typen ausreichend verschieden sind, Angabe.

### <a name="explicit-numeric-conversions"></a>Explizite numerische Konvertierungen

Die expliziten numerischen Konvertierungen sind Konvertierungen von einer *numeric_type* in eine andere *numeric_type* , für die eine implizite numerische Konvertierung ([implizite numerische Konvertierungen](conversions.md#implicit-numeric-conversions)) nicht bereits vorhanden ist:

*  Von `sbyte` bis `byte`, `ushort`, `uint`, `ulong`oder `char`.
*  Von `byte` bis `sbyte` und `char`.
*  Von `short` bis `sbyte`, `byte`, `ushort`, `uint`, `ulong`oder `char`.
*  Von `ushort` bis `sbyte`, `byte`, `short`oder `char`.
*  Von `int` bis `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`oder `char`.
*  Von `uint` bis `sbyte`, `byte`, `short`, `ushort`, `int`oder `char`.
*  Von `long` bis `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`oder `char`.
*  Von `ulong` bis `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`oder `char`.
*  Von `char` bis `sbyte`, `byte`oder `short`.
*  Von `float` bis `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`oder `decimal`.
*  Von `double` bis `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`oder `decimal`.
*  Von `decimal` bis `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`oder `double`.

Da die expliziten Konvertierungen alle impliziten und expliziten numerischen Konvertierungen einschließen, ist es immer möglich, mithilfe eines Umwandlungs Ausdrucks (Umwandlungs[Ausdrücke](expressions.md#cast-expressions)) von beliebigen *numeric_type* in andere *numeric_type* zu konvertieren.

Die expliziten numerischen Konvertierungen verlieren möglicherweise Informationen oder bewirken möglicherweise, dass Ausnahmen ausgelöst werden. Eine explizite numerische Konvertierung wird wie folgt verarbeitet:

*  Für eine Konvertierung eines ganzzahligen Typs in einen anderen ganzzahligen Typ hängt die Verarbeitung vom Kontext der Überlauf Überprüfung (die aktivierten und deaktivierten[Operatoren](expressions.md#the-checked-and-unchecked-operators)) ab, in der die Konvertierung stattfindet:
    * In einem `checked` Kontext wird die Konvertierung erfolgreich ausgeführt, wenn der Wert des Quell Operanden innerhalb des Bereichs des Zieltyps liegt, aber eine `System.OverflowException` auslöst, wenn der Wert des Quell Operanden außerhalb des Bereichs des Zieltyps liegt.
    * In einem `unchecked` Kontext ist die Konvertierung immer erfolgreich und wird wie folgt fortgesetzt.
        * Wenn der Quelltyp größer als der Zieltyp ist, wird der Quellwert abgeschnitten, indem die wichtigsten „zusätzlichen“ Teile verworfen werden. Das Ergebnis wird dann als Wert des Zieltyps behandelt.
        * Wenn der Quelltyp kleiner als der Zieltyp ist, wird der Quellwert entweder mit Vorzeichen oder Nullen (0) erweitert, sodass er die gleiche Größe wie der Zieltyp aufweist. Die Vorzeichenerweiterung wird verwendet, wenn der Quelltyp mit einem Vorzeichen versehen ist. Die Erweiterung mit Nullen (0) wird verwendet, wenn der Quelltyp mit keinem Vorzeichen versehen ist. Das Ergebnis wird dann als Wert des Zieltyps behandelt.
        * Wenn der Quelltyp die gleiche Größe wie der Zieltyp aufweist, wird der Quellwert als Wert vom Zieltyp behandelt.
*  Bei einer Konvertierung von `decimal` in einen ganzzahligen Typ wird der Quellwert auf 0 (null) bis zum nächstgelegenen ganzzahligen Wert gerundet, und dieser ganzzahlige Wert wird das Ergebnis der Konvertierung. Wenn sich der resultierende ganzzahlige Wert außerhalb des Bereichs des Zieltyps befindet, wird eine `System.OverflowException` ausgelöst.
*  Bei einer Konvertierung von `float` oder `double` in einen ganzzahligen Typ hängt die Verarbeitung vom Kontext der Überlauf Überprüfung (die aktivierten und deaktivierten[Operatoren](expressions.md#the-checked-and-unchecked-operators)) ab, in der die Konvertierung stattfindet:
    * In einem `checked` Kontext verläuft die Konvertierung wie folgt:
        * Wenn der Wert des Operanden NaN oder Infinite ist, wird ein `System.OverflowException` ausgelöst.
        * Andernfalls wird der Quell Operand auf den nächsten ganzzahligen Wert in Richtung 0 (null) gerundet. Wenn sich dieser ganzzahlige Wert innerhalb des Bereichs des Zieltyps befindet, ist dieser Wert das Ergebnis der Konvertierung.
        * Andernfalls wird eine `System.OverflowException` ausgelöst.
    * In einem `unchecked` Kontext ist die Konvertierung immer erfolgreich und wird wie folgt fortgesetzt.
        * Wenn der Wert des Operanden NaN oder Infinite ist, ist das Ergebnis der Konvertierung ein nicht spezifizierter Wert des Zieltyps.
        * Andernfalls wird der Quell Operand auf den nächsten ganzzahligen Wert in Richtung 0 (null) gerundet. Wenn sich dieser ganzzahlige Wert innerhalb des Bereichs des Zieltyps befindet, ist dieser Wert das Ergebnis der Konvertierung.
        * Andernfalls ist das Ergebnis der Konvertierung ein nicht spezifizierter Wert des Zieltyps.
*  Bei einer Konvertierung von `double` in `float`wird der `double` Wert auf den nächsten `float` Wert gerundet. Wenn der `double` Wert zu klein ist, um als `float`darzustellen, wird das Ergebnis positiv 0 (null) oder negativ 0 (null). Wenn der `double` Wert zu groß ist, um als `float`darzustellen, wird das Ergebnis positiv unendlich oder minus unendlich. Wenn der `double` Wert NaN ist, ist das Ergebnis ebenfalls NaN.
*  Bei einer Konvertierung von `float` oder `double` zu `decimal`wird der Quellwert in `decimal` Darstellung konvertiert und bei Bedarf auf die nächste Zahl nach dem 28. Dezimaltrennzeichen gerundet ([der Decimal-Typ](types.md#the-decimal-type)). Wenn der Quellwert zu klein ist, um als `decimal`darzustellen, wird das Ergebnis 0 (null). Wenn der Quellwert Nan, Infinity oder zu groß ist, um als `decimal`darzustellen, wird eine `System.OverflowException` ausgelöst.
*  Bei einer Konvertierung von `decimal` in `float` oder `double`wird der `decimal` Wert auf den nächstgelegenen `double` oder `float` Wert gerundet. Während diese Konvertierung die Genauigkeit verlieren kann, bewirkt dies nie, dass eine Ausnahme ausgelöst wird.

### <a name="explicit-enumeration-conversions"></a>Explizite Enumerationskonvertierungen

Die expliziten Enumerationskonvertierungen sind:

*  Von `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`oder `decimal` zu beliebigen *enum_type*.
*  Von allen *enum_type* bis `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`oder `decimal`.
*  Von allen *enum_type* zu anderen *enum_type*.

Eine explizite Enumerationskonvertierung zwischen zwei Typen wird verarbeitet, indem alle Beteiligten *enum_type* als zugrunde liegender Typ dieser *enum_type*behandelt und dann eine implizite oder explizite numerische Konvertierung zwischen den resultierenden Typen durchgeführt wird. Wenn z. b. eine *enum_type* `E` mit und zugrunde liegender `int`ist, wird eine Konvertierung von `E` in `byte` als explizite numerische Konvertierung ([explizite numerische Konvertierungen](conversions.md#explicit-numeric-conversions)) von `int` in `byte`verarbeitet, und eine Konvertierung von `byte` in `E` wird als implizite numerische Konvertierung ([implizite numerische Konvertierungen](conversions.md#implicit-numeric-conversions)) von `byte` in `int`verarbeitet.

### <a name="explicit-nullable-conversions"></a>Explizite Konvertierungen, die NULL zulassen

***Explizite Konvertierungen*** , die NULL-Werte zulassen, ermöglichen vordefinierte explizite Konvertierungen, die für nicht auf NULL festleg Bare Werttypen verwendet werden, auch mit null-fähigen Formen dieser Typen Für jede der vordefinierten expliziten Konvertierungen, die von einem Werttyp, der keine NULL-Werte zulässt, `S` in einen Werttyp, der keine NULL-Werte zulässt `T` ([Identitäts Konvertierung](conversions.md#identity-conversion), [implizite numerische Konvertierungen](conversions.md#implicit-numeric-conversions), [Implizite Enumerationskonvertierungen](conversions.md#implicit-enumeration-conversions), [explizite numerische Konvertierungen](conversions.md#explicit-numeric-conversions)und [Explizite Enumerationskonvertierungen](conversions.md#explicit-enumeration-conversions)), sind die folgenden Konvertierungen

*  Eine explizite Konvertierung von `S?` in `T?`.
*  Eine explizite Konvertierung von `S` in `T?`.
*  Eine explizite Konvertierung von `S?` in `T`.

Die Auswertung einer Konvertierung, die NULL-Werte zulässt, basierend auf einer zugrunde liegenden Konvertierung von `S` in `T` verläuft wie folgt:

*  Wenn die Konvertierung, die NULL-Werte zulässt, von `S?` in `T?`erfolgt:
    * Wenn der Quellwert NULL ist (`HasValue`-Eigenschaft ist false), ist das Ergebnis der NULL-Wert des Typs `T?`.
    * Andernfalls wird die Konvertierung als ein zum Entpacken von `S?` in `S`ausgewertet, gefolgt von der zugrunde liegenden Konvertierung von `S` in `T`, gefolgt von einer Konvertierung von `T` zu `T?`.
*  Wenn die Konvertierung, die NULL-Werte zulässt, von `S` auf `T?`erfolgt, wird die Konvertierung als die zugrunde liegende Konvertierung von `S` in `T` gefolgt von einem Wrapping von `T` zu `T?`ausgewertet.
*  Wenn die Konvertierung, die NULL-Werte zulässt, von `S?` in `T`erfolgt, wird die Konvertierung als ein zum Entpacken von `S?` in `S` gefolgt von der zugrunde liegenden Konvertierung von `S` in `T`ausgewertet.

Beachten Sie, dass beim Versuch, einen Werte zulässt-Wert zu entpacken, eine Ausnahme ausgelöst wird, wenn der Wert `null`ist.

### <a name="explicit-reference-conversions"></a>Explizite Verweis Konvertierungen

Die expliziten Verweis Konvertierungen lauten:

*  Von `object` und `dynamic` bis hin zu anderen *reference_type*.
*  Von allen *class_type* `S` bis hin zu beliebigen *class_type* `T`ist die angegebene `S` eine Basisklasse von `T`.
*  Von allen *class_type* `S` bis hin zu beliebigen *INTERFACE_TYPE* `T`ist die angegebene `S` nicht versiegelt und bereitgestellt `S` implementiert `T`nicht.
*  Von allen *INTERFACE_TYPE* `S` bis hin zu beliebigen *class_type* `T`ist die angegebene `T` nicht versiegelt oder bereitgestellt `T` implementiert `S`.
*  Von allen *INTERFACE_TYPE* `S` zu beliebigen *INTERFACE_TYPE* `T`wird die angegebene `S` nicht von `T`abgeleitet.
*  Aus einer *array_type* `S` mit einem Elementtyp `SE` zu einer *array_type* `T` mit einem Elementtyp `TE`, wenn alle folgenden Punkte zutreffen:
    * `S` und `T` unterscheiden sich nur in Elementtyp. Anders ausgedrückt: `S` und `T` haben die gleiche Anzahl von Dimensionen.
    * Sowohl `SE` als auch `TE` sind *reference_type*s.
    * Eine explizite Verweis Konvertierung ist von `SE` in `TE`vorhanden.
*  Von `System.Array` und den Schnittstellen, die es für beliebige *array_type*implementiert.
*  Von einem eindimensionalen Arraytyp `S[]` zu `System.Collections.Generic.IList<T>` und den zugehörigen Basis Schnittstellen, vorausgesetzt, dass es eine explizite Verweis Konvertierung von `S` in `T`gibt.
*  Von `System.Collections.Generic.IList<S>` und den zugehörigen Basis Schnittstellen bis zu einem eindimensionalen Arraytyp `T[]`, vorausgesetzt, dass es eine explizite Identitäts-oder Verweis Konvertierung von `S` in `T`gibt.
*  Von `System.Delegate` und den Schnittstellen, die es für beliebige *delegate_type*implementiert.
*  Von einem Referenztyp zu einem Verweistyp `T`, wenn er eine explizite Verweis Konvertierung in einen Verweistyp aufweist `T0` und `T0` über eine Identitäts Konvertierung `T`verfügt.
*  Von einem Referenztyp zu einer Schnittstelle oder einem Delegattyp `T`, wenn er eine explizite Verweis Konvertierung in eine Schnittstelle oder einen Delegattyp aufweist `T0` und entweder `T0` Varianz konvertierbar in `T` oder `T` von Varianz konvertierbar in `T0` ([Varianz Konvertierung](interfaces.md#variance-conversion)) ist.
*  Von `D<S1...Sn>` bis `D<T1...Tn>`, bei dem `D<X1...Xn>` ein generischer Delegattyp ist, ist `D<S1...Sn>` nicht mit `D<T1...Tn>`kompatibel, und für jeden Typparameter `Xi` `D` Folgendes:
    * Wenn `Xi` invariant ist, ist `Si` identisch mit `Ti`.
    * Wenn `Xi` kovariant ist, gibt es eine implizite oder explizite Identitäts-oder Verweis Konvertierung von `Si` in `Ti`.
    * Wenn `Xi` kontra Variant ist, sind `Si` und `Ti` entweder identische oder beide Verweis Typen.
*  Explizite Konvertierungen mit Typparametern, die als Verweis Typen bekannt sind. Weitere Informationen zu expliziten Konvertierungen, die Typparameter einschließen, finden Sie unter [explizite Konvertierungen mit Typparametern](conversions.md#explicit-conversions-involving-type-parameters).

Die expliziten Verweis Konvertierungen sind Konvertierungen zwischen Verweis Typen, für die Laufzeitüberprüfungen erforderlich sind, um sicherzustellen, dass Sie korrekt sind.

Damit eine explizite Verweis Konvertierung zur Laufzeit erfolgreich ist, muss der Wert des Quell Operanden `null`werden, oder der tatsächliche Typ des Objekts, auf das vom Quell Operanden verwiesen wird, muss ein Typ sein, der durch eine implizite Verweis Konvertierung ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions)[) oder](conversions.md#boxing-conversions)Boxing-Konvertierung in den Zieltyp konvertiert werden kann. Wenn eine explizite Verweis Konvertierung fehlschlägt, wird eine `System.InvalidCastException` ausgelöst.

Verweis Konvertierungen, implizit oder explizit, ändern niemals die referenzielle Identität des Objekts, das konvertiert wird. Anders ausgedrückt: während eine Verweis Konvertierung den Typ des Verweises ändern kann, ändert Sie niemals den Typ oder Wert des Objekts, auf das verwiesen wird.

### <a name="unboxing-conversions"></a>Unboxing-Konvertierungen

Eine Unboxing-Konvertierung ermöglicht das explizite Konvertieren eines Verweis Typs in einen *value_type*. Eine Unboxing-Konvertierung ist von den Typen `object`, `dynamic` und `System.ValueType` zu beliebigen *non_nullable_value_type*und von allen *INTERFACE_TYPE* zu beliebigen *non_nullable_value_type* , die den *INTERFACE_TYPE*implementieren, vorhanden. Darüber hinaus können Sie `System.Enum` in beliebige *enum_type*entpackt werden.

Eine Unboxing-Konvertierung ist von einem Referenztyp zu einem *nullable_type* vorhanden, wenn eine Unboxing-Konvertierung vom Verweistyp in den zugrunde liegenden *non_nullable_value_type* der *nullable_type*vorhanden ist.

Ein Werttyp `S` hat eine Unboxing-Konvertierung von einem Schnittstellentyp `I`, wenn er eine Unboxing-Konvertierung von einem Schnittstellentyp hat `I0` und `I0` über eine Identitäts Konvertierung in `I`verfügt.

Ein Werttyp `S` hat eine Unboxing-Konvertierung von einem Schnittstellentyp `I`, wenn er eine Unboxing-Konvertierung von einer Schnittstelle oder einem Delegattyp aufweist `I0` und entweder `I0` von Varianz konvertierbar in `I` oder `I` ist Varianz konvertierbar in `I0` ([Varianz Konvertierung](interfaces.md#variance-conversion)).

Ein Unboxing-Vorgang besteht darin, zuerst zu überprüfen, ob die Objektinstanz ein geachtelter Wert der angegebenen *value_type*ist, und dann den Wert aus der-Instanz zu kopieren. Beim Unboxing eines NULL-Verweises auf einen *nullable_type* wird der NULL-Wert des *nullable_type*erzeugt. Eine Struktur kann vom Typ `System.ValueType`entpackt werden, da dies eine Basisklasse für alle Strukturen ([Vererbung](structs.md#inheritance)) ist.

Unboxing-Konvertierungen werden weiter unten in [Unboxing-Konvertierungen](types.md#unboxing-conversions)beschrieben.

### <a name="explicit-dynamic-conversions"></a>Explizite dynamische Konvertierungen

Eine explizite dynamische Konvertierung ist von einem Ausdruck vom Typ `dynamic` in einen beliebigen Typ `T`vorhanden. Die Konvertierung ist dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)), was bedeutet, dass zur Laufzeit eine explizite Konvertierung vom Lauf Zeittyp des Ausdrucks zum `T`durchgeführt wird. Wenn keine Konvertierung gefunden wird, wird eine Lauf Zeit Ausnahme ausgelöst.

Wenn die dynamische Bindung der Konvertierung nicht erwünscht ist, kann der Ausdruck zuerst in `object`und dann in den gewünschten Typ konvertiert werden.

Nehmen Sie an, dass die folgende Klasse definiert ist:
```csharp
class C
{
    int i;

    public C(int i) { this.i = i; }

    public static explicit operator C(string s) 
    {
        return new C(int.Parse(s));
    }
}
```

Das folgende Beispiel veranschaulicht explizite dynamische Konvertierungen:
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

Die beste Konvertierung von `o` in `C` wird zur Kompilierzeit als explizite Verweis Konvertierung gefunden. Dies schlägt zur Laufzeit fehl, da `"1"` nicht tatsächlich eine `C`ist. Die Konvertierung von `d` in `C` wird jedoch als explizite dynamische Konvertierung zur Laufzeit angehalten, bei der eine benutzerdefinierte Konvertierung vom Lauf Zeittyp `d` -- `string` `C` gefunden wird und erfolgreich ist.

### <a name="explicit-conversions-involving-type-parameters"></a>Explizite Konvertierungen mit Typparametern

Die folgenden expliziten Konvertierungen sind für einen angegebenen Typparameter `T`vorhanden:

*  Von der effektiven Basisklasse `C` `T` bis `T` und von jeder Basisklasse `C` bis `T`. Wenn `T` ein Werttyp ist, wird die Konvertierung zur Laufzeit als Unboxing-Konvertierung ausgeführt. Andernfalls wird die Konvertierung als explizite Verweis Konvertierung oder Identitäts Konvertierung ausgeführt.
*  Von einem beliebigen Schnittstellentyp zu `T`. Wenn `T` ein Werttyp ist, wird die Konvertierung zur Laufzeit als Unboxing-Konvertierung ausgeführt. Andernfalls wird die Konvertierung als explizite Verweis Konvertierung oder Identitäts Konvertierung ausgeführt.
*  Von `T` bis zu allen *INTERFACE_TYPE* `I` gibt es keine implizite Konvertierung von `T` in `I`. Wenn `T` ein Werttyp ist, wird die Konvertierung zur Laufzeit als Boxing-Konvertierung gefolgt von einer expliziten Verweis Konvertierung ausgeführt. Andernfalls wird die Konvertierung als explizite Verweis Konvertierung oder Identitäts Konvertierung ausgeführt.
*  Von einem Typparameter `U` zum `T`, sofern `T` von `U` abhängig sind ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)). Wenn `U` ein Werttyp ist, sind `T` und `U` bei der Laufzeit notwendigerweise denselben Typ, und es wird keine Konvertierung durchgeführt. Andernfalls wird die Konvertierung als Unboxing-Konvertierung ausgeführt, wenn `T` ein Werttyp ist. Andernfalls wird die Konvertierung als explizite Verweis Konvertierung oder Identitäts Konvertierung ausgeführt.

Wenn `T` bekannt ist, dass es sich um einen Verweistyp handelt, werden die obigen Konvertierungen als explizite Verweis Konvertierungen klassifiziert ([explizite Verweis Konvertierungen](conversions.md#explicit-reference-conversions)). Wenn `T` nicht bekannt ist, dass es sich um einen Verweistyp handelt, werden die obigen Konvertierungen als Unboxing-Konvertierungen ([Unboxing-Konvertierungen](conversions.md#unboxing-conversions)) klassifiziert.

Die obigen Regeln erlauben keine direkte explizite Konvertierung von einem uneingeschränkten Typparameter in einen nicht-Schnittstellentyp, was möglicherweise überraschend ist. Der Grund für diese Regel besteht darin, Verwirrung zu vermeiden und die Semantik solcher Konvertierungen zu löschen. Betrachten Sie beispielsweise die folgende Deklaration:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

Wenn die direkte explizite Konvertierung von `t` in `int` zulässig wäre, kann man leicht davon ausgehen, dass `X<int>.F(7)` `7L`zurückgeben würde. Dies würde jedoch nicht der Fall sein, da die standardmäßigen numerischen Konvertierungen nur berücksichtigt werden, wenn die Typen bekannt sind, dass Sie zur Bindungs Zeit numerisch sind. Damit die Semantik eindeutig ist, muss das obige Beispiel stattdessen geschrieben werden:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

Dieser Code wird jetzt kompiliert, aber das Ausführen von `X<int>.F(7)` würde zur Laufzeit eine Ausnahme auslösen, da ein geachtelter `int` nicht direkt in eine `long`konvertiert werden kann.

### <a name="user-defined-explicit-conversions"></a>Benutzerdefinierte explizite Konvertierungen

Eine benutzerdefinierte explizite Konvertierung besteht aus einer optionalen expliziten Standard Konvertierung, gefolgt von der Ausführung eines benutzerdefinierten impliziten oder expliziten Konvertierungs Operators, gefolgt von einer anderen optionalen expliziten Standard Konvertierung. Die genauen Regeln für das Auswerten benutzerdefinierter expliziter Konvertierungen werden bei der [Verarbeitung benutzerdefinierter expliziter Konvertierungen](conversions.md#processing-of-user-defined-explicit-conversions)beschrieben.

## <a name="standard-conversions"></a>Standardkonvertierungen

Bei den Standard Konvertierungen handelt es sich um vordefinierte Konvertierungen, die als Teil einer benutzerdefinierten Konvertierung auftreten können.

### <a name="standard-implicit-conversions"></a>Implizite Standard Konvertierungen

Die folgenden impliziten Konvertierungen werden als implizite Standard Konvertierungen klassifiziert:

*  Identitäts Konvertierungen ([Identitäts Konvertierung](conversions.md#identity-conversion))
*  Implizite numerische Konvertierungen ([implizite numerische Konvertierungen](conversions.md#implicit-numeric-conversions))
*  Implizite Konvertierungen, die NULL-Werte zulassen ([implizite Konvertierungen auf NULL](conversions.md#implicit-nullable-conversions)
*  Implizite Verweis Konvertierungen ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions))
*  Boxing-Konvertierungen ([Boxing-Konvertierungen](conversions.md#boxing-conversions))
*  Implizite Konstante Ausdrucks Konvertierungen ([implizite dynamische Konvertierungen](conversions.md#implicit-dynamic-conversions))
*  Implizite Konvertierungen mit Typparametern ([implizite Konvertierungen mit Typparametern](conversions.md#implicit-conversions-involving-type-parameters))

Die standardmäßigen impliziten Konvertierungen schließen speziell benutzerdefinierte implizite Konvertierungen aus.

### <a name="standard-explicit-conversions"></a>Explizite Standard Konvertierungen

Die expliziten Standard Konvertierungen sind alle standardmäßigen impliziten Konvertierungen und die Teilmenge der expliziten Konvertierungen, für die eine gegenteilige standardmäßige implizite Konvertierung vorhanden ist. Anders ausgedrückt: Wenn eine implizite Standard Konvertierung von einem Typ `A` in einen Typ `B`vorhanden ist, ist eine explizite Standard Konvertierung vom Typ `A` in den Typ `B` und vom Typ `B` zum Typ `A`vorhanden.

## <a name="user-defined-conversions"></a>Benutzerdefinierte Konvertierungen

C#ermöglicht das Erweitern der vordefinierten und expliziten Konvertierungen durch ***benutzerdefinierte Konvertierungen***. Benutzerdefinierte Konvertierungen werden durch das Deklarieren von Konvertierungs Operatoren ([Konvertierungs Operatoren](classes.md#conversion-operators)) in Klassen-und Strukturtypen eingeführt.

### <a name="permitted-user-defined-conversions"></a>Zulässige benutzerdefinierte Konvertierungen

C#gestattet, dass nur bestimmte benutzerdefinierte Konvertierungen deklariert werden. Insbesondere ist es nicht möglich, eine bereits vorhandene implizite oder explizite Konvertierung neu zu definieren.

Verwenden Sie für einen angegebenen Quelltyp `S` und einen Zieltyp `T`, wenn `S` oder `T` Typen sind, die NULL-Werte zulassen, `S0` und `T0` auf ihre zugrunde liegenden Typen. andernfalls sind `S0` und `T0` gleich `S` bzw. `T`. Eine Klasse oder Struktur darf eine Konvertierung von einem Quelltyp `S` in einen Zieltyp deklarieren `T` nur dann, wenn Folgendes zutrifft:

*  `S0` und `T0` sind unterschiedliche Typen.
*  Entweder `S0` oder `T0` ist der Klassen-oder Strukturtyp, in dem die Operator Deklaration stattfindet.
*  Weder `S0` noch `T0` ist ein *INTERFACE_TYPE*.
*  Das Ausschließen von benutzerdefinierten Konvertierungen ist nicht von `S` zu `T` oder von `T` zum `S`vorhanden.

Die Einschränkungen, die für benutzerdefinierte Konvertierungen gelten, werden in den [Konvertierungs Operatoren](classes.md#conversion-operators)weiter erläutert.

### <a name="lifted-conversion-operators"></a>Operator für gesteigerte Konvertierung

Bei einem benutzerdefinierten Konvertierungs Operator, der von einem Werttyp, der keine NULL-Werte zulässt, `S` in einen Werttyp, der keine NULL-Werte zulässt `T`, ist ein ***Operator mit erhöhten Konvertierungen*** vorhanden, der von `S?` in `T?`konvertiert Dieser Operator mit erhöhten Konvertierungen führt einen zum Entpacken von `S?` auf `S` gefolgt von der benutzerdefinierten Konvertierung von `S` in `T` gefolgt von einem Umbrüchen von `T` zu `T?`, mit dem Unterschied, dass ein NULL-Wert `S?` direkt in eine `T?`mit NULL-Wert konvertiert.

Ein Operator mit erhöhten Konvertierungen hat dieselbe implizite oder explizite Klassifizierung wie der zugrunde liegende benutzerdefinierte Konvertierungs Operator. Der Begriff "benutzerdefinierte Konvertierung" gilt für die Verwendung von benutzerdefinierten und erhöhten Konvertierungs Operatoren.

### <a name="evaluation-of-user-defined-conversions"></a>Auswertung von benutzerdefinierten Konvertierungen

Eine benutzerdefinierte Konvertierung konvertiert einen Wert vom Typ, der als ***Quelltyp***bezeichnet wird, in einen anderen Typ, der als ***Zieltyp***bezeichnet wird. Bei der Auswertung einer benutzerdefinierten Konvertierung wird das Auffinden des ***spezifischsten*** benutzerdefinierten Konvertierungs Operators für bestimmte Quell-und Zieltypen ermittelt. Diese Bestimmung ist in mehrere Schritte unterteilt:

*  Suchen des Satzes von Klassen und Strukturen, von denen benutzerdefinierte Konvertierungs Operatoren berücksichtigt werden. Diese Gruppe besteht aus dem Quelltyp und den zugehörigen Basisklassen sowie dem Zieltyp und den zugehörigen Basisklassen (mit den impliziten Annahmen, dass nur Klassen und Strukturen benutzerdefinierte Operatoren deklarieren können, und dass nicht Klassentypen keine Basisklassen aufweisen). Wenn entweder der Quell-oder Zieltyp ein nullable_type ist, wird stattdessen der zugrunde liegende Typ verwendet, wenn der Quell-oder Zieltyp einist.
*  Aus diesem Satz von Typen, um zu bestimmen, welche benutzerdefinierten und erhöhten Konvertierungs Operatoren anwendbar sind. Damit ein Konvertierungs Operator anwendbar ist, muss es möglich sein, eine Standard Konvertierung ([Standard Konvertierungen](conversions.md#standard-conversions)) vom Quelltyp in den Operanden des Operators auszuführen. Außerdem muss es möglich sein, eine Standard Konvertierung vom Ergebnistyp des Operators in den Zieltyp auszuführen.
*  Aus dem Satz der anwendbaren benutzerdefinierten Operatoren, wobei festgelegt wird, welcher Operator eindeutig der spezifischsten ist. In der Regel ist der spezifischere Operator der Operator, dessen Operanden dem Quelltyp "am nächsten" und dessen Ergebnistyp "am nächsten" dem Zieltyp entspricht. Benutzerdefinierte Konvertierungs Operatoren werden für gesteigerte Konvertierungs Operatoren bevorzugt. Die genauen Regeln zum Einrichten des spezifischsten benutzerdefinierten Konvertierungs Operators werden in den folgenden Abschnitten definiert.

Nachdem ein spezifiziererer benutzerdefinierter Konvertierungs Operator identifiziert wurde, umfasst die tatsächliche Ausführung der benutzerdefinierten Konvertierung bis zu drei Schritte:

*  Wenn dies erforderlich ist, wird eine Standard Konvertierung vom Quelltyp in den Operanden des benutzerdefinierten oder des aufzurufenden Konvertierungs Operators durchgeführt.
*  Im nächsten Schritt wird der benutzerdefinierte oder der Operator für die Aufhebung der Konvertierung aufgerufen, um die Konvertierung auszuführen.
*  Wenn dies erforderlich ist, wird eine Standard Konvertierung vom Ergebnistyp des benutzerdefinierten oder des Operators für die gesteigerte Konvertierung in den Zieltyp durchgeführt.

Die Auswertung einer benutzerdefinierten Konvertierung umfasst nie mehr als einen benutzerdefinierten oder einen Operator mit erhöhten Konvertierungen. Anders ausgedrückt: bei einer Konvertierung vom Typ `S` in den Typ `T` wird nie zuerst eine benutzerdefinierte Konvertierung von `S` in `X` ausgeführt und dann eine benutzerdefinierte Konvertierung von `X` in `T`ausgeführt.

Die genauen Definitionen der Auswertung benutzerdefinierter impliziter oder expliziter Konvertierungen werden in den folgenden Abschnitten angegeben. In den Definitionen werden die folgenden Begriffe verwendet:

*  Wenn eine standardmäßige implizite Konvertierung ([standardmäßige implizite Konvertierungen](conversions.md#standard-implicit-conversions)) von einem Typ vorhanden ist, der `A` in einen Typ `B`ist, und wenn weder `A` noch `B` *INTERFACE_TYPE*s sind, wird `A` als ***mit `B`umschlossen*** bezeichnet, und `B` ***wird als "`A`"*** bezeichnet.
*  Der ***umfassendste Typ*** in einer Reihe von Typen ist der einzige Typ, der alle anderen Typen im Satz umfasst. Wenn kein einzelner Typ alle anderen Typen umfasst, hat der Satz keinen ganz umfassenden Typ. In intuitiver Hinsicht ist der umfassendste Typ der "größte" Typ im Satz – der einzige Typ, in den jeder der anderen Typen implizit konvertiert werden kann.
*  Der ***am häufigsten*** in einem Satz von Typen eingeschlossenen Typ ist ein Typ, der von allen anderen Typen im Satz eingeschlossen wird. Wenn kein einzelner Typ von allen anderen Typen eingeschlossen wird, hat der Satz nicht den meisten Typ. In intuitiver Hinsicht ist der Typ, der am häufigsten in der Menge enthalten ist, der "kleinste" Typ im Satz – ein Typ, der implizit in jeden der anderen Typen konvertiert werden kann.

### <a name="processing-of-user-defined-implicit-conversions"></a>Verarbeiten von benutzerdefinierten impliziten Konvertierungen

Eine benutzerdefinierte implizite Konvertierung vom Typ `S` in den Typ `T` wird wie folgt verarbeitet:

*  Bestimmen Sie die Typen `S0` und `T0`. Wenn `S` oder `T` Typen NULL-Werte zulassen, sind `S0` und `T0` ihre zugrunde liegenden Typen. andernfalls sind `S0` und `T0` gleich `S` bzw. `T`.
*  Suchen Sie den Satz von Typen, `D`, von dem benutzerdefinierte Konvertierungs Operatoren berücksichtigt werden. Dieser Satz besteht aus `S0` (wenn `S0` eine Klasse oder Struktur ist), den Basisklassen von `S0` (wenn `S0` eine Klasse ist) und `T0` (wenn `T0` eine Klasse oder Struktur ist).
*  Suchen Sie den Satz der anwendbaren benutzerdefinierten und erhöhten Konvertierungs Operatoren, `U`. Dieser Satz besteht aus den benutzerdefinierten und angehobenen impliziten Konvertierungs Operatoren, die von den Klassen oder Strukturen in `D` deklariert werden, die von einem Typ konvertieren, der `S` in einen von `T`umschlossen Typ konvertiert. Wenn `U` leer ist, ist die Konvertierung nicht definiert, und es tritt ein Kompilierzeitfehler auf.
*  Suchen Sie den spezifischsten Quelltyp (`SX`) der Operatoren in `U`:
    * Wenn einer der Operatoren in `U` aus `S`konvertieren, wird `SX` `S`.
    * Andernfalls ist `SX` der in der kombinierten Gruppe von Quell Typen der Operatoren in `U`der Typ, der am häufigsten eingeschlossen ist. Wenn nicht genau ein Typ gefunden werden kann, der in der zwischen Version enthalten ist, ist die Konvertierung mehrdeutig, und es tritt ein Kompilierzeitfehler auf.
*  Suchen Sie den spezifischsten Zieltyp (`TX`) der Operatoren in `U`:
    * Wenn einer der Operatoren in `U` in `T`konvertiert wird, wird `TX` `T`.
    * Andernfalls ist `TX` der umfassendste Typ in der kombinierten Gruppe von Zieltypen der Operatoren in `U`. Wenn genau ein Typ mit der höchsten Verfügbarkeit nicht gefunden werden kann, ist die Konvertierung mehrdeutig, und es tritt ein Kompilierzeitfehler auf.
*  Suchen Sie den spezifischsten Konvertierungs Operator:
    * Wenn `U` genau einen benutzerdefinierten Konvertierungs Operator enthält, der von `SX` in `TX`konvertiert, ist dies der spezifischere Konvertierungs Operator.
    * Andernfalls, wenn `U` genau einen Operator mit erhöhten Umwandlungen enthält, der von `SX` in `TX`konvertiert, ist dies der spezifischere Konvertierungs Operator.
    * Andernfalls ist die Konvertierung mehrdeutig, und es tritt ein Kompilierzeitfehler auf.
*  Übernehmen Sie schließlich die Konvertierung:
    * Wenn `S` nicht `SX`ist, wird eine standardmäßige implizite Konvertierung von `S` in `SX` ausgeführt.
    * Der spezifischere Konvertierungs Operator wird aufgerufen, um von `SX` in `TX`zu konvertieren.
    * Wenn `TX` nicht `T`ist, wird eine standardmäßige implizite Konvertierung von `TX` in `T` ausgeführt.

### <a name="processing-of-user-defined-explicit-conversions"></a>Verarbeiten benutzerdefinierter expliziter Konvertierungen

Eine benutzerdefinierte explizite Konvertierung vom Typ `S` in den Typ `T` wird wie folgt verarbeitet:

*  Bestimmen Sie die Typen `S0` und `T0`. Wenn `S` oder `T` Typen NULL-Werte zulassen, sind `S0` und `T0` ihre zugrunde liegenden Typen. andernfalls sind `S0` und `T0` gleich `S` bzw. `T`.
*  Suchen Sie den Satz von Typen, `D`, von dem benutzerdefinierte Konvertierungs Operatoren berücksichtigt werden. Dieser Satz besteht aus `S0` (wenn `S0` eine Klasse oder Struktur ist), den Basisklassen von `S0` (wenn `S0` eine Klasse ist), `T0` (wenn `T0` eine Klasse oder Struktur ist) und den Basisklassen von `T0` (wenn `T0` eine Klasse ist).
*  Suchen Sie den Satz der anwendbaren benutzerdefinierten und erhöhten Konvertierungs Operatoren, `U`. Dieser Satz besteht aus den benutzerdefinierten, impliziten oder expliziten Konvertierungs Operatoren, die von den Klassen oder Strukturen in `D` deklariert werden, die von einem Typ konvertieren, der durch `S` in einen Typ konvertiert wird, der durch `T`eingeschlossen oder eingeschlossen wird. Wenn `U` leer ist, ist die Konvertierung nicht definiert, und es tritt ein Kompilierzeitfehler auf.
*  Suchen Sie den spezifischsten Quelltyp (`SX`) der Operatoren in `U`:
    * Wenn einer der Operatoren in `U` aus `S`konvertieren, wird `SX` `S`.
    * Andernfalls ist `SX`, wenn einer der Operatoren in `U` von Typen konvertiert, die `S`umfassen, der am häufigsten in der kombinierten Gruppe von Quell Typen dieser Operatoren enthaltenen Typ. Wenn kein Typ gefunden werden kann, ist die Konvertierung mehrdeutig, und es tritt ein Kompilierzeitfehler auf.
    * Andernfalls ist `SX` der umfassendste Typ in der kombinierten Gruppe von Quell Typen der Operatoren in `U`. Wenn genau ein Typ mit der höchsten Verfügbarkeit nicht gefunden werden kann, ist die Konvertierung mehrdeutig, und es tritt ein Kompilierzeitfehler auf.
*  Suchen Sie den spezifischsten Zieltyp (`TX`) der Operatoren in `U`:
    * Wenn einer der Operatoren in `U` in `T`konvertiert wird, wird `TX` `T`.
    * Wenn einer der Operatoren in `U` in Typen konvertiert werden soll, die von `T`eingeschlossen werden, dann ist `TX` der umfassendste Typ in der kombinierten Gruppe von Zieltypen dieser Operatoren. Wenn genau ein Typ mit der höchsten Verfügbarkeit nicht gefunden werden kann, ist die Konvertierung mehrdeutig, und es tritt ein Kompilierzeitfehler auf.
    * Andernfalls ist `TX` der kombinierte Typ in der kombinierten Menge von Zieltypen der Operatoren in `U`. Wenn kein Typ gefunden werden kann, ist die Konvertierung mehrdeutig, und es tritt ein Kompilierzeitfehler auf.
*  Suchen Sie den spezifischsten Konvertierungs Operator:
    * Wenn `U` genau einen benutzerdefinierten Konvertierungs Operator enthält, der von `SX` in `TX`konvertiert, ist dies der spezifischere Konvertierungs Operator.
    * Andernfalls, wenn `U` genau einen Operator mit erhöhten Umwandlungen enthält, der von `SX` in `TX`konvertiert, ist dies der spezifischere Konvertierungs Operator.
    * Andernfalls ist die Konvertierung mehrdeutig, und es tritt ein Kompilierzeitfehler auf.
*  Übernehmen Sie schließlich die Konvertierung:
    * Wenn `S` nicht `SX`ist, wird eine explizite Standard Konvertierung von `S` in `SX` ausgeführt.
    * Der spezifischere benutzerdefinierte Konvertierungs Operator wird aufgerufen, um von `SX` in `TX`zu konvertieren.
    * Wenn `TX` nicht `T`ist, wird eine explizite Standard Konvertierung von `TX` in `T` ausgeführt.

## <a name="anonymous-function-conversions"></a>Anonyme Funktions Konvertierungen

Eine *anonymous_method_expression* oder *lambda_expression* wird als anonyme Funktion ([Anonyme Funktions Ausdrücke](expressions.md#anonymous-function-expressions)) klassifiziert. Der Ausdruck weist keinen Typ auf, kann aber implizit in einen kompatiblen Delegattyp oder Ausdrucks Strukturtyp konvertiert werden. Ein anonymer Funktions `F` ist insbesondere mit einem `D` bereitgestellten Delegattyp kompatibel:

*  Wenn `F` eine *anonymous_function_signature*enthält, verfügen `D` und `F` über die gleiche Anzahl von Parametern.
*  Wenn `F` keinen *anonymous_function_signature*enthält, haben `D` möglicherweise NULL oder mehr Parameter eines beliebigen Typs, solange kein Parameter von `D` den `out` Parametermodifizierer aufweist.
*  Wenn `F` über eine explizit typisierte Parameterliste verfügt, weist jeder Parameter in `D` den gleichen Typ und die gleichen Modifizierer wie der entsprechende Parameter in `F`auf.
*  Wenn `F` eine implizit typisierte Parameterliste aufweist, hat `D` keine `ref` oder `out` Parameter.
*  Wenn der Text des `F` ein Ausdruck ist und entweder `D` einen `void` Rückgabetyp hat oder `F` Async ist und `D` den Rückgabetyp `Task`hat, dann ist der Text des `F` ein gültiger Ausdruck (WRT [Expressions](expressions.md)), der als *`D`* ([Ausdrucks Anweisungen](statements.md#expression-statements)) zulässig ist.
*  Wenn der Hauptteil des `F` ein Anweisungsblock ist und entweder `D` einen `void` Rückgabetyp hat oder `F` Async und `D` den Rückgabetyp `Task`hat, ist der Text des `F` ein gültiger Anweisungsblock (WRT- [Blöcke](statements.md#blocks)), in dem keine `D`Anweisung einen Ausdruck angibt.
*  Wenn der Text des `F` ein Ausdruck ist und *entweder* `F` nicht Async ist und `D` einen nicht leeren Rückgabetyp `T`hat, *oder* `F` asynchron ist und `D` einen Rückgabetyp `Task<T>`hat, ist der Text des `F` ein gültiger Ausdruck (WRT [Expressions](expressions.md)), der implizit in `D`konvertiert werden kann.
*  Wenn der Hauptteil des `F` ein Anweisungsblock ist und *entweder* `F` nicht Async ist und `D` einen nicht leeren Rückgabetyp `T`hat, *oder* `F` ist async, und `D` hat einen Rückgabetyp `Task<T>`. Wenn jedem Parameter `F` der Typ des entsprechenden Parameters in `D`zugewiesen wird, ist der Text des `F` ein gültiger Anweisungsblock (WRT- [Blöcke](statements.md#blocks)) mit einem nicht erreichbaren Endpunkt, bei dem jede `return` Anweisung einen Ausdruck angibt, der implizit in `T`konvertiert werden kann.

Aus Gründen der Kürze verwendet dieser Abschnitt die Kurzform für die Aufgaben Typen `Task` und `Task<T>` ([Async-Funktionen](classes.md#async-functions)).

Ein Lambda-Ausdruck `F` ist mit einem Ausdrucks bauentyp kompatibel `Expression<D>` wenn `F` mit dem Delegattyp `D`kompatibel ist. Beachten Sie, dass dies nicht für anonyme Methoden gilt, sondern nur für Lambda-Ausdrücke.

Bestimmte Lambda-Ausdrücke können nicht in Ausdrucks Baumstruktur Typen konvertiert werden: Obwohl die Konvertierung *vorhanden*ist, schlägt Sie zur Kompilierzeit fehl. Dies ist der Fall, wenn der Lambda-Ausdruck:

*  Weist einen *Block* Text auf.
*  Enthält einfache oder Verbund Zuweisungs Operatoren.
*  Enthält einen dynamisch gebundenen Ausdruck.
*  Ist Async

In den folgenden Beispielen wird ein generischer Delegattyp `Func<A,R>` verwendet, der eine Funktion darstellt, die ein Argument vom Typ `A` annimmt und einen Wert vom Typ `R`zurückgibt:
```csharp
delegate R Func<A,R>(A arg);
```

In den Zuweisungen
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
der Parameter und die Rückgabe Typen der einzelnen anonymen Funktionen werden vom Typ der Variablen bestimmt, der die anonyme Funktion zugewiesen wird.

Bei der ersten Zuweisung wird die anonyme Funktion erfolgreich in den Delegattyp konvertiert `Func<int,int>` weil `x` den Typ `int`erhält, `x+1` ein gültiger Ausdruck ist, der implizit in den Typ `int`konvertiert werden kann.

Entsprechend konvertiert die zweite Zuweisung die anonyme Funktion erfolgreich in den Delegattyp `Func<int,double>`, da das Ergebnis `x+1` (vom Typ `int`) implizit in den Typ `double`konvertiert werden kann.

Die dritte Zuweisung ist jedoch ein Kompilierzeitfehler, da das Ergebnis von `x+1` (vom Typ `double`) nicht implizit in den Typ `int`konvertiert werden kann, wenn `x` den Typ `double`erhält.

Mit der vierten Zuweisung wird die anonyme Async-Funktion erfolgreich in den Delegattyp konvertiert `Func<int, Task<int>>` da das Ergebnis `x+1` (vom Typ `int`) implizit in den Ergebnistyp `int` des Aufgabentyp `Task<int>`konvertiert werden kann.

Anonyme Funktionen können die Überladungs Auflösung beeinflussen und an einem Typrückschluss teilnehmen. Weitere Informationen finden Sie unter [Funktionsmember](expressions.md#function-members) .

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>Auswertung anonymer Funktions Konvertierungen in Delegattypen

Die Konvertierung einer anonymen Funktion in einen Delegattyp erzeugt eine Delegatinstanz, die auf die anonyme Funktion und den (möglicherweise leeren) Satz erfasster äußerer Variablen verweist, die zum Zeitpunkt der Auswertung aktiv sind. Wenn der Delegat aufgerufen wird, wird der Text der anonymen Funktion ausgeführt. Der Code im Text wird mit dem Satz erfasster äußerer Variablen ausgeführt, auf die der Delegat verweist.

Die Aufruf Liste eines Delegaten, der aus einer anonymen Funktion erstellt wurde, enthält einen einzelnen Eintrag. Das genaue Zielobjekt und die Ziel Methode des Delegaten sind nicht angegeben. Insbesondere ist nicht angegeben, ob das Zielobjekt des Delegaten `null`, der `this` Wert des einschließenden Funktionsmembers oder ein anderes Objekt ist.

Konvertierungen von semantisch identischen anonymen Funktionen mit dem gleichen (möglicherweise leeren) Satz erfasster externer Variablen Instanzen in dieselben Delegattypen sind zulässig (jedoch nicht erforderlich), um dieselbe Delegatinstanz zurückzugeben. Der Begriff semantisch identisch wird hier verwendet, um zu bedeuten, dass die Ausführung der anonymen Funktionen in allen Fällen dieselben Effekte mit denselben Argumenten erzeugt. Diese Regel ermöglicht es, Code wie den folgenden zu optimieren.

```csharp
delegate double Function(double x);

class Test
{
    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void F(double[] a, double[] b) {
        a = Apply(a, (double x) => Math.Sin(x));
        b = Apply(b, (double y) => Math.Sin(y));
        ...
    }
}
```

Da die beiden anonymen Funktions Delegaten denselben (leeren) Satz erfasster äußerer Variablen aufweisen und die anonymen Funktionen semantisch identisch sind, ist es dem Compiler gestattet, dass die Delegaten auf dieselbe Ziel Methode verweisen. Tatsächlich ist es dem Compiler gestattet, dieselbe Delegatinstanz aus beiden anonymen Funktions Ausdrücken zurückzugeben.

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>Auswertung anonymer Funktions Konvertierungen in Ausdrucks Baumstruktur Typen

Die Konvertierung einer anonymen Funktion in einen Ausdrucks bauentyp erzeugt eine Ausdrucks Baumstruktur ([Ausdrucks Baumstruktur Typen](types.md#expression-tree-types)). Genauer bedeutet, dass die Auswertung der anonymen Funktions Konvertierung zur Erstellung einer Objektstruktur führt, die die Struktur der anonymen Funktion darstellt. Die genaue Struktur der Ausdrucks Baumstruktur sowie der genaue Prozess für die Erstellung werden implementiert.

### <a name="implementation-example"></a>Beispiel für die Implementierung

In diesem Abschnitt wird eine mögliche Implementierung anonymer Funktions Konvertierungen in Bezug C# auf andere Konstrukte beschrieben. Die hier beschriebene Implementierung basiert auf denselben Prinzipien, die vom Microsoft C# -Compiler verwendet werden, aber es handelt sich nicht um eine obligatorische Implementierung, und es handelt sich nicht um die einzige Möglichkeit. Die Konvertierung in Ausdrucks Baumstrukturen wird nur kurz erwähnt, da die genaue Semantik außerhalb des Gültigkeits Bereichs dieser Spezifikation liegt.

Der Rest dieses Abschnitts enthält mehrere Beispiele für Code, der anonyme Funktionen mit unterschiedlichen Merkmalen enthält. Für jedes Beispiel wird eine entsprechende Übersetzung in Code bereitgestellt, C# der nur andere Konstrukte verwendet. In den Beispielen wird davon ausgegangen, dass der Bezeichner `D` von den folgenden Delegattyp repräsentiert:
```csharp
public delegate void D();
```

Die einfachste Form einer anonymen Funktion ist eine, die keine äußeren Variablen aufzeichnet:
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

Dies kann in eine Delegatinstanziierung übersetzt werden, die auf eine vom Compiler generierte statische Methode verweist, in der der Code der anonymen Funktion platziert wird:
```csharp
class Test
{
    static void F() {
        D d = new D(__Method1);
    }

    static void __Method1() {
        Console.WriteLine("test");
    }
}
```

Im folgenden Beispiel verweist die anonyme Funktion auf Instanzmember von `this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

Dies kann in eine vom Compiler generierte Instanzmethode übersetzt werden, die den Code der anonymen Funktion enthält:
```csharp
class Test
{
    int x;

    void F() {
        D d = new D(__Method1);
    }

    void __Method1() {
        Console.WriteLine(x);
    }
}
```

In diesem Beispiel erfasst die anonyme Funktion eine lokale Variable:
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

Die Lebensdauer der lokalen Variablen muss nun mindestens zur Lebensdauer des anonymen Funktions Delegaten verlängert werden. Dies kann erreicht werden, indem Sie die lokale Variable in ein Feld einer vom Compiler generierten Klasse einbinden. Die Instanziierung der lokalen Variablen ([Instanziierung lokaler Variablen](expressions.md#instantiation-of-local-variables)) entspricht dann dem Erstellen einer Instanz der vom Compiler generierten Klasse, und der Zugriff auf die lokale Variable entspricht dem Zugriff auf ein Feld in der Instanz der vom Compiler generierten Klasse. Außerdem wird die anonyme Funktion zu einer Instanzmethode der vom Compiler generierten Klasse:
```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.y = 123;
        D d = new D(__locals1.__Method1);
    }

    class __Locals1
    {
        public int y;

        public void __Method1() {
            Console.WriteLine(y);
        }
    }
}
```

Zum Schluss erfasst die folgende anonyme Funktion `this` und zwei lokale Variablen mit unterschiedlichen Lebens dauern:
```csharp
class Test
{
    int x;

    void F() {
        int y = 123;
        for (int i = 0; i < 10; i++) {
            int z = i * 2;
            D d = () => { Console.WriteLine(x + y + z); };
        }
    }
}
```

Hier wird eine vom Compiler generierte Klasse für jeden Anweisungsblock erstellt, in dem lokale Variablen aufgezeichnet werden, sodass die lokalen Variablen in den verschiedenen Blöcken eine unabhängige Lebensdauer aufweisen können. Eine Instanz von `__Locals2`, die vom Compiler generierte-Klasse für den inneren Anweisungsblock, enthält die lokale Variable `z` und ein Feld, das auf eine Instanz von `__Locals1`verweist.  Eine Instanz von `__Locals1`, die vom Compiler generierte-Klasse für den äußeren Anweisungsblock, enthält die lokale Variable `y` und ein Feld, das auf `this` des einschließenden Funktionsmembers verweist. Mit diesen Datenstrukturen ist es möglich, alle aufgezeichneten äußeren Variablen über eine Instanz von `__Local2`zu erreichen, und der Code der anonymen Funktion kann daher als Instanzmethode dieser Klasse implementiert werden.

```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.__this = this;
        __locals1.y = 123;
        for (int i = 0; i < 10; i++) {
            __Locals2 __locals2 = new __Locals2();
            __locals2.__locals1 = __locals1;
            __locals2.z = i * 2;
            D d = new D(__locals2.__Method1);
        }
    }

    class __Locals1
    {
        public Test __this;
        public int y;
    }

    class __Locals2
    {
        public __Locals1 __locals1;
        public int z;

        public void __Method1() {
            Console.WriteLine(__locals1.__this.x + __locals1.y + z);
        }
    }
}
```

Dieselbe Technik, die hier zum Erfassen von lokalen Variablen angewendet wird, kann auch beim Umrechnen anonymer Funktionen in Ausdrucks Baumstrukturen verwendet werden: Verweise auf die vom Compiler generierten Objekte können in der Ausdrucks Baumstruktur gespeichert werden, und der Zugriff auf die lokalen Variablen kann wird als Feld Zugriffen für diese Objekte dargestellt. Der Vorteil dieses Ansatzes besteht darin, dass die "angehobenen" lokalen Variablen für Delegaten und Ausdrucks Baumstrukturen freigegeben werden können.

## <a name="method-group-conversions"></a>Methoden Gruppen Konvertierungen

Eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) ist in einer Methoden Gruppe ([Ausdrucks Klassifizierungen](expressions.md#expression-classifications)) zu einem kompatiblen Delegattyp vorhanden. Bei einem Delegattyp `D` und einem Ausdrucks `E`, der als Methoden Gruppe klassifiziert ist, gibt es eine implizite Konvertierung von `E` in `D`, wenn `E` mindestens eine Methode enthält, die in ihrer normalen Form ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) auf eine Argumentliste anwendbar ist, die mithilfe der Parametertypen und Modifizierer von `D`erstellt wurde, wie im folgenden beschrieben.

Die Kompilierzeit Anwendung einer Konvertierung von einer Methoden Gruppe `E` in einen Delegattyp `D` wird im folgenden beschrieben. Beachten Sie, dass das vorhanden sein einer impliziten Konvertierung von `E` in `D` nicht sicherstellt, dass die Kompilierzeit Anwendung der Konvertierung ohne Fehler erfolgreich ausgeführt wird.

*  Es wird eine einzelne Methode `M` ausgewählt, die einem Methodenaufruf ([Methodenaufrufe](expressions.md#method-invocations)) der Form `E(A)`entspricht, mit den folgenden Änderungen:
    * Bei der Argumentliste `A` handelt es sich um eine Liste von Ausdrücken, die jeweils als Variable und mit dem Typ und Modifizierer (`ref` oder `out`) des entsprechenden Parameters in der *formal_parameter_list* von `D`klassifiziert sind.
    * Bei den Kandidaten Methoden handelt es sich nur um die Methoden, die in ihrer normalen Form ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) anwendbar sind, nicht die, die nur in der erweiterten Form anwendbar sind.
*  Wenn der Algorithmus von [Methoden](expressions.md#method-invocations) aufrufen einen Fehler erzeugt, tritt ein Kompilierzeitfehler auf. Andernfalls erzeugt der Algorithmus eine einzige beste Methode `M` die gleiche Anzahl von Parametern wie `D` haben, und die Konvertierung wird als vorhanden betrachtet.
*  Die ausgewählte Methode `M` muss kompatibel sein ([delegatkompatibilität](delegates.md#delegate-compatibility)) mit dem Delegattyp `D`, oder andernfalls tritt ein Kompilierzeitfehler auf.
*  Wenn die ausgewählte Methode `M` eine Instanzmethode ist, bestimmt der Instanzausdruck, der `E` zugeordnet ist, das Zielobjekt des Delegaten.
*  Wenn die ausgewählte Methode M eine Erweiterungsmethode ist, die mithilfe eines Element Zugriffs auf einen Instanzausdruck bezeichnet wird, bestimmt dieser Instanzausdruck das Zielobjekt des Delegaten.
*  Das Ergebnis der Konvertierung ist ein Wert vom Typ `D`, nämlich ein neu erstellter Delegat, der auf die ausgewählte Methode und das ausgewählte Zielobjekt verweist.
*  Beachten Sie, dass dieser Prozess zur Erstellung eines Delegaten zu einer Erweiterungsmethode führen kann, wenn der Algorithmus von [Methoden](expressions.md#method-invocations) aufrufen keine Instanzmethode findet, sondern den Aufruf von `E(A)` als Erweiterungs Methodenaufruf ([Erweiterungs Methodenaufrufe](expressions.md#extension-method-invocations)) erfolgreich verarbeitet. Ein Delegat, der auf diese Weise erstellt wurde, erfasst sowohl die Erweiterungsmethode als auch das erste Argument.

Im folgenden Beispiel werden Methoden Gruppen Konvertierungen veranschaulicht:
```csharp
delegate string D1(object o);

delegate object D2(string s);

delegate object D3();

delegate string D4(object o, params object[] a);

delegate string D5(int i);

class Test
{
    static string F(object o) {...}

    static void G() {
        D1 d1 = F;            // Ok
        D2 d2 = F;            // Ok
        D3 d3 = F;            // Error -- not applicable
        D4 d4 = F;            // Error -- not applicable in normal form
        D5 d5 = F;            // Error -- applicable but not compatible

    }
}
```

Durch die Zuweisung zu `d1` wird die `F` der Methoden Gruppe implizit in einen Wert vom Typ `D1`konvertiert.

Die Zuweisung zu `d2` zeigt, wie es möglich ist, einen Delegaten für eine Methode zu erstellen, die weniger abgeleitete Parametertypen (kontra Variant) und einen stärker abgeleiteten (kovarianten) Rückgabetyp aufweist.

Die Zuweisung zu `d3` zeigt, wie keine Konvertierung vorhanden ist, wenn die Methode nicht anwendbar ist.

Die Zuweisung zu `d4` zeigt, wie die Methode in ihrer normalen Form anwendbar sein muss.

Die Zuweisung zu `d5` zeigt, wie Parameter und Rückgabe Typen des Delegaten und der Methode nur für Verweis Typen unterschiedlich sein dürfen.

Wie bei allen anderen impliziten und expliziten Konvertierungen kann der Cast-Operator verwendet werden, um eine Konvertierung der Methoden Gruppe explizit auszuführen. Das Beispiel
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
Stattdessen können Sie schreiben.
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

Methoden Gruppen können die Überladungs Auflösung beeinflussen und am Typrückschluss teilnehmen. Weitere Informationen finden Sie unter [Funktionsmember](expressions.md#function-members) .

Die Lauf Zeit Auswertung einer Methoden Gruppen Konvertierung verläuft wie folgt:

*  Wenn die zur Kompilierzeit ausgewählte Methode eine Instanzmethode ist oder es sich um eine Erweiterungsmethode handelt, auf die als Instanzmethode zugegriffen wird, wird das Zielobjekt des Delegaten aus dem Instanzausdruck bestimmt, der `E`zugeordnet ist:
    * Der Instanzausdruck wird ausgewertet. Wenn diese Auswertung eine Ausnahme verursacht, werden keine weiteren Schritte ausgeführt.
    * Wenn der Instanzausdruck eine *reference_type*ist, wird der durch den Instanzausdruck berechnete Wert zum Zielobjekt. Wenn die ausgewählte Methode eine Instanzmethode ist und das Zielobjekt `null`ist, wird eine `System.NullReferenceException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.
    * Wenn der Instanzausdruck eine *value_type*ist, wird ein Boxing-Vorgang ([Boxing-Konvertierungen](types.md#boxing-conversions)) ausgeführt, um den Wert in ein Objekt zu konvertieren, und dieses Objekt wird zum Zielobjekt.
*  Andernfalls ist die ausgewählte Methode Teil eines statischen Methoden Aufrufes, und das Zielobjekt des Delegaten wird `null`.
*  Eine neue Instanz des Delegattyps `D` zugeordnet wird. Wenn nicht genügend Arbeitsspeicher zum Zuordnen der neuen Instanz verfügbar ist, wird ein `System.OutOfMemoryException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.
*  Die neue Delegatinstanz wird mit einem Verweis auf die Methode initialisiert, die zur Kompilierzeit bestimmt wurde, und ein Verweis auf das oben berechnete Zielobjekt.
