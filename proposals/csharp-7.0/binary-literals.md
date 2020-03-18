---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483433"
---
# <a name="binary-literals"></a>Binäre Literale

Es gibt eine relativ häufige Anforderung, binäre Literale zu C# und VB hinzuzufügen. Für Bitmasken (z. b. Flag-Aufstände) scheint dies wirklich nützlich zu sein, aber es wäre auch für Schulungszwecke gut geeignet.

Binäre Literale würden wie folgt aussehen:

```csharp
int nineteen = 0b10011;
```

Syntaktisch und semantisch sind Sie mit hexadezimalen Literalen identisch, mit Ausnahme der Verwendung `b`/`B` anstelle `x`/`X`, wobei nur Ziffern `0` und `1` und in Basis 2 anstelle von 16 interpretiert werden.

Es gibt wenig Kosten für die Implementierung dieser und wenig konzeptionellen Aufwand für Benutzer der Sprache.

## <a name="syntax"></a>Syntax

Die Grammatik sieht wie folgt aus:

```antlr
integer-literal:
    : ...
    | binary-integer-literal
    ;
binary-integer-literal:
    : `0b` binary-digits integer-type-suffix-opt
    | `0B` binary-digits integer-type-suffix-opt
    ;
binary-digits:
    : binary-digit
    | binary-digits binary-digit
    ;
binary-digit:
    : `0`
    | `1`
    ;
```
