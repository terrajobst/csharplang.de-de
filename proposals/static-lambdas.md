---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "79483997"
---
# <a name="static-lambdas"></a>Statische Lambdas

## <a name="summary"></a>Zusammenfassung

Unterstützung von Lambdas, die das Erfassen des Zustands aus dem einschließenden Bereich nicht zulassen.

## <a name="motivation"></a>Motivation

Vermeiden Sie unbeabsichtigt das Erfassen des Zustands aus dem einschließenden Kontext.

## <a name="detailed-design"></a>Detaillierter Entwurf

Ein Lambdas mit `static` kann den Zustand nicht aus dem einschließenden Bereich erfassen.
Demzufolge sind lokale, Parameter und `this` aus dem einschließenden Bereich innerhalb eines `static` Lambda-Ausdrucks nicht verfügbar.

Ein `static` Lambda kann nicht auf Instanzmember von einem impliziten oder expliziten `this` oder `base` Verweis verweisen.

Ein `static` Lambda kann auf `static` Member aus dem einschließenden Bereich verweisen.

Ein `static` Lambda-Ausdruck kann auf `constant` Definitionen aus dem einschließenden Bereich verweisen.

`nameof()` in einem `static` Lambda kann auf lokale, Parameter oder `this` oder `base` aus dem einschließenden Bereich verweisen.

Zugriffsregeln für `private` Elemente im einschließenden Bereich sind für `static` und nicht`static` Lambdas identisch.

Es wird keine Garantie gegeben, ob eine `static` Lambda-Definition als `static` Methode in den Metadaten ausgegeben wird. Dies wird von der compilerimplementierung bis zur Optimierung verbleiben.

Eine nicht`static` lokale Funktion oder ein Lambda-Ausdruck kann den Zustand von einem einschließenden `static` Lambda aufzeichnen, aber keinen Zustand außerhalb des einschließenden `static` Lambda-Ausdrucks erfassen.

Ein `static` Lambda-Ausdruck kann in einer Ausdrucks Baumstruktur verwendet werden.

Wenn Sie den `static` Modifizierer aus einem Lambda in einem gültigen Programm entfernen, wird die Bedeutung des Programms nicht geändert.
