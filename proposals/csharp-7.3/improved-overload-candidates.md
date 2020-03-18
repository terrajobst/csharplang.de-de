---
ms.openlocfilehash: 833ea0469149cbd434e8950e844740a3adb4d386
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483577"
---
# <a name="improved-overload-candidates"></a>Verbesserte Überladungskandidaten

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Die Regeln für die Überladungs Auflösung wurden in nahezu C# jedem sprach Update aktualisiert, um die Möglichkeiten für Programmierer zu verbessern, indem mehrdeutige Aufrufe ausgewählt werden. Dies muss sorgfältig durchgeführt werden, um die Abwärtskompatibilität zu gewährleisten. da wir jedoch in der Regel die Fehlerfälle auflösen, funktionieren diese Verbesserungen in der Regel gut.

1. Wenn eine Methoden Gruppe sowohl Instanzmember als auch statische Member enthält, werden die Instanzmember verworfen, wenn Sie ohne instanzempfänger oder-Kontext aufgerufen werden, und die statischen Member verwerfen, wenn Sie mit einem instanzempfänger aufgerufen werden. Wenn kein Empfänger vorhanden ist, schließen wir nur statische Member in einen statischen Kontext ein, andernfalls sowohl statische als auch Instanzmember. Wenn der Empfänger aufgrund einer Farb farbsituation mehrdeutig eine Instanz oder einen Typ ist, enthalten wir beides. Ein statischer Kontext, in dem ein impliziter instanzempfänger nicht verwendet werden kann, enthält den Hauptteil der Member, in denen keine definiert ist, wie z. b. statische Member, und Orte, an denen dies nicht verwendet werden kann, wie z. b. Feldinitialisierer und Konstruktorinitialisierer.
2. Wenn eine Methodengruppe einige generische Methoden enthält, deren Typargumente ihre Einschränkungen nicht erfüllen, werden diese Member aus dem Satz von Kandidaten entfernt.
3. Für eine Methodengruppenkonvertierung werden Kandidatenmethoden, deren Rückgabetyp nicht mit dem des Delegaten übereinstimmt, aus dem Satz entfernt.
