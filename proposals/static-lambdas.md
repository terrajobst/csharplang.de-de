---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "79483997"
---
# <a name="static-lambdas"></a><span data-ttu-id="a67a9-101">Statische Lambdas</span><span class="sxs-lookup"><span data-stu-id="a67a9-101">Static lambdas</span></span>

## <a name="summary"></a><span data-ttu-id="a67a9-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="a67a9-102">Summary</span></span>

<span data-ttu-id="a67a9-103">Unterstützung von Lambdas, die das Erfassen des Zustands aus dem einschließenden Bereich nicht zulassen.</span><span class="sxs-lookup"><span data-stu-id="a67a9-103">Support lambdas that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="a67a9-104">Motivation</span><span class="sxs-lookup"><span data-stu-id="a67a9-104">Motivation</span></span>

<span data-ttu-id="a67a9-105">Vermeiden Sie unbeabsichtigt das Erfassen des Zustands aus dem einschließenden Kontext.</span><span class="sxs-lookup"><span data-stu-id="a67a9-105">Avoid unintentionally capturing state from the enclosing context.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a67a9-106">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="a67a9-106">Detailed design</span></span>

<span data-ttu-id="a67a9-107">Ein Lambdas mit `static` kann den Zustand nicht aus dem einschließenden Bereich erfassen.</span><span class="sxs-lookup"><span data-stu-id="a67a9-107">A lambdas with `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="a67a9-108">Demzufolge sind lokale, Parameter und `this` aus dem einschließenden Bereich innerhalb eines `static` Lambda-Ausdrucks nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a67a9-108">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` lambda.</span></span>

<span data-ttu-id="a67a9-109">Ein `static` Lambda kann nicht auf Instanzmember von einem impliziten oder expliziten `this` oder `base` Verweis verweisen.</span><span class="sxs-lookup"><span data-stu-id="a67a9-109">A `static` lambda cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="a67a9-110">Ein `static` Lambda kann auf `static` Member aus dem einschließenden Bereich verweisen.</span><span class="sxs-lookup"><span data-stu-id="a67a9-110">A `static` lambda may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="a67a9-111">Ein `static` Lambda-Ausdruck kann auf `constant` Definitionen aus dem einschließenden Bereich verweisen.</span><span class="sxs-lookup"><span data-stu-id="a67a9-111">A `static` lambda may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="a67a9-112">`nameof()` in einem `static` Lambda kann auf lokale, Parameter oder `this` oder `base` aus dem einschließenden Bereich verweisen.</span><span class="sxs-lookup"><span data-stu-id="a67a9-112">`nameof()` in a `static` lambda may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="a67a9-113">Zugriffsregeln für `private` Elemente im einschließenden Bereich sind für `static` und nicht`static` Lambdas identisch.</span><span class="sxs-lookup"><span data-stu-id="a67a9-113">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` lambdas.</span></span>

<span data-ttu-id="a67a9-114">Es wird keine Garantie gegeben, ob eine `static` Lambda-Definition als `static` Methode in den Metadaten ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="a67a9-114">No guarantee is made as to whether a `static` lambda definition is emitted as a `static` method in metadata.</span></span> <span data-ttu-id="a67a9-115">Dies wird von der compilerimplementierung bis zur Optimierung verbleiben.</span><span class="sxs-lookup"><span data-stu-id="a67a9-115">This is left up to the compiler implementation to optimize.</span></span>

<span data-ttu-id="a67a9-116">Eine nicht`static` lokale Funktion oder ein Lambda-Ausdruck kann den Zustand von einem einschließenden `static` Lambda aufzeichnen, aber keinen Zustand außerhalb des einschließenden `static` Lambda-Ausdrucks erfassen.</span><span class="sxs-lookup"><span data-stu-id="a67a9-116">A non-`static` local function or lambda can capture state from an enclosing `static` lambda but cannot capture state outside the enclosing `static` lambda.</span></span>

<span data-ttu-id="a67a9-117">Ein `static` Lambda-Ausdruck kann in einer Ausdrucks Baumstruktur verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a67a9-117">A `static` lambda can be used in an expression tree.</span></span>

<span data-ttu-id="a67a9-118">Wenn Sie den `static` Modifizierer aus einem Lambda in einem gültigen Programm entfernen, wird die Bedeutung des Programms nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="a67a9-118">Removing the `static` modifier from a lambda in a valid program does not change the meaning of the program.</span></span>
