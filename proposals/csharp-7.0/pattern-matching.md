---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484069"
---
# <a name="pattern-matching-for-c-7"></a>Musterabgleich C# für 7

Muster Vergleichs Erweiterungen für C# ermöglichen viele der Vorteile von algebraischen Datentypen und Musterabgleich von funktionalen Sprachen, aber auf eine Weise, die nahtlos in das Verhalten der zugrunde liegenden Sprache integriert ist. Die grundlegenden Features sind: [Daten Satz Typen](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), bei denen es sich um Typen handelt, deren semantische Bedeutung durch die Form der Daten beschrieben wird. und Musterabgleich, bei dem es sich um ein neues Ausdrucks Formular handelt, das eine extrem präzise Zerlegung dieser Datentypen ermöglicht. Elemente dieses Ansatzes sind von verwandten Features in den Programmiersprachen [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Erweiterbarer Musterabgleich über eine vereinfachte Sprache") und [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Abgleichen von Objekten mit Mustern")inspiriert.

## <a name="is-expression"></a>Is-Ausdruck

Der `is`-Operator wird erweitert, um einen Ausdruck mit einem *Muster*zu testen.

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

Diese Form von *relational_expression* wird zusätzlich zu den in der C# Spezifikation vorhandenen Formularen angezeigt. Es handelt sich um einen Kompilierzeitfehler, wenn der *relational_expression* auf der linken Seite des `is`-Tokens keinen Wert oder keinen Typ festgelegt hat.

Jeder *Bezeichner* des Musters führt eine neue lokale Variable ein, die *definitiv zugewiesen* wird, nachdem der `is`-Operator `true` wurde (d.h. *definitiv zugewiesen, wenn true*).

> Hinweis: Es gibt technisch gesehen eine Mehrdeutigkeit zwischen dem *Typ* in einer `is-expression` und *constant_pattern*, von denen jede eine gültige Analyse eines qualifizierten Bezeichners sein könnte. Wir versuchen, Sie als Typ für die Kompatibilität mit früheren Versionen der Sprache zu binden. nur wenn dies nicht möglich ist, lösen wir es wie in anderen Kontexten, bis zum ersten gefundenen (das entweder eine Konstante oder ein Typ sein muss). Diese Mehrdeutigkeit ist nur auf der rechten Seite eines `is` Ausdrucks vorhanden.

## <a name="patterns"></a>Muster

Muster werden im `is` Operator und in einem *switch_statement* verwendet, um die Form der Daten auszudrücken, mit denen eingehende Daten verglichen werden sollen. Muster können rekursiv sein, damit Teile der Daten mit unter Mustern verglichen werden können.

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> Hinweis: Es gibt technisch gesehen eine Mehrdeutigkeit zwischen dem *Typ* in einer `is-expression` und *constant_pattern*, von denen jede eine gültige Analyse eines qualifizierten Bezeichners sein könnte. Wir versuchen, Sie als Typ für die Kompatibilität mit früheren Versionen der Sprache zu binden. nur wenn dies nicht möglich ist, lösen wir es wie in anderen Kontexten, bis zum ersten gefundenen (das entweder eine Konstante oder ein Typ sein muss). Diese Mehrdeutigkeit ist nur auf der rechten Seite eines `is` Ausdrucks vorhanden.

### <a name="declaration-pattern"></a>Deklarations Muster

Der *declaration_pattern* testet, ob ein Ausdruck einen bestimmten Typ hat, und wandelt ihn in diesen Typ um, wenn der Test erfolgreich ist. Wenn die *simple_designation* ein Bezeichner ist, wird eine lokale Variable des angegebenen Typs eingeführt, der durch den angegebenen Bezeichner benannt wird. Diese lokale Variable ist *definitiv zugewiesen* , wenn das Ergebnis der Muster Vergleichsoperation "true" ist.

```antlr
declaration_pattern
    : type simple_designation
    ;
```

Die Lauf Zeit Semantik dieses Ausdrucks besteht darin, dass er den Lauf Zeittyp des linken *relational_expression* Operanden mit dem *Typ* im Muster testet. Wenn es sich um einen Lauf Zeittyp (oder einen Untertyp) handelt, wird das Ergebnis des `is operator` `true`. Er deklariert eine neue lokale Variable namens durch den *Bezeichner* , dem der Wert des linken Operanden zugewiesen wird, wenn das Ergebnis `true`ist.

Bestimmte Kombinationen von statischem Typ der linken Seite und des angegebenen Typs werden als nicht kompatibel betrachtet und führen zu einem Kompilierzeitfehler. Ein Wert vom Typ "statischer Typ `E`" ist mit dem Typ " *Muster kompatibel* " `T`, wenn eine Identitäts Konvertierung, eine implizite Verweis Konvertierung, eine Boxing-Konvertierung, eine explizite Verweis Konvertierung oder eine Unboxing-Konvertierung von `E` in `T`vorhanden ist. Es handelt sich um einen Kompilierzeitfehler, wenn ein Ausdruck vom Typ `E` nicht mit dem Typ in einem Typmuster kompatibel ist, mit dem er übereinstimmt.

> Hinweis: [in C# 7,1 erweitern wir dies](../csharp-7.1/generics-pattern-match.md) , um einen Muster Vergleichs Vorgang zuzulassen, wenn entweder der Eingabetyp oder der Typ `T` ein offener Typ ist. Dieser Absatz wird durch Folgendes ersetzt:
> 
> Bestimmte Kombinationen von statischem Typ der linken Seite und des angegebenen Typs werden als nicht kompatibel betrachtet und führen zu einem Kompilierzeitfehler. Ein Wert vom Typ "statischer Typ `E`" ist mit dem Typ " *Muster kompatibel* " `T`, wenn eine Identitäts Konvertierung, eine implizite Verweis Konvertierung, eine Boxing-Konvertierung, eine explizite Verweis Konvertierung oder eine Unboxing-Konvertierung von `E` in `T`vorhanden **ist, oder wenn entweder `E` oder `T` ein offener Typ ist**. Es handelt sich um einen Kompilierzeitfehler, wenn ein Ausdruck vom Typ `E` nicht mit dem Typ in einem Typmuster kompatibel ist, mit dem er übereinstimmt.

Das Deklarations Muster ist nützlich zum Ausführen von Lauf Zeittyp Tests von Verweis Typen und ersetzt die Ausdrucksweise.

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

Mit etwas präziseren

```csharp
if (expr is Type v) { // code using v }
```

Es ist ein Fehler, wenn der *Typ* ein Werte zulässt-Werttyp ist.

Das Deklarations Muster kann verwendet werden, um Werte von Typen zu testen, die NULL-Werte zulassen: ein Wert vom Typ `Nullable<T>` (oder ein gebocktes `T`) entspricht einem Typmuster `T2 id` wenn der Wert nicht NULL ist und der Typ der `T2` `T`oder ein Basistyp oder eine Schnittstelle `T`ist. Beispielsweise im Code Fragment

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

Die Bedingung der `if`-Anweisung wird zur Laufzeit `true`, und die Variable `v` enthält den Wert `3` vom Typ `int` innerhalb des Blocks.

### <a name="constant-pattern"></a>Konstantenmuster

```antlr
constant_pattern
    : shift_expression
    ;
```

Ein konstantes Muster testet den Wert eines Ausdrucks mit einem konstanten Wert. Die Konstante kann ein beliebiger konstanter Ausdruck sein, z. b. ein Literalwert, der Name einer deklarierten `const` Variablen oder eine Enumerationskonstante oder ein `typeof` Ausdruck.

Wenn sowohl *e* als auch *c* ganzzahlige Typen sind, wird das Muster als Übereinstimmung betrachtet, wenn das Ergebnis des Ausdrucks `e == c` `true`ist.

Andernfalls wird das Muster als übereinstimmend betrachtet, wenn `object.Equals(e, c)` `true`zurückgibt. In diesem Fall handelt es sich um einen Kompilierzeitfehler, wenn der statische Typ von *e* nicht mit dem Typ der Konstante *kompatibel* ist.

### <a name="var-pattern"></a>Var-Muster

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

Ein Ausdruck *e* stimmt mit einem *var_pattern* immer überein. Anders ausgedrückt, eine Entsprechung für ein *var-Muster* ist immer erfolgreich. Wenn die *simple_designation* ein Bezeichner ist, wird zur Laufzeit der Wert von *e* an eine neu eingeführte lokale Variable gebunden. Der Typ der lokalen Variablen ist der statische Typ von *e*.

Wenn der Name `var` an einen Typ bindet, ist ein Fehler aufgetreten.

## <a name="switch-statement"></a>switch-Anweisung

Die `switch`-Anweisung wird erweitert, um für die Ausführung auszuwählen. der erste Block verfügt über ein zugeordnetes Muster, das dem *Switch-Ausdruck*entspricht

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

Die Reihenfolge, in der Muster abgeglichen werden, ist nicht definiert. Ein Compiler ist berechtigt, Muster in falscher Reihenfolge abzugleichen und die Ergebnisse von bereits übereinstimmenden Mustern wiederzuverwenden, um das Ergebnis der Übereinstimmung von anderen Mustern zu berechnen.

Wenn ein *Case-Guard* vorhanden ist, ist der Ausdruck vom Typ `bool`. Es wird als zusätzliche Bedingung ausgewertet, die erfüllt werden muss, damit die Groß-/Kleinschreibung als erfüllt betrachtet wird.

Es tritt ein Fehler auf, wenn ein *switch_label* zur Laufzeit keine Auswirkung haben kann, da sein Muster in vorherigen Fällen subsudiert wird. [TODO: Wir sollten präziser zu den Techniken sein, die der Compiler zum Erreichen dieses Urteils benötigt.]

Eine in einem *switch_label* deklarierte Muster Variable wird in Ihrem Fall Block definitiv zugewiesen, wenn dieser Fall Block genau einen *switch_label*enthält.

[TODO: Wir sollten angeben, wann ein *switch-Block* erreichbar ist.]

### <a name="scope-of-pattern-variables"></a>Bereich von Muster Variablen

Der Gültigkeitsbereich einer Variablen, die in einem Muster deklariert ist, lautet wie folgt:

- Wenn das Muster eine Case-Bezeichnung ist, ist der Gültigkeitsbereich der Variablen der *Case-Block*.

Andernfalls wird die Variable in einem *is_pattern* Ausdruck deklariert, und ihr Bereich basiert auf dem-Konstrukt, das den Ausdruck, der den *is_pattern* Ausdruck enthält, unmittelbar wie folgt umschließt:

- Wenn sich der Ausdruck in einem Ausdrucks Körper-Lambda befindet, ist sein Bereich der Text des Lambda-Ausdrucks.
- Wenn sich der Ausdruck in einer Ausdrucks Körper-Methode oder-Eigenschaft befindet, ist sein Bereich der Text der Methode oder Eigenschaft.
- Wenn sich der Ausdruck in einer `when`-Klausel einer `catch`-Klausel befindet, ist der Gültigkeitsbereich die `catch`-Klausel.
- Wenn sich der Ausdruck in einem *iteration_statement*befindet, ist sein Bereich nur diese Anweisung.
- Andernfalls ist der Gültigkeitsbereich der Bereich, der die Anweisung enthält, wenn sich der Ausdruck in einem anderen Anweisungs Formular befindet.

Zum Ermitteln des Bereichs wird ein *embedded_statement* als in seinem eigenen Bereich betrachtet. Beispielsweise ist die Grammatik für eine *if_statement*

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Wenn die gesteuerte Anweisung einer *if_statement* eine Muster Variable deklariert, ist der Gültigkeitsbereich auf diese *embedded_statement*beschränkt:

```csharp
if (x) M(y is var z);
```

In diesem Fall ist der Bereich `z` die eingebettete Anweisung `M(y is var z);`.

Andere Fälle sind Fehler aus anderen Gründen (z. b. im Standardwert eines Parameters oder in einem Attribut, bei denen es sich um einen Fehler handelt, da für diese Kontexte ein konstanter Ausdruck erforderlich ist).

> [In C# 7,3 haben wir die folgenden Kontexte hinzugefügt](../csharp-7.3/expression-variables-in-initializers.md) , in denen eine Muster Variable deklariert werden kann:
> - Wenn sich der Ausdruck in einem *Konstruktorinitialisierer*befindet, ist sein Bereich der *Konstruktorinitialisierer* und der Text des Konstruktors.
> - Wenn sich der Ausdruck in einem Feldinitialisierer befindet, ist sein Bereich der *equals_value_clause* , in dem er angezeigt wird.
> - Wenn der Ausdruck in einer Abfrage Klausel enthalten ist, die angegeben wird, dass Sie in den Text eines Lambda-Ausdrucks übersetzt werden soll, ist der Gültigkeitsbereich nur dieser Ausdruck.

## <a name="changes-to-syntactic-disambiguation"></a>Änderungen an syntaktischer Eindeutigkeit

Es gibt Situationen, in denen die C# Grammatik mehrdeutig ist, und die Sprachspezifikation gibt an, wie diese Mehrdeutigkeiten gelöst werden können:

> #### <a name="7652-grammar-ambiguities"></a>7.6.5.2 Grammatik Mehrdeutigkeiten
> Die Produktion für *einfachen Namen* ("7.6.3") und " *Member-Access* " (§ 7.6.5) kann zu Mehrdeutigkeiten in der Grammatik für Ausdrücke führen. Beispielsweise ist die-Anweisung:
> ```csharp
> F(G<A,B>(7));
> ```
> kann als `F` mit zwei Argumenten interpretiert werden, `G < A` und `B > (7)`. Alternativ könnte Sie auch als `F` mit einem Argument interpretiert werden, bei dem es sich um einen aufzurufenden generischen Methoden `G` mit zwei Typargumenten und einem regulären Argument handelt.

> Wenn eine Sequenz von Token (im Kontext) als *Simple-Name* (7.6.3), Element *-Access* (§ 7.6.5) oder *Pointer-Member-Access* (§ 18.5.2) analysiert werden kann, die mit einer *Type-Argument-List* (§ 4.4.1) endet, wird das Token, das unmittelbar auf das schließende `>` Token folgt, untersucht. Wenn eine von
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> Anschließend wird die *Type-Argument-List* als Teil des *Simple-namens*, des *Mitglieds Zugriffs* oder des *Zeigers für den Zeiger Zugriff* aufbewahrt, und jede andere mögliche Analyse der Sequenz von Token wird verworfen. Andernfalls wird die *Type-Argument-List* nicht als Teil des *Simple-namens*, des *Member-Access* -oder > *Pointer-Member-Access*betrachtet, auch wenn es keine andere Möglichkeit gibt, die Sequenz von Token zu analysieren. Beachten Sie, dass diese Regeln beim Parsen einer *Type-Argument-List* in einem *Namespace-oder-Type-Name* (§ 3,8) nicht angewendet werden. Die Anweisung
> ```csharp
> F(G<A,B>(7));
> ```
> wird gemäß dieser Regel als ein `F` mit einem Argument interpretiert, bei dem es sich um einen aufzurufenden generischen Methoden `G` mit zwei Typargumenten und einem regulären Argument handelt. Die Anweisungen
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> wird als `F` mit zwei Argumenten interpretiert. Die Anweisung
> ```csharp
> x = F < A > +y;
> ```
> wird als ein kleiner-als-Operator, größer als-Operator und Unärer Plus-Operator interpretiert, als wäre die Anweisung `x = (F < A) > (+y)`geschrieben worden, anstatt als *einfacher Name* mit einer *Type-Argument-List* gefolgt von einem binären Plus-Operator. In der-Anweisung
> ```csharp
> x = y is C<T> + z;
> ```
> die Token `C<T>` werden als *Namespace-oder-Typname* mit einer *Type-Argument-List*interpretiert.

Es gibt eine Reihe von Änderungen, die in C# 7 eingeführt werden, die diese mehrdeutigkeits Regeln nicht mehr ausreichen, um die Komplexität der Sprache zu verarbeiten.

### <a name="out-variable-declarations"></a>out-Variablendeklaration

Es ist nun möglich, eine Variable in einem out-Argument zu deklarieren:

```csharp
M(out Type name);
```

Der Typ kann jedoch generisch sein: 

```csharp
M(out A<B> name);
```

Da die Sprachgrammatik für das-Argument *Expression*verwendet, unterliegt dieser Kontext der disambiguations-Regel. In diesem Fall folgt dem schließenden `>` ein *Bezeichner*, bei dem es sich nicht um eines der Token handelt, das es ermöglicht, als *Type-Argument-List*behandelt zu werden. Ich schlage daher vor, **einen *Bezeichner* zu dem Satz von Token hinzuzufügen, der die Eindeutigkeit einer *Type-Argument-List*auslöst.**

### <a name="tuples-and-deconstruction-declarations"></a>Tupel-und Debug-Deklarationen

Ein tupelliteral wird genau das gleiche Problem haben. Beachten Sie den Tupelausdruck.

```csharp
(A < B, C > D, E < F, G > H)
```

Unter den alten C# 6 Regeln zum Analysieren einer Argumentliste würde dies als Tupel mit vier Elementen analysiert werden, beginnend mit `A < B` als ersten. Wenn dies jedoch auf der linken Seite einer Debug-Datei angezeigt wird, soll die Unterscheidung wie oben beschrieben durch das *Bezeichnertoken* ausgelöst werden:

```csharp
(A<B,C> D, E<F,G> H) = e;
```

Dies ist eine Dekonstruktions Deklaration, die zwei Variablen deklariert, wobei der erste vom Typ `A<B,C>` und der benannte `D`ist. Das heißt, das tupelliterale enthält zwei Ausdrücke, von denen jeder ein Deklarations Ausdruck ist.

Aus Gründen der Einfachheit der Spezifikation und des Compilers schlage ich vor, dass dieses tupelliterals ein Tupel mit zwei Elementen analysiert wird, wo es angezeigt wird (unabhängig davon, ob es auf der linken Seite einer Zuweisung angezeigt wird). Dies wäre ein natürliches Ergebnis der Eindeutigkeit, die im vorherigen Abschnitt beschrieben wurde.

### <a name="pattern-matching"></a>Muster Vergleich

Der Muster Vergleich führt einen neuen Kontext ein, bei dem die Ausdrucks typmehrdeutigkeit auftritt. Zuvor war die Rechte Seite eines `is` Operators ein Typ. Nun kann es sich um einen Typ oder einen Ausdruck handeln, und wenn es sich um einen Typ handelt, kann ein Bezeichner folgen. Dies kann technisch gesehen die Bedeutung von vorhandenem Code ändern:

```csharp
var x = e is T < A > B;
```

Diese kann unter c# 6-Regeln analysiert werden als

```csharp
var x = ((e is T) < A) > B;
```

unter "c# 7"-Regeln (mit der oben vorgeschlagenen Mehrdeutigkeit) werden jedoch als

```csharp
var x = e is T<A> B;
```

deklariert eine Variable `B` vom Typ `T<A>`. Glücklicherweise haben die systemeigenen und Roslyn-Compiler einen Fehler, wenn Sie einen Syntax Fehler im c# 6-Code verursachen. Daher ist diese spezielle Breaking Change kein Problem.

Bei Pattern-Matching werden zusätzliche Token eingeführt, die die mehrdeutigkeitsauflösung bei der Auswahl eines Typs steuern. Die folgenden Beispiele für vorhandenen gültigen c# 6-Code werden ohne zusätzliche mehrdeutigkeits Regeln getrennt:

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a>Vorgeschlagene Änderung der Eindeutigkeits Regel

Ich schlage vor, die Spezifikation zu überarbeiten, um die Liste der mehrdeutigkeits Token zu ändern.

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

An

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

In bestimmten Kontexten behandeln wir den *Bezeichner* als Eindeutigkeits Token. In diesen Kontexten wird der Sequenz von Token, die unterschieden werden, unmittelbar eines der Schlüsselwörter `is`, `case`oder `out`vorangestellt oder beim Parsen des ersten Elements eines tupelliterals vorangestellt (in diesem Fall werden den Token `(` oder `:` vorangestellt, und der Bezeichner folgt einem `,`) oder einem nachfolgenden Element eines tupelliterals.

### <a name="modified-disambiguation-rule"></a>Geänderte disambiguations-Regel

Die überarbeitete Regel für die Mehrdeutigkeit würde etwa wie folgt aussehen.

> Wenn eine Sequenz von Token (im Kontext) als *Simple-Name* (7.6.3), Element *-Access* (§ 7.6.5) oder *Pointer-Member-Access* (§ 18.5.2) analysiert werden kann, die mit einer *Type-Argument-List* (§ 4.4.1) endet, wird das Token, das unmittelbar auf das schließende `>` Token folgt, untersucht, um festzustellen, ob es
> - Eines der `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; noch
> - Einer der relationalen Operatoren `<  >  <=  >=  is as`; noch
> - Ein Kontext abhängiges Abfrage Schlüsselwort in einem Abfrage Ausdruck. noch
> - In bestimmten Kontexten behandeln wir den *Bezeichner* als Eindeutigkeits Token. In diesen Kontexten wird der Sequenz von Token, die unterschieden werden, unmittelbar eines der Schlüsselwörter `is`, `case` oder `out`, oder beim Parsen des ersten Elements eines tupelliterals vorangestellt. (in diesem Fall werden den Token `(` oder `:` vorangestellt, und der Bezeichner folgt einem `,`) oder einem nachfolgenden Element eines tupelliterals.
> 
> Wenn das folgende Token in dieser Liste enthalten ist, oder ein Bezeichner in einem solchen Kontext, wird die *Type-Argument-List* als Teil des *Simple-namens*, des Element *Zugriffs* oder des *Zeigers des Zeiger* Members beibehalten, und jede andere mögliche Analyse der Sequenz von Token wird verworfen.  Andernfalls wird die *Type-Argument-List* nicht als Bestandteil des *Simple-namens*, des Element *Zugriffs* oder des *Zeigers für Zeiger*Elemente angesehen, auch wenn es keine andere Möglichkeit gibt, die Sequenz von Token zu analysieren. Beachten Sie, dass diese Regeln beim Parsen einer *Type-Argument-List* in einem *Namespace-oder-Type-Name* (§ 3,8) nicht angewendet werden.

### <a name="breaking-changes-due-to-this-proposal"></a>Wichtige Änderungen aufgrund dieses Angebots

Aufgrund dieser vorgeschlagenen mehrdeutigkeits Regel sind keine wichtigen Änderungen bekannt.

### <a name="interesting-examples"></a>Interessante Beispiele

Im folgenden finden Sie einige interessante Ergebnisse dieser mehrdeutigkeits Regeln:

Der Ausdruck `(A < B, C > D)` ist ein Tupel mit zwei Elementen, jeweils ein Vergleich.

Der Ausdruck `(A<B,C> D, E)` ist ein Tupel mit zwei Elementen, wobei der erste ein Deklarations Ausdruck ist.

Der Aufruf `M(A < B, C > D, E)` hat drei Argumente.

Der Aufruf `M(out A<B,C> D, E)` hat zwei Argumente, wobei der erste eine `out` Deklaration ist.

Der Ausdrucks `e is A<B> C` verwendet einen Deklarations Ausdruck.

Die Case-Bezeichnung `case A<B> C:` verwendet einen Deklarations Ausdruck.

## <a name="some-examples-of-pattern-matching"></a>Einige Beispiele für den Musterabgleich

### <a name="is-as"></a>Ist-As

Wir können die Ausdrucksweise ersetzen.

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

Mit etwas präziseren und direkteren

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a>Testen von NULL-Werten

Wir können die Ausdrucksweise ersetzen.

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

Mit etwas präziseren und direkteren

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a>Arithmetische Vereinfachung

Angenommen, wir definieren eine Reihe von rekursiven Typen zur Darstellung von Ausdrücken (gemäß einem separaten Vorschlag):

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

Nun können wir eine Funktion definieren, um die (nicht reduzierte) Ableitung eines Ausdrucks zu berechnen:

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

Ein Ausdrucks vereinfachende zeigt Positions Muster:

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
