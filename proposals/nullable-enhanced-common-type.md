---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483523"
---
# <a name="nullable-enhanced-common-type"></a>Nullable-Enhanced Common Type

* [x] vorgeschlagen
* [] Prototyp: keine
* [] Implementierung: keine
* [] Spezifikation: siehe unten

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Es gibt eine Situation, in der die aktuellen Algorithmus-Ergebnisse des allgemeinen Typs kontraintuitiv sind, und führt dazu, dass der Programmierer eine redundante Umwandlung zum Code hinzufügt. Bei dieser Änderung würde ein Ausdruck wie `condition ? 1 : null` einen Wert vom Typ "`int?`" ergeben, und ein Ausdruck wie `condition ? x : 1.0`, bei dem `x` vom Typ "`int?`" einen Wert vom Typ "`double?`" ergibt.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Dies ist eine häufige Ursache dafür, wie sich der Programmierer wie der Code für unnötige Bausteine eignet.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Wir ändern die Spezifikation, um [den besten allgemeinen Typ eines Satzes von Ausdrücken zu ermitteln](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) , der sich auf die folgenden Situationen auswirkt:

- Wenn ein Ausdruck aus einem Werttyp, der nicht auf NULL festgelegt werden kann `T` und der andere ein NULL-Literalwert ist, ist das Ergebnis vom Typ `T?`.
- Wenn ein Ausdruck einen Werttyp hat, der NULL-Werte zulässt `T?` und der andere einen Werttyp `U`und eine implizite Konvertierung von `T` in `U`vorliegt, ist das Ergebnis vom Typ `U?`.

Dies wirkt sich auf die folgenden Aspekte der Sprache aus:

- der [ternäre Ausdruck](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)
- implizit typisierter [Array Erstellungs Ausdruck](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)
- Ableiten des [Rückgabe Typs eines Lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) -Ausdrucks für den Typrückschluss
- Fälle mit Generika, z. b. das Aufrufen von `M<T>(T a, T b)` als `M(1, null)`.

Genauer ist, dass wir die folgenden Abschnitte der Spezifikation ändern (Einfügungen in Fett Formatierung, Löschungen in durch Strichen):

> #### <a name="output-type-inferences"></a>Ausschlüsse auf Ausgabetyp
> 
> Ein *ausgabetyprückschluss* erfolgt auf folgende Weise *von* einem Ausdruck `E` *auf* einen Typ `T`:
> 
> *  Wenn `E` eine anonyme Funktion mit dem abzurufenden Rückgabetyp `U` (abzurufender[Rückgabetyp](expressions.md#inferred-return-type)) ist und `T` ein Delegattyp oder ein Ausdrucks Strukturtyp mit dem Rückgabetyp `Tb`ist, wird ein untergeordneter Daten *Rückschluss* ([niedrigerer gebundener Schlussfolgerung](expressions.md#lower-bound-inferences)) *von* `U` *bis* `Tb`vorgenommen.
> *  Andernfalls, wenn `E` eine Methoden Gruppe ist und `T` ein Delegattyp oder ein Ausdrucks Strukturtyp mit Parametertypen `T1...Tk` und der Rückgabetyp `Tb`ist und die Überladungs Auflösung von `E` mit den Typen `T1...Tk` eine einzelne Methode mit dem Rückgabetyp `U`ergibt, wird ein *niedrigerer gebundener Rückschluss* *von* `U` *bis* `Tb`ausgelöst.
> *  \* * Andernfalls, wenn `E` ein Ausdruck mit einem Werttyp, der NULL-Werte zulässt `U?`ist, wird ein Rückschluss *von* `U` *zum* `T` hergestellt, und `T`wird eine NULL *-* *Grenze* hinzugefügt. **
> *  Andernfalls, wenn `E` ein Ausdruck mit dem Typ `U`ist, wird ein Rück *Schluss* *von* `U` *zum* `T`gemacht.
> *  **Andernfalls, wenn `E` ein konstanter Ausdruck mit einem Wert `null`ist, wird eine *null-Grenze* zu `T`** 
> *  Andernfalls werden keine Rückschlüsse gemacht.

> #### <a name="fixing"></a>Festnahme
> 
> Eine Variable vom Typ " *unfixed* " `Xi` mit einem Satz von Begrenzungen ist wie folgt *festgelegt* :
> 
> *  Der Satz von *Kandidaten Typen* `Uj` beginnt als Satz aller Typen im Satz von Begrenzungen für `Xi`.
> *  Anschließend untersuchen wir die einzelnen gebundenen `Xi` wiederum: für jede exakte gebundene `U` `Xi` alle Typen `Uj` die nicht mit `U` identisch sind, werden aus dem Kandidaten Satz entfernt. Für jede Untergrenze `U` `Xi` alle Typen `Uj` die *keine* implizite Konvertierung von `U` aus dem Kandidaten Satz entfernt werden. Für jede obere Grenze `U` `Xi` alle Typen `Uj` aus denen *keine* implizite Konvertierung in `U` aus dem Kandidaten Satz entfernt werden.
> *  Wenn zwischen den verbleibenden Kandidaten Typen `Uj` ein eindeutiger Typ `V` der eine implizite Konvertierung in alle anderen Kandidaten Typen enthält, ~~wird`Xi` auf `V`korrigiert.~~
>     -  **Wenn `V` ein Werttyp ist und ein NULL-Wert an `Xi`*gebunden* ist, wird `Xi` auf `V?`**
>     -  **Andernfalls wird `Xi` korrigiert `V`**
> *  Andernfalls schlägt der Typrückschluss fehl.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Es gibt möglicherweise einige Inkompatibilitäten, die von diesem Vorschlag eingeführt werden.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

None.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

- [] Was ist der Schweregrad der Inkompatibilität, der von diesem Vorschlag eingeführt wurde, und wie kann er moderiert werden?

## <a name="design-meetings"></a>Treffen von Besprechungen

None.
