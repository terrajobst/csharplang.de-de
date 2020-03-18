---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483493"
---
# <a name="null-conditional-await"></a>NULL bedingter warte Wert

* [x] vorgeschlagen
* [] Prototyp: keine
* [] Implementierung: keine
* [] Spezifikation: gestartet, unten

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Unterstützen Sie einen Ausdruck der Form `await? e`, der `e` erwartet, wenn er nicht NULL ist, andernfalls führt er zu `null`.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Dabei handelt es sich um ein gängiges Codierungs Muster, und dieses Feature hätte eine schöne Zusammenarbeit mit den vorhandenen NULL-propagierenden und NULL-Sammel Operatoren.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Wir fügen eine neue Form der *await_expression*hinzu:

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

Der NULL bedingte `await` Operator erwartet nur dann seinen Operanden, wenn dieser Operand nicht NULL ist. Andernfalls ist das Ergebnis der Anwendung des Operators NULL.

Der Ergebnistyp wird mithilfe der [Regeln für den NULL bedingten Operator](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator)berechnet.

> **Hinweis:** Wenn `e` vom Typ `Task`ist, würde `await? e;` nichts tun, wenn `e` `null`ist, und `e`, wenn es nicht `null`ist.
>
> Wenn `e` vom Typ `Task<K>` ist, bei dem `K` ein Werttyp ist, würde `await? e` einen Wert vom Typ `K?`ergeben.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Wie bei allen Sprach Features müssen wir Fragen, ob die zusätzliche Komplexität der Sprache in der zusätzlichen Klarheit für den Text der C# Programme, die von der Funktion profitieren würden, zurückgegeben wird.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Obwohl es Code Bausteine erfordert, kann die Verwendung dieses Operators häufig durch einen Ausdruck wie `(e == null) ? null : await e` oder eine Anweisung wie `if (e != null) await e`ersetzt werden.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

- [] Erfordert eine LDM-Überprüfung

## <a name="design-meetings"></a>Treffen von Besprechungen

None.
