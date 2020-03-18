---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "79484009"
---
# <a name="static-local-functions"></a>Statische lokale Funktionen

## <a name="summary"></a>Zusammenfassung

Unterstützung lokaler Funktionen, die das Erfassen des Zustands aus dem einschließenden Bereich nicht zulassen.

## <a name="motivation"></a>Motivation

Vermeiden Sie unbeabsichtigt das Erfassen des Zustands aus dem einschließenden Kontext.
Ermöglicht die Verwendung lokaler Funktionen in Szenarien, in denen eine `static` Methode erforderlich ist.

## <a name="detailed-design"></a>Detaillierter Entwurf

Eine lokale Funktion, die deklariert wurde `static` kann den Zustand nicht aus dem einschließenden Bereich erfassen.
Demzufolge sind lokale, Parameter und `this` aus dem einschließenden Bereich in einer `static` lokalen Funktion nicht verfügbar.

Eine `static` lokale Funktion kann nicht auf Instanzmember von einem impliziten oder expliziten `this` oder `base` Verweis verweisen.

Eine `static` lokale Funktion kann auf `static` Member aus dem einschließenden Bereich verweisen.

Eine `static` lokale Funktion kann auf `constant` Definitionen aus dem einschließenden Bereich verweisen.

`nameof()` in einer `static` lokalen Funktion kann auf lokale, Parameter oder `this` oder `base` aus dem einschließenden Bereich verweisen.

Zugriffsregeln für `private` Elemente im einschließenden Bereich sind für `static` und nicht`static` lokale Funktionen identisch.

Eine `static` lokale Funktionsdefinition wird als `static` Methode in den Metadaten ausgegeben, auch wenn Sie nur in einem Delegaten verwendet wird.

Eine nicht`static` lokale Funktion oder ein Lambda-Ausdruck kann den Zustand von einer einschließenden `static` lokalen Funktion erfassen, aber keinen Zustand außerhalb der einschließenden `static` lokalen Funktion erfassen.

Eine `static` lokale Funktion kann nicht in einer Ausdrucks Baumstruktur aufgerufen werden.

Ein-Aufrufe an eine lokale Funktion wird als `call` ausgegeben, anstatt `callvirt`, unabhängig davon, ob die lokale Funktion `static`ist.

Die Überladungs Auflösung eines Aufrufes in einer lokalen Funktion ist nicht davon betroffen, ob die lokale Funktion `static`ist.

Wenn Sie den `static` Modifizierer aus einer lokalen Funktion in einem gültigen Programm entfernen, wird die Bedeutung des Programms nicht geändert.

## <a name="design-meetings"></a>Treffen von Besprechungen

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
