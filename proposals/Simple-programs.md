---
ms.openlocfilehash: 54ae4ffabde6dca49b7e6bfb626d65837eabc8f5
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281943"
---
# <a name="simple-programs"></a><span data-ttu-id="a1119-101">Einfache Programme</span><span class="sxs-lookup"><span data-stu-id="a1119-101">Simple programs</span></span>

* <span data-ttu-id="a1119-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="a1119-102">[x] Proposed</span></span>
* <span data-ttu-id="a1119-103">[x] Prototyp: gestartet</span><span class="sxs-lookup"><span data-stu-id="a1119-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="a1119-104">[] Implementierung: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="a1119-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="a1119-105">[] Spezifikation: nicht gestartet</span><span class="sxs-lookup"><span data-stu-id="a1119-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="a1119-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="a1119-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a1119-107">Ermöglicht das Auftreten einer Sequenz von- *Anweisungen* direkt vor den *namespace_member_declaration*s einer *compilation_unit* (d. h. Quelldatei).</span><span class="sxs-lookup"><span data-stu-id="a1119-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="a1119-108">Die Semantik besteht darin, dass die folgende Typdeklaration, wenn eine solche Sequenz von *Anweisungen* vorhanden ist, den tatsächlichen Typnamen und den Methodennamen Modulo ergibt:</span><span class="sxs-lookup"><span data-stu-id="a1119-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="a1119-109">Siehe auch https://github.com/dotnet/csharplang/issues/3117.</span><span class="sxs-lookup"><span data-stu-id="a1119-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="a1119-110">Motivation</span><span class="sxs-lookup"><span data-stu-id="a1119-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a1119-111">Aufgrund der Notwendigkeit einer expliziten `Main` Methode gibt es auch eine bestimmte Anzahl von Code Steinen, die auch die einfachsten Programme umgeben.</span><span class="sxs-lookup"><span data-stu-id="a1119-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="a1119-112">Dies scheint das Erlernen von Sprache und Programm Klarheit zu erreichen.</span><span class="sxs-lookup"><span data-stu-id="a1119-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="a1119-113">Das Hauptziel des Features besteht daher darin, Programme ohne C# unnötige Bausteine für die Lernprogramme und die Klarheit des Codes zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="a1119-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a1119-114">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="a1119-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="a1119-115">Syntax</span><span class="sxs-lookup"><span data-stu-id="a1119-115">Syntax</span></span>

<span data-ttu-id="a1119-116">Die einzige zusätzliche Syntax besteht darin, eine Sequenz *von-* Anweisungen in einer Kompilierungseinheit direkt vor den *namespace_member_declaration*s zuzulassen:</span><span class="sxs-lookup"><span data-stu-id="a1119-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="a1119-117">Nur ein *compilation_unit* darf *Anweisung*s enthalten.</span><span class="sxs-lookup"><span data-stu-id="a1119-117">Only one *compilation_unit* is allowed to have *statement*s.</span></span> 

<span data-ttu-id="a1119-118">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a1119-118">Example:</span></span>

``` c#
if (System.Environment.CommandLine.Length == 0
    || !int.TryParse(System.Environment.CommandLine, out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a><span data-ttu-id="a1119-119">Semantik</span><span class="sxs-lookup"><span data-stu-id="a1119-119">Semantics</span></span>

<span data-ttu-id="a1119-120">Wenn alle Anweisungen der obersten Ebene in einer Kompilierungseinheit des Programms vorhanden sind, ist die Bedeutung so, als ob Sie im Block Körper einer `Main` Methode einer `Program` Klasse im globalen Namespace kombiniert wurden, wie im folgenden dargestellt:</span><span class="sxs-lookup"><span data-stu-id="a1119-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="a1119-121">Beachten Sie, dass die Namen "Program" und "Main" nur zu Illustrations Zwecken verwendet werden, dass die vom Compiler verwendeten Namen von der Implementierung abhängen und weder der Typ noch die Methode anhand des Namens aus dem Quellcode referenziert werden kann.</span><span class="sxs-lookup"><span data-stu-id="a1119-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="a1119-122">Die-Methode wird als Einstiegspunkt des Programms bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="a1119-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="a1119-123">Explizit deklarierte Methoden, die gemäß der Konvention als Einstiegspunkt Kandidaten angesehen werden können, werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="a1119-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="a1119-124">Wenn dies der Fall ist, wird eine Warnung ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="a1119-124">A warning is reported when that happens.</span></span> <span data-ttu-id="a1119-125">Es ist ein Fehler, `-main:<type>` Compilerschalter anzugeben, wenn die Anweisungen der obersten Ebene vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="a1119-125">It is an error to specify `-main:<type>` compiler switch when there are top-level statements.</span></span>

<span data-ttu-id="a1119-126">Asynchrone Vorgänge sind in den Anweisungen der obersten Ebene zulässig, bis zu dem Grad, den Sie in-Anweisungen innerhalb einer regulären Methode für asynchrone Einstiegspunkte haben.</span><span class="sxs-lookup"><span data-stu-id="a1119-126">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="a1119-127">Sie sind jedoch nicht erforderlich. wenn `await` Ausdrücke und andere asynchrone Vorgänge ausgelassen werden, wird keine Warnung erzeugt.</span><span class="sxs-lookup"><span data-stu-id="a1119-127">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span> <span data-ttu-id="a1119-128">Stattdessen entspricht die Signatur der generierten Einstiegspunkt Methode</span><span class="sxs-lookup"><span data-stu-id="a1119-128">Instead the signature of the generated entry point method is equivalent to</span></span> 
``` c#
    static void Main()
```

<span data-ttu-id="a1119-129">Das obige Beispiel ergibt die folgende `$Main` Methoden Deklaration:</span><span class="sxs-lookup"><span data-stu-id="a1119-129">The example above would yield the following `$Main` method declaration:</span></span>

``` c#
static class $Program
{
    static void $Main()
    {
        if (System.Environment.CommandLine.Length == 0
            || !int.TryParse(System.Environment.CommandLine, out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

<span data-ttu-id="a1119-130">Gleichzeitig ein Beispiel wie das folgende:</span><span class="sxs-lookup"><span data-stu-id="a1119-130">At the same time an example like this:</span></span>
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="a1119-131">würde ergeben:</span><span class="sxs-lookup"><span data-stu-id="a1119-131">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task $Main()
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="a1119-132">Bereich von lokalen Variablen der obersten Ebene und lokalen Funktionen</span><span class="sxs-lookup"><span data-stu-id="a1119-132">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="a1119-133">Obwohl lokale Variablen und Funktionen der obersten Ebene in der generierten Einstiegspunkt Methode "umfänden" sind, sollten Sie im gesamten Programm in jeder Kompilierungseinheit weiterhin im Gültigkeitsbereich sein.</span><span class="sxs-lookup"><span data-stu-id="a1119-133">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program in every compilation unit.</span></span>
<span data-ttu-id="a1119-134">Für den Zweck der Auswertung mit einfachem Namen wird der globale Namespace erreicht:</span><span class="sxs-lookup"><span data-stu-id="a1119-134">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="a1119-135">Zuerst wird versucht, den Namen innerhalb der generierten Einstiegspunkt Methode auszuwerten, und zwar nur, wenn dieser Versuch fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="a1119-135">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="a1119-136">Die "reguläre" Auswertung innerhalb der globalen Namespace Deklaration wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="a1119-136">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="a1119-137">Dies kann zu einem namens shadodown von Namespaces und Typen führen, die innerhalb des globalen Namespace deklariert sind, sowie zum überschatten importierter Namen.</span><span class="sxs-lookup"><span data-stu-id="a1119-137">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="a1119-138">Wenn die einfache namens Auswertung außerhalb der Anweisungen der obersten Ebene stattfindet und die Auswertung eine lokale Variable oder Funktion der obersten Ebene ergibt, sollte dies zu einem Fehler führen.</span><span class="sxs-lookup"><span data-stu-id="a1119-138">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="a1119-139">Auf diese Weise schützen wir unsere zukünftige Fähigkeit zur besseren Adressierung von "Funktionen der obersten Ebene" (Szenario 2 in https://github.com/dotnet/csharplang/issues/3117)und können Benutzern nützliche Diagnosen zur Verfügung stellen.</span><span class="sxs-lookup"><span data-stu-id="a1119-139">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

