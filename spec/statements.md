---
ms.openlocfilehash: 7248a91976c479dc1b6b64b799639635617a7bec
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704042"
---
# <a name="statements"></a><span data-ttu-id="acac3-101">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="acac3-101">Statements</span></span>

<span data-ttu-id="acac3-102">C#stellt eine Reihe von-Anweisungen bereit.</span><span class="sxs-lookup"><span data-stu-id="acac3-102">C# provides a variety of statements.</span></span> <span data-ttu-id="acac3-103">Die meisten dieser Anweisungen werden Entwicklern vertraut sein, die in C und C++programmiert haben.</span><span class="sxs-lookup"><span data-stu-id="acac3-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

<span data-ttu-id="acac3-104">Das *embedded_statement* nicht Terminal wird für Anweisungen verwendet, die in anderen Anweisungen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="acac3-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="acac3-105">Die Verwendung von *embedded_statement* anstelle von- *Anweisungen schließt die* Verwendung von Deklarations Anweisungen und bezeichneten Anweisungen in diesen Kontexten aus.</span><span class="sxs-lookup"><span data-stu-id="acac3-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="acac3-106">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="acac3-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="acac3-107">führt zu einem Kompilierzeitfehler, da eine `if`-Anweisung anstelle einer- *Anweisung* für Ihre if-Verzweigung eine *embedded_statement* -Anweisung erfordert.</span><span class="sxs-lookup"><span data-stu-id="acac3-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="acac3-108">Wenn dieser Code zulässig ist, wird die Variable `i` deklariert, Sie konnte aber nie verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="acac3-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="acac3-109">Beachten Sie jedoch, dass das Beispiel `i`durch Platzieren der Deklaration in einem-Block gültig ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="acac3-110">Endpunkte und Erreichbarkeit</span><span class="sxs-lookup"><span data-stu-id="acac3-110">End points and reachability</span></span>

<span data-ttu-id="acac3-111">Jede-Anweisung verfügt über einen ***Endpunkt***.</span><span class="sxs-lookup"><span data-stu-id="acac3-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="acac3-112">Der Endpunkt einer-Anweisung ist in intuitiver Hinsicht der Speicherort, der direkt auf die-Anweisung folgt.</span><span class="sxs-lookup"><span data-stu-id="acac3-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="acac3-113">Die Ausführungs Regeln für zusammengesetzte Anweisungen (Anweisungen, die eingebettete Anweisungen enthalten) geben die Aktion an, die durchgeführt wird, wenn das Steuerelement den Endpunkt einer eingebetteten Anweisung erreicht.</span><span class="sxs-lookup"><span data-stu-id="acac3-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="acac3-114">Wenn das Steuerelement z. b. den Endpunkt einer-Anweisung in einem-Block erreicht, wird die Steuerung an die nächste Anweisung im-Block übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="acac3-115">Wenn eine-Anweisung möglicherweise durch die Ausführung erreicht werden kann, wird die-Anweisung als ***erreichbar***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="acac3-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="acac3-116">Wenn es nicht möglich ist, dass eine-Anweisung ausgeführt wird, wird die-Anweisung als ***nicht erreichbar***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="acac3-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="acac3-117">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="acac3-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="acac3-118">der zweite Aufruf von `Console.WriteLine` ist nicht erreichbar, da es nicht möglich ist, dass die Anweisung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="acac3-119">Eine Warnung wird gemeldet, wenn der Compiler feststellt, dass eine-Anweisung nicht erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="acac3-120">Es ist kein Fehler, wenn eine-Anweisung nicht erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="acac3-121">Um zu ermitteln, ob eine bestimmte Anweisung oder ein Endpunkt erreichbar ist, führt der Compiler die Fluss Analyse gemäß den für jede Anweisung definierten Erreichbarkeits Regeln aus.</span><span class="sxs-lookup"><span data-stu-id="acac3-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="acac3-122">Die Fluss Analyse berücksichtigt die Werte konstanter Ausdrücke ([Konstantenausdrücke](expressions.md#constant-expressions)), die das Verhalten von-Anweisungen steuern, aber die möglichen Werte von nicht konstanten Ausdrücken werden nicht berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="acac3-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="acac3-123">Dies bedeutet, dass ein nicht konstanter Ausdruck eines bestimmten Typs für die Ablauf Steuerungs Analyse einen möglichen Wert dieses Typs hat.</span><span class="sxs-lookup"><span data-stu-id="acac3-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="acac3-124">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="acac3-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="acac3-125">der boolesche Ausdruck der `if` -Anweisung ist ein konstanter Ausdruck, da beide Operanden `==` des Operators Konstanten sind.</span><span class="sxs-lookup"><span data-stu-id="acac3-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="acac3-126">Da der Konstante Ausdruck zum Zeitpunkt der Kompilierung ausgewertet wird und den Wert `false`erzeugt, `Console.WriteLine` wird der Aufruf als nicht erreichbar betrachtet.</span><span class="sxs-lookup"><span data-stu-id="acac3-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="acac3-127">Wenn `i` jedoch in eine lokale Variable geändert wird</span><span class="sxs-lookup"><span data-stu-id="acac3-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="acac3-128">der `Console.WriteLine` Aufruf wird als erreichbar betrachtet, obwohl er in Wirklichkeit nie ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="acac3-129">Der *Block* eines Funktionsmembers gilt immer als erreichbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="acac3-130">Wenn Sie die Erreichbarkeits Regeln der einzelnen Anweisungen in einem Block nacheinander auswerten, kann die Erreichbarkeit einer beliebigen Anweisung bestimmt werden.</span><span class="sxs-lookup"><span data-stu-id="acac3-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="acac3-131">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="acac3-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="acac3-132">die Erreichbarkeit der zweiten `Console.WriteLine` wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="acac3-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="acac3-133">Die erste `Console.WriteLine` Ausdrucks Anweisung ist erreichbar, da der-Block `F` der-Methode erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="acac3-134">Der Endpunkt der ersten `Console.WriteLine` Ausdrucks Anweisung ist erreichbar, da diese Anweisung erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="acac3-135">Die `if` -Anweisung ist erreichbar, da der Endpunkt der ersten `Console.WriteLine` Ausdrucks Anweisung erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="acac3-136">Die zweite `Console.WriteLine` Ausdrucks Anweisung ist erreichbar, da der boolesche Ausdruck `if` der Anweisung nicht über den konstanten Wert `false`verfügt.</span><span class="sxs-lookup"><span data-stu-id="acac3-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="acac3-137">Es gibt zwei Situationen, in denen es sich um einen Kompilierzeitfehler für den Endpunkt einer-Anweisung handelt, der erreichbar ist:</span><span class="sxs-lookup"><span data-stu-id="acac3-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="acac3-138">Da die `switch` Anweisung nicht zulässt, dass ein Switch-Abschnitt zum nächsten switch-Abschnitt "durchläuft", ist dies ein Kompilierzeitfehler für den Endpunkt der Anweisungs Liste eines Switch-Abschnitts, der erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="acac3-139">Wenn dieser Fehler auftritt, ist dies in der Regel ein Hinweis `break` darauf, dass eine-Anweisung fehlt.</span><span class="sxs-lookup"><span data-stu-id="acac3-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="acac3-140">Es handelt sich um einen Kompilierzeitfehler für den Endpunkt des Blocks eines Funktionsmembers, der einen Wert berechnet, der erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="acac3-141">Wenn dieser Fehler auftritt, ist dies in der Regel ein Hinweis `return` darauf, dass eine-Anweisung fehlt.</span><span class="sxs-lookup"><span data-stu-id="acac3-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="acac3-142">Blöcke</span><span class="sxs-lookup"><span data-stu-id="acac3-142">Blocks</span></span>

<span data-ttu-id="acac3-143">Ein *Block* ermöglicht, mehrere Anweisungen in Kontexten zu schreiben, in denen eine einzelne Anweisung zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="acac3-144">Ein- *Block* besteht aus einer optionalen *statement_list* ([Anweisungs Liste](statements.md#statement-lists)), die in geschweiften Klammern eingeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="acac3-145">Wenn die Anweisungs Liste weggelassen wird, wird der Block als leer bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="acac3-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="acac3-146">Ein-Block kann Deklarations Anweisungen ([Deklarations Anweisungen](statements.md#declaration-statements)) enthalten.</span><span class="sxs-lookup"><span data-stu-id="acac3-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="acac3-147">Der Gültigkeitsbereich einer lokalen Variablen oder Konstanten, die in einem-Block deklariert ist, ist der-Block.</span><span class="sxs-lookup"><span data-stu-id="acac3-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="acac3-148">Ein-Block wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="acac3-149">Wenn der-Block leer ist, wird die Steuerung an den Endpunkt des-Blocks übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="acac3-150">Wenn der Block nicht leer ist, wird die Steuerung an die Anweisungs Liste übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="acac3-151">Wenn und wenn das Steuerelement den Endpunkt der Anweisungs Liste erreicht, wird die Steuerung an den Endpunkt des Blocks übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="acac3-152">Die Anweisungs Liste eines-Blocks ist erreichbar, wenn der Block selbst erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="acac3-153">Der Endpunkt eines-Blocks ist erreichbar, wenn der-Block leer ist oder der Endpunkt der Anweisungs Liste erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="acac3-154">Ein- *Block* , der eine oder `yield` mehrere-Anweisungen ([die yield-Anweisung](statements.md#the-yield-statement)) enthält, wird als Iteratorblock bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="acac3-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="acac3-155">Iteratorblöcke werden verwendet, um Funktionsmember als Iteratoren ([Iteratoren](classes.md#iterators)) zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="acac3-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="acac3-156">Für iteratorblöcke gelten einige zusätzliche Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="acac3-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="acac3-157">Es ist ein Kompilierzeitfehler, wenn `return` eine Anweisung in einem Iteratorblock angezeigt wird ( `yield return` Anweisungen sind jedoch zulässig).</span><span class="sxs-lookup"><span data-stu-id="acac3-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="acac3-158">Es ist ein Kompilierzeitfehler für einen Iteratorblock, der einen unsicheren Kontext ([unsichere Kontexte](unsafe-code.md#unsafe-contexts)) enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="acac3-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="acac3-159">Ein Iteratorblock definiert immer einen sicheren Kontext, auch wenn seine Deklaration in einem unsicheren Kontext eingebettet ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="acac3-160">Anweisungs Listen</span><span class="sxs-lookup"><span data-stu-id="acac3-160">Statement lists</span></span>

<span data-ttu-id="acac3-161">Eine ***Anweisungs Liste*** besteht aus einer oder mehreren Anweisungen, die nacheinander geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="acac3-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="acac3-162">Anweisungs Listen treten in *Block*s ([Blocks](statements.md#blocks)) und in *switch_block*s ([der Switch-Anweisung](statements.md#the-switch-statement)) auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="acac3-163">Eine Anweisungs Liste wird ausgeführt, indem die Steuerung an die erste Anweisung übertragen wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="acac3-164">Wenn und wenn das Steuerelement den Endpunkt einer-Anweisung erreicht, wird die Steuerung an die nächste Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="acac3-165">Wenn und wenn das Steuerelement den Endpunkt der letzten Anweisung erreicht, wird die Steuerung an den Endpunkt der Anweisungs Liste übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="acac3-166">Eine-Anweisung in einer Anweisungs Liste ist erreichbar, wenn mindestens einer der folgenden Punkte zutrifft:</span><span class="sxs-lookup"><span data-stu-id="acac3-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="acac3-167">Die-Anweisung ist die erste Anweisung, und die Anweisungs Liste selbst ist erreichbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="acac3-168">Der Endpunkt der vorhergehenden Anweisung ist erreichbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="acac3-169">Die-Anweisung ist eine Anweisung mit Bezeichnung, und auf die Bezeichnung wird `goto` durch eine erreichbare Anweisung verwiesen.</span><span class="sxs-lookup"><span data-stu-id="acac3-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="acac3-170">Der Endpunkt einer Anweisungs Liste ist erreichbar, wenn der Endpunkt der letzten Anweisung in der Liste erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="acac3-171">Die leere Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-171">The empty statement</span></span>

<span data-ttu-id="acac3-172">Ein *empty_statement* tut nichts.</span><span class="sxs-lookup"><span data-stu-id="acac3-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="acac3-173">Eine leere-Anweisung wird verwendet, wenn keine Vorgänge in einem Kontext durchgeführt werden müssen, in dem eine-Anweisung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="acac3-174">Durch die Ausführung einer leeren-Anweisung wird die Steuerung einfach an den Endpunkt der Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="acac3-175">Daher ist der Endpunkt einer leeren Anweisung erreichbar, wenn die leere Anweisung erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="acac3-176">Eine leere Anweisung kann beim Schreiben einer `while` Anweisung mit einem NULL-Text verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="acac3-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="acac3-177">Außerdem kann eine leere Anweisung verwendet werden, um eine Bezeichnung direkt vor dem schließenden "`}`" eines Blocks zu deklarieren:</span><span class="sxs-lookup"><span data-stu-id="acac3-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="acac3-178">Anweisungen mit Bezeichnung</span><span class="sxs-lookup"><span data-stu-id="acac3-178">Labeled statements</span></span>

<span data-ttu-id="acac3-179">Ein *labeled_statement* ermöglicht einer-Anweisung, eine Bezeichnung als Präfix festzustellen.</span><span class="sxs-lookup"><span data-stu-id="acac3-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="acac3-180">Anweisung mit Bezeichnung ist in-Blöcken zulässig, aber nicht als eingebettete Anweisungen zulässig.</span><span class="sxs-lookup"><span data-stu-id="acac3-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="acac3-181">Eine Anweisung mit Bezeichnung deklariert eine Bezeichnung mit dem Namen, der vom *Bezeichner*angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="acac3-182">Der Gültigkeitsbereich einer Bezeichnung ist der gesamte Block, in dem die Bezeichnung deklariert wird, einschließlich aller Blöcke, die in der Liste enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="acac3-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="acac3-183">Es handelt sich um einen Kompilierzeitfehler für zwei Bezeichnungen mit dem gleichen Namen, die sich überlappende Bereiche befinden.</span><span class="sxs-lookup"><span data-stu-id="acac3-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="acac3-184">Auf eine Bezeichnung kann in- `goto` Anweisungen ([der GOTO-Anweisung](statements.md#the-goto-statement)) innerhalb des Bereichs der Bezeichnung verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="acac3-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="acac3-185">Dies bedeutet, `goto` dass-Anweisungen die Steuerung innerhalb von Blöcken und aus Blöcken, aber nie in Blöcke übertragen können.</span><span class="sxs-lookup"><span data-stu-id="acac3-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="acac3-186">Bezeichnungen verfügen über einen eigenen Deklarations Bereich und stören andere Bezeichner nicht.</span><span class="sxs-lookup"><span data-stu-id="acac3-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="acac3-187">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="acac3-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="acac3-188">ist gültig und verwendet den Namen `x` sowohl als Parameter als auch als Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="acac3-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="acac3-189">Die Ausführung einer Anweisung mit Bezeichnung entspricht genau der Ausführung der Anweisung nach der Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="acac3-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="acac3-190">Zusätzlich zur Erreichbarkeit der normalen Ablauf Steuerung ist eine Anweisung mit der Bezeichnung erreichbar, wenn von einer erreichbaren `goto` Anweisung auf die Bezeichnung verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="acac3-191">Distanzieren Wenn sich `goto` eine-Anweisung innerhalb `try` eines-Blocks `finally` befindet `try`, der einen-Block enthält, und die Anweisung mit der Bezeichnung außerhalb von `finally` liegt und der Endpunkt des Blocks nicht erreichbar ist, ist die Anweisung mit der Bezeichnung nicht erreichbar. Diese `goto` Anweisung.)</span><span class="sxs-lookup"><span data-stu-id="acac3-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="acac3-192">Deklarationsanweisungen</span><span class="sxs-lookup"><span data-stu-id="acac3-192">Declaration statements</span></span>

<span data-ttu-id="acac3-193">Ein *declaration_statement* deklariert eine lokale Variable oder Konstante.</span><span class="sxs-lookup"><span data-stu-id="acac3-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="acac3-194">Deklarations Anweisungen sind in-Blöcken zulässig, aber nicht als eingebettete Anweisungen zulässig.</span><span class="sxs-lookup"><span data-stu-id="acac3-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="acac3-195">Deklarationen von lokalen Variablen</span><span class="sxs-lookup"><span data-stu-id="acac3-195">Local variable declarations</span></span>

<span data-ttu-id="acac3-196">Ein *local_variable_declaration* deklariert eine oder mehrere lokale Variablen.</span><span class="sxs-lookup"><span data-stu-id="acac3-196">A *local_variable_declaration* declares one or more local variables.</span></span>

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

<span data-ttu-id="acac3-197">Der *local_variable_type* eines *local_variable_declaration* gibt entweder direkt den Typ der Variablen an, die von der Deklaration eingeführt wurden, oder gibt mit dem Bezeichner an, `var`, dass der Typ basierend auf einem Initialisierer abgeleitet werden soll.</span><span class="sxs-lookup"><span data-stu-id="acac3-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="acac3-198">Auf den Typ folgt eine Liste von *local_variable_declarator*s, von denen jeder eine neue Variable einführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="acac3-199">Ein *local_variable_declarator* besteht aus einem *Bezeichner* , der die Variable benennt, optional gefolgt von einem "`=`"-Token und einem *local_variable_initializer* , das den Anfangswert der Variablen liefert.</span><span class="sxs-lookup"><span data-stu-id="acac3-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="acac3-200">Im Kontext einer lokalen Variablen Deklaration fungiert der Bezeichner var als kontextbezogenes Schlüsselwort ([Schlüssel](lexical-structure.md#keywords)Wort). Wenn *local_variable_type* als `var` angegeben wird und sich kein Typ mit dem Namen `var` im Gültigkeitsbereich befindet, ist die Deklaration eine ***implizit typisierte lokale Variablen Deklaration***, deren Typ vom Typ des zugeordneten initialisiererausdrucks abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="acac3-201">Implizit typisierte lokale Variablen Deklarationen unterliegen den folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="acac3-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="acac3-202">*Local_variable_declaration* kann nicht mehrere *local_variable_declarator*s enthalten.</span><span class="sxs-lookup"><span data-stu-id="acac3-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="acac3-203">*Local_variable_declarator* muss ein *local_variable_initializer*enthalten.</span><span class="sxs-lookup"><span data-stu-id="acac3-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="acac3-204">Der *local_variable_initializer* muss ein *Ausdruck*sein.</span><span class="sxs-lookup"><span data-stu-id="acac3-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="acac3-205">Der initialisiererausdruck muss einen Kompilier Zeittyp aufweisen.</span><span class="sxs-lookup"><span data-stu-id="acac3-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="acac3-206">Der initialisiererausdruck kann nicht auf die deklarierte Variable selbst verweisen.</span><span class="sxs-lookup"><span data-stu-id="acac3-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="acac3-207">Im folgenden finden Sie Beispiele für falsche implizit typisierte lokale Variablen Deklarationen:</span><span class="sxs-lookup"><span data-stu-id="acac3-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="acac3-208">Der Wert einer lokalen Variablen wird in einem Ausdruck mithilfe eines *Simple_name* ([simple names](expressions.md#simple-names)) abgerufen, und der Wert einer lokalen Variablen wird mithilfe einer *Zuweisung* ([Zuweisungs Operatoren](expressions.md#assignment-operators)) geändert.</span><span class="sxs-lookup"><span data-stu-id="acac3-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="acac3-209">Eine lokale Variable muss an jedem Speicherort, an dem ihr Wert abgerufen wird, definitiv zugewiesen werden ([definitive Zuweisung](variables.md#definite-assignment)).</span><span class="sxs-lookup"><span data-stu-id="acac3-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="acac3-210">Der Gültigkeitsbereich einer lokalen Variablen, die in einem *local_variable_declaration* deklariert ist, ist der Block, in dem die Deklaration auftritt.</span><span class="sxs-lookup"><span data-stu-id="acac3-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="acac3-211">Es ist ein Fehler, auf eine lokale Variable in einer Textposition zu verweisen, die der *local_variable_declarator* der lokalen Variablen vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="acac3-212">Innerhalb des Gültigkeits Bereichs einer lokalen Variablen ist es ein Kompilierzeitfehler, eine andere lokale Variable oder Konstante mit dem gleichen Namen zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="acac3-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="acac3-213">Eine lokale Variablen Deklaration, die mehrere Variablen deklariert, entspricht mehreren Deklarationen von einzelnen Variablen mit demselben Typ.</span><span class="sxs-lookup"><span data-stu-id="acac3-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="acac3-214">Außerdem entspricht ein Variableninitialisierer in einer lokalen Variablen Deklaration genau einer Zuweisungsanweisung, die unmittelbar nach der Deklaration eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="acac3-215">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="acac3-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="acac3-216">entspricht genau</span><span class="sxs-lookup"><span data-stu-id="acac3-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="acac3-217">In einer implizit typisierten lokalen Variablen Deklaration wird der Typ der zu deklarierenden lokalen Variablen mit dem Typ des Ausdrucks, der zum Initialisieren der Variablen verwendet wird, identisch sein.</span><span class="sxs-lookup"><span data-stu-id="acac3-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="acac3-218">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="acac3-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="acac3-219">Die oben genannten implizit typisierten lokalen Variablen Deklarationen entsprechen genau den folgenden explizit typisierten Deklarationen:</span><span class="sxs-lookup"><span data-stu-id="acac3-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="acac3-220">Lokale Konstante Deklarationen</span><span class="sxs-lookup"><span data-stu-id="acac3-220">Local constant declarations</span></span>

<span data-ttu-id="acac3-221">Ein *local_constant_declaration* deklariert eine oder mehrere lokale Konstanten.</span><span class="sxs-lookup"><span data-stu-id="acac3-221">A *local_constant_declaration* declares one or more local constants.</span></span>

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="acac3-222">Der *Typ* eines *local_constant_declaration* gibt den Typ der Konstanten an, die von der Deklaration eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="acac3-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="acac3-223">Auf den Typ folgt eine Liste von *constant_declarator*s, von denen jeder eine neue Konstante einführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="acac3-224">Ein *constant_declarator* besteht aus einem *Bezeichner* , der die Konstante benennt, gefolgt von einem "`=`"-Token, gefolgt von einem *constant_expression* ([Konstantenausdrücken](expressions.md#constant-expressions)), der den Wert der Konstante liefert.</span><span class="sxs-lookup"><span data-stu-id="acac3-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="acac3-225">Der- *Typ* und der *constant_expression* -Wert einer lokalen Konstanten Deklaration müssen dieselben Regeln wie die einer Konstanten Element Deklaration ([Konstanten](classes.md#constants)) einhalten.</span><span class="sxs-lookup"><span data-stu-id="acac3-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="acac3-226">Der Wert einer lokalen Konstante wird in einem Ausdruck mithilfe eines *Simple_name* ([simple names](expressions.md#simple-names)) abgerufen.</span><span class="sxs-lookup"><span data-stu-id="acac3-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="acac3-227">Der Gültigkeitsbereich einer lokalen Konstante ist der Block, in dem die Deklaration auftritt.</span><span class="sxs-lookup"><span data-stu-id="acac3-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="acac3-228">Es ist ein Fehler, auf eine lokale Konstante in einer Textposition zu verweisen, die der *constant_declarator*vorausgeht.</span><span class="sxs-lookup"><span data-stu-id="acac3-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="acac3-229">Innerhalb des Gültigkeits Bereichs einer lokalen Konstante handelt es sich um einen Kompilierzeitfehler, um eine andere lokale Variable oder Konstante mit dem gleichen Namen zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="acac3-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="acac3-230">Eine lokale Konstantendeklaration, die mehrere Konstanten deklariert, entspricht mehreren Deklarationen von einzelnen Konstanten desselben Typs.</span><span class="sxs-lookup"><span data-stu-id="acac3-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="acac3-231">Ausdrucksanweisungen</span><span class="sxs-lookup"><span data-stu-id="acac3-231">Expression statements</span></span>

<span data-ttu-id="acac3-232">Ein *expression_statement* wertet einen angegebenen Ausdruck aus.</span><span class="sxs-lookup"><span data-stu-id="acac3-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="acac3-233">Der von dem Ausdruck berechnete Wert, falls vorhanden, wird verworfen.</span><span class="sxs-lookup"><span data-stu-id="acac3-233">The value computed by the expression, if any, is discarded.</span></span>

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

<span data-ttu-id="acac3-234">Nicht alle Ausdrücke sind als Anweisungen zulässig.</span><span class="sxs-lookup"><span data-stu-id="acac3-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="acac3-235">Insbesondere Ausdrücke wie `x + y` und, die nur `x == 1` einen Wert berechnen (der verworfen wird), sind nicht als-Anweisungen zulässig.</span><span class="sxs-lookup"><span data-stu-id="acac3-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="acac3-236">Die Ausführung eines *expression_statement* wertet den enthaltenen Ausdruck aus und überträgt dann die Steuerung an den Endpunkt des *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="acac3-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="acac3-237">Der Endpunkt eines *expression_statement* ist erreichbar, wenn dieser *expression_statement* erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="acac3-238">Auswahlanweisungen</span><span class="sxs-lookup"><span data-stu-id="acac3-238">Selection statements</span></span>

<span data-ttu-id="acac3-239">Auswahl Anweisungen wählen Sie eine der möglichen Anweisungen für die Ausführung basierend auf dem Wert eines Ausdrucks aus.</span><span class="sxs-lookup"><span data-stu-id="acac3-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="acac3-240">Die if-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-240">The if statement</span></span>

<span data-ttu-id="acac3-241">Die `if` -Anweisung wählt eine Anweisung für die Ausführung basierend auf dem Wert eines booleschen Ausdrucks aus.</span><span class="sxs-lookup"><span data-stu-id="acac3-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="acac3-242">Ein `else` Teil ist mit dem lexikalisch nächstgelegenen `if` vorangehenden-Element verknüpft, das von der Syntax zugelassen wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="acac3-243">Folglich eine `if` -Anweisung der Form</span><span class="sxs-lookup"><span data-stu-id="acac3-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="acac3-244">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="acac3-244">is equivalent to</span></span>
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

<span data-ttu-id="acac3-245">Eine `if` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="acac3-246">Die *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)) werden ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="acac3-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="acac3-247">Wenn der boolesche Ausdruck ergibt `true`, wird die Steuerung an die erste eingebettete Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="acac3-248">Wenn und wenn das Steuerelement den Endpunkt dieser Anweisung erreicht, wird die Steuerung an den Endpunkt `if` der Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="acac3-249">Wenn der boolesche Ausdruck ergibt `false` und ein `else` Teil vorhanden ist, wird die Steuerung an die zweite eingebettete Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="acac3-250">Wenn und wenn das Steuerelement den Endpunkt dieser Anweisung erreicht, wird die Steuerung an den Endpunkt `if` der Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="acac3-251">Wenn der boolesche Ausdruck ergibt `false` und ein `else` Teil nicht vorhanden ist, wird die Steuerung an `if` den Endpunkt der Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="acac3-252">Die erste eingebettete Anweisung einer `if` -Anweisung ist erreichbar, `if` wenn die-Anweisung erreichbar ist und der boolesche Ausdruck nicht über den `false`Konstanten Wert verfügt.</span><span class="sxs-lookup"><span data-stu-id="acac3-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="acac3-253">Die zweite eingebettete Anweisung einer `if` -Anweisung (falls vorhanden) ist erreichbar, `if` wenn die-Anweisung erreichbar ist und der boolesche Ausdruck nicht über den `true`Konstanten Wert verfügt.</span><span class="sxs-lookup"><span data-stu-id="acac3-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="acac3-254">Der Endpunkt `if` einer-Anweisung ist erreichbar, wenn der Endpunkt von mindestens einer der eingebetteten Anweisungen erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="acac3-255">Außerdem ist der `if` Endpunkt einer-Anweisung ohne `else` Teil erreichbar, wenn die `if` -Anweisung erreichbar ist und der boolesche Ausdruck nicht über den konstanten Wert `true`verfügt.</span><span class="sxs-lookup"><span data-stu-id="acac3-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="acac3-256">Die Switch-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-256">The switch statement</span></span>

<span data-ttu-id="acac3-257">Die Switch-Anweisung wählt für die Ausführung eine Anweisungs Liste mit einer zugeordneten Switch-Bezeichnung aus, die dem Wert des switch-Ausdrucks entspricht.</span><span class="sxs-lookup"><span data-stu-id="acac3-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

<span data-ttu-id="acac3-258">Ein *switch_statement* besteht aus dem Schlüsselwort `switch`, gefolgt von einem Ausdruck in Klammern (der als Switch-Ausdruck bezeichnet wird), gefolgt von einem *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="acac3-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="acac3-259">Der *switch_block* besteht aus null oder mehr *switch_section*s, der in geschweiften Klammern eingeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="acac3-260">Jede *switch_section* besteht aus mindestens einer *switch_label*s, gefolgt von einer *statement_list* ([Anweisungs Liste](statements.md#statement-lists)).</span><span class="sxs-lookup"><span data-stu-id="acac3-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="acac3-261">Der ***governancetyp*** einer `switch` Anweisung wird durch den Switch-Ausdruck festgelegt.</span><span class="sxs-lookup"><span data-stu-id="acac3-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="acac3-262">Wenn der Switch-Ausdruck `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, 0 oder ein *enum_type*, oder wenn es sich um den Typ handelt, der NULL-Werte zulässt, der einem dieser Typen entspricht. , dann ist dies der regierende Typ der 2-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="acac3-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="acac3-263">Andernfalls muss genau eine benutzerdefinierte implizite Konvertierung ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)) `sbyte`vom Typ des switch-Ausdrucks bis zu einem der folgenden möglichen governancetypen vorhanden sein:, `byte`,, `ushort` `short` , `int`, `uint`, ,`long` ,`char`,oder, ein Typ, der NULL-Werte zulässt, der einem dieser Typen entspricht. `ulong` `string`</span><span class="sxs-lookup"><span data-stu-id="acac3-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="acac3-264">Andernfalls tritt ein Kompilierzeitfehler auf, wenn keine solche implizite Konvertierung vorhanden ist oder wenn mehr als eine implizite Konvertierung vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="acac3-265">Der Konstante Ausdruck jeder `case` Bezeichnung muss einen Wert angeben, der implizit konvertierbar ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den richtentyp `switch` der Anweisung ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="acac3-266">Ein Kompilierzeitfehler tritt auf, wenn zwei `case` oder mehr Bezeichnungen in `switch` derselben Anweisung denselben Konstanten Wert angeben.</span><span class="sxs-lookup"><span data-stu-id="acac3-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="acac3-267">In einer Switch-Anweisung darf `default` höchstens eine Bezeichnung vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="acac3-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="acac3-268">Eine `switch` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="acac3-269">Der Switch-Ausdruck wird ausgewertet und in den regierenden Typ konvertiert.</span><span class="sxs-lookup"><span data-stu-id="acac3-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="acac3-270">Wenn eine der Konstanten, die in einer `case` Bezeichnung in derselben `switch` Anweisung angegeben ist, gleich dem Wert des switch-Ausdrucks ist, wird die Steuerung an die Anweisungs Liste nach `case` der passenden Bezeichnung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="acac3-271">Wenn keine der Konstanten, die in `case` Bezeichnungen in derselben `switch` Anweisung angegeben sind, gleich dem Wert des switch-Ausdrucks ist und eine `default` Bezeichnung vorhanden ist, wird die Steuerung an die Anweisungs Liste übertragen `default` , die nach dem ETI.</span><span class="sxs-lookup"><span data-stu-id="acac3-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="acac3-272">Wenn keine der Konstanten, die in `case` Bezeichnungen in derselben `switch` Anweisung angegeben sind, gleich dem Wert des switch-Ausdrucks ist, und wenn `default` keine Bezeichnung vorhanden ist, `switch` wird die Steuerung an den Endpunkt der Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="acac3-273">Wenn der Endpunkt der Anweisungs Liste eines Switch-Abschnitts erreichbar ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="acac3-274">Dies wird als "No FallThrough"-Regel bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="acac3-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="acac3-275">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="acac3-275">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
<span data-ttu-id="acac3-276">ist gültig, da kein Switch-Abschnitt über einen erreichbaren Endpunkt verfügt.</span><span class="sxs-lookup"><span data-stu-id="acac3-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="acac3-277">Anders als bei C++C und ist die Ausführung eines Switch-Abschnitts nicht zulässig, um zum nächsten switchabschnitt zu wechseln, und das Beispiel</span><span class="sxs-lookup"><span data-stu-id="acac3-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
<span data-ttu-id="acac3-278">führt zu einem Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="acac3-278">results in a compile-time error.</span></span> <span data-ttu-id="acac3-279">Wenn die Ausführung eines Switch-Abschnitts durch die Ausführung eines anderen Switch-Abschnitts befolgt werden soll, `goto case` muss `goto default` eine explizite-oder-Anweisung verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="acac3-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

<span data-ttu-id="acac3-280">Mehrere Bezeichnungen sind in einem *switch_section*zulässig.</span><span class="sxs-lookup"><span data-stu-id="acac3-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="acac3-281">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="acac3-281">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
<span data-ttu-id="acac3-282">ist gültig.</span><span class="sxs-lookup"><span data-stu-id="acac3-282">is valid.</span></span> <span data-ttu-id="acac3-283">Das Beispiel verstößt nicht gegen die Regel "No FallThrough", da die Bezeichnungen `case 2:` und `default:` Teil desselben *switch_section*sind.</span><span class="sxs-lookup"><span data-stu-id="acac3-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="acac3-284">Die "No FallThrough"-Regel verhindert eine gängige Klasse von Fehlern, die in C C++ auftreten `break` , und wenn Anweisungen versehentlich ausgelassen werden.</span><span class="sxs-lookup"><span data-stu-id="acac3-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="acac3-285">Außerdem können die Switch-Abschnitte einer `switch` Anweisung aufgrund dieser Regel beliebig neu angeordnet werden, ohne dass sich dies auf das Verhalten der Anweisung auswirkt.</span><span class="sxs-lookup"><span data-stu-id="acac3-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="acac3-286">Beispielsweise können die Abschnitte der `switch` obigen Anweisung umgekehrt werden, ohne dass sich dies auf das Verhalten der-Anweisung auswirkt:</span><span class="sxs-lookup"><span data-stu-id="acac3-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

<span data-ttu-id="acac3-287">Die Anweisungs Liste eines Switch-Abschnitts endet in der `break`Regel `goto case`mit einer `goto default` -,-oder-Anweisung, aber alle Konstrukte, die den Endpunkt der Anweisungs Liste nicht erreichbar rendern, sind zulässig.</span><span class="sxs-lookup"><span data-stu-id="acac3-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="acac3-288">Beispielsweise ist es `while` bekannt, dass eine vom booleschen `true` Ausdruck gesteuerte Anweisung den Endpunkt niemals erreicht.</span><span class="sxs-lookup"><span data-stu-id="acac3-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="acac3-289">Ebenso überträgt eine `throw` - `return` oder-Anweisung immer an eine andere Stelle und erreicht ihren Endpunkt nicht.</span><span class="sxs-lookup"><span data-stu-id="acac3-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="acac3-290">Daher ist das folgende Beispiel gültig:</span><span class="sxs-lookup"><span data-stu-id="acac3-290">Thus, the following example is valid:</span></span>
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

<span data-ttu-id="acac3-291">Der governancetyp einer `switch` Anweisung kann der Typ `string`sein.</span><span class="sxs-lookup"><span data-stu-id="acac3-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="acac3-292">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="acac3-292">For example:</span></span>
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

<span data-ttu-id="acac3-293">Wie bei Zeichen folgen Gleichheits Operatoren ([Zeichen folgen Gleichheits Operatoren](expressions.md#string-equality-operators)) wird bei der Anweisung die `switch` Groß-/Kleinschreibung beachtet, und es wird nur ein bestimmter Schalter Abschnitt ausgeführt, wenn die `case` Zeichenfolge des switch-Ausdrucks</span><span class="sxs-lookup"><span data-stu-id="acac3-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="acac3-294">Wenn der governancetyp einer `switch` -Anweisung `string`ist, ist `null` der Wert als Konstante für die Case-Bezeichnung zulässig.</span><span class="sxs-lookup"><span data-stu-id="acac3-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="acac3-295">Die *statement_list*s eines *switch_block* können Deklarations Anweisungen ([Deklarations Anweisungen](statements.md#declaration-statements)) enthalten.</span><span class="sxs-lookup"><span data-stu-id="acac3-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="acac3-296">Der Gültigkeitsbereich einer lokalen Variablen oder Konstanten, die in einem Switch-Block deklariert ist, ist der Switch-Block.</span><span class="sxs-lookup"><span data-stu-id="acac3-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="acac3-297">Die Anweisungs Liste eines bestimmten switchabschnitts ist erreichbar, wenn `switch` die-Anweisung erreichbar ist und mindestens einer der folgenden Punkte zutrifft:</span><span class="sxs-lookup"><span data-stu-id="acac3-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="acac3-298">Der Switch-Ausdruck ist ein nicht konstanter Wert.</span><span class="sxs-lookup"><span data-stu-id="acac3-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="acac3-299">Der Switch-Ausdruck ist ein konstanter Wert, der `case` mit einer Bezeichnung im Switch-Abschnitt übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="acac3-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="acac3-300">Der Switch-Ausdruck ist ein konstanter Wert, der keiner `case` Bezeichnung entspricht, und der Switch-Abschnitt `default` enthält die Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="acac3-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="acac3-301">Auf eine Switch-Bezeichnung des Switch-Abschnitts wird durch eine `goto case` erreichbare-oder `goto default` -Anweisung verwiesen.</span><span class="sxs-lookup"><span data-stu-id="acac3-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="acac3-302">Der Endpunkt einer `switch` -Anweisung ist erreichbar, wenn mindestens einer der folgenden Punkte zutrifft:</span><span class="sxs-lookup"><span data-stu-id="acac3-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="acac3-303">Die `switch` -Anweisung enthält eine `break` erreichbare Anweisung, `switch` die die-Anweisung beendet.</span><span class="sxs-lookup"><span data-stu-id="acac3-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="acac3-304">Die `switch` -Anweisung ist erreichbar, der Switch-Ausdruck ist ein nicht konstanter Wert, und `default` es ist keine Bezeichnung vorhanden.</span><span class="sxs-lookup"><span data-stu-id="acac3-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="acac3-305">Die `switch` -Anweisung ist erreichbar, der Switch-Ausdruck ist ein konstanter Wert, der `case` keiner Bezeichnung entspricht, `default` und es ist keine Bezeichnung vorhanden.</span><span class="sxs-lookup"><span data-stu-id="acac3-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="acac3-306">Iterationsanweisungen</span><span class="sxs-lookup"><span data-stu-id="acac3-306">Iteration statements</span></span>

<span data-ttu-id="acac3-307">Iterations Anweisungen führen eine eingebettete Anweisung wiederholt aus.</span><span class="sxs-lookup"><span data-stu-id="acac3-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="acac3-308">Die while-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-308">The while statement</span></span>

<span data-ttu-id="acac3-309">Die `while` -Anweisung führt eine eingebettete Anweisung bedingt NULL oder mehrmals aus.</span><span class="sxs-lookup"><span data-stu-id="acac3-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="acac3-310">Eine `while` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="acac3-311">Die *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)) werden ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="acac3-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="acac3-312">Wenn der boolesche Ausdruck ergibt `true`, wird die Steuerung an die eingebettete Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="acac3-313">Wenn und wenn das Steuerelement den Endpunkt der eingebetteten Anweisung erreicht (möglicherweise aus der Ausführung `continue` einer-Anweisung), wird die Steuerung an den Anfang `while` der Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="acac3-314">Wenn der boolesche Ausdruck ergibt `false`, wird die Steuerung an den Endpunkt `while` der Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="acac3-315">Innerhalb der eingebetteten Anweisung `while` einer-Anweisung kann eine `break` -Anweisung ([die Break-Anweisung](statements.md#the-break-statement)) verwendet werden, um die `while` Steuerung an den Endpunkt der-Anweisung zu übertragen (wodurch die Iterationen der eingebetteten- Anweisungbeendetwerden).`continue` die-Anweisung ([die Continue-Anweisung](statements.md#the-continue-statement)) kann verwendet werden, um die Steuerung an den Endpunkt der eingebetteten Anweisung zu übertragen (wodurch eine `while` andere Iterations Anweisung ausgeführt wird).</span><span class="sxs-lookup"><span data-stu-id="acac3-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="acac3-316">Die eingebettete Anweisung einer `while` -Anweisung ist erreichbar, `while` wenn die-Anweisung erreichbar ist und der boolesche Ausdruck nicht über den `false`Konstanten Wert verfügt.</span><span class="sxs-lookup"><span data-stu-id="acac3-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="acac3-317">Der Endpunkt einer `while` -Anweisung ist erreichbar, wenn mindestens einer der folgenden Punkte zutrifft:</span><span class="sxs-lookup"><span data-stu-id="acac3-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="acac3-318">Die `while` -Anweisung enthält eine `break` erreichbare Anweisung, `while` die die-Anweisung beendet.</span><span class="sxs-lookup"><span data-stu-id="acac3-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="acac3-319">Die `while` -Anweisung ist erreichbar, und der boolesche Ausdruck weist nicht den Konstanten `true`Wert auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="acac3-320">Die Do-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-320">The do statement</span></span>

<span data-ttu-id="acac3-321">Die `do` Anweisung führt mindestens einmal eine eingebettete Anweisung aus.</span><span class="sxs-lookup"><span data-stu-id="acac3-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="acac3-322">Eine `do` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="acac3-323">Das Steuerelement wird an die eingebettete Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="acac3-324">Wenn und wenn das Steuerelement den Endpunkt der eingebetteten Anweisung erreicht (möglicherweise aus der Ausführung einer `continue`-Anweisung), wird die *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)) ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="acac3-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="acac3-325">Wenn der boolesche Ausdruck ergibt `true`, wird die Steuerung an den Anfang `do` der Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="acac3-326">Andernfalls wird die Steuerung an den Endpunkt `do` der Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="acac3-327">Innerhalb der eingebetteten Anweisung `do` einer-Anweisung kann eine `break` -Anweisung ([die Break-Anweisung](statements.md#the-break-statement)) verwendet werden, um die `do` Steuerung an den Endpunkt der-Anweisung zu übertragen (wodurch die Iterationen der eingebetteten- Anweisungbeendetwerden).`continue` die-Anweisung ([die Continue-Anweisung](statements.md#the-continue-statement)) kann verwendet werden, um die Steuerung an den Endpunkt der eingebetteten Anweisung zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="acac3-328">Die eingebettete Anweisung einer `do` -Anweisung ist erreichbar, `do` wenn die-Anweisung erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="acac3-329">Der Endpunkt einer `do` -Anweisung ist erreichbar, wenn mindestens einer der folgenden Punkte zutrifft:</span><span class="sxs-lookup"><span data-stu-id="acac3-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="acac3-330">Die `do` -Anweisung enthält eine `break` erreichbare Anweisung, `do` die die-Anweisung beendet.</span><span class="sxs-lookup"><span data-stu-id="acac3-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="acac3-331">Der Endpunkt der eingebetteten Anweisung ist erreichbar, und der boolesche Ausdruck weist nicht den konstanten Wert `true`auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="acac3-332">Die for-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-332">The for statement</span></span>

<span data-ttu-id="acac3-333">Die `for` -Anweisung wertet eine Sequenz von Initialisierungs Ausdrücken aus und führt dann, während eine Bedingung true ist, wiederholt eine eingebettete Anweisung aus und wertet eine Sequenz von Iterations Ausdrücken aus.</span><span class="sxs-lookup"><span data-stu-id="acac3-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

<span data-ttu-id="acac3-334">*For_initializer*, falls vorhanden, besteht entweder aus einer *local_variable_declaration* ([lokale Variablen Deklarationen](statements.md#local-variable-declarations)) oder einer Liste von *statement_expression*s ([Ausdrucks Anweisungen](statements.md#expression-statements)), die durch Kommas getrennt sind.</span><span class="sxs-lookup"><span data-stu-id="acac3-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="acac3-335">Der Gültigkeitsbereich einer lokalen Variablen, die von einem *for_initializer* deklariert wird, beginnt bei *local_variable_declarator* für die Variable und reicht bis zum Ende der eingebetteten Anweisung aus.</span><span class="sxs-lookup"><span data-stu-id="acac3-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="acac3-336">Der Bereich umfasst *for_condition* und *for_iterator*.</span><span class="sxs-lookup"><span data-stu-id="acac3-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="acac3-337">Der *for_condition*muss, falls vorhanden, ein *Boolean_expression* ([boolescher Ausdruck](expressions.md#boolean-expressions)) sein.</span><span class="sxs-lookup"><span data-stu-id="acac3-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="acac3-338">Der *for_iterator*besteht, falls vorhanden, aus einer Liste von *statement_expression*s ([Ausdrucks Anweisungen](statements.md#expression-statements)), die durch Kommas getrennt sind.</span><span class="sxs-lookup"><span data-stu-id="acac3-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="acac3-339">Eine for-Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="acac3-340">Wenn ein *for_initializer* vorhanden ist, werden die Variableninitialisierer oder Anweisungs Ausdrücke in der Reihenfolge ausgeführt, in der Sie geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="acac3-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="acac3-341">Dieser Schritt wird nur einmal ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-341">This step is only performed once.</span></span>
*  <span data-ttu-id="acac3-342">Wenn ein *for_condition* vorhanden ist, wird es ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="acac3-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="acac3-343">Wenn *for_condition* nicht vorhanden ist oder die Auswertung `true` ergibt, wird die Steuerung an die eingebettete Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="acac3-344">Wenn und wenn das Steuerelement den Endpunkt der eingebetteten Anweisung erreicht (möglicherweise aus der Ausführung einer `continue`-Anweisung), werden die Ausdrücke des *for_iterator*, sofern vorhanden, nacheinander ausgewertet. Anschließend wird eine weitere Iterationen ausgeführt, beginnend mit Auswertung des *for_condition* im obigen Schritt.</span><span class="sxs-lookup"><span data-stu-id="acac3-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="acac3-345">Wenn *for_condition* vorhanden ist und die Auswertung `false` ergibt, wird die Steuerung an den Endpunkt der `for`-Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="acac3-346">Innerhalb der eingebetteten Anweisung einer `for`-Anweisung kann eine `break`-Anweisung ([die Break-Anweisung](statements.md#the-break-statement)) verwendet werden, um die Steuerung an den Endpunkt der `for`-Anweisung (d. h. die Iterations Ende der eingebetteten Anweisung) und eine `continue`-Anweisung ([ Die Continue-Anweisung](statements.md#the-continue-statement)) kann verwendet werden, um die Steuerung an den Endpunkt der eingebetteten Anweisung zu übertragen (d. h., der *for_iterator* wird ausgeführt, und es wird eine weitere Iterations Anweisung der `for`-Anweisung ausgeführt, beginnend mit dem *for_condition*).</span><span class="sxs-lookup"><span data-stu-id="acac3-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="acac3-347">Die eingebettete Anweisung einer `for` -Anweisung ist erreichbar, wenn eine der folgenden Aussagen zutrifft:</span><span class="sxs-lookup"><span data-stu-id="acac3-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="acac3-348">Die `for`-Anweisung ist erreichbar, und es ist keine *for_condition* vorhanden.</span><span class="sxs-lookup"><span data-stu-id="acac3-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="acac3-349">Die `for`-Anweisung ist erreichbar, und ein *for_condition* ist vorhanden und weist nicht den konstanten Wert `false` auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="acac3-350">Der Endpunkt einer `for` -Anweisung ist erreichbar, wenn mindestens einer der folgenden Punkte zutrifft:</span><span class="sxs-lookup"><span data-stu-id="acac3-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="acac3-351">Die `for` -Anweisung enthält eine `break` erreichbare Anweisung, `for` die die-Anweisung beendet.</span><span class="sxs-lookup"><span data-stu-id="acac3-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="acac3-352">Die `for`-Anweisung ist erreichbar, und ein *for_condition* ist vorhanden und weist nicht den konstanten Wert `true` auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="acac3-353">Die foreach-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-353">The foreach statement</span></span>

<span data-ttu-id="acac3-354">Die `foreach` -Anweisung zählt die Elemente einer Auflistung auf und führt eine eingebettete Anweisung für jedes Element der Auflistung aus.</span><span class="sxs-lookup"><span data-stu-id="acac3-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="acac3-355">Der *Typ* und der *Bezeichner* einer `foreach` -Anweisung deklarieren die ***iterations Variable*** der-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="acac3-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="acac3-356">Wenn der Bezeichner "`var`" als *local_variable_type*angegeben wird und kein Typ mit dem Namen `var` im Gültigkeitsbereich ist, wird die Iterations Variable als ***implizit typisierte Iterations Variable***bezeichnet, und ihr Typ wird als Elementtyp des `foreach` verwendet. -Anweisung, wie unten angegeben.</span><span class="sxs-lookup"><span data-stu-id="acac3-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="acac3-357">Die Iterations Variable entspricht einer schreibgeschützten lokalen Variablen mit einem Bereich, der die eingebettete Anweisung erweitert.</span><span class="sxs-lookup"><span data-stu-id="acac3-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="acac3-358">Während der Ausführung einer `foreach` -Anweisung stellt die Iterations Variable das Auflistungs Element dar, für das zurzeit eine Iterations Ausführung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="acac3-359">Ein Kompilierzeitfehler tritt auf, wenn die eingebettete Anweisung versucht, die Iterations Variable (über die `++` - `--` und-Operatoren) zu ändern oder die `ref` Iterations `out` Variable als-oder-Parameter zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="acac3-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="acac3-360">Im folgenden wird `IEnumerable`aus Gründen `IEnumerator` `System.Collections` `IEnumerable<T>` der Übersichtlichkeit,, `IEnumerator<T>` und auf die entsprechenden Typen in den-Namespaces `System.Collections.Generic`und verwiesen.</span><span class="sxs-lookup"><span data-stu-id="acac3-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="acac3-361">Die Kompilierzeit Verarbeitung einer foreach-Anweisung bestimmt zuerst den ***Auflistungstyp***, den ***Enumeratortyp*** und den ***Elementtyp*** des Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="acac3-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="acac3-362">Diese Bestimmung verläuft wie folgt:</span><span class="sxs-lookup"><span data-stu-id="acac3-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="acac3-363">Wenn der Typ `X` des *Ausdrucks* ein Arraytyp ist, gibt es eine implizite Verweis Konvertierung `X` von in `IEnumerable` die-Schnitt `System.Array` Stelle (da diese Schnittstelle implementiert).</span><span class="sxs-lookup"><span data-stu-id="acac3-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="acac3-364">Der ***Sammlungstyp*** ist `IEnumerable` die-Schnittstelle, der ***Enumeratortyp*** ist die `IEnumerator` -Schnittstelle, und der ***Elementtyp*** ist der `X`Elementtyp des Arraytyps.</span><span class="sxs-lookup"><span data-stu-id="acac3-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="acac3-365">Wenn der Typ `X` des *Ausdrucks* ist `dynamic` , gibt es eine implizite Konvertierung von einem *Ausdruck* in `IEnumerable` die-Schnittstelle ([implizite dynamische Konvertierungen](conversions.md#implicit-dynamic-conversions)).</span><span class="sxs-lookup"><span data-stu-id="acac3-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="acac3-366">Der ***Sammlungstyp*** ist `IEnumerable` die-Schnittstelle, und der ***Enumeratortyp*** ist die `IEnumerator` -Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="acac3-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="acac3-367">Wenn der Bezeichner "`var`" als *local_variable_type* angegeben wird, ist der ***Elementtyp*** `dynamic`; andernfalls ist er `object`.</span><span class="sxs-lookup"><span data-stu-id="acac3-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="acac3-368">Stellen Sie andernfalls fest, ob `X` der Typ über `GetEnumerator` eine geeignete Methode verfügt:</span><span class="sxs-lookup"><span data-stu-id="acac3-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="acac3-369">Führt eine Element Suche für den Typ `X` mit Bezeichnern `GetEnumerator` und ohne Typargumente durch.</span><span class="sxs-lookup"><span data-stu-id="acac3-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="acac3-370">Wenn die Element Suche keine Entsprechung erzeugt oder eine Mehrdeutigkeit erzeugt oder eine Entsprechung erzeugt, die keine Methoden Gruppe ist, überprüfen Sie wie unten beschrieben, ob eine Aufzähl Bare-Schnittstelle vorliegt.</span><span class="sxs-lookup"><span data-stu-id="acac3-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="acac3-371">Es wird empfohlen, dass eine Warnung ausgegeben wird, wenn die Element Suche nur eine Methoden Gruppe oder keine Entsprechung erzeugt.</span><span class="sxs-lookup"><span data-stu-id="acac3-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="acac3-372">Führt die Überladungs Auflösung mithilfe der resultierenden Methoden Gruppe und einer leeren Argumentliste durch.</span><span class="sxs-lookup"><span data-stu-id="acac3-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="acac3-373">Wenn die Überladungs Auflösung keine anwendbaren Methoden zur Folge hat, zu einer Mehrdeutigkeit führt oder zu einer einzigen optimalen Methode führt, diese Methode jedoch entweder statisch oder nicht öffentlich ist, überprüfen Sie, wie unten beschrieben, eine Aufzähl Bare Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="acac3-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="acac3-374">Es wird empfohlen, dass eine Warnung ausgegeben wird, wenn die Überladungs Auflösung nur eine eindeutige öffentliche Instanzmethode oder keine anwendbaren Methoden erzeugt.</span><span class="sxs-lookup"><span data-stu-id="acac3-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="acac3-375">Wenn der `E` Rückgabetyp `GetEnumerator` der Methode keine Klasse, Struktur oder Schnittstellentyp ist, wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="acac3-376">Die Element Suche erfolgt `E` mit dem-Bezeichner `Current` und ohne Typargumente.</span><span class="sxs-lookup"><span data-stu-id="acac3-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="acac3-377">Wenn die Member-Suche keine Entsprechung erzeugt, ist das Ergebnis ein Fehler, oder es handelt sich um ein anderes Ergebnis als eine öffentliche Instanzeigenschaft, die Lesevorgänge zulässt, es wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="acac3-378">Die Element Suche erfolgt `E` mit dem-Bezeichner `MoveNext` und ohne Typargumente.</span><span class="sxs-lookup"><span data-stu-id="acac3-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="acac3-379">Wenn die Member-Suche keine Entsprechung erzeugt, ist das Ergebnis ein Fehler, oder es handelt sich um ein anderes Ergebnis als eine Methoden Gruppe. es wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="acac3-380">Die Überladungs Auflösung wird für die Methoden Gruppe mit einer leeren Argumentliste ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="acac3-381">Wenn die Überladungs Auflösung keine anwendbaren Methoden zur Folge hat, führt dies zu einer Mehrdeutigkeit, oder führt dies zu einer einzigen optimalen Methode, aber diese Methode ist entweder statisch oder nicht öffentlich `bool`, oder der Rückgabetyp ist nicht. es wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="acac3-382">Der ***Auflistungstyp*** ist `X`, der ***Enumeratortyp*** ist `E`, `Current` und der ***Elementtyp*** ist der Typ der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="acac3-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="acac3-383">Überprüfen Sie andernfalls eine Aufzähl Bare-Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="acac3-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="acac3-384">Wenn zwischen `Ti` allen Typen, für die eine implizite Konvertierung von `X` in `IEnumerable<Ti>`vorhanden ist, gibt es einen eindeutigen Typ `T` , der `T` nicht `dynamic` ist und für alle anderen `Ti` ein implizite Konvertierung von `IEnumerable<T>` in `IEnumerable<Ti>`, dann ist der Sammlungstyp die `IEnumerable<T>`-Schnittstelle, der ***Enumeratortyp*** ist die-Schnittstelle `IEnumerator<T>`, und der ***Elementtyp*** ist `T`.</span><span class="sxs-lookup"><span data-stu-id="acac3-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="acac3-385">Andernfalls wird ein Fehler erzeugt, wenn mehr als ein `T`solcher Typ vorhanden ist, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="acac3-386">Andernfalls `X` ist bei einer impliziten Konvertierung von in die `System.Collections.IEnumerable` -Schnittstelle der Sammlungstyp diese Schnittstelle, der ***Enumeratortyp*** ist die-Schnitt `System.Collections.IEnumerator`Stelle, und der ***Elementtyp*** ist. `object`.</span><span class="sxs-lookup"><span data-stu-id="acac3-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="acac3-387">Andernfalls wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="acac3-388">Die oben genannten Schritte, wenn erfolgreich, führen zu einer eindeutigen `C`Generierung eines Auflistungs `E` Typs, enumeratortyps und Elementtyps `T`.</span><span class="sxs-lookup"><span data-stu-id="acac3-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="acac3-389">Eine foreach-Anweisung des Formulars</span><span class="sxs-lookup"><span data-stu-id="acac3-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="acac3-390">wird dann auf erweitert:</span><span class="sxs-lookup"><span data-stu-id="acac3-390">is then expanded to:</span></span>
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

<span data-ttu-id="acac3-391">Die Variable `e` ist für den Ausdruck `x` oder die eingebettete Anweisung oder einen anderen Quellcode des Programms nicht sichtbar oder kann nicht darauf zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="acac3-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="acac3-392">Die Variable `v` ist in der eingebetteten Anweisung schreibgeschützt.</span><span class="sxs-lookup"><span data-stu-id="acac3-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="acac3-393">Wenn keine explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-conversions)) von `T` (dem Elementtyp) in `V` (der *local_variable_type* in der foreach-Anweisung) vorhanden ist, wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="acac3-394">Wenn `x` den Wert `null`hat, wird `System.NullReferenceException` zur Laufzeit eine ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="acac3-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="acac3-395">Eine Implementierung darf eine bestimmte foreach-Anweisung anders implementieren, z. b. aus Leistungsgründen, solange das Verhalten mit der obigen Erweiterung konsistent ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="acac3-396">Die Platzierung von `v` innerhalb der while-Schleife ist wichtig für die Erfassung durch eine anonyme Funktion, die in *embedded_statement*auftritt.</span><span class="sxs-lookup"><span data-stu-id="acac3-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="acac3-397">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="acac3-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="acac3-398">Wenn `v` außerhalb der while-Schleife deklariert wurde, wird Sie von allen Iterationen gemeinsam genutzt, und ihr Wert nach der for-Schleife wäre der endgültige `13`Wert,, der den Aufruf von `f` druckt.</span><span class="sxs-lookup"><span data-stu-id="acac3-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="acac3-399">Da jede Iterations Variable über eine eigene Variable `v`verfügt, hält die, die von `f` in der ersten Iterationen aufgezeichnet wird, den Wert `7`, der gedruckt wird, weiterhin.</span><span class="sxs-lookup"><span data-stu-id="acac3-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="acac3-400">(Hinweis: frühere Versionen von C# , `v` die außerhalb der while-Schleife deklariert wurden.)</span><span class="sxs-lookup"><span data-stu-id="acac3-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="acac3-401">Der Hauptteil des Blocks zum Schluss wird gemäß den folgenden Schritten erstellt:</span><span class="sxs-lookup"><span data-stu-id="acac3-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="acac3-402">Wenn eine implizite Konvertierung von `E` in die `System.IDisposable` -Schnittstelle erfolgt, dann</span><span class="sxs-lookup"><span data-stu-id="acac3-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="acac3-403">Wenn `E` ein Werttyp ist, der keine NULL-Werte zulässt, wird die schließlich-Klausel auf die semantische Entsprechung von erweitert:</span><span class="sxs-lookup"><span data-stu-id="acac3-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="acac3-404">Andernfalls wird die schließlich-Klausel auf die semantische Entsprechung von erweitert:</span><span class="sxs-lookup"><span data-stu-id="acac3-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="acac3-405">mit der Ausnahme `E` , dass bei einem Werttyp oder einem Typparameter, der auf einen Werttyp instanziiert `e` wird `System.IDisposable` , die Umwandlung von in nicht dazu führt, dass Boxing auftritt.</span><span class="sxs-lookup"><span data-stu-id="acac3-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="acac3-406">Andernfalls, wenn `E` ein versiegelter Typ ist, wird die letzte-Klausel auf einen leeren Block erweitert:</span><span class="sxs-lookup"><span data-stu-id="acac3-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="acac3-407">Andernfalls wird die schließlich-Klausel auf erweitert:</span><span class="sxs-lookup"><span data-stu-id="acac3-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="acac3-408">Die lokale Variable `d` ist für keinen Benutzercode sichtbar oder nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="acac3-409">Dies hat insbesondere keinen Konflikt mit einer anderen Variablen, deren Bereich den letzten Block enthält.</span><span class="sxs-lookup"><span data-stu-id="acac3-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="acac3-410">Die Reihenfolge, `foreach` in der die Elemente eines Arrays durchlaufen werden, lautet wie folgt: Für eindimensionale Arrays werden Elemente in steigender Index Reihenfolge durchlaufen, beginnend mit `0` Index und endende `Length - 1`mit Index.</span><span class="sxs-lookup"><span data-stu-id="acac3-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="acac3-411">Bei mehrdimensionalen Arrays werden Elemente so durchlaufen, dass die Indizes der äußersten rechten Dimension zuerst angehoben werden, dann die nächste linke Dimension usw.</span><span class="sxs-lookup"><span data-stu-id="acac3-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="acac3-412">Im folgenden Beispiel werden die einzelnen Werte in der Reihenfolge der Elemente in einem zweidimensionalen Array ausgegeben:</span><span class="sxs-lookup"><span data-stu-id="acac3-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
<span data-ttu-id="acac3-413">Die erstellte Ausgabe lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="acac3-413">The output produced is as follows:</span></span>
```console
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="acac3-414">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="acac3-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="acac3-415">der Typ von `n` wird als `int`, der Elementtyp von `numbers`, abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="acac3-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="acac3-416">Sprunganweisungen</span><span class="sxs-lookup"><span data-stu-id="acac3-416">Jump statements</span></span>

<span data-ttu-id="acac3-417">Jump-Anweisungen: Bedingungs Übertragung der Steuerung.</span><span class="sxs-lookup"><span data-stu-id="acac3-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="acac3-418">Der Speicherort, an den eine Jump-Anweisung die Steuerung überträgt, wird als ***Ziel*** der Jump-Anweisung bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="acac3-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="acac3-419">Wenn eine Jump-Anweisung innerhalb eines-Blocks auftritt und sich das Ziel dieser Jump-Anweisung außerhalb dieses Blocks befindet, wird die Jump-Anweisung zum ***Beenden*** des Blocks bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="acac3-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="acac3-420">Während eine Jump-Anweisung die Steuerung von einem-Block übertragen kann, kann Sie die Steuerung niemals in einen-Block übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="acac3-421">Die Ausführung von Jump-Anweisungen wird durch das vorhanden sein `try` von dazwischenliegenden Anweisungen erschwert.</span><span class="sxs-lookup"><span data-stu-id="acac3-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="acac3-422">Wenn solche `try` Anweisungen fehlen, überträgt eine Jump-Anweisung bedingungslos die Steuerung von der Jump-Anweisung an das Ziel.</span><span class="sxs-lookup"><span data-stu-id="acac3-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="acac3-423">Wenn diese dazwischenliegenden `try` Anweisungen vorhanden sind, ist die Ausführung komplexer.</span><span class="sxs-lookup"><span data-stu-id="acac3-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="acac3-424">Wenn die Jump-Anweisung einen oder mehrere `try` Blöcke mit zugeordneten `finally` Blöcken beendet, wird die Steuerung anfänglich an den `finally` Block der `try` innersten Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="acac3-425">Wenn und wenn das Steuerelement den Endpunkt eines `finally` -Blocks erreicht, wird die Steuerung an den `finally` -Block der nächsten einschließenden `try` Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="acac3-426">Dieser Vorgang wird wiederholt, `finally` bis die Blöcke aller `try` dazwischen liegenden Anweisungen ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="acac3-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="acac3-427">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="acac3-427">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
<span data-ttu-id="acac3-428">die `finally` Blöcke, die zwei `try` Anweisungen zugeordnet sind, werden ausgeführt, bevor die Steuerung an das Ziel der Jump-Anweisung übertragen wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="acac3-429">Die erstellte Ausgabe lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="acac3-429">The output produced is as follows:</span></span>
```console
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="acac3-430">Die Break-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-430">The break statement</span></span>

<span data-ttu-id="acac3-431">Die `break` -Anweisung beendet die nächste `while`einschließende `do` `switch` `for`-,- `foreach` ,-,-oder-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="acac3-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="acac3-432">Das `break` Ziel einer-Anweisung ist der Endpunkt der nächsten einschließenden `switch`, `while`, `do` `for`, oder `foreach` -Anweisung.</span><span class="sxs-lookup"><span data-stu-id="acac3-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="acac3-433">Wenn eine `break` -Anweisung nicht durch eine `switch`-, `while` `do` `for`-,-, `foreach` -oder-Anweisung eingeschlossen ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="acac3-434">Wenn mehrere `switch`- `while` `foreach` `break` , `do`-,-,-oder-Anweisungen ineinander geschachtelt sind, gilt eine-Anweisung nur für die innerste-Anweisung. `for`</span><span class="sxs-lookup"><span data-stu-id="acac3-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="acac3-435">Um die Steuerung auf mehrere Schachtelungs Ebenen `goto` zu übertragen, muss eine-Anweisung ([die GoTo-Anweisung](statements.md#the-goto-statement)) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="acac3-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="acac3-436">Eine `break` -Anweisung kann einen `finally` Block nicht beenden ([try-Anweisung](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="acac3-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="acac3-437">Wenn eine `break` -Anweisung innerhalb eines `finally` -Blocks auftritt `break` , muss sich das Ziel der-Anweisung innerhalb `finally` desselben-Blocks befinden; andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="acac3-438">Eine `break` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="acac3-439">Wenn die `break` Anweisung einen oder mehrere `try` Blöcke mit zugeordneten `finally` Blöcken beendet, wird die Steuerung anfänglich `finally` an den Block der innersten `try` Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="acac3-440">Wenn und wenn das Steuerelement den Endpunkt eines `finally` -Blocks erreicht, wird die Steuerung an den `finally` -Block der nächsten einschließenden `try` Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="acac3-441">Dieser Vorgang wird wiederholt, `finally` bis die Blöcke aller `try` dazwischen liegenden Anweisungen ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="acac3-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="acac3-442">Das Steuerelement wird an das Ziel der `break` -Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="acac3-443">Da eine `break` Anweisung die Steuerung an andere Stellen überträgt, ist der Endpunkt `break` einer Anweisung nie erreichbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="acac3-444">Die Continue-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-444">The continue statement</span></span>

<span data-ttu-id="acac3-445">Mit `continue` der-Anweisung wird eine neue Iterations Anweisung der nächsten `while` `do`einschließenden `for`- `foreach` ,-,-oder-Anweisung gestartet.</span><span class="sxs-lookup"><span data-stu-id="acac3-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="acac3-446">Das `continue` Ziel einer-Anweisung ist der Endpunkt der eingebetteten Anweisung der nächsten `do`einschließenden `while` `for`-,-,-oder `foreach` -Anweisung.</span><span class="sxs-lookup"><span data-stu-id="acac3-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="acac3-447">Wenn eine `continue` -Anweisung nicht durch eine `while`-, `do` `for`-,- `foreach` oder-Anweisung eingeschlossen ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="acac3-448">Wenn mehrere `while`- `do` `foreach` ,-,-oder-Anweisungen ineinander geschachtelt sind `continue` , gilt eine-Anweisung nur für die innerste-Anweisung. `for`</span><span class="sxs-lookup"><span data-stu-id="acac3-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="acac3-449">Um die Steuerung auf mehrere Schachtelungs Ebenen `goto` zu übertragen, muss eine-Anweisung ([die GoTo-Anweisung](statements.md#the-goto-statement)) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="acac3-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="acac3-450">Eine `continue` -Anweisung kann einen `finally` Block nicht beenden ([try-Anweisung](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="acac3-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="acac3-451">Wenn eine `continue` -Anweisung innerhalb eines `finally` -Blocks auftritt `continue` , muss sich das Ziel der-Anweisung innerhalb `finally` desselben-Blocks befinden; andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="acac3-452">Eine `continue` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="acac3-453">Wenn die `continue` Anweisung einen oder mehrere `try` Blöcke mit zugeordneten `finally` Blöcken beendet, wird die Steuerung anfänglich `finally` an den Block der innersten `try` Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="acac3-454">Wenn und wenn das Steuerelement den Endpunkt eines `finally` -Blocks erreicht, wird die Steuerung an den `finally` -Block der nächsten einschließenden `try` Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="acac3-455">Dieser Vorgang wird wiederholt, `finally` bis die Blöcke aller `try` dazwischen liegenden Anweisungen ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="acac3-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="acac3-456">Das Steuerelement wird an das Ziel der `continue` -Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="acac3-457">Da eine `continue` Anweisung die Steuerung an andere Stellen überträgt, ist der Endpunkt `continue` einer Anweisung nie erreichbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="acac3-458">Die goto-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-458">The goto statement</span></span>

<span data-ttu-id="acac3-459">Mit `goto` der-Anweisung wird die Steuerung an eine Anweisung übertragen, die durch eine Bezeichnung gekennzeichnet ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="acac3-460">Das Ziel einer `goto` *bezeichneranweisung* ist die bezeichnete Anweisung mit der angegebenen Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="acac3-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="acac3-461">Wenn eine Bezeichnung mit dem angegebenen Namen nicht im aktuellen Funktionsmember vorhanden ist oder die `goto` Anweisung nicht innerhalb des Bereichs der Bezeichnung liegt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="acac3-462">Diese Regel ermöglicht die Verwendung einer `goto` -Anweisung, um die Steuerung aus einem nicht in einen Bereich eingefügten Bereich zu übertragen, jedoch nicht in einen eingefügten Bereich.</span><span class="sxs-lookup"><span data-stu-id="acac3-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="acac3-463">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="acac3-463">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
<span data-ttu-id="acac3-464">eine `goto` -Anweisung wird verwendet, um die Steuerung aus einem in einem Bereich befindlichen Bereich zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="acac3-465">Das Ziel `goto case` einer-Anweisung ist die Anweisungs Liste in der unmittelbar `switch` einschließenden Anweisung ([Switch-Anweisung](statements.md#the-switch-statement)), die `case` eine Bezeichnung mit dem angegebenen konstanten Wert enthält.</span><span class="sxs-lookup"><span data-stu-id="acac3-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="acac3-466">, Wenn die `goto case`-Anweisung nicht durch eine `switch`-Anweisung eingeschlossen ist,, wenn die *constant_expression* nicht implizit konvertierbar ist ([implizite Konvertierungen](conversions.md#implicit-conversions)), in den regierenden Typ der nächstgelegenen `switch`-Anweisung oder, wenn die nächste einschließende `switch`-Anweisung enthält keine `case`-Bezeichnung mit dem angegebenen konstanten Wert. ein Kompilierzeitfehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="acac3-467">Das Ziel `goto default` einer-Anweisung ist die Anweisungs Liste in der unmittelbar `switch` einschließenden Anweisung ([Switch-Anweisung](statements.md#the-switch-statement)), die `default` eine Bezeichnung enthält.</span><span class="sxs-lookup"><span data-stu-id="acac3-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="acac3-468">Wenn die `goto default` Anweisung nicht durch eine `switch` -Anweisung eingeschlossen ist, oder wenn die nächste einschließende `switch` Anweisung keine `default` Bezeichnung enthält, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="acac3-469">Eine `goto` -Anweisung kann einen `finally` Block nicht beenden ([try-Anweisung](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="acac3-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="acac3-470">Wenn eine `goto` -Anweisung innerhalb eines `finally` -Blocks auftritt `goto` , muss sich das Ziel der-Anweisung innerhalb `finally` desselben Blocks befinden, andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="acac3-471">Eine `goto` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="acac3-472">Wenn die `goto` Anweisung einen oder mehrere `try` Blöcke mit zugeordneten `finally` Blöcken beendet, wird die Steuerung anfänglich `finally` an den Block der innersten `try` Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="acac3-473">Wenn und wenn das Steuerelement den Endpunkt eines `finally` -Blocks erreicht, wird die Steuerung an den `finally` -Block der nächsten einschließenden `try` Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="acac3-474">Dieser Vorgang wird wiederholt, `finally` bis die Blöcke aller `try` dazwischen liegenden Anweisungen ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="acac3-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="acac3-475">Das Steuerelement wird an das Ziel der `goto` -Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="acac3-476">Da eine `goto` Anweisung die Steuerung an andere Stellen überträgt, ist der Endpunkt `goto` einer Anweisung nie erreichbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="acac3-477">Return-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-477">The return statement</span></span>

<span data-ttu-id="acac3-478">Die `return` -Anweisung gibt die Steuerung an den aktuellen Aufrufer der Funktion `return` zurück, in der die-Anweisung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="acac3-479">Eine `return` -Anweisung ohne Ausdruck kann nur in einem Funktionsmember verwendet werden, der keinen Wert berechnet, d. h. eine Methode mit dem Ergebnistyp ([Methoden Text](classes.md#method-body)) `void`, `set` der-Accessor einer Eigenschaft oder eines Indexers, der `add` - `remove` und-Accessoren eines Ereignisses, eines Instanzkonstruktors, eines statischen Konstruktors oder eines Dekonstruktors.</span><span class="sxs-lookup"><span data-stu-id="acac3-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="acac3-480">Eine `return` -Anweisung mit einem-Ausdruck kann nur in einem Funktionsmember verwendet werden, der einen-Wert berechnet, d. h. eine Methode mit einem nicht leeren `get` Ergebnistyp, den-Accessor einer Eigenschaft oder einen Indexer oder einen benutzerdefinierten Operator.</span><span class="sxs-lookup"><span data-stu-id="acac3-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="acac3-481">Eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) muss vom Typ des Ausdrucks bis zum Rückgabetyp des enthaltenden Funktionsmembers vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="acac3-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="acac3-482">Return-Anweisungen können auch im Text der anonymen Funktions Ausdrücke ([Anonyme Funktions](expressions.md#anonymous-function-expressions)Ausdrücke) verwendet werden und daran beteiligt sein, zu bestimmen, welche Konvertierungen für diese Funktionen vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="acac3-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="acac3-483">Es handelt sich um einen Kompilierzeitfehler `return` , damit eine-Anweisung `finally` in einem-Block ([der try-Anweisung](statements.md#the-try-statement)) angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="acac3-484">Eine `return` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="acac3-485">Wenn die `return` Anweisung einen Ausdruck angibt, wird der Ausdruck ausgewertet, und der resultierende Wert wird durch eine implizite Konvertierung in den Rückgabetyp der enthaltenden Funktion konvertiert.</span><span class="sxs-lookup"><span data-stu-id="acac3-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="acac3-486">Das Ergebnis der Konvertierung wird der Ergebniswert, der von der-Funktion erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="acac3-487">Wenn die `return` -Anweisung von einem oder mehreren `try` -oder `catch` -Blöcken mit `finally` zugeordneten-Blöcken eingeschlossen wird, wird `finally` die Steuerung anfänglich an `try` den-Block der innersten-Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="acac3-488">Wenn und wenn das Steuerelement den Endpunkt eines `finally` -Blocks erreicht, wird die Steuerung an den `finally` -Block der nächsten einschließenden `try` Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="acac3-489">Dieser Vorgang wird wiederholt, `finally` bis die Blöcke aller einschließenden `try` Anweisungen ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="acac3-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="acac3-490">Wenn die enthaltende Funktion keine Async-Funktion ist, wird die Steuerung an den Aufrufer der enthaltenden Funktion zusammen mit dem Ergebniswert zurückgegeben, falls vorhanden.</span><span class="sxs-lookup"><span data-stu-id="acac3-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="acac3-491">Wenn die enthaltende Funktion eine Async-Funktion ist, wird die Steuerung an den aktuellen Aufrufer zurückgegeben, und der Ergebniswert wird ggf. in der Rückgabe Aufgabe aufgezeichnet, wie in ([Enumeratorschnittstellen](classes.md#enumerator-interfaces)) beschrieben.</span><span class="sxs-lookup"><span data-stu-id="acac3-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="acac3-492">Da eine `return` Anweisung die Steuerung an andere Stellen überträgt, ist der Endpunkt `return` einer Anweisung nie erreichbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="acac3-493">Die throw-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-493">The throw statement</span></span>

<span data-ttu-id="acac3-494">Die `throw` -Anweisung löst eine Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="acac3-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="acac3-495">Eine `throw` -Anweisung mit einem Ausdruck löst den Wert aus, der durch Auswerten des Ausdrucks erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="acac3-496">Der Ausdruck muss einen Wert des Klassen Typs `System.Exception`, eines Klassen Typs, der von `System.Exception` abgeleitet ist `System.Exception` , oder von einem typparametertyp mit (oder einer Unterklasse) als effektive Basisklasse bezeichnen.</span><span class="sxs-lookup"><span data-stu-id="acac3-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="acac3-497">Wenn die Auswertung des Ausdrucks erzeugt `null`, wird `System.NullReferenceException` stattdessen eine ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="acac3-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="acac3-498">Eine `throw` -Anweisung ohne Ausdruck kann nur in einem `catch` -Block verwendet werden. in diesem Fall löst die-Anweisung die Ausnahme erneut aus, die zurzeit von `catch` diesem Block behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="acac3-499">Da eine `throw` Anweisung die Steuerung an andere Stellen überträgt, ist der Endpunkt `throw` einer Anweisung nie erreichbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="acac3-500">Wenn eine Ausnahme ausgelöst wird, wird die Steuerung an die erste `catch` Klausel in einer einschließenden `try` Anweisung übertragen, die die Ausnahme behandeln kann.</span><span class="sxs-lookup"><span data-stu-id="acac3-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="acac3-501">Der Prozess, der ab dem Zeitpunkt der Ausnahme ausgelöst wird, der zum Zeitpunkt der Übertragung der Steuerung an einen geeigneten Ausnahmehandler ausgelöst wird, wird als ***Ausnahme***Weitergabe bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="acac3-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="acac3-502">Die Weitergabe einer Ausnahme besteht aus dem wiederholten Auswerten der folgenden `catch` Schritte, bis eine Klausel gefunden wird, die der Ausnahme entspricht.</span><span class="sxs-lookup"><span data-stu-id="acac3-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="acac3-503">In dieser Beschreibung ist der ***throw-Punkt*** anfänglich der Speicherort, an dem die Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="acac3-504">Im aktuellen Funktionsmember wird jede `try` Anweisung, die den Throw-Punkt einschließt, untersucht.</span><span class="sxs-lookup"><span data-stu-id="acac3-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="acac3-505">Für jede Anweisung `S`, beginnend mit der innersten `try` -Anweisung und mit der äußersten `try` -Anweisung, werden die folgenden Schritte ausgewertet:</span><span class="sxs-lookup"><span data-stu-id="acac3-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="acac3-506">Wenn der `try` -Block `S` von den Throw-Punkt einschließt und S über eine oder mehrere `catch` Klauseln verfügt, `catch` werden die Klauseln in der Reihenfolge der Darstellung untersucht, um nach einem geeigneten Handler für die Ausnahme zu suchen. Dies erfolgt gemäß den in Abschnitt [die try-Anweisung](statements.md#the-try-statement).</span><span class="sxs-lookup"><span data-stu-id="acac3-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="acac3-507">Wenn eine überein `catch` stimmende Klausel gefunden wird, wird die Ausnahme Weitergabe durch übertragen der Steuerung an `catch` den Block dieser Klausel abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="acac3-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="acac3-508">Andernfalls wird die Steuerung `try` an den `S` `finally` `S` `catch` -`finally` Block übertragen, wenn der-Block oder ein-Block den Throw-Punkt einschließt und wenn über einen-Block verfügt.</span><span class="sxs-lookup"><span data-stu-id="acac3-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="acac3-509">Wenn der `finally` -Block eine andere Ausnahme auslöst, wird die Verarbeitung der aktuellen Ausnahme beendet.</span><span class="sxs-lookup"><span data-stu-id="acac3-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="acac3-510">Andernfalls wird die Verarbeitung der aktuellen Ausnahme fortgesetzt, `finally` wenn die Steuerung den Endpunkt des Blocks erreicht.</span><span class="sxs-lookup"><span data-stu-id="acac3-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="acac3-511">Wenn ein Ausnahmehandler im aktuellen Funktionsaufruf nicht gefunden wurde, wird der Funktionsaufruf beendet, und eine der folgenden Aktionen wird ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="acac3-512">Wenn die aktuelle Funktion nicht Async ist, werden die obigen Schritte für den Aufrufer der Funktion mit einem Throw-Punkt wiederholt, der der Anweisung entspricht, aus der der Funktionsmember aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="acac3-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="acac3-513">Wenn die aktuelle Funktion Async ist und die Aufgabe zurückgibt, wird die Ausnahme in der Rückgabe Aufgabe aufgezeichnet, die in einen fehlerhaften oder abgebrochenen Zustand versetzt wird, wie in [Enumeratorschnittstellen](classes.md#enumerator-interfaces)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="acac3-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="acac3-514">Wenn die aktuelle Funktion Async ist und void zurückgibt, wird der Synchronisierungs Kontext des aktuellen Threads wie in [Aufzähl baren Schnittstellen](classes.md#enumerable-interfaces)beschrieben benachrichtigt.</span><span class="sxs-lookup"><span data-stu-id="acac3-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="acac3-515">Wenn die Ausnahme Verarbeitung alle Funktionselement Aufrufe im aktuellen Thread beendet und angibt, dass der Thread keinen Handler für die Ausnahme aufweist, wird der Thread selbst beendet.</span><span class="sxs-lookup"><span data-stu-id="acac3-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="acac3-516">Die Auswirkungen dieser Beendigung sind Implementierungs definiert.</span><span class="sxs-lookup"><span data-stu-id="acac3-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="acac3-517">Die try-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-517">The try statement</span></span>

<span data-ttu-id="acac3-518">Die `try` -Anweisung stellt einen Mechanismus zum Abfangen von Ausnahmen bereit, die während der Ausführung eines-Blocks auftreten.</span><span class="sxs-lookup"><span data-stu-id="acac3-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="acac3-519">Außerdem bietet die `try` -Anweisung die Möglichkeit, einen Codeblock anzugeben, der immer ausgeführt wird, wenn das Steuer `try` Element die-Anweisung verlässt.</span><span class="sxs-lookup"><span data-stu-id="acac3-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

<span data-ttu-id="acac3-520">Es gibt drei mögliche Formen von `try` -Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="acac3-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="acac3-521">Ein `try` -Block, gefolgt von einem `catch` oder mehreren-Blöcken.</span><span class="sxs-lookup"><span data-stu-id="acac3-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="acac3-522">Ein `try` -Block, gefolgt `finally` von einem-Block.</span><span class="sxs-lookup"><span data-stu-id="acac3-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="acac3-523">Ein `try` -Block, gefolgt von einem `catch` oder mehreren-Blöcken `finally` , gefolgt von einem-Block.</span><span class="sxs-lookup"><span data-stu-id="acac3-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="acac3-524">Wenn eine `catch`-Klausel ein *exception_specifier*-Element angibt, muss der Typ `System.Exception`, ein Typ, der von `System.Exception` abgeleitet ist, oder ein typparametertyp sein, der über `System.Exception` (oder eine Unterklasse) als effektive Basisklasse verfügt.</span><span class="sxs-lookup"><span data-stu-id="acac3-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="acac3-525">Wenn eine `catch`-Klausel sowohl eine *exception_specifier* mit einem *Bezeichner*angibt, wird eine ***Ausnahme Variable*** mit dem angegebenen Namen und Typ deklariert.</span><span class="sxs-lookup"><span data-stu-id="acac3-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="acac3-526">Die Exception-Variable entspricht einer lokalen Variablen mit einem Bereich, der die `catch` -Klausel erweitert.</span><span class="sxs-lookup"><span data-stu-id="acac3-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="acac3-527">Während der Ausführung des *exception_filter* und des *Blocks*stellt die Ausnahme Variable die derzeit behandelte Ausnahme dar.</span><span class="sxs-lookup"><span data-stu-id="acac3-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="acac3-528">Zum Zweck der eindeutigen Zuweisungs Überprüfung wird die Ausnahme Variable als definitiv in Ihrem gesamten Bereich zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="acac3-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="acac3-529">Es ist `catch` nicht möglich, auf das Ausnahme Objekt im Filter und `catch` Block zuzugreifen, es sei denn, eine Klausel enthält einen Ausnahme Variablennamen.</span><span class="sxs-lookup"><span data-stu-id="acac3-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="acac3-530">Eine `catch`-Klausel, die keine *exception_specifier* angibt, wird als allgemeine `catch`-Klausel bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="acac3-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="acac3-531">Einige Programmiersprachen unterstützen möglicherweise Ausnahmen, die nicht als von `System.Exception`abgeleitete Objekt darstellbar sind, obwohl solche Ausnahmen nie durch C# Code generiert werden könnten.</span><span class="sxs-lookup"><span data-stu-id="acac3-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="acac3-532">Eine allgemeine `catch` Klausel kann verwendet werden, um solche Ausnahmen abzufangen.</span><span class="sxs-lookup"><span data-stu-id="acac3-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="acac3-533">Daher unterscheidet sich `catch` eine allgemeine Klausel semantisch von einer, die den Typ `System.Exception`angibt, da der erste auch Ausnahmen von anderen Sprachen abfängt.</span><span class="sxs-lookup"><span data-stu-id="acac3-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="acac3-534">Um einen Handler für eine Ausnahme zu finden, `catch` werden Klauseln in lexikalischer Reihenfolge untersucht.</span><span class="sxs-lookup"><span data-stu-id="acac3-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="acac3-535">Wenn eine `catch` Klausel einen Typ, aber keinen Ausnahme Filter angibt, ist dies ein Kompilierzeitfehler für eine `catch` spätere Klausel in derselben `try` Anweisung, um einen Typ anzugeben, der dem Typ entspricht oder von diesem abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="acac3-536">Wenn eine `catch` Klausel keinen Typ und keinen Filter angibt, muss es sich um die `catch` letzte Klausel für `try` diese Anweisung handeln.</span><span class="sxs-lookup"><span data-stu-id="acac3-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="acac3-537">Innerhalb eines `catch` -Blocks kann `throw` eine-Anweisung ([die throw-Anweisung](statements.md#the-throw-statement)) ohne Ausdruck verwendet werden, um `catch` die vom-Block aufgefangene Ausnahme erneut auszulösen.</span><span class="sxs-lookup"><span data-stu-id="acac3-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="acac3-538">Bei Zuweisungen zu einer Ausnahme Variablen wird die Ausnahme, die erneut ausgelöst wird, nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="acac3-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="acac3-539">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="acac3-539">In the example</span></span>
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
<span data-ttu-id="acac3-540">die Methode `F` fängt eine Ausnahme ab, schreibt einige Diagnoseinformationen in die Konsole, ändert die Ausnahme Variable und löst die Ausnahme erneut aus.</span><span class="sxs-lookup"><span data-stu-id="acac3-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="acac3-541">Die Ausnahme, die erneut ausgelöst wird, ist die ursprüngliche Ausnahme, sodass die Ausgabe erzeugt wird:</span><span class="sxs-lookup"><span data-stu-id="acac3-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```console
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="acac3-542">Wenn der erste catch-Block `e` ausgelöst wurde und die aktuelle Ausnahme nicht erneut ausgelöst wurde, sieht die Ausgabe wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="acac3-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```console
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="acac3-543">Es handelt sich um einen Kompilierzeitfehler `break`für `continue`eine- `goto` ,-oder-Anweisung zum über `finally` tragen der Steuerung aus einem-Block.</span><span class="sxs-lookup"><span data-stu-id="acac3-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="acac3-544">Wenn eine `break`- `continue`,- `goto` oder-Anweisung in `finally` einem-Block auftritt, muss sich das Ziel der-Anweisung `finally` innerhalb desselben Blocks befinden. andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="acac3-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="acac3-545">Es handelt sich um einen Kompilierzeitfehler `return` , damit eine-Anweisung `finally` in einem-Block auftritt.</span><span class="sxs-lookup"><span data-stu-id="acac3-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="acac3-546">Eine `try` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="acac3-547">Das Steuerelement wird an `try` den-Block übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="acac3-548">Wenn und wenn das Steuerelement den Endpunkt des `try` Blocks erreicht:</span><span class="sxs-lookup"><span data-stu-id="acac3-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="acac3-549">Wenn die `try` Anweisung über einen `finally` -Block verfügt `finally` , wird der-Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="acac3-550">Das Steuerelement wird an den Endpunkt `try` der Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="acac3-551">Wenn eine Ausnahme während der Ausführung `try` `try` des Blocks an die Anweisung weitergegeben wird:</span><span class="sxs-lookup"><span data-stu-id="acac3-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="acac3-552">Die `catch` Klauseln werden ggf. in der Reihenfolge ihrer Darstellung untersucht, um einen geeigneten Handler für die Ausnahme zu suchen.</span><span class="sxs-lookup"><span data-stu-id="acac3-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="acac3-553">Wenn eine `catch` -Klausel keinen Typ angibt oder den Ausnahmetyp oder einen Basistyp des Ausnahme Typs angibt:</span><span class="sxs-lookup"><span data-stu-id="acac3-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="acac3-554">Wenn die `catch` -Klausel eine Exception-Variable deklariert, wird das Ausnahme Objekt der Ausnahme Variablen zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="acac3-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="acac3-555">Wenn die `catch` -Klausel einen Ausnahme Filter deklariert, wird der Filter ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="acac3-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="acac3-556">Wenn ausgewertet `false`wird, ist die catch-Klausel keine Entsprechung, und die Suche wird durch alle nachfolg `catch` enden Klauseln für einen geeigneten Handler fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="acac3-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="acac3-557">Andernfalls wird die `catch` -Klausel als Übereinstimmung angesehen, und die Steuerung wird an den `catch` entsprechenden-Block übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="acac3-558">Wenn und wenn das Steuerelement den Endpunkt des `catch` Blocks erreicht:</span><span class="sxs-lookup"><span data-stu-id="acac3-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="acac3-559">Wenn die `try` Anweisung über einen `finally` -Block verfügt `finally` , wird der-Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="acac3-560">Das Steuerelement wird an den Endpunkt `try` der Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="acac3-561">Wenn eine Ausnahme während der Ausführung `try` `catch` des Blocks an die Anweisung weitergegeben wird:</span><span class="sxs-lookup"><span data-stu-id="acac3-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="acac3-562">Wenn die `try` Anweisung über einen `finally` -Block verfügt `finally` , wird der-Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="acac3-563">Die Ausnahme wird an die nächste einschließende `try` Anweisung weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="acac3-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="acac3-564">Wenn die `try` Anweisung keine `catch` Klauseln aufweist oder wenn keine `catch` Klausel mit der Ausnahme übereinstimmt:</span><span class="sxs-lookup"><span data-stu-id="acac3-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="acac3-565">Wenn die `try` Anweisung über einen `finally` -Block verfügt `finally` , wird der-Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="acac3-566">Die Ausnahme wird an die nächste einschließende `try` Anweisung weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="acac3-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="acac3-567">Die-Anweisungen eines `finally` -Blocks werden immer ausgeführt, wenn die `try` Steuerung eine-Anweisung verlässt.</span><span class="sxs-lookup"><span data-stu-id="acac3-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="acac3-568">Dies gilt unabhängig davon, ob die Steuerung aufgrund der normalen Ausführung `break`aufgrund der Ausführung einer- `goto`, `continue`-,-oder `return` -Anweisung oder als Ergebnis der Weitergabe einer Ausnahme aus der `try` an.</span><span class="sxs-lookup"><span data-stu-id="acac3-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="acac3-569">Wenn während der Ausführung `finally` eines-Blocks eine Ausnahme ausgelöst wird und nicht innerhalb desselben letzten Blocks abgefangen wird, wird die Ausnahme an die nächste einschließende `try` Anweisung weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="acac3-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="acac3-570">Wenn eine andere Ausnahme gerade weitergegeben wurde, geht diese Ausnahme verloren.</span><span class="sxs-lookup"><span data-stu-id="acac3-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="acac3-571">Der Prozess der Weitergabe einer Ausnahme wird weiter unten in der Beschreibung der `throw` Anweisung ([der throw-Anweisung](statements.md#the-throw-statement)) erläutert.</span><span class="sxs-lookup"><span data-stu-id="acac3-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="acac3-572">Der `try` -Block einer `try` -Anweisung ist erreichbar, `try` wenn die-Anweisung erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="acac3-573">Ein `catch` Block `try` einer-Anweisung ist erreichbar, wenn die-Anweisung erreichbar ist. `try`</span><span class="sxs-lookup"><span data-stu-id="acac3-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="acac3-574">Der `finally` -Block einer `try` -Anweisung ist erreichbar, `try` wenn die-Anweisung erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="acac3-575">Der Endpunkt einer `try` -Anweisung ist erreichbar, wenn beide der folgenden Punkte zutreffen:</span><span class="sxs-lookup"><span data-stu-id="acac3-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="acac3-576">Der Endpunkt des `try` Blocks ist erreichbar, oder der Endpunkt von mindestens einem `catch` Block ist erreichbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="acac3-577">Wenn ein `finally` -Block vorhanden ist, ist der Endpunkt `finally` des-Blocks erreichbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="acac3-578">Die aktivierten und deaktivierten Anweisungen</span><span class="sxs-lookup"><span data-stu-id="acac3-578">The checked and unchecked statements</span></span>

<span data-ttu-id="acac3-579">Die `checked` - `unchecked` und-Anweisungen werden verwendet, um den ***Überlauf Überprüfungs Kontext*** für arithmetische Operationen im ganzzahligen Typ und Konvertierungen zu steuern.</span><span class="sxs-lookup"><span data-stu-id="acac3-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="acac3-580">Die `checked` -Anweisung bewirkt, dass alle Ausdrücke im- *Block* in einem überprüften Kontext ausgewertet werden `unchecked` , und die-Anweisung bewirkt, dass alle Ausdrücke im *Block* in einem nicht überprüften Kontext ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="acac3-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="acac3-581">Die `checked` - `unchecked` und-Anweisungen entsprechen genau den `checked` Operatoren und (die aktivierten und deaktivierten[Operatoren](expressions.md#the-checked-and-unchecked-operators)), mit dem Unterschied, dass Sie anstelle von Ausdrücken an- `unchecked` Blöcken arbeiten.</span><span class="sxs-lookup"><span data-stu-id="acac3-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="acac3-582">Die Lock-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-582">The lock statement</span></span>

<span data-ttu-id="acac3-583">Die `lock` -Anweisung ruft die Sperre für den gegenseitigen Ausschluss für ein bestimmtes Objekt ab, führt eine-Anweisung aus und gibt dann die Sperre frei.</span><span class="sxs-lookup"><span data-stu-id="acac3-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="acac3-584">Der Ausdruck einer `lock`-Anweisung muss einen Wert eines Typs bezeichnen, der bekanntermaßen *reference_type*ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="acac3-585">Für den Ausdruck einer `lock`-Anweisung wird nie eine implizite Boxing-Konvertierung ([Boxing-Konvertierungen](conversions.md#boxing-conversions)) ausgeführt, und daher ist es ein Kompilierzeitfehler, wenn der Ausdruck den Wert eines *value_type*angibt.</span><span class="sxs-lookup"><span data-stu-id="acac3-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="acac3-586">Eine `lock` -Anweisung der Form</span><span class="sxs-lookup"><span data-stu-id="acac3-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="acac3-587">Wenn `x` ein Ausdruck eines *reference_type*ist, ist genau Äquivalent zu</span><span class="sxs-lookup"><span data-stu-id="acac3-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
<span data-ttu-id="acac3-588">außer dass `x` nur einmal überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="acac3-589">Während eine gegenseitige Ausschluss Sperre aufrechterhalten wird, kann der Code, der im selben Ausführungs Thread ausgeführt wird, auch die Sperre abrufen und freigeben.</span><span class="sxs-lookup"><span data-stu-id="acac3-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="acac3-590">Der Code, der in anderen Threads ausgeführt wird, wird jedoch blockiert, bis die Sperre aufgehoben wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="acac3-591">Das `System.Type` Sperren von Objekten, um den Zugriff auf statische Daten zu synchronisieren, wird nicht empfohlen.</span><span class="sxs-lookup"><span data-stu-id="acac3-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="acac3-592">Anderer Code kann denselben Typ sperren, was zu einem Deadlock führen kann.</span><span class="sxs-lookup"><span data-stu-id="acac3-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="acac3-593">Ein besserer Ansatz besteht darin, den Zugriff auf statische Daten zu synchronisieren, indem ein privates statisches Objekt gesperrt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="acac3-594">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="acac3-594">For example:</span></span>
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a><span data-ttu-id="acac3-595">Die using-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-595">The using statement</span></span>

<span data-ttu-id="acac3-596">Die `using` -Anweisung ruft eine oder mehrere Ressourcen ab, führt eine-Anweisung aus und verwirft dann die Ressource.</span><span class="sxs-lookup"><span data-stu-id="acac3-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="acac3-597">Eine ***Ressource*** ist eine Klasse oder Struktur, die `System.IDisposable`implementiert, die eine einzelne Parameter lose Methode namens `Dispose`enthält.</span><span class="sxs-lookup"><span data-stu-id="acac3-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="acac3-598">Code, der eine Ressource verwendet, kann `Dispose` aufzurufen, um anzugeben, dass die Ressource nicht mehr benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="acac3-599">Wenn `Dispose` nicht aufgerufen wird, wird die automatische Entfernung schließlich aufgrund Garbage Collection ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="acac3-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="acac3-600">Wenn die Form von *resource_acquisition* *local_variable_declaration* ist, muss der Typ des *local_variable_declaration* entweder `dynamic` oder ein Typ sein, der implizit in `System.IDisposable` konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="acac3-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="acac3-601">Wenn die Form von *resource_acquisition* *Expression* ist, muss dieser Ausdruck implizit in `System.IDisposable` konvertiert werden können.</span><span class="sxs-lookup"><span data-stu-id="acac3-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="acac3-602">Lokale Variablen, die in einem *resource_acquisition* deklariert werden, sind schreibgeschützt und müssen einen Initialisierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="acac3-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="acac3-603">Ein Kompilierzeitfehler tritt auf, wenn die eingebettete Anweisung versucht, diese lokalen Variablen (über die `++` -und-Operatoren) zu ändern, die Adresse dieser Variablen zu `ref` über `out` nehmen oder Sie als-oder- `--` Parameter zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="acac3-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="acac3-604">Eine `using` -Anweisung wird in drei Teile übersetzt: Beschaffung, Verwendung und Entsorgung.</span><span class="sxs-lookup"><span data-stu-id="acac3-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="acac3-605">Die Verwendung der Ressource ist implizit in eine `try` -Anweisung eingeschlossen, die eine `finally` -Klausel einschließt.</span><span class="sxs-lookup"><span data-stu-id="acac3-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="acac3-606">Diese `finally` Klausel verwirft die Ressource.</span><span class="sxs-lookup"><span data-stu-id="acac3-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="acac3-607">Wenn eine `null` Ressource `Dispose` abgerufen wird, wird kein-Rückruf durchgeführt, und es wird keine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="acac3-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="acac3-608">Wenn die Ressource vom Typ `dynamic` ist, wird Sie dynamisch durch eine implizite dynamische Konvertierung ([implizite dynamische](conversions.md#implicit-dynamic-conversions) `IDisposable` Konvertierungen) in in den Erwerb konvertiert, um sicherzustellen, dass die Konvertierung erfolgreich war, bevor die Verwendung und Ihnen.</span><span class="sxs-lookup"><span data-stu-id="acac3-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="acac3-609">Eine `using` -Anweisung der Form</span><span class="sxs-lookup"><span data-stu-id="acac3-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="acac3-610">entspricht einer von drei möglichen Erweiterungen.</span><span class="sxs-lookup"><span data-stu-id="acac3-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="acac3-611">Wenn `ResourceType` ein Werttyp ist, der keine NULL-Werte zulässt, ist die Erweiterung</span><span class="sxs-lookup"><span data-stu-id="acac3-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="acac3-612">Andernfalls: Wenn `ResourceType` ein Werte zulässt-Werttyp oder ein anderer Verweistyp `dynamic`als ist, wird die Erweiterung</span><span class="sxs-lookup"><span data-stu-id="acac3-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="acac3-613">Andernfalls ist die `ResourceType` Erweiterung `dynamic`, wenn den Wert hat.</span><span class="sxs-lookup"><span data-stu-id="acac3-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

<span data-ttu-id="acac3-614">Bei beiden Erweiterungen ist die `resource` -Variable in der eingebetteten-Anweisung schreibgeschützt, `d` und die-Variable ist in der eingebetteten-Anweisung nicht verfügbar, und Sie ist nicht sichtbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="acac3-615">Eine Implementierung darf eine angegebene using-Anweisung anders implementieren, z. b. aus Leistungsgründen, solange das Verhalten mit der obigen Erweiterung konsistent ist.</span><span class="sxs-lookup"><span data-stu-id="acac3-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="acac3-616">Eine `using` -Anweisung der Form</span><span class="sxs-lookup"><span data-stu-id="acac3-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="acac3-617">hat dieselben drei möglichen Erweiterungen.</span><span class="sxs-lookup"><span data-stu-id="acac3-617">has the same three possible expansions.</span></span> <span data-ttu-id="acac3-618">In diesem Fall `ResourceType` ist implizit der Kompilier Zeittyp `expression`von, wenn er über einen verfügt.</span><span class="sxs-lookup"><span data-stu-id="acac3-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="acac3-619">Andernfalls wird die `IDisposable` -Schnittstelle selbst `ResourceType`als verwendet.</span><span class="sxs-lookup"><span data-stu-id="acac3-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="acac3-620">Auf `resource` die-Variable kann nicht zugegriffen werden, und die eingebettete-Anweisung ist unsichtbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="acac3-621">Wenn ein *resource_acquisition* das Format eines *local_variable_declaration*annimmt, ist es möglich, mehrere Ressourcen eines bestimmten Typs zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="acac3-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="acac3-622">Eine `using` -Anweisung der Form</span><span class="sxs-lookup"><span data-stu-id="acac3-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="acac3-623">entspricht genau einer Sequenz von `using` -Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="acac3-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="acac3-624">Im folgenden Beispiel wird eine Datei mit `log.txt` dem Namen erstellt, und es werden zwei Textzeilen in die Datei geschrieben.</span><span class="sxs-lookup"><span data-stu-id="acac3-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="acac3-625">Im Beispiel wird dann dieselbe Datei zum Lesen geöffnet, und die enthaltenen Textzeilen werden in die Konsole kopiert.</span><span class="sxs-lookup"><span data-stu-id="acac3-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

<span data-ttu-id="acac3-626">Da die `TextWriter` Klassen `TextReader` und die `IDisposable` -Schnittstelle implementieren, kann im `using` Beispiel-Anweisungen verwendet werden, um sicherzustellen, dass die zugrunde liegende Datei nach den Schreib-oder Lesevorgängen ordnungsgemäß geschlossen wird</span><span class="sxs-lookup"><span data-stu-id="acac3-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="acac3-627">Die yield-Anweisung</span><span class="sxs-lookup"><span data-stu-id="acac3-627">The yield statement</span></span>

<span data-ttu-id="acac3-628">Die `yield` -Anweisung wird in einem Iteratorblock ([Blocks](statements.md#blocks)) verwendet, um einen Wert für das Enumeratorobjekt ([Enumeratorobjekte](classes.md#enumerator-objects)) oder das Aufzähl Bare Objekt ([Aufzähl Bare Objekte](classes.md#enumerable-objects)) eines Iterators oder das Ende der Iterationen anzugeben.</span><span class="sxs-lookup"><span data-stu-id="acac3-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="acac3-629">`yield`ist kein reserviertes Wort. Sie hat nur dann eine besondere Bedeutung, wenn Sie `return` direkt `break` vor einem Schlüsselwort oder verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="acac3-630">In anderen Kontexten `yield` kann als Bezeichner verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="acac3-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="acac3-631">Es gibt mehrere Einschränkungen hinsichtlich des Orts `yield` , an dem eine-Anweisung angezeigt werden kann, wie im folgenden beschrieben.</span><span class="sxs-lookup"><span data-stu-id="acac3-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="acac3-632">Es handelt sich um einen Kompilierzeitfehler für eine `yield`-Anweisung (von beiden Formularen), die außerhalb von *method_body*, *operator_body* oder *accessor_body* angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="acac3-633">Es handelt sich um einen Kompilierzeitfehler `yield` für eine-Anweisung (von beiden Formularen), die in einer anonymen Funktion angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="acac3-634">Es handelt sich um einen Kompilierzeitfehler `yield` für eine-Anweisung (von beiden Formularen), `finally` die in der `try` -Klausel einer-Anweisung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="acac3-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="acac3-635">Es ist ein Kompilierzeitfehler, wenn `yield return` eine-Anweisung an einer beliebigen `try` Stelle in einer- `catch` Anweisung angezeigt wird, die Klauseln enthält.</span><span class="sxs-lookup"><span data-stu-id="acac3-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="acac3-636">Das folgende Beispiel zeigt einige gültige und ungültige Verwendungen von `yield` -Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="acac3-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

<span data-ttu-id="acac3-637">Eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) muss vom Typ des Ausdrucks in der `yield return` -Anweisung bis zum Yield-Typ ([Yield-Typ](classes.md#yield-type)) des Iterators vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="acac3-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="acac3-638">Eine `yield return` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="acac3-639">Der in der-Anweisung angegebene Ausdruck wird ausgewertet, implizit in den Yield-Typ konvertiert und der `Current` -Eigenschaft des Enumeratorobjekts zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="acac3-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="acac3-640">Die Ausführung des Iteratorblocks wurde angehalten.</span><span class="sxs-lookup"><span data-stu-id="acac3-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="acac3-641">Wenn sich `yield return` die-Anweisung innerhalb eines oder `try` mehrerer Blöcke befindet, `finally` werden die zugeordneten Blöcke zurzeit nicht ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acac3-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="acac3-642">Die `MoveNext` -Methode des Enumeratorobjekts wird `true` an den Aufrufer zurückgegeben und gibt an, dass das Enumeratorobjekt erfolgreich auf das nächste Element erweitert wurde.</span><span class="sxs-lookup"><span data-stu-id="acac3-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="acac3-643">Der nächste aufrufungs Vorgang der- `MoveNext` Methode des Enumeratorobjekts setzt die Ausführung des Iteratorblocks fort, von wo er zuletzt angehalten wurde.</span><span class="sxs-lookup"><span data-stu-id="acac3-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="acac3-644">Eine `yield break` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="acac3-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="acac3-645">Wenn die `yield break` -Anweisung von einem oder mehreren `try` -Blöcken mit zugeordneten `finally` -Blöcken eingeschlossen wird, wird die `finally` Steuerung anfänglich an den `try` -Block der innersten-Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="acac3-646">Wenn und wenn das Steuerelement den Endpunkt eines `finally` -Blocks erreicht, wird die Steuerung an den `finally` -Block der nächsten einschließenden `try` Anweisung übertragen.</span><span class="sxs-lookup"><span data-stu-id="acac3-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="acac3-647">Dieser Vorgang wird wiederholt, `finally` bis die Blöcke aller einschließenden `try` Anweisungen ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="acac3-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="acac3-648">Das Steuerelement wird an den Aufrufer des Iteratorblocks zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="acac3-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="acac3-649">Dies ist entweder die `MoveNext` -Methode `Dispose` oder die-Methode des Enumeratorobjekts.</span><span class="sxs-lookup"><span data-stu-id="acac3-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="acac3-650">Da eine `yield break` Anweisung die Steuerung an andere Stellen überträgt, ist der Endpunkt `yield break` einer Anweisung nie erreichbar.</span><span class="sxs-lookup"><span data-stu-id="acac3-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
