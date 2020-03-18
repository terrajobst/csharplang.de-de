---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2019
ms.locfileid: "79484045"
---
# <a name="permit-stackalloc-in-nested-contexts"></a><span data-ttu-id="77ffe-101">`stackalloc` in einem Kontext in der Liste zulassen</span><span class="sxs-lookup"><span data-stu-id="77ffe-101">Permit `stackalloc` in nested contexts</span></span>

### <a name="stack-allocation"></a><span data-ttu-id="77ffe-102">Stapel Zuordnung</span><span class="sxs-lookup"><span data-stu-id="77ffe-102">Stack allocation</span></span>

<span data-ttu-id="77ffe-103">Wir ändern den Abschnitt [*Stapel Zuordnung*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) der C# Sprachspezifikation, um die Stellen zu lockern, wenn ein `stackalloc` Ausdruck möglicherweise angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="77ffe-103">We modify the section [*Stack allocation*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) of the C# language specification to relax the places when a `stackalloc` expression may appear.</span></span> <span data-ttu-id="77ffe-104">Wir löschen</span><span class="sxs-lookup"><span data-stu-id="77ffe-104">We delete</span></span>

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="77ffe-105">und ersetzen Sie Sie durch</span><span class="sxs-lookup"><span data-stu-id="77ffe-105">and replace them with</span></span>

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

<span data-ttu-id="77ffe-106">Beachten Sie, dass das Hinzufügen einer *array_initializer* zu *stackalloc_initializer* (und der Index Ausdruck optional) eine [Erweiterung in C# 7,3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) war und hier nicht beschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="77ffe-106">Note that the addition of an *array_initializer* to *stackalloc_initializer* (and making the index expression optional) was an [extension in C# 7.3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) and is not described here.</span></span>

<span data-ttu-id="77ffe-107">Der *Elementtyp* des `stackalloc` Ausdrucks ist die *unmanaged_type* mit dem Namen im stackzuzuordnen-Ausdruck, sofern vorhanden, oder der gemeinsame Typ unter den Elementen der *array_initializer* andernfalls.</span><span class="sxs-lookup"><span data-stu-id="77ffe-107">The *element type* of the `stackalloc` expression is the *unmanaged_type* named in the stackalloc expression, if any, or the common type among the elements of the *array_initializer* otherwise.</span></span>

<span data-ttu-id="77ffe-108">Der Typ der *stackalloc_initializer* mit dem *Elementtyp* `K` hängt von seinem syntaktischen Kontext ab:</span><span class="sxs-lookup"><span data-stu-id="77ffe-108">The type of the *stackalloc_initializer* with *element type* `K` depends on its syntactic context:</span></span>
- <span data-ttu-id="77ffe-109">Wenn die *stackalloc_initializer* direkt als *local_variable_initializer* einer *local_variable_declaration* -Anweisung oder einer *for_initializer*angezeigt wird, ist Ihr Typ `K*`.</span><span class="sxs-lookup"><span data-stu-id="77ffe-109">If the *stackalloc_initializer* appears directly as the *local_variable_initializer* of a *local_variable_declaration* statement or a *for_initializer*, then its type is `K*`.</span></span>
- <span data-ttu-id="77ffe-110">Andernfalls ist der Typ `System.Span<K>`.</span><span class="sxs-lookup"><span data-stu-id="77ffe-110">Otherwise its type is `System.Span<K>`.</span></span>

### <a name="stackalloc-conversion"></a><span data-ttu-id="77ffe-111">Stackzuweisung-Konvertierung</span><span class="sxs-lookup"><span data-stu-id="77ffe-111">Stackalloc Conversion</span></span>

<span data-ttu-id="77ffe-112">Die *Stack-Zuweisung* ist eine neue integrierte implizite Konvertierung von Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="77ffe-112">The *stackalloc conversion* is a new built-in implicit conversion from expression.</span></span> <span data-ttu-id="77ffe-113">Wenn der Typ eines *stackalloc_initializer* `K*`ist, gibt es eine implizite *stackzugewiesene-Konvertierung* vom *stackalloc_initializer* in den Typ `System.Span<K>`.</span><span class="sxs-lookup"><span data-stu-id="77ffe-113">When the type of a *stackalloc_initializer* is `K*`, there is an implicit *stackalloc conversion* from the *stackalloc_initializer* to the type `System.Span<K>`.</span></span>
