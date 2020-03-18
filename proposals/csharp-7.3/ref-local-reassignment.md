---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483583"
---
# <a name="ref-local-reassignment"></a>Lokale Ref-Neuzuweisung

In C# 7,3 wird Unterstützung für das erneute Binden des Verweises einer lokalen Ref-Variablen oder eines ref-Parameters hinzugefügt.

Wir fügen dem Satz von `assignment_operator`s Folgendes hinzu:

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

Der `=ref`-Operator wird als ***ref-Zuweisungs Operator***bezeichnet. Es handelt sich nicht um einen *Verbund Zuweisungs Operator*. Der linke Operand muss ein Ausdruck sein, der an eine lokale Ref-Variable, einen ref-Parameter (außer `this`) oder einen out-Parameter bindet. Der rechte Operand muss ein Ausdruck sein, der einen lvalue ergibt, der einen Wert des gleichen Typs wie der linke Operand angibt.

Der rechte Operand muss definitiv am Punkt der Ref-Zuweisung zugewiesen werden.

Wenn der linke Operand an einen `out`-Parameter bindet, ist es ein Fehler, wenn dieser `out` Parameter am Anfang des ref-Zuweisungs Operators nicht definitiv zugewiesen wurde.

Wenn der linke Operand ein Beschreib barer Verweis ist (d. h., er legt einen anderen als einen `ref readonly` local-oder `in`-Parameter fest), dann muss der rechte Operand ein Beschreib barer lvalue sein.

Der Ref-Zuweisungs Operator ergibt einen lvalue des zugewiesenen Typs. Es ist beschreibbar, wenn der linke Operand beschreibbar ist (d. h. nicht `ref readonly` oder `in`).

Die Sicherheitsregeln für diesen Operator lauten:

- Bei einer `e1 = ref e2`Ref-Neuzuweisung muss die *ref-sicher-zu-* Escapezeichen von `e2` mindestens so breit sein, wie der *ref-Safe-to-Escape* -`e1`ist.

Where *ref-Safe-to-Escape* ist [für Ref-like-Typen in Sicherheit](../csharp-7.2/span-safety.md) definiert.
