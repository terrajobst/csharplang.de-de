# <a name="statements"></a><span data-ttu-id="45972-101">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="45972-101">Statements</span></span>

<span data-ttu-id="45972-102">C# bietet eine Vielzahl von Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="45972-102">C# provides a variety of statements.</span></span> <span data-ttu-id="45972-103">Die meisten dieser Anweisungen werden Entwicklern vertraut sein, die in C und C++ programmiert haben.</span><span class="sxs-lookup"><span data-stu-id="45972-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

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

<span data-ttu-id="45972-104">Die *Embedded_statement* nichtterminal wird verwendet, Anweisungen, die in anderen Anweisungen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="45972-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="45972-105">Die Verwendung von *Embedded_statement* statt *Anweisung* schließt die Verwendung von deklarationsanweisungen und Anweisungen mit Bezeichnung in diesen Kontexten.</span><span class="sxs-lookup"><span data-stu-id="45972-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="45972-106">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="45972-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="45972-107">führt zu einem Kompilierzeitfehler, da ein `if` -Anweisung erfordert eine *Embedded_statement* anstelle eines *Anweisung* für die If Branch.</span><span class="sxs-lookup"><span data-stu-id="45972-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="45972-108">Wenn dieser Code wurden zulässig, klicken Sie dann die Variable `i` deklariert werden würde, aber nie verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="45972-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="45972-109">Beachten Sie jedoch, indem Sie platzieren `i`der Deklaration in einem Block, der im Beispiel ist gültig.</span><span class="sxs-lookup"><span data-stu-id="45972-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="45972-110">Endpunkte und Erreichbarkeit</span><span class="sxs-lookup"><span data-stu-id="45972-110">End points and reachability</span></span>

<span data-ttu-id="45972-111">Jede Anweisung verfügt über eine ***Endpunkt***.</span><span class="sxs-lookup"><span data-stu-id="45972-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="45972-112">Intuitiv ausgedrückt ist der Endpunkt einer Anweisung den Speicherort an, der die Anweisung unmittelbar folgt.</span><span class="sxs-lookup"><span data-stu-id="45972-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="45972-113">Die Ausführungsregeln für zusammengesetzte Statements (Anweisungen, die eingebettete Anweisungen enthalten) Geben Sie die Aktion, die ausgeführt wird, wenn die Steuerung der Endpunkt, der eine eingebettete Anweisung erreicht.</span><span class="sxs-lookup"><span data-stu-id="45972-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="45972-114">Bei der Kontrolle über den Endpunkt einer Anweisung in einem Block erreicht, wird z. B. die Steuerung an die nächste Anweisung im-Block übergeben.</span><span class="sxs-lookup"><span data-stu-id="45972-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="45972-115">Wenn eine Anweisung möglicherweise durch die Ausführung erreicht werden kann, die Anweisung gilt ***erreichbar***.</span><span class="sxs-lookup"><span data-stu-id="45972-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="45972-116">Im Gegensatz dazu ist es nicht möglich, dass eine Anweisung ausgeführt wird, die Anweisung gilt ***nicht erreichbar***.</span><span class="sxs-lookup"><span data-stu-id="45972-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="45972-117">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="45972-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="45972-118">der zweite Aufruf von `Console.WriteLine` ist nicht erreichbar, da besteht kein Risiko, dass die Anweisung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="45972-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="45972-119">Eine Warnung wird ausgegeben, wenn der Compiler feststellt, dass eine Anweisung nicht erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="45972-120">Es ist insbesondere nicht um einen Fehler für eine Anweisung als nicht erreichbar.</span><span class="sxs-lookup"><span data-stu-id="45972-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="45972-121">Um zu bestimmen, ob eine bestimmte Anweisung oder der Endpunkt erreichbar ist, führt der Compiler Flussanalyse gemäß den Erreichbarkeitsregeln für jede Anweisung an.</span><span class="sxs-lookup"><span data-stu-id="45972-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="45972-122">Die Flussanalyse berücksichtigt die Werte der Konstanten Ausdrücke ([Konstante Ausdrücke](expressions.md#constant-expressions)), die das Verhalten der Anweisungen steuern, aber die möglichen Werte nicht Konstante Ausdrücke werden nicht berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="45972-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="45972-123">Das heißt, für Zwecke der Ablaufsteuerungsanalyse, ein nicht konstanter Ausdruck eines bestimmten Typs gilt alle möglichen Werte dieses Typs haben.</span><span class="sxs-lookup"><span data-stu-id="45972-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="45972-124">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="45972-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="45972-125">der boolesche Ausdruck, der die `if` -Anweisung ist ein konstanter Ausdruck, weil beide Operanden aus der `==` Operator Konstanten sind.</span><span class="sxs-lookup"><span data-stu-id="45972-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="45972-126">Wie der Konstante Ausdruck zur Kompilierzeit ausgewertet wird, den Wert erzeugt `false`, `Console.WriteLine` Aufruf gilt als nicht erreichbar.</span><span class="sxs-lookup"><span data-stu-id="45972-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="45972-127">Aber wenn `i` wird geändert, um eine lokale Variable.</span><span class="sxs-lookup"><span data-stu-id="45972-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="45972-128">die `Console.WriteLine` Aufruf gilt als erreichbar sind, auch wenn sich in der Praxis es nie ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="45972-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="45972-129">Die *Block* einer Funktion Element immer als erreichbar.</span><span class="sxs-lookup"><span data-stu-id="45972-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="45972-130">Durch die Auswertung der nacheinander der Erreichbarkeitsregeln für jede Anweisung in einem Block, kann die Erreichbarkeit des jede angegebene Anweisung ermittelt werden.</span><span class="sxs-lookup"><span data-stu-id="45972-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="45972-131">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="45972-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="45972-132">die Erreichbarkeit des zweiten `Console.WriteLine` wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="45972-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="45972-133">Die erste `Console.WriteLine` Ausdrucksanweisung erreichbar ist. da der Block der `F` Methode erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="45972-134">Der Endpunkt des ersten `Console.WriteLine` Ausdrucksanweisung ist erreichbar, da die Anweisung erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="45972-135">Die `if` -Anweisung erreichbar ist, daran, dass das Ende des ersten `Console.WriteLine` Ausdrucksanweisung ist erreichbar.</span><span class="sxs-lookup"><span data-stu-id="45972-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="45972-136">Die zweite `Console.WriteLine` Ausdrucksanweisung erreichbar ist. da der boolesche Ausdruck, der die `if` Anweisung verfügt nicht über den konstanten Wert `false`.</span><span class="sxs-lookup"><span data-stu-id="45972-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="45972-137">Es gibt zwei Situationen, in denen ein Fehler während der Kompilierung für den Endpunkt einer Anweisung erreichbar ist:</span><span class="sxs-lookup"><span data-stu-id="45972-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="45972-138">Da die `switch` Anweisung ist einen Switch-Abschnitt auf "Fortfahren" mit dem nächsten Switch-Abschnitt nicht zulässig, es ist ein Fehler während der Kompilierung für den Endpunkt der Anweisungsliste einen Switch-Abschnitt erreichbar sein.</span><span class="sxs-lookup"><span data-stu-id="45972-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="45972-139">Wenn dieser Fehler auftritt, ist es in der Regel ein Hinweis auf, die eine `break` Anweisung ist nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="45972-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="45972-140">Es ist ein Fehler während der Kompilierung für den Endpunkt des Codeblocks an ein Funktionsmember, der berechnet einen Wert als erreichbar sein.</span><span class="sxs-lookup"><span data-stu-id="45972-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="45972-141">Wenn dieser Fehler auftritt, ist es in der Regel ein Hinweis auf, die eine `return` Anweisung ist nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="45972-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="45972-142">Blöcke</span><span class="sxs-lookup"><span data-stu-id="45972-142">Blocks</span></span>

<span data-ttu-id="45972-143">Ein *Block* ermöglicht, mehrere Anweisungen in Kontexten zu schreiben, in denen eine einzelne Anweisung zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="45972-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="45972-144">Ein *Block* besteht aus einem optionalen *Statement_list* ([Anweisung listet](statements.md#statement-lists)) in geschweiften Klammern.</span><span class="sxs-lookup"><span data-stu-id="45972-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="45972-145">Wenn die Anweisungsliste ausgelassen wird, wird der Block als leer sein. bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="45972-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="45972-146">Ein Block kann deklarationsanweisungen enthalten ([deklarationsanweisungen](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="45972-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="45972-147">Im Rahmen einer lokalen Variable oder Konstante wird in einem Block deklariert der Block.</span><span class="sxs-lookup"><span data-stu-id="45972-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="45972-148">Ein Block ist wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="45972-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="45972-149">Wenn der Block leer ist, wird die Steuerung an den Endpunkt des Blocks übergeben.</span><span class="sxs-lookup"><span data-stu-id="45972-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="45972-150">Wenn der Block nicht leer ist, wird die Steuerung an die Anweisungsliste übergeben.</span><span class="sxs-lookup"><span data-stu-id="45972-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="45972-151">Wenn und Steuerung am Ende der Anweisungsliste erreicht, wird die Steuerung an den Endpunkt des Blocks übergeben.</span><span class="sxs-lookup"><span data-stu-id="45972-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="45972-152">Die Anweisungsliste eines Blocks ist erreichbar, wenn der Block erreicht werden kann.</span><span class="sxs-lookup"><span data-stu-id="45972-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="45972-153">Der Endpunkt eines Blocks ist erreichbar, wenn der Block leer ist oder wenn der Endpunkt der Anweisungsliste erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="45972-154">Ein *Block* , enthält eine oder mehrere `yield` Anweisungen ([die Yield-Anweisung](statements.md#the-yield-statement)) kein Iteratorblock aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="45972-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="45972-155">Iteratorblöcke werden verwendet, um Funktionsmember als Iteratoren implementiert ([Iteratoren](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="45972-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="45972-156">Es gelten zusätzlichen Einschränkungen gelten für Iteratorblöcke:</span><span class="sxs-lookup"><span data-stu-id="45972-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="45972-157">Es ist ein Fehler während der Kompilierung für eine `return` -Anweisung in einem Iteratorblock angezeigt (aber `yield return` -Anweisungen sind zulässig).</span><span class="sxs-lookup"><span data-stu-id="45972-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="45972-158">Es ist ein Fehler während der Kompilierung für die kein Iteratorblock einen unsicheren Kontext enthalten ([nicht sicheren Kontexten](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="45972-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="45972-159">Kein Iteratorblock definiert immer einen sicheren Kontext ein, auch wenn der Deklaration in einem unsicheren Kontext geschachtelt sind.</span><span class="sxs-lookup"><span data-stu-id="45972-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="45972-160">Anweisungslisten</span><span class="sxs-lookup"><span data-stu-id="45972-160">Statement lists</span></span>

<span data-ttu-id="45972-161">Ein ***Anweisungsliste*** besteht aus mindestens einer Anweisung, die in der Sequenz geschrieben wurden.</span><span class="sxs-lookup"><span data-stu-id="45972-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="45972-162">Anweisungslisten kommen *Block*s ([Blöcke](statements.md#blocks)) und im *Switch_block*s ([der Switch-Anweisung](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="45972-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="45972-163">Eine Anweisungsliste wird durch Übergabe der Steuerung an die erste Anweisung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="45972-164">Wenn und Kontrolle über den Endpunkt einer Anweisung erreicht, wird die Steuerung an die nächste Anweisung übergeben.</span><span class="sxs-lookup"><span data-stu-id="45972-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="45972-165">Wenn und Steuerung der Endpunkt der letzten Anweisung erreicht, wird die Steuerung an den Endpunkt der Anweisungsliste übergeben.</span><span class="sxs-lookup"><span data-stu-id="45972-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="45972-166">Eine Anweisung in eine Anweisungsliste ist erreichbar, wenn mindestens eine der folgenden "true" ist:</span><span class="sxs-lookup"><span data-stu-id="45972-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="45972-167">Die Anweisung ist die erste Anweisung aus, und die Anweisungsliste selbst erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="45972-168">Der Endpunkt der vorherigen Anweisung ist erreichbar.</span><span class="sxs-lookup"><span data-stu-id="45972-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="45972-169">Die Anweisung ist eine Anweisung mit Bezeichnung und die Bezeichnung verweist auf einen erreichbaren `goto` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="45972-170">Der Endpunkt eine Anweisungsliste ist erreichbar, wenn der Endpunkt, der die letzte Anweisung in der Liste erreicht werden kann.</span><span class="sxs-lookup"><span data-stu-id="45972-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="45972-171">Die leere Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-171">The empty statement</span></span>

<span data-ttu-id="45972-172">Ein *Empty_statement* hat keine Auswirkungen.</span><span class="sxs-lookup"><span data-stu-id="45972-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="45972-173">Eine leere Anweisung wird verwendet, wenn es keine Vorgänge in einem Kontext ausgeführt werden, wenn eine Anweisung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="45972-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="45972-174">Ausführung eine leere Anweisung wird einfach die Steuerung an den Endpunkt der Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="45972-175">Daher ist der Endpunkt, der eine leere Anweisung erreichbar, wenn die leere Anweisung erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="45972-176">Eine leere Anweisung kann verwendet werden, wenn das Schreiben einer `while` -Anweisung mit einem null-Text:</span><span class="sxs-lookup"><span data-stu-id="45972-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="45972-177">Darüber hinaus kann eine leere Anweisung verwendet werden, deklarieren Sie eine Bezeichnung direkt vor dem abschließenden "`}`" eines Blocks:</span><span class="sxs-lookup"><span data-stu-id="45972-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="45972-178">Anweisungen mit Bezeichnung</span><span class="sxs-lookup"><span data-stu-id="45972-178">Labeled statements</span></span>

<span data-ttu-id="45972-179">Ein *Labeled_statement* ermöglicht eine Anweisung eine Bezeichnung vorangestellt werden.</span><span class="sxs-lookup"><span data-stu-id="45972-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="45972-180">Anweisungen mit Bezeichnung sind in Blöcken zulässig, aber Sie sind als eingebettete Anweisungen nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="45972-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="45972-181">Eine bezeichnete Anweisung deklariert eine Bezeichnung mit dem Namen durch die *Bezeichner*.</span><span class="sxs-lookup"><span data-stu-id="45972-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="45972-182">Der Bereich einer Bezeichnung ist auf den ganzen Block in der die Bezeichnung deklariert ist, einschließlich aller geschachtelten Blöcke.</span><span class="sxs-lookup"><span data-stu-id="45972-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="45972-183">Es ist ein Fehler während der Kompilierung für zwei Bezeichnungen mit dem gleichen Namen, um überlappende Bereiche zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="45972-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="45972-184">Eine Bezeichnung aus verwiesen werden kann `goto` Anweisungen ([Goto-Anweisung](statements.md#the-goto-statement)) innerhalb des Bereichs der Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="45972-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="45972-185">Dies bedeutet, dass `goto` Anweisungen können Steuerelement innerhalb von Blöcken und Out-of-Blöcken, aber nie in Blöcke übertragen.</span><span class="sxs-lookup"><span data-stu-id="45972-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="45972-186">Bezeichnungen müssen ihre eigenen Deklarationsabschnitt und verursachen keine Konflikte mit anderen Bezeichnern.</span><span class="sxs-lookup"><span data-stu-id="45972-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="45972-187">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="45972-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="45972-188">gültig ist und verwendet den Namen des `x` als Parameter und eine Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="45972-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="45972-189">Ausführung einer Anweisung mit Bezeichnung entspricht der Ausführung der Anweisung nach der Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="45972-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="45972-190">Zusätzlich zu der Erreichbarkeit von normalen ablaufsteuerung bereitgestellt wird, ist eine Anweisung mit Bezeichnung erreichbar, wenn die Bezeichnung einen erreichbaren verweist `goto` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="45972-191">(Ausnahme: Wenn eine `goto` -Anweisung ist innerhalb einer `try` , enthält eine `finally` Block und der Anweisung mit Bezeichnung befindet sich außerhalb der `try`, und den Endpunkt des der `finally` Block ist nicht erreichbar, und klicken Sie dann die Anweisung mit Bezeichnung ist nicht erreichbar, `goto` Anweisung.)</span><span class="sxs-lookup"><span data-stu-id="45972-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="45972-192">Deklarationsanweisungen</span><span class="sxs-lookup"><span data-stu-id="45972-192">Declaration statements</span></span>

<span data-ttu-id="45972-193">Ein *Declaration_statement* deklariert eine lokale Variable oder Konstante.</span><span class="sxs-lookup"><span data-stu-id="45972-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="45972-194">Deklarationsanweisungen sind in Blöcken zulässig, aber Sie sind als eingebettete Anweisungen nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="45972-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="45972-195">Deklarationen von lokalen Variablen</span><span class="sxs-lookup"><span data-stu-id="45972-195">Local variable declarations</span></span>

<span data-ttu-id="45972-196">Ein *Local_variable_declaration* deklariert eine oder mehrere lokale Variablen.</span><span class="sxs-lookup"><span data-stu-id="45972-196">A *local_variable_declaration* declares one or more local variables.</span></span>

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

<span data-ttu-id="45972-197">Die *Local_variable_type* von einem *Local_variable_declaration* direkt gibt den Typ der Variablen durch die Deklaration eingeführt wurden, oder gibt an, mit dem Bezeichner `var` , Der Typ sollte basierend auf einem Initialisierer abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="45972-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="45972-198">Der Typ folgt eine Liste der *Local_variable_declarator*s, von denen jede eine neue Variable führt.</span><span class="sxs-lookup"><span data-stu-id="45972-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="45972-199">Ein *Local_variable_declarator* besteht aus einer *Bezeichner* mit dem Namen der Variablen, optional gefolgt von einer "`=`" token und einem *Local_variable_initializer* , durch den Anfangswert der Variablen erhalten.</span><span class="sxs-lookup"><span data-stu-id="45972-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="45972-200">Im Kontext der Deklaration einer lokalen Variablen, Var Bezeichner fungiert, als ein kontextbezogenes Schlüsselwort ([Schlüsselwörter](lexical-structure.md#keywords)). Bei der *Local_variable_type* angegeben ist, als `var` und kein Typ mit dem Namen `var` ist im Gültigkeitsbereich die Deklaration ist eine ***implizit typisierten Deklaration lokalen Variablen***, dessen Typ abgeleitet aus dem Typ des initialisiererausdrucks zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="45972-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="45972-201">Implizit typisierte lokale Variable Deklarationen sind jedoch mit folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="45972-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="45972-202">Die *Local_variable_declaration* kann nicht mehrere enthalten *Local_variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="45972-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="45972-203">Die *Local_variable_declarator* müssen eine *Local_variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="45972-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="45972-204">Die *Local_variable_initializer* muss ein *Ausdruck*.</span><span class="sxs-lookup"><span data-stu-id="45972-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="45972-205">Der Initialisierer *Ausdruck* muss eine während der Kompilierung aufweisen.</span><span class="sxs-lookup"><span data-stu-id="45972-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="45972-206">Der Initialisierer *Ausdruck* kann nicht auf die deklarierte Variable sich selbst verweisen</span><span class="sxs-lookup"><span data-stu-id="45972-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="45972-207">Es folgen Beispiele für falsche implizit typisierte lokale Variable Deklarationen:</span><span class="sxs-lookup"><span data-stu-id="45972-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="45972-208">Der Wert einer lokalen Variablen wird abgerufen, in einem Ausdruck mit einer *Simple_name* ([einfache Namen](expressions.md#simple-names)), sowie der Wert einer lokalen Variablen mit geändert wird ein *Zuweisung* ( [Zuweisungsoperatoren](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="45972-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="45972-209">Eine lokale Variable muss definitiv zugewiesen werden ([definitive Zuweisung](variables.md#definite-assignment)) an jedem Standort, bei dem der Wert abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="45972-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="45972-210">Eine lokale Variable deklariert, die im Rahmen einer *Local_variable_declaration* ist der Block in dem die Deklaration erfolgt.</span><span class="sxs-lookup"><span data-stu-id="45972-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="45972-211">Es ist ein Fehler auf Sie verweisen auf eine lokale Variable in der Lage, Text vor dem *Local_variable_declarator* der lokalen Variablen.</span><span class="sxs-lookup"><span data-stu-id="45972-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="45972-212">Innerhalb des Bereichs einer lokalen Variablen ist es ein Fehler während der Kompilierung eine andere lokale Variable oder Konstante mit dem gleichen Namen deklariert.</span><span class="sxs-lookup"><span data-stu-id="45972-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="45972-213">Die Deklaration eine lokale Variable, die mehrere Variablen deklariert entspricht mehreren Deklarationen der einzelnen Variablen mit dem gleichen Typ.</span><span class="sxs-lookup"><span data-stu-id="45972-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="45972-214">Darüber hinaus entspricht einem Variableninitialisierer in einer lokalen Variablendeklaration genau einer zuweisungsanweisung, die sofort nach der Deklaration eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="45972-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="45972-215">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="45972-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="45972-216">genau entspricht</span><span class="sxs-lookup"><span data-stu-id="45972-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="45972-217">In einer implizit typisierten lokalen Variablendeklaration wird der Typ der deklarierten lokalen Variablen erstellt, um den Typ des Ausdrucks verwendet, um die Variable initialisieren identisch sein.</span><span class="sxs-lookup"><span data-stu-id="45972-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="45972-218">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="45972-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="45972-219">Implizit typisierten lokalen Variablendeklarationen oben sind die folgenden explizit typisierten Deklarationen genaue Entsprechung:</span><span class="sxs-lookup"><span data-stu-id="45972-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="45972-220">Deklaration von lokalen Konstanten</span><span class="sxs-lookup"><span data-stu-id="45972-220">Local constant declarations</span></span>

<span data-ttu-id="45972-221">Ein *Local_constant_declaration* eine oder mehrere lokale Konstanten deklariert.</span><span class="sxs-lookup"><span data-stu-id="45972-221">A *local_constant_declaration* declares one or more local constants.</span></span>

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

<span data-ttu-id="45972-222">Die *Typ* von einem *Local_constant_declaration* gibt den Typ der Konstanten, die durch die Deklaration eingeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="45972-223">Der Typ folgt eine Liste der *Constant_declarator*s, von denen jede eine neue Konstante führt.</span><span class="sxs-lookup"><span data-stu-id="45972-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="45972-224">Ein *Constant_declarator* besteht aus einer *Bezeichner* , Namen der Konstanten gefolgt von einer "`=`" token, gefolgt von einer *Constant_expression* ([ Konstante Ausdrücke](expressions.md#constant-expressions)), die den Wert der Konstanten bietet.</span><span class="sxs-lookup"><span data-stu-id="45972-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="45972-225">Die *Typ* und *Constant_expression* der Deklaration von lokalen Konstanten muss, gelten dieselben Regeln wie für eine Deklaration einer Konstante Member ([Konstanten](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="45972-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="45972-226">Der Wert einer lokalen Konstanten wird ermittelt, in einem Ausdruck mit einem *Simple_name* ([einfache Namen](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="45972-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="45972-227">Der Gültigkeitsbereich der lokale Konstante ist, den Block in dem die Deklaration erfolgt.</span><span class="sxs-lookup"><span data-stu-id="45972-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="45972-228">Es ist ein Fehler auf Sie verweisen auf eine lokale Konstante in der Lage, Text vor dessen *Constant_declarator*.</span><span class="sxs-lookup"><span data-stu-id="45972-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="45972-229">Innerhalb des Bereichs einer lokalen Konstanten ist es ein Fehler während der Kompilierung eine andere lokale Variable oder Konstante mit dem gleichen Namen deklariert.</span><span class="sxs-lookup"><span data-stu-id="45972-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="45972-230">Deklaration von lokale Konstante, die mehrere Konstanten deklariert entspricht mehreren Deklarationen der einzelnen Konstanten desselben Typs.</span><span class="sxs-lookup"><span data-stu-id="45972-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="45972-231">Ausdrucksanweisungen</span><span class="sxs-lookup"><span data-stu-id="45972-231">Expression statements</span></span>

<span data-ttu-id="45972-232">Ein *Expression_statement* wertet einen angegebenen Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="45972-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="45972-233">Der Wert berechnet werden, durch den Ausdruck wird ggf. verworfen.</span><span class="sxs-lookup"><span data-stu-id="45972-233">The value computed by the expression, if any, is discarded.</span></span>

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

<span data-ttu-id="45972-234">Nicht alle Ausdrücke werden als Anweisungen zulässig.</span><span class="sxs-lookup"><span data-stu-id="45972-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="45972-235">Insbesondere Ausdrücke wie z. B. `x + y` und `x == 1` , die lediglich einen Wert (die werden verworfen) berechnen, werden als Anweisungen nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="45972-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="45972-236">Ausführung einer *Expression_statement* der enthaltenen Ausdruck ausgewertet, und klicken Sie dann überträgt die Steuerung an den Endpunkt, der die *Expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="45972-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="45972-237">Der Endpunkt, der eine *Expression_statement* ist erreichbar. wenn das *Expression_statement* erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="45972-238">Auswahlanweisungen</span><span class="sxs-lookup"><span data-stu-id="45972-238">Selection statements</span></span>

<span data-ttu-id="45972-239">Auswahlanweisungen wählen Sie eine Anzahl von möglichen Anweisungen für die Ausführung anhand des Werts eines Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="45972-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="45972-240">Die bei Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-240">The if statement</span></span>

<span data-ttu-id="45972-241">Die `if` -Anweisung wählt eine Anweisung für die Ausführung anhand des Werts eines booleschen Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="45972-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="45972-242">Ein `else` Teil bezieht sich auf dem lexikalisch nächsten vorausgehenden `if` , durch die Syntax zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="45972-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="45972-243">Daher eine `if` -Anweisung der Form</span><span class="sxs-lookup"><span data-stu-id="45972-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="45972-244">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="45972-244">is equivalent to</span></span>
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

<span data-ttu-id="45972-245">Ein `if` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="45972-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="45972-246">Die *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)) ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="45972-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="45972-247">Wenn der boolesche Ausdruck ergibt `true`, wird die Steuerung an die erste eingebettete Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="45972-248">Wenn und Steuerung der Endpunkt, der diese Anweisung erreicht, wird die Steuerung an den Endpunkt der `if` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="45972-249">Wenn der boolesche Ausdruck ergibt `false` und, wenn ein `else` Teil vorhanden ist, wird die Steuerung an die zweite, eingebettete Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="45972-250">Wenn und Steuerung der Endpunkt, der diese Anweisung erreicht, wird die Steuerung an den Endpunkt der `if` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="45972-251">Wenn der boolesche Ausdruck ergibt `false` und, wenn ein `else` Teil ist nicht vorhanden, wird die Steuerung an den Endpunkt, der die `if` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="45972-252">Die erste eingebettete Anweisung, der ein `if` Anweisung erreichbar ist. wenn die `if` -Anweisung erreichbar ist und der boolesche Ausdruck verfügt nicht über den konstanten Wert `false`.</span><span class="sxs-lookup"><span data-stu-id="45972-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="45972-253">Die zweite eingebettete Anweisung, die von einer `if` -Anweisung, falls vorhanden, ist erreichbar Wenn die `if` -Anweisung erreichbar ist und der boolesche Ausdruck verfügt nicht über den konstanten Wert `true`.</span><span class="sxs-lookup"><span data-stu-id="45972-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="45972-254">Der Endpunkt, der eine `if` -Anweisung ist erreichbar, wenn der Endpunkt, der mindestens eines der eingebetteten Anweisungen erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="45972-255">Darüber hinaus zeigen Sie das Ende des ein `if` Anweisung ohne `else` Teil erreichbar ist. wenn die `if` -Anweisung erreichbar ist und der boolesche Ausdruck verfügt nicht über den konstanten Wert `true`.</span><span class="sxs-lookup"><span data-stu-id="45972-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="45972-256">Der Switch-Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-256">The switch statement</span></span>

<span data-ttu-id="45972-257">Die Switch-Anweisung wählt eine Anweisungsliste mit einer zugeordneten Switch-Bezeichnung, die den Wert des Switch-Ausdrucks entspricht, für die Ausführung.</span><span class="sxs-lookup"><span data-stu-id="45972-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

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

<span data-ttu-id="45972-258">Ein *Switch_statement* besteht aus dem Schlüsselwort `switch`, gefolgt von einem Ausdruck in Klammern (den Switch-Ausdruck bezeichnet), gefolgt von einem *Switch_block*.</span><span class="sxs-lookup"><span data-stu-id="45972-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="45972-259">Die *Switch_block* besteht aus 0 (null) oder mehreren *Switch_section*s, die in geschweifte Klammern eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="45972-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="45972-260">Jede *Switch_section* besteht aus einem oder mehreren *Switch_label*s gefolgt von einem *Statement_list* ([Anweisung listet](statements.md#statement-lists)).</span><span class="sxs-lookup"><span data-stu-id="45972-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="45972-261">Die ***für Typ*** von einem `switch` Anweisung wird hergestellt, indem der Switch-Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="45972-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="45972-262">Wenn der Typ des Switch-Ausdrucks ist `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, oder ein *Enum_type*, oder wenn es für einen dieser Typen einen nullable-Typ ist, dann wird die Steuerung zu geben, der die `switch` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="45972-263">Andernfalls genau eine benutzerdefinierte implizite Konvertierung ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)) muss vorhanden sein, von dem Typ des Switch-Ausdrucks auf einen der folgenden möglichen Typen, die steuern: `sbyte`, `byte`, `short` , `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, oder einen nullable-Typ, der einer dieser Typen entspricht.</span><span class="sxs-lookup"><span data-stu-id="45972-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="45972-264">Andernfalls tritt auf, wenn solch eine implizite Konvertierung vorhanden ist oder wenn mehr als eine implizite Konvertierung vorhanden ist, ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="45972-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="45972-265">Der Konstante Ausdruck jeder `case` Bezeichnung muss einen Wert, der implizit konvertiert werden deuten ([implizite Konvertierungen](conversions.md#implicit-conversions)) auf der vorherrschende Datentyp der `switch` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="45972-266">Ein Fehler während der Kompilierung tritt auf, wenn zwei oder mehr `case` Bezeichnungen in der gleichen `switch` Anweisung geben Sie den konstanten Wert.</span><span class="sxs-lookup"><span data-stu-id="45972-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="45972-267">Es kann höchstens eine `default` Bezeichnung in einer Switch-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="45972-268">Ein `switch` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="45972-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="45972-269">Der Switch-Ausdruck wird ausgewertet und in der vorherrschende Datentyp konvertiert.</span><span class="sxs-lookup"><span data-stu-id="45972-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="45972-270">Wenn eine der Konstanten in angegeben ein `case` Bezeichnung in der gleichen `switch` -Anweisung ist gleich dem Wert des Switch-Ausdrucks, die Steuerung an die Anweisungsliste, die nach der entsprechenden `case` Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="45972-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="45972-271">Wenn keine der Konstanten in angegebenen `case` Bezeichnungen in der gleichen `switch` -Anweisung ist gleich dem Wert des Switch-Ausdrucks, und wenn ein `default` Bezeichnung vorhanden ist, wird die Steuerung an die Anweisung Liste nach der `default` Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="45972-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="45972-272">Wenn keine der Konstanten in angegebenen `case` Bezeichnungen in der gleichen `switch` -Anweisung ist gleich dem Wert des Switch-Ausdrucks, und wenn keine `default` Bezeichnung vorhanden ist, wird die Steuerung an den Endpunkt der `switch` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="45972-273">Wenn der Endpunkt der Anweisungsliste einen Switch-Abschnitt erreichbar ist, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="45972-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="45972-274">Dies wird wie in der Regel "keine fortfahren" bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="45972-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="45972-275">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="45972-275">The example</span></span>
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
<span data-ttu-id="45972-276">ist gültig, da keine Switch-Abschnitt auf einen erreichbaren Endpunkt enthält.</span><span class="sxs-lookup"><span data-stu-id="45972-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="45972-277">Im Gegensatz zu C und C++ Ausführung ein Switch-Abschnitt darf nicht "Fortfahren" zu den nächsten Switch-Abschnitt im Beispiel</span><span class="sxs-lookup"><span data-stu-id="45972-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
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
<span data-ttu-id="45972-278">führt zu einem Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="45972-278">results in a compile-time error.</span></span> <span data-ttu-id="45972-279">Bei Ausführung des ein Switch-Abschnitt ist durch die Ausführung von einem anderen Switch-Abschnitt, eine explizite befolgt werden `goto case` oder `goto default` -Anweisung verwendet werden muss:</span><span class="sxs-lookup"><span data-stu-id="45972-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
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

<span data-ttu-id="45972-280">Mehrere Bezeichnungen dürfen sich einem *Switch_section*.</span><span class="sxs-lookup"><span data-stu-id="45972-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="45972-281">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="45972-281">The example</span></span>
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
<span data-ttu-id="45972-282">ist gültig.</span><span class="sxs-lookup"><span data-stu-id="45972-282">is valid.</span></span> <span data-ttu-id="45972-283">Im Beispiel wird die Regel "keine fortfahren" nicht verletzt, da die Bezeichnungen `case 2:` und `default:` sind Teil derselben *Switch_section*.</span><span class="sxs-lookup"><span data-stu-id="45972-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="45972-284">Die Regel "keine fortfahren" wird verhindert, dass eine allgemeine Klasse von Fehlern, die in C und C++ auftreten, wenn `break` Anweisungen versehentlich ausgelassen werden.</span><span class="sxs-lookup"><span data-stu-id="45972-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="45972-285">Darüber hinaus aufgrund dieser Regel, die Switch-Abschnitte von einem `switch` Anweisung willkürlich neu angeordnet werden kann ohne Auswirkungen auf das Verhalten der Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="45972-286">Z. B. die Abschnitte der `switch` obigen Anweisung ohne Auswirkungen auf das Verhalten der Anweisung rückgängig gemacht werden kann:</span><span class="sxs-lookup"><span data-stu-id="45972-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
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

<span data-ttu-id="45972-287">In der Regel die Anweisungsliste, der ein Switch-Abschnitt endet, in einem `break`, `goto case`, oder `goto default` -Anweisung, jedoch Konstrukt, das am Ende der Anweisungsliste unerreichbar ist zulässig.</span><span class="sxs-lookup"><span data-stu-id="45972-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="45972-288">Z. B. eine `while` Anweisung, die von der boolesche Ausdruck gesteuert `true` nie Reichweite seinen Endpunkt bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="45972-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="45972-289">Ebenso eine `throw` oder `return` -Anweisung immer überträgt die Steuerung an anderer Stelle und seinen Endpunkt nicht erreicht.</span><span class="sxs-lookup"><span data-stu-id="45972-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="45972-290">Im folgende Beispiel ist daher gültig:</span><span class="sxs-lookup"><span data-stu-id="45972-290">Thus, the following example is valid:</span></span>
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

<span data-ttu-id="45972-291">Der vorherrschende Datentyp eine `switch` Anweisung kann es sich um den Typ `string`.</span><span class="sxs-lookup"><span data-stu-id="45972-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="45972-292">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="45972-292">For example:</span></span>
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

<span data-ttu-id="45972-293">Wie die Gleichheitsoperatoren ([Zeichenfolge Gleichheitsoperatoren](expressions.md#string-equality-operators)), wird die `switch` -Anweisung wird die Groß-/Kleinschreibung beachtet und einen bestimmten Switch-Abschnitt nur ausgeführt, wenn die Zeichenfolge des Switch-Ausdrucks identisch eine `case` Bezeichnung Konstante.</span><span class="sxs-lookup"><span data-stu-id="45972-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="45972-294">Wenn der Typ den leitenden eine `switch` -Anweisung ist `string`, den Wert `null` als Konstante Case-Bezeichnung zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="45972-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="45972-295">Die *Statement_list*s eine *Switch_block* darf deklarationsanweisungen ([deklarationsanweisungen](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="45972-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="45972-296">Im Rahmen einer lokalen Variable oder Konstante wird, die in einer Switch-Block deklariert der Switch-Block.</span><span class="sxs-lookup"><span data-stu-id="45972-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="45972-297">Die Anweisungsliste, der einem bestimmten Switch-Abschnitt ist erreichbar. wenn die `switch` -Anweisung erreichbar ist und mindestens eine der folgenden gilt:</span><span class="sxs-lookup"><span data-stu-id="45972-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="45972-298">Der Switch-Ausdruck ist ein nicht konstanter Wert.</span><span class="sxs-lookup"><span data-stu-id="45972-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="45972-299">Der Switch-Ausdruck ist ein konstanter Wert, der entspricht einem `case` Bezeichnung im Switch-Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="45972-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="45972-300">Der Switch-Ausdruck ist ein konstanter Wert, der bundesstaatscode keiner entspricht `case` Bezeichnung und Switch-Abschnitt enthält die `default` Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="45972-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="45972-301">Eine Switch-Bezeichnung der Switch-Abschnitt verweist auf einen erreichbaren `goto case` oder `goto default` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="45972-302">Der Endpunkt, der eine `switch` -Anweisung ist erreichbar, wenn mindestens eine der folgenden "true" ist:</span><span class="sxs-lookup"><span data-stu-id="45972-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="45972-303">Die `switch` -Anweisung enthält einen erreichbaren `break` -Anweisung, die beendet die `switch` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="45972-304">Die `switch` -Anweisung erreichbar ist, der Switch-Ausdruck ist ein nicht konstanter Wert und keine `default` -Bezeichnung ist vorhanden.</span><span class="sxs-lookup"><span data-stu-id="45972-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="45972-305">Die `switch` -Anweisung erreichbar ist, der Switch-Ausdruck ist ein konstanter Wert, der bundesstaatscode keiner entspricht `case` Bezeichnung und ohne `default` -Bezeichnung ist vorhanden.</span><span class="sxs-lookup"><span data-stu-id="45972-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="45972-306">Iterationsanweisungen</span><span class="sxs-lookup"><span data-stu-id="45972-306">Iteration statements</span></span>

<span data-ttu-id="45972-307">Iterationsanweisungen hat eine eingebettete Anweisung wiederholt ausführen.</span><span class="sxs-lookup"><span data-stu-id="45972-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="45972-308">Die while-Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-308">The while statement</span></span>

<span data-ttu-id="45972-309">Die `while` Anweisung führt bedingt eine eingebettete Anweisung nullmal oder häufiger abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="45972-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="45972-310">Ein `while` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="45972-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="45972-311">Die *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)) ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="45972-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="45972-312">Wenn der boolesche Ausdruck ergibt `true`, wird die Steuerung an die eingebettete Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="45972-313">Wenn und die Steuerung der Endpunkt, der die eingebettete Anweisung erreicht (möglicherweise durch Ausführen von einer `continue` Anweisung), wird die Steuerung an den Anfang der `while` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="45972-314">Wenn der boolesche Ausdruck ergibt `false`, die Steuerung an den Endpunkt der `while` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="45972-315">In der die eingebettete Anweisung eine `while` -Anweisung eine `break` Anweisung ([die Break-Anweisung](statements.md#the-break-statement)) kann verwendet werden, die Steuerung an den Endpunkt übertragen die `while` (also mit der Endung Iteration, der die eingebettete Anweisung -Anweisung), und ein `continue` Anweisung ([die Continue-Anweisung](statements.md#the-continue-statement)) kann verwendet werden, um die Steuerung an den Endpunkt, der die eingebettete Anweisung zu übertragen (daher Ausführen einer anderen Iteration von der `while` Anweisung).</span><span class="sxs-lookup"><span data-stu-id="45972-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="45972-316">Die eingebettete Anweisung, der eine `while` Anweisung erreichbar ist. wenn die `while` -Anweisung erreichbar ist und der boolesche Ausdruck verfügt nicht über den konstanten Wert `false`.</span><span class="sxs-lookup"><span data-stu-id="45972-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="45972-317">Der Endpunkt, der eine `while` -Anweisung ist erreichbar, wenn mindestens eine der folgenden "true" ist:</span><span class="sxs-lookup"><span data-stu-id="45972-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="45972-318">Die `while` -Anweisung enthält einen erreichbaren `break` -Anweisung, die beendet die `while` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="45972-319">Die `while` -Anweisung erreichbar ist und der boolesche Ausdruck verfügt nicht über den konstanten Wert `true`.</span><span class="sxs-lookup"><span data-stu-id="45972-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="45972-320">Die Do-Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-320">The do statement</span></span>

<span data-ttu-id="45972-321">Die `do` Anweisung führt bedingt eine eingebettete Anweisung einmal oder mehrmals.</span><span class="sxs-lookup"><span data-stu-id="45972-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="45972-322">Ein `do` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="45972-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="45972-323">Steuerung wird an die eingebettete Anweisung übergeben.</span><span class="sxs-lookup"><span data-stu-id="45972-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="45972-324">Wenn und die Steuerung der Endpunkt, der die eingebettete Anweisung erreicht (möglicherweise durch Ausführen von einer `continue` Anweisung), wird die *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)) ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="45972-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="45972-325">Wenn der boolesche Ausdruck ergibt `true`, wird die Steuerung an den Anfang der `do` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="45972-326">Andernfalls wird die Steuerung an den Endpunkt, der die `do` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="45972-327">In der die eingebettete Anweisung eine `do` -Anweisung eine `break` Anweisung ([die Break-Anweisung](statements.md#the-break-statement)) kann verwendet werden, die Steuerung an den Endpunkt übertragen die `do` (also mit der Endung Iteration, der die eingebettete Anweisung -Anweisung), und ein `continue` Anweisung ([die Continue-Anweisung](statements.md#the-continue-statement)) kann verwendet werden, um die Steuerung an den Endpunkt, der die eingebettete Anweisung zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="45972-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="45972-328">Die eingebettete Anweisung des eine `do` Anweisung erreichbar ist. wenn die `do` -Anweisung erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="45972-329">Der Endpunkt, der eine `do` -Anweisung ist erreichbar, wenn mindestens eine der folgenden "true" ist:</span><span class="sxs-lookup"><span data-stu-id="45972-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="45972-330">Die `do` -Anweisung enthält einen erreichbaren `break` -Anweisung, die beendet die `do` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="45972-331">Der Endpunkt, der die eingebettete Anweisung erreichbar ist und der boolesche Ausdruck verfügt nicht über den konstanten Wert `true`.</span><span class="sxs-lookup"><span data-stu-id="45972-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="45972-332">Die für die Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-332">The for statement</span></span>

<span data-ttu-id="45972-333">Die `for` Anweisung wertet eine Reihe von Initialisierungsausdrücken und dann, solange eine Bedingung "true" wird wiederholt führt eine eingebettete Anweisung und eine Sequenz von Ausdrücken für Iteration ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="45972-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

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

<span data-ttu-id="45972-334">Die *For_initializer*, falls vorhanden, besteht entweder aus einem *Local_variable_declaration* ([lokale Variablendeklarationen](statements.md#local-variable-declarations)) oder eine Liste von *Statement_ Ausdruck*s ([ausdrucksanweisungen](statements.md#expression-statements)) durch Kommas getrennt.</span><span class="sxs-lookup"><span data-stu-id="45972-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="45972-335">Der Bereich, der eine lokale Variable deklariert, indem eine *For_initializer* beginnt bei der *Local_variable_declarator* für die Variable und reicht bis zum Ende der embedded-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="45972-336">Der Gültigkeitsbereich umfasst die *For_condition* und *For_iterator*.</span><span class="sxs-lookup"><span data-stu-id="45972-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="45972-337">Die *For_condition*, falls vorhanden, muss ein *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="45972-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="45972-338">Die *For_iterator*, falls vorhanden, besteht aus einer Liste von *Statement_expression*s ([ausdrucksanweisungen](statements.md#expression-statements)) durch Kommas getrennt.</span><span class="sxs-lookup"><span data-stu-id="45972-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="45972-339">Eine for-Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="45972-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="45972-340">Wenn eine *For_initializer* ist vorhanden, wird die Variable Initialisierer oder Anweisungsausdrücke ausgeführt werden, in der Reihenfolge, die sie geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="45972-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="45972-341">Dieser Schritt ist nur einmal ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-341">This step is only performed once.</span></span>
*  <span data-ttu-id="45972-342">Wenn eine *For_condition* vorhanden ist, wird ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="45972-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="45972-343">Wenn die *For_condition* nicht vorhanden ist oder wenn das Ergebnis der Auswertung `true`, wird die Steuerung an die eingebettete Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="45972-344">Wenn und die Steuerung der Endpunkt, der die eingebettete Anweisung erreicht (möglicherweise durch Ausführen von eine `continue` Anweisung), die Ausdrücke der *For_iterator*, sofern vorhanden, werden in der Reihenfolge ausgewertet, und klicken Sie dann eine andere Iteration ist durchgeführt, beginnend mit der Auswertung der *For_condition* im vorherigen Schritt.</span><span class="sxs-lookup"><span data-stu-id="45972-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="45972-345">Wenn die *For_condition* vorhanden ist und die Auswertung ergibt `false`, die Steuerung an den Endpunkt der `for` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="45972-346">In der die eingebettete Anweisung eine `for` -Anweisung eine `break` Anweisung ([die Break-Anweisung](statements.md#the-break-statement)) kann verwendet werden, die Steuerung an den Endpunkt übertragen die `for` (also mit der Endung Iteration, der die eingebettete Anweisung -Anweisung), und ein `continue` Anweisung ([die Continue-Anweisung](statements.md#the-continue-statement)) kann verwendet werden, um die Steuerung an den Endpunkt, der die eingebettete Anweisung zu übertragen (somit Ausführen der *For_iterator* und Ausführen einer anderen Iteration, der die `for` -Anweisung, beginnend mit der *For_condition*).</span><span class="sxs-lookup"><span data-stu-id="45972-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="45972-347">Die eingebettete Anweisung von einem `for` -Anweisung ist erreichbar, wenn eine der folgenden Aussagen zutrifft:</span><span class="sxs-lookup"><span data-stu-id="45972-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="45972-348">Die `for` -Anweisung erreichbar ist und es wird kein *For_condition* vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="45972-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="45972-349">Die `for` -Anweisung erreichbar ist und ein *For_condition* vorhanden ist und keine den konstanten Wert `false`.</span><span class="sxs-lookup"><span data-stu-id="45972-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="45972-350">Der Endpunkt, der eine `for` -Anweisung ist erreichbar, wenn mindestens eine der folgenden "true" ist:</span><span class="sxs-lookup"><span data-stu-id="45972-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="45972-351">Die `for` -Anweisung enthält einen erreichbaren `break` -Anweisung, die beendet die `for` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="45972-352">Die `for` -Anweisung erreichbar ist und ein *For_condition* vorhanden ist und keine den konstanten Wert `true`.</span><span class="sxs-lookup"><span data-stu-id="45972-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="45972-353">Die Foreach-Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-353">The foreach statement</span></span>

<span data-ttu-id="45972-354">Die `foreach` Anweisung listet die Elemente einer Auflistung und führt eine eingebettete Anweisung für jedes Element der Auflistung.</span><span class="sxs-lookup"><span data-stu-id="45972-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="45972-355">Die *Typ* und *Bezeichner* von einer `foreach` Anweisung deklariert den ***Iterationsvariable*** der Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="45972-356">Wenn die `var` Bezeichner angegeben wird, als die *Local_variable_type*, und kein Typ mit dem Namen `var` ist im Gültigkeitsbereich der Iterationsvariablen gilt eine ***implizit typisierte Iterationsvariable***, und sein Typ wird erstellt, um den Elementtyp des werden die `foreach` Anweisung, wie unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="45972-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="45972-357">Die Iterationsvariable entspricht einer schreibgeschützten lokalen Variablen mit einem Bereich, der über die eingebettete Anweisung erweitert.</span><span class="sxs-lookup"><span data-stu-id="45972-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="45972-358">Während der Ausführung einer `foreach` -Anweisung, die Iterationsvariable stellt das Auflistungselement für die Iteration gegenwärtig ausgeführt wird wird.</span><span class="sxs-lookup"><span data-stu-id="45972-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="45972-359">Ein Fehler während der Kompilierung tritt auf, wenn die eingebettete Anweisung versucht, so ändern Sie die Iterationsvariable (per Zuweisung oder `++` und `--` Operatoren) oder übergeben Sie die Iterationsvariable als einen `ref` oder `out` Parameter.</span><span class="sxs-lookup"><span data-stu-id="45972-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="45972-360">Im folgenden wird aus Gründen der Übersichtlichkeit `IEnumerable`, `IEnumerator`, `IEnumerable<T>` und `IEnumerator<T>` finden Sie in die entsprechenden Typen in den Namespaces `System.Collections` und `System.Collections.Generic`.</span><span class="sxs-lookup"><span data-stu-id="45972-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="45972-361">Die Verarbeitung während der Kompilierung einer Foreach-Anweisung zuerst bestimmt das ***Auflistungstyp***, ***Enumeratortyp*** und ***Elementtyp*** des Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="45972-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="45972-362">Diese Ermittlung wird wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="45972-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="45972-363">Wenn der Typ `X` von *Ausdruck* Arraytyp ist, dann gibt es eine implizite verweiskonvertierung von `X` auf die `IEnumerable` Schnittstelle (da `System.Array` implementiert diese Schnittstelle).</span><span class="sxs-lookup"><span data-stu-id="45972-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="45972-364">Die ***Auflistungstyp*** ist die `IEnumerable` -Schnittstelle, die ***Enumeratortyp*** ist die `IEnumerator` Schnittstelle und die ***Elementtyp*** ist der Elementtyp des der Arraytyp `X`.</span><span class="sxs-lookup"><span data-stu-id="45972-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="45972-365">Wenn der Typ `X` von *Ausdruck* ist `dynamic` dann gibt es eine implizite Konvertierung von *Ausdruck* auf die `IEnumerable` Schnittstelle ([implizite Dynamic Konvertierungen](conversions.md#implicit-dynamic-conversions)).</span><span class="sxs-lookup"><span data-stu-id="45972-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="45972-366">Die ***Auflistungstyp*** ist die `IEnumerable` Schnittstelle und die ***Enumeratortyp*** ist die `IEnumerator` Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="45972-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="45972-367">Wenn der `var` Bezeichner angegeben wird, als die *Local_variable_type* die ***Elementtyp*** ist `dynamic`, andernfalls ist es `object`.</span><span class="sxs-lookup"><span data-stu-id="45972-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="45972-368">Andernfalls bestimmt, ob der Typ `X` verfügt über eine entsprechende `GetEnumerator` Methode:</span><span class="sxs-lookup"><span data-stu-id="45972-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="45972-369">Führen Sie die Suche nach Membern des Typs `X` mit dem Bezeichner `GetEnumerator` und keine Typargumente.</span><span class="sxs-lookup"><span data-stu-id="45972-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="45972-370">Wenn die Membersuche keine Übereinstimmung ergibt oder eine Mehrdeutigkeit erzeugt oder erzeugt eine Übereinstimmung, die keine Methodengruppe ist, überprüfen Sie für eine enumerable-Schnittstelle, wie unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="45972-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="45972-371">Es wird empfohlen, dass eine Warnung ausgegeben werden, wenn die Suche nach Membern erzeugt alles außer einer Methodengruppe oder keine Übereinstimmung.</span><span class="sxs-lookup"><span data-stu-id="45972-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="45972-372">Führen Sie die Auflösung von funktionsüberladungen mit der resultierenden Methodengruppe und einer leeren Argumentliste.</span><span class="sxs-lookup"><span data-stu-id="45972-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="45972-373">Wenn keine entsprechenden Methoden Auflösung von funktionsüberladungen ergibt, führt zu einer Mehrdeutigkeit und führt zu einer einzelnen bewährte Methode, aber diese Methode entweder statisch oder nicht öffentlich ist, überprüfen Sie für eine enumerable-Schnittstelle, ist wie unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="45972-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="45972-374">Es wird empfohlen, dass eine Warnung ausgegeben werden, wenn die Auflösung von funktionsüberladungen alles mit Ausnahme von eine eindeutige öffentliche Instanzmethode oder keine anwendbaren Methoden erzeugt.</span><span class="sxs-lookup"><span data-stu-id="45972-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="45972-375">Wenn der Rückgabetyp `E` von der `GetEnumerator` Methode ist keine Klasse, Struktur oder Schnittstelle-Typ, ein Fehler erzeugt wird und keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="45972-376">Suche nach Membern erfolgt auf `E` mit dem Bezeichner `Current` und keine Typargumente.</span><span class="sxs-lookup"><span data-stu-id="45972-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="45972-377">Wenn die Suche nach Membern keine Übereinstimmung erzeugt, das Ergebnis ein Fehler ist oder das Ergebnis ist nichts außer eine öffentliche Instanz-Eigenschaft, die beim Lesen zu können, ein Fehler erzeugt wird und keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="45972-378">Suche nach Membern erfolgt auf `E` mit dem Bezeichner `MoveNext` und keine Typargumente.</span><span class="sxs-lookup"><span data-stu-id="45972-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="45972-379">Wenn die Suche nach Membern keine Übereinstimmung erzeugt, das Ergebnis ein Fehler ist oder das Ergebnis alles außer einer Methodengruppe ist, ein Fehler erzeugt wird und keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="45972-380">Überladungsauflösung erfolgt auf die Methodengruppe mit einer leeren Argumentliste.</span><span class="sxs-lookup"><span data-stu-id="45972-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="45972-381">Wenn Überladung Lösung führt keine entsprechenden Methoden, führt zu einer Mehrdeutigkeit oder Ergebnisse in einer einzelnen bewährte Methode, aber diese Methode entweder statisch oder nicht öffentlich ist oder der Rückgabetyp nicht ist `bool`, ein Fehler erzeugt wird und keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="45972-382">Die ***Auflistungstyp*** ist `X`, ***Enumeratortyp*** ist `E`, und die ***Elementtyp*** ist der Typ des der `Current` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="45972-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="45972-383">Überprüfen Sie andernfalls für eine enumerable-Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="45972-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="45972-384">If für alle Typen `Ti` für den es eine implizite Konvertierung von `X` zu `IEnumerable<Ti>`, es ist ein eindeutiger Typ `T` so, dass `T` ist nicht `dynamic` und für alle anderen `Ti` gibt es ein implizite Konvertierung von `IEnumerable<T>` zu `IEnumerable<Ti>`, und klicken Sie dann die ***Auflistungstyp*** ist die Schnittstelle `IEnumerable<T>`, ***Enumeratortyp*** ist die Schnittstelle `IEnumerator<T>`, und die ***Elementtyp*** ist `T`.</span><span class="sxs-lookup"><span data-stu-id="45972-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="45972-385">Wenn mehr als ein solcher Typ vorhanden ist, andernfalls `T`, klicken Sie dann ein Fehler erzeugt wird und keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="45972-386">Andernfalls, wenn es eine implizite Konvertierung von `X` auf die `System.Collections.IEnumerable` -Schnittstelle, die ***Auflistungstyp*** ist diese Schnittstelle, die ***Enumeratortyp*** ist die Schnittstelle `System.Collections.IEnumerator`, und die ***Elementtyp*** ist `object`.</span><span class="sxs-lookup"><span data-stu-id="45972-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="45972-387">Andernfalls wird ein Fehler wird ausgelöst, und keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="45972-388">Die oben genannten Schritte, wenn erfolgreich, eindeutig zu erzeugen einen Auflistungstyp `C`, Enumeratortyp `E` und Elementtyp `T`.</span><span class="sxs-lookup"><span data-stu-id="45972-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="45972-389">Eine Foreach-Anweisung der form</span><span class="sxs-lookup"><span data-stu-id="45972-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="45972-390">Klicken Sie dann auf Erweitert:</span><span class="sxs-lookup"><span data-stu-id="45972-390">is then expanded to:</span></span>
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

<span data-ttu-id="45972-391">Die Variable `e` ist nicht sichtbar bzw. zugegriffen werden kann, auf den Ausdruck `x` oder die eingebettete Anweisung oder jeden anderen Quellcode des Programms.</span><span class="sxs-lookup"><span data-stu-id="45972-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="45972-392">Die Variable `v` in die eingebettete Anweisung schreibgeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="45972-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="45972-393">Wenn keine explizite Konvertierung vorhanden ist ([explizite Konvertierungen](conversions.md#explicit-conversions)) von `T` (der Typ des Elements), `V` (die *Local_variable_type* in der Foreach-Anweisung), ein Fehler wird ausgelöst, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="45972-394">Wenn `x` hat den Wert `null`, `System.NullReferenceException` wird zur Laufzeit ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="45972-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="45972-395">Eine Implementierung ist auf eine gegebenen Foreach-Anweisung auf andere Weise, z. B. zur Verbesserung der Leistung, implementiert zulässig, solange das Verhalten konsistent mit der Erweiterung der oben genannten ist.</span><span class="sxs-lookup"><span data-stu-id="45972-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="45972-396">Die Platzierung von `v` innerhalb der While-Schleife ist wichtig, dass Sie wie sie eine anonyme Funktion, die im erfasst wird die *Embedded_statement*.</span><span class="sxs-lookup"><span data-stu-id="45972-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="45972-397">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="45972-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="45972-398">Wenn `v` deklariert wurde, außerhalb der While-Schleife wird für alle Iterationen und dessen Wert nach gemeinsam genutzt werden die für Schleife den endgültigen Wert wäre `13`, d.h. welche der Aufruf von `f` druckt.</span><span class="sxs-lookup"><span data-stu-id="45972-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="45972-399">Stattdessen, da jede Iteration eine eigene Variable weist `v`, die von erfasst `f` in der ersten Iteration weiterhin den Wert enthalten soll `7`, dies ist, was gedruckt werden.</span><span class="sxs-lookup"><span data-stu-id="45972-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="45972-400">(Hinweis: frühere Versionen von c# deklariert `v` außerhalb der While-Schleife.)</span><span class="sxs-lookup"><span data-stu-id="45972-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="45972-401">Der Hauptteil der Block wird schließlich anhand der folgenden Schritte erstellt:</span><span class="sxs-lookup"><span data-stu-id="45972-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="45972-402">Es ist eine implizite Konvertierung von `E` auf die `System.IDisposable` Schnittstelle ist, klicken Sie dann</span><span class="sxs-lookup"><span data-stu-id="45972-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="45972-403">Wenn `E` ist ein NULL-Werte wird die Klausel wird schließlich erweitert, um die semantischen Darstellung:</span><span class="sxs-lookup"><span data-stu-id="45972-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="45972-404">Andernfalls die Klausel wird schließlich erweitert, um die semantischen Darstellung:</span><span class="sxs-lookup"><span data-stu-id="45972-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="45972-405">mit der Ausnahme, dass bei `E` ist ein Werttyp oder ein Typparameter instanziiert, um einen Werttyp handelt, und klicken Sie dann auf die Umwandlung von `e` zu `System.IDisposable` verursacht keine Boxing auftreten.</span><span class="sxs-lookup"><span data-stu-id="45972-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="45972-406">Andernfalls gilt: Wenn `E` einen versiegelten Typ, der zum Schluss wird die Klausel zu einem leeren Block erweitert:</span><span class="sxs-lookup"><span data-stu-id="45972-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="45972-407">Andernfalls, die schließlich-Klausel erweitert:</span><span class="sxs-lookup"><span data-stu-id="45972-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="45972-408">Die lokale Variable `d` ist nicht sichtbar bzw. Benutzercode zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="45972-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="45972-409">Es vor allem keine Konflikte mit einer anderen Variablen, deren Bereich umfasst, die finally-block.</span><span class="sxs-lookup"><span data-stu-id="45972-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="45972-410">Die Reihenfolge, in der `foreach` durchläuft die Elemente eines Arrays, lautet wie folgt: für eindimensionale Arrays von Elementen in aufsteigender Indexreihenfolge durchlaufen werden, beginnend mit dem Index `0` und endend mit Index `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="45972-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="45972-411">Für mehrdimensionale Arrays werden Elemente durchlaufen, sodass die Indizes von die Dimension ganz rechts erhöhte zuerst, dann weiter linken Dimension, und so weiter auf der linken Seite sind.</span><span class="sxs-lookup"><span data-stu-id="45972-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="45972-412">Das folgende Beispiel gibt jeden Wert in ein zweidimensionales Array, in der Reihenfolge der Elemente:</span><span class="sxs-lookup"><span data-stu-id="45972-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
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
<span data-ttu-id="45972-413">Die Ausgabe lautet wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="45972-413">The output produced is as follows:</span></span>
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="45972-414">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="45972-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="45972-415">Der Typ des `n` wird davon ausgegangen werden `int`, den Elementtyp des `numbers`.</span><span class="sxs-lookup"><span data-stu-id="45972-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="45972-416">Sprunganweisungen</span><span class="sxs-lookup"><span data-stu-id="45972-416">Jump statements</span></span>

<span data-ttu-id="45972-417">Sprunganweisungen übertragen der Steuerung bedingungslos.</span><span class="sxs-lookup"><span data-stu-id="45972-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="45972-418">Der Speicherort, der eine Jump-Anweisung überträgt die Steuerung, wird aufgerufen, die ***Ziel*** der Jump-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="45972-419">Wenn eine Jump-Anweisung innerhalb eines Blocks auftritt und das Ziel dieser Jump-Anweisung außerhalb dieses Blocks ist, der Jump-Anweisung gilt als ***beenden*** des Blocks.</span><span class="sxs-lookup"><span data-stu-id="45972-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="45972-420">Während eine sprunganweisung die Steuerung einen Block übertragen, können sie nie Steuerelement in einem Block übertragen.</span><span class="sxs-lookup"><span data-stu-id="45972-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="45972-421">Sprunganweisungen Ausführung wird durch das Vorhandensein der dazwischen liegenden verkompliziert `try` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="45972-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="45972-422">In Ermangelung eines solchen `try` Anweisungen, die eine Jump-Anweisung überträgt die Steuerung bedingungslos aus der Jump-Anweisung an das Ziel.</span><span class="sxs-lookup"><span data-stu-id="45972-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="45972-423">Bei solchen dazwischen liegenden `try` -Anweisungen, die Ausführung ist komplexer.</span><span class="sxs-lookup"><span data-stu-id="45972-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="45972-424">Wenn der Jump-Anweisung beendet, eine oder mehrere wird `try` Blöcke mit verknüpften `finally` Blöcken Steuerelement anfänglich auf übertragen wird die `finally` Block des innersten `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="45972-425">Wenn und die Steuerung über den Endpunkt erreicht eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block der nächsten einschließenden `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="45972-426">Dieser Prozess wird wiederholt, bis die `finally` Blöcke aller beteiligten `try` Anweisungen ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="45972-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="45972-427">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="45972-427">In the example</span></span>
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
<span data-ttu-id="45972-428">die `finally` zwei zugeordneten Blöcken `try` -Anweisungen werden ausgeführt, bevor die Steuerung an das Ziel der Jump-Anweisung übertragen wird.</span><span class="sxs-lookup"><span data-stu-id="45972-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="45972-429">Die Ausgabe lautet wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="45972-429">The output produced is as follows:</span></span>
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="45972-430">Die Break-Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-430">The break statement</span></span>

<span data-ttu-id="45972-431">Die `break` -Anweisung beendet der nächsten einschließenden `switch`, `while`, `do`, `for`, oder `foreach` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="45972-432">Das Ziel einer `break` Anweisung ist der Endpunkt der nächsten einschließenden `switch`, `while`, `do`, `for`, oder `foreach` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="45972-433">Wenn eine `break` -Anweisung ist nicht eingeschlossen, indem eine `switch`, `while`, `do`, `for`, oder `foreach` -Anweisung ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="45972-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="45972-434">Wenn mehrere `switch`, `while`, `do`, `for`, oder `foreach` Anweisungen geschachtelt sind, eine `break` Aussage gilt nur für die innerste-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="45972-435">Um das Steuerelement über mehrere Verschachtelungsebenen übertragen einer `goto` Anweisung ([Goto-Anweisung](statements.md#the-goto-statement)) muss verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="45972-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="45972-436">Ein `break` Anweisung kann nicht beendet eine `finally` Block ([der Try-Anweisung](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="45972-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="45972-437">Wenn eine `break` -Anweisung befindet sich in eine `finally` blockieren, das Ziel des der `break` -Anweisung muss innerhalb eines Abonnements sein `finally` blockieren; andernfalls ein Kompilierungsfehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="45972-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="45972-438">Ein `break` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="45972-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="45972-439">Wenn die `break` -Anweisung beendet eine oder mehrere `try` Blöcke mit verknüpften `finally` Blöcken Steuerelement anfänglich auf übertragen wird die `finally` Block des innersten `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="45972-440">Wenn und die Steuerung über den Endpunkt erreicht eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block der nächsten einschließenden `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="45972-441">Dieser Prozess wird wiederholt, bis die `finally` Blöcke aller beteiligten `try` Anweisungen ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="45972-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="45972-442">Die Steuerung an das Ziel der `break` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="45972-443">Da eine `break` Anweisung bedingungslos überträgt die Steuerung an anderen Positionen wird der Endpunkt, der eine `break` -Anweisung ist nicht erreichbar.</span><span class="sxs-lookup"><span data-stu-id="45972-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="45972-444">Die Continue-Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-444">The continue statement</span></span>

<span data-ttu-id="45972-445">Die `continue` Anweisung startet eine neue Iteration der nächsten einschließenden `while`, `do`, `for`, oder `foreach` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="45972-446">Das Ziel einer `continue` -Anweisung ist der Endpunkt, der die eingebettete Anweisung der nächsten einschließenden `while`, `do`, `for`, oder `foreach` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="45972-447">Wenn eine `continue` -Anweisung ist nicht eingeschlossen, indem eine `while`, `do`, `for`, oder `foreach` -Anweisung ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="45972-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="45972-448">Wenn mehrere `while`, `do`, `for`, oder `foreach` Anweisungen geschachtelt sind, eine `continue` Aussage gilt nur für die innerste-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="45972-449">Um das Steuerelement über mehrere Verschachtelungsebenen übertragen einer `goto` Anweisung ([Goto-Anweisung](statements.md#the-goto-statement)) muss verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="45972-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="45972-450">Ein `continue` Anweisung kann nicht beendet eine `finally` Block ([der Try-Anweisung](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="45972-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="45972-451">Beim ein `continue` -Anweisung befindet sich in eine `finally` blockieren, das Ziel der `continue` -Anweisung muss innerhalb eines Abonnements sein `finally` blockieren; andernfalls tritt ein Fehler während der Kompilierung auf.</span><span class="sxs-lookup"><span data-stu-id="45972-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="45972-452">Ein `continue` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="45972-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="45972-453">Wenn die `continue` -Anweisung beendet eine oder mehrere `try` Blöcke mit verknüpften `finally` Blöcken Steuerelement anfänglich auf übertragen wird die `finally` Block des innersten `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="45972-454">Wenn und die Steuerung über den Endpunkt erreicht eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block der nächsten einschließenden `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="45972-455">Dieser Prozess wird wiederholt, bis die `finally` Blöcke aller beteiligten `try` Anweisungen ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="45972-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="45972-456">Die Steuerung an das Ziel der `continue` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="45972-457">Da eine `continue` Anweisung bedingungslos überträgt die Steuerung an anderen Positionen wird der Endpunkt, der eine `continue` -Anweisung ist nicht erreichbar.</span><span class="sxs-lookup"><span data-stu-id="45972-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="45972-458">Die goto-Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-458">The goto statement</span></span>

<span data-ttu-id="45972-459">Die `goto` -Anweisung überträgt die Steuerung an eine Anweisung, die von einer Bezeichnung markiert ist.</span><span class="sxs-lookup"><span data-stu-id="45972-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="45972-460">Das Ziel einer `goto` *Bezeichner* ist die bezeichnete Anweisung mit der angegebenen Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="45972-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="45972-461">Wenn eine Bezeichnung mit dem angegebenen Namen in das aktuelle Funktionselement nicht vorhanden ist, oder wenn die `goto` Anweisung ist nicht innerhalb des Bereichs für die Beschriftung, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="45972-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="45972-462">Diese Regel ermöglicht die Verwendung von einem `goto` Anweisung, um das Steuerelement aus einem geschachtelten Bereich, jedoch nicht in einem geschachtelten Bereich übertragen.</span><span class="sxs-lookup"><span data-stu-id="45972-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="45972-463">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="45972-463">In the example</span></span>
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
<span data-ttu-id="45972-464">eine `goto` -Anweisung verwendet, um das Steuerelement aus einem geschachtelten Bereich zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="45972-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="45972-465">Das Ziel einer `goto case` -Anweisung ist der Anweisungsliste im unmittelbar einschließenden `switch` Anweisung ([der Switch-Anweisung](statements.md#the-switch-statement)), enthält eine `case` Bezeichnung mit dem angegebenen konstanten Wert.</span><span class="sxs-lookup"><span data-stu-id="45972-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="45972-466">Wenn die `goto case` -Anweisung ist nicht eingeschlossen, indem eine `switch` -Anweisung, wenn die *Constant_expression* kann nicht implizit konvertiert werden ([implizite Konvertierungen](conversions.md#implicit-conversions)) auf der vorherrschende Datentyp der nächste einschließende `switch` -Anweisung, oder wenn der nächsten einschließenden `switch` Anweisung enthält keine `case` Bezeichnung mit dem angegebenen konstanten Wert ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="45972-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="45972-467">Das Ziel einer `goto default` -Anweisung ist der Anweisungsliste im unmittelbar einschließenden `switch` Anweisung ([der Switch-Anweisung](statements.md#the-switch-statement)), enthält eine `default` Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="45972-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="45972-468">Wenn die `goto default` -Anweisung ist nicht eingeschlossen, indem eine `switch` -Anweisung, oder, wenn der nächsten einschließenden `switch` Anweisung enthält keine `default` beschriften, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="45972-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="45972-469">Ein `goto` Anweisung kann nicht beendet eine `finally` Block ([der Try-Anweisung](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="45972-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="45972-470">Beim eine `goto` -Anweisung befindet sich in eine `finally` blockieren, das Ziel der `goto` -Anweisung muss innerhalb eines Abonnements sein `finally` Block, oder andernfalls ein Kompilierungsfehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="45972-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="45972-471">Ein `goto` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="45972-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="45972-472">Wenn die `goto` -Anweisung beendet eine oder mehrere `try` Blöcke mit verknüpften `finally` Blöcken Steuerelement anfänglich auf übertragen wird die `finally` Block des innersten `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="45972-473">Wenn und die Steuerung über den Endpunkt erreicht eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block der nächsten einschließenden `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="45972-474">Dieser Prozess wird wiederholt, bis die `finally` Blöcke aller beteiligten `try` Anweisungen ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="45972-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="45972-475">Die Steuerung an das Ziel der `goto` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="45972-476">Da eine `goto` Anweisung bedingungslos überträgt die Steuerung an anderen Positionen wird der Endpunkt, der eine `goto` -Anweisung ist nicht erreichbar.</span><span class="sxs-lookup"><span data-stu-id="45972-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="45972-477">Die return-Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-477">The return statement</span></span>

<span data-ttu-id="45972-478">Die `return` -Anweisung übergibt die Kontrolle an den aktuellen Aufrufer der Funktion in der die `return` -Anweisung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="45972-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="45972-479">Ein `return` Anweisung ohne Ausdruck kann verwendet werden, nur in ein Funktionsmember, die keinen Wert, d. h. eine Methode mit dem Ergebnistyp berechnet ([Methodentext](classes.md#method-body)) `void`, `set` Accessor einer Eigenschaft oder Indexer der `add` und `remove` Accessoren der ein Ereignis, ein Instanzkonstruktor, einen statischen Konstruktor oder Destruktor.</span><span class="sxs-lookup"><span data-stu-id="45972-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="45972-480">Ein `return` -Anweisung mit einem Ausdruck kann nur verwendet werden, in ein Funktionsmember, die einen Wert, d. h. eine Methode mit einem nicht-Void-Ergebnistyp, berechnet die `get` Accessor einer Eigenschaft oder der Indexer oder ein benutzerdefinierter Operator.</span><span class="sxs-lookup"><span data-stu-id="45972-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="45972-481">Eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) muss vorhanden sein, von dem Typ des Ausdrucks in den Rückgabetyp des enthaltenden Elements Funktion.</span><span class="sxs-lookup"><span data-stu-id="45972-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="45972-482">Zurückgeben, Anweisungen können auch verwendet werden, im Text der anonyme Funktionsausdrücke ([anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions)), und daran teilnehmen, die bestimmen, welche Konvertierungen für diese Funktionen vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="45972-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="45972-483">Es ist ein Fehler während der Kompilierung für eine `return` -Anweisung auftreten einer `finally` Block ([der Try-Anweisung](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="45972-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="45972-484">Ein `return` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="45972-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="45972-485">Wenn die `return` -Anweisung gibt einen Ausdruck, der Ausdruck wird ausgewertet, und der resultierende Wert wird durch eine implizite Konvertierung in den Rückgabetyp der enthaltenden Funktion konvertiert.</span><span class="sxs-lookup"><span data-stu-id="45972-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="45972-486">Das Ergebnis der Konvertierung wird der Ergebniswert, der von der Funktion generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="45972-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="45972-487">Wenn die `return` -Anweisung wird durch eine oder mehrere eingeschlossen `try` oder `catch` Blöcke mit verknüpften `finally` Blöcken Steuerelement anfänglich auf übertragen wird die `finally` Block des innersten `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="45972-488">Wenn und die Steuerung über den Endpunkt erreicht eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block der nächsten einschließenden `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="45972-489">Dieser Prozess wird wiederholt, bis die `finally` Blöcke alle einschließenden `try` Anweisungen ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="45972-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="45972-490">Wenn die enthaltende-Funktion keine Async-Funktion ist, wird die Steuerung an den Aufrufer der enthaltenden Funktion zusammen mit dem Ergebniswert ggf. zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="45972-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="45972-491">Wenn die enthaltende-Funktion eine asynchrone Funktion ist, die Steuerung wieder an den aktuellen Aufrufer und der Ergebniswert, sofern vorhanden, in der return-Aufgabe aufgezeichnet wird wie in beschrieben ([Enumeratorschnittstellen](classes.md#enumerator-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="45972-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="45972-492">Da eine `return` Anweisung bedingungslos überträgt die Steuerung an anderen Positionen wird der Endpunkt, der eine `return` -Anweisung ist nicht erreichbar.</span><span class="sxs-lookup"><span data-stu-id="45972-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="45972-493">Die Throw-Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-493">The throw statement</span></span>

<span data-ttu-id="45972-494">Die `throw` Anweisung löst eine Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="45972-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="45972-495">Ein `throw` -Anweisung mit einem Ausdruck löst aus, der von der Auswertung des Ausdrucks erzeugte Wert.</span><span class="sxs-lookup"><span data-stu-id="45972-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="45972-496">Der Ausdruck muss einen Wert des Klassentyps deuten `System.Exception`, eines Klassentyps, die von abgeleitet `System.Exception` oder von einem Typ der Type-Parameter, die `System.Exception` (oder eine Unterklasse davon) als effektive Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="45972-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="45972-497">Wenn die Auswertung des Ausdrucks erzeugt `null`, `System.NullReferenceException` sondern ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="45972-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="45972-498">Ein `throw` Anweisung ohne Ausdruck kann nur in verwendet werden eine `catch` blockieren, die in diesem Fall erneut die Anweisung wird die Ausnahme ausgelöst, die derzeit von diesem behandelt wird `catch` Block.</span><span class="sxs-lookup"><span data-stu-id="45972-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="45972-499">Da eine `throw` Anweisung bedingungslos überträgt die Steuerung an anderen Positionen wird der Endpunkt, der eine `throw` -Anweisung ist nicht erreichbar.</span><span class="sxs-lookup"><span data-stu-id="45972-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="45972-500">Wenn eine Ausnahme ausgelöst wird, wird die Steuerung an die erste `catch` -Klausel in einer einschließenden `try` -Anweisung, die die Ausnahme behandeln kann.</span><span class="sxs-lookup"><span data-stu-id="45972-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="45972-501">Der Prozess, der ab dem Zeitpunkt der ausgelösten Ausnahme zum Zeitpunkt des Übertragens der Steuerung an einen geeigneten Ausnahmehandler stattfindet, wird als bezeichnet ***ausnahmeweitergabe***.</span><span class="sxs-lookup"><span data-stu-id="45972-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="45972-502">Weitergabe einer Ausnahme besteht aus der Auswertung wiederholt die folgenden Schritte aus, bis eine `catch` -Klausel, die die Ausnahme entspricht gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="45972-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="45972-503">In dieser Beschreibung die ***Punkt auslösen*** beträgt anfänglich der Speicherort, an dem die Ausnahme wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="45972-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="45972-504">In das aktuelle Funktionselement jede `try` -Anweisung, die den Throw einschließt, wird untersucht.</span><span class="sxs-lookup"><span data-stu-id="45972-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="45972-505">Für jede Anweisung `S`, beginnend mit der innersten `try` -Anweisung und bis hin zu den äußersten `try` -Anweisung werden ausgewertet, die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="45972-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="45972-506">Wenn die `try` -Block `S` umschließt der auslösepunkt und S eine oder mehrere `catch` -Klauseln, die `catch` -Klauseln nacheinander überprüft werden, in der Reihenfolge ihres Auftretens einen geeigneten Handler für die Ausnahme, gemäß den Regeln, die im angegebenen gefunden Abschnitt [der Try-Anweisung](statements.md#the-try-statement).</span><span class="sxs-lookup"><span data-stu-id="45972-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="45972-507">Wenn kein übereinstimmendes `catch` Klausel befindet, die ausnahmeweitergabe erfolgt durch Übergabe der Steuerung an den Block, `catch` Klausel.</span><span class="sxs-lookup"><span data-stu-id="45972-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="45972-508">Andernfalls gilt: Wenn die `try` Block oder ein `catch` -Block `S` umschließt der auslösepunkt und, wenn `S` verfügt über eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block.</span><span class="sxs-lookup"><span data-stu-id="45972-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="45972-509">Wenn die `finally` Block eine weitere Ausnahme auslöst, die Verarbeitung der aktuellen Ausnahme beendet.</span><span class="sxs-lookup"><span data-stu-id="45972-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="45972-510">Wenn die Steuerung über den Endpunkt erreicht, andernfalls die `finally` Block Verarbeitung der aktuellen Ausnahme wird fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="45972-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="45972-511">Wenn ein Ausnahmehandler in den aktuellen Funktionsaufruf nicht gefunden wurde, der Funktionsaufruf wird beendet und eine der folgenden Bedingungen zutrifft:</span><span class="sxs-lookup"><span data-stu-id="45972-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="45972-512">Wenn die aktuelle Funktion nicht asynchronen ist, werden die oben genannten Schritte wiederholt für den Aufrufer der Funktion mit einer Throw-Punkt für die Anweisung, die aus der das Funktionselement aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="45972-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="45972-513">Ist die aktuelle Funktion Async "und" Aufgabe zurückgibt, wird die Ausnahme in der Task zurückgeben, die in einem fehlerhaften oder abgebrochenen Zustand versetzt wird, wie in beschrieben aufgezeichnet [Enumeratorschnittstellen](classes.md#enumerator-interfaces).</span><span class="sxs-lookup"><span data-stu-id="45972-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="45972-514">Wenn die aktuelle Funktion Async "und" void "zurückgebende ist, benachrichtigt der Synchronisierungskontext des aktuellen Threads wie in beschrieben [Enumerable-Schnittstellen](classes.md#enumerable-interfaces).</span><span class="sxs-lookup"><span data-stu-id="45972-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="45972-515">Wenn die ausnahmeverarbeitung alle Member für Funktionsaufrufe im aktuellen Thread beendet wird, ist, der angibt, dass der Thread kein Handler für die Ausnahme, hat der Thread selbst beendet.</span><span class="sxs-lookup"><span data-stu-id="45972-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="45972-516">Die Auswirkungen des Abbruchs ist implementierungsdefiniert.</span><span class="sxs-lookup"><span data-stu-id="45972-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="45972-517">Der Try-Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-517">The try statement</span></span>

<span data-ttu-id="45972-518">Die `try` Anweisung bietet einen Mechanismus zum Abfangen von Ausnahmen, die während der Ausführung eines Blocks auftreten.</span><span class="sxs-lookup"><span data-stu-id="45972-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="45972-519">Darüber hinaus die `try` Anweisung bietet die Möglichkeit, einen Codeblock angeben, die immer ausgeführt wird, wenn das Steuerelement verlässt die `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

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

<span data-ttu-id="45972-520">Es gibt drei mögliche Formen von `try` Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="45972-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="45972-521">Ein `try` -Block gefolgt von einer oder mehreren `catch` Blöcke.</span><span class="sxs-lookup"><span data-stu-id="45972-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="45972-522">Ein `try` -Block gefolgt von einem `finally` Block.</span><span class="sxs-lookup"><span data-stu-id="45972-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="45972-523">Ein `try` -Block gefolgt von einer oder mehreren `catch` Blöcke gefolgt von einem `finally` Block.</span><span class="sxs-lookup"><span data-stu-id="45972-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="45972-524">Wenn eine `catch` -Klausel gibt ein *Exception_specifier*, aufweisen muss `System.Exception`, ein Typ, der von abgeleitet ist `System.Exception` oder einen Typ-Parametertyp, die `System.Exception` (oder eine Unterklasse davon) als der effektive Basis-Klasse.</span><span class="sxs-lookup"><span data-stu-id="45972-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="45972-525">Wenn eine `catch` -Klausel gibt sowohl eine *Exception_specifier* mit eine *Bezeichner*, eine ***Ausnahmevariable*** mit dem angegebenen Namen und Typ deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="45972-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="45972-526">Die Ausnahmevariable entspricht einer lokalen Variablen mit einem Bereich, der über erweitert die `catch` Klausel.</span><span class="sxs-lookup"><span data-stu-id="45972-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="45972-527">Während der Ausführung der *Exception_filter* und *Block*, die Ausnahmevariable stellt die Ausnahme, die gerade verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="45972-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="45972-528">Für Zwecke der definitive Zuweisungen zu überprüfen gilt die Ausnahmevariable im gesamten Bereich definitiv zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="45972-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="45972-529">Es sei denn, eine `catch` -Klausel enthält einen Variablennamen der Ausnahme, es ist nicht möglich, auf das Ausnahmeobjekt im Filter und `catch` Block.</span><span class="sxs-lookup"><span data-stu-id="45972-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="45972-530">Ein `catch` -Klausel nicht angegeben ist, die eine *Exception_specifier* wird aufgerufen, eine allgemeine `catch` Klausel.</span><span class="sxs-lookup"><span data-stu-id="45972-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="45972-531">Einige Programmiersprachen unterstützen möglicherweise Ausnahmen, die nicht darstellbar sind, wie ein Objekt abgeleitet `System.Exception`, obwohl solche Ausnahmen niemals von c#-Code generiert werden können.</span><span class="sxs-lookup"><span data-stu-id="45972-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="45972-532">Eine allgemeine `catch` -Klausel kann verwendet werden, um solche Ausnahmen abzufangen.</span><span class="sxs-lookup"><span data-stu-id="45972-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="45972-533">Daher eine allgemeine `catch` -Klausel ist semantisch, die den Typ angibt, `System.Exception`, die erste auch aus anderen Sprachen Abfangen von Ausnahmen kann.</span><span class="sxs-lookup"><span data-stu-id="45972-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="45972-534">Um einen Handler für eine Ausnahme aus, suchen `catch` -Klauseln nacheinander überprüft werden, in der lexikalischen Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="45972-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="45972-535">Wenn eine `catch` -Klausel gibt an, ein Typ, aber keine Ausnahmefilter, es ist ein Fehler während der Kompilierung für eine höhere `catch` Klausel in der gleichen `try` -Anweisung geben Sie einen Typ, die entspricht, oder wird abgeleitet, die eingeben.</span><span class="sxs-lookup"><span data-stu-id="45972-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="45972-536">Wenn eine `catch` -Klausel gibt an, kein Typ und kein Filter, er muss die letzte `catch` -Klausel, die für die `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="45972-537">Innerhalb einer `catch` Block eine `throw` Anweisung ([die Throw-Anweisung](statements.md#the-throw-statement)) kein Ausdruck kann verwendet werden, um die Ausnahme erneut auszulösen, die von abgefangen wurde die `catch` Block.</span><span class="sxs-lookup"><span data-stu-id="45972-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="45972-538">Zuweisungen für eine Ausnahmevariable ändern nicht die Ausnahme, die erneut ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="45972-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="45972-539">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="45972-539">In the example</span></span>
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
<span data-ttu-id="45972-540">die Methode `F` wird eine Ausnahme abgefangen, Diagnoseinformationen an die Konsole schreibt, ändert die Ausnahmevariable und löst die Ausnahme erneut aus.</span><span class="sxs-lookup"><span data-stu-id="45972-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="45972-541">Die Ausnahme, die erneut ausgelöst wird ist die ursprüngliche Ausnahme, ist die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="45972-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="45972-542">Wenn die erste Catch-Block ausgelöst hatte `e` anstatt erneut die aktuelle Ausnahme ausgelöst, würde die Ausgabe wie folgt lauten:</span><span class="sxs-lookup"><span data-stu-id="45972-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```csharp
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="45972-543">Es ist ein Fehler während der Kompilierung für eine `break`, `continue`, oder `goto` Anweisung, um das Übergeben der Steuerung aus einer `finally` Block.</span><span class="sxs-lookup"><span data-stu-id="45972-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="45972-544">Wenn eine `break`, `continue`, oder `goto` -Anweisung befindet sich eine `finally` Block, der das Ziel der Anweisung muss im selben `finally` Block oder andernfalls ein Kompilierungsfehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="45972-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="45972-545">Es ist ein Fehler während der Kompilierung für eine `return` -Anweisung auftreten, in eine `finally` Block.</span><span class="sxs-lookup"><span data-stu-id="45972-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="45972-546">Ein `try` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="45972-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="45972-547">Die Steuerung an die `try` Block.</span><span class="sxs-lookup"><span data-stu-id="45972-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="45972-548">Wenn und die Steuerung über den Endpunkt erreicht die `try` blockieren:</span><span class="sxs-lookup"><span data-stu-id="45972-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="45972-549">Wenn die `try` Anweisung verfügt über eine `finally` Block, der `finally` Block wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="45972-550">Die Steuerung an den Endpunkt der `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="45972-551">Wenn eine Ausnahme, um weitergegeben wird die `try` Anweisung während der Ausführung der `try` blockieren:</span><span class="sxs-lookup"><span data-stu-id="45972-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="45972-552">Die `catch` -Klauseln, sofern vorhanden, werden in der Reihenfolge ihres Auftretens einen geeigneten Handler für die Ausnahme gefunden überprüft.</span><span class="sxs-lookup"><span data-stu-id="45972-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="45972-553">Wenn eine `catch` -Klausel kein Typ angegeben ist, oder gibt an, der Typ der Ausnahme oder ein Basistyp des Ausnahmetyps:</span><span class="sxs-lookup"><span data-stu-id="45972-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="45972-554">Wenn die `catch` -Klausel deklariert eine Ausnahmevariable, die Ausnahmevariable das Ausnahmeobjekt zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="45972-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="45972-555">Wenn die `catch` -Klausel deklariert einen Ausnahmefilter, der Filter ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="45972-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="45972-556">Ergibt die Auswertung `false`die Catch-Klausel ist keine Übereinstimmung und die Suche wird fortgesetzt, bis alle nachfolgenden `catch` Klauseln für einen geeigneten Handler.</span><span class="sxs-lookup"><span data-stu-id="45972-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="45972-557">Andernfalls die `catch` Klausel wird als Übereinstimmung angesehen, und die Steuerung an den entsprechenden `catch` Block.</span><span class="sxs-lookup"><span data-stu-id="45972-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="45972-558">Wenn und die Steuerung über den Endpunkt erreicht die `catch` blockieren:</span><span class="sxs-lookup"><span data-stu-id="45972-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="45972-559">Wenn die `try` Anweisung verfügt über eine `finally` Block, der `finally` Block wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="45972-560">Die Steuerung an den Endpunkt der `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="45972-561">Wenn eine Ausnahme, um weitergegeben wird die `try` Anweisung während der Ausführung der `catch` blockieren:</span><span class="sxs-lookup"><span data-stu-id="45972-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="45972-562">Wenn die `try` Anweisung verfügt über eine `finally` Block, der `finally` Block wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="45972-563">Die Ausnahme wird an der nächsten einschließenden weitergegeben `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="45972-564">Wenn die `try` Anweisung hat keine `catch` Klauseln oder wenn keine `catch` Klausel entspricht, der die Ausnahme:</span><span class="sxs-lookup"><span data-stu-id="45972-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="45972-565">Wenn die `try` Anweisung verfügt über eine `finally` Block, der `finally` Block wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="45972-566">Die Ausnahme wird an der nächsten einschließenden weitergegeben `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="45972-567">Die Anweisungen des eine `finally` Block werden immer ausgeführt, wenn das Steuerelement verlässt eine `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="45972-568">Dies ist "true" gibt an, ob die Steuerelement-Übertragung infolge einer normalen Ausführung, die als Ergebnis der Ausführung erfolgt eine `break`, `continue`, `goto`, oder `return` -Anweisung oder als Ergebnis eine Ausnahme von der `try` -Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="45972-569">Wenn eine Ausnahme, während der Ausführung ausgelöst wird einer `finally` blockieren, und nicht abgefangen innerhalb desselben finally-Block, der die Ausnahme an der nächsten einschließenden weitergegeben wird `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="45972-570">Wenn eine andere Ausnahme war gerade weitergegeben wird, geht diese Ausnahme verloren.</span><span class="sxs-lookup"><span data-stu-id="45972-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="45972-571">Erläutert der Prozess der Weitergabe einer Ausnahme in der Beschreibung der weiteren der `throw` Anweisung ([die Throw-Anweisung](statements.md#the-throw-statement)).</span><span class="sxs-lookup"><span data-stu-id="45972-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="45972-572">Die `try` -Block eine `try` Anweisung erreichbar ist. wenn die `try` -Anweisung erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="45972-573">Ein `catch` -Block eine `try` Anweisung erreichbar ist. wenn die `try` -Anweisung erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="45972-574">Die `finally` -Block eine `try` Anweisung erreichbar ist. wenn die `try` -Anweisung erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="45972-575">Der Endpunkt, der eine `try` -Anweisung ist erreichbar, wenn die beiden folgenden Bedingungen erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="45972-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="45972-576">Der Endpunkt des der `try` Block erreichbar ist oder das Ende der mindestens eine `catch` Block erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="45972-577">Wenn eine `finally` Block vorhanden ist, den Endpunkt der `finally` Block erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="45972-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="45972-578">Die Anweisungen checked und unchecked</span><span class="sxs-lookup"><span data-stu-id="45972-578">The checked and unchecked statements</span></span>

<span data-ttu-id="45972-579">Die `checked` und `unchecked` Anweisungen werden verwendet, um zu steuern die ***Kontext der überlaufprüfung*** für arithmetische Operationen für ganzzahlige Typen und Konvertierungen.</span><span class="sxs-lookup"><span data-stu-id="45972-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="45972-580">Die `checked` Anweisung bewirkt, dass alle Ausdrücke in der *Block* in einem überprüften Kontext ausgewertet werden und die `unchecked` Anweisung bewirkt, dass alle Ausdrücke in der *Block* in ausgewertet werden ein nicht geprüften Kontext.</span><span class="sxs-lookup"><span data-stu-id="45972-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="45972-581">Die `checked` und `unchecked` -Anweisungen sind genaue Entsprechung der `checked` und `unchecked` Operatoren ([checked und unchecked Operatoren](expressions.md#the-checked-and-unchecked-operators)), außer dass sie Blöcke anstelle von Ausdrücken ausgeführt werden .</span><span class="sxs-lookup"><span data-stu-id="45972-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="45972-582">Die Lock-Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-582">The lock statement</span></span>

<span data-ttu-id="45972-583">Die `lock` -Anweisung ruft die Sperre für gegenseitigen Ausschluss für ein bestimmtes Objekt, führt eine Anweisung aus und gibt dann die Sperre frei.</span><span class="sxs-lookup"><span data-stu-id="45972-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="45972-584">Der Ausdruck eine `lock` Anweisung muss einen Wert eines Typs, die bekanntermaßen kennzeichnen einer *Reference_type*.</span><span class="sxs-lookup"><span data-stu-id="45972-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="45972-585">Kein implizites Boxing-Konvertierung ([Boxingkonvertierungen](conversions.md#boxing-conversions)) erfolgt einmal für den Ausdruck, der eine `lock` -Anweisung, und daher ist ein Fehler während der Kompilierung für den Ausdruck zur Bezeichnung eines Werts von einer *Value_type*.</span><span class="sxs-lookup"><span data-stu-id="45972-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="45972-586">Ein `lock` -Anweisung der Form</span><span class="sxs-lookup"><span data-stu-id="45972-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="45972-587">wo `x` ist ein Ausdruck, der eine *Reference_type*, entspricht exakt dem</span><span class="sxs-lookup"><span data-stu-id="45972-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
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
<span data-ttu-id="45972-588">außer dass `x` nur einmal überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="45972-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="45972-589">Während eine Sperre für gegenseitigen Ausschluss gehalten wird, kann Ausführung von Code in der gleichen Ausführungsthread auch zu erhalten und die Sperre freigibt.</span><span class="sxs-lookup"><span data-stu-id="45972-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="45972-590">Allerdings wird die Ausführung von Code in anderen Threads blockiert, die Sperre abrufen, bis die Sperre aufgehoben wird.</span><span class="sxs-lookup"><span data-stu-id="45972-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="45972-591">Sperren `System.Type` Objekte zum Synchronisieren des Zugriffs auf statische Daten wird nicht empfohlen.</span><span class="sxs-lookup"><span data-stu-id="45972-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="45972-592">Anderer Code kann auf dem gleichen Typ, sperren, was zu Deadlocks führen kann.</span><span class="sxs-lookup"><span data-stu-id="45972-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="45972-593">Ein besserer Ansatz ist, den Zugriff auf statische Daten zu synchronisieren, indem Sie ein privates statisches Objekt zu sperren.</span><span class="sxs-lookup"><span data-stu-id="45972-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="45972-594">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="45972-594">For example:</span></span>
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

## <a name="the-using-statement"></a><span data-ttu-id="45972-595">Die using-Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-595">The using statement</span></span>

<span data-ttu-id="45972-596">Die `using` -Anweisung ruft eine oder mehrere Ressourcen, führt eine Anweisung aus und klicken Sie dann die Ressource frei.</span><span class="sxs-lookup"><span data-stu-id="45972-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="45972-597">Ein ***Ressource*** ist eine Klasse oder Struktur, die implementiert `System.IDisposable`, einschließlich eine einzelne parameterlose Methode, die mit dem Namen `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="45972-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="45972-598">Erreichen von Code, der eine Ressource verwendet `Dispose` um anzugeben, dass die Ressource nicht mehr benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="45972-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="45972-599">Wenn `Dispose` nicht aufgerufen wird, wird automatische Löschung letztendlich als Folge der Garbagecollection durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="45972-600">Wenn die Form der *Resource_acquisition* ist *Local_variable_declaration* der Typ des der *Local_variable_declaration* muss `dynamic` oder einen Typ der implizit in konvertiert werden kann `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="45972-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="45972-601">Wenn die Form der *Resource_acquisition* ist *Ausdruck* dieser Ausdruck muss implizit in sein `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="45972-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="45972-602">Lokale Variablen, die in einem *Resource_acquisition* sind schreibgeschützt und muss einen Initialisierer enthalten.</span><span class="sxs-lookup"><span data-stu-id="45972-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="45972-603">Ein Fehler während der Kompilierung tritt auf, wenn die eingebettete Anweisung versucht, diese lokalen Variablen zu ändern (per Zuweisung oder `++` und `--` Operatoren), nehmen sie die Adresse, oder übergeben Sie sie als `ref` oder `out` Parameter.</span><span class="sxs-lookup"><span data-stu-id="45972-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="45972-604">Ein `using` -Anweisung übersetzt in drei Teile: Abruf, Verwendung und Freigabe.</span><span class="sxs-lookup"><span data-stu-id="45972-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="45972-605">Bei Verwendung der Ressource steht implizit in einen `try` -Anweisung mit einer `finally` Klausel.</span><span class="sxs-lookup"><span data-stu-id="45972-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="45972-606">Dies `finally` Klausel gibt die Ressource frei.</span><span class="sxs-lookup"><span data-stu-id="45972-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="45972-607">Wenn eine `null` Ressource wird abgerufen, dann kein Aufruf von `Dispose` vorgenommen wird, und es wird keine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="45972-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="45972-608">Wenn die Ressource vom Typ `dynamic` wird dynamisch durch eine implizite Konvertierung für die dynamische Konvertierung ([implizite dynamische Konvertierungen](conversions.md#implicit-dynamic-conversions)) zu `IDisposable` während der Übernahme, um sicherzustellen, dass die Konvertierung ist vor der Nutzung und die Freigabe erfolgreich.</span><span class="sxs-lookup"><span data-stu-id="45972-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="45972-609">Ein `using` -Anweisung der Form</span><span class="sxs-lookup"><span data-stu-id="45972-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="45972-610">entspricht einem der drei möglichen Erweiterungen.</span><span class="sxs-lookup"><span data-stu-id="45972-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="45972-611">Wenn `ResourceType` ist ein Typ NULL-Werte, die Erweiterung ist</span><span class="sxs-lookup"><span data-stu-id="45972-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
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

<span data-ttu-id="45972-612">Andernfalls gilt bei `ResourceType` ist ein Werttyp oder ein Verweistyp als `dynamic`, ist die Erweiterung</span><span class="sxs-lookup"><span data-stu-id="45972-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="45972-613">Andernfalls gilt bei `ResourceType` ist `dynamic`, ist die Erweiterung</span><span class="sxs-lookup"><span data-stu-id="45972-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="45972-614">In beiden Erweiterung der `resource` Variable ist schreibgeschützt und in die eingebettete Anweisung, und die `d` Variable ist nicht sichtbar und kann nicht zugegriffen werden, in an, die eingebettete Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="45972-615">Eine Implementierung ist auf eine bestimmte using-Anweisung auf andere Weise, z. B. zur Verbesserung der Leistung, implementiert zulässig, solange das Verhalten aufgrund der oben genannten steigenden konsistent ist.</span><span class="sxs-lookup"><span data-stu-id="45972-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="45972-616">Ein `using` -Anweisung der Form</span><span class="sxs-lookup"><span data-stu-id="45972-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="45972-617">verfügt über die gleichen drei möglichen Erweiterungen.</span><span class="sxs-lookup"><span data-stu-id="45972-617">has the same three possible expansions.</span></span> <span data-ttu-id="45972-618">In diesem Fall `ResourceType` ist implizit der Kompilierzeit-Typ, der die `expression`, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="45972-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="45972-619">Andernfalls die Schnittstelle `IDisposable` selbst dient als die `ResourceType`.</span><span class="sxs-lookup"><span data-stu-id="45972-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="45972-620">Die `resource` Variable ist nicht sichtbar und kann nicht zugegriffen werden, in an, die eingebettete Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="45972-621">Wenn eine *Resource_acquisition* nimmt die Form einer *Local_variable_declaration*, es ist möglich, mehrere Ressourcen eines bestimmten Typs abrufen.</span><span class="sxs-lookup"><span data-stu-id="45972-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="45972-622">Ein `using` -Anweisung der Form</span><span class="sxs-lookup"><span data-stu-id="45972-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="45972-623">genau entspricht einer Sequenz von geschachtelt `using` Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="45972-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="45972-624">Das folgende Beispiel erstellt eine Datei namens `log.txt` und zwei Textzeilen in die Datei geschrieben.</span><span class="sxs-lookup"><span data-stu-id="45972-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="45972-625">Im Beispiel wird dann der gleichen Datei zum Lesen geöffnet und die enthaltenen Textzeilen in der Konsole kopiert.</span><span class="sxs-lookup"><span data-stu-id="45972-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
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

<span data-ttu-id="45972-626">Da die `TextWriter` und `TextReader` -Klassen implementieren die `IDisposable` -Schnittstelle, im Beispiel können `using` stellen Sie sicher, dass die zugrunde liegende Datei ordnungsgemäß geschlossen wird nach dem Schreiben oder Lesen Sie die Operations-Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="45972-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="45972-627">Die Yield-Anweisung</span><span class="sxs-lookup"><span data-stu-id="45972-627">The yield statement</span></span>

<span data-ttu-id="45972-628">Die `yield` Anweisung wird in einem Iteratorblock verwendet ([Blöcke](statements.md#blocks)) ergibt einen Wert auf das Enumeratorobjekt ([Enumeratorobjekte](classes.md#enumerator-objects)) oder ein aufzählbares Objekt ([aufzählbare Objekte](classes.md#enumerable-objects)) ein Iterator oder das Ende der Iteration zu signalisieren.</span><span class="sxs-lookup"><span data-stu-id="45972-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="45972-629">`yield` ist kein reserviertes Wort. Es hat eine besondere Bedeutung, die erst bei der unmittelbar vor einem `return` oder `break` Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="45972-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="45972-630">In anderen Kontexten `yield` als Bezeichner verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="45972-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="45972-631">Es gibt mehrere Einschränkungen dazu, wo ein `yield` Anweisung angezeigt werden kann, wie im folgenden beschrieben.</span><span class="sxs-lookup"><span data-stu-id="45972-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="45972-632">Es ist ein Fehler während der Kompilierung für eine `yield` -Anweisung (entweder Form) außerhalb angezeigt werden. eine *Method_body*, *Operator_body* oder *Accessor_body*</span><span class="sxs-lookup"><span data-stu-id="45972-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="45972-633">Es ist ein Fehler während der Kompilierung für eine `yield` -Anweisung (entweder Form) innerhalb einer anonymen Funktion angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="45972-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="45972-634">Es ist ein Fehler während der Kompilierung für eine `yield` -Anweisung (entweder Form) angezeigt werden die `finally` -Klausel einer `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="45972-635">Es ist ein Fehler während der Kompilierung für eine `yield return` Anweisung in eine beliebige Stelle angezeigt werden eine `try` -Anweisung, die enthält `catch` Klauseln.</span><span class="sxs-lookup"><span data-stu-id="45972-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="45972-636">Das folgende Beispiel zeigt einige gültigen und ungültigen Verwendungen von `yield` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="45972-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

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

<span data-ttu-id="45972-637">Eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) muss vorhanden sein, von dem Typ des Ausdrucks in der `yield return` Anweisung, um den Typ "yield" ([Yield-Typ](classes.md#yield-type)) des Iterators.</span><span class="sxs-lookup"><span data-stu-id="45972-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="45972-638">Ein `yield return` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="45972-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="45972-639">Die in der Anweisung angegebene Ausdruck wird ausgewertet, implizit in den Typ "yield" konvertiert und zugewiesen der `Current` Eigenschaft des Enumeratorobjekts.</span><span class="sxs-lookup"><span data-stu-id="45972-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="45972-640">Die Ausführung des Iteratorblocks wird angehalten.</span><span class="sxs-lookup"><span data-stu-id="45972-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="45972-641">Wenn die `yield return` -Anweisung ist in einem oder mehreren `try` blockiert wird, zugeordneten `finally` Blöcke werden zu diesem Zeitpunkt nicht ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="45972-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="45972-642">Die `MoveNext` gibt die Methode des Enumeratorobjekts `true` an den Aufrufer, der angibt, dass das Enumeratorobjekt erfolgreich auf das nächste Element gesetzt wurde.</span><span class="sxs-lookup"><span data-stu-id="45972-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="45972-643">Der nächste Aufruf auf des Enumeratorobjekts `MoveNext` -Methode setzt die Ausführung des Iteratorblocks aus, in dem es zuletzt angehalten wurde.</span><span class="sxs-lookup"><span data-stu-id="45972-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="45972-644">Ein `yield break` -Anweisung wird wie folgt ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="45972-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="45972-645">Wenn die `yield break` -Anweisung wird durch eine oder mehrere eingeschlossen `try` Blöcke mit verknüpften `finally` Blöcken Steuerelement anfänglich auf übertragen wird die `finally` Block des innersten `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="45972-646">Wenn und die Steuerung über den Endpunkt erreicht eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block der nächsten einschließenden `try` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="45972-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="45972-647">Dieser Prozess wird wiederholt, bis die `finally` Blöcke alle einschließenden `try` Anweisungen ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="45972-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="45972-648">Steuerelement wird an den Aufrufer des Iteratorblocks zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="45972-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="45972-649">Dies ist entweder der `MoveNext` Methode oder `Dispose` Methode des Enumeratorobjekts.</span><span class="sxs-lookup"><span data-stu-id="45972-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="45972-650">Da eine `yield break` Anweisung bedingungslos überträgt die Steuerung an anderen Positionen wird der Endpunkt, der eine `yield break` -Anweisung ist nicht erreichbar.</span><span class="sxs-lookup"><span data-stu-id="45972-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
