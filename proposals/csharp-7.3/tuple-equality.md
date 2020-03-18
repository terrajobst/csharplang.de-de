---
ms.openlocfilehash: f238a711e710bbac7f5b7400fa938bd85dec00c6
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79484141"
---
# <a name="support-for--and--on-tuple-types"></a>Unterstützung für = = und! = für Tupeltypen

Lassen Sie Ausdrücke `t1 == t2`, in denen `t1` und `t2` Tupeltypen mit gleicher Kardinalität sind, und Werten Sie Sie ungefähr wie `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` aus (vorausgesetzt `var temp1 = t1; var temp2 = t2;`).

Umgekehrt würde es `t1 != t2` ermöglichen und es als `temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`auswerten.

In Fällen, in denen Nullwerte zulässig sind, werden zusätzliche Überprüfungen für `temp1.HasValue` und `temp2.HasValue` verwendet. Beispielsweise wird `nullableT1 == nullableT2` als `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`ausgewertet.

Wenn ein Element weiser Vergleich ein nicht-boolescher Ergebnis zurückgibt (z. b. Wenn ein nicht boolescher benutzerdefinierter `operator ==` oder `operator !=` verwendet wird, oder in einem dynamischen Vergleich), wird dieses Ergebnis entweder in `bool` konvertiert oder durch `operator true` oder `operator false` ausgeführt, um eine `bool`zu erhalten. Beim tupelvergleich wird immer ein `bool`zurückgegeben.

Ab C# 7,2 erzeugt dieser Code einen Fehler (`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`), es sei denn, es gibt eine benutzerdefinierte `operator==`.

## <a name="details"></a>Details

Wenn Sie den `==`-Operator (oder `!=`) binden, sind die vorhandenen Regeln: (1) dynamischer Fall, (2) Überladungs Auflösung und (3) fehlgeschlagen.
Dieses Angebot fügt einen tupelfall zwischen (1) und (2) hinzu: Wenn beide Operanden eines Vergleichs Operators Tupel sind (über Tupeltypen verfügen oder tupelliterale sind) und eine passende Kardinalität aufweisen, wird der Vergleich Element Weise ausgeführt. Diese tupelgleichheit wird auch auf auf NULL festleg Bare Tupel angehoben.

Beide Operanden (und im Fall von tupelliteralen) werden in der Reihenfolge von links nach rechts ausgewertet. Jedes Element Paar wird dann als Operanden verwendet, um den Operator `==` (oder `!=`) rekursiv zu binden. Alle Elemente mit der Kompilier Zeittyp-`dynamic` verursachen einen Fehler. Die Ergebnisse dieser Element weisen Vergleiche werden als Operanden in einer Kette von bedingten Operatoren und-Operatoren (oder oder) verwendet.

Beispielsweise werden im Kontext `(int, (int, int)) t1, t2;``t1 == (1, (2, 3))` als `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`ausgewertet.

Wenn ein tupelliteral als Operand (auf beiden Seiten) verwendet wird, empfängt es einen konvertierten tupeltyp, der durch die Element bezogenen Konvertierungen gebildet wird, die beim Binden des Operators `==` (oder `!=`) Element Weise eingeführt werden. 

Beispielsweise ist in `(1L, 2, "hello") == (1, 2L, null)`der konvertierte Typ für beide tupelliterale `(long, long, string)`, und das zweite Literale hat keinen natürlichen Typ.


### <a name="deconstruction-and-conversions-to-tuple"></a>Debug und Konvertierungen in Tupel
In `(a, b) == x`spielt die Tatsache, dass `x` in zwei Elemente decoeinigen kann, keine Rolle. Dies könnte in einem zukünftigen Vorschlag der Fall sein, obwohl es Fragen zu `x == y` aufwerfen würde (ist dies ein einfacher Vergleich oder ein Element weiser Vergleich, und wenn dies der Fall ist, welche Kardinalität verwendet wird).
Ebenso spielen Konvertierungen in Tupel keine Rolle.

### <a name="tuple-element-names"></a>Tupelelementnamen

Beim Umrechnen eines tupelliterals warnen wir, wenn ein expliziter tupelelementname im Literalformat angegeben wurde, aber nicht mit dem Ziel-tupelelementnamen identisch ist.
Wir verwenden die gleiche Regel beim Tupel-Vergleich, sodass `(int a, int b) t` wir bei `d` in `t == (c, d: 0)`warnen.

### <a name="non-bool-element-wise-comparison-results"></a>Nicht boolesche Vergleichsergebnisse (Element Weise)

Wenn ein Element weiser Vergleich in einer tupelgleichheit dynamisch ist, verwenden wir einen dynamischen Aufruf des Operators `false` und negieren diese, um eine `bool` zu erhalten und mit weiteren Element bezogenen vergleichen fortzufahren. 

Wenn ein Element weiser Vergleich einen anderen nicht booleschen Typ in einer tupelgleichheit zurückgibt, gibt es zwei Fälle:
- Wenn der Typ, der kein boolescher Typ ist, in `bool`konvertiert wird, wird diese Konvertierung angewendet.
- Wenn keine solche Konvertierung vorhanden ist, der Typ jedoch einen Operator `false`hat, verwenden wir diesen und negieren das Ergebnis.

Bei einer tupelungleichheit gelten dieselben Regeln, mit der Ausnahme, dass der Operator `true` (ohne Negation) anstelle des Operators `false`verwendet wird.

Diese Regeln ähneln den Regeln für die Verwendung eines nicht-booleschen Typs in einer `if`-Anweisung und einigen anderen vorhandenen Kontexten.

## <a name="evaluation-order-and-special-cases"></a>Auswertungs Reihenfolge und Sonderfälle
Der Wert auf der linken Seite wird zuerst ausgewertet, dann der Wert auf der rechten Seite, dann die Element weisen Vergleiche von links nach rechts (einschließlich Konvertierungen und mit Early Exit basierend auf vorhandenen Regeln für bedingte and/or-Operatoren).

Wenn beispielsweise eine Konvertierung vom Typ `A` in den Typ `B` und eine Methode `(A, A) GetTuple()`erfolgt, bedeutet das Auswerten `(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` Folgendes:
- `new A(1)`
- `new B(2)`
- `new B(3)`
- `new B(4)`
- `GetTuple()`
- Anschließend werden die Element weisen Konvertierungen und Vergleiche und die bedingte Logik ausgewertet (konvertieren Sie `new A(1)` in den Typ `B`, und vergleichen Sie ihn dann mit `new B(4)`usw.).

### <a name="comparing-null-to-null"></a>Vergleichen von `null` mit `null`

Dies ist ein Sonderfall von regulären vergleichen, der auf tupelvergleiche überträgt. Der `null == null` Vergleich ist zulässig, und die `null` Literale erhalten keinen Typ.
Bei tupelgleichheit bedeutet dies, dass `(0, null) == (0, null)` ebenfalls zulässig ist und die `null`-und tupelliterale keinen Typ erhalten.

### <a name="comparing-a-nullable-struct-to-null-without-operator"></a>Vergleichen einer Struktur, die NULL-Werte zulässt, mit `null` ohne `operator==`

Dies ist ein weiterer Sonderfall von regulären vergleichen, der auf tupelvergleiche überträgt.
Wenn Sie ein `struct S` ohne `operator==`haben, ist der `(S?)x == null` Vergleich zulässig, und er wird als `((S?).x).HasValue`interpretiert.
Bei der tupelgleichheit wird dieselbe Regel angewendet, sodass `(0, (S?)x) == (0, null)` zulässig ist.

## <a name="compatibility"></a>Kompatibilität

Wenn ein Benutzer seine eigenen `ValueTuple` Typen mit einer Implementierung des Vergleichs Operators verfasst hat, wäre er zuvor durch Überladungs Auflösung übernommen worden. Da jedoch der neue tupelfall vor der Überladungs Auflösung steht, wird dieser Fall mit tupelvergleichen behandelt, anstatt auf den benutzerdefinierten Vergleich zu vertrauen.

----

Bezieht sich auf [relationale und Typtest Operatoren](../../spec/expressions.md#relational-and-type-testing-operators) in Beziehung zu [#190](https://github.com/dotnet/csharplang/issues/190)
