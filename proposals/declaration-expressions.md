---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483565"
---
# <a name="declaration-expressions"></a><span data-ttu-id="93aef-101">Deklarations Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="93aef-101">Declaration expressions</span></span>

<span data-ttu-id="93aef-102">Unterstützen von Deklarations Zuweisungen als Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="93aef-102">Support declaration assignments as expressions.</span></span>

## <a name="motivation"></a><span data-ttu-id="93aef-103">Motivation</span><span class="sxs-lookup"><span data-stu-id="93aef-103">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="93aef-104">Ermöglicht die Initialisierung zum Zeitpunkt der Deklaration in mehreren Fällen, vereinfacht den Code und ermöglicht die Verwendung von `var`.</span><span class="sxs-lookup"><span data-stu-id="93aef-104">Allow initialization at the point of declaration in more cases, simplifying code, and allowing `var` to be used.</span></span>

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

<span data-ttu-id="93aef-105">Lässt Deklarationen für `ref` Argumente zu, ähnlich wie `out var`.</span><span class="sxs-lookup"><span data-stu-id="93aef-105">Allow declarations for `ref` arguments, similar to `out var`.</span></span>

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a><span data-ttu-id="93aef-106">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="93aef-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="93aef-107">Ausdrücke werden so erweitert, dass Sie Deklarations Zuweisung einschließen.</span><span class="sxs-lookup"><span data-stu-id="93aef-107">Expressions are extended to include declaration assignment.</span></span> <span data-ttu-id="93aef-108">Die Rangfolge entspricht der Zuweisung.</span><span class="sxs-lookup"><span data-stu-id="93aef-108">Precedence is the same as assignment.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

<span data-ttu-id="93aef-109">Die Deklarations Zuweisung ist eine einzelne lokale.</span><span class="sxs-lookup"><span data-stu-id="93aef-109">The declaration assignment is of a single local.</span></span>

<span data-ttu-id="93aef-110">Der Typ eines Deklarations Zuweisungs Ausdrucks ist der Typ der Deklaration.</span><span class="sxs-lookup"><span data-stu-id="93aef-110">The type of a declaration assignment expression is the type of the declaration.</span></span>
<span data-ttu-id="93aef-111">Wenn der Typ `var`ist, ist der abherleitet Typ der Typ des Initialisierungs Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="93aef-111">If the type is `var`, the inferred type is the type of the initializing expression.</span></span> 

<span data-ttu-id="93aef-112">Der Deklarations Zuweisungs Ausdruck kann ein l-Wert sein, der insbesondere für `ref` Argument Werte gilt.</span><span class="sxs-lookup"><span data-stu-id="93aef-112">The declaration assignment expression may be an l-value, for `ref` argument values in particular.</span></span>

<span data-ttu-id="93aef-113">Wenn der Deklarations Zuweisungs Ausdruck einen Werttyp deklariert und der Ausdruck ein r-Wert ist, ist der Wert des Ausdrucks eine Kopie.</span><span class="sxs-lookup"><span data-stu-id="93aef-113">If the declaration assignment expression declares a value type, and the expression is an r-value, the value of the expression is a copy.</span></span>

<span data-ttu-id="93aef-114">Der Deklarations Zuweisungs Ausdruck kann eine `ref` lokalen deklarieren.</span><span class="sxs-lookup"><span data-stu-id="93aef-114">The declaration assignment expression may declare a `ref` local.</span></span>
<span data-ttu-id="93aef-115">Es besteht eine Mehrdeutigkeit, wenn `ref` für einen Deklarations Ausdruck in einem `ref`-Argument verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="93aef-115">There is an ambiguity when `ref` is used for a declaration expression in a `ref` argument.</span></span>
<span data-ttu-id="93aef-116">Der lokale Variableninitialisierer bestimmt, ob die Deklaration eine `ref` Local ist.</span><span class="sxs-lookup"><span data-stu-id="93aef-116">The local variable initializer determines whether the declaration is a `ref` local.</span></span>

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

<span data-ttu-id="93aef-117">Der Bereich der in Deklarations Zuweisungs Ausdrücken deklarierten lokalen Variablen entspricht dem Bereich der entsprechenden Deklarations Ausdrücke aus C # 7.0.</span><span class="sxs-lookup"><span data-stu-id="93aef-117">The scope of locals declared in declaration assignment expressions is the same the scope of corresponding declaration expressions from C#7.0.</span></span>

<span data-ttu-id="93aef-118">Es handelt sich um einen Kompilierzeitfehler, der auf eine lokale in Text vor dem Deklarations Ausdruck verweist.</span><span class="sxs-lookup"><span data-stu-id="93aef-118">It is a compile time error to refer to a local in text preceding the declaration expression.</span></span>

## <a name="alternatives"></a><span data-ttu-id="93aef-119">Alternativen</span><span class="sxs-lookup"><span data-stu-id="93aef-119">Alternatives</span></span>
[alternatives]: #alternatives
<span data-ttu-id="93aef-120">Keine Änderung.</span><span class="sxs-lookup"><span data-stu-id="93aef-120">No change.</span></span> <span data-ttu-id="93aef-121">Diese Funktion ist einfach syntaktische Kurzweile.</span><span class="sxs-lookup"><span data-stu-id="93aef-121">This feature is just syntactic shorthand after all.</span></span>

<span data-ttu-id="93aef-122">Allgemeinere Sequenz Ausdrücke: siehe [#377](https://github.com/dotnet/csharplang/issues/377).</span><span class="sxs-lookup"><span data-stu-id="93aef-122">More general sequence expressions: see [#377](https://github.com/dotnet/csharplang/issues/377).</span></span>

<span data-ttu-id="93aef-123">Um die Verwendung von `var` in mehreren Fällen zuzulassen, lassen Sie eine separate Deklaration und Zuweisung `var` lokalen Variablen zu, und leiten Sie den Typ von Zuweisungen von allen Codepfade ab.</span><span class="sxs-lookup"><span data-stu-id="93aef-123">To allow use of `var` in more cases, allow separate declaration and assignment of `var` locals, and infer the type from assignments from all code paths.</span></span>

## <a name="see-also"></a><span data-ttu-id="93aef-124">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="93aef-124">See also</span></span>
[see-also]: #see-also
<span data-ttu-id="93aef-125">Siehe grundlegender Deklarations Ausdruck in [#595](https://github.com/dotnet/csharplang/issues/595).</span><span class="sxs-lookup"><span data-stu-id="93aef-125">See Basic Declaration Expression in [#595](https://github.com/dotnet/csharplang/issues/595).</span></span>

<span data-ttu-id="93aef-126">Weitere Informationen finden Sie unter Dekonstruktions Deklaration im [Dekonstruktions](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) Feature.</span><span class="sxs-lookup"><span data-stu-id="93aef-126">See Deconstruction Declaration in the [deconstruction](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) feature.</span></span>
