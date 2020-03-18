---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483673"
---
# <a name="pattern-matching-with-generics"></a>Muster Vergleich mit Generika

* [x] vorgeschlagen
* [] Prototyp:
* []-Implementierung:
* []-Spezifikation:

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Die Spezifikation für den [vorhandenen C# as-Operator](../../spec/expressions.md#the-as-operator) ermöglicht es, keine Konvertierung zwischen dem Typ des Operanden und dem angegebenen Typ durchzusetzen, wenn ein offener Typ ist. In C# 7 erfordert das `Type identifier` Muster jedoch eine Konvertierung zwischen dem Typ der Eingabe und dem angegebenen Typ.

Es wird vorgeschlagen, diesen Vorgang zu lockern und `expression is Type identifier`zu ändern, zusätzlich zu den Bedingungen, die in C# 7 zulässig sind, auch zulässig, wenn `expression as Type` zulässig wäre. Insbesondere handelt es sich bei den neuen Fällen um Fälle, in denen der Typ des Ausdrucks oder der angegebene Typ ein offener Typ ist. 

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Fälle, in denen Muster Vergleiche "offensichtlich" zugelassen werden, können derzeit nicht kompiliert werden. Siehe z. b. https://github.com/dotnet/roslyn/issues/16195.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Wir ändern den Absatz in der Spezifikation für die Muster Übereinstimmung (die vorgeschlagene Addition ist fett dargestellt):

> Bestimmte Kombinationen von statischem Typ der linken Seite und des angegebenen Typs werden als nicht kompatibel betrachtet und führen zu einem Kompilierzeitfehler. Ein Wert vom Typ "statischer Typ `E`" ist mit dem Typ " *Muster kompatibel* " `T`, wenn eine Identitäts Konvertierung, eine implizite Verweis Konvertierung, eine Boxing-Konvertierung, eine explizite Verweis Konvertierung oder eine Unboxing-Konvertierung von `E` in `T`vorhanden **ist, oder wenn entweder `E` oder `T` ein offener Typ ist**. Es handelt sich um einen Kompilierzeitfehler, wenn ein Ausdruck vom Typ `E` nicht mit dem Typ in einem Typmuster kompatibel ist, mit dem er übereinstimmt.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

None.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

None.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

None.

## <a name="design-meetings"></a>Treffen von Besprechungen

LDM hat diese Frage in Erwägung gezogen und gespürt, dass es sich um eine Fehler Behebungs Ebene handelt. Wir behandeln Sie als separate Sprachfunktion, da die Änderung nach der Veröffentlichung der Sprache eine vorwärts Inkompatibilität mit sich bringen würde. Für die Verwendung der vorgeschlagenen Änderung muss der Programmierer Sprachversion 7,1 angeben.
