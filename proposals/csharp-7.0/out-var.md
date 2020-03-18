---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483505"
---
# <a name="out-variable-declarations"></a><span data-ttu-id="da5f7-101">out-Variablendeklaration</span><span class="sxs-lookup"><span data-stu-id="da5f7-101">Out variable declarations</span></span>

<span data-ttu-id="da5f7-102">Mit der *Deklaration der Out-Variablen Deklaration* kann eine Variable an dem Speicherort deklariert werden, an dem Sie als `out` Argument an Sie übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="da5f7-102">The *out variable declaration* feature enables a variable to be declared at the location that it is being passed as an `out` argument.</span></span>

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

<span data-ttu-id="da5f7-103">Eine auf diese Weise deklarierte Variable wird als *out-Variable*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="da5f7-103">A variable declared this way is called an *out variable*.</span></span> <span data-ttu-id="da5f7-104">Sie können das kontextabhängige Schlüsselwort `var` für den Variablentyp verwenden.</span><span class="sxs-lookup"><span data-stu-id="da5f7-104">You may use the contextual keyword `var` for the variable's type.</span></span> <span data-ttu-id="da5f7-105">Der Gültigkeitsbereich ist derselbe wie für eine *Pattern-Variable* , die durch Muster Übereinstimmung eingeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="da5f7-105">The scope will be the same as for a *pattern-variable* introduced via pattern-matching.</span></span>

<span data-ttu-id="da5f7-106">Gemäß der Sprachspezifikation (Abschnitt 7.6.7 Element Access) enthält die Argument-List eines Element Zugriffs (Index Ausdruck) keine ref-oder out-Argumente.</span><span class="sxs-lookup"><span data-stu-id="da5f7-106">According to Language Specification (section 7.6.7 Element access) the argument-list of an element-access (indexing expression) does not contain ref or out arguments.</span></span> <span data-ttu-id="da5f7-107">Allerdings werden Sie vom Compiler für verschiedene Szenarien zugelassen, z. b. Indexer, die in Metadaten deklariert sind, die `out`akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="da5f7-107">However, they are permitted by the compiler for various scenarios, for example indexers declared in metadata that accept `out`.</span></span>

<span data-ttu-id="da5f7-108">Innerhalb des Gültigkeits Bereichs einer lokalen Variablen, die von einem argument_value eingeführt wurde, handelt es sich um einen Kompilierzeitfehler, der auf diese lokale Variable in einer Textposition verweist, die der Deklaration vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="da5f7-108">Within the scope of a local variable introduced by an argument_value, it is a compile-time error to refer to that local variable in a textual position that precedes its declaration.</span></span>

<span data-ttu-id="da5f7-109">Es ist auch ein Fehler, auf eine implizit typisierte (§ 8.5.1) out-Variable in derselben Argumentliste zu verweisen, die sofort Ihre Deklaration enthält.</span><span class="sxs-lookup"><span data-stu-id="da5f7-109">It is also an error to reference an implicitly-typed (§8.5.1) out variable in the same argument list that immediately contains its declaration.</span></span>

<span data-ttu-id="da5f7-110">Die Überladungs Auflösung wird wie folgt geändert:</span><span class="sxs-lookup"><span data-stu-id="da5f7-110">Overload resolution is modified as follows:</span></span>

<span data-ttu-id="da5f7-111">Wir fügen eine neue Konvertierung hinzu:</span><span class="sxs-lookup"><span data-stu-id="da5f7-111">We add a new conversion:</span></span>

> <span data-ttu-id="da5f7-112">Es gibt eine *Konvertierung von einem Ausdruck* aus einer implizit typisierten out-Variablen Deklaration in jeden Typ.</span><span class="sxs-lookup"><span data-stu-id="da5f7-112">There is a *conversion from expression* from an implicitly-typed out variable declaration to every type.</span></span>

<span data-ttu-id="da5f7-113">Außerdem</span><span class="sxs-lookup"><span data-stu-id="da5f7-113">Also</span></span>

> <span data-ttu-id="da5f7-114">Der Typ eines explizit typisierten out-Variablen Arguments ist der deklarierte Typ.</span><span class="sxs-lookup"><span data-stu-id="da5f7-114">The type of an explicitly-typed out variable argument is the declared type.</span></span>

<span data-ttu-id="da5f7-115">und</span><span class="sxs-lookup"><span data-stu-id="da5f7-115">and</span></span>

> <span data-ttu-id="da5f7-116">Ein implizit typisiertes out-Variablen Argument weist keinen Typ auf.</span><span class="sxs-lookup"><span data-stu-id="da5f7-116">An implicitly-typed out variable argument has no type.</span></span>

<span data-ttu-id="da5f7-117">Die *Konvertierung von einem Ausdruck* aus einer implizit typisierten out-Variablen Deklaration ist nicht besser als eine andere *Konvertierung von Ausdruck*.</span><span class="sxs-lookup"><span data-stu-id="da5f7-117">The *conversion from expression* from an implicitly-typed out variable declaration is not considered better than any other *conversion from expression*.</span></span>

<span data-ttu-id="da5f7-118">Der Typ der implizit typisierten out-Variablen ist der Typ des entsprechenden Parameters in der Signatur der Methode, die von der Überladungs Auflösung ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="da5f7-118">The type of an implicitly-typed out variable is the type of the corresponding parameter in the signature of the method selected by overload resolution.</span></span>

<span data-ttu-id="da5f7-119">Der neue Syntax Knoten `DeclarationExpressionSyntax` wird zur Darstellung der Deklaration in einem out var-Argument hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="da5f7-119">The new syntax node `DeclarationExpressionSyntax` is added to represent the declaration in an out var argument.</span></span>
