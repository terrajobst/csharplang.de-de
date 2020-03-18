---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483535"
---
# <a name="throw-expression"></a>Throw-Ausdruck

Wir erweitern den Satz von Ausdrucks Formularen auf include.

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

Die Typregeln lauten wie folgt:

- Ein *throw_expression* weist keinen Typ auf.
- Eine *throw_expression* kann durch eine implizite Konvertierung in jeden Typ konvertiert werden.

Ein *throw-Ausdruck* löst den Wert aus, der durch Auswerten des *null_coalescing_expression*erzeugt wird, der einen Wert des Klassen Typs `System.Exception`angeben muss, eines Klassen Typs, der von `System.Exception` abgeleitet ist, oder eines Typparameter Typs, der über `System.Exception` (oder eine Unterklasse) als effektive Basisklasse verfügt. Wenn die Auswertung des Ausdrucks `null`erzeugt, wird stattdessen eine `System.NullReferenceException` ausgelöst.

Das Verhalten zur Laufzeit der Auswertung eines Throw- *Ausdrucks* entspricht dem [für eine *throw-Anweisung*angegebenen](../../spec/statements.md#the-throw-statement)Verhalten.

Die Regeln für die Fluss Analyse lauten wie folgt:

- Für jede Variable *v*wird *v* definitiv vor dem *null_coalescing_expression* eines *throw_expression* IFF zugewiesen, dass es definitiv vor dem *throw_expression*zugewiesen wird.
- Für jede Variable *v*wird *v* nach *throw_expression*definitiv zugewiesen.

Ein *throw-Ausdruck* ist nur in den folgenden syntaktischen Kontexten zulässig:
- Der zweite oder dritte Operand eines ternären bedingten Operators `?:`
- Der zweite Operand eines NULL-Sammel Operators `??`
- Als Text eines Ausdrucks Körper Ausdrucks oder einer Methode.
