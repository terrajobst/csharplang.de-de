---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483481"
---
# <a name="readonly-locals-and-parameters"></a><span data-ttu-id="c5db8-101">schreibgeschützte lokale Variablen und Parameter</span><span class="sxs-lookup"><span data-stu-id="c5db8-101">readonly locals and parameters</span></span>

* <span data-ttu-id="c5db8-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="c5db8-102">[x] Proposed</span></span>
* <span data-ttu-id="c5db8-103">[] Prototyp</span><span class="sxs-lookup"><span data-stu-id="c5db8-103">[ ] Prototype</span></span>
* <span data-ttu-id="c5db8-104">[]-Implementierung</span><span class="sxs-lookup"><span data-stu-id="c5db8-104">[ ] Implementation</span></span>
* <span data-ttu-id="c5db8-105">[]-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="c5db8-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="c5db8-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="c5db8-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="c5db8-107">Zulassen, dass lokale und Parameter als schreibgeschützt kommentiert werden, um eine flache Mutation dieser lokalen Variablen und Parameter zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="c5db8-107">Allow locals and parameters to be annotated as readonly in order to prevent shallow mutation of those locals and parameters.</span></span>

## <a name="motivation"></a><span data-ttu-id="c5db8-108">Motivation</span><span class="sxs-lookup"><span data-stu-id="c5db8-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="c5db8-109">Heute kann das `readonly`-Schlüsselwort auf Felder angewendet werden. Dadurch wird sichergestellt, dass ein Feld nur während der Erstellung geschrieben werden kann (statische Konstruktion bei einem statischen Feld oder Instanzerstellung im Fall eines Instanzfelds). Dadurch können Entwickler Fehler vermeiden, indem Sie versehentlich einen Zustand überschreiben, der nicht geändert werden sollte.</span><span class="sxs-lookup"><span data-stu-id="c5db8-109">Today, the `readonly` keyword can be applied to fields; this has the effect of ensuring that a field can only be written to during construction (static construction in the case of a static field, or instance construction in the case of an instance field), which helps developers avoid mistakes by accidentally overwriting state which should not be modified.</span></span> <span data-ttu-id="c5db8-110">Aber Felder sind nicht die einzigen Orte, die Entwickler sicherstellen möchten, dass die Werte nicht mutiert werden.</span><span class="sxs-lookup"><span data-stu-id="c5db8-110">But fields aren't the only places developers want to ensure that values aren't mutated.</span></span> <span data-ttu-id="c5db8-111">Insbesondere ist es üblich, eine lokale Variable zu erstellen, um den temporären Zustand zu speichern. das versehentliche aktualisieren dieses temporären Zustands kann zu fehlerhaften Berechnungen und anderen Fehlern führen, insbesondere wenn solche "Locals" in Lambdas aufgezeichnet werden. an diesem Punkt werden Sie zu Feldern entfernt. es gibt jedoch keine Möglichkeit, solche gehoben Felder als `readonly`zu markieren.</span><span class="sxs-lookup"><span data-stu-id="c5db8-111">In particular, it's common to create a local variable to store temporary state, and accidentally updating that temporary state can result in erroneous calculations and other such bugs, especially when such "locals" are captured in lambdas, at which point they are lifted to fields, but there's no way today to mark such lifted fields as `readonly`.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="c5db8-112">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="c5db8-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="c5db8-113">Lokale Variablen können auch `readonly` auch als festgelegt werden, und der Compiler stellt sicher, dass Sie nur zum Zeitpunkt der Deklaration festgelegt sind C# (bestimmte lokale in sind bereits implizit schreibgeschützt, wie z. b. die Iterations Variable in einer foreach-Schleife oder die verwendete Variable in einem Using-Block), aber derzeit kann ein Entwickler andere lokale lokal nicht als `readonly`markieren.</span><span class="sxs-lookup"><span data-stu-id="c5db8-113">Locals will be annotatable as `readonly` as well, with the compiler ensuring that they're only set at the time of declaration (certain locals in C# are already implicitly readonly, such as the iteration variable in a 'foreach' loop or the used variable in a 'using' block, but currently a developer has no ability to mark other locals as `readonly`).</span></span> <span data-ttu-id="c5db8-114">Solche `readonly` lokalen Variablen müssen einen Initialisierer aufweisen:</span><span class="sxs-lookup"><span data-stu-id="c5db8-114">Such `readonly` locals must have an initializer:</span></span>

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="c5db8-115">Und als Kurzform für `readonly var`kann das vorhandene kontextabhängige Schlüsselwort `let` verwendet werden, z. b.</span><span class="sxs-lookup"><span data-stu-id="c5db8-115">And as shorthand for `readonly var`, the existing contextual keyword `let` may be used, e.g.</span></span>

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="c5db8-116">Es gibt keine besonderen Einschränkungen für den Initialisierer und kann als Initialisierer für lokale Elemente gelten, z. b.</span><span class="sxs-lookup"><span data-stu-id="c5db8-116">There are no special constraints for what the initializer can be, and can be anything currently valid as an initializer for locals, e.g.</span></span>

```csharp
readonly T data = arg1 ?? arg2;
```

<span data-ttu-id="c5db8-117">`readonly` on Locals ist besonders wertvoll, wenn Sie mit Lambdas und Abschlüssen arbeiten.</span><span class="sxs-lookup"><span data-stu-id="c5db8-117">`readonly` on locals is particularly valuable when working with lambdas and closures.</span></span> <span data-ttu-id="c5db8-118">Wenn eine anonyme Methode oder ein Lambda auf den lokalen Zustand aus dem einschließenden Bereich zugreift, wird dieser Zustand von dem Compiler als Closure aufgezeichnet, der durch eine "Anzeige Klasse" dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="c5db8-118">When an anonymous method or lambda accesses local state from the enclosing scope, that state is captured into a closure by the compiler, which is represented by a "display class."</span></span>  <span data-ttu-id="c5db8-119">Jedes "local", das erfasst wird, ist ein Feld in dieser Klasse, aber da der Compiler dieses Feld in Ihrem Namen erstellt, haben Sie keine Möglichkeit, es als `readonly` zu kommentieren, um zu verhindern, dass der Lambda-Ausdruck fälschlicherweise in das "lokale" schreibt (in Anführungszeichen, weil es sich nicht um eine lokale Datei handelt, zumindest nicht in der resultierenden MSIL).</span><span class="sxs-lookup"><span data-stu-id="c5db8-119">Each "local" that's captured is a field in this class, yet because the compiler is generating this field on your behalf, you have no opportunity to annotate it as `readonly` in order to prevent the lambda from erroneously writing to the "local" (in quotes because it's really not a local, at least not in the resulting MSIL).</span></span> <span data-ttu-id="c5db8-120">Bei `readonly` lokalen Variablen kann der Compiler verhindern, dass der Lambda-Ausdruck in den lokalen Schreibvorgang geschrieben wird. Dies ist besonders nützlich in Szenarios mit Multithreading, bei denen ein fehlerhafter Schreibvorgang zu einem gefährlichen, aber seltenen und schwer zu suchenden Parallelitäts Fehler führen könnte.</span><span class="sxs-lookup"><span data-stu-id="c5db8-120">With `readonly` locals, the compiler can prevent the lambda from writing to local, which is particularly valuable in scenarios involving multithreading where an erroneous write could result in a dangerous but rare and hard-to-find concurrency bug.</span></span>

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

<span data-ttu-id="c5db8-121">Als spezielle Form von local können Parameter auch als `readonly`angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="c5db8-121">As a special form of local, parameters will also be annotatable as `readonly`.</span></span> <span data-ttu-id="c5db8-122">Dies hat keine Auswirkung darauf, was der Aufrufer der Methode an den-Parameter übergeben kann (so wie es keine Einschränkung gibt, welche Werte in einem `readonly` Feld gespeichert werden können), aber wie bei allen `readonly` lokalen kann der Compiler verhindern, dass Code nach der Deklaration in den Parameter schreibt, was bedeutet, dass der Text der Methode nicht in den-Parameter geschrieben werden kann.</span><span class="sxs-lookup"><span data-stu-id="c5db8-122">This would have no effect on what the caller of the method is able to pass to the parameter (just as there's no constraint on what values may be stored into a `readonly` field), but as with any `readonly` local, the compiler would prohibit code from writing to the parameter after declaration, which means the body of the method is prohibited from writing to the parameter.</span></span>

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

<span data-ttu-id="c5db8-123">`readonly` Parameter wirken sich nicht auf die Signatur/Metadaten aus, die vom Compiler für diese Methode ausgegeben werden, und wirken sich einfach darauf aus, wie der Compiler die Kompilierung des Methoden Texts behandelt.</span><span class="sxs-lookup"><span data-stu-id="c5db8-123">`readonly` parameters do not affect the signature/metadata emitted by the compiler for that method, and simply affect how the compiler handles the compilation of the method's body.</span></span> <span data-ttu-id="c5db8-124">So kann beispielsweise eine virtuelle Basis Methode einen `readonly`-Parameter haben, und dieser Parameter kann in einer außer Kraft Setzung beschreibbar sein.</span><span class="sxs-lookup"><span data-stu-id="c5db8-124">Thus, for example, a base virtual method could have a `readonly` parameter, and that parameter could be writable in an override.</span></span>

<span data-ttu-id="c5db8-125">Wie bei Feldern sind `readonly` für lokale und Parameter flach, was sich auf den Speicherort auswirkt, aber nicht transitiv auf das Objekt Diagramm wirkt.</span><span class="sxs-lookup"><span data-stu-id="c5db8-125">As with fields, `readonly` for locals and parameters is shallow, affecting the storage location but not transitively affecting the object graph.</span></span> <span data-ttu-id="c5db8-126">Ebenso wie bei Feldern wird durch das Aufrufen einer Methode für eine `readonly` local/Parameter-Struktur tatsächlich eine Kopie der Struktur erstellt und die-Methode für die Kopie aufgerufen, um eine interne Mutation `this`zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="c5db8-126">However, also as with fields, calling a method on a `readonly` local/parameter struct will actually make a copy of the struct and call the method on the copy, in order to avoid internal mutation of `this`.</span></span>

<span data-ttu-id="c5db8-127">`readonly` lokale und Parameter können nicht als `ref` oder `out` Argumente übermittelt werden, es sei denn,/bis `ref readonly` wird ebenfalls unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c5db8-127">`readonly` locals and parameters can't be passed as `ref` or `out` arguments, unless/until `ref readonly` is also supported.</span></span>

## <a name="alternatives"></a><span data-ttu-id="c5db8-128">Alternativen</span><span class="sxs-lookup"><span data-stu-id="c5db8-128">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="c5db8-129">`val` können als Alternative Kurzform zum `let`verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c5db8-129">`val` could be used as an alternative shorthand to `let`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="c5db8-130">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="c5db8-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="c5db8-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: Ich habe die Frage gestellt, wie `ref readonly` getrennt von diesem Vorschlag behandelt werden soll.</span><span class="sxs-lookup"><span data-stu-id="c5db8-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: I've left the question of how to handle `ref readonly` as separate from this proposal.</span></span>
- <span data-ttu-id="c5db8-132">Dieser Vorschlag behandelt nicht schreibgeschützte Strukturen/unveränderliche Typen.</span><span class="sxs-lookup"><span data-stu-id="c5db8-132">This proposal does not tackle readonly structs / immutable types.</span></span> <span data-ttu-id="c5db8-133">Dies bleibt für einen separaten Vorschlag.</span><span class="sxs-lookup"><span data-stu-id="c5db8-133">That is left for a separate proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="c5db8-134">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="c5db8-134">Design meetings</span></span>

- <span data-ttu-id="c5db8-135">Kurz erläutert am 21. Januar 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span><span class="sxs-lookup"><span data-stu-id="c5db8-135">Briefly discussed on Jan 21, 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span></span>
