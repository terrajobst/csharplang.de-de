---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483637"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a><span data-ttu-id="c183e-101">Automatisch implementierte Eigenschaften Feld-Ziel Attribute</span><span class="sxs-lookup"><span data-stu-id="c183e-101">Auto-Implemented Property Field-Targeted Attributes</span></span>

## <a name="summary"></a><span data-ttu-id="c183e-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="c183e-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="c183e-103">Diese Funktion soll es Entwicklern ermöglichen, Attribute direkt auf die Unterstützungs Felder von automatisch implementierten Eigenschaften anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="c183e-103">This feature intends to allow developers to apply attributes directly to the backing fields of auto-implemented properties.</span></span>

## <a name="motivation"></a><span data-ttu-id="c183e-104">Motivation</span><span class="sxs-lookup"><span data-stu-id="c183e-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="c183e-105">Derzeit ist es nicht möglich, Attribute auf die Unterstützungs Felder von automatisch implementierten Eigenschaften anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="c183e-105">Currently it is not possible to apply attributes to the backing fields of auto-implemented properties.</span></span>  <span data-ttu-id="c183e-106">In Fällen, in denen der Entwickler ein Attribut für die Feld Ausrichtung verwenden muss, wird er gezwungen, das Feld manuell zu deklarieren und die ausführlichere Eigenschaften Syntax zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="c183e-106">In those cases where the developer must use a field-targeting attribute they are forced to declare the field manually and use the more verbose property syntax.</span></span>  <span data-ttu-id="c183e-107">Wenn die C# feldorientierten Attribute für das generierte Unterstützungs Feld für Ereignisse immer von unterstützt werden, ist es sinnvoll, die gleiche Funktionalität auf ihre Eigenschaften-Kin auszuweiten.</span><span class="sxs-lookup"><span data-stu-id="c183e-107">Given that C# has always supported field-targeted attributes on the generated backing field for events it makes sense to extend the same functionality to their property kin.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="c183e-108">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="c183e-108">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="c183e-109">Kurz gesagt, wäre Folgendes zulässig, C# und es wird keine Warnung ausgegeben:</span><span class="sxs-lookup"><span data-stu-id="c183e-109">In short, the following would be legal C# and not produce a warning:</span></span>

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

<span data-ttu-id="c183e-110">Dies würde dazu führen, dass die feldorientierten Attribute auf das vom Compiler generierte Unterstützungs Feld angewendet werden:</span><span class="sxs-lookup"><span data-stu-id="c183e-110">This would result in the field-targeted attributes being applied to the compiler-generated backing field:</span></span>

```csharp
[Serializable]
public class Foo 
{
    [NonSerialized]
    private string _mySecretBackingField;
    
    public string MySecret
    {
        get { return _mySecretBackingField; }
        set { _mySecretBackingField = value; }
    }
}
```

<span data-ttu-id="c183e-111">Wie bereits erwähnt, wird die Parität mit der Ereignis C# Syntax von 1,0 bewirkt, da Folgendes bereits zulässig ist und sich wie erwartet verhält:</span><span class="sxs-lookup"><span data-stu-id="c183e-111">As mentioned, this brings parity with event syntax from C# 1.0 as the following is already legal and behaves as expected:</span></span>

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a><span data-ttu-id="c183e-112">Nachteile</span><span class="sxs-lookup"><span data-stu-id="c183e-112">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="c183e-113">Es gibt zwei mögliche Nachteile bei der Implementierung dieser Änderung:</span><span class="sxs-lookup"><span data-stu-id="c183e-113">There are two potential drawbacks to implementing this change:</span></span>

1. <span data-ttu-id="c183e-114">Wenn Sie versuchen, ein Attribut auf das Feld einer automatisch implementierten Eigenschaft anzuwenden, wird eine Compilerwarnung erzeugt, dass die Attribute in diesem Block ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="c183e-114">Attempting to apply an attribute to the field of an auto-implemented property produces a compiler warning that the attributes in that block will be ignored.</span></span>  <span data-ttu-id="c183e-115">Wenn der Compiler dahingehend geändert wurde, dass diese Attribute unterstützt werden, werden Sie auf das dahinter liegende Feld bei einer nachfolgenden Neukompilierung angewendet, wodurch das Verhalten des Programms zur Laufzeit geändert werden kann.</span><span class="sxs-lookup"><span data-stu-id="c183e-115">If the compiler were changed to support those attributes they would be applied to the backing field on a subsequent recompilation which could alter the behavior of the program at runtime.</span></span>
1. <span data-ttu-id="c183e-116">Der Compiler überprüft derzeit nicht die AttributeUsage-Ziele der Attribute, wenn versucht wird, diese auf das Feld der automatisch implementierten Eigenschaft anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="c183e-116">The compiler does not currently validate the AttributeUsage targets of the attributes when attempting to apply them to the field of the auto-implemented property.</span></span>  <span data-ttu-id="c183e-117">Wenn der Compiler geändert wurde, um feldorientierte Attribute zu unterstützen, und das betreffende Attribut nicht auf ein Feld angewendet werden kann, würde der Compiler einen Fehler ausgeben, anstatt eine Warnung zu erstellen, die den Build unterbricht.</span><span class="sxs-lookup"><span data-stu-id="c183e-117">If the compiler were changed to support field-targeted attributes and the attribute in question cannot be applied to a field the compiler would emit an error instead of a warning, breaking the build.</span></span>

## <a name="alternatives"></a><span data-ttu-id="c183e-118">Alternativen</span><span class="sxs-lookup"><span data-stu-id="c183e-118">Alternatives</span></span>
[alternatives]: #alternatives

## <a name="unresolved-questions"></a><span data-ttu-id="c183e-119">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="c183e-119">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="c183e-120">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="c183e-120">Design meetings</span></span>
