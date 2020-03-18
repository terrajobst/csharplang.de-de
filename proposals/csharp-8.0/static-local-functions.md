---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "79484009"
---
# <a name="static-local-functions"></a><span data-ttu-id="84570-101">Statische lokale Funktionen</span><span class="sxs-lookup"><span data-stu-id="84570-101">Static local functions</span></span>

## <a name="summary"></a><span data-ttu-id="84570-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="84570-102">Summary</span></span>

<span data-ttu-id="84570-103">Unterstützung lokaler Funktionen, die das Erfassen des Zustands aus dem einschließenden Bereich nicht zulassen.</span><span class="sxs-lookup"><span data-stu-id="84570-103">Support local functions that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="84570-104">Motivation</span><span class="sxs-lookup"><span data-stu-id="84570-104">Motivation</span></span>

<span data-ttu-id="84570-105">Vermeiden Sie unbeabsichtigt das Erfassen des Zustands aus dem einschließenden Kontext.</span><span class="sxs-lookup"><span data-stu-id="84570-105">Avoid unintentionally capturing state from the enclosing context.</span></span>
<span data-ttu-id="84570-106">Ermöglicht die Verwendung lokaler Funktionen in Szenarien, in denen eine `static` Methode erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="84570-106">Allow local functions to be used in scenarios where a `static` method is required.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="84570-107">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="84570-107">Detailed design</span></span>

<span data-ttu-id="84570-108">Eine lokale Funktion, die deklariert wurde `static` kann den Zustand nicht aus dem einschließenden Bereich erfassen.</span><span class="sxs-lookup"><span data-stu-id="84570-108">A local function declared `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="84570-109">Demzufolge sind lokale, Parameter und `this` aus dem einschließenden Bereich in einer `static` lokalen Funktion nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="84570-109">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` local function.</span></span>

<span data-ttu-id="84570-110">Eine `static` lokale Funktion kann nicht auf Instanzmember von einem impliziten oder expliziten `this` oder `base` Verweis verweisen.</span><span class="sxs-lookup"><span data-stu-id="84570-110">A `static` local function cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="84570-111">Eine `static` lokale Funktion kann auf `static` Member aus dem einschließenden Bereich verweisen.</span><span class="sxs-lookup"><span data-stu-id="84570-111">A `static` local function may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="84570-112">Eine `static` lokale Funktion kann auf `constant` Definitionen aus dem einschließenden Bereich verweisen.</span><span class="sxs-lookup"><span data-stu-id="84570-112">A `static` local function may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="84570-113">`nameof()` in einer `static` lokalen Funktion kann auf lokale, Parameter oder `this` oder `base` aus dem einschließenden Bereich verweisen.</span><span class="sxs-lookup"><span data-stu-id="84570-113">`nameof()` in a `static` local function may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="84570-114">Zugriffsregeln für `private` Elemente im einschließenden Bereich sind für `static` und nicht`static` lokale Funktionen identisch.</span><span class="sxs-lookup"><span data-stu-id="84570-114">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` local functions.</span></span>

<span data-ttu-id="84570-115">Eine `static` lokale Funktionsdefinition wird als `static` Methode in den Metadaten ausgegeben, auch wenn Sie nur in einem Delegaten verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="84570-115">A `static` local function definition is emitted as a `static` method in metadata, even if only used in a delegate.</span></span>

<span data-ttu-id="84570-116">Eine nicht`static` lokale Funktion oder ein Lambda-Ausdruck kann den Zustand von einer einschließenden `static` lokalen Funktion erfassen, aber keinen Zustand außerhalb der einschließenden `static` lokalen Funktion erfassen.</span><span class="sxs-lookup"><span data-stu-id="84570-116">A non-`static` local function or lambda can capture state from an enclosing `static` local function but cannot capture state outside the enclosing `static` local function.</span></span>

<span data-ttu-id="84570-117">Eine `static` lokale Funktion kann nicht in einer Ausdrucks Baumstruktur aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="84570-117">A `static` local function cannot be invoked in an expression tree.</span></span>

<span data-ttu-id="84570-118">Ein-Aufrufe an eine lokale Funktion wird als `call` ausgegeben, anstatt `callvirt`, unabhängig davon, ob die lokale Funktion `static`ist.</span><span class="sxs-lookup"><span data-stu-id="84570-118">A call to a local function is emitted as `call` rather than `callvirt`, regardless of whether the local function is `static`.</span></span>

<span data-ttu-id="84570-119">Die Überladungs Auflösung eines Aufrufes in einer lokalen Funktion ist nicht davon betroffen, ob die lokale Funktion `static`ist.</span><span class="sxs-lookup"><span data-stu-id="84570-119">Overload resolution of a call within a local function not affected by whether the local function is `static`.</span></span>

<span data-ttu-id="84570-120">Wenn Sie den `static` Modifizierer aus einer lokalen Funktion in einem gültigen Programm entfernen, wird die Bedeutung des Programms nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="84570-120">Removing the `static` modifier from a local function in a valid program does not change the meaning of the program.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="84570-121">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="84570-121">Design meetings</span></span>

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
