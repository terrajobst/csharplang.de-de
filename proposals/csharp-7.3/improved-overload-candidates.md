---
ms.openlocfilehash: 833ea0469149cbd434e8950e844740a3adb4d386
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483577"
---
# <a name="improved-overload-candidates"></a><span data-ttu-id="ce720-101">Verbesserte Überladungskandidaten</span><span class="sxs-lookup"><span data-stu-id="ce720-101">Improved overload candidates</span></span>

## <a name="summary"></a><span data-ttu-id="ce720-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="ce720-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ce720-103">Die Regeln für die Überladungs Auflösung wurden in nahezu C# jedem sprach Update aktualisiert, um die Möglichkeiten für Programmierer zu verbessern, indem mehrdeutige Aufrufe ausgewählt werden.</span><span class="sxs-lookup"><span data-stu-id="ce720-103">The overload resolution rules have been updated in nearly every C# language update to improve the experience for programmers, making ambiguous invocations select the "obvious" choice.</span></span> <span data-ttu-id="ce720-104">Dies muss sorgfältig durchgeführt werden, um die Abwärtskompatibilität zu gewährleisten. da wir jedoch in der Regel die Fehlerfälle auflösen, funktionieren diese Verbesserungen in der Regel gut.</span><span class="sxs-lookup"><span data-stu-id="ce720-104">This has to be done carefully to preserve backward compatibility, but since we are usually resolving what would otherwise be error cases, these enhancements usually work out nicely.</span></span>

1. <span data-ttu-id="ce720-105">Wenn eine Methoden Gruppe sowohl Instanzmember als auch statische Member enthält, werden die Instanzmember verworfen, wenn Sie ohne instanzempfänger oder-Kontext aufgerufen werden, und die statischen Member verwerfen, wenn Sie mit einem instanzempfänger aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="ce720-105">When a method group contains both instance and static members, we discard the instance members if invoked without an instance receiver or context, and discard the static members if invoked with an instance receiver.</span></span> <span data-ttu-id="ce720-106">Wenn kein Empfänger vorhanden ist, schließen wir nur statische Member in einen statischen Kontext ein, andernfalls sowohl statische als auch Instanzmember.</span><span class="sxs-lookup"><span data-stu-id="ce720-106">When there is no receiver, we include only static members in a static context, otherwise both static and instance members.</span></span> <span data-ttu-id="ce720-107">Wenn der Empfänger aufgrund einer Farb farbsituation mehrdeutig eine Instanz oder einen Typ ist, enthalten wir beides.</span><span class="sxs-lookup"><span data-stu-id="ce720-107">When the receiver is ambiguously an instance or type due to a color-color situation, we include both.</span></span> <span data-ttu-id="ce720-108">Ein statischer Kontext, in dem ein impliziter instanzempfänger nicht verwendet werden kann, enthält den Hauptteil der Member, in denen keine definiert ist, wie z. b. statische Member, und Orte, an denen dies nicht verwendet werden kann, wie z. b. Feldinitialisierer und Konstruktorinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="ce720-108">A static context, where an implicit this instance receiver cannot be used, includes the body of members where no this is defined, such as static members, as well as places where this cannot be used, such as field initializers and constructor-initializers.</span></span>
2. <span data-ttu-id="ce720-109">Wenn eine Methodengruppe einige generische Methoden enthält, deren Typargumente ihre Einschränkungen nicht erfüllen, werden diese Member aus dem Satz von Kandidaten entfernt.</span><span class="sxs-lookup"><span data-stu-id="ce720-109">When a method group contains some generic methods whose type arguments do not satisfy their constraints, these members are removed from the candidate set.</span></span>
3. <span data-ttu-id="ce720-110">Für eine Methodengruppenkonvertierung werden Kandidatenmethoden, deren Rückgabetyp nicht mit dem des Delegaten übereinstimmt, aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="ce720-110">For a method group conversion, candidate methods whose return type doesn't match up with the delegate's return type are removed from the set.</span></span>