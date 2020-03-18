---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483559"
---
# <a name="lambda-attributes"></a><span data-ttu-id="a68bb-101">Lambda-Attribute</span><span class="sxs-lookup"><span data-stu-id="a68bb-101">Lambda Attributes</span></span>

* <span data-ttu-id="a68bb-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="a68bb-102">[x] Proposed</span></span>
* <span data-ttu-id="a68bb-103">[] Prototyp</span><span class="sxs-lookup"><span data-stu-id="a68bb-103">[ ] Prototype</span></span>
* <span data-ttu-id="a68bb-104">[]-Implementierung</span><span class="sxs-lookup"><span data-stu-id="a68bb-104">[ ] Implementation</span></span>
* <span data-ttu-id="a68bb-105">[]-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="a68bb-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="a68bb-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="a68bb-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a68bb-107">Ermöglicht das Anwenden von Attributen auf Lambdas (und anonyme Methoden) und auf Parameter der Lambda-/anonymen Methode, da Sie auf reguläre Methoden angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="a68bb-107">Allow attributes to be applied to lambdas (and anonymous methods) and to lambda / anonymous method parameters, as they can be on regular methods.</span></span>

## <a name="motivation"></a><span data-ttu-id="a68bb-108">Motivation</span><span class="sxs-lookup"><span data-stu-id="a68bb-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a68bb-109">Zwei Hauptgründe:</span><span class="sxs-lookup"><span data-stu-id="a68bb-109">Two primary motivations:</span></span>

1. <span data-ttu-id="a68bb-110">Zum Bereitstellen von Metadaten, die Analysen zur Kompilierzeit sichtbar sind.</span><span class="sxs-lookup"><span data-stu-id="a68bb-110">To provide metadata visible to analyzers at compile-time.</span></span>
2. <span data-ttu-id="a68bb-111">Zum Bereitstellen von Metadaten, die für die Reflektion und die Tools zur Laufzeit sichtbar sind.</span><span class="sxs-lookup"><span data-stu-id="a68bb-111">To provide metadata visible to reflection and tooling at run-time.</span></span>

<span data-ttu-id="a68bb-112">Als Beispiel für (1): bei Leistungs sensiblem Code ist es hilfreich, wenn Sie über einen Analyzer verfügen, der die Zuordnung von Abschlüssen und Delegaten für Lambdas zuweist, die den Zustand überschreiten.</span><span class="sxs-lookup"><span data-stu-id="a68bb-112">As an example of (1): For performance-sensitive code, it is helpful to be able to have an analyzer that flags when closures and delegates are being allocated for lambdas that close over state.</span></span>  <span data-ttu-id="a68bb-113">Häufig geht ein Entwickler dieses Codes von seiner Methode aus, um die Erfassung eines Zustands zu vermeiden, sodass der Compiler eine statische Methode und einen zwischen speicherbaren Delegaten für die Methode generieren kann, oder der Entwickler stellt sicher, dass der einzige Zustand, der geschlossen wird, `this`ist, sodass der Compiler zumindest die Zuordnung eines Closure-Objekts vermeiden kann.</span><span class="sxs-lookup"><span data-stu-id="a68bb-113">Often a developer of such code will go out of his or her way to avoid capturing any state, so that the compiler can generate a static method and a cacheable delegate for the method, or the developer will ensure that the only state being closed over is `this`, allowing the compiler at least to avoid allocating a closure object.</span></span>  <span data-ttu-id="a68bb-114">Ohne Sprachunterstützung für das Einschränken der erfassten Elemente ist es jedoch nicht einfach, versehentlich den Zustand zu schließen.</span><span class="sxs-lookup"><span data-stu-id="a68bb-114">But, without language support for limiting what may be captured, it is all too easy to accidentally close over state.</span></span>  <span data-ttu-id="a68bb-115">Es wäre hilfreich, wenn ein Entwickler Lambdas mit Attributen kommentieren könnte, um anzugeben, in welchem Zustand Sie geschlossen werden dürfen, z. b.:</span><span class="sxs-lookup"><span data-stu-id="a68bb-115">It would be valuable if a developer could annotate lambdas with attributes to indicate what state they're allowed to close over, for example:</span></span>

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

<span data-ttu-id="a68bb-116">Anschließend kann ein Analyzer geschrieben werden, um zu markieren, wenn der Zustand falsch aufgezeichnet wird, z. b.:</span><span class="sxs-lookup"><span data-stu-id="a68bb-116">Then an analyzer can be written to flag when state is captured incorrectly, for example:</span></span>

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a><span data-ttu-id="a68bb-117">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="a68bb-117">Detailed design</span></span>
[design]: #detailed-design

- <span data-ttu-id="a68bb-118">Wenn Sie dieselbe Attribut Syntax wie bei normalen Methoden verwenden, können Attribute am Anfang einer Lambda-oder anonymen Methode angewendet werden, z. b.:</span><span class="sxs-lookup"><span data-stu-id="a68bb-118">Using the same attribute syntax as on normal methods, attributes may be applied at the beginning of a lambda or anonymous method, for example:</span></span>

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- <span data-ttu-id="a68bb-119">Um Mehrdeutigkeiten zu vermeiden, ob ein Attribut für die Lambda-Methode oder eines der Argumente gilt, dürfen Attribute nur verwendet werden, wenn Klammern um Argumente verwendet werden, z. b.:</span><span class="sxs-lookup"><span data-stu-id="a68bb-119">To avoid ambiguity as to whether an attribute applies to the lambda method or to one of the arguments, attributes may only be used when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- <span data-ttu-id="a68bb-120">Bei anonymen Methoden werden keine Parser benötigt, um ein Attribut auf die Methode vor dem `delegate`-Schlüsselwort anzuwenden, z. b.:</span><span class="sxs-lookup"><span data-stu-id="a68bb-120">With anonymous methods, parens are not needed in order to apply an attribute to the method before the `delegate` keyword, for example:</span></span>

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- <span data-ttu-id="a68bb-121">Mehrere Attribute können entweder über eine standardmäßige, durch Trennzeichen getrennte Syntax oder über eine vollständige Attribut Syntax angewendet werden, z. b.:</span><span class="sxs-lookup"><span data-stu-id="a68bb-121">Multiple attributes may be applied, either via standard comma-delimited syntax or via full-attribute syntax, for example:</span></span>

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- <span data-ttu-id="a68bb-122">Attribute können auf die Parameter einer anonymen Methode oder eines Lambda-Ausdrucks angewendet werden, jedoch nur, wenn Klammern um Argumente herum verwendet werden, z. b.:</span><span class="sxs-lookup"><span data-stu-id="a68bb-122">Attributes may be applied to the parameters to an anonymous method or lambda, but only when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- <span data-ttu-id="a68bb-123">Mehrere Attribute können auf die Parameter einer anonymen Methode oder eines Lambda-Ausdrucks angewendet werden, indem entweder die durch Kommas getrennte oder vollständige Attribut Syntax verwendet wird, z. b.:</span><span class="sxs-lookup"><span data-stu-id="a68bb-123">Multiple attributes may be applied to the parameters of an anonymous method or lambda, using either the comma-delimited or full-attribute syntax, for example:</span></span>

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- <span data-ttu-id="a68bb-124">`return`Ziel Attribute können auch in Lambda-Ausdrücken verwendet werden, z. b.:</span><span class="sxs-lookup"><span data-stu-id="a68bb-124">`return`-targeted attributes may also be used on lambdas, for example:</span></span>

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- <span data-ttu-id="a68bb-125">Der Compiler gibt die Attribute für die generierte Methode und die Argumente für diese Methoden wie für jede andere Methode aus.</span><span class="sxs-lookup"><span data-stu-id="a68bb-125">The compiler outputs the attributes onto the generated method and arguments to those methods as it would for any other method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="a68bb-126">Nachteile</span><span class="sxs-lookup"><span data-stu-id="a68bb-126">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a68bb-127">–</span><span class="sxs-lookup"><span data-stu-id="a68bb-127">n/a</span></span>

## <a name="alternatives"></a><span data-ttu-id="a68bb-128">Alternativen</span><span class="sxs-lookup"><span data-stu-id="a68bb-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a68bb-129">–</span><span class="sxs-lookup"><span data-stu-id="a68bb-129">n/a</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="a68bb-130">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="a68bb-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="a68bb-131">–</span><span class="sxs-lookup"><span data-stu-id="a68bb-131">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="a68bb-132">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="a68bb-132">Design meetings</span></span>

<span data-ttu-id="a68bb-133">–</span><span class="sxs-lookup"><span data-stu-id="a68bb-133">n/a</span></span>