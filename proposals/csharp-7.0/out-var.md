---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483505"
---
# <a name="out-variable-declarations"></a>out-Variablendeklaration

Mit der *Deklaration der Out-Variablen Deklaration* kann eine Variable an dem Speicherort deklariert werden, an dem Sie als `out` Argument an Sie übermittelt wird.

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

Eine auf diese Weise deklarierte Variable wird als *out-Variable*bezeichnet. Sie können das kontextabhängige Schlüsselwort `var` für den Variablentyp verwenden. Der Gültigkeitsbereich ist derselbe wie für eine *Pattern-Variable* , die durch Muster Übereinstimmung eingeführt wurde.

Gemäß der Sprachspezifikation (Abschnitt 7.6.7 Element Access) enthält die Argument-List eines Element Zugriffs (Index Ausdruck) keine ref-oder out-Argumente. Allerdings werden Sie vom Compiler für verschiedene Szenarien zugelassen, z. b. Indexer, die in Metadaten deklariert sind, die `out`akzeptieren.

Innerhalb des Gültigkeits Bereichs einer lokalen Variablen, die von einem argument_value eingeführt wurde, handelt es sich um einen Kompilierzeitfehler, der auf diese lokale Variable in einer Textposition verweist, die der Deklaration vorangestellt ist.

Es ist auch ein Fehler, auf eine implizit typisierte (§ 8.5.1) out-Variable in derselben Argumentliste zu verweisen, die sofort Ihre Deklaration enthält.

Die Überladungs Auflösung wird wie folgt geändert:

Wir fügen eine neue Konvertierung hinzu:

> Es gibt eine *Konvertierung von einem Ausdruck* aus einer implizit typisierten out-Variablen Deklaration in jeden Typ.

Außerdem

> Der Typ eines explizit typisierten out-Variablen Arguments ist der deklarierte Typ.

und

> Ein implizit typisiertes out-Variablen Argument weist keinen Typ auf.

Die *Konvertierung von einem Ausdruck* aus einer implizit typisierten out-Variablen Deklaration ist nicht besser als eine andere *Konvertierung von Ausdruck*.

Der Typ der implizit typisierten out-Variablen ist der Typ des entsprechenden Parameters in der Signatur der Methode, die von der Überladungs Auflösung ausgewählt wird.

Der neue Syntax Knoten `DeclarationExpressionSyntax` wird zur Darstellung der Deklaration in einem out var-Argument hinzugefügt.
