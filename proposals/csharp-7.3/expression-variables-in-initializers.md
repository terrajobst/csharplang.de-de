---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483859"
---
# <a name="expression-variables-in-initializers"></a><span data-ttu-id="b04d7-101">Ausdrucksvariablen in Initialisierern</span><span class="sxs-lookup"><span data-stu-id="b04d7-101">Expression variables in initializers</span></span>

## <a name="summary"></a><span data-ttu-id="b04d7-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="b04d7-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b04d7-103">Wir erweitern die in C# 7 eingeführten Features, um Ausdrücke mit Ausdrucks Variablen (out-Variablen Deklarationen und Deklarations Muster) in feldinitialisierern, eigenschafteninitialisierern, ctor-Initialisierern und Abfrage Klauseln zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="b04d7-103">We extend the features introduced in C# 7 to permit expressions containing expression variables (out variable declarations and declaration patterns) in field initializers, property initializers, ctor-initializers, and query clauses.</span></span>

## <a name="motivation"></a><span data-ttu-id="b04d7-104">Motivation</span><span class="sxs-lookup"><span data-stu-id="b04d7-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b04d7-105">Dadurch werden einige der groben Kanten in der C# Sprache aufgrund fehlender Zeit abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="b04d7-105">This completes a couple of the rough edges left in the C# language due to lack of time.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b04d7-106">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="b04d7-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="b04d7-107">Wir entfernen die Einschränkung, die die Deklaration von Ausdrucks Variablen (out-Variablen Deklarationen und Deklarations Muster) in einem ctor-Initialisierer verhindert.</span><span class="sxs-lookup"><span data-stu-id="b04d7-107">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a ctor-initializer.</span></span> <span data-ttu-id="b04d7-108">Eine solche deklarierte Variable befindet sich im Bereich im Hauptteil des Konstruktors.</span><span class="sxs-lookup"><span data-stu-id="b04d7-108">Such a declared variable is in scope throughout the body of the constructor.</span></span>

<span data-ttu-id="b04d7-109">Wir entfernen die Einschränkung, die die Deklaration von Ausdrucks Variablen (out-Variablen Deklarationen und Deklarations Muster) in einem Feld-oder Eigenschafteninitialisierer verhindert.</span><span class="sxs-lookup"><span data-stu-id="b04d7-109">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a field or property initializer.</span></span> <span data-ttu-id="b04d7-110">Eine solche deklarierte Variable befindet sich im Gültigkeitsbereich des Initialisierungs Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="b04d7-110">Such a declared variable is in scope throughout the initializing expression.</span></span>

<span data-ttu-id="b04d7-111">Die Einschränkung, die die Deklaration von Ausdrucks Variablen (out-Variablen Deklarationen und Deklarations Muster) verhindert, wird in einer Abfrage Ausdrucks Klausel entfernt, die in den Text eines Lambda-Ausdrucks übersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="b04d7-111">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a query expression clause that is translated into the body of a lambda.</span></span> <span data-ttu-id="b04d7-112">Eine solche deklarierte Variable befindet sich im Gültigkeitsbereich des gesamten Ausdrucks der Abfrage Klausel.</span><span class="sxs-lookup"><span data-stu-id="b04d7-112">Such a declared variable is in scope throughout that expression of the query clause.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="b04d7-113">Nachteile</span><span class="sxs-lookup"><span data-stu-id="b04d7-113">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="b04d7-114">None.</span><span class="sxs-lookup"><span data-stu-id="b04d7-114">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="b04d7-115">Alternativen</span><span class="sxs-lookup"><span data-stu-id="b04d7-115">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="b04d7-116">Der geeignete Bereich für in diesen Kontexten deklarierte Ausdrucks Variablen ist nicht offensichtlich und verdient weitere LDM-Diskussionen.</span><span class="sxs-lookup"><span data-stu-id="b04d7-116">The appropriate scope for expression variables declared in these contexts is not obvious, and deserves further LDM discussion.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="b04d7-117">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="b04d7-117">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="b04d7-118">[] Was ist der geeignete Bereich für diese Variablen?</span><span class="sxs-lookup"><span data-stu-id="b04d7-118">[ ] What is the appropriate scope for these variables?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="b04d7-119">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="b04d7-119">Design meetings</span></span>

<span data-ttu-id="b04d7-120">None.</span><span class="sxs-lookup"><span data-stu-id="b04d7-120">None.</span></span>
