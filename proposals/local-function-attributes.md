---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509492"
---
# <a name="attributes-on-local-functions"></a><span data-ttu-id="ce217-101">Attribute für lokale Funktionen</span><span class="sxs-lookup"><span data-stu-id="ce217-101">Attributes on local functions</span></span>

## <a name="attributes"></a><span data-ttu-id="ce217-102">Attribute</span><span class="sxs-lookup"><span data-stu-id="ce217-102">Attributes</span></span>

<span data-ttu-id="ce217-103">Lokale Funktions Deklarationen dürfen jetzt über [Attribute](../spec/attributes.md)verfügen.</span><span class="sxs-lookup"><span data-stu-id="ce217-103">Local function declarations are now permitted to have [attributes](../spec/attributes.md).</span></span> <span data-ttu-id="ce217-104">Parameter und Typparameter für lokale Funktionen dürfen auch über Attribute verfügen.</span><span class="sxs-lookup"><span data-stu-id="ce217-104">Parameters and type parameters on local functions are also allowed to have attributes.</span></span>

<span data-ttu-id="ce217-105">Attribute mit einer bestimmten Bedeutung, wenn Sie auf eine Methode, deren Parameter oder die Typparameter angewendet werden, haben die gleiche Bedeutung, wenn Sie auf eine lokale Funktion, ihre Parameter oder die zugehörigen Typparameter angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="ce217-105">Attributes with a specified meaning when applied to a method, its parameters, or its type parameters will have the same meaning when applied to a local function, its parameters, or its type parameters, respectively.</span></span>

<span data-ttu-id="ce217-106">Eine lokale Funktion kann in demselben Sinne wie eine [bedingte Methode](../spec/attributes.md#the-conditional-attribute) bedingt erstellt werden, indem Sie mit einem `[ConditionalAttribute]`versehen wird.</span><span class="sxs-lookup"><span data-stu-id="ce217-106">A local function can be made conditional in the same sense as a [conditional method](../spec/attributes.md#the-conditional-attribute) by decorating it with a `[ConditionalAttribute]`.</span></span> <span data-ttu-id="ce217-107">Eine bedingte lokale Funktion muss ebenfalls `static`werden.</span><span class="sxs-lookup"><span data-stu-id="ce217-107">A conditional local function must also be `static`.</span></span> <span data-ttu-id="ce217-108">Alle Einschränkungen für bedingte Methoden gelten auch für bedingte lokale Funktionen, einschließlich, dass der Rückgabetyp `void`werden muss.</span><span class="sxs-lookup"><span data-stu-id="ce217-108">All restrictions on conditional methods also apply to conditional local functions, including that the return type must be `void`.</span></span>

## <a name="extern"></a><span data-ttu-id="ce217-109">Extern</span><span class="sxs-lookup"><span data-stu-id="ce217-109">Extern</span></span>

<span data-ttu-id="ce217-110">Der `extern` Modifizierer ist nun für lokale Funktionen zulässig.</span><span class="sxs-lookup"><span data-stu-id="ce217-110">The `extern` modifier is now permitted on local functions.</span></span> <span data-ttu-id="ce217-111">Dadurch wird die lokale Funktion extern im gleichen Sinne wie eine [externe Methode](../spec/classes.md#external-methods).</span><span class="sxs-lookup"><span data-stu-id="ce217-111">This makes the local function external in the same sense as an [external method](../spec/classes.md#external-methods).</span></span>

<span data-ttu-id="ce217-112">Ähnlich wie bei einer externen Methode muss der *lokale Funktions Text* einer externen lokalen Funktion ein Semikolon sein.</span><span class="sxs-lookup"><span data-stu-id="ce217-112">Similarly to an external method, the *local-function-body* of an external local function must be a semicolon.</span></span> <span data-ttu-id="ce217-113">Ein Semikolon " *local-Function-Body* " ist nur für eine externe lokale Funktion zulässig.</span><span class="sxs-lookup"><span data-stu-id="ce217-113">A semicolon *local-function-body* is only permitted on an external local function.</span></span> 

<span data-ttu-id="ce217-114">Eine externe lokale Funktion muss ebenfalls `static`werden.</span><span class="sxs-lookup"><span data-stu-id="ce217-114">An external local function must also be `static`.</span></span>

## <a name="syntax"></a><span data-ttu-id="ce217-115">Syntax</span><span class="sxs-lookup"><span data-stu-id="ce217-115">Syntax</span></span>

<span data-ttu-id="ce217-116">Die [Grammatik der lokalen Funktionen](csharp-7.0/local-functions.md#syntax-grammar) wird wie folgt geändert:</span><span class="sxs-lookup"><span data-stu-id="ce217-116">The [local functions grammar](csharp-7.0/local-functions.md#syntax-grammar) is modified as follows:</span></span>
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
