---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2019
ms.locfileid: "79484057"
---
# <a name="lambda-discard-parameters"></a>Lambda-Verwerfungs Parameter

## <a name="summary"></a>Zusammenfassung

Zulassen, dass verworfene (`_`) als Parameter von Lambdas und anonymen Methoden verwendet werden.
Beispiel:
- Lambdas: `(_, _) => 0``(int _, int _) => 0`
- anonyme Methoden: `delegate(int _, int _) { return 0; }`

## <a name="motivation"></a>Motivation

Nicht verwendete Parameter müssen nicht benannt werden. Der Zweck der verworfenen verworfenen ist klar, d. h. Sie werden nicht verwendet/verworfen

## <a name="detailed-design"></a>Detaillierter Entwurf

[Methoden Parameter](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) In der Parameterliste einer Lambda-Methode oder anonymen Methode mit mehreren Parametern namens "`_`" sind solche Parameter Parameter verwerfen.
Hinweis: Wenn ein einzelner Parameter `_` benannt ist, handelt es sich um einen regulären Parameter aus Gründen der Abwärtskompatibilität.

Bei Verwerfen von Parametern werden keine Namen in Bereiche eingeführt.
Hinweis Dies impliziert, dass Sie keine `_` Namen (Unterstriche) ausblenden.

[Einfache Namen](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) Wenn `K` 0 (null) ist und die *Simple_name* in einem- *Block* angezeigt wird und der *Block*(oder ein einschließender *Block*) der Deklaration der lokalen Variablen Deklaration ([Deklarationen](basic-concepts.md#declarations)) eine lokale Variable, einen Parameter (mit Ausnahme von Parameter verwerfen) oder eine Konstante mit dem Namen `I`enthält, verweist der *Simple_name* auf diese lokale Variable, den Parameter oder die Konstante und wird als Variable oder Wert

[Bereiche](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) Mit Ausnahme von Verwerfungs Parametern ist der Gültigkeitsbereich eines in einem *lambda_expression* ([Anonyme Funktions Ausdrücke](expressions.md#anonymous-function-expressions)) deklarierten Parameters der *anonymous_function_body* von, der mit Ausnahme von Parameter verwerfen *lambda_expression* . der Gültigkeitsbereich eines in einem *anonymous_method_expression* ([anonymen Funktions Ausdruck](expressions.md#anonymous-function-expressions)) deklarierten Parameters ist der *Block* dieser *anonymous_method_expression*.

## <a name="related-spec-sections"></a>Verwandte spec-Abschnitte
- [Entsprechende Parameter](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
