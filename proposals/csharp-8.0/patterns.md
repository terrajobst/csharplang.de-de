---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483979"
---
# <a name="recursive-pattern-matching"></a>Rekursiver Musterabgleich

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Muster Vergleichs Erweiterungen für C# ermöglichen viele der Vorteile von algebraischen Datentypen und Musterabgleich von funktionalen Sprachen, aber auf eine Weise, die nahtlos in das Verhalten der zugrunde liegenden Sprache integriert ist. Elemente dieses Ansatzes sind von verwandten Features in den Programmiersprachen [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Erweiterbarer Musterabgleich über eine vereinfachte Sprache") und [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Abgleichen von Objekten mit Mustern, Seite 273")inspiriert.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

### <a name="is-expression"></a>Is-Ausdruck

Der `is`-Operator wird erweitert, um einen Ausdruck mit einem *Muster*zu testen.

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

Diese Form von *relational_expression* wird zusätzlich zu den in der C# Spezifikation vorhandenen Formularen angezeigt. Es handelt sich um einen Kompilierzeitfehler, wenn der *relational_expression* auf der linken Seite des `is`-Tokens keinen Wert oder keinen Typ festgelegt hat.

Jeder *Bezeichner* des Musters führt eine neue lokale Variable ein, die *definitiv zugewiesen* wird, nachdem der `is`-Operator `true` wurde (d.h. *definitiv zugewiesen, wenn true*).

> Hinweis: Es gibt technisch gesehen eine Mehrdeutigkeit zwischen dem *Typ* in einer `is-expression` und *constant_pattern*, von denen jede eine gültige Analyse eines qualifizierten Bezeichners sein könnte. Wir versuchen, Sie als Typ für die Kompatibilität mit früheren Versionen der Sprache zu binden. nur wenn dies nicht möglich ist, lösen wir es wie einen Ausdruck in anderen Kontexten auf, um den ersten gefundenen Vorgang (bei dem es sich um eine Konstante oder einen Typ handeln muss) durchzuführen. Diese Mehrdeutigkeit ist nur auf der rechten Seite eines `is` Ausdrucks vorhanden.

### <a name="patterns"></a>Muster

Muster werden im *is_pattern* Operator, in einem *switch_statement*und in einem *switch_expression* verwendet, um die Form der Daten auszudrücken, mit denen eingehende Daten (die als Eingabe Wert bezeichnet werden) verglichen werden sollen. Muster können rekursiv sein, damit Teile der Daten mit unter Mustern verglichen werden können.

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a>Deklarations Muster

```antlr
declaration_pattern
    : type simple_designation
    ;
```

Der *declaration_pattern* testet, ob ein Ausdruck einen bestimmten Typ hat, und wandelt ihn in diesen Typ um, wenn der Test erfolgreich ist. Dadurch kann eine lokale Variable des angegebenen Typs, benannt durch den angegebenen Bezeichner, eingeführt werden, wenn die Bezeichnung eine *single_variable_designation*ist. Diese lokale Variable ist *definitiv zugewiesen* , wenn das Ergebnis der Muster Vergleichsoperation `true`wird.

Die Lauf Zeit Semantik dieses Ausdrucks besteht darin, dass er den Lauf Zeittyp des linken *relational_expression* Operanden mit dem *Typ* im Muster testet.  Wenn es sich um einen Lauf Zeittyp (oder einen Untertyp) handelt und nicht `null`, ist das Ergebnis der `is operator` `true`.

Bestimmte Kombinationen von statischem Typ der linken Seite und des angegebenen Typs werden als nicht kompatibel betrachtet und führen zu einem Kompilierzeitfehler. Ein Wert vom Typ "static Type `E`" ist mit einem Typ " *Muster kompatibel* " `T`, wenn eine Identitäts Konvertierung, eine implizite Verweis Konvertierung, eine Boxing-Konvertierung, eine explizite Verweis Konvertierung oder eine Unboxing-Konvertierung von `E` in `T`vorhanden ist, oder wenn einer dieser Typen ein offener Typ ist. Es handelt sich um einen Kompilierzeitfehler, wenn eine Eingabe vom Typ `E` nicht mit dem *Typ* in einem Typmuster *kompatibel* ist, mit dem Sie übereinstimmt.

Das Typmuster ist nützlich zum Ausführen von Lauf Zeittyp Tests von Verweis Typen und ersetzt die Ausdrucksweise.

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

Mit etwas präziseren

```csharp
if (expr is Type v) { // code using v
```

Es ist ein Fehler, wenn der *Typ* ein Werte zulässt-Werttyp ist.

Das Typmuster kann verwendet werden, um Werte von Typen zu testen, die NULL-Werte zulassen: ein Wert vom Typ `Nullable<T>` (oder ein gebocktes `T`) entspricht einem Typmuster `T2 id` wenn der Wert nicht NULL ist und der Typ der `T2` `T`oder ein Basistyp oder eine Schnittstelle `T`ist. Beispielsweise im Code Fragment

```csharp
int? x = 3;
if (x is int v) { // code using v
```

Die Bedingung der `if`-Anweisung wird zur Laufzeit `true`, und die Variable `v` enthält den Wert `3` vom Typ `int` innerhalb des Blocks. Nach dem Block ist die Variable `v` im Gültigkeitsbereich, aber nicht definitiv zugewiesen.

#### <a name="constant-pattern"></a>Konstantes Muster

```antlr
constant_pattern
    : constant_expression
    ;
```

Ein konstantes Muster testet den Wert eines Ausdrucks mit einem konstanten Wert. Die Konstante kann ein beliebiger konstanter Ausdruck sein, z. b. ein Literalwert, der Name einer deklarierten `const` Variablen oder eine Enumerationskonstante. Wenn der Eingabe Wert kein offener Typ ist, wird der Konstante Ausdruck implizit in den Typ des übereinstimmenden Ausdrucks konvertiert. Wenn der Typ des Eingabe Werts nicht mit dem Typ des konstanten Ausdrucks *Muster kompatibel* ist, ist der Muster Vergleichs Vorgang ein Fehler.

Das Muster *c* wird als Übereinstimmung mit dem konvertierten Eingabe Wert *e* betrachtet, wenn `object.Equals(c, e)` `true`zurückgeben würde.

Es ist zu erwarten, dass `e is null` als gängigste Methode zum Testen auf `null` in neu geschriebenem Code angezeigt wird, da keine benutzerdefinierte `operator==`aufgerufen werden kann.

#### <a name="var-pattern"></a>Var-Muster

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

Wenn die *Bezeichnung* eine *simple_designation*ist, entspricht der Ausdruck *e* dem Muster. Anders ausgedrückt: eine Entsprechung zu einem *var-Muster* ist immer mit einem *simple_designation*erfolgreich. Wenn die *simple_designation* eine *single_variable_designation*ist, liegt der Wert von *e* an einer neu eingeführten lokalen Variablen. Der Typ der lokalen Variablen ist der statische Typ von *e*.

Wenn es sich bei der *Bezeichnung* um einen *tuple_designation*handelt, entspricht das Muster einem *positional_pattern* der Form `(var` *Bezeichnung*,... `)`, bei der die *Bezeichnungen*im *tuple_designation*gefunden werden.  Beispielsweise entspricht das Muster `var (x, (y, z))` `(var x, (var y, var z))`.

Wenn der Name `var` an einen Typ bindet, ist ein Fehler aufgetreten.

#### <a name="discard-pattern"></a>Muster verwerfen

```antlr
discard_pattern
    : '_'
    ;
```

Ein Ausdruck *e* stimmt mit dem Muster `_` immer überein. Dies bedeutet, dass jeder Ausdruck mit dem Verwerfungs Muster übereinstimmt.

Ein Verwerfungs Muster kann nicht als Muster eines *is_pattern_expression*verwendet werden.

#### <a name="positional-pattern"></a>Positions Muster

Ein Positions Muster prüft, ob der Eingabe Wert nicht `null`ist, ruft eine entsprechende `Deconstruct` Methode auf und führt einen weiteren Musterabgleich für die resultierenden Werte aus.  Sie unterstützt auch eine tupelähnliche Muster Syntax (ohne den Typ), wenn der Typ des Eingabe Werts mit dem Typ übereinstimmt, der `Deconstruct`enthält, oder wenn der Typ des Eingabe Werts ein tupeltyp ist, oder wenn der Typ des Eingabe Werts `object` oder `ITuple` ist und der Lauf Zeittyp des Ausdrucks `ITuple`implementiert.

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

Wenn der *Typ* weggelassen wird, wird er als statischer Typ des Eingabe Werts übernommen.

Wenn eine Übereinstimmung eines Eingabe Werts mit dem *Mustertyp* `(` *subpattern_list* `)`vorliegt, wird eine Methode ausgewählt, indem der *Typ* nach zugänglichen Deklarationen von `Deconstruct` durchsucht und eine Methode ausgewählt wird, die dieselben Regeln wie für die Dekonstruktions Deklaration verwendet.

Es ist ein Fehler, wenn *ein positional_pattern* den Typ auslässt, über ein *einzelnes unter Muster* ohne einen *Bezeichner*verfügt, keine *property_subpattern* hat und keine *simple_designation*hat. Dies unterscheidet sich zwischen einer *constant_pattern* , die in Klammern gesetzt ist, und einem *positional_pattern*.

Um die Werte zu extrahieren, die mit den Mustern in der Liste abgeglichen werden sollen,
- Wenn *Type* ausgelassen wurde und der Typ des Eingabe Werts ein tupeltyp ist, muss die Anzahl der Teil Muster der Kardinalität des Tupels entsprechen. Jedes Tupelelement wird mit dem entsprechenden *Teil Muster*verglichen, und die Übereinstimmung ist erfolgreich, wenn alle diese erfolgreich sind. Wenn ein *Teil Muster* über einen *Bezeichner*verfügt, muss dieser ein Tupelelement an der entsprechenden Position im tupeltyp benennen.
- Wenn eine geeignete `Deconstruct` als Member des *Typs*vorhanden ist, handelt es sich um einen Kompilierzeitfehler, wenn der Typ des Eingabe Werts nicht mit dem *Typ*" *Muster kompatibel* " ist. Zur Laufzeit wird der Eingabe Wert anhand des *Typs*getestet. Wenn dies fehlschlägt, schlägt die Zuordnung des Positions Musters fehl. Wenn dies erfolgreich ist, wird der Eingabe Wert in diesen Typ konvertiert, und `Deconstruct` wird mit neuen vom Compiler generierten Variablen aufgerufen, um die `out` Parameter zu empfangen. Jeder empfangene Wert wird mit dem entsprechenden *Teil Muster*verglichen, und die Übereinstimmung ist erfolgreich, wenn alle diese erfolgreich sind. Wenn ein *Teil Muster* über einen *Bezeichner*verfügt, muss dieser einen Parameter an der entsprechenden Position `Deconstruct`benennen.
- Andernfalls, wenn der *Typ* weggelassen wurde und der Eingabe Wert vom Typ `object` oder `ITuple` oder ein Typ ist, der durch eine implizite Verweis Konvertierung in `ITuple` konvertiert werden kann, und in den Teil Mustern kein *Bezeichner* angezeigt wird, stimmen wir mithilfe `ITuple`ab.
- Andernfalls ist das Muster ein Kompilierzeitfehler.

Die Reihenfolge, in der Teil Muster zur Laufzeit abgeglichen werden, ist nicht angegeben, und eine fehlgeschlagene Übereinstimmung versucht möglicherweise nicht, alle Teil Muster abzugleichen.

##### <a name="example"></a>Beispiel

In diesem Beispiel werden viele Funktionen verwendet, die in dieser Spezifikation beschrieben werden.

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a>Eigenschafts Muster

Ein Eigenschafts Muster prüft, ob der Eingabe Wert nicht `null` ist, und vergleicht rekursiv Werte, die durch die Verwendung von zugänglichen Eigenschaften oder Feldern extrahiert werden.

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

Es tritt ein Fehler auf, wenn ein _Teil Muster_ einer _property_pattern_ keinen _Bezeichner_ enthält (es muss sich um das zweite Formular handeln, das einen _Bezeichner_aufweist).  Ein nach gestelltes Komma nach dem letzten Teil Muster ist optional.

Beachten Sie, dass ein Muster mit NULL-Überprüfung von einem trivialen Eigenschafts Muster abfällt. Um zu überprüfen, ob die Zeichenfolge `s` nicht NULL ist, können Sie jedes der folgenden Formulare schreiben.

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

Wenn eine Entsprechung eines Ausdrucks *e* zum *Mustertyp* `{` *property_pattern_list* `}`vorliegt, handelt es sich um einen Kompilierzeitfehler, wenn der Ausdruck *e* mit dem Typ *t* , der vom *Typ*festgelegt ist, nicht mit dem *Muster kompatibel* ist. Wenn der Typ nicht vorhanden ist, wird er als statischer Typ von *e*übernommen. Wenn der *Bezeichner* vorhanden ist, wird eine Muster Variable vom Typ *Type deklariert.* Jeder Bezeichner, der auf der linken Seite des *property_pattern_list* angezeigt wird, muss eine barrierefreie lesbare Eigenschaft oder ein Feld von *T*angeben. Wenn die *simple_designation* des *property_pattern* vorhanden ist, wird eine Muster Variable vom Typ *T*definiert.

Zur Laufzeit wird der Ausdruck für *T*getestet. Wenn dies fehlschlägt, schlägt das Eigenschafts Musterabgleich fehl, und das Ergebnis wird `false`. Wenn der Vorgang erfolgreich ist, werden die einzelnen *property_subpattern* Felder oder-Eigenschaften gelesen und deren Wert mit dem entsprechenden Muster übereinstimmen. Das Ergebnis der ganzen Übereinstimmung ist nur `false`, wenn das Ergebnis eines dieser `false`ist. Die Reihenfolge, in der Teil Muster abgeglichen werden, ist nicht angegeben, und eine fehlgeschlagene Übereinstimmung entspricht möglicherweise nicht allen Teil Mustern zur Laufzeit. Wenn die Übereinstimmung erfolgreich ist und die *simple_designation* der *property_pattern* ein *single_variable_designation*ist, wird eine Variable vom Typ *T* definiert, der der übereinstimmende Wert zugewiesen wird.

> Hinweis: das Eigenschafts Muster kann verwendet werden, um eine Muster Übereinstimmung mit anonymen Typen zu erhalten.

##### <a name="example"></a>Beispiel

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a>Switch-Ausdruck

Es wird ein *switch_expression* hinzugefügt, um `switch`ähnliche Semantik für einen Ausdrucks Kontext zu unterstützen.

Die C# Sprachsyntax wird durch die folgenden syntaktischen Produktionen erweitert:

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

Der *switch_expression* ist als *expression_statement*nicht zulässig.

> Wir möchten dies in einer zukünftigen Revision lockern.

Der Typ der *switch_expression* ist der [*am häufigsten*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) vorkommende Typ der Ausdrücke, die auf der rechten Seite des `=>` Tokens der *switch_expression_arm*s angezeigt werden, wenn ein solcher Typ vorhanden ist und der Ausdruck in jedem Arm des switch-Ausdrucks implizit in diesen Typ konvertiert werden kann.  Außerdem fügen wir eine neue *switchausdrucks Konvertierung*hinzu, bei der es sich um eine vordefinierte implizite Konvertierung von einem Switch-Ausdruck in alle Typen handelt `T` für die eine implizite Konvertierung von jedem Arm-Ausdruck in `T`vorhanden ist.

Wenn sich das Muster einiger *switch_expression_arm*nicht auf das Ergebnis auswirken kann, tritt ein Fehler auf, da einige vorherige Muster und Wächter immer eine Entsprechung haben.

Ein Switch-Ausdruck wird als voll *ständig* bezeichnet, wenn ein Arm des switch-Ausdrucks jeden Wert seiner Eingabe behandelt.  Der Compiler erzeugt eine Warnung, wenn ein Switch-Ausdruck nicht voll *ständig*ist.

Zur Laufzeit ist das Ergebnis des *switch_expression* der Wert des *Ausdrucks* des ersten *switch_expression_arm* , für den der Ausdruck auf der linken Seite des *switch_expression* dem Muster der *switch_expression_arm*entspricht und *für den case_guard der* *switch_expression_arm*, falls vorhanden, zu `true`ausgewertet wird. Wenn keine solche *switch_expression_arm*vorhanden ist, löst die *switch_expression* eine Instanz der Ausnahme `System.Runtime.CompilerServices.SwitchExpressionException`aus.

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a>Optionale Unterstriche beim Umschalten eines tupelliterals.

Um mit dem *switch_statement*ein tupelliteralelement zu wechseln, müssen Sie die scheinbar redundante Parameter schreiben.

```csharp
switch ((a, b))
{
```

Zum zulassen

```csharp
switch (a, b)
{
```

die Klammern der Switch-Anweisung sind optional, wenn der Ausdruck, für den gewechselt wird, ein tupelliteral ist.

### <a name="order-of-evaluation-in-pattern-matching"></a>Reihenfolge der Auswertung in Pattern-Matching

Die Flexibilität des Compilers beim Neuordnen der während des Musterabgleich ausgeführten Vorgänge kann die Flexibilität ermöglichen, die zur Verbesserung der Effizienz der Muster Übereinstimmung verwendet werden kann. Die (nicht erzwungene) Anforderung wäre, dass Eigenschaften, auf die in einem Muster zugegriffen wird, und die dekonstruktionsmethoden "Pure" (Nebeneffekt frei, idempotent usw.) sein müssen. Dies bedeutet nicht, dass wir die Reinheit als sprach Konzept hinzufügen würden, nur dass wir die compilerflexibilität beim Neuanordnen von Vorgängen ermöglichen würden.

**Lösung 2018-04-04 LDM**: bestätigt: der Compiler ist berechtigt, Aufrufe an `Deconstruct`, Eigenschaften Zugriffe und Aufrufe von Methoden in `ITuple`neu anzuordnen, und es kann davon ausgegangen werden, dass die zurückgegebenen Werte bei mehreren aufrufen identisch sind. Der Compiler sollte keine Funktionen aufrufen, die das Ergebnis nicht beeinflussen können, und wir werden sehr vorsichtig sein, bevor wir Änderungen an der vom Compiler generierten Reihenfolge der Auswertung in Zukunft vornehmen.

### <a name="some-possible-optimizations"></a>Einige mögliche Optimierungen

Die Kompilierung des Musterabgleich kann allgemeine Teile von Mustern nutzen. Wenn z. b. der Typtest der obersten Ebene von zwei aufeinander folgenden Mustern in einer *switch_statement* denselben Typ hat, kann der generierte Code den Typtest für das zweite Muster überspringen.

Wenn einige der Muster ganze Zahlen oder Zeichen folgen sind, kann der Compiler dieselbe Art von Code generieren, der für eine Switch-Anweisung in früheren Versionen der Sprache generiert wurde.

Weitere Informationen zu diesen Optimierungs Arten finden Sie unter [[Scott und Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "Wann ist die Heuristik für die Kompilierung wichtig?").
