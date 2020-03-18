---
ms.openlocfilehash: ecdad8c863d0695bc901e4d96d9ca3decbc248eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483595"
---
# <a name="nullable-reference-types-in-c"></a>Verweis Typen, die NULL-Werte zulassenC# #

Ziel dieses Features ist Folgendes:

* Ermöglicht es Entwicklern, auszudrücken, ob eine Variable, ein Parameter oder ein Ergebnis eines Verweis Typs NULL sein soll oder nicht.
* Geben Sie Warnungen an, wenn derartige Variablen, Parameter und Ergebnisse nicht gemäß dieser Absicht verwendet werden.

## <a name="expression-of-intent"></a>Ausdruck der Absicht

Die Sprache enthält bereits die `T?` Syntax für Werttypen. Es ist einfach, diese Syntax auf Verweis Typen auszuweiten.

Es wird davon ausgegangen, dass die Absicht eines nicht verzierten Verweis Typs `T` ist, dass er ungleich NULL ist.

## <a name="checking-of-nullable-references"></a>Überprüfen von verweisen, die NULL zulassen

Eine Fluss Analyse verfolgt Verweis Variablen, die NULL-Werte zulassen. Wenn die Analyse nicht NULL ist (z. b. nach einer Überprüfung oder einer Zuweisung), wird Ihr Wert als nicht-NULL-Verweis betrachtet.

Ein Verweis, der NULL-Werte zulässt, kann mit dem postfix-`x!` Operator (dem "dammit"-Operator) auch explizit als ungleich NULL behandelt werden, wenn die Fluss Analyse keine nicht-NULL-Situation festlegen kann, die der Entwickler kennt.

Andernfalls wird eine Warnung ausgegeben, wenn ein Verweis, der NULL-Werte zulässt, dereferenziert wird oder in einen nicht-NULL-Typ konvertiert wird.

Beim Umrechnen von `S[]` in `T?[]` und von `S?[]` in `T[]`wird eine Warnung ausgegeben.

Beim Umrechnen von `C<S>` in `C<T?>` wird eine Warnung ausgegeben, es sei denn, der Typparameter ist kovariant (`out`), und bei der Umstellung von `C<S?>` in `C<T>`, außer wenn der Typparameter kontra Variant (`in`) ist.

Beim `C<T?>` wird eine Warnung ausgegeben, wenn der Typparameter Einschränkungen ungleich NULL aufweist. 

## <a name="checking-of-non-null-references"></a>Überprüfen von nicht-NULL-verweisen

Eine Warnung wird ausgegeben, wenn ein NULL-wahrsten einer Variablen ungleich NULL zugewiesen oder als nicht-NULL-Parameter übergeben wird.

Eine Warnung wird auch ausgegeben, wenn ein Konstruktor nicht NULL-Verweis Felder explizit initialisiert.

Wir können nicht ausreichend nachverfolgen, ob alle Elemente eines Arrays von verweisen, die nicht NULL sind, initialisiert werden. Wir könnten jedoch eine Warnung ausgeben, wenn kein Element eines neu erstellten Arrays zugewiesen wird, bevor das Array gelesen oder weitergereicht wird. Dies kann den häufigen Fall verarbeiten, ohne zu stark zu sein.

Wir müssen entscheiden, ob `default(T)` eine Warnung generiert oder einfach als Typ `T?`behandelt wird.

## <a name="metadata-representation"></a>Metadatendarstellung

Zusatzelemente der NULL-Zulässigkeit sollten in Metadaten als Attribute dargestellt werden. Dies bedeutet, dass downlevelcompiler diese ignorieren.

Wir müssen entscheiden, ob nur NULL-Werte zulässig sind, oder es gibt Aufschluss darüber, ob ein nicht-NULL-Element in der Assembly "on" war.

## <a name="generics"></a>Generika

Wenn ein Typparameter `T` keine NULL-Werte zulässt, wird er innerhalb seines Gültigkeits Bereichs als nicht auf NULL festlegbar behandelt.

Wenn ein Typparameter nicht eingeschränkt ist oder nur NULL-Werte zulässt, ist die Situation etwas komplexer: Dies bedeutet, dass das entsprechende Typargument *entweder* NULL-Werte zulässt oder keine NULL-Werte zulässt. Die in dieser Situation sichere Aufgabe besteht darin, den Typparameter *sowohl* als Werte zulässt als auch als nicht-NULL-Werte zu behandeln, und es werden Warnungen ausgegeben, wenn eine Verletzung auftritt. 

Es lohnt sich zu erwägen, ob explizite Verweis Einschränkungen auf NULL-Werte zulässig sind. Beachten Sie jedoch, dass es nicht möglich ist, Verweis Typen, die NULL-Werte zulassen, *implizit* in bestimmten Fällen eingeschränkt zu werden (geerbte Einschränkungen).

Die `class` Einschränkung ist nicht NULL. Wir können berücksichtigen, ob `class?` eine gültige Einschränkung, die NULL-Werte zulässt, als Verweis Typen, die NULL-Werte zulassen, sein sollten.

## <a name="type-inference"></a>Typrückschluss

Wenn beim Typrückschluss ein Mitwirkender Typ ein Verweistyp ist, der NULL-Werte zulässt, sollte der resultierende Typ NULL-Werte zulassen. Das heißt, dass NULL-Werte weitergegeben werden.

Wir sollten berücksichtigen, ob der `null` Literale als teilnehmende Ausdruck NULL sein sollte. Dies ist heute nicht der Fall: für Werttypen führt dies zu einem Fehler, während für Verweis Typen der NULL-Wert erfolgreich in den Plain-Typ konvertiert wird. 

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="breaking-changes"></a>Aktuelle Änderungen

Warnungen ungleich NULL sind eine offensichtliche Breaking Change für vorhandenen Code und sollten mit einem Opt-in-Mechanismus einhergehen.

Weniger offensichtlich sind Warnungen von Typen, die NULL-Werte zulassen (wie oben beschrieben), eine Breaking Change auf vorhandenem Code in bestimmten Szenarien, in denen die NULL-Zulässigkeit implizit ist:

* Nicht eingeschränkte Typparameter werden als implizit NULL-Werte behandelt, sodass Sie `object` oder beim Zugriff auf z. b. `ToString` Warnungen erhalten.
* Wenn der Typrückschluss NULL-Werte von `null` Ausdrücken ableitet, liefert vorhandener Code in manchen Fällen NULL-Werte und nicht auf NULL festleg Bare Typen, was zu neuen Warnungen führen kann.

Daher müssen auf NULL festleg Bare Warnungen ebenfalls optional sein.

Zum Schluss ist das Hinzufügen von Anmerkungen zu einer vorhandenen API eine Breaking Change für Benutzer, die sich für Warnungen entschieden haben, wenn Sie die Bibliothek aktualisieren. Dies hat auch die Möglichkeit, sich zu entscheiden. "Ich möchte die Fehlerbehebungen, aber ich bin nicht bereit, mit ihren neuen Anmerkungen umzugehen"

Zusammenfassend müssen Sie in der Lage sein, Folgendes zu abonnieren:
* NULL-Werte zulassen
* Nicht-NULL-Warnungen
* Warnungen von Anmerkungen in anderen Dateien

Die Granularität der Zustimmung deutet auf ein Analyse ähnliches Modell hin, bei dem sich Teil Striche mit Pragmas anmelden können, und Schweregrade können vom Benutzer ausgewählt werden. Außerdem kann es sein, dass pro Bibliothek-Optionen ("die Anmerkungen von JSON.net ignorieren, bis ich bereit für den Umgang mit dem Fallout sind") möglicherweise im Code als Attribute ausgedrückt werden kann.

Der Entwurf der Opt-in-/Übergangs--Funktion ist entscheidend für den Erfolg und die Nützlichkeit dieses Features. Wir müssen Folgendes sicherstellen:

* Benutzer können die Überprüfung der NULL-Zulässigkeit nach und nach nach Bedarf übernehmen
* Bibliotheks Autoren können NULL-Zulässigkeit-Anmerkungen hinzufügen
* Obwohl dies nicht der Fall ist, ist "Konfigurations Alptraum" nicht sinnvoll.

## <a name="tweaks"></a>Anpassungen

Wir könnten die `?` Anmerkungen nicht für lokale Variablen verwenden, sondern nur feststellen, ob Sie in Übereinstimmung mit den Ihnen zugewiesenen Informationen verwendet werden. Ich bevorzuge dies nicht. Ich denke, wir sollten den Benutzern die Absicht überlassen, ihre Absicht auszudrücken.

Wir könnten eine kurz`T! x` zu Parametern in Erwägung nehmen, die automatisch eine NULL-Lauf Zeit Überprüfung generiert.

Bestimmte Muster bei generischen Typen, wie z. b. `FirstOrDefault` oder `TryGet`, haben etwas merkwürdige Verhalten mit nicht auf NULL festleg baren Typargumenten, da Sie in bestimmten Situationen explizit Standardwerte ergeben. Wir könnten versuchen, das Typsystem zu unterstützen, um diese bessere Anpassung zu ermöglichen. Beispielsweise können wir `?` auf nicht eingeschränkten Typparametern zulassen, obwohl das Typargument bereits NULL-Werte zulassen kann. Ich bin mir sicher, dass es Wert ist, und es führt zu einer Bedeutung, die sich auf die Interaktion mit auf NULL festleg baren *Werttypen* bezieht. 

## <a name="nullable-value-types"></a>Auf NULL festlegbare Werttypen

Wir könnten auch einige der obigen Semantik für Werttypen, die NULL-Werte zulassen, in Erwägung gezogen.

Wir haben bereits den Typrückschluss erwähnt, bei dem wir `int?` aus `(7, null)`ableiten konnten, anstatt nur einen Fehler zu geben.

Eine weitere Möglichkeit besteht darin, die Fluss Analyse auf Werte zulässt-Werttypen anzuwenden. Wenn Sie als ungleich Null angesehen werden, können wir die Verwendung von als nicht auf NULL festleg baren Typ auf bestimmte Weise zulassen (z. b. Element Zugriff). Wir müssen nur darauf achten, dass die Dinge, die Sie *bereits* für einen Werte zulässt-Werttyp ausführen können, für die Back-Kompatibilitäts-Gründe bevorzugt werden.
