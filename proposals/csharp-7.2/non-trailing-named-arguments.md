---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483643"
---
# <a name="non-trailing-named-arguments"></a><span data-ttu-id="64168-101">Nicht schließende benannte Argumente</span><span class="sxs-lookup"><span data-stu-id="64168-101">Non-trailing named arguments</span></span>

## <a name="summary"></a><span data-ttu-id="64168-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="64168-102">Summary</span></span>
[summary]: #summary
<span data-ttu-id="64168-103">Zulassen, dass benannte Argumente an einer nicht nachfolgenden Position verwendet werden, solange Sie an der richtigen Position verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="64168-103">Allow named arguments to be used in non-trailing position, as long as they are used in their correct position.</span></span> <span data-ttu-id="64168-104">Beispiel: `DoSomething(isEmployed:true, name, age);`.</span><span class="sxs-lookup"><span data-stu-id="64168-104">For example: `DoSomething(isEmployed:true, name, age);`.</span></span>

## <a name="motivation"></a><span data-ttu-id="64168-105">Motivation</span><span class="sxs-lookup"><span data-stu-id="64168-105">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="64168-106">Die Hauptmotivation besteht darin, redundante Informationen zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="64168-106">The main motivation is to avoid typing redundant information.</span></span> <span data-ttu-id="64168-107">Es ist üblich, ein Argument, bei dem es sich um eine Literale handelt (z. b. `null``true`), für die Verdeutlichung des Codes zu benennen, anstatt Argumente außerhalb der Reihenfolge zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="64168-107">It is common to name an argument that is a literal (such as `null`, `true`) for the purpose of clarifying the code, rather than of passing arguments out-of-order.</span></span>
<span data-ttu-id="64168-108">Dies ist zurzeit nicht zulässig (`CS1738`), es sei denn, die folgenden Argumente werden ebenfalls benannt.</span><span class="sxs-lookup"><span data-stu-id="64168-108">That is currently disallowed (`CS1738`) unless all the following arguments are also named.</span></span>

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

<span data-ttu-id="64168-109">Einige zusätzliche Beispiele:</span><span class="sxs-lookup"><span data-stu-id="64168-109">Some additional examples:</span></span>
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

<span data-ttu-id="64168-110">Dies würde auch mit Parametern funktionieren:</span><span class="sxs-lookup"><span data-stu-id="64168-110">This would also work with params:</span></span>
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a><span data-ttu-id="64168-111">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="64168-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="64168-112">In "7.5.1" (Argument Listen) gibt die Spezifikation derzeit Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="64168-112">In §7.5.1 (Argument lists), the spec currently says:</span></span>
> <span data-ttu-id="64168-113">Ein *Argument* mit einem *Argument Namen* wird als __benanntes Argument__bezeichnet, wohingegen ein *Argument* ohne *Argument Name* ein __Positions Argument__ist.</span><span class="sxs-lookup"><span data-stu-id="64168-113">An *argument* with an *argument-name* is referred to as a __named argument__, whereas an *argument* without an *argument-name* is a __positional argument__.</span></span> <span data-ttu-id="64168-114">Es ist ein Fehler für ein Positions Argument, das nach einem benannten Argument in einer *Argumentliste*angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="64168-114">It is an error for a positional argument to appear after a named argument in an *argument-list*.</span></span>

<span data-ttu-id="64168-115">Der Vorschlag besteht darin, diesen Fehler zu entfernen und die Regeln für die Suche nach dem entsprechenden Parameter für ein Argument ("7.5.1.1") zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="64168-115">The proposal is to remove this error and update the rules for finding the corresponding parameter for an argument (§7.5.1.1):</span></span>

<span data-ttu-id="64168-116">Argumente in der Argumentliste von Instanzkonstruktoren, Methoden, Indexern und Delegaten:</span><span class="sxs-lookup"><span data-stu-id="64168-116">Arguments in the argument-list of instance constructors, methods, indexers and delegates:</span></span>
- <span data-ttu-id="64168-117">[vorhandene Regeln]</span><span class="sxs-lookup"><span data-stu-id="64168-117">[existing rules]</span></span>
- <span data-ttu-id="64168-118">Ein unbenanntes Argument entspricht keinem Parameter, wenn es sich nach einem benannten Argument außerhalb der Position oder einem benannten params-Argument befindet.</span><span class="sxs-lookup"><span data-stu-id="64168-118">An unnamed argument corresponds to no parameter when it is after an out-of-position named argument or a named params argument.</span></span>

<span data-ttu-id="64168-119">Dies verhindert insbesondere das Aufrufen von `void M(bool a = true, bool b = true, bool c = true, );` mit `M(c: false, valueB);`.</span><span class="sxs-lookup"><span data-stu-id="64168-119">In particular, this prevents invoking `void M(bool a = true, bool b = true, bool c = true, );` with `M(c: false, valueB);`.</span></span> <span data-ttu-id="64168-120">Das erste Argument wird außerhalb der Position verwendet (das Argument wird an der ersten Position verwendet, aber der Parameter mit dem Namen "c" befindet sich an der dritten Position). Daher sollten die folgenden Argumente benannt werden.</span><span class="sxs-lookup"><span data-stu-id="64168-120">The first argument is used out-of-position (the argument is used in first position, but the parameter named "c" is in third position), so the following arguments should be named.</span></span>

<span data-ttu-id="64168-121">Anders ausgedrückt: nicht nachfolgende benannte Argumente sind nur zulässig, wenn der Name und die Position dazu führen, den gleichen entsprechenden Parameter zu suchen.</span><span class="sxs-lookup"><span data-stu-id="64168-121">In other words, non-trailing named arguments are only allowed when the name and the position result in finding the same corresponding parameter.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="64168-122">Nachteile</span><span class="sxs-lookup"><span data-stu-id="64168-122">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="64168-123">Dieser Vorschlag verschärft vorhandene Feinheiten mit benannten Argumenten in der Überladungs Auflösung.</span><span class="sxs-lookup"><span data-stu-id="64168-123">This proposal exacerbates existing subtleties with named arguments in overload resolution.</span></span> <span data-ttu-id="64168-124">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="64168-124">For instance:</span></span>

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

<span data-ttu-id="64168-125">Sie können diese Situation heute erzielen, indem Sie die Parameter austauschen:</span><span class="sxs-lookup"><span data-stu-id="64168-125">You could get this situation today by swapping the parameters:</span></span>

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

<span data-ttu-id="64168-126">Wenn Sie zwei Methoden `void M(int a, int b)` und `void M(int x, string y)`haben, erzeugt der falsche Aufruf `M(x: 1, 2)` eine Diagnose auf der Grundlage der zweiten Überladung ("kann nicht von" int "in" String "konvertiert werden).</span><span class="sxs-lookup"><span data-stu-id="64168-126">Similarly, if you have two methods `void M(int a, int b)` and `void M(int x, string y)`, the mistaken invocation `M(x: 1, 2)` will produce a diagnostic based on the second overload ("cannot convert from 'int' to 'string'").</span></span> <span data-ttu-id="64168-127">Dieses Problem ist bereits vorhanden, wenn das benannte Argument an einer nachfolgenden Position verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="64168-127">This problem already exists when the named argument is used in a trailing position.</span></span>

## <a name="alternatives"></a><span data-ttu-id="64168-128">Alternativen</span><span class="sxs-lookup"><span data-stu-id="64168-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="64168-129">Es gibt einige Alternativen zu berücksichtigende Aspekte:</span><span class="sxs-lookup"><span data-stu-id="64168-129">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="64168-130">Der Status quo</span><span class="sxs-lookup"><span data-stu-id="64168-130">The status quo</span></span>
- <span data-ttu-id="64168-131">Bereitstellen von IDE-Unterstützung zum Ausfüllen aller Namen von nachfolgenden Argumenten, wenn Sie einen bestimmten Namen in der Mitte eingeben.</span><span class="sxs-lookup"><span data-stu-id="64168-131">Providing IDE assistance to fill-in all the names of trailing arguments when you type specific a name in the middle.</span></span>

<span data-ttu-id="64168-132">Beide sind von ausführlicheren Ausführlichkeit, da Sie mehrere benannte Argumente einführen, auch wenn Sie nur einen Namen eines Literals am Anfang der Argumentliste benötigen.</span><span class="sxs-lookup"><span data-stu-id="64168-132">Both of those suffer from more verbosity, as they introduce multiple named arguments even if you just need one name of a literal at the beginning of the argument list.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="64168-133">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="64168-133">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="64168-134">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="64168-134">Design meetings</span></span>
[ldm]: #ldm
<span data-ttu-id="64168-135">Die Funktion wurde kurz am 16. Mai 2017 in LDM erläutert, mit Genehmigung im Prinzip ("OK", um zu "Vorschlag/Prototyp" zu wechseln).</span><span class="sxs-lookup"><span data-stu-id="64168-135">The feature was briefly discussed in LDM on May 16th 2017, with approval in principle (ok to move to proposal/prototype).</span></span> <span data-ttu-id="64168-136">Es wurde auch kurz am 28. Juni 2017 erläutert.</span><span class="sxs-lookup"><span data-stu-id="64168-136">It was also briefly discussed on June 28th 2017.</span></span>

<span data-ttu-id="64168-137">Bezieht sich auf die anfängliche Erörterung https://github.com/dotnet/csharplang/issues/518 die sich auf das Problem https://github.com/dotnet/csharplang/issues/570</span><span class="sxs-lookup"><span data-stu-id="64168-137">Relates to initial discussion https://github.com/dotnet/csharplang/issues/518 Relates to championed issue https://github.com/dotnet/csharplang/issues/570</span></span>
