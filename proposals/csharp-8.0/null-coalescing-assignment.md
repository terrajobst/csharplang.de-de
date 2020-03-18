---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2019
ms.locfileid: "79483889"
---
# <a name="null-coalescing-assignment"></a>Koaleszierende NULL-Zuweisung

* [x] vorgeschlagen
* [] Prototyp: nicht gestartet
* [] Implementierung: nicht gestartet
* [] Spezifikation: unten

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Vereinfacht ein gängiges Codierungs Muster, bei dem einer Variablen ein Wert zugewiesen wird, wenn Sie NULL ist.

Im Rahmen dieses Angebots werden auch die typanforderungen auf `??` gelockert, damit ein Ausdruck, dessen Typ ein uneingeschränkter Typparameter ist, auf der linken Seite verwendet werden kann.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Es kommt häufig vor, dass Code des Formulars angezeigt wird.

```csharp
if (variable == null)
{
    variable = expression;
}
```

In diesem Vorschlag wird der Sprache, die diese Funktion ausführt, ein nicht über ladbarer binärer Operator hinzugefügt.

Für dieses Feature gab es mindestens acht communityanforderungen.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Wir fügen eine neue Form von Zuweisungs Operatoren hinzu.

``` antlr
assignment_operator
    : '??='
    ;
```

Der den [vorhandenen Semantik Regeln für Verbund Zuweisungs Operatoren](../../spec/expressions.md#compound-assignment)folgt, mit dem Unterschied, dass die Zuweisung entfernt wird, wenn die linke Seite nicht NULL ist. Die Regeln für dieses Feature lauten wie folgt.

Wenn `A` der `a``a ??= b`ist, ist `B` der Typ des `b`, und `A0` ist der zugrunde liegende Typ von `A`, wenn `A` ein Werttyp ist, der NULL-Werte zulässt:

1. Wenn `A` nicht vorhanden ist oder ein nicht auf NULL festleg barer Werttyp ist, tritt ein Kompilierzeitfehler auf.
2. Wenn `B` nicht implizit in `A` oder `A0` konvertiert werden kann (wenn `A0` vorhanden ist), tritt ein Kompilierzeitfehler auf.
3. Wenn `A0` vorhanden ist und `B` implizit in `A0`konvertierbar ist und `B` nicht dynamisch ist, wird der Typ des `a ??= b` `A0`. `a ??= b` wird zur Laufzeit wie folgt ausgewertet:
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   Mit der Ausnahme, dass `a` nur einmal ausgewertet wird.
4. Andernfalls wird der Typ des `a ??= b` `A`. `a ??= b` wird zur Laufzeit als `a ?? (a = b)`ausgewertet, mit dem Unterschied, dass `a` nur einmal ausgewertet wird.


Um die typanforderungen `??`zu erfüllen, aktualisieren wir die Spezifikation, in der Sie derzeit angibt, dass `a ?? b`, wobei `A` der `a`ist:

> 1. Wenn ein vorhanden ist und kein Werte zulässt-Typ oder Verweistyp ist, tritt ein Kompilierzeitfehler auf.

Wir lockern diese Anforderung für Folgendes:

1. Wenn eine vorhanden ist und ein Werttyp ist, der keine NULL-Werte zulässt, tritt ein Kompilierzeitfehler auf.

Dadurch kann der NULL-Sammel Operator an nicht eingeschränkten Typparametern arbeiten, da der nicht eingeschränkte Typparameter T vorhanden ist, kein Werte zulässt-Typ ist und kein Referenztyp ist.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Wie bei allen Sprach Features müssen wir Fragen, ob die zusätzliche Komplexität der Sprache in der zusätzlichen Klarheit für den Text der C# Programme, die von der Funktion profitieren würden, zurückgegeben wird.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Der Programmierer kann `(x = x ?? y)`, `if (x == null) x = y;`oder `x ?? (x = y)` per Hand schreiben.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

- [] Erfordert eine LDM-Überprüfung
- [] Sollten auch `&&=`-und `||=`-Operatoren unterstützt werden?

## <a name="design-meetings"></a>Treffen von Besprechungen

None.
