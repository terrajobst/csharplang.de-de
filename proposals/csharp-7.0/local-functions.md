---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483445"
---
# <a name="local-functions"></a><span data-ttu-id="fd11f-101">Lokale Funktionen</span><span class="sxs-lookup"><span data-stu-id="fd11f-101">Local functions</span></span>

<span data-ttu-id="fd11f-102">Wir erweitern C# , um die Deklaration von Funktionen im Block Bereich zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="fd11f-102">We extend C# to support the declaration of functions in block scope.</span></span> <span data-ttu-id="fd11f-103">Lokale Funktionen können (Capture)-Variablen aus dem einschließenden Gültigkeitsbereich verwenden.</span><span class="sxs-lookup"><span data-stu-id="fd11f-103">Local functions may use (capture) variables from the enclosing scope.</span></span>

<span data-ttu-id="fd11f-104">Der Compiler erkennt anhand der Datenflussanalyse, welche Variablen eine lokale Funktion verwendet, bevor Sie Ihr einen Wert zuweist.</span><span class="sxs-lookup"><span data-stu-id="fd11f-104">The compiler uses flow analysis to detect which variables a local function uses before assigning it a value.</span></span> <span data-ttu-id="fd11f-105">Jeder Aufrufe der Funktion erfordert, dass solche Variablen definitiv zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="fd11f-105">Every call of the function requires such variables to be definitely assigned.</span></span> <span data-ttu-id="fd11f-106">Auf ähnliche Weise bestimmt der Compiler, welche Variablen bei Rückgabe definitiv zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="fd11f-106">Similarly the compiler determines which variables are definitely assigned on return.</span></span> <span data-ttu-id="fd11f-107">Diese Variablen werden als definitiv zugewiesen, nachdem die lokale Funktion aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="fd11f-107">Such variables are considered definitely assigned after the local function is invoked.</span></span>

<span data-ttu-id="fd11f-108">Lokale Funktionen können vor ihrer Definition von einem lexikalischen Punkt aus aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="fd11f-108">Local functions may be called from a lexical point before its definition.</span></span> <span data-ttu-id="fd11f-109">Lokale Funktions Deklarations Anweisungen verursachen keine Warnung, wenn Sie nicht erreichbar sind.</span><span class="sxs-lookup"><span data-stu-id="fd11f-109">Local function declaration statements do not cause a warning when they are not reachable.</span></span>

<span data-ttu-id="fd11f-110">TODO: _Spezifikation schreiben_</span><span class="sxs-lookup"><span data-stu-id="fd11f-110">TODO: _WRITE SPEC_</span></span>

## <a name="syntax-grammar"></a><span data-ttu-id="fd11f-111">Syntax Grammatik</span><span class="sxs-lookup"><span data-stu-id="fd11f-111">Syntax grammar</span></span>

<span data-ttu-id="fd11f-112">Diese Grammatik wird als diff von der aktuellen Spezifikations Grammatik dargestellt.</span><span class="sxs-lookup"><span data-stu-id="fd11f-112">This grammar is represented as a diff from the current spec grammar.</span></span>

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

<span data-ttu-id="fd11f-113">Lokale Funktionen können im einschließenden Bereich definierte Variablen verwenden.</span><span class="sxs-lookup"><span data-stu-id="fd11f-113">Local functions may use variables defined in the enclosing scope.</span></span> <span data-ttu-id="fd11f-114">Die aktuelle Implementierung erfordert, dass jede in einer lokalen Funktion gelesene Variable definitiv zugewiesen wird, als ob die lokale Funktion an ihrer Definitions Stelle ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fd11f-114">The current implementation requires that every variable read inside a local function be definitely assigned, as if executing the local function at its point of definition.</span></span> <span data-ttu-id="fd11f-115">Außerdem muss die Definition der lokalen Funktion an jedem beliebigen Verwendungs Punkt "ausgeführt" sein.</span><span class="sxs-lookup"><span data-stu-id="fd11f-115">Also, the local function definition must have been "executed" at any use point.</span></span>

<span data-ttu-id="fd11f-116">Nachdem Sie etwas ausprobiert haben (es ist z. b. nicht möglich, zwei gegenseitig rekursive lokale Funktionen zu definieren), haben wir seitdem überarbeitet, wie die definitive Zuweisung funktionieren soll.</span><span class="sxs-lookup"><span data-stu-id="fd11f-116">After experimenting with that a bit (for example, it is not possible to define two mutually recursive local functions), we've since revised how we want the definite assignment to work.</span></span> <span data-ttu-id="fd11f-117">Die Revision (noch nicht implementiert) besteht darin, dass alle lokalen Variablen, die in einer lokalen Funktion gelesen werden, bei jedem Aufruf der lokalen Funktion definitiv zugewiesen werden müssen.</span><span class="sxs-lookup"><span data-stu-id="fd11f-117">The revision (not yet implemented) is that all local variables read in a local function must be definitely assigned at each invocation of the local function.</span></span> <span data-ttu-id="fd11f-118">Das ist wirklich etwas feiner als das, und es gibt eine Reihe von verbleibenden arbeiten, um die Arbeit zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="fd11f-118">That's actually more subtle than it sounds, and there is a bunch of work remaining to make it work.</span></span> <span data-ttu-id="fd11f-119">Nachdem der Vorgang abgeschlossen ist, können Sie Ihre lokalen Funktionen an das Ende des einschließenden Blocks verschieben.</span><span class="sxs-lookup"><span data-stu-id="fd11f-119">Once it is done you'll be able to move your local functions to the end of its enclosing block.</span></span>

<span data-ttu-id="fd11f-120">Die neuen eindeutigen Zuweisungs Regeln sind nicht mit dem Ableiten des Rückgabe Typs einer lokalen Funktion kompatibel. Daher wird die Unterstützung für das Ableiten des Rückgabe Typs wahrscheinlich entfernt.</span><span class="sxs-lookup"><span data-stu-id="fd11f-120">The new definite assignment rules are incompatible with inferring the return type of a local function, so we'll likely be removing support for inferring the return type.</span></span>

<span data-ttu-id="fd11f-121">Wenn Sie eine lokale Funktion nicht in einen Delegaten konvertieren, erfolgt die Erfassung in Frames, die Werttypen sind.</span><span class="sxs-lookup"><span data-stu-id="fd11f-121">Unless you convert a local function to a delegate, capturing is done into frames that are value types.</span></span> <span data-ttu-id="fd11f-122">Dies bedeutet, dass Sie keinen GC-Druck durch die Verwendung lokaler Funktionen mit der Erfassung erhalten.</span><span class="sxs-lookup"><span data-stu-id="fd11f-122">That means you don't get any GC pressure from using local functions with capturing.</span></span>

### <a name="reachability"></a><span data-ttu-id="fd11f-123">Erreichbarkeit</span><span class="sxs-lookup"><span data-stu-id="fd11f-123">Reachability</span></span>

<span data-ttu-id="fd11f-124">Wir fügen die Spezifikation hinzu.</span><span class="sxs-lookup"><span data-stu-id="fd11f-124">We add to the spec</span></span>

> <span data-ttu-id="fd11f-125">Der Text eines Lambda-Ausdrucks oder einer lokalen Funktion mit Anweisungs Körper ist als erreichbar.</span><span class="sxs-lookup"><span data-stu-id="fd11f-125">The body of a statement-bodied lambda expression or local function is considered reachable.</span></span>
