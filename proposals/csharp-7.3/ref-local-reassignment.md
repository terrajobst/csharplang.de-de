---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483583"
---
# <a name="ref-local-reassignment"></a><span data-ttu-id="3d929-101">Lokale Ref-Neuzuweisung</span><span class="sxs-lookup"><span data-stu-id="3d929-101">Ref Local Reassignment</span></span>

<span data-ttu-id="3d929-102">In C# 7,3 wird Unterstützung für das erneute Binden des Verweises einer lokalen Ref-Variablen oder eines ref-Parameters hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="3d929-102">In C# 7.3, we add support for rebinding the referent of a ref local variable or a ref parameter.</span></span>

<span data-ttu-id="3d929-103">Wir fügen dem Satz von `assignment_operator`s Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="3d929-103">We add the following to the set of `assignment_operator`s.</span></span>

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

<span data-ttu-id="3d929-104">Der `=ref`-Operator wird als ***ref-Zuweisungs Operator***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="3d929-104">The `=ref` operator is called the ***ref assignment operator***.</span></span> <span data-ttu-id="3d929-105">Es handelt sich nicht um einen *Verbund Zuweisungs Operator*.</span><span class="sxs-lookup"><span data-stu-id="3d929-105">It is not a *compound assignment operator*.</span></span> <span data-ttu-id="3d929-106">Der linke Operand muss ein Ausdruck sein, der an eine lokale Ref-Variable, einen ref-Parameter (außer `this`) oder einen out-Parameter bindet.</span><span class="sxs-lookup"><span data-stu-id="3d929-106">The left operand must be an expression that binds to a ref local variable, a ref parameter (other than `this`), or an out parameter.</span></span> <span data-ttu-id="3d929-107">Der rechte Operand muss ein Ausdruck sein, der einen lvalue ergibt, der einen Wert des gleichen Typs wie der linke Operand angibt.</span><span class="sxs-lookup"><span data-stu-id="3d929-107">The right operand must be an expression that yields an lvalue designating a value of the same type as the left operand.</span></span>

<span data-ttu-id="3d929-108">Der rechte Operand muss definitiv am Punkt der Ref-Zuweisung zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="3d929-108">The right operand must be definitely assigned at the point of the ref assignment.</span></span>

<span data-ttu-id="3d929-109">Wenn der linke Operand an einen `out`-Parameter bindet, ist es ein Fehler, wenn dieser `out` Parameter am Anfang des ref-Zuweisungs Operators nicht definitiv zugewiesen wurde.</span><span class="sxs-lookup"><span data-stu-id="3d929-109">When the left operand binds to an `out` parameter, it is an error if that `out` parameter has not been definitely assigned at the beginning of the ref assignment operator.</span></span>

<span data-ttu-id="3d929-110">Wenn der linke Operand ein Beschreib barer Verweis ist (d. h., er legt einen anderen als einen `ref readonly` local-oder `in`-Parameter fest), dann muss der rechte Operand ein Beschreib barer lvalue sein.</span><span class="sxs-lookup"><span data-stu-id="3d929-110">If the left operand is a writeable ref (i.e. it designates anything other than a `ref readonly` local or  `in` parameter), then the right operand must be a writeable lvalue.</span></span>

<span data-ttu-id="3d929-111">Der Ref-Zuweisungs Operator ergibt einen lvalue des zugewiesenen Typs.</span><span class="sxs-lookup"><span data-stu-id="3d929-111">The ref assignment operator yields an lvalue of the assigned type.</span></span> <span data-ttu-id="3d929-112">Es ist beschreibbar, wenn der linke Operand beschreibbar ist (d. h. nicht `ref readonly` oder `in`).</span><span class="sxs-lookup"><span data-stu-id="3d929-112">It is writeable if the left operand is writeable (i.e. not `ref readonly` or `in`).</span></span>

<span data-ttu-id="3d929-113">Die Sicherheitsregeln für diesen Operator lauten:</span><span class="sxs-lookup"><span data-stu-id="3d929-113">The safety rules for this operator are:</span></span>

- <span data-ttu-id="3d929-114">Bei einer `e1 = ref e2`Ref-Neuzuweisung muss die *ref-sicher-zu-* Escapezeichen von `e2` mindestens so breit sein, wie der *ref-Safe-to-Escape* -`e1`ist.</span><span class="sxs-lookup"><span data-stu-id="3d929-114">For a ref reassignment `e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

<span data-ttu-id="3d929-115">Where *ref-Safe-to-Escape* ist [für Ref-like-Typen in Sicherheit](../csharp-7.2/span-safety.md) definiert.</span><span class="sxs-lookup"><span data-stu-id="3d929-115">Where *ref-safe-to-escape* is defined in [Safety for ref-like types](../csharp-7.2/span-safety.md)</span></span>
