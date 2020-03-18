---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483973"
---
# <a name="pattern-based-using-and-using-declarations"></a><span data-ttu-id="cb993-101">"Musterbasiertes verwenden" und "Verwenden von Deklarationen"</span><span class="sxs-lookup"><span data-stu-id="cb993-101">"pattern-based using" and "using declarations"</span></span>

## <a name="summary"></a><span data-ttu-id="cb993-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="cb993-102">Summary</span></span>

<span data-ttu-id="cb993-103">Die Sprache fügt zwei neue Funktionen um die `using`-Anweisung hinzu, um die Ressourcenverwaltung zu vereinfachen: `using` sollten zusätzlich zu `IDisposable` ein verwerfbares Muster erkennen und der Sprache eine `using` Deklaration hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="cb993-103">The language will add two new capabilities around the `using` statement in order to make resource management simpler: `using` should recognize a disposable pattern in addition to `IDisposable` and add a `using` declaration to the language.</span></span>

## <a name="motivation"></a><span data-ttu-id="cb993-104">Motivation</span><span class="sxs-lookup"><span data-stu-id="cb993-104">Motivation</span></span>

<span data-ttu-id="cb993-105">Die `using`-Anweisung ist zurzeit ein effektives Tool für die Ressourcenverwaltung, erfordert aber eine ganze Stunde.</span><span class="sxs-lookup"><span data-stu-id="cb993-105">The `using` statement is an effective tool for resource management today but it requires quite a bit of ceremony.</span></span> <span data-ttu-id="cb993-106">Methoden mit einer Reihe von Ressourcen, die verwaltet werden müssen, können mit einer Reihe von `using`-Anweisungen syntaktisch blockiert werden.</span><span class="sxs-lookup"><span data-stu-id="cb993-106">Methods that have a number of resources to manage can get syntactically bogged down with a series of `using` statements.</span></span> <span data-ttu-id="cb993-107">Diese Syntax Belastung ist ausreichend, dass die meisten Richtlinien zum Codierungsstil in diesem Szenario explizit mit geschweiften Klammern zu tun haben.</span><span class="sxs-lookup"><span data-stu-id="cb993-107">This syntax burden is enough that most coding style guidelines explicitly have an exception around braces for this scenario.</span></span> 

<span data-ttu-id="cb993-108">Mit der `using` Deklaration wird C# ein Großteil der Zeremonie an dieser Stelle entfernt und mit anderen Sprachen, die Ressourcen Verwaltungs Blöcke einschließen, abgerufen.</span><span class="sxs-lookup"><span data-stu-id="cb993-108">The `using` declaration removes much of the ceremony here and gets C# on par with other languages that include resource management blocks.</span></span> <span data-ttu-id="cb993-109">Außerdem ermöglicht die Musterbasierte `using` Entwicklern, den Satz von Typen zu erweitern, die an dieser Stelle teilnehmen können.</span><span class="sxs-lookup"><span data-stu-id="cb993-109">Additionally the pattern-based `using` lets developers expand the set of types that can participate here.</span></span> <span data-ttu-id="cb993-110">In vielen Fällen ist es nicht mehr erforderlich, Wrapper Typen zu erstellen, die nur vorhanden sind, um Werte in einer `using` Anweisung zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="cb993-110">In many cases removing the need to create wrapper types that only exist to allow for a values use in a `using` statement.</span></span> 

<span data-ttu-id="cb993-111">Diese Features ermöglichen es Entwicklern, die Szenarien, in denen `using` angewendet werden können, zu vereinfachen und zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="cb993-111">Together these features allow developers to simplify and expand the scenarios where `using` can be applied.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="cb993-112">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="cb993-112">Detailed Design</span></span> 

### <a name="using-declaration"></a><span data-ttu-id="cb993-113">using-Deklaration</span><span class="sxs-lookup"><span data-stu-id="cb993-113">using declaration</span></span>

<span data-ttu-id="cb993-114">Die Sprache ermöglicht das Hinzufügen von `using` zu einer lokalen Variablen Deklaration.</span><span class="sxs-lookup"><span data-stu-id="cb993-114">The language will allow for `using` to be added to a local variable declaration.</span></span> <span data-ttu-id="cb993-115">Eine solche Deklaration hat denselben Effekt wie das Deklarieren der Variablen in einer `using`-Anweisung am gleichen Speicherort.</span><span class="sxs-lookup"><span data-stu-id="cb993-115">Such a declaration will have the same effect as declaring the variable in a `using` statement at the same location.</span></span>

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

<span data-ttu-id="cb993-116">Die Lebensdauer eines `using` lokalen wird bis zum Ende des Bereichs erweitert, in dem er deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="cb993-116">The lifetime of a `using` local will extend to the end of the scope in which it is declared.</span></span> <span data-ttu-id="cb993-117">Die `using` lokalen Variablen werden dann in umgekehrter Reihenfolge verworfen, in der Sie deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cb993-117">The `using` locals will then be disposed in the reverse order in which they are declared.</span></span> 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

<span data-ttu-id="cb993-118">Es gibt keine Einschränkungen in Bezug auf `goto`oder ein anderes Ablauf Steuerungs Konstrukt im Zusammenhang mit einer `using` Deklaration.</span><span class="sxs-lookup"><span data-stu-id="cb993-118">There are no restrictions around `goto`, or any other control flow construct in the face of a `using` declaration.</span></span> <span data-ttu-id="cb993-119">Stattdessen verhält sich der Code genauso wie für die entsprechende `using`-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="cb993-119">Instead the code acts just as it would for the equivalent `using` statement:</span></span>

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

<span data-ttu-id="cb993-120">Eine lokale Deklaration, die in einer `using` lokalen Deklaration deklariert wird, ist implizit schreibgeschützt.</span><span class="sxs-lookup"><span data-stu-id="cb993-120">A local declared in a `using` local declaration will be implicitly read-only.</span></span> <span data-ttu-id="cb993-121">Dies entspricht dem Verhalten der in einer `using` Anweisung deklarierten lokalen Variablen.</span><span class="sxs-lookup"><span data-stu-id="cb993-121">This matches the behavior of locals declared in a `using` statement.</span></span> 

<span data-ttu-id="cb993-122">Die Sprachgrammatik für `using` Deklarationen lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="cb993-122">The language grammar for `using` declarations will be the following:</span></span>

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

<span data-ttu-id="cb993-123">Einschränkungen bei der `using` Deklaration:</span><span class="sxs-lookup"><span data-stu-id="cb993-123">Restrictions around `using` declaration:</span></span>

- <span data-ttu-id="cb993-124">Darf nicht direkt in einer `case` Bezeichnung vorkommen, sondern muss sich innerhalb eines Blocks innerhalb der `case` Bezeichnung befinden.</span><span class="sxs-lookup"><span data-stu-id="cb993-124">May not appear directly inside a `case` label but instead must be within a block inside the `case` label.</span></span>
- <span data-ttu-id="cb993-125">Kann nicht als Teil einer `out` Variablen Deklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="cb993-125">May not appear as part of an `out` variable declaration.</span></span> 
- <span data-ttu-id="cb993-126">Für jeden Deklarator muss ein Initialisierer vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="cb993-126">Must have an initializer for each declarator.</span></span>
- <span data-ttu-id="cb993-127">Der lokale Typ muss implizit konvertierbar sein, um das `using` Muster zu `IDisposable` oder zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="cb993-127">The local type must be implicitly convertible to `IDisposable` or fulfill the `using` pattern.</span></span>

### <a name="pattern-based-using"></a><span data-ttu-id="cb993-128">Muster basiert mithilfe von</span><span class="sxs-lookup"><span data-stu-id="cb993-128">pattern-based using</span></span>

<span data-ttu-id="cb993-129">Die Sprache fügt das Konzept eines verwerfbaren Musters hinzu: ein Typ, der über eine `Dispose` Instanzmethode verfügt, auf die zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="cb993-129">The language will add the notion of a disposable pattern: that is a type which has an accessible `Dispose` instance method.</span></span> <span data-ttu-id="cb993-130">Typen, die das verwerfbare Muster erfüllen, können an einer `using` Anweisung oder Deklaration teilnehmen, ohne dass `IDisposable`implementiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="cb993-130">Types which fit the disposable pattern can participate in a `using` statement or declaration without being required to implement `IDisposable`.</span></span> 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

<span data-ttu-id="cb993-131">Auf diese Weise können Entwickler `using` in einer Reihe neuer Szenarien nutzen:</span><span class="sxs-lookup"><span data-stu-id="cb993-131">This will allow developers to leverage `using` in a number of new scenarios:</span></span>

- <span data-ttu-id="cb993-132">`ref struct`: diese Typen können heute keine Schnittstellen implementieren und können daher nicht an `using` Anweisungen teilnehmen.</span><span class="sxs-lookup"><span data-stu-id="cb993-132">`ref struct`: These types can't implement interfaces today and hence can't participate in `using` statements.</span></span>
- <span data-ttu-id="cb993-133">Erweiterungs Methoden ermöglichen Entwicklern das Erweitern von Typen in anderen Assemblys, sodass Sie an `using`-Anweisungen teilnehmen können.</span><span class="sxs-lookup"><span data-stu-id="cb993-133">Extension methods will allow developers to augment types in other assemblies to participate in `using` statements.</span></span>

<span data-ttu-id="cb993-134">In der Situation, in der ein Typ implizit in `IDisposable` konvertiert werden kann, und auch das verwerfbare Muster entspricht, wird `IDisposable` bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="cb993-134">In the situation where a type can be implicitly converted to `IDisposable` and also fits the disposable pattern, then `IDisposable` will be preferred.</span></span> <span data-ttu-id="cb993-135">Wenngleich dies der gegen übergesetzte Ansatz von `foreach` ist (Muster, das gegenüber Interface bevorzugt ist), ist es für Abwärtskompatibilität erforderlich.</span><span class="sxs-lookup"><span data-stu-id="cb993-135">While this takes the opposite approach of `foreach` (pattern preferred over interface) it is necessary for backwards compatibility.</span></span>

<span data-ttu-id="cb993-136">Dies gilt auch für die gleichen Einschränkungen wie für eine herkömmliche `using`-Anweisung: lokale Variablen, die im `using` deklariert sind, sind schreibgeschützt, ein `null` Wert führt nicht dazu, dass eine Ausnahme ausgelöst wird usw... Die Codegenerierung unterscheidet sich nur darin, dass vor dem Aufrufen von "verwerfen" keine Umwandlung in `IDisposable` erfolgt:</span><span class="sxs-lookup"><span data-stu-id="cb993-136">The same restrictions from a traditional `using` statement apply here as well: local variables declared in the `using` are read-only, a `null` value will not cause an exception to be thrown, etc ... The code generation will be different only in that there will not be a cast to `IDisposable` before calling Dispose:</span></span>

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

<span data-ttu-id="cb993-137">Um dem verwerfbaren Muster gerecht zu werden, muss auf die `Dispose` Methode zugegriffen werden können, Parameter lose Parameter und haben einen `void` Rückgabetyp.</span><span class="sxs-lookup"><span data-stu-id="cb993-137">In order to fit the disposable pattern the `Dispose` method must be accessible, parameterless and have a `void` return type.</span></span> <span data-ttu-id="cb993-138">Es gibt keine weiteren Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="cb993-138">There are no other restrictions.</span></span> <span data-ttu-id="cb993-139">Dies bedeutet explizit, dass hier Erweiterungs Methoden verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="cb993-139">This explicitly means that extension methods can be used here.</span></span>

## <a name="considerations"></a><span data-ttu-id="cb993-140">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="cb993-140">Considerations</span></span>

### <a name="case-labels-without-blocks"></a><span data-ttu-id="cb993-141">Case-Bezeichnungen ohne Blöcke</span><span class="sxs-lookup"><span data-stu-id="cb993-141">case labels without blocks</span></span>

<span data-ttu-id="cb993-142">Eine `using declaration` ist aufgrund von Komplikationen bei der tatsächlichen Lebensdauer direkt innerhalb einer `case` Bezeichnung unzulässig.</span><span class="sxs-lookup"><span data-stu-id="cb993-142">A `using declaration` is illegal directly inside a `case` label due to complications around its actual lifetime.</span></span> <span data-ttu-id="cb993-143">Eine mögliche Lösung besteht darin, die gleiche Lebensdauer wie eine `out var` am gleichen Speicherort anzugeben.</span><span class="sxs-lookup"><span data-stu-id="cb993-143">One potential solution is to simply give it the same lifetime as an `out var` in the same location.</span></span> <span data-ttu-id="cb993-144">Dies war die zusätzliche Komplexität der Featureimplementierung und die einfache Problem Umgehung (fügen Sie der `case` Bezeichnung einen Block hinzu) nicht rechtfertigen diese Route.</span><span class="sxs-lookup"><span data-stu-id="cb993-144">It was deemed the extra complexity to the feature implementation and the ease of the work around (just add a block to the `case` label) didn't justify taking this route.</span></span>

## <a name="future-expansions"></a><span data-ttu-id="cb993-145">Zukünftige Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="cb993-145">Future Expansions</span></span>

### <a name="fixed-locals"></a><span data-ttu-id="cb993-146">lokale Variablen</span><span class="sxs-lookup"><span data-stu-id="cb993-146">fixed locals</span></span>

<span data-ttu-id="cb993-147">Eine `fixed`-Anweisung verfügt über alle Eigenschaften von `using`-Anweisungen, die die Möglichkeit haben, `using` lokale zu haben.</span><span class="sxs-lookup"><span data-stu-id="cb993-147">A `fixed` statement has all of the properties of `using` statements that motivated the ability to have `using` locals.</span></span> <span data-ttu-id="cb993-148">Beachten Sie, dass Sie dieses Feature auch auf `fixed` lokalen Erweiterungen erweitern müssen.</span><span class="sxs-lookup"><span data-stu-id="cb993-148">Consideration should be given to extending this feature to `fixed` locals as well.</span></span> <span data-ttu-id="cb993-149">Die Lebensdauer-und Bestell Regeln sollten für `using` und `fixed` hier gleichermaßen gut anwendbar sein.</span><span class="sxs-lookup"><span data-stu-id="cb993-149">The lifetime and ordering rules should apply equally well for `using` and `fixed` here.</span></span>
