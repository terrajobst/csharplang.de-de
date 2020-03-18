---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483535"
---
# <a name="throw-expression"></a><span data-ttu-id="8bd01-101">Throw-Ausdruck</span><span class="sxs-lookup"><span data-stu-id="8bd01-101">Throw expression</span></span>

<span data-ttu-id="8bd01-102">Wir erweitern den Satz von Ausdrucks Formularen auf include.</span><span class="sxs-lookup"><span data-stu-id="8bd01-102">We extend the set of expression forms to include</span></span>

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

<span data-ttu-id="8bd01-103">Die Typregeln lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="8bd01-103">The type rules are as follows:</span></span>

- <span data-ttu-id="8bd01-104">Ein *throw_expression* weist keinen Typ auf.</span><span class="sxs-lookup"><span data-stu-id="8bd01-104">A *throw_expression* has no type.</span></span>
- <span data-ttu-id="8bd01-105">Eine *throw_expression* kann durch eine implizite Konvertierung in jeden Typ konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="8bd01-105">A *throw_expression* is convertible to every type by an implicit conversion.</span></span>

<span data-ttu-id="8bd01-106">Ein *throw-Ausdruck* löst den Wert aus, der durch Auswerten des *null_coalescing_expression*erzeugt wird, der einen Wert des Klassen Typs `System.Exception`angeben muss, eines Klassen Typs, der von `System.Exception` abgeleitet ist, oder eines Typparameter Typs, der über `System.Exception` (oder eine Unterklasse) als effektive Basisklasse verfügt.</span><span class="sxs-lookup"><span data-stu-id="8bd01-106">A *throw expression* throws the value produced by evaluating the *null_coalescing_expression*, which must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="8bd01-107">Wenn die Auswertung des Ausdrucks `null`erzeugt, wird stattdessen eine `System.NullReferenceException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="8bd01-107">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="8bd01-108">Das Verhalten zur Laufzeit der Auswertung eines Throw- *Ausdrucks* entspricht dem [für eine *throw-Anweisung*angegebenen](../../spec/statements.md#the-throw-statement)Verhalten.</span><span class="sxs-lookup"><span data-stu-id="8bd01-108">The behavior at runtime of the evaluation of a *throw expression* is the same [as specified for a *throw statement*](../../spec/statements.md#the-throw-statement).</span></span>

<span data-ttu-id="8bd01-109">Die Regeln für die Fluss Analyse lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="8bd01-109">The flow-analysis rules are as follows:</span></span>

- <span data-ttu-id="8bd01-110">Für jede Variable *v*wird *v* definitiv vor dem *null_coalescing_expression* eines *throw_expression* IFF zugewiesen, dass es definitiv vor dem *throw_expression*zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="8bd01-110">For every variable *v*, *v* is definitely assigned before the *null_coalescing_expression* of a *throw_expression* iff it is definitely assigned before the *throw_expression*.</span></span>
- <span data-ttu-id="8bd01-111">Für jede Variable *v*wird *v* nach *throw_expression*definitiv zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="8bd01-111">For every variable *v*, *v* is definitely assigned after *throw_expression*.</span></span>

<span data-ttu-id="8bd01-112">Ein *throw-Ausdruck* ist nur in den folgenden syntaktischen Kontexten zulässig:</span><span class="sxs-lookup"><span data-stu-id="8bd01-112">A *throw expression* is permitted in only the following syntactic contexts:</span></span>
- <span data-ttu-id="8bd01-113">Der zweite oder dritte Operand eines ternären bedingten Operators `?:`</span><span class="sxs-lookup"><span data-stu-id="8bd01-113">As the second or third operand of a ternary conditional operator `?:`</span></span>
- <span data-ttu-id="8bd01-114">Der zweite Operand eines NULL-Sammel Operators `??`</span><span class="sxs-lookup"><span data-stu-id="8bd01-114">As the second operand of a null coalescing operator `??`</span></span>
- <span data-ttu-id="8bd01-115">Als Text eines Ausdrucks Körper Ausdrucks oder einer Methode.</span><span class="sxs-lookup"><span data-stu-id="8bd01-115">As the body of an expression-bodied lambda or method.</span></span>
