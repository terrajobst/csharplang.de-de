---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483859"
---
# <a name="expression-variables-in-initializers"></a>Ausdrucksvariablen in Initialisierern

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Wir erweitern die in C# 7 eingeführten Features, um Ausdrücke mit Ausdrucks Variablen (out-Variablen Deklarationen und Deklarations Muster) in feldinitialisierern, eigenschafteninitialisierern, ctor-Initialisierern und Abfrage Klauseln zuzulassen.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Dadurch werden einige der groben Kanten in der C# Sprache aufgrund fehlender Zeit abgeschlossen.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Wir entfernen die Einschränkung, die die Deklaration von Ausdrucks Variablen (out-Variablen Deklarationen und Deklarations Muster) in einem ctor-Initialisierer verhindert. Eine solche deklarierte Variable befindet sich im Bereich im Hauptteil des Konstruktors.

Wir entfernen die Einschränkung, die die Deklaration von Ausdrucks Variablen (out-Variablen Deklarationen und Deklarations Muster) in einem Feld-oder Eigenschafteninitialisierer verhindert. Eine solche deklarierte Variable befindet sich im Gültigkeitsbereich des Initialisierungs Ausdrucks.

Die Einschränkung, die die Deklaration von Ausdrucks Variablen (out-Variablen Deklarationen und Deklarations Muster) verhindert, wird in einer Abfrage Ausdrucks Klausel entfernt, die in den Text eines Lambda-Ausdrucks übersetzt wird. Eine solche deklarierte Variable befindet sich im Gültigkeitsbereich des gesamten Ausdrucks der Abfrage Klausel.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

None.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Der geeignete Bereich für in diesen Kontexten deklarierte Ausdrucks Variablen ist nicht offensichtlich und verdient weitere LDM-Diskussionen.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

- [] Was ist der geeignete Bereich für diese Variablen?

## <a name="design-meetings"></a>Treffen von Besprechungen

None.
