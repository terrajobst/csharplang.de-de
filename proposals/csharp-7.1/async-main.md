---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483751"
---
# <a name="async-main"></a><span data-ttu-id="c2a32-101">Async-Haupt-</span><span class="sxs-lookup"><span data-stu-id="c2a32-101">Async Main</span></span>

* <span data-ttu-id="c2a32-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="c2a32-102">[x] Proposed</span></span>
* <span data-ttu-id="c2a32-103">[] Prototyp</span><span class="sxs-lookup"><span data-stu-id="c2a32-103">[ ] Prototype</span></span>
* <span data-ttu-id="c2a32-104">[]-Implementierung</span><span class="sxs-lookup"><span data-stu-id="c2a32-104">[ ] Implementation</span></span>
* <span data-ttu-id="c2a32-105">[]-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="c2a32-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="c2a32-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="c2a32-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="c2a32-107">Ermöglicht die Verwendung von `await` in der Main/EntryPoint-Methode einer Anwendung, indem der EntryPoint `Task` / `Task<int>` zurückgeben und als `async`gekennzeichnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="c2a32-107">Allow `await` to be used in an application's Main / entrypoint method by allowing the entrypoint to return `Task` / `Task<int>` and be marked `async`.</span></span>

## <a name="motivation"></a><span data-ttu-id="c2a32-108">Motivation</span><span class="sxs-lookup"><span data-stu-id="c2a32-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="c2a32-109">Beim Schreiben von konsolenbasierten Dienst C#Programmen und beim Schreiben von kleinen Test-apps, die `async` Methoden von Main `await` aufgerufen werden sollen, kommt es sehr häufig vor.</span><span class="sxs-lookup"><span data-stu-id="c2a32-109">It is very common when learning C#, when writing console-based utilities, and when writing small test apps to want to call and `await` `async` methods from Main.</span></span>  <span data-ttu-id="c2a32-110">Heute fügen wir hier einen Komplexitäts Grad hinzu, indem wir das Erstellen einer solchen `await`"in einer separaten Async-Methode erzwingen, was dazu beiträgt, dass Entwickler wie folgt Code Bausteine schreiben müssen, um zu beginnen:</span><span class="sxs-lookup"><span data-stu-id="c2a32-110">Today we add a level of complexity here by forcing such `await`'ing to be done in a separate async method, which causes developers to need to write boilerplate like the following just to get started:</span></span>

```csharp
public static void Main()
{
    MainAsync().GetAwaiter().GetResult();
}

private static async Task MainAsync()
{
    ... // Main body here
}
```

<span data-ttu-id="c2a32-111">Wir können die Notwendigkeit dieses Bausteine entfernen und den Einstieg vereinfachen, indem wir einfach den eigentlichen `async` so lassen, dass `await`s darin verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="c2a32-111">We can remove the need for this boilerplate and make it easier to get started simply by allowing Main itself to be `async` such that `await`s can be used in it.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="c2a32-112">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="c2a32-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="c2a32-113">Die folgenden Signaturen sind zurzeit zulässige entryPoints:</span><span class="sxs-lookup"><span data-stu-id="c2a32-113">The following signatures are currently allowed entrypoints:</span></span>

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

<span data-ttu-id="c2a32-114">Wir erweitern die Liste der zulässigen entryPoints um Folgendes:</span><span class="sxs-lookup"><span data-stu-id="c2a32-114">We extend the list of allowed entrypoints to include:</span></span>

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

<span data-ttu-id="c2a32-115">Um Kompatibilitäts Risiken zu vermeiden, werden diese neuen Signaturen nur als gültige Einstiegspunkte betrachtet, wenn keine über Ladungen der vorherigen Menge vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="c2a32-115">To avoid compatibility risks, these new signatures will only be considered as valid entrypoints if no overloads of the previous set are present.</span></span>
<span data-ttu-id="c2a32-116">Die Sprache/der Compiler erfordert nicht, dass der EntryPoint als `async`gekennzeichnet wird, obwohl wir erwarten, dass die meisten Verwendungen als solche gekennzeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="c2a32-116">The language / compiler will not require that the entrypoint be marked as `async`, though we expect the vast majority of uses will be marked as such.</span></span>

<span data-ttu-id="c2a32-117">Wenn einer dieser Methoden als EntryPoint identifiziert wird, erstellt der Compiler eine tatsächliche EntryPoint-Methode, die eine dieser codierten Methoden aufruft:</span><span class="sxs-lookup"><span data-stu-id="c2a32-117">When one of these is identified as the entrypoint, the compiler will synthesize an actual entrypoint method that calls one of these coded methods:</span></span>
- <span data-ttu-id="c2a32-118">```static Task Main()``` führt dazu, dass der Compiler das Äquivalent von ausgibt ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="c2a32-118">```static Task Main()``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="c2a32-119">```static Task Main(string[])``` führt dazu, dass der Compiler das Äquivalent von ausgibt ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="c2a32-119">```static Task Main(string[])``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="c2a32-120">```static Task<int> Main()``` führt dazu, dass der Compiler das Äquivalent von ausgibt ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="c2a32-120">```static Task<int> Main()``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="c2a32-121">```static Task<int> Main(string[])``` führt dazu, dass der Compiler das Äquivalent von ausgibt ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="c2a32-121">```static Task<int> Main(string[])``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>

<span data-ttu-id="c2a32-122">Beispielsyntax:</span><span class="sxs-lookup"><span data-stu-id="c2a32-122">Example usage:</span></span>

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a><span data-ttu-id="c2a32-123">Nachteile</span><span class="sxs-lookup"><span data-stu-id="c2a32-123">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="c2a32-124">Der Hauptnachteil ist einfach die zusätzliche Komplexität der Unterstützung zusätzlicher EntryPoint-Signaturen.</span><span class="sxs-lookup"><span data-stu-id="c2a32-124">The main drawback is simply the additional complexity of supporting additional entrypoint signatures.</span></span>

## <a name="alternatives"></a><span data-ttu-id="c2a32-125">Alternativen</span><span class="sxs-lookup"><span data-stu-id="c2a32-125">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="c2a32-126">Weitere Varianten, die berücksichtigt werden:</span><span class="sxs-lookup"><span data-stu-id="c2a32-126">Other variants considered:</span></span>

<span data-ttu-id="c2a32-127">Zulassen von `async void`.</span><span class="sxs-lookup"><span data-stu-id="c2a32-127">Allowing `async void`.</span></span>  <span data-ttu-id="c2a32-128">Wir müssen die Semantik für Code, der Sie direkt aufruft, unverändert lassen, wodurch es für einen generierten EntryPoint schwierig wird, ihn aufzurufen (keine Aufgabe zurückgegeben).</span><span class="sxs-lookup"><span data-stu-id="c2a32-128">We need to keep the semantics the same for code calling it directly, which would then make it difficult for a generated entrypoint to call it (no Task returned).</span></span>  <span data-ttu-id="c2a32-129">Wir könnten dies beheben, indem wir zwei andere Methoden erstellen, z. b.</span><span class="sxs-lookup"><span data-stu-id="c2a32-129">We could solve this by generating two other methods, e.g.</span></span>

```csharp
public static async void Main()
{
   ... // await code
}
```

<span data-ttu-id="c2a32-130">wird zu</span><span class="sxs-lookup"><span data-stu-id="c2a32-130">becomes</span></span>

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

<span data-ttu-id="c2a32-131">Außerdem gibt es Bedenken hinsichtlich der Verwendung von `async void`.</span><span class="sxs-lookup"><span data-stu-id="c2a32-131">There are also concerns around encouraging usage of `async void`.</span></span>

<span data-ttu-id="c2a32-132">Verwenden von "mainasync" anstelle von "Main" als Name.</span><span class="sxs-lookup"><span data-stu-id="c2a32-132">Using "MainAsync" instead of "Main" as the name.</span></span>  <span data-ttu-id="c2a32-133">Obwohl das Async-Suffix für Methoden zurückgegeben wird, die Methoden zurückgeben, liegt das in erster Linie in der Bibliotheks Funktionalität, bei der es sich nicht um den Hauptschlüssel handelt, und die Unterstützung zusätzlicher entrypointnamen über "Main" hinaus.</span><span class="sxs-lookup"><span data-stu-id="c2a32-133">While the async suffix is recommended for Task-returning methods, that's primarily about library functionality, which Main is not, and supporting additional entrypoint names beyond "Main" is not worth it.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="c2a32-134">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="c2a32-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="c2a32-135">–</span><span class="sxs-lookup"><span data-stu-id="c2a32-135">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="c2a32-136">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="c2a32-136">Design meetings</span></span>

<span data-ttu-id="c2a32-137">–</span><span class="sxs-lookup"><span data-stu-id="c2a32-137">n/a</span></span>
