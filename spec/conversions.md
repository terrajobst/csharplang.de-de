---
ms.openlocfilehash: 61eeae6173eaa19f9cf6d6e985f3dc107d4c3ac9
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "50245525"
---
# <a name="conversions"></a>Konvertierungen

Ein ***Konvertierung*** ermöglicht einem Ausdruck als einen bestimmten Typ behandelt werden soll. Eine Konvertierung möglicherweise dazu führen, dass einen Ausdruck eines bestimmten Typs als einen anderen Typ behandelt werden soll, oder es kann dazu führen, dass einen Ausdruck ohne einen Typ um einen Typ zu erhalten. Konvertierungen möglich ***implizite*** oder ***explizite***, dadurch wird bestimmt, ob eine explizite Umwandlung erforderlich ist. Z. B. die Konvertierung von Typ `int` eingeben `long` ist implizit, also Ausdrücke vom Typ `int` kann implizit als Typ behandelt werden `long`. Die umgekehrte Konvertierung von Typ `long` eingeben `int`, explizite und damit eine explizite Umwandlung erforderlich ist.

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

Bei einigen Konvertierungen werden von der Sprache definiert. Programme können auch eigene Konvertierungen definieren ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)).

## <a name="implicit-conversions"></a>Implizite Konvertierungen

Die folgenden Konvertierungen werden als implizite Konvertierungen klassifiziert:

*  Identity-Konvertierungen
*  Implizite numerische Konvertierungen
*  Enumeration von impliziten Konvertierungen.
*  Implizite NULL-Werte zulassen Konvertierungen
*  NULL-literal-Konvertierungen
*  Ein impliziter verweiskonvertierungen
*  Boxing-Konvertierung
*  Implizite dynamische Konvertierungen
*  Implizite konstanter Ausdruck-Konvertierungen
*  Benutzerdefinierte implizite Konvertierungen
*  Anonyme Funktion Konvertierungen
*  Konvertierungen für Gruppe

Implizite Konvertierungen können auftreten, in einer Vielzahl von Situationen, einschließlich der Element-Funktionsaufrufe ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), Umwandlungsausdrücke ([Umwandlungsausdrücke](expressions.md#cast-expressions)), und Zuweisungen ([Zuweisungsoperatoren](expressions.md#assignment-operators)).

Die vordefinierten implizite Konvertierungen immer erfolgreich, und nie Ausnahmen verursachen. Richtig entworfene benutzerdefinierte implizite Konvertierungen sollte auch diese Merkmale aufweisen.

Im Rahmen der Konvertierung, die Typen `object` und `dynamic` als äquivalent betrachtet werden.

Jedoch dynamische Konvertierungen ([implizite dynamische Konvertierungen](conversions.md#implicit-dynamic-conversions) und [explizite dynamische Konvertierungen](conversions.md#explicit-dynamic-conversions)) gelten nur für Ausdrücke vom Typ `dynamic` ([der dynamische Typ](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>Identitätskonvertierung

Eine identitätskonvertierung von einem Typ die in denselben Typ konvertiert werden. Diese Konvertierung vorhanden ist, dass eine Entität, die bereits den erforderlichen Typ besitzt bezeichnet werden kann, um auf diesen Typ konvertiert werden können.

*  Da das Objekt und dynamischer als gleichwertig angesehen werden besteht eine für die identitätskonvertierung `object` und `dynamic`, und zwischen konstruierte Typen, die gleich sind, wenn Sie alle Vorkommen von ersetzen `dynamic` mit `object`.

### <a name="implicit-numeric-conversions"></a>Implizite numerische Konvertierungen

Die impliziten numerischen Konvertierungen sind:

*  Von `sbyte` zu `short`, `int`, `long`, `float`, `double`, oder `decimal`.
*  Von `byte` zu `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, oder `decimal`.
*  Von `short` zu `int`, `long`, `float`, `double`, oder `decimal`.
*  Von `ushort` zu `int`, `uint`, `long`, `ulong`, `float`, `double`, oder `decimal`.
*  Von `int` zu `long`, `float`, `double`, oder `decimal`.
*  Von `uint` zu `long`, `ulong`, `float`, `double`, oder `decimal`.
*  Von `long` zu `float`, `double`, oder `decimal`.
*  Von `ulong` zu `float`, `double`, oder `decimal`.
*  Von `char` zu `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, oder `decimal`.
*  Von `float` zu `double`.

Konvertierungen von `int`, `uint`, `long`, oder `ulong` zu `float` und `long` oder `ulong` zu `double` möglicherweise zu einem Genauigkeitsverlust führen, wird aber nie Ursache ein Verlust der Größe. Die anderen impliziten numerischen Konvertierungen verlieren keine Informationen.

Es gibt keine impliziten Konvertierungen für die `char` eingeben, damit die Werte der anderen ganzzahligen Typen nicht automatisch in konvertiert werden die `char` Typ.

### <a name="implicit-enumeration-conversions"></a>Enumeration von impliziten Konvertierungen

Eine Enumeration der impliziten Konvertierung lässt die *Decimal_integer_literal* `0` in eine konvertiert werden *Enum_type* sowie an ggf. *Nullable_type* , deren zugrunde liegende Typ ist ein *Enum_type*. Im letzteren Fall wird die Konvertierung ausgewertet, durch die Konvertierung in den zugrunde liegenden *Enum_type* und das Ergebnis ([auf NULL festlegbare Typen](types.md#nullable-types)).

### <a name="implicit-interpolated-string-conversions"></a>Implizite interpolierte zeichenfolgenkonvertierungen

Ein impliziter interpolierte Zeichenfolge Konvertierung lässt eine *Interpolated_string_expression* ([interpolierte Zeichenfolgen](expressions.md#interpolated-strings)) zu konvertierenden `System.IFormattable` oder `System.FormattableString` (implementiert `System.IFormattable`).

Wenn diese Konvertierung angewendet wird besteht ein String-Wert aus der interpolierten Zeichenfolge nicht. Stattdessen eine Instanz von `System.FormattableString` erstellt, in gemäß Erläuterung unten [interpolierte Zeichenfolgen](expressions.md#interpolated-strings).

### <a name="implicit-nullable-conversions"></a>Implizite NULL-Werte zulassen Konvertierungen

Vordefinierte implizite Konvertierungen, die nicht auf NULL festlegbare Werttypen verarbeiten können auch mit NULL-Werte zulassen Forms dieser Typen verwendet werden. Für jede vordefinierte implizite Identitäts- und numerische Konvertierungen, die von einem Typ NULL-Werte konvertieren `S` auf einen NULL-Werttyp `T`, existieren die folgenden Konvertierungen für das implizite NULL-Werte zulassen:

*  Eine implizite Konvertierung von `S?` zu `T?`.
*  Eine implizite Konvertierung von `S` zu `T?`.

Auswertung der eine implizite Konvertierung für die NULL-Werte zulassen basierend auf eine zugrunde liegende Konvertierung von `S` zu `T` wird wie folgt:

*  Wenn eine auf NULL festlegbare Konvertierung von `S?` zu `T?`:
    * Wenn der Quellwert null ist (`HasValue` Eigenschaft ist "false"), das Ergebnis ist der null-Wert des Typs `T?`.
    * Andernfalls wird die Konvertierung ausgewertet, als ein Entpacken von `S?` zu `S`, gefolgt von der zugrunde liegenden Konvertierung von `S` zu `T`, gefolgt von einem Wrapper ([auf NULL festlegbare Typen](types.md#nullable-types)) von `T` zu `T?`.

*  Wenn eine auf NULL festlegbare Konvertierung von `S` zu `T?`, die Konvertierung wird ausgewertet, als die zugrunde liegenden Konvertierung von `S` zu `T` gefolgt von einem Textumbruch aus `T` zu `T?`.

### <a name="null-literal-conversions"></a>NULL-literal-Konvertierungen

Eine implizite Konvertierung vorhanden ist, aus der `null` Zeichenfolgenliteral in einen beliebigen Typ NULL-Werte zulässt. Diese Konvertierung erzeugt, den null-Wert ([auf NULL festlegbare Typen](types.md#nullable-types)) des angegebenen Typs NULL-Werte zulässt.

### <a name="implicit-reference-conversions"></a>Ein impliziter verweiskonvertierungen

Die implizite verweiskonvertierungen sind:

*  Von jedem *Reference_type* zu `object` und `dynamic`.
*  Von jedem *Class_type* `S` auf *Class_type* `T`, bereitgestellt wird, `S` ergibt sich aus `T`.
*  Von jedem *Class_type* `S` auf *Interface_type* `T`aus `S` implementiert `T`.
*  Von jedem *Interface_type* `S` auf *Interface_type* `T`, bereitgestellt wird, `S` ergibt sich aus `T`.
*  Aus einer *Array_type* `S` mit einem Elementtyp `SE` auf eine *Array_type* `T` mit einem Elementtyp `TE`, sofern alle der folgenden Bedingungen erfüllt sind:
    * `S` und `T` unterscheiden sich nur hinsichtlich der Elementtyp. Das heißt, `S` und `T` haben die gleiche Anzahl von Dimensionen.
    * Beide `SE` und `TE` sind *Reference_type*s.
    * Implizite verweiskonvertierung vorhanden ist, von `SE` zu `TE`.
*  Von jedem *Array_type* zu `System.Array` und die Schnittstellen implementiert.
*  Aus einem eindimensionalen Array-Typ `S[]` zu `System.Collections.Generic.IList<T>` und die Basisschnittstellen, vorausgesetzt, es gibt eine implizite Identitäts- oder verweiskonvertierung aus `S` zu `T`.
*  Von jedem *Delegate_type* zu `System.Delegate` und die Schnittstellen implementiert.
*  Von der null-Literal in einem *Reference_type*.
*  Aus allen *Reference_type* auf eine *Reference_type* `T` verfügt eine implizite Identitäts- oder verweiskonvertierung, einer *Reference_type* `T0` und `T0` verfügt über eine identitätskonvertierung in `T`.
*  Von jedem *Reference_type* auf einen Typ von Schnittstellen oder Delegate `T` verfügt eine implizite Identitäts- oder verweiskonvertierung auf einen Typ von Schnittstellen oder Delegate `T0` und `T0` ist varianzkonvertierbar ([ Varianzkonvertierungen](interfaces.md#variance-conversion)) zu `T`.
*  Implizite Konvertierungen, die im Zusammenhang mit Typparametern, die bekannt ist, dass Verweistypen sein. Finden Sie unter [implizite Konvertierungen, die im Zusammenhang mit Typparametern](conversions.md#implicit-conversions-involving-type-parameters) ausführliche Informationen zum impliziten Konvertierungen, die im Zusammenhang mit Typparametern.

Die implizite verweiskonvertierungen sind Konvertierungen zwischen *Reference_type*s, die belegt werden kann, dass Sie immer erfolgreich sein und erfordern daher keine Überprüfungen zur Laufzeit.

Verweiskonvertierungen, implizite oder explizite, ändern sich nie die referenzielle Identität des Objekts, der konvertiert wird. Das heißt, während referenzkonvertierung den Typ des Verweises ändern kann, ändert es nie den Typ oder Wert des Objekts auf die verwiesen wird.

### <a name="boxing-conversions"></a>Boxing-Konvertierung

Eine Boxing-Konvertierung, ermöglicht eine *Value_type* implizit in einen Verweistyp konvertiert werden. Eine Boxingkonvertierung vorhanden ist, von jedem *Non_nullable_value_type* zu `object` und `dynamic`zu `System.ValueType` sowie an ggf. *Interface_type* implementiert die *Non_ Nullable_value_type*. Darüber hinaus eine *Enum_type* konvertiert werden kann, in den Typ `System.Enum`.

Eine Boxingkonvertierung vorhanden ist, aus einer *Nullable_type* ein Verweistyp, wenn ein Boxing-Konvertierung vorhanden ist aus der zugrunde liegenden *Non_nullable_value_type* in den Referenztyp.

Ein Werttyp ist, eine Boxing-Konvertierung in einen Schnittstellentyp `I` verfügt eine Boxing-Konvertierung in einen Schnittstellentyp `I0` und `I0` verfügt über eine identitätskonvertierung in `I`.

Ein Werttyp ist, eine Boxing-Konvertierung in einen Schnittstellentyp `I` verfügt eine Boxing-Konvertierung in einen Typ von Schnittstellen oder Delegate `I0` und `I0` ist varianzkonvertierbar ([varianzkonvertierungen](interfaces.md#variance-conversion)) zu `I`.

Boxing-Wert eine *Non_nullable_value_type* besteht aus eine Objektinstanz zuordnen und das Kopieren der *Value_type* Wert in dieser Instanz. Eine Struktur kann geschachtelt werden, in den Typ `System.ValueType`, da dies eine Basisklasse für alle Strukturen ist ([Vererbung](structs.md#inheritance)).

Boxing-Wert eine *Nullable_type* wird wie folgt:

*  Wenn der Quellwert null ist (`HasValue` Eigenschaft ist "false"), das Ergebnis ist ein null-Verweis des Zieltyps.
*  Das Ergebnis ist, andernfalls ein Verweis auf eine geschachtelte `T` von entpacken und den Wert des boxing erzeugt werden.

Boxing-Konvertierung werden ausführlich in [Boxingkonvertierungen](types.md#boxing-conversions).

### <a name="implicit-dynamic-conversions"></a>Implizite dynamische Konvertierungen

Eine implizite Konvertierung für die dynamische vorhanden ist, von einem Ausdruck vom Typ `dynamic` auf einen beliebigen Typ `T`. Die Konvertierung dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)), was bedeutet, dass eine implizite Konvertierung zur Laufzeit von der Runtime-Typ des Ausdrucks, der gesucht werden soll, wird `T`. Wenn keine Konvertierung gefunden wird, wird eine Laufzeitausnahme ausgelöst.

Beachten Sie, dass diese implizite Konvertierung den Rat am Anfang des scheinbar verstößt gegen [implizite Konvertierungen](conversions.md#implicit-conversions) , dass eine implizite Konvertierung niemals eine Ausnahme auslösen soll. Ist es jedoch nicht die Konvertierung selbst, aber die *suchen* der Konvertierung, die die Ausnahme ausgelöst hat. Bei der Verwendung der dynamischen Bindung ist das Risiko von Laufzeitausnahmen. Wenn die dynamische Bindung für die Konvertierung nicht gewünscht ist, der Ausdruck konvertiert werden kann zunächst `object`, und klicken Sie dann den gewünschten Typ.

Das folgende Beispiel veranschaulicht die implizite dynamische Konvertierungen:

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

Die Zuweisungen zu `s2` und `i` sowohl der implizite dynamische Konvertierungen, die Bindung der Vorgänge, bis zur Laufzeit angehalten wird verwenden. Zur Laufzeit, implizite Konvertierungen aus der Run-Time-Typ, der gesucht werden `d`  --  `string` – in den Zieltyp. Eine Konvertierung wird festgestellt, dass `string` aber nicht für `int`.

### <a name="implicit-constant-expression-conversions"></a>Implizite konstanter Ausdruck-Konvertierungen

Eine Konvertierung implizit Konstantenausdruck ermöglicht die folgenden Konvertierungen:

*  Ein *Constant_expression* ([Konstante Ausdrücke](expressions.md#constant-expressions)) vom Typ `int` Typ konvertiert werden kann `sbyte`, `byte`, `short`, `ushort`, `uint`, oder `ulong`, vorausgesetzt, der Wert, der die *Constant_expression* liegt innerhalb des Bereichs des Zieltyps.
*  Ein *Constant_expression* des Typs `long` Typ konvertiert werden kann `ulong`, vorausgesetzt, der Wert, der die *Constant_expression* nicht negativ ist.

### <a name="implicit-conversions-involving-type-parameters"></a>Implizite Konvertierungen, die im Zusammenhang mit Typparametern

Die folgenden implizite Konvertierungen für einen Parameter angegebenen Typ vorhanden `T`:

*  Von `T` zu deren effektiven Basisklasse `C`, von `T` auf einer Basisklasse von `C`, und von `T` für jede Schnittstelle implementiert, indem `C`. AT-Laufzeit-If `T` ist ein Werttyp, einer Boxing-Konvertierung die Konvertierung ausgeführt wird. Andernfalls wird die Konvertierung als implizite verweiskonvertierung oder identitätskonvertierung ausgeführt.
*  Aus `T` in einen Schnittstellentyp `I` in `T`effektive Schnittstelle des festgelegt werden und von `T` auf alle Basisschnittstelle `I`. AT-Laufzeit-If `T` ist ein Werttyp, einer Boxing-Konvertierung die Konvertierung ausgeführt wird. Andernfalls wird die Konvertierung als implizite verweiskonvertierung oder identitätskonvertierung ausgeführt.
*  Von `T` auf einen Typparameter `U`aus `T` hängt `U` ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)). AT-Laufzeit-If `U` ein Werttyp ist, klicken Sie dann `T` und `U` unbedingt den gleichen Typ aufweisen und wird keine Konvertierung durchgeführt. Andernfalls gilt: Wenn `T` ist ein Werttyp, einer Boxing-Konvertierung die Konvertierung ausgeführt wird. Andernfalls wird die Konvertierung als implizite verweiskonvertierung oder identitätskonvertierung ausgeführt.
*  Von der null-Literal in `T`aus `T` ist ein Verweistyp ist bekannt.
*  Von `T` in einen Verweistyp `I` , wenn es sich um eine implizite Konvertierung in einen Verweistyp hat `S0` und `S0` verfügt über eine identitätskonvertierung in `S`. Zur Laufzeit wird die Konvertierung ausgeführt, die gleiche Weise wie die Konvertierung in `S0`.
*  Von `T` in einen Schnittstellentyp `I` verfügt eine implizite Konvertierung in einen Typ von Schnittstellen oder Delegate `I0` und `I0` ist varianzkonvertierbar zu `I` ([varianzkonvertierungen](interfaces.md#variance-conversion) ). AT-Laufzeit-If `T` ist ein Werttyp, einer Boxing-Konvertierung die Konvertierung ausgeführt wird. Andernfalls wird die Konvertierung als implizite verweiskonvertierung oder identitätskonvertierung ausgeführt.

Wenn `T` bekannt ist ein Verweistyp sein ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)), die oben genannten Konvertierungen werden als implizite Verweis-klassifiziert ([implizite Verweis-](conversions.md#implicit-reference-conversions)). Wenn `T` ist nicht bekannt ist, ein Verweistyp sein muss, sind die Konvertierungen, die höher als Boxingkonvertierungen klassifiziert ([Boxingkonvertierungen](conversions.md#boxing-conversions)).

### <a name="user-defined-implicit-conversions"></a>Benutzerdefinierte implizite Konvertierungen

Eine implizite Konvertierung von benutzerdefinierten umfasst eine optionale standard implizite Konvertierung, gefolgt von der Ausführung einer benutzerdefinierten implizite Konvertierungsoperator, gefolgt von einem anderen optionalen standard implizite Konvertierung. Die genauen Regeln für Ihre Evaluierung von benutzerdefinierten implizite Konvertierungen werden in beschrieben [von impliziten Konvertierungen eine benutzerdefinierte Verarbeitung](conversions.md#processing-of-user-defined-implicit-conversions).

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>Anonyme Funktion Konvertierungen und Konvertierungen für Gruppe

Anonyme Funktionen und Methodengruppen keine Typen an und für sich allerdings um Typen oder ausdrucksbaumstrukturtypen delegieren implizit konvertiert werden können. Anonyme Funktion Konvertierungen werden ausführlicher beschrieben [anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions) und Gruppe Konvertierungen in [Gruppe Konvertierungen](conversions.md#method-group-conversions).

## <a name="explicit-conversions"></a>Explizite Konvertierungen

Die folgenden Konvertierungen werden explizite Konvertierungen klassifiziert:

*  Alle impliziten Konvertierungen.
*  Explizite numerische Konvertierungen.
*  Konvertierungen von expliziten-Enumeration.
*  Explizite Konvertierungen für NULL-Werte zulässt.
*  Explizite Konvertierungen.
*  Explizite Konvertierungen.
*  Unboxing-Konvertierungen.
*  Explizite dynamische Konvertierungen
*  Benutzerdefinierte explizite Konvertierungen.

Explizite Konvertierungen können in der Cast-Ausdrücke auftreten ([Umwandlungsausdrücke](expressions.md#cast-expressions)).

Der explizite Konvertierungen enthält alle impliziter Konvertierungen. Dies bedeutet, dass redundante Cast-Ausdrücke zulässig sind.

Die explizite Konvertierungen, die keine impliziten Konvertierungen sind sind Konvertierungen, die belegt werden können, dass immer erfolgreich, Konvertierungen, die bekannt ist, dass Sie möglicherweise Informationen verloren gehen und Konvertierungen zwischen Domänen unterscheiden, auch explizite Typen Notation.

### <a name="explicit-numeric-conversions"></a>Explizite numerische Konvertierungen

Die expliziten numerischen Konvertierungen sind Konvertierungen aus einer *Numeric_type* in ein anderes *Numeric_type* für die eine implizite numerische Konvertierung ([implizite numerische Konvertierungen](conversions.md#implicit-numeric-conversions)) ist noch nicht vorhanden:

*  Von `sbyte` zu `byte`, `ushort`, `uint`, `ulong`, oder `char`.
*  Von `byte` zu `sbyte` und `char`.
*  Von `short` zu `sbyte`, `byte`, `ushort`, `uint`, `ulong`, oder `char`.
*  Von `ushort` zu `sbyte`, `byte`, `short`, oder `char`.
*  Von `int` zu `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, oder `char`.
*  Von `uint` zu `sbyte`, `byte`, `short`, `ushort`, `int`, oder `char`.
*  Von `long` zu `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, oder `char`.
*  Von `ulong` zu `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, oder `char`.
*  Von `char` zu `sbyte`, `byte`, oder `short`.
*  Von `float` zu `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, oder `decimal`.
*  Von `double` zu `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, oder `decimal`.
*  Von `decimal` zu `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, oder `double`.

Da die explizite Konvertierungen alle implizite und explizite numerische Konvertierungen enthalten, ist es immer möglich, die aus einem konvertieren *Numeric_type* auch einer beliebigen anderen *Numeric_type* mithilfe eines Cast-Ausdruck ( [Umwandlungsausdrücke](expressions.md#cast-expressions)).

Die expliziten numerischen Konvertierungen Informationen möglicherweise verloren gehen oder möglicherweise dazu führen, dass Ausnahmen ausgelöst werden. Eine explizite numerische Konvertierung wird wie folgt verarbeitet:

*  Für eine Konvertierung in einen ganzzahligen Typ in einen anderen ganzzahligen Typ hängt von die Verarbeitung Kontext der überlaufprüfung ([checked und unchecked Operatoren](expressions.md#the-checked-and-unchecked-operators)) in der die Konvertierung akzeptiert platzieren:
    * In einer `checked` Kontext ist die Konvertierung erfolgreich ist, wenn der Wert des Quelloperanden innerhalb des Bereichs des Zieltyps liegt, aber löst einen `System.OverflowException` , wenn der Wert des Quelloperanden liegt außerhalb des Bereichs des Zieltyps ist.
    * In einer `unchecked` Kontext ist die Konvertierung immer erfolgreich ist, und wird wie folgt.
        * Wenn der Quelltyp größer als der Zieltyp ist, wird der Quellwert abgeschnitten, indem die wichtigsten „zusätzlichen“ Teile verworfen werden. Das Ergebnis wird dann als Wert des Zieltyps behandelt.
        * Wenn der Quelltyp kleiner als der Zieltyp ist, wird der Quellwert entweder mit Vorzeichen oder Nullen (0) erweitert, sodass er die gleiche Größe wie der Zieltyp aufweist. Die Vorzeichenerweiterung wird verwendet, wenn der Quelltyp mit einem Vorzeichen versehen ist. Die Erweiterung mit Nullen (0) wird verwendet, wenn der Quelltyp mit keinem Vorzeichen versehen ist. Das Ergebnis wird dann als Wert des Zieltyps behandelt.
        * Wenn der Quelltyp die gleiche Größe wie der Zieltyp aufweist, wird der Quellwert als Wert vom Zieltyp behandelt.
*  Für eine Konvertierung von `decimal` in einen ganzzahligen Typ, wird der Quellwert in Richtung 0 (null), um den nächsten ganzzahligen Wert gerundet, und dieser ganzzahlige Wert wird das Ergebnis der Konvertierung. Wenn der erzeugte Integralwert sich außerhalb des Bereichs des Zieltyps, ist eine `System.OverflowException` ausgelöst.
*  Für eine Konvertierung von `float` oder `double` in einen ganzzahligen Typ, hängt die Verarbeitung von Kontext der überlaufprüfung ([checked und unchecked Operatoren](expressions.md#the-checked-and-unchecked-operators)) in der die Konvertierung akzeptiert platzieren:
    * In einem `checked` Kontext ist die Konvertierung wird wie folgt:
        * Wenn der Wert des Operanden NaN oder unendlich sein, eine `System.OverflowException` ausgelöst.
        * Andernfalls wird Quelloperanden gegen 0 (null), um den nächsten ganzzahligen Wert gerundet. Wenn dieser ganzzahlige Wert innerhalb des Bereichs des Zieltyps ist, ist dieser Wert das Ergebnis der Konvertierung.
        * Andernfalls wird eine `System.OverflowException` ausgelöst.
    * In einer `unchecked` Kontext ist die Konvertierung immer erfolgreich ist, und wird wie folgt.
        * Wenn der Wert des Operanden NaN oder unendlich ist, ist das Ergebnis der Konvertierung einen nicht angegebenen Wert des Zieltyps.
        * Andernfalls wird Quelloperanden gegen 0 (null), um den nächsten ganzzahligen Wert gerundet. Wenn dieser ganzzahlige Wert innerhalb des Bereichs des Zieltyps ist, ist dieser Wert das Ergebnis der Konvertierung.
        * Andernfalls ist das Ergebnis der Konvertierung einen nicht angegebenen Wert des Zieltyps.
*  Für eine Konvertierung von `double` zu `float`, `double` Wert wird gerundet, um die nächste `float` Wert. Wenn die `double` Wert ist zu klein, um die Darstellung als eine `float`, das Ergebnis positiv oder negativ 0 (null). Wenn die `double` Wert ist zu groß, um die Darstellung als eine `float`, wird das Ergebnis, positive oder negative Unendlichkeit. Wenn die `double` Wert ist NaN, es ist auch das Ergebnis NaN.
*  Für eine Konvertierung von `float` oder `double` zu `decimal`, wird der Quellwert in konvertiert `decimal` Darstellung und bei Bedarf auf die nächste Zahl nach der 28. Dezimalstelle gerundet ([Dezimaltyps](types.md#the-decimal-type)). Wenn der Quellwert zu klein, um die Darstellung als ist eine `decimal`, wird das Ergebnis 0 (null). Wenn der Quellwert NaN ist, ist unendlich oder zu groß, um die Darstellung als eine `decimal`, eine `System.OverflowException` ausgelöst.
*  Für eine Konvertierung von `decimal` zu `float` oder `double`, `decimal` Wert wird gerundet, um die nächste `double` oder `float` Wert. Während dieser Konvertierung an Genauigkeit verlieren kann, wird nie eine Ausnahme ausgelöst werden.

### <a name="explicit-enumeration-conversions"></a>Enumeration der explizite Konvertierungen

Die Enumeration der explizite Konvertierungen sind:

*  Von `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, oder `decimal` auf alle *Enum_type*.
*  Von jedem *Enum_type* zu `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, oder `decimal`.
*  Von jedem *Enum_type* auch einer beliebigen anderen *Enum_type*.

Eine Enumeration der expliziten Konvertierung zwischen zwei Typen von behandeln alle Teil verarbeitet wird *Enum_type* als den zugrunde liegenden Typ, der *Enum_type*, und klicken Sie dann ausführen, die eine implizite oder explizite numerische Konvertierung zwischen den resultierenden Typen. Angenommen, ein *Enum_type* `E` mit und die zugrunde liegende Standardtyp von `int`, eine Konvertierung von `E` zu `byte` als eine explizite numerische Konvertierung verarbeitet ([Explicit numerische Konvertierungen](conversions.md#explicit-numeric-conversions)) von `int` zu `byte`, und eine Konvertierung von `byte` zu `E` wird als implizite numerische Konvertierung verarbeitet ([implizite numerische Konvertierungen](conversions.md#implicit-numeric-conversions)) von `byte` zu `int`.

### <a name="explicit-nullable-conversions"></a>Explizite NULL-Werte zulassen Konvertierungen

***Explizite NULL-Werte zulassen Konvertierungen*** zulassen vordefinierte explizite Konvertierungen, die Vorgänge an nicht auf NULL festlegbare Werttypen auch mit NULL-Werte zulassen Forms dieser Typen verwendet werden soll. Für jedes der vordefinierten explizite Konvertierungen, die von einem Typ NULL-Werte konvertieren `S` auf einen NULL-Werttyp `T` ([identitätskonvertierung](conversions.md#identity-conversion), [implizite numerische Konvertierungen](conversions.md#implicit-numeric-conversions), [Enumeration von impliziten Konvertierungen](conversions.md#implicit-enumeration-conversions), [explizite numerische Konvertierungen](conversions.md#explicit-numeric-conversions), und [explizite Enumeration Konvertierungen](conversions.md#explicit-enumeration-conversions)), die folgenden auf NULL festlegbare Konvertierungen sind vorhanden:

*  Eine explizite Konvertierung vom `S?` zu `T?`.
*  Eine explizite Konvertierung vom `S` zu `T?`.
*  Eine explizite Konvertierung vom `S?` zu `T`.

Auswertung von eine auf NULL festlegbare Konvertierung basierend auf eine zugrunde liegende Konvertierung von `S` zu `T` wird wie folgt:

*  Wenn eine auf NULL festlegbare Konvertierung von `S?` zu `T?`:
    * Wenn der Quellwert null ist (`HasValue` Eigenschaft ist "false"), das Ergebnis ist der null-Wert des Typs `T?`.
    * Andernfalls wird die Konvertierung ausgewertet, als ein Entpacken von `S?` zu `S`, gefolgt von der zugrunde liegenden Konvertierung von `S` zu `T`, gefolgt von einer Wrapping von `T` zu `T?`.
*  Wenn eine auf NULL festlegbare Konvertierung von `S` zu `T?`, die Konvertierung wird ausgewertet, als die zugrunde liegenden Konvertierung von `S` zu `T` gefolgt von einem Textumbruch aus `T` zu `T?`.
*  Wenn eine auf NULL festlegbare Konvertierung von `S?` zu `T`, die Konvertierung wird als ein Entpacken von ausgewertet `S?` zu `S` gefolgt von der zugrunde liegenden Konvertierung von `S` zu `T`.

Beachten Sie, dass der Versuch, einen NULL-Wert zu entpacken eine Ausnahme auslöst, wenn der Wert ist `null`.

### <a name="explicit-reference-conversions"></a>Explizite Konvertierungen

Die explizite Konvertierungen sind:

*  Von `object` und `dynamic` auch einer beliebigen anderen *Reference_type*.
*  Von jedem *Class_type* `S` auf *Class_type* `T`, bereitgestellt wird, `S` ist eine Basisklasse von `T`.
*  Von jedem *Class_type* `S` auf *Interface_type* `T`, bereitgestellt wird, `S` ist nicht versiegelt ist und `S` implementiert nicht `T`.
*  Von jedem *Interface_type* `S` auf *Class_type* `T`aus `T` ist nicht versiegelt oder bereitgestellt `T` implementiert `S`.
*  Von jedem *Interface_type* `S` auf *Interface_type* `T`aus `S` stammt nicht aus `T`.
*  Aus einer *Array_type* `S` mit einem Elementtyp `SE` auf eine *Array_type* `T` mit einem Elementtyp `TE`, sofern alle der folgenden Bedingungen erfüllt sind:
    * `S` und `T` unterscheiden sich nur hinsichtlich der Elementtyp. Das heißt, `S` und `T` haben die gleiche Anzahl von Dimensionen.
    * Beide `SE` und `TE` sind *Reference_type*s.
    * Eine explizite Konvertierung vorhanden ist, von `SE` zu `TE`.
*  Von `System.Array` und die Schnittstellen implementiert, alle *Array_type*.
*  Aus einem eindimensionalen Array-Typ `S[]` zu `System.Collections.Generic.IList<T>` und die Basisschnittstellen, vorausgesetzt, es gibt eine explizite Konvertierung von `S` zu `T`.
*  Von `System.Collections.Generic.IList<S>` und Basis Schnittstellen verwenden, um einen eindimensionalen Arraytyp `T[]`, vorausgesetzt, es gibt eine explizite Identitäts- oder verweiskonvertierung aus `S` zu `T`.
*  Von `System.Delegate` und die Schnittstellen implementiert, alle *Delegate_type*.
*  Von einem Referenztyp in einen Verweistyp `T` , wenn sie eine explizite Konvertierung in einen Verweistyp hat `T0` und `T0` verfügt über eine identitätskonvertierung `T`.
*  Von einem Referenztyp zu einem Typ von Schnittstellen oder Delegate `T` verfügt eine explizite Konvertierung in einen Typ von Schnittstellen oder Delegate `T0` und entweder `T0` ist varianzkonvertierbar zu `T` oder `T` ist Um varianzkonvertierbar `T0` ([varianzkonvertierungen](interfaces.md#variance-conversion)).
*  Aus `D<S1...Sn>` zu `D<T1...Tn>` , in denen `D<X1...Xn>` ist ein generischer Delegattyp `D<S1...Sn>` ist nicht kompatibel mit oder identisch mit `D<T1...Tn>`, und für jeden Typparameter `Xi` von `D` in der folgenden enthalten:
    * Wenn `Xi` nicht Variant, klicken Sie dann `Si` ist identisch mit `Ti`.
    * Wenn `Xi` kovariant ist, dann gibt es eine implizite oder explizite Identitäts- oder -Konvertierung von `Si` zu `Ti`.
    * Wenn `Xi` ist kontravariant `Si` und `Ti` sind identisch oder beide Verweistypen.
*  Explizite Konvertierungen, die im Zusammenhang mit Typparametern, die bekannt ist, dass Verweistypen sein. Weitere Informationen zu expliziten Konvertierungen, die im Zusammenhang mit Typparametern finden Sie unter [explizite Konvertierungen, die im Zusammenhang mit Typparametern](conversions.md#explicit-conversions-involving-type-parameters).

Die explizite Konvertierungen sind Konvertierungen zwischen Verweistypen, die erfordern, stellen Sie sicher, dass sie korrekt sind Überprüfungen zur Laufzeit.

Für eine explizite Konvertierung zur Laufzeit erfolgreich ist, muss der Wert des Quelloperanden `null`, oder der tatsächliche Typ des Objekts, auf die Quelloperanden verweist, muss ein Typ, der durch einen impliziten Verweis in den Zieltyp konvertiert werden kann Konvertierung ([implizite Verweis-](conversions.md#implicit-reference-conversions)) oder Boxing-Konvertierung ([Boxingkonvertierungen](conversions.md#boxing-conversions)). Wenn eine explizite Konvertierung ein Fehler auftritt, eine `System.InvalidCastException` ausgelöst.

Verweiskonvertierungen, implizite oder explizite, ändern sich nie die referenzielle Identität des Objekts, der konvertiert wird. Das heißt, während referenzkonvertierung den Typ des Verweises ändern kann, ändert es nie den Typ oder Wert des Objekts auf die verwiesen wird.

### <a name="unboxing-conversions"></a>Unboxing-Konvertierungen

Eine unboxing-Konvertierung ermöglicht einen Verweistyp explizit zu konvertierenden eine *Value_type*. Eine unboxing-Konvertierung vorhanden ist, von den Typen `object`, `dynamic` und `System.ValueType` auf *Non_nullable_value_type*, und von jedem *Interface_type* auf *Non_ Nullable_value_type* , implementiert die *Interface_type*. Geben Sie außerdem `System.Enum` kann nicht auf eine geschachtelte sein *Enum_type*.

Eine unboxing-Konvertierung vorhanden ist, von einem Referenztyp zu einem *Nullable_type* Falls von den Verweistyp eine unboxing-Konvertierung vorhanden, auf die zugrunde liegende ist *Non_nullable_value_type* von der  *Nullable_type*.

Ein Werttyp `S` eine unboxing-Konvertierung eines Schnittstellentyps hat `I` , wenn sie eine unboxing-Konvertierung eines Schnittstellentyps hat `I0` und `I0` verfügt über eine identitätskonvertierung in `I`.

Ein Werttyp `S` eine unboxing-Konvertierung eines Schnittstellentyps hat `I` verfügt eine unboxing-Konvertierung von einem Typ von Schnittstellen oder Delegate `I0` und entweder `I0` ist varianzkonvertierbar zu `I` oder `I`ist varianzkonvertierbar zu `I0` ([varianzkonvertierungen](interfaces.md#variance-conversion)).

Zunächst geprüft wird, die einen geschachtelten Wert eines der Objekttyp ist ein unboxing-Vorgang besteht aus den angegebenen *Value_type*, und klicken Sie dann kopieren den Wert aus der Instanz. Unboxing einen null-Verweis auf eine *Nullable_type* erzeugt den null-Wert von der *Nullable_type*. Eine Struktur kann nicht vom Typ geschachtelte sein `System.ValueType`, da dies eine Basisklasse für alle Strukturen ist ([Vererbung](structs.md#inheritance)).

Unboxing-Konvertierungen werden ausführlich in [Unboxing-Konvertierungen](types.md#unboxing-conversions).

### <a name="explicit-dynamic-conversions"></a>Explizite dynamische Konvertierungen

Eine explizite Konvertierung für die dynamische vorhanden ist, von einem Ausdruck vom Typ `dynamic` auf einen beliebigen Typ `T`. Die Konvertierung dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)), was bedeutet, dass eine explizite Konvertierung zur Laufzeit von der Runtime-Typ des Ausdrucks, der gesucht werden soll, wird `T`. Wenn keine Konvertierung gefunden wird, wird eine Laufzeitausnahme ausgelöst.

Wenn die dynamische Bindung für die Konvertierung nicht gewünscht ist, der Ausdruck konvertiert werden kann zunächst `object`, und klicken Sie dann den gewünschten Typ.

Angenommen Sie, die folgende Klasse definiert ist:
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

Das folgende Beispiel veranschaulicht die explizite dynamische Konvertierungen:
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

Die beste Konvertierung `o` zu `C` befindet sich zum Zeitpunkt der Kompilierung einer expliziten verweiskonvertierung sein. Zur Laufzeit, nicht, da `"1"` ist nicht in der Tat ein `C`. Die Konvertierung von `d` zu `C` jedoch als eine explizite Konvertierung dynamische wird angehalten, Laufzeit, in denen ein Konvertierung von der Laufzeit-Typinformationen von benutzerdefinierter `d`  --  `string` – zu `C` gefunden wird, und was nicht.

### <a name="explicit-conversions-involving-type-parameters"></a>Explizite Konvertierungen, die im Zusammenhang mit Typparametern

Die folgenden expliziten Konvertierungen für einen Parameter angegebenen Typ vorhanden `T`:

*  Der effektive Basisklasse `C` von `T` zu `T` und von einer Basisklasse von `C` zu `T`. AT-Laufzeit-If `T` ein Werttyp ist, als einer unboxing-Konvertierung die Konvertierung ausgeführt wird. Andernfalls wird die Konvertierung als explizite Konvertierung oder identitätskonvertierung ausgeführt.
*  Über einen beliebigen anderen Schnittstellentyp, `T`. AT-Laufzeit-If `T` ein Werttyp ist, als einer unboxing-Konvertierung die Konvertierung ausgeführt wird. Andernfalls wird die Konvertierung als explizite Konvertierung oder identitätskonvertierung ausgeführt.
*  Von `T` auf *Interface_type* `I` vorausgesetzt, es ist nicht bereits eine implizite Konvertierung von `T` zu `I`. AT-Laufzeit-If `T` ein Werttyp handelt, wird die Konvertierung wird als ein Boxing-Konvertierung, gefolgt von einer expliziten verweiskonvertierung ausgeführt. Andernfalls wird die Konvertierung als explizite Konvertierung oder identitätskonvertierung ausgeführt.
*  Von einem Typparameter `U` zu `T`aus `T` hängt `U` ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)). AT-Laufzeit-If `U` ein Werttyp ist, klicken Sie dann `T` und `U` unbedingt den gleichen Typ aufweisen und wird keine Konvertierung durchgeführt. Andernfalls gilt: Wenn `T` ein Werttyp ist, als einer unboxing-Konvertierung die Konvertierung ausgeführt wird. Andernfalls wird die Konvertierung als explizite Konvertierung oder identitätskonvertierung ausgeführt.

Wenn `T` ist bekannt, die ein Verweistyp sein muss, die oben genannten Konvertierungen sind alle klassifiziert als explizite Konvertierungen ([explizite Konvertierungen](conversions.md#explicit-reference-conversions)). Wenn `T` ist nicht bekannt ist, ein Verweistyp sein muss, sind die Konvertierungen, die höher als unboxing-Konvertierungen klassifiziert ([Unboxing-Konvertierungen](conversions.md#unboxing-conversions)).

Die oben genannten Regeln lassen keine direkte explizite Konvertierung von einer nicht eingeschränkten Typparameter auf einen Typ Nichtschnittstellen-überraschend sein kann. Der Grund für diese Regel ist Verwechslungen und die Semantik der solche Konvertierungen deaktivieren. Betrachten Sie beispielsweise die folgende Deklaration:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

Wenn die direkte explizite Konvertierung `t` zu `int` erteilt wurden, wird einfach erwartet, `X<int>.F(7)` zurück `7L`. Es wäre jedoch nicht der Fall, da die standardmäßigen numerischen Konvertierungen nur gelten, wenn die Typen bekannt ist, dass zum Zeitpunkt der Bindung numerisch sein. Um die Semantik machen muss klare und im obige Beispiel stattdessen geschrieben werden:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

Dieser Code kompiliert aber ausgeführten `X<int>.F(7)` würde dann eine Ausnahme zur Laufzeit, da eine geschachtelte `int` kann nicht direkt konvertiert werden eine `long`.

### <a name="user-defined-explicit-conversions"></a>Benutzerdefinierte explizite Konvertierungen

Eine explizite Konvertierung von benutzerdefinierten umfasst eine optionale standard explizite Konvertierung, gefolgt von der Ausführung eines benutzerdefinierten impliziten oder expliziten Konvertierungsoperators, gefolgt von einem anderen optionalen standard explizite Konvertierung. Die genauen Regeln für Ihre Evaluierung von benutzerdefinierten explizite Konvertierungen werden in beschrieben [expliziter Konvertierungen eine benutzerdefinierte Verarbeitung](conversions.md#processing-of-user-defined-explicit-conversions).

## <a name="standard-conversions"></a>Standardkonvertierungen

Die standard-Konvertierungen sind Konvertierungen vorab definierten, die als Teil einer benutzerdefinierten Konvertierung auftreten können.

### <a name="standard-implicit-conversions"></a>Standard implizite Konvertierungen

Die folgenden implizite Konvertierungen werden als implizite standardkonvertierungen klassifiziert:

*  Identity-Konvertierungen ([identitätskonvertierung](conversions.md#identity-conversion))
*  Implizite numerische Konvertierungen ([implizite numerische Konvertierungen](conversions.md#implicit-numeric-conversions))
*  Konvertierungen für implizite NULL-Werte zulässt ([implizite NULL-Werte zulassen Konvertierungen](conversions.md#implicit-nullable-conversions))
*  Implizite verweiskonvertierungen ([implizite Verweis-](conversions.md#implicit-reference-conversions))
*  Boxing-Konvertierung ([Boxingkonvertierungen](conversions.md#boxing-conversions))
*  Implizite konstanter Ausdruck-Konvertierungen ([implizite dynamische Konvertierungen](conversions.md#implicit-dynamic-conversions))
*  Implizite Konvertierungen, die im Zusammenhang mit Typparametern ([implizite Konvertierungen, die im Zusammenhang mit Typparametern](conversions.md#implicit-conversions-involving-type-parameters))

Die implizite standardkonvertierungen schließen insbesondere benutzerdefinierte implizite Konvertierungen aus.

### <a name="standard-explicit-conversions"></a>Standard explizite Konvertierungen

Die standardmäßige explizite Konvertierungen sind alle standardmäßigen implizite Konvertierungen sowie die Teilmenge der explizite Konvertierungen für die eine umgekehrte standard implizite Konvertierung vorhanden ist. Das heißt, wenn ein Standard, die implizite Konvertierung vorhanden ist aus einem Typ `A` auf einen Typ `B`, und klicken Sie dann eine standard explizite Konvertierung von Typ vorhanden ist `A` eingeben `B` und vom Typ `B` eingeben `A`.

## <a name="user-defined-conversions"></a>Benutzerdefinierte Konvertierungen

In c# können Sie die vordefinierten impliziten und expliziten Konvertierungen von ergänzt werden ***benutzerdefinierte Konvertierungen***. Benutzerdefinierte Konvertierungen werden eingeführt, durch das Deklarieren von Konvertierungsoperatoren ([Konvertierungsoperatoren](classes.md#conversion-operators)) in der Klasse und Strukturtypen.

### <a name="permitted-user-defined-conversions"></a>Benutzerdefinierte Konvertierungen zulässig

C# lässt nur bestimmte benutzerdefinierte Konvertierungen deklariert werden. Insbesondere ist es nicht möglich, eine bereits vorhandene implizite oder explizite Konvertierung neu definieren.

Für einen bestimmten Quell- `S` und einen Zieltyp `T`, wenn `S` oder `T` werden auf NULL festlegbare Typen können `S0` und `T0` finden Sie in die zugrunde liegende Typen, andernfalls `S0` und `T0` sind gleich `S` und `T` bzw. Eine Klasse oder Struktur ist zulässig, deklarieren Sie eine Konvertierung aus einer Datenquelle `S` in einen Zieltyp `T` nur dann, wenn alle der folgenden Bedingungen erfüllt sind:

*  `S0` und `T0` gibt verschiedene Typen.
*  Entweder `S0` oder `T0` ist der Typ Klasse oder Struktur, die in der die Operatordeklaration erfolgt.
*  Weder `S0` noch `T0` ist ein *Interface_type*.
*  Mit Ausnahme von benutzerdefinierten Konvertierungen, eine Konvertierung ist nicht vom `S` zu `T` oder `T` zu `S`.

Die Einschränkungen für benutzerdefinierte Konvertierungen werden erläutert. im weiteren [Konvertierungsoperatoren](classes.md#conversion-operators).

### <a name="lifted-conversion-operators"></a>Transformierten Konvertierungsoperatoren

Erhalten einen benutzerdefinierten Konvertierungsoperator, der von einem Typ NULL-Werte konvertiert `S` auf einen NULL-Werttyp `T`, ***angehoben Konvertierungsoperator*** vorhanden ist, konvertiert in `S?` zu `T?`. Dieser transformierten Konvertierungsoperator führt eine Entpacken von `S?` zu `S` gefolgt von der eine benutzerdefinierte Konvertierung von `S` zu `T` gefolgt von einer Wrapping von `T` zu `T?`, außer dass ein NULL-Wert Valued `S?` wandelt den Wert Null direkt `T?`.

Ein Konvertierungsoperator für die transformierten hat die gleiche implizite oder explizite-Klassifizierung als die zugrunde liegenden benutzerdefinierten Konvertierungsoperator. Der Begriff, die die Verwendung von "benutzerdefinierte Konvertierung" gilt eine benutzerdefinierte und Konvertierungsoperatoren aufgehoben.

### <a name="evaluation-of-user-defined-conversions"></a>Auswertung von benutzerdefinierten Konvertierungen

Eine benutzerdefinierte Konvertierung konvertiert einen Wert als der Typ, dem Namen der ***Quelltyp***, einen anderen Typ, mit dem Namen der ***Zieltyp***. Auswertung von einer benutzerdefinierten Konvertierung konzentriert sich auf Suchen der ***spezifischste*** benutzerdefinierten Konvertierungsoperator für die bestimmten Typen von Quelle und Ziel. Diese Ermittlung ist in mehrere Schritte unterteilt:

*  Suchen den Satz von Klassen und Strukturen, die von denen benutzerdefinierten Konvertierungsoperatoren betrachtet werden. Dieser Satz besteht aus den Quelltyp und ihre Basisklassen und den Zieltyp und ihre Basisklassen (mit implizit vorausgesetzt, dass benutzerdefinierte Operatoren nur Klassen und Strukturen deklarieren und nichtklassentypen keine Basisklassen aufweisen). Im Rahmen dieser Schritt, wenn entweder die Quell-oder Zieltyp ist eine *Nullable_type*, deren zugrunde liegender Typ wird stattdessen verwendet.
*  Aus diesem Satz von Typen bestimmen die benutzerdefinierten und Konvertierungsoperatoren aufgehoben gelten. Für einen Konvertierungsoperator, angewendet werden, es muss möglich sein, eine standardkonvertierung ausführen ([standardkonvertierungen](conversions.md#standard-conversions)) aus einer Datenquelle mit dem Operanden Typ des Operators an, und es muss möglich sein, eine standardkonvertierung ausführen von der Ergebnistyp des Operators, der den Zieltyp.
*  Aus der Gruppe der entsprechenden benutzerdefinierten Operatoren bestimmt der Operator eindeutig am spezifischsten ist. Allgemein gesagt ist der am genauesten Operator den Operator aus, deren Operandentyp "nächstgelegene" in den Quelltyp und, dessen Ergebnistyp "nächstgelegene" in den Zieltyp ist. Eine benutzerdefinierte Konvertierungsoperatoren werden gegenüber transformierten Konvertierungsoperatoren bevorzugt. Die genauen Regeln für die Einrichtung des spezifischsten benutzerdefinierten Konvertierungsoperator werden in den folgenden Abschnitten definiert.

Nachdem ein spezifischste benutzerdefinierten Konvertierungsoperator identifiziert wurde, umfasst die eigentliche Ausführung der benutzerdefinierten Konvertierung bis zu drei Schritte aus:

*  Zuerst bei Bedarf ausführen eine standardkonvertierung von den Quelltyp in der Operandentyp der benutzerdefinierten oder transformierten Konvertierungsoperator.
*  Als Nächstes das Aufrufen des benutzerdefinierten oder transformierten Konvertierungsoperators, um die Konvertierung auszuführen.
*  Bei Bedarf ausführen schließlich eine standardkonvertierung von der Ergebnistyp des Operators oder transformierten, eine benutzerdefinierte Konvertierung in den Zieltyp auf.

Auswertung von nie eine benutzerdefinierte Konvertierung umfasst mehr als eine benutzerdefinierte oder transformierten Konvertierungsoperator. Das heißt, eine Konvertierung von Typ `S` eingeben `T` führt niemals zuerst eine benutzerdefinierte Konvertierung von `S` zu `X` und führen Sie dann eine benutzerdefinierte Konvertierung von `X` zu `T`.

Genaue Definitionen der Auswertung des benutzerdefinierten impliziten oder expliziten Konvertierungen sind in den folgenden Abschnitten enthalten. Die Definitionen stellen die folgenden Begriffe verwendet:

*  Wenn eine implizite standardkonvertierung ([Standard implizite Konvertierungen](conversions.md#standard-implicit-conversions)) vorhanden ist, von einem Typ `A` auf einen Typ `B`, und wenn weder `A` noch `B` sind *Interface_type*s, klicken Sie dann `A` gilt als ***von darin enthaltenen*** `B`, und `B` gilt als ***umfassen*** `A`.
*  Die ***umfassendste Typ*** in einen Satz von Typen wird der eine Typ, der alle anderen Typen in der Menge umfasst. Wenn kein Typ auf alle anderen Typen umfasst, hat dann die Gruppe keine umfassendste. Intuitiver ausgedrückt, ist der umfassendste Typ "größten"-Typs in der Gruppe, der eine Typ, der alle anderen Typen implizit konvertiert werden kann.
*  Die ***die darin enthaltenen Typ*** in einen Satz von Typen wird der eine Typ, der von allen anderen Typen in der Gruppe Datenbankprotokolls enthalten ist. Wenn kein Typ von allen anderen Typen Datenbankprotokolls enthalten ist, hat die Gruppe nicht am häufigsten Typ einschließt. Intuitiver ausgedrückt, ist die am stärksten umfasste Typ den "kleinsten" Datentyp in der Gruppe, der eine Typ, der implizit in jeder der anderen Typen konvertiert werden kann.

### <a name="processing-of-user-defined-implicit-conversions"></a>Verarbeitung von benutzerdefinierten implizite Konvertierungen

Eine implizite Konvertierung von Typ `S` eingeben `T` wird wie folgt verarbeitet:

*  Geben Sie die Typen `S0` und `T0`. Wenn `S` oder `T` nullable-Typen sind `S0` und `T0` ihre zugrunde liegende Typen sind, andernfalls `S0` und `T0` gleich `S` und `T` bzw.
*  Suchen Sie den Satz von Typen, `D`, über die benutzerdefinierte Konvertierung Operatoren betrachtet werden. Dieser Satz besteht aus `S0` (Wenn `S0` ist eine Klasse oder Struktur), die Basisklassen `S0` (Wenn `S0` ist eine Klasse), und `T0` (Wenn `T0` ist eine Klasse oder Struktur).
*  Suchen Sie den Satz von entsprechenden benutzerdefinierten und transformierten Konvertierungsoperatoren, `U`. Dieser Satz besteht aus den benutzerdefinierten und transformierten Implizite Konvertierungsoperatoren deklariert, indem Sie die Klassen oder Strukturen in `D` , konvertiert in einen Typ umfasst `S` auf einen Typ von darin enthaltenen `T`. Wenn `U` leer ist, die Konvertierung ist nicht definiert ist, und ein Fehler während der Kompilierung auftritt.
*  Suchen Sie einen möglichst spezifischen Quelltyp, `SX`, der Operatoren in `U`:
    * Wenn einer der Operatoren in `U` Konvertieren von `S`, klicken Sie dann `SX` ist `S`.
    * Andernfalls `SX` ist der am stärksten umfasste Typ in der kombinierten Gruppe von Typen von Operatoren in `U`. Wenn genau ein die darin enthaltenen Typ nicht gefunden werden, und klicken Sie dann die Konvertierung mehrdeutig ist, und ein Fehler während der Kompilierung auftritt.
*  Suchen Sie einen möglichst spezifischen Zieltyp, `TX`, der Operatoren in `U`:
    * Wenn einer der Operatoren in `U` konvertieren in `T`, klicken Sie dann `TX` ist `T`.
    * Andernfalls `TX` ist der umfassendste Typ in der kombinierten Gruppe von Zieltypen von Operatoren in `U`. Wenn genau ein umfassendste-Typ nicht gefunden wird, klicken Sie dann die Konvertierung mehrdeutig ist, und ein Fehler während der Kompilierung auftritt.
*  Suchen Sie den spezifischsten Konvertierungsoperator:
    * Wenn `U` enthält genau eine benutzerdefinierte Konvertierung-Operator, der von konvertiert `SX` zu `TX`, ist dies die spezifischste Konvertierungsoperator.
    * Andernfalls gilt: Wenn `U` enthält genau eine angehobene Konvertierungsoperator, der von konvertiert `SX` zu `TX`, ist dies die spezifischste Konvertierungsoperator.
    * Andernfalls die Konvertierung mehrdeutig ist, und ein Fehler während der Kompilierung auftritt.
*  Wenden Sie schließlich die Konvertierung:
    * Wenn `S` nicht `SX`, klicken Sie dann eine standard implizite Konvertierung von `S` zu `SX` erfolgt.
    * Die spezifischste Konvertierungsoperator wird aufgerufen, für die Konvertierung von `SX` zu `TX`.
    * Wenn `TX` nicht `T`, klicken Sie dann eine standard implizite Konvertierung von `TX` zu `T` erfolgt.

### <a name="processing-of-user-defined-explicit-conversions"></a>Verarbeitung von benutzerdefinierten explizite Konvertierungen

Eine benutzerdefinierte, explizite Konvertierung von Typ `S` eingeben `T` wird wie folgt verarbeitet:

*  Geben Sie die Typen `S0` und `T0`. Wenn `S` oder `T` nullable-Typen sind `S0` und `T0` ihre zugrunde liegende Typen sind, andernfalls `S0` und `T0` gleich `S` und `T` bzw.
*  Suchen Sie den Satz von Typen, `D`, über die benutzerdefinierte Konvertierung Operatoren betrachtet werden. Dieser Satz besteht aus `S0` (Wenn `S0` ist eine Klasse oder Struktur), die Basisklassen `S0` (Wenn `S0` ist eine Klasse), `T0` (Wenn `T0` ist eine Klasse oder Struktur), und die Basisklassen `T0` (Wenn `T0`ist eine Klasse).
*  Suchen Sie den Satz von entsprechenden benutzerdefinierten und transformierten Konvertierungsoperatoren, `U`. Dieser Satz besteht aus den benutzerdefinierten und transformierten implizite oder explizite Konvertierungsoperatoren deklariert, von den Klassen oder Strukturen in `D` , Konvertierung von einem Typ umfasst oder von darin enthaltenen `S` auf einen Typ umfasst oder durch einschließt`T`. Wenn `U` leer ist, die Konvertierung ist nicht definiert ist, und ein Fehler während der Kompilierung auftritt.
*  Suchen Sie einen möglichst spezifischen Quelltyp, `SX`, der Operatoren in `U`:
    * Wenn einer der Operatoren in `U` Konvertieren von `S`, klicken Sie dann `SX` ist `S`.
    * Andernfalls, wenn einer der Operatoren in `U` Konvertieren von Typen, die umfassen `S`, klicken Sie dann `SX` ist der am stärksten umfasste Typ in der kombinierten Gruppe von Quelltypen diese Operatoren. Wenn kein am darin enthaltenen Typ gefunden werden kann, und klicken Sie dann die Konvertierung mehrdeutig ist, und ein Kompilierungsfehler tritt auf.
    * Andernfalls `SX` ist der umfassendste Typ in der kombinierten Gruppe von Typen von Operatoren in `U`. Wenn genau ein umfassendste-Typ nicht gefunden wird, klicken Sie dann die Konvertierung mehrdeutig ist, und ein Fehler während der Kompilierung auftritt.
*  Suchen Sie einen möglichst spezifischen Zieltyp, `TX`, der Operatoren in `U`:
    * Wenn einer der Operatoren in `U` konvertieren in `T`, klicken Sie dann `TX` ist `T`.
    * Andernfalls, wenn einer der Operatoren in `U` konvertieren in Typen, die vom enthalten sind `T`, klicken Sie dann `TX` ist der umfassendste Typ in der kombinierten Gruppe von Zieltypen diese Operatoren. Wenn genau ein umfassendste-Typ nicht gefunden wird, klicken Sie dann die Konvertierung mehrdeutig ist, und ein Fehler während der Kompilierung auftritt.
    * Andernfalls `TX` ist der am stärksten umfasste Typ in der kombinierten Gruppe von Zieltypen von Operatoren in `U`. Wenn kein am darin enthaltenen Typ gefunden werden kann, und klicken Sie dann die Konvertierung mehrdeutig ist, und ein Kompilierungsfehler tritt auf.
*  Suchen Sie den spezifischsten Konvertierungsoperator:
    * Wenn `U` enthält genau eine benutzerdefinierte Konvertierung-Operator, der von konvertiert `SX` zu `TX`, ist dies die spezifischste Konvertierungsoperator.
    * Andernfalls gilt: Wenn `U` enthält genau eine angehobene Konvertierungsoperator, der von konvertiert `SX` zu `TX`, ist dies die spezifischste Konvertierungsoperator.
    * Andernfalls die Konvertierung mehrdeutig ist, und ein Fehler während der Kompilierung auftritt.
*  Wenden Sie schließlich die Konvertierung:
    * Wenn `S` nicht `SX`, klicken Sie dann eine explizite standardkonvertierung von `S` zu `SX` erfolgt.
    * Die spezifischste benutzerdefinierten Konvertierungsoperator wird aufgerufen, für die Konvertierung von `SX` zu `TX`.
    * Wenn `TX` nicht `T`, klicken Sie dann eine explizite standardkonvertierung von `TX` zu `T` erfolgt.

## <a name="anonymous-function-conversions"></a>Anonyme Funktion Konvertierungen

Ein *Anonymous_method_expression* oder *Lambda_expression* wird eine anonyme Funktion klassifiziert ([anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions)). Der Ausdruck nicht über einen Datentyp verfügt jedoch implizit in einen kompatiblen Delegattyp bzw. den Typ für die Ausdrucksbaumstruktur konvertiert werden kann. Insbesondere eine anonyme Funktion `F` ist kompatibel mit einem Delegattypen `D` bereitgestellt:

*  Wenn `F` enthält ein *Anonymous_function_signature*, klicken Sie dann `D` und `F` die gleiche Anzahl von Parametern aufweisen.
*  Wenn `F` enthält kein *Anonymous_function_signature*, klicken Sie dann `D` möglicherweise NULL oder mehr Parameter eines beliebigen Typs, solange kein Parameter der `D` hat die `out` Modifizierer für Parameter.
*  Wenn `F` verfügt über eine explizit typisierte Parameterliste, jeden Parameter in `D` hat den gleichen Typ und -Modifizierern als der entsprechende Parameter im `F`.
*  Wenn `F` verfügt über eine implizit typisierte Parameterliste `D` hat keine `ref` oder `out` Parameter.
*  Wenn der Text der `F` ist ein Ausdruck, und entweder `D` verfügt über eine `void` Rückgabetyp oder `F` Async ist und `D` weist den Rückgabetyp `Task`, daraufhin beim jeder Parameter des `F` erhält den Typ des der entsprechende Parameter im `D`, den Text der `F` ein gültiger Ausdruck (wrt- [Ausdrücke](expressions.md)), würde als berechtigt sein, eine *Statement_expression* ([Ausdrucksanweisungen](statements.md#expression-statements)).
*  Wenn der Text der `F` einen Anweisungsblock ein, und entweder `D` verfügt über eine `void` Rückgabetyp oder `F` Async ist und `D` weist den Rückgabetyp `Task`, daraufhin beim jeder Parameter des `F` erhält den Typ des der entsprechende Parameter im `D`, den Text der `F` ist ein gültiger Anweisungsblock (wrt- [Blöcke](statements.md#blocks)) das keine `return` Anweisung gibt einen Ausdruck an.
*  Wenn der Text der `F` ist ein Ausdruck, und *entweder* `F` ist nicht asynchronen und `D` hat einen nicht-Void-Rückgabetyp `T`, *oder* `F` ist asynchron und `D` hat einen Rückgabetyp `Task<T>`, und wenn jeder Parameter des `F` erhält den Typ des entsprechenden Parameters in `D`, den Text der `F` ein gültiger Ausdruck (wrt- [ Ausdrücke](expressions.md)), die implizit konvertierbar ist `T`.
*  Wenn der Text der `F` ist ein Anweisungsblock und *entweder* `F` ist nicht asynchronen und `D` hat einen nicht-Void-Rückgabetyp `T`, *oder* `F` ist asynchron und `D` hat einen Rückgabetyp `Task<T>`, und wenn jeder Parameter des `F` erhält den Typ des entsprechenden Parameters in `D`, den Text der `F` ist ein gültiger Anweisungsblock (wrt- [Blöcke ](statements.md#blocks)) mit einem nicht erreichbaren Endpunkt in der jedes `return` -Anweisung gibt einen Ausdruck, der implizit in `T`.

Im Rahmen der Kürze halber wird in diesem Abschnitt die Kurzform für die Tasktypen `Task` und `Task<T>` ([asynchrone Funktionen](classes.md#async-functions)).

Ein Lambda-Ausdruck `F` ist kompatibel mit einem Typ für die Ausdrucksbaumstruktur `Expression<D>` Wenn `F` ist kompatibel mit dem Delegattyp `D`. Beachten Sie, dass dies nicht für anonyme Methoden, Lambda-Ausdrücke gelten.

Bestimmte Lambda-Ausdrücke können nicht in ausdrucksbaumstrukturtypen konvertiert werden: Auch wenn die Konvertierung *vorhanden*, zum Zeitpunkt der Kompilierung ein Fehler auftritt. Dies ist der Fall, wenn der Lambda-Ausdruck:

*  Verfügt über eine *Block* Text
*  Enthält einfache oder zusammengesetzte Zuweisungsoperatoren
*  Enthält einen dynamisch gebundenen Ausdruck
*  Async ist

Verwenden Sie die folgenden Beispielen einen generischer Delegattyp `Func<A,R>` steht für eine Funktion, die ein des Typs Argument `A` und gibt einen Wert vom Typ `R`:
```csharp
delegate R Func<A,R>(A arg);
```

In der Zuweisungen
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
die Typen für Parameter und Rückgabetypen für jede anonyme Funktion werden aus dem Typ der Variablen bestimmt, die die anonyme Funktion zugewiesen wird.

Die erste Zuweisung konvertiert die anonyme Funktion wurde erfolgreich in den Delegattyp `Func<int,int>` da Wenn `x` erhält den Typ `int`, `x+1` ist ein gültiger Ausdruck, der implizit in den Typ `int`.

Entsprechend die zweite Zuweisung erfolgreich konvertiert die anonyme Funktion in den Delegattyp `Func<int,double>` da das Ergebnis des `x+1` (des Typs `int`) wird implizit in den Typ `double`.

Die dritte Zuweisung ist jedoch ein Fehler während der Kompilierung, da bei `x` erhält den Typ `double`, das Ergebnis des `x+1` (des Typs `double`) kann nicht implizit in den Typ in `int`.

Die vierte Zuweisung erfolgreich konvertiert die anonymen Async-Funktion in den Delegattyp `Func<int, Task<int>>` da das Ergebnis des `x+1` (des Typs `int`) wird implizit in den Ergebnistyp `int` der Vorgangsart `Task<int>`.

Anonyme Funktionen möglicherweise Auflösung von funktionsüberladungen beeinflussen und Typrückschluss teilnehmen. Finden Sie unter [Funktionsmember](expressions.md#function-members) Weitere Details.

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>Auswertung der anonymen Funktion-Konvertierungen in Delegattypen

Konvertierung von einer anonymen Funktion in einen Delegattyp erstellt eine Delegatinstanz, die ein Verweis auf die anonyme Funktion und der (möglicherweise leeren) Satz von erfassten äußeren Variablen, die zum Zeitpunkt der Auswertung aktiv sind. Wenn der Delegat aufgerufen wird, wird der Text der anonymen Funktion ausgeführt. Der Code im Funktionstext wird ausgeführt, mithilfe der erfassten äußere Variablen, die der Delegat verweist.

Die Aufrufliste eines Delegaten, die aus einer anonymen Funktion enthält einen einzelnen Eintrag. Die genaue Zielobjekt und die Zielmethode des Delegaten sind nicht angegeben. Insbesondere ist nicht angegeben, ob das Zielobjekt des Delegaten ist `null`, `this` Wert der einschließenden Funktionsmember oder ein anderes Objekt.

Konvertierungen von semantisch identisch anonyme Funktionen mit denselben (möglicherweise leere) erfassten äußere Variable-Instanzen, den gleichen Delegattypen sind zulässig (jedoch nicht erforderlich) auf die gleiche Delegatinstanz zurückgeben. Der Begriff, die semantisch identisch wird hier verwendet, bedeutet, dass die gleichen Auswirkungen, die die gleichen Argumenten angegeben wird, Ausführung von anonymen Funktionen in allen Fällen erzeugt. Diese Regel erlaubt, Code wie den folgenden optimiert werden soll.

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

Da die beiden anonyme Funktionsdelegaten demselben (leeren haben) Satz von erfassten äußeren Variablen, und da die anonyme Funktionen semantisch identisch sind, wird der Compiler zulässig die Delegaten, der auf die gleiche Zielmethode zu verweisen. Tatsächlich ist der Compiler zulässig, die dieselbe Delegatinstanz aus sowohl anonyme Funktionsausdrücke zurückgegeben.

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>Auswertung von Konvertierungen ausdrucksbaumstrukturtypen anonyme Funktion

Konvertierung von einer anonymen Funktion in einen Typ für die Ausdrucksbaumstruktur erzeugt eine Ausdrucksbaumstruktur ([ausdrucksbaumstrukturtypen](types.md#expression-tree-types)). Genauer gesagt, führt die Auswertung der Konvertierung des anonymen Funktion für die Entwicklung von einem Objektstruktur, die die Struktur der anonymen Funktion selbst darstellt. Die genaue Struktur der Ausdrucksbaumstruktur als auch die genauen Schritte zum Erstellen, sind die Implementierung definiert.

### <a name="implementation-example"></a>Beispiel für die Implementierung

Dieser Abschnitt beschreibt eine mögliche Implementierung der anonymen Funktion Konvertierungen in Bezug auf andere Konstrukte in c#. Die hier beschriebene Implementierung basiert darauf, dass die gleichen Prinzipien, die vom Microsoft C#-Compiler verwendet, aber es ist keine beauftragten-Implementierung, noch ist es das einzig mögliche. Konvertierungen in Ausdrucksbaumstrukturen, wird nur kurz erwähnt, wie die genaue Semantik außerhalb des Bereichs dieser Spezifikation.

Der übrige Teil dieses Abschnitts enthält mehrere Beispiele für Code, der anonyme Funktionen mit unterschiedlichen Eigenschaften enthält. Für jedes Beispiel erfolgt eine entsprechende Übersetzung in Code, der nur anderen C#-Konstrukten verwendet. In den Beispielen wird der Bezeichner `D` davon stellen den folgenden Delegattyp:
```csharp
public delegate void D();
```

Die einfachste Form einer anonymen Funktion ist eine, die keine äußeren Variablen erfasst:
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

Dies kann in eine Instanziierung von Delegaten übersetzt werden, die vom Compiler generierte statische Methode verweist, in dem der Code der anonymen Funktion platziert wird:
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

Im folgenden Beispiel verweist die anonyme Funktion Instanzmember `this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

Dies kann auf eine vom Compiler generierten Instanz-Methode, die den Code der anonymen Funktion übersetzt werden:
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

In diesem Beispiel werden die anonyme Funktion eine lokale Variable erfasst:
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

Die Lebensdauer der lokalen Variablen muss jetzt auf mindestens die Lebensdauer des Delegaten anonyme Funktion erweitert werden. Dies kann erreicht werden, indem "anheben" die lokale Variable in einem Feld einer vom Compiler generierten Klasse. Instanziierung der lokalen Variablen ([Instanziierung von lokalen Variablen](expressions.md#instantiation-of-local-variables)) klicken Sie dann zum Erstellen einer Instanz von der vom Compiler generierten Klasse und den Zugriff auf die lokale Variable entspricht, für den Zugriff auf ein Feld in der Instanz von entspricht. die vom Compiler generierten Klasse. Darüber hinaus wird die anonyme Funktion eine Instanzmethode der vom Compiler generierten Klasse:
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

Zum Schluss die folgenden anonymen Funktion erfasst `this` und zweier lokaler Variablen mit verschiedenen Lebensdauer:
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

Hier wird eine vom Compiler generierten Klasse erstellt, für jede Anweisung-block in der lokalen Variablen erfasst werden, so, dass die lokalen Variablen in anderen Blöcken unabhängige Lebensdauer aufweisen können. Eine Instanz von `__Locals2`, die vom Compiler generierten-Klasse für den Block inner-Anweisung enthält, die lokale Variable `z` und ein Feld, das eine Instanz des verweist `__Locals1`.  Eine Instanz von `__Locals1`, die vom Compiler generierten-Klasse für den äußeren Anweisungsblock enthält die lokale Variable `y` und ein Feld, das Verweise `this` der einschließenden Funktion-Elements. Erfasst alle mit diese Datenstrukturen zu erreichen, kann das äußere Variablen durch eine Instanz der `__Local2`, und der Code der anonymen Funktion kann daher als eine Instanzmethode dieser Klasse implementiert werden.

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

Das gleiche Verfahren, die hier angewendet werden, um die lokale Variablen erfassen kann auch verwendet werden, wenn anonyme Funktionen in Ausdrucksbaumstrukturen konvertiert: Verweise auf die vom Compiler generierten Objekte können in der Ausdrucksbaumstruktur gespeichert werden, und den Zugriff auf die lokalen Variablen dargestellt werden kann, wie das Feld für die folgenden Objekte zugreift. Der Vorteil dieses Ansatzes ist, dass die "transformierten" lokalen Variablen von Delegaten und Ausdrucksbaumstrukturen gemeinsam genutzt werden können.

## <a name="method-group-conversions"></a>Konvertierungen für Gruppe

Eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) aus einer Methodengruppe vorhanden ist ([ausdrucksklassifizierungen](expressions.md#expression-classifications)) in einen kompatiblen Delegattyp. Erhalten einen Delegattyp `D` und einen Ausdruck `E` , die als eine Methodengruppe klassifiziert ist, ist eine implizite Konvertierung von vorhanden `E` zu `D` Wenn `E` enthält mindestens eine Methode, die in sein (Normalform angewendet wird [Anwendbarer Funktionsmember](expressions.md#applicable-function-member)) auf eine Argumentliste erstellt, durch Nutzung der Parametertypen und Modifizierern von `D`, wie im folgenden beschrieben.

Die Anwendung während der Kompilierung einer Konvertierung von einer Methodengruppe `E` in einen Delegattyp `D` wird im folgenden beschrieben. Beachten Sie, dass das Vorhandensein eine implizite Konvertierung von `E` zu `D` garantiert nicht, dass die Anwendung während der Kompilierung der Konvertierung ohne Fehler durchgeführt werden.

*  Eine einzelne Methode `M` ausgewählt ist, den Aufruf einer Methode entsprechen ([Methodenaufrufe](expressions.md#method-invocations)) des Formulars `E(A)`, mit der folgenden Änderungen:
    * Die Argumentliste `A` ist eine Liste von Ausdrücken, jede klassifizierte, wie eine Variable und mit dem Typ und die Modifizierer (`ref` oder `out`) des entsprechenden Parameters in der *Formal_parameter_list* von `D`.
    * Die Candidate-Methoden, die berücksichtigt werden nur die Methoden, die in ihren Normalform angewendet werden ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)), nicht auf die nur in ihre erweiterte Form angewendet.
*  Wenn der Algorithmus des [Methodenaufrufe](expressions.md#method-invocations) einen Fehler erzeugt, und klicken Sie dann ein Fehler während der Kompilierung auftritt. Andernfalls erzeugt der Algorithmus eine einzelne bewährte Methode `M` müssen die gleiche Anzahl von Parametern wie `D` und die Konvertierung wird als vorhanden angesehen.
*  Die ausgewählte Methode `M` kompatibel sein muss ([delegieren Kompatibilität](delegates.md#delegate-compatibility)) mit dem Delegattyp `D`, oder andernfalls ein Kompilierungsfehler tritt auf.
*  Wenn die ausgewählte Methode `M` ist eine Instanzmethode, die zugeordneten Instanzausdruck `E` bestimmt das Zielobjekt des Delegaten.
*  Wenn die ausgewählte Methode M eine Erweiterungsmethode die durch einen Elementzugriff auf ein Instanzenausdruck gekennzeichnet ist ist, bestimmt dieser Instanzausdruck das Zielobjekt des Delegaten.
*  Das Ergebnis der Konvertierung ist ein Wert vom Typ `D`, d. h. ein neu erstellter Delegat, der auf dem ausgewählten Methode und die Ziel-Objekt verweist.
*  Beachten Sie, die diesen Prozess für die Erstellung eines Delegaten an eine Erweiterungsmethode führen können, wenn der Algorithmus des [Methodenaufrufe](expressions.md#method-invocations) finden Sie eine Instanzmethode jedoch erfolgreich ausgeführt wird, bei der Verarbeitung des Aufrufs `E(A)` als Erweiterung Methodenaufruf ([Erweiterung Methodenaufrufe](expressions.md#extension-method-invocations)). Ein Delegat, die so erstellte erfasst die Erweiterungsmethode als auch als erstes Argument.

Das folgende Beispiel zeigt die Gruppe Konvertierungen:
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

Die Zuweisung zu `d1` implizit konvertiert die Methodengruppe `F` auf einen Wert vom Typ `D1`.

Die Zuweisung zu `d2` zeigt, wie es ist möglich, einen Delegaten an eine Methode erstellen, die über weniger abgeleitete (Contravariant) Typen verfügt und eine abgeleitete (covariant)-Rückgabetyp.

Die Zuweisung zu `d3` zeigt wie keine Konvertierung möglich ist, wenn die Methode nicht anwendbar ist.

Die Zuweisung zu `d4` zeigt, wie die Methode in der Normalform angewendet werden muss.

Die Zuweisung zu `d5` zeigt, wie Parameter und Rückgabetypen Typen des Delegaten und der Methode unterscheidet nur für Verweistypen zulässig sind.

Wie bei allen anderen implizite und explizite Konvertierungen kann der Cast-Operator verwendet werden, um explizit eine Methode gruppenkonvertierung auszuführen. Das Beispiel
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
könnte stattdessen geschrieben werden
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

Methodengruppen möglicherweise Auflösung von funktionsüberladungen beeinflussen und Typrückschluss teilnehmen. Finden Sie unter [Funktionsmember](expressions.md#function-members) Weitere Details.

Die Laufzeit-Auswertung von einer Methode die gruppenkonvertierung wird wie folgt aus:

*  Wenn die Methode, die zum Zeitpunkt der Kompilierung ausgewählt, eine Instanzmethode ist, oder es ist eine Erweiterungsmethode, die als eine Instanzmethode zugegriffen wird, richtet sich das Zielobjekt des Delegaten aus der zugeordneten Instanzausdruck `E`:
    * Die Instanzausdruck wird ausgewertet. Wenn diese Evaluierungsversion auf eine Ausnahme auslöst, werden keine weiteren Schritte ausgeführt.
    * Wenn der Instanzausdruck ist eine *Reference_type*, der Wert, der durch den Instanzausdruck berechnet wird, das Zielobjekt. Wenn die ausgewählte Methode eine Instanzmethode ist und das Zielobjekt ist `null`, `System.NullReferenceException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.
    * Wenn der Instanzausdruck ist eine *Value_type*, ein Boxing-Vorgang ([Boxing-Konvertierung](types.md#boxing-conversions)) wird ausgeführt, um den Wert auf ein Objekt zu konvertieren und dieses Objekt wird das Zielobjekt.
*  Andernfalls wird die ausgewählte Methode Teil eines Aufrufs der statischen Methode aus, und das Zielobjekt des Delegaten ist `null`.
*  Eine neue Instanz des Delegattyps `D` zugeordnet ist. Wenn nicht genügend Arbeitsspeicher verfügbar, um die neue Instanz einer `System.OutOfMemoryException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.
*  Die neue Delegatinstanz wird mit einem Verweis auf die Methode, die zum Zeitpunkt der Kompilierung bestimmt wurde initialisiert, und ein Verweis auf das Zielobjekt oben berechnete.
