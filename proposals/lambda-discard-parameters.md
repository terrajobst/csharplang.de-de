---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2019
ms.locfileid: "79484057"
---
# <a name="lambda-discard-parameters"></a><span data-ttu-id="8f729-101">Lambda-Verwerfungs Parameter</span><span class="sxs-lookup"><span data-stu-id="8f729-101">Lambda discard parameters</span></span>

## <a name="summary"></a><span data-ttu-id="8f729-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="8f729-102">Summary</span></span>

<span data-ttu-id="8f729-103">Zulassen, dass verworfene (`_`) als Parameter von Lambdas und anonymen Methoden verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8f729-103">Allow discards (`_`) to be used as parameters of lambdas and anonymous methods.</span></span>
<span data-ttu-id="8f729-104">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8f729-104">For example:</span></span>
- <span data-ttu-id="8f729-105">Lambdas: `(_, _) => 0``(int _, int _) => 0`</span><span class="sxs-lookup"><span data-stu-id="8f729-105">lambdas: `(_, _) => 0`, `(int _, int _) => 0`</span></span>
- <span data-ttu-id="8f729-106">anonyme Methoden: `delegate(int _, int _) { return 0; }`</span><span class="sxs-lookup"><span data-stu-id="8f729-106">anonymous methods: `delegate(int _, int _) { return 0; }`</span></span>

## <a name="motivation"></a><span data-ttu-id="8f729-107">Motivation</span><span class="sxs-lookup"><span data-stu-id="8f729-107">Motivation</span></span>

<span data-ttu-id="8f729-108">Nicht verwendete Parameter müssen nicht benannt werden.</span><span class="sxs-lookup"><span data-stu-id="8f729-108">Unused parameters do not need to be named.</span></span> <span data-ttu-id="8f729-109">Der Zweck der verworfenen verworfenen ist klar, d. h. Sie werden nicht verwendet/verworfen</span><span class="sxs-lookup"><span data-stu-id="8f729-109">The intent of discards is clear, i.e. they are unused/discarded.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="8f729-110">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="8f729-110">Detailed design</span></span>

<span data-ttu-id="8f729-111">[Methoden Parameter](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) In der Parameterliste einer Lambda-Methode oder anonymen Methode mit mehreren Parametern namens "`_`" sind solche Parameter Parameter verwerfen.</span><span class="sxs-lookup"><span data-stu-id="8f729-111">[Method parameters](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) In the parameter list of a lambda or anonymous method with more than one parameter named `_`, such parameters are discard parameters.</span></span>
<span data-ttu-id="8f729-112">Hinweis: Wenn ein einzelner Parameter `_` benannt ist, handelt es sich um einen regulären Parameter aus Gründen der Abwärtskompatibilität.</span><span class="sxs-lookup"><span data-stu-id="8f729-112">Note: if a single parameter is named `_` then it is a regular parameter for backwards compatibility reasons.</span></span>

<span data-ttu-id="8f729-113">Bei Verwerfen von Parametern werden keine Namen in Bereiche eingeführt.</span><span class="sxs-lookup"><span data-stu-id="8f729-113">Discard parameters do not introduce any names to any scopes.</span></span>
<span data-ttu-id="8f729-114">Hinweis Dies impliziert, dass Sie keine `_` Namen (Unterstriche) ausblenden.</span><span class="sxs-lookup"><span data-stu-id="8f729-114">Note this implies they do not cause any `_` (underscore) names to be hidden.</span></span>

<span data-ttu-id="8f729-115">[Einfache Namen](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) Wenn `K` 0 (null) ist und die *Simple_name* in einem- *Block* angezeigt wird und der *Block*(oder ein einschließender *Block*) der Deklaration der lokalen Variablen Deklaration ([Deklarationen](basic-concepts.md#declarations)) eine lokale Variable, einen Parameter (mit Ausnahme von Parameter verwerfen) oder eine Konstante mit dem Namen `I`enthält, verweist der *Simple_name* auf diese lokale Variable, den Parameter oder die Konstante und wird als Variable oder Wert</span><span class="sxs-lookup"><span data-stu-id="8f729-115">[Simple names](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter (with the exception of discard parameters) or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>

<span data-ttu-id="8f729-116">[Bereiche](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) Mit Ausnahme von Verwerfungs Parametern ist der Gültigkeitsbereich eines in einem *lambda_expression* ([Anonyme Funktions Ausdrücke](expressions.md#anonymous-function-expressions)) deklarierten Parameters der *anonymous_function_body* von, der mit Ausnahme von Parameter verwerfen *lambda_expression* . der Gültigkeitsbereich eines in einem *anonymous_method_expression* ([anonymen Funktions Ausdruck](expressions.md#anonymous-function-expressions)) deklarierten Parameters ist der *Block* dieser *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="8f729-116">[Scopes](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) With the exception of discard parameters, the scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression* With the exception of discard parameters, the scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>

## <a name="related-spec-sections"></a><span data-ttu-id="8f729-117">Verwandte spec-Abschnitte</span><span class="sxs-lookup"><span data-stu-id="8f729-117">Related spec sections</span></span>
- [<span data-ttu-id="8f729-118">Entsprechende Parameter</span><span class="sxs-lookup"><span data-stu-id="8f729-118">Corresponding parameters</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
