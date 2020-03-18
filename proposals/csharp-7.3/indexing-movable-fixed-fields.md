---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483607"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a><span data-ttu-id="34fce-101">Die Indizierung `fixed` Felder sollte nicht unabhängig vom verschiebbaren/nicht verschiebbaren Kontext fixiert werden.</span><span class="sxs-lookup"><span data-stu-id="34fce-101">Indexing `fixed` fields should not require pinning regardless of the movable/unmovable context.</span></span> #

<span data-ttu-id="34fce-102">Die Änderung hat die Größe einer Fehlerbehebung.</span><span class="sxs-lookup"><span data-stu-id="34fce-102">The change has the size of a bug fix.</span></span> <span data-ttu-id="34fce-103">Sie kann sich in 7,3 befinden und steht nicht in Konflikt mit der von uns weiterführenden Richtung.</span><span class="sxs-lookup"><span data-stu-id="34fce-103">It can be in 7.3 and does not conflict with whatever direction we take further.</span></span>
<span data-ttu-id="34fce-104">Diese Änderung besteht lediglich darin, dass das folgende Szenario funktioniert, auch wenn `s` in der Lage ist, dies zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="34fce-104">This change is only about allowing the following scenario to work even though `s` is moveable.</span></span> <span data-ttu-id="34fce-105">Sie ist bereits gültig, wenn `s` nicht verschoben werden kann.</span><span class="sxs-lookup"><span data-stu-id="34fce-105">It is already valid when `s` is not moveable.</span></span> 

<span data-ttu-id="34fce-106">Hinweis: in beiden Fällen ist weiterhin `unsafe` Kontext erforderlich.</span><span class="sxs-lookup"><span data-stu-id="34fce-106">NOTE: in either case, it still requires `unsafe` context.</span></span> <span data-ttu-id="34fce-107">Es ist möglich, nicht initialisierte Daten zu lesen oder sogar außerhalb des gültigen Bereichs.</span><span class="sxs-lookup"><span data-stu-id="34fce-107">It is possible to read uninitialized data or even out of range.</span></span> <span data-ttu-id="34fce-108">Dies ändert sich nicht.</span><span class="sxs-lookup"><span data-stu-id="34fce-108">That is not changing.</span></span>

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int p = s.myFixedField[5]; // indexing fixed-size array fields would be ok
    }
}
```

<span data-ttu-id="34fce-109">Die wichtigste "Herausforderung", die hier angezeigt wird, ist die Erläuterung der Entspannung in der Spezifikation. Dies ist insbesondere erforderlich, da das anhepup weiterhin erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="34fce-109">The main “challenge” that I see here is how to explain the relaxation in the spec. In particular, since the following would still need pinning.</span></span> <span data-ttu-id="34fce-110">(da `s` in der Lage ist, das Feld explizit als Zeiger zu verwenden)</span><span class="sxs-lookup"><span data-stu-id="34fce-110">(because `s` is moveable and we explicitly use the field as a pointer)</span></span>

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int* ptr = s.myFixedField; // taking a pointer explicitly still requires pinning.
        int p = ptr[5];
    }
}
```

<span data-ttu-id="34fce-111">Ein Grund für das Anheften des Ziels, wenn es verschoben werden muss, ist das Artefakt unserer Code Generierungs Strategie,-wir konvertieren immer in einen nicht verwalteten Zeiger und erzwingen somit, dass der Benutzer die PIN über `fixed` Anweisung anheftet.</span><span class="sxs-lookup"><span data-stu-id="34fce-111">One reason why we require pinning of the target when it is movable is the artifact of our code generation strategy, - we always convert to an unmanaged pointer and thus force the user to pin via `fixed` statement.</span></span> <span data-ttu-id="34fce-112">Die Konvertierung in nicht verwaltete ist jedoch bei der Indizierung nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="34fce-112">However, conversion to unmanaged is unnecessary when doing indexing.</span></span> <span data-ttu-id="34fce-113">Die gleiche unsichere Zeiger Mathematik ist gleichermaßen anwendbar, wenn der Empfänger in Form eines verwalteten Zeigers vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="34fce-113">The same unsafe pointer math is equally applicable when we have the receiver in the form of a managed pointer.</span></span> <span data-ttu-id="34fce-114">Wenn dies der Fall ist, wird der zwischen Verweis verwaltet (GC-nachverfolgt), und das anheunen ist nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="34fce-114">If we do that, then the intermediate ref is managed (GC-tracked) and the pinning is unnecessary.</span></span>

<span data-ttu-id="34fce-115">Der Änderungs https://github.com/dotnet/roslyn/pull/24966 ist ein Prototyp-PR, der diese Anforderung lockert.</span><span class="sxs-lookup"><span data-stu-id="34fce-115">The change https://github.com/dotnet/roslyn/pull/24966 is a prototype PR that relaxes this requirement.</span></span>
