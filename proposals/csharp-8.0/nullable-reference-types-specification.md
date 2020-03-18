---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2020
ms.locfileid: "79484105"
---
# <a name="nullable-reference-types-specification"></a>Verweis Typen Spezifikation für Nullwerte zulassen

Dies ist eine laufende Arbeit. mehrere Teile fehlen oder sind unvollständig. ***

## <a name="syntax"></a>Syntax

### <a name="nullable-reference-types"></a>Nullwerte zulassende Verweistypen

Verweis Typen, die NULL-Werte zulassen, haben dieselbe Syntax `T?` wie die Kurzform von Werttypen, die NULL-Werte zulassen, aber nicht über eine entsprechende lange Form verfügen.

Zum Zweck der Spezifikation wird die aktuelle `nullable_type` Produktion in `nullable_value_type`umbenannt, und es wird eine `nullable_reference_type` Produktion hinzugefügt:

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

Der `non_nullable_reference_type` in einem `nullable_reference_type` muss ein Verweistyp sein, der keine NULL-Werte zulässt (Klasse, Schnittstelle, Delegat oder Array), oder ein Typparameter, der auf einen Verweistyp beschränkt ist, der keine NULL-Werte zulässt (durch die `class`-Einschränkung oder eine andere Klasse als `object`).

Verweis Typen, die NULL-Werte zulassen, können nicht in folgenden Positionen auftreten:

- als Basisklasse oder Schnittstelle
- als Empfänger eines `member_access`
- als `type` in einer `object_creation_expression`
- als `delegate_type` in einer `delegate_creation_expression`
- als `type` in einer `is_expression`, eine `catch_clause` oder eine `type_pattern`
- als `interface` in einem voll qualifizierten Schnittstellenmember-Namen

Eine Warnung wird bei einer `nullable_reference_type` angegeben, bei der der Anmerkung-Kontext der Nullwerte deaktiviert ist.

### <a name="nullable-class-constraint"></a>NULL-Werte zulassen-Klassen Einschränkung

Die `class`-Einschränkung verfügt über ein `class?`, das NULL-Werte zulässt:

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a>Der NULL-Forgiving-Operator

Der postfix `!`-Operator wird als NULL-forder Operator bezeichnet.

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

Der `primary_expression` muss ein Verweistyp sein.  

Der postfix-`!` Operator hat keinen Lauf Zeiteffekt und wertet das Ergebnis des zugrunde liegenden Ausdrucks aus. Die einzige Rolle besteht darin, den NULL-Status des Ausdrucks zu ändern und die bei der Verwendung angegebenen Warnungen einzuschränken.

### <a name="nullable-implicitly-typed-local-variables"></a>implizit typisierte lokale Variablen, die NULL zulassen

`var` leitet einen Typ mit Anmerkungen für Verweis Typen ab.
Beispielsweise wird in `var s = "";` die `var` als `string?`abgeleitet.

### <a name="nullable-compiler-directives"></a>Nullable-Compilerdirektiven

`#nullable` Direktiven steuern den Kontext, der auf NULL festgelegt werden kann, und Warnungen.

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

`#pragma warning` Direktiven werden erweitert, um das Ändern des Warnungs Kontexts, der NULL-Werte zulässt, und das Zulassen von einzelnen Warnungen auch dann zuzulassen, wenn Sie standardmäßig deaktiviert sind:

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

Beachten Sie, dass die neue Form von `pragma_warning_body` `nullable_action`verwendet, nicht `warning_action`.

## <a name="nullable-contexts"></a>Nullable-Kontexte

Jede Zeile des Quellcodes enthält einen *Anmerkung-Kontext mit NULL-Werten* und einen *Warnungs Kontext*, der NULL-Werte zulässt. Diese Steuern, ob auf NULL festleg Bare Anmerkungen wirksam sind und ob NULL-Zulässigkeit-Warnungen angegeben werden. Der Anmerkung-Kontext einer bestimmten Zeile ist entweder *deaktiviert* oder *aktiviert*. Der Warnungs Kontext einer bestimmten Zeile ist entweder *deaktiviert* oder *aktiviert*.

Beide Kontexte können auf Projektebene (außerhalb des C# Quellcodes) oder an einer beliebigen Stelle innerhalb einer Quelldatei über `#nullable` Präprozessordirektiven angegeben werden. Wenn keine Einstellungen auf Projektebene bereitgestellt werden, ist der Standardwert, wenn beide Kontexte *deaktiviert*werden.

Die `#nullable`-Direktive steuert die-Anmerkung und die Warnungs Kontexte innerhalb des Quell Texts und hat Vorrang vor den Einstellungen auf Projektebene.

Eine-Direktive legt die für nachfolgende Codezeilen fest zufügende Kontexts fest, bis Sie von einer anderen Direktive überschrieben werden, oder bis zum Ende der Quelldatei.

Die-Direktiven haben folgende Auswirkung:

- `#nullable disable`: legt die Anmerkung und den Warnungs Kontexte der Nullwerte auf " *deaktiviert* " fest
- `#nullable enable`: legt fest, dass die Anmerkung und der Warnungs Kontexte der Nullwerte *aktiviert* werden
- `#nullable restore`: stellt die Anmerkung und den Warnungs Kontexte der NULL-Werte auf Projekteinstellungen wieder her
- `#nullable disable annotations`: legt den Kontext der Anmerkung auf " *deaktiviert* " fest.
- `#nullable enable annotations`: legt den Kontext der Anmerkung, der NULL zulässt, auf *aktiviert* fest
- `#nullable restore annotations`: stellt den Kontext der Anmerkung mit NULL-Werten in den Projekteinstellungen wieder her
- `#nullable disable warnings`: legt den Warnungs Kontext, der NULL zulässt, auf *deaktiviert* fest
- `#nullable enable warnings`: legt den Warnungs Kontext, der NULL zulässt, auf *aktiviert* fest
- `#nullable restore warnings`: Wiederherstellen des Warnungs Kontexts, der NULL-Werte zulässt,

## <a name="nullability-of-types"></a>NULL-Zulässigkeit von Typen

Ein bestimmter Typ kann eine von vier NULL-Fähigkeiten aufweisen *:* *nicht*zulässig, NULL *-Werte zulassen* und *unbekannt*. 

*Nicht auf NULL* festleg Bare und *unbekannte* Typen können Warnungen auslösen, wenn Ihnen ein potenzieller `null` Wert zugewiesen wird. Typen, die*null-* Werte zulassen, sind jedoch "NULL *zustellbar* *" und können* `null` Werte ohne Warnungen zugewiesen werden. 

*Oblivious* *Nicht auf NULL* festleg Bare Typen können ohne Warnungen dereferenziert oder zugewiesen werden. Werte von *Werte zulässt* -Typen und *unbekannten* Typen sind jedoch "*null-* Ergebnisse" und können Warnungen verursachen, wenn Sie ohne entsprechende NULL-Überprüfung dereferenziert oder zugewiesen werden. 

Der *standardmäßige NULL-Status* eines Typs, der NULL ergibt, ist "vielleicht NULL". Der standardmäßige NULL-Status eines Typs, der keine NULL-Werte ergibt, ist "not NULL".

Die Art des Typs und der Kontext, der NULL-Werte zulässt, in dem er auftritt, um seine NULL-Zulässigkeit zu ermitteln:

- Ein Werttyp, der keine NULL-Werte zulässt, *`S` immer NULL-Werte zulässt*
- Ein Werttyp, der auf NULL festgelegt werden kann `S?` immer *null*
- Ein referenzter Verweistyp `C` in einem *deaktivierten* Anmerkung-Kontext ist nicht *sichtbar* .
- Ein Referenztyp, der in einem *aktivierten* Anmerkung-Kontext `C` ist, lässt keine *NULL-Werte* zu.
- Ein Verweistyp, der NULL-Werte zulässt `C?` *NULL-Werte* zulässt (es kann jedoch eine Warnung in einem *deaktivierten* Anmerkung-Kontext ausgegeben werden)

Typparameter berücksichtigen zusätzlich die Einschränkungen:

- Ein Typparameter `T`, bei dem alle Einschränkungen (sofern vorhanden) entweder NULL-Werte (*Werte zulässt* und *Unknown*) oder die `class?` Einschränkung *unbekannt* sind.
- Ein Typparameter `T`, bei dem mindestens eine Einschränkung entweder nicht zulässig ist oder keine *NULL-Werte* zulässt oder eine der `struct`-oder `class` Einschränkungen ist. *oblivious*
    - nicht *in einem* *deaktivierten* Anmerkung-Kontext.
    - *nicht auf NULL festlegbar* in einem *aktivierten* Anmerkung-Kontext
- Ein Typparameter, der NULL-Werte zulässt `T?` ist, wenn mindestens eine der Einschränkungen von `T`*nicht* *zulässig oder eine* der `struct`-oder `class` Einschränkungen ist.
    - *NULL-Werte* in einem *deaktivierten* Anmerkung-Kontext (es wird jedoch eine Warnung ausgegeben)
    - *NULL-Werte* in *aktiviertem* Anmerkung-Kontext

Bei einem Typparameter `T`ist `T?` nur zulässig, wenn `T` bekanntermaßen ein Werttyp oder bekannt ist, dass es sich um einen Verweistyp handelt.

### <a name="oblivious-vs-nonnullable"></a>Schädliche und nicht auf NULL festleg Bare Werte

Ein `type` wird in einem gegebenen Anmerkung-Kontext als auftreten betrachtet, wenn das letzte Token des Typs innerhalb dieses Kontexts liegt.

Ob ein angegebener Verweistyp, der im Quellcode `C` wird, als nicht zulässig oder nicht NULL-Werte interpretiert wird, hängt vom Anmerkung-Kontext dieses Quellcodes ab. Nachdem er festgelegt wurde, wird er als Teil dieses Typs angesehen und mit ihm "bewegt", z. b. während der Ersetzung von generischen Typargumenten. Es ist so, als ob eine Anmerkung wie `?` für den Typ vorhanden ist, aber unsichtbar ist.

## <a name="constraints"></a>Einschränkungen

Verweis Typen, die NULL-Werte zulassen, können als generische Einschränkungen verwendet werden. Darüber hinaus ist `object` jetzt als explizite Einschränkung gültig. Das Fehlen einer Einschränkung entspricht nun einer `object?` Einschränkung (statt `object`), aber (im Gegensatz `object` vor) `object?` nicht als explizite Einschränkung zulässig.

`class?` ist eine neue Einschränkung, die "möglicherweise Werte zulässt Reference Type" angibt, wohingegen `class` "Verweis Typen, die keine NULL-Werte zulassen" bezeichnet.

Die NULL-Zulässigkeit eines Typarguments oder einer Einschränkung wirkt sich nicht darauf aus, ob der Typ die Einschränkung erfüllt, es sei denn, dies ist bereits der Fall, da die Werttypen, die NULL-Werte zulassen, die `struct` Einschränkung nicht erfüllen). Wenn das Typargument jedoch die Anforderungen für die NULL-Zulässigkeit der Einschränkung nicht erfüllt, kann eine Warnung ausgegeben werden.

## <a name="null-state-and-null-tracking"></a>NULL-Status und NULL-Nachverfolgung

Jeder Ausdruck an einem angegebenen Quell Speicherort weist einen *NULL-Status*auf, der angibt, ob der Wert potenziell NULL ergibt. Der NULL-Status ist entweder "not NULL" oder "vielleicht NULL". Der NULL-Status wird verwendet, um zu bestimmen, ob eine Warnung über Null-unsichere Konvertierungen und Dereferenzierungen ausgegeben werden soll.

### <a name="null-tracking-for-variables"></a>NULL-Nachverfolgung für Variablen

Für bestimmte Ausdrücke, die Variablen oder Eigenschaften kennzeichnen, wird der NULL-Status zwischen den vorkommen nachverfolgt, basierend auf den zugewiesenen Zuweisungen, den darauf ausgeführten Tests und der Ablauf Steuerung zwischen den vorkommen. Dies ähnelt der Art und Weise, wie eine definitive Zuweisung für Variablen nachverfolgt wird. Die nach verfolgten Ausdrücke sind in der folgenden Form:

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

, Wobei die Bezeichner Felder oder Eigenschaften kennzeichnen.

***Beschreiben von NULL-Status Übergängen, die einer bestimmten Zuweisung ähneln***

### <a name="null-state-for-expressions"></a>NULL-Status für Ausdrücke

Der NULL-Status eines Ausdrucks wird von seinem Formular und Typ und vom Null-Zustand der an ihm beteiligten Variablen abgeleitet.

### <a name="literals"></a>Literale

Der NULL-Status eines `null` Literals ist "vielleicht NULL". Der NULL-Status eines `default` Literals, das in einen Typ konvertiert wird, dem bekannt ist, dass es sich um einen Werttyp handelt, der nicht NULL sein kann, ist "vielleicht NULL". Der NULL-Status eines beliebigen anderen Literals ist "not NULL".

### <a name="simple-names"></a>Einfache Namen

Wenn ein `simple_name` nicht als Wert klassifiziert wird, ist sein NULL-Status "not NULL". Andernfalls handelt es sich um einen nach verfolgten Ausdruck, und sein NULL-Status ist der überwachte NULL-Status an diesem Quell Speicherort.

### <a name="member-access"></a>Memberzugriff

Wenn ein `member_access` nicht als Wert klassifiziert wird, ist sein NULL-Status "not NULL". Andernfalls ist der NULL-Status bei einem überwachten Ausdruck der überwachte NULL-Status an diesem Quell Speicherort. Andernfalls ist der NULL-Status der standardmäßige NULL-Status seines Typs.

### <a name="invocation-expressions"></a>Aufrufausdrücke

Wenn eine `invocation_expression` einen Member aufruft, der mit einem oder mehreren Attributen für ein spezielles NULL-Verhalten deklariert ist, wird der NULL-Status durch diese Attribute bestimmt. Andernfalls ist der NULL-Status des Ausdrucks der standardmäßige NULL-Status seines Typs.

### <a name="element-access"></a>Element Zugriff

Wenn ein `element_access` einen Indexer aufruft, der mit einem oder mehreren Attributen für ein spezielles NULL-Verhalten deklariert ist, wird der NULL-Status durch diese Attribute bestimmt. Andernfalls ist der NULL-Status des Ausdrucks der standardmäßige NULL-Status seines Typs.

### <a name="base-access"></a>Basis Zugriff

Wenn `B` den Basistyp des einschließenden Typs angibt, hat `base.I` denselben NULL-Status wie `((B)this).I`, und `base[E]` hat denselben NULL-Status wie `((B)this)[E]`.

### <a name="default-expressions"></a>Standardausdrücke

`default(T)` weist den NULL-Status "nicht-NULL" auf, wenn bekannt ist, dass `T` ein Werttyp ist, der keine NULL-Werte zulässt. Andernfalls hat Sie den NULL-Status "vielleicht NULL".

### <a name="null-conditional-expressions"></a>NULL bedingte Ausdrücke

Ein `null_conditional_expression` hat den NULL-Status "vielleicht NULL".

### <a name="cast-expressions"></a>Umwandlungs Ausdrücke

Wenn ein Cast Ausdruck `(T)E` eine benutzerdefinierte Konvertierung aufruft, ist der NULL-Status des Ausdrucks der standardmäßige NULL-Status für seinen Typ. Andernfalls ist der NULL-Status "vielleicht NULL", wenn `T` NULL ergibt (*Werte zulässt* oder *Unknown*). Andernfalls ist der NULL-Status mit dem NULL-Status `E`identisch.

### <a name="await-expressions"></a>Erwartungs Ausdrücke

Der NULL-Status `await E` ist der standardmäßige NULL-Status seines Typs.

### <a name="the-as-operator"></a>Der `as`-Operator

Ein `as` Ausdruck hat den NULL-Status "vielleicht NULL".

### <a name="the-null-coalescing-operator"></a>Der NULL-Sammel Operator

`E1 ?? E2` hat denselben NULL-Status wie `E2`

### <a name="the-conditional-operator"></a>Der bedingte Operator

Der NULL-Status `E1 ? E2 : E3` ist "not NULL", wenn der NULL-Status von `E2` und `E3` "not NULL" ist. Andernfalls ist es "vielleicht NULL".

### <a name="query-expressions"></a>Abfrageausdrücke

Der NULL-Status eines Abfrage Ausdrucks ist der standardmäßige NULL-Status seines Typs.

### <a name="assignment-operators"></a>Zuweisungsoperatoren

`E1 = E2` und `E1 op= E2` haben den gleichen NULL-Status wie `E2`, nachdem alle impliziten Konvertierungen angewendet wurden.

### <a name="unary-and-binary-operators"></a>Unäre Operatoren und binäre Operatoren

Wenn ein unärer oder binärer Operator einen benutzerdefinierten Operator aufruft, der mit einem oder mehreren Attributen für ein spezielles NULL-Verhalten deklariert ist, wird der NULL-Status durch diese Attribute bestimmt. Andernfalls ist der NULL-Status des Ausdrucks der standardmäßige NULL-Status seines Typs.

***Was ist für binäre `+` über Zeichen folgen und Delegaten etwas Besonderes zu tun?***

### <a name="expressions-that-propagate-null-state"></a>Ausdrücke, die einen NULL-Status

`(E)`, `checked(E)` und `unchecked(E)` alle denselben NULL-Status aufweisen wie `E`.

### <a name="expressions-that-are-never-null"></a>Ausdrücke, die nie NULL sind

Der NULL-Status der folgenden Ausdrucks Formulare ist immer "not NULL":

- `this` -Zugriff
- interpoliert Zeichen folgen
- `new` Ausdrücke (Objekt-, Delegat-, anonyme Objekt-und Array Erstellungs Ausdrücke)
- `typeof`-Ausdrücke
- `nameof`-Ausdrücke
- Anonyme Funktionen (anonyme Methoden und Lambda-Ausdrücke)
- NULL-Ausdrucks Ausdrücke
- `is`-Ausdrücke

## <a name="type-inference"></a>Typrückschluss

### <a name="type-inference-for-var"></a>Typrückschluss für `var`

Der Typ, der für lokale Variablen abgeleitet wird, die mit `var` deklariert werden, wird durch den NULL-Status des Initialisierungs Ausdrucks informiert.

```csharp
var x = E;
```

Wenn der Typ des `E` ein Verweistyp ist, der NULL-Werte zulässt `C?` und der NULL-Status von `E` "not NULL" ist, wird der für `x` herleitet Typ `C`. Andernfalls ist der abherleitet Typ der Typ der `E`.

Die NULL-Zulässigkeit des Typs, der für `x` abgeleitet wurde, wird wie oben beschrieben basierend auf dem Anmerkung-Kontext der `var`bestimmt, so als ob der Typ explizit an dieser Position angegeben worden wäre.

### <a name="type-inference-for-var"></a>Typrückschluss für `var?`

Der Typ, der für lokale Variablen abgeleitet wurde, die mit `var?` deklariert werden, ist unabhängig vom NULL-Status des Initialisierungs Ausdrucks.

```csharp
var? x = E;
```

Wenn der Typ `T` `E` ein auf NULL festleg barer Werttyp oder ein Verweistyp ist, der NULL-Werte zulässt, wird der für `x` herleitet Typ `T`. Andernfalls, wenn `T` ein Werttyp ist, der keine NULL-Werte zulässt `S` wird der abherstell Ende Typ `S?`. Andernfalls, wenn `T` ein Referenztyp ist, der keine NULL-Werte zulässt `C` wird der abherzuende Typ `C?`. Andernfalls ist die Deklaration unzulässig.

Die NULL-Zulässigkeit des Typs, der für `x` abgeleitet ist, ist immer *NULL-Werte*zulässig.

### <a name="generic-type-inference"></a>Generischer Typrückschluss

Der generische Typrückschluss wurde verbessert, um zu entscheiden, ob abzurufende Verweis Typen NULL-Werte zulassen sollen oder nicht. Dabei handelt es sich um einen optimalen Aufwand, der nicht in und selbst Warnungen liefert, aber möglicherweise zu Warnungen führt, wenn die abzurufenden Typen der ausgewählten Überladung auf die Argumente angewendet werden.

Der Typrückschluss beruht nicht auf dem Anmerkung-Kontext eingehender Typen. Stattdessen wird ein `type` abgeleitet, das seinen eigenen Anmerkung-Kontext von dem Speicherort abruft, in dem er explizit ausgedrückt wurde. Dies unterstreicht die Rolle des Typrückschlusses als praktische Hilfe für das, was Sie selbst geschrieben hätten.

Genauer ist, dass der Anmerkung-Kontext für ein abhergegriffes Typargument der Kontext des Tokens ist, dem die `<...>`-Typparameter Liste gefolgt wäre, hätte eine vorhanden sein. d. h. der Name der generischen Methode, die aufgerufen wird. Für Abfrage Ausdrücke, die in solche Aufrufe übersetzt werden, wird der Kontext aus dem anfänglichen kontextbezogenen Schlüsselwort der Abfrage Klausel entnommen, von der der Aufruf generiert wird.

### <a name="the-first-phase"></a>Die erste Phase

Verweis Typen, die NULL-Werte zulassen, werden wie unten beschrieben in die Begrenzungen der anfänglichen Ausdrücke übertragen. Außerdem werden zwei neue Arten von Begrenzungen eingeführt, nämlich `null` und `default`. Ihr Zweck besteht darin, Vorkommen von `null` oder `default` in den Eingabe Ausdrücken zu durchlaufen, was dazu führen kann, dass ein abgerichteter Typ NULL-Werte zulässt, auch wenn dies andernfalls nicht der Fall wäre. Dies funktioniert sogar für *Werttypen* , die NULL-Werte zulassen, die verbessert werden, um "nullness" im Rückschluss Prozess zu erfassen.

Die Festlegung, welche Begrenzungen in der ersten Phase hinzugefügt werden sollen, wird wie folgt erweitert:

Wenn ein Argument `Ei` einen Verweistyp aufweist, hängt der Typ `U`, der für den Rückschluss verwendet wird, vom NULL-Status `Ei` und vom deklarierten Typ ab:
- Wenn der deklarierte Typ ein Verweistyp ist, der keine NULL-Werte zulässt `U0` oder ein Verweistyp, der NULL-Werte zulässt `U0?`
    - Wenn der NULL-Status `Ei` "not NULL" ist, wird `U` `U0`
    - Wenn der NULL-Status `Ei` "vielleicht NULL" ist, wird `U` `U0?`
- Andernfalls, wenn `Ei` über einen deklarierten Typ verfügt, ist `U` dieser Typ.
- Andernfalls, wenn `Ei` `null`, ist `U` die spezielle Grenze `null`
- Andernfalls, wenn `Ei` `default`, ist `U` die spezielle Grenze `default`
- Andernfalls wird kein Rückschluss erstellt.

### <a name="exact-upper-bound-and-lower-bound-inferences"></a>Exakte, obere und untere gebundene Rückschlüsse

Bei Rückschlüsse *vom* Typ `U` *zum* Typ `V`wird in den folgenden Klauseln `V0` anstelle von `V` verwendet, wenn `V` ein Referenztyp ist, der NULL-Werte zulässt `V0?`.
- Wenn `V` eine der unfixed-Typvariablen ist, wird `U` als exakte, obere oder untere Grenze wie zuvor hinzugefügt.
- Andernfalls, wenn `U` `null` oder `default`ist, wird kein Rückschluss erstellt.
- Andernfalls, wenn `U` ein Verweistyp, der NULL-Werte zulässt `U0?`ist, wird `U0` anstelle der `U` in den nachfolgenden Klauseln verwendet.

Der Grund dafür ist, dass die NULL-Zulässigkeit, die sich direkt auf eine der Variablen ohne fester Größe bezieht, in ihren Grenzen bewahrt wird. Für die Rückschlüsse, die weiter unten in den Quell-und Zieltyp rekurdieren, wird die NULL-Zulässigkeit ignoriert. Dies ist möglicherweise nicht möglich, aber wenn dies nicht der Fall ist, wird später eine Warnung ausgegeben, wenn die Überladung ausgewählt und angewendet wird.

### <a name="fixing"></a>Festnahme

Die Spezifikation führt derzeit keine gute Aufgabe aus, zu beschreiben, was geschieht, wenn mehrere Grenzen in einander umsetzbar sind, aber unterschiedlich sind. Dies kann zwischen `object` und `dynamic`auftreten, zwischen Tupeltypen, die sich nur in Elementnamen unterscheiden, zwischen Typen, die hiervon erstellt werden, und jetzt auch zwischen `C` und `C?` für Verweis Typen.

Außerdem müssen wir "nullness" aus den Eingabe Ausdrücken an den Ergebnistyp weitergeben. 

Um diese Schritte durchzuführen, fügen wir weitere Phasen zur Korrektur hinzu. Dies ist jetzt der:

1. Erfassen aller Typen in allen Begrenzungen als Kandidaten, Entfernen von `?` aus allen, die auf NULL festleg Bare Verweis Typen sind
2. Entfernen Sie Kandidaten auf der Grundlage von exakten, unteren und oberen Begrenzungen (behalten Sie `null` und `default` Grenzen bei)
3. Entfernen Sie Kandidaten, die keine implizite Konvertierung in alle anderen Kandidaten haben.
4. Wenn die verbleibenden Kandidaten nicht alle über Identitäts Konvertierungen miteinander verfügen, schlägt der Typrückschluss fehl.
5. *Führen* Sie die übrigen Kandidaten wie unten beschrieben zusammen.
6. Wenn der resultierende Kandidat ein Referenztyp oder ein nicht auf NULL festleg barer Werttyp ist und *alle* exakten Begrenzungen oder *eine* der unteren Begrenzungen auf NULL festleg Bare Werttypen, auf NULL festleg Bare Verweis Typen, `null` oder `default`, werden dem resultierenden Kandidaten `?` hinzugefügt, sodass es sich um einen Werttyp oder Verweistyp handelt, der NULL-Werte zulässt.

Die zusammen *Führung* wird zwischen zwei Kandidaten Typen beschrieben. Es ist transitiv und kommutativ, sodass die Kandidaten in beliebiger Reihenfolge mit dem gleichen ultimativen Ergebnis zusammengeführt werden können. Er ist nicht definiert, wenn die beiden Kandidaten Typen nicht in einander konvertierbar sind.

Die *Merge* -Funktion nimmt zwei Kandidaten Typen und eine Richtung ( *+* oder *-* ) an:

- *Merge*(`T`, `T`, *d*) = t
- *Merge*(`S`, `T?` *+* ) = *Merge*(`S?`, `T`, *+* ) = *Merge*(`S`, `T`, *+* )`?`
- *Merge*(`S`, `T?`, *-* ) = *Merge*(`S?`, `T`, *-* ) = *Merge*(`S`, `T`, *-* )
- *Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>` *+* ) = `C<`*Merge*(`S1`, `T1`, *D1*)`,...,`*Merge*(`Sn`, `Tn`, *DN*)`>`, *wobei*
    - `di` =  *+* , wenn der `i`"th Type-Parameter von `C<...>` kovariant ist.
    - `di` =  *-* , wenn der `i`"th Type-Parameter von `C<...>` Kontra oder invariante ist.
- *Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>` *-* ) = `C<`*Merge*(`S1`, `T1`, *D1*)`,...,`*Merge*(`Sn`, `Tn`, *DN*)`>`, *wobei*
    - `di` =  *-* , wenn der `i`"th Type-Parameter von `C<...>` kovariant ist.
    - `di` =  *+* , wenn der `i`"th Type-Parameter von `C<...>` Kontra oder invariante ist.
- *Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*Merge*(`S1`, `T1`, *d*)`n1,...,`*Merge*(`Sn`, `Tn`, *d*) `nn)`, *wobei*
    - `ni` fehlt, wenn sich `si` und `ti` unterscheiden oder wenn beide nicht vorhanden sind.
    - `ni` ist `si`, wenn `si` und `ti` identisch sind.
- *Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`

## <a name="warnings"></a>Warnungen

### <a name="potential-null-assignment"></a>Mögliche NULL-Zuweisung

### <a name="potential-null-dereference"></a>Mögliche Null-Dereferenzierung

### <a name="constraint-nullability-mismatch"></a>Einschränkung der NULL-Zulässigkeit der Einschränkung

### <a name="nullable-types-in-disabled-annotation-context"></a>Typen, die NULL-Werte zulassen

## <a name="attributes-for-special-null-behavior"></a>Attribute für das besondere NULL-Verhalten


