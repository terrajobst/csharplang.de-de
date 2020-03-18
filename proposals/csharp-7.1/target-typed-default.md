---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "79483913"
---
# <a name="target-typed-default-literal"></a>"Standardliteralname" mit Zieltyp

* [x] vorgeschlagen
* [x] Prototyp
* [x]-Implementierung
* []-Spezifikation

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Die `default` Funktion mit Zieltyp ist eine kürzere Formular Variation des `default(T)`-Operators, sodass der Typ ausgelassen werden kann. Der Typ wird stattdessen von der Ziel Typisierung abgeleitet. Abgesehen davon verhält sie sich wie `default(T)`.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Die Hauptmotivation besteht darin, redundante Informationen zu vermeiden.

Wenn beispielsweise `void Method(ImmutableArray<SomeType> array)`aufgerufen wird, lässt *default* das standardliteralzeichen `M(default)` anstelle von `M(default(ImmutableArray<SomeType>))`zu.

Dies gilt für eine Reihe von Szenarien, wie z. b.:

- Deklarieren von lokalen Variablen (`ImmutableArray<SomeType> x = default;`)
- TERNÄRE Vorgänge (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)
- zurückgeben in Methoden und Lambdas (`return default;`)
- Deklarieren von Standardwerten für optionale Parameter (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)
- einschließen von Standardwerten in Array Erstellungs Ausdrücken (`var x = new[] { default, ImmutableArray.Create(y) };`)


## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Ein neuer Ausdruck wird eingeführt, das *default* standardliteralzeichen. Ein Ausdruck mit dieser Klassifizierung kann durch eine *standardliteralkonvertierung*implizit in einen beliebigen Typ konvertiert werden. 

Das Ableiten des Typs für das standardliteralformat funktioniert genauso wie für das *null* -Literale, mit dem Unterschied, dass ein beliebiger Typ zulässig ist (nicht nur Verweis Typen). *default*

Diese Konvertierung erzeugt den Standardwert des abhergenten Typs.

Das *standardliterale* kann abhängig vom abhergenten Typ einen konstanten Wert aufweisen. Daher ist `const int x = default;` Recht, aber `const int? y = default;` ist nicht.

Beim *default* standardliteralwert kann es sich um den Operanden von Gleichheits Operatoren handeln, solange der andere Operand einen Typ aufweist. Daher sind `default == x` und `x == default` gültige Ausdrücke, aber `default == default` ist unzulässig.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Ein geringfügiger Nachteil ist, dass in den meisten Kontexten anstelle von *null* -literalen *standardliterale* verwendet werden kann. Zwei Ausnahmen sind `throw null;` und `null == null`, die für das *null* -Literalzeichen zulässig sind, aber nicht das standardliteralzeichen. *default*

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Es gibt einige Alternativen zu berücksichtigende Aspekte:

- Der Status quo: das Feature ist nicht für seine eigenen Vorzüge gerechtfertigt, und Entwickler verwenden weiterhin den Standard Operator mit einem expliziten Typ.
- Erweitern des NULL-Literals: Dies ist der VB-Ansatz mit `Nothing`. Wir könnten `int x = null;`erlauben.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

- [x] sollte *standardmäßig* als Operand des *is* -oder *As* -Operators zulässig sein? Antwort: nicht zulassen von `default is T`, zulassen von `x is default`, zulassen von `default as RefType` (bei Always-NULL-Warnung)

## <a name="design-meetings"></a>Treffen von Besprechungen

- [LDM 3/7/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [LDM 3/28/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [LDM 5/31/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
