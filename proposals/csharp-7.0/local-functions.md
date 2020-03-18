---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483445"
---
# <a name="local-functions"></a>Lokale Funktionen

Wir erweitern C# , um die Deklaration von Funktionen im Block Bereich zu unterstützen. Lokale Funktionen können (Capture)-Variablen aus dem einschließenden Gültigkeitsbereich verwenden.

Der Compiler erkennt anhand der Datenflussanalyse, welche Variablen eine lokale Funktion verwendet, bevor Sie Ihr einen Wert zuweist. Jeder Aufrufe der Funktion erfordert, dass solche Variablen definitiv zugewiesen werden. Auf ähnliche Weise bestimmt der Compiler, welche Variablen bei Rückgabe definitiv zugewiesen werden. Diese Variablen werden als definitiv zugewiesen, nachdem die lokale Funktion aufgerufen wurde.

Lokale Funktionen können vor ihrer Definition von einem lexikalischen Punkt aus aufgerufen werden. Lokale Funktions Deklarations Anweisungen verursachen keine Warnung, wenn Sie nicht erreichbar sind.

TODO: _Spezifikation schreiben_

## <a name="syntax-grammar"></a>Syntax Grammatik

Diese Grammatik wird als diff von der aktuellen Spezifikations Grammatik dargestellt.

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

Lokale Funktionen können im einschließenden Bereich definierte Variablen verwenden. Die aktuelle Implementierung erfordert, dass jede in einer lokalen Funktion gelesene Variable definitiv zugewiesen wird, als ob die lokale Funktion an ihrer Definitions Stelle ausgeführt wird. Außerdem muss die Definition der lokalen Funktion an jedem beliebigen Verwendungs Punkt "ausgeführt" sein.

Nachdem Sie etwas ausprobiert haben (es ist z. b. nicht möglich, zwei gegenseitig rekursive lokale Funktionen zu definieren), haben wir seitdem überarbeitet, wie die definitive Zuweisung funktionieren soll. Die Revision (noch nicht implementiert) besteht darin, dass alle lokalen Variablen, die in einer lokalen Funktion gelesen werden, bei jedem Aufruf der lokalen Funktion definitiv zugewiesen werden müssen. Das ist wirklich etwas feiner als das, und es gibt eine Reihe von verbleibenden arbeiten, um die Arbeit zu vereinfachen. Nachdem der Vorgang abgeschlossen ist, können Sie Ihre lokalen Funktionen an das Ende des einschließenden Blocks verschieben.

Die neuen eindeutigen Zuweisungs Regeln sind nicht mit dem Ableiten des Rückgabe Typs einer lokalen Funktion kompatibel. Daher wird die Unterstützung für das Ableiten des Rückgabe Typs wahrscheinlich entfernt.

Wenn Sie eine lokale Funktion nicht in einen Delegaten konvertieren, erfolgt die Erfassung in Frames, die Werttypen sind. Dies bedeutet, dass Sie keinen GC-Druck durch die Verwendung lokaler Funktionen mit der Erfassung erhalten.

### <a name="reachability"></a>Erreichbarkeit

Wir fügen die Spezifikation hinzu.

> Der Text eines Lambda-Ausdrucks oder einer lokalen Funktion mit Anweisungs Körper ist als erreichbar.
