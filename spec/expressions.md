---
ms.openlocfilehash: 130898a8b5a7b8eb986b314cb4cf78038e840b02
ms.sourcegitcommit: 09e0ddec3bb6aa99b7340158bbac86a5a8243b43
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/24/2019
ms.locfileid: "66193933"
---
# <a name="expressions"></a><span data-ttu-id="411e5-101">Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-101">Expressions</span></span>

<span data-ttu-id="411e5-102">Ein Ausdruck ist eine Sequenz von Operatoren und Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="411e5-103">In diesem Kapitel wird die Syntax, die Reihenfolge der Auswertung von Operanden und Operatoren und Bedeutung von Ausdrücken definiert.</span><span class="sxs-lookup"><span data-stu-id="411e5-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="411e5-104">Ausdrucksklassifizierungen</span><span class="sxs-lookup"><span data-stu-id="411e5-104">Expression classifications</span></span>

<span data-ttu-id="411e5-105">Ein Ausdruck ist eines der folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="411e5-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="411e5-106">Ein Wert.</span><span class="sxs-lookup"><span data-stu-id="411e5-106">A value.</span></span> <span data-ttu-id="411e5-107">Jeder Wert verfügt über einen zugeordneten Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="411e5-108">Eine Variable.</span><span class="sxs-lookup"><span data-stu-id="411e5-108">A variable.</span></span> <span data-ttu-id="411e5-109">Jede Variable hat einen zugeordneten Typ, d. h. den deklarierten Typ der Variablen.</span><span class="sxs-lookup"><span data-stu-id="411e5-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="411e5-110">Ein Namespace.</span><span class="sxs-lookup"><span data-stu-id="411e5-110">A namespace.</span></span> <span data-ttu-id="411e5-111">Ein Ausdruck mit dieser Klassifizierung kann nur verwendet werden, als der linken Seite von einem *Member_access* ([Memberzugriff](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="411e5-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="411e5-112">In einem anderen Kontext bewirkt, dass ein Ausdruck, der als Namespace klassifiziert einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="411e5-113">Ein Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-113">A type.</span></span> <span data-ttu-id="411e5-114">Ein Ausdruck mit dieser Klassifizierung kann nur verwendet werden, als der linken Seite von einem *Member_access* ([Memberzugriff](expressions.md#member-access)), oder als Operand für die `as` Operator ([den Operator ](expressions.md#the-as-operator)), wird die `is` Operator ([der is-Operator](expressions.md#the-is-operator)), oder die `typeof` Operator ([der Typeof-Operator](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="411e5-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="411e5-115">In einem anderen Kontext bewirkt, dass ein Ausdruck, der als Typ klassifiziert einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="411e5-116">Eine Methodengruppe, ein Satz von überladenen Methoden, die sich aus einer Membersuche ergibt ([Membersuche](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="411e5-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="411e5-117">Eine Methodengruppe kann es sich um einen zugeordneten Instanzausdruck und einer Argumentliste für den zugeordneten Typ haben.</span><span class="sxs-lookup"><span data-stu-id="411e5-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="411e5-118">Wenn eine Instanzmethode aufgerufen wird, wird das Ergebnis der Auswertung des Ausdrucks für die Instanz die Instanz, die durch dargestellt `this` ([diesen Zugriff](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="411e5-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="411e5-119">Eine Methodengruppe ist zulässig, eine *Invocation_expression* ([Aufrufausdrücke](expressions.md#invocation-expressions)), ein *Delegate_creation_expression* ([Delegaterstellung Ausdrücke](expressions.md#delegate-creation-expressions)) sowie der linken Seite des ein is-Operator, und in einen kompatiblen Delegattyp implizit konvertiert werden kann ([Gruppe Konvertierungen](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="411e5-120">In einem anderen Kontext bewirkt, dass ein Ausdruck, der als eine Methodengruppe klassifiziert einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="411e5-121">Ein null-Literal.</span><span class="sxs-lookup"><span data-stu-id="411e5-121">A null literal.</span></span> <span data-ttu-id="411e5-122">Ein Ausdruck mit dieser Klassifizierung kann implizit in einem Referenztyp oder nullable-Typ konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="411e5-123">Eine anonyme Funktion.</span><span class="sxs-lookup"><span data-stu-id="411e5-123">An anonymous function.</span></span> <span data-ttu-id="411e5-124">Ein Ausdruck mit dieser Klassifizierung kann implizit in einen kompatiblen Delegattyp bzw. den Typ für die Ausdrucksbaumstruktur konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="411e5-125">Ein Eigenschaftenzugriff.</span><span class="sxs-lookup"><span data-stu-id="411e5-125">A property access.</span></span> <span data-ttu-id="411e5-126">Jedem Eigenschaftenzugriff ist einen zugeordneten Typ, d. h. den Typ der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="411e5-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="411e5-127">Darüber hinaus möglicherweise Zugriff auf eine Eigenschaft einen zugeordneten Instanzausdruck.</span><span class="sxs-lookup"><span data-stu-id="411e5-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="411e5-128">Wenn ein Accessor (die `get` oder `set` Block) einer Instanz der Zugriff auf Eigenschaften aufgerufen wird, das Ergebnis der Auswertung des Ausdrucks für die Instanz wird die Instanz, die durch dargestellt `this` ([diesen Zugriff](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="411e5-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="411e5-129">Ein Event-Zugriff.</span><span class="sxs-lookup"><span data-stu-id="411e5-129">An event access.</span></span> <span data-ttu-id="411e5-130">Jedes Ereigniszugriff verfügt über einen zugeordneten Typ, d. h. den Typ des Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="411e5-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="411e5-131">Darüber hinaus kann ein Ereignis einen zugeordneten Instanzausdruck zugreifen.</span><span class="sxs-lookup"><span data-stu-id="411e5-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="411e5-132">Ein Ereigniszugriff möglicherweise angezeigt, wie der linke Operand des der `+=` und `-=` Operatoren ([Ereignis Zuweisung](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="411e5-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="411e5-133">In einem anderen Kontext bewirkt, dass ein Ausdruck, der klassifiziert als Zugriffs-Ereignis einen Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="411e5-134">Indexzugriff.</span><span class="sxs-lookup"><span data-stu-id="411e5-134">An indexer access.</span></span> <span data-ttu-id="411e5-135">Jeder Indexzugriff verfügt über einen zugeordneten Typ, d. h. der Elementtyp des Indexers.</span><span class="sxs-lookup"><span data-stu-id="411e5-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="411e5-136">Darüber hinaus hat Indexzugriff einen zugeordneten Instanzausdruck und eine Liste zugeordnete Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="411e5-137">Wenn ein Accessor (die `get` oder `set` Block) eines Indexers Zugriff aufgerufen wird, das Ergebnis der Auswertung des Ausdrucks für die Instanz wird die Instanz, die durch dargestellt `this` ([diesen Zugriff](expressions.md#this-access)), und das Ergebnis des Auswerten der Argumentliste, wird die Parameterliste des Aufrufs.</span><span class="sxs-lookup"><span data-stu-id="411e5-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="411e5-138">"Nothing".</span><span class="sxs-lookup"><span data-stu-id="411e5-138">Nothing.</span></span> <span data-ttu-id="411e5-139">Dies tritt auf, wenn der Ausdruck einen Aufruf einer Methode mit einem Rückgabetyp von `void`.</span><span class="sxs-lookup"><span data-stu-id="411e5-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="411e5-140">Ein Ausdruck, der als "nothing" nur im Kontext gültig ist klassifiziert einen *Statement_expression* ([ausdrucksanweisungen](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="411e5-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="411e5-141">Das Endergebnis eines Ausdrucks ist nie ein Namespace, Typ, Methodengruppe oder Ereigniszugriff.</span><span class="sxs-lookup"><span data-stu-id="411e5-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="411e5-142">Stattdessen sind diese Kategorien von Ausdrücken wie oben erwähnt, intermediate-Konstrukte, die nur in bestimmten Kontexten zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="411e5-143">Eine Eigenschaft oder Indexzugriff ist immer neu klassifiziert als Wert durch Aufrufen von der *get-Accessor* oder *set-Accessor*.</span><span class="sxs-lookup"><span data-stu-id="411e5-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="411e5-144">Die bestimmte Zugriffsmethode richtet sich nach der im Rahmen der Eigenschaft oder der Indexer-Zugriff: Wenn der Zugriff auf das Ziel einer Zuweisung, ist die *set-Accessor* wird aufgerufen, um einen neuen Wert zuzuweisen ([einfache Zuweisung](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="411e5-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="411e5-145">Andernfalls die *get-Accessor* wird aufgerufen, um den aktuellen Wert zu erhalten ([Werte Ausdrücke](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="411e5-146">Werte der Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-146">Values of expressions</span></span>

<span data-ttu-id="411e5-147">Die meisten Konstrukte, bei denen einen Ausdruck erfordern schließlich den Ausdruck zur Angabe einer ***Wert***.</span><span class="sxs-lookup"><span data-stu-id="411e5-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="411e5-148">In solchen Fällen, wenn der tatsächliche Ausdruck steht für einen Namespace, einen Typ, einer Methodengruppe oder nichts, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="411e5-149">Wenn der Ausdruck einen Eigenschaftenzugriff, Indexzugriff oder eine Variable kennzeichnet, wird der Wert der Eigenschaft, Indexer oder Variable jedoch implizit ersetzt:</span><span class="sxs-lookup"><span data-stu-id="411e5-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="411e5-150">Der Wert einer Variablen ist einfach der Wert, die derzeit in den Speicherort identifiziert, die von der Variablen gespeichert.</span><span class="sxs-lookup"><span data-stu-id="411e5-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="411e5-151">Eine Variable muss definitiv zugewiesen berücksichtigt werden ([definitive Zuweisung](variables.md#definite-assignment)), bevor der Wert abgerufen werden kann, oder andernfalls tritt ein Fehler während der Kompilierung auf.</span><span class="sxs-lookup"><span data-stu-id="411e5-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="411e5-152">Der Wert eines Eigenschaftsausdrucks Zugriff wird ermittelt, durch den Aufruf der *get-Accessor* der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="411e5-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="411e5-153">Wenn die Eigenschaft keine *get-Accessor*, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="411e5-154">Andernfalls einen Member-Funktionsaufruf ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) ausgeführt wird, und das Ergebnis des Aufrufs wird der Wert der Eigenschaftsausdruck den Zugriff.</span><span class="sxs-lookup"><span data-stu-id="411e5-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="411e5-155">Der Wert eines Ausdrucks der Indexer-Zugriff wird ermittelt, durch den Aufruf der *get-Accessor* des Indexers.</span><span class="sxs-lookup"><span data-stu-id="411e5-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="411e5-156">Wenn der Indexer keine *get-Accessor*, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="411e5-157">Andernfalls einen Member-Funktionsaufruf ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) erfolgt mit dem Argument Liste der Indexer-Access-Ausdruck zugeordnet ist, und das Ergebnis des Aufrufs wird der Wert der Zugriff Indexerausdrucks.</span><span class="sxs-lookup"><span data-stu-id="411e5-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="411e5-158">Statische und dynamische Bindung</span><span class="sxs-lookup"><span data-stu-id="411e5-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="411e5-159">Der Prozess des bestimmens der Bedeutung eines Vorgangs basierend auf den Typ oder den Wert der enthaltenen Ausdrücke (Argumente, Operanden, Empfänger) wird häufig als bezeichnet ***Bindung***.</span><span class="sxs-lookup"><span data-stu-id="411e5-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="411e5-160">Beispielsweise wird die Bedeutung eines Methodenaufrufs bestimmt basierend auf dem Typ, der den Empfänger und die Argumente.</span><span class="sxs-lookup"><span data-stu-id="411e5-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="411e5-161">Die Bedeutung eines Operators wird basierend auf den Typ des Operanden bestimmt.</span><span class="sxs-lookup"><span data-stu-id="411e5-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="411e5-162">In C# geschrieben, die Bedeutung eines Vorgangs in der Regel richtet sich zum Zeitpunkt der Kompilierung, basierend auf dem Zeitpunkt der Kompilierung von der enthaltenen Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="411e5-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="411e5-163">Ebenso, wenn ein Ausdruck einen Fehler enthält, der Fehler wird erkannt und vom Compiler gemeldet.</span><span class="sxs-lookup"><span data-stu-id="411e5-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="411e5-164">Dieser Ansatz wird bezeichnet als ***statische Bindung***.</span><span class="sxs-lookup"><span data-stu-id="411e5-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="411e5-165">Allerdings ist ein Ausdruck eines dynamischen Ausdrucks (z. B. weist den Typ `dynamic`) Dies bedeutet, dass eine Bindung, die sie ein Teil ist basieren soll auf die Laufzeit-Typinformationen (d. h. der tatsächliche Typ des Objekts zur Laufzeit Dies bedeutet) anstatt den Typ am hat während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="411e5-166">Die Bindung eines solchen Vorgangs ist die Zeit aus diesem Grund zurückgestellt, in dem der Vorgang ist während der Ausführung des Programms ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="411e5-167">Dies wird als bezeichnet ***dynamische Bindung***.</span><span class="sxs-lookup"><span data-stu-id="411e5-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="411e5-168">Wenn ein Vorgang dynamisch gebunden ist, wird vom Compiler nur wenig oder keine Überprüfung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="411e5-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="411e5-169">Wenn die Bindung zur Laufzeit ein Fehler auftritt, werden stattdessen Fehler als Ausnahmen zur Laufzeit gemeldet.</span><span class="sxs-lookup"><span data-stu-id="411e5-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="411e5-170">Die folgenden Vorgänge in c# unterliegen Bindung aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="411e5-171">Memberzugriff: `e.M`</span><span class="sxs-lookup"><span data-stu-id="411e5-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="411e5-172">Methodenaufruf: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="411e5-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="411e5-173">Delegataufruf:`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="411e5-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="411e5-174">Der Elementzugriff: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="411e5-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="411e5-175">Erstellen von Objekten: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="411e5-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="411e5-176">Überladen von Unäroperatoren: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span><span class="sxs-lookup"><span data-stu-id="411e5-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="411e5-177">Binäre Operatoren überladen: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<` , `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span><span class="sxs-lookup"><span data-stu-id="411e5-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="411e5-178">Zuweisungsoperatoren: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span><span class="sxs-lookup"><span data-stu-id="411e5-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="411e5-179">Implizite und explizite Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="411e5-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="411e5-180">Wenn Sie keine dynamischen Ausdrücke beteiligt sind, standardmäßig c# statische Bindung, was bedeutet, dass die Typen während der Kompilierung der enthaltenen Ausdrücke in den Prozess verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="411e5-181">Wenn einer der enthaltenen Ausdrücke in den oben aufgeführten Vorgängen eines dynamischen Ausdrucks ist, ist jedoch, der Vorgang, stattdessen dynamisch gebunden.</span><span class="sxs-lookup"><span data-stu-id="411e5-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="411e5-182">Bindung-time</span><span class="sxs-lookup"><span data-stu-id="411e5-182">Binding-time</span></span>

<span data-ttu-id="411e5-183">Statische Bindung erfolgt zum Zeitpunkt der Kompilierung, während dynamische Bindung zur Laufzeit stattfindet.</span><span class="sxs-lookup"><span data-stu-id="411e5-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="411e5-184">In den folgenden Abschnitten der Begriff ***Bindung Kompilierzeit*** bezieht sich entweder während der Kompilierung oder zur Laufzeit, je nachdem, wann die Bindung erfolgt.</span><span class="sxs-lookup"><span data-stu-id="411e5-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="411e5-185">Das folgende Beispiel veranschaulicht die Konzepte der statische und dynamische Bindung und der Bindung Zeit:</span><span class="sxs-lookup"><span data-stu-id="411e5-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="411e5-186">Die ersten beiden Aufrufe statisch gebunden sind: die Überladung von `Console.WriteLine` wird ausgewählt, basierend auf dem Zeitpunkt der Kompilierung von deren Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="411e5-187">Daher ist die Bindung-die Zeit während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="411e5-188">Der dritte Aufruf dynamisch gebunden ist: die Überladung von `Console.WriteLine` wird ausgewählt, basierend auf dem Laufzeit-Typ des Arguments.</span><span class="sxs-lookup"><span data-stu-id="411e5-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="411e5-189">Dies liegt daran, dass das Argument eines dynamischen Ausdrucks – der Kompilierzeittyp `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="411e5-190">Daher ist die Bindung und die Uhrzeit für die dritte Aufruf zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="411e5-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="411e5-191">Dynamische Bindung</span><span class="sxs-lookup"><span data-stu-id="411e5-191">Dynamic binding</span></span>

<span data-ttu-id="411e5-192">Die dynamische Bindung dient zum Zulassen von C#-Programme für die Interaktion mit ***dynamische Objekte***, d. h. Objekte, die nicht die üblichen Regeln der C# folgen-Typsystem.</span><span class="sxs-lookup"><span data-stu-id="411e5-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="411e5-193">Dynamische Objekte können Objekte aus anderen Programmiersprachen mit verschiedenen Systemen werden, oder sie möglicherweise Objekte, die programmgesteuert einrichten, um ihre eigene Bindung-Semantik für verschiedene Vorgänge zu implementieren sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="411e5-194">Der Mechanismus, mit dem ein dynamisches Objekt eine eigene Semantik implementiert, ist die Implementierung definiert.</span><span class="sxs-lookup"><span data-stu-id="411e5-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="411e5-195">Eine bestimmte Schnittstelle – erneut Implementierung definiert – wird durch dynamische Objekte für die c#-Laufzeit zu signalisieren, dass sie spezielle Semantik haben implementiert.</span><span class="sxs-lookup"><span data-stu-id="411e5-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="411e5-196">Daher, wenn Vorgänge für ein dynamisches Objekt dynamisch gebunden sind, haben ihre eigene Bindung-Semantik, anstatt die C# -Code wie in diesem Dokument angegeben.</span><span class="sxs-lookup"><span data-stu-id="411e5-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="411e5-197">Während der Zweck der dynamischen Bindung ist um interoperation mit dynamischen Objekten zu ermöglichen, ermöglicht c# dynamische Bindung für alle Objekte, ob sie dynamisch oder nicht sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="411e5-198">Dies ermöglicht eine nahtlosere Integration der dynamische Objekte, wie die Ergebnisse der Vorgänge auf diesen können selbst keine dynamische Objekte, aber immer noch von einem Typ, der dem Programmierer zum Zeitpunkt der Kompilierung nicht bekannt sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="411e5-199">Dynamische Bindung können fehleranfällige Reflection basierender Code entfernen, selbst, wenn Sie keine beteiligten Objekte dynamische Objekte sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="411e5-200">In den folgenden Abschnitten beschreiben für jedes Konstrukt in der Sprache, genau Wenn dynamische Bindung übernommen wird, was zur Kompilierzeit – Wenn--wird angewendet, und welche die während der Kompilierung Result und Expression-Klassifizierung ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="411e5-201">Typen von einzelnen Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="411e5-201">Types of constituent expressions</span></span>

<span data-ttu-id="411e5-202">Wenn ein Vorgang statisch gebunden ist, gilt der Typ eines einzelnen Ausdrucks (z. B. einen Empfänger, ein Argument, einen Index oder ein Operand) immer der Kompilierzeit-Typ dieses Ausdrucks sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="411e5-203">Wenn ein Vorgang dynamisch gebunden ist, wird der Typ eines einzelnen Ausdrucks auf unterschiedliche Weise abhängig vom Zeitpunkt der Kompilierung der enthaltenen Ausdruck bestimmt:</span><span class="sxs-lookup"><span data-stu-id="411e5-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="411e5-204">Einen einzelnen Ausdruck Kompilierzeittyp `dynamic` gilt den Typ des tatsächlichen Werts haben die Auswertung des Ausdrucks, zur Laufzeit</span><span class="sxs-lookup"><span data-stu-id="411e5-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="411e5-205">Gilt als ein einzelnen Ausdruck, dessen Typ während der Kompilierung ein Typparameter ist, den Typ verfügen, die der Type-Parameter zur Laufzeit gebunden ist</span><span class="sxs-lookup"><span data-stu-id="411e5-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="411e5-206">Andernfalls gilt der enthaltenen Ausdruck der Kompilierzeit-Typ aufweisen.</span><span class="sxs-lookup"><span data-stu-id="411e5-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="411e5-207">Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-207">Operators</span></span>

<span data-ttu-id="411e5-208">Ausdrücke bestehen aus ***Operanden*** und ***Operatoren***.</span><span class="sxs-lookup"><span data-stu-id="411e5-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="411e5-209">Die Operatoren eines Ausdrucks geben an, welche Operationen auf die Operanden angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="411e5-210">Beispiele für Operatoren sind `+`, `-`, `*`, `/` und `new`.</span><span class="sxs-lookup"><span data-stu-id="411e5-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="411e5-211">Beispiele für Operanden sind Literale, Felder, lokale Variablen und Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="411e5-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="411e5-212">Es gibt drei Arten von Operatoren:</span><span class="sxs-lookup"><span data-stu-id="411e5-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="411e5-213">Unäre Operatoren.</span><span class="sxs-lookup"><span data-stu-id="411e5-213">Unary operators.</span></span> <span data-ttu-id="411e5-214">Die Unäroperatoren verwenden einen Operanden, und verwenden Sie entweder Präfix-Notation (z. B. `--x`) oder postfix-Notation (z. B. `x++`).</span><span class="sxs-lookup"><span data-stu-id="411e5-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="411e5-215">Binäre Operatoren.</span><span class="sxs-lookup"><span data-stu-id="411e5-215">Binary operators.</span></span> <span data-ttu-id="411e5-216">Die binären Operatoren nehmen zwei Operanden, und alle Infix-Notation verwenden (z. B. `x + y`).</span><span class="sxs-lookup"><span data-stu-id="411e5-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="411e5-217">ternärer operator</span><span class="sxs-lookup"><span data-stu-id="411e5-217">Ternary operator.</span></span> <span data-ttu-id="411e5-218">Nur ein ternärer Operator, `?:`, vorhanden ist; er übernimmt drei Operanden und infix-Notation (`c ? x : y`).</span><span class="sxs-lookup"><span data-stu-id="411e5-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="411e5-219">Die Reihenfolge der Auswertung von Operatoren in einem Ausdruck richtet sich nach der ***Rangfolge*** und ***Assoziativität*** der Operatoren ([Operatorrangfolge und Assoziativität](expressions.md#operator-precedence-and-associativity)) .</span><span class="sxs-lookup"><span data-stu-id="411e5-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="411e5-220">Die Operanden in einem Ausdruck werden von links nach rechts ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="411e5-221">Z. B. in `F(i) + G(i++) * H(i)`, Methode `F` wird aufgerufen, mit der alte Wert des `i`, then-Methode `G` wird aufgerufen, mit dem alten Wert der `i`, und schließlich-Methode `H` wird aufgerufen, mit dem neuen Wert der `i`.</span><span class="sxs-lookup"><span data-stu-id="411e5-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="411e5-222">Dadurch wird unabhängig vom Operatorrangfolge.</span><span class="sxs-lookup"><span data-stu-id="411e5-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="411e5-223">Bestimmte Operatoren können werden ***überladen***.</span><span class="sxs-lookup"><span data-stu-id="411e5-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="411e5-224">Überladen von Operatoren ermöglicht eine benutzerdefinierte operatorimplementierungen für Vorgänge, bei denen ein angegeben werden, oder beide der Operanden sind von einer benutzerdefinierten Klasse oder Struktur-Typ ([operatorüberladung](expressions.md#operator-overloading)).</span><span class="sxs-lookup"><span data-stu-id="411e5-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="411e5-225">Operatorrangfolge und Assoziativität</span><span class="sxs-lookup"><span data-stu-id="411e5-225">Operator precedence and associativity</span></span>

<span data-ttu-id="411e5-226">Wenn ein Ausdruck mehrere Operatoren enthält, bestimmt die ***Rangfolge*** der Operatoren die Reihenfolge, in der die einzelnen Operatoren ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="411e5-227">Beispiel: der Ausdruck `x + y * z` wird als ausgewertet, `x + (y * z)` da die `*` Operator hat Vorrang vor der Binärdatei `+` Operator.</span><span class="sxs-lookup"><span data-stu-id="411e5-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="411e5-228">Die Rangfolge von einem Operator wird durch die Definition ihrer zugeordneten Grammatik Produktion eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="411e5-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="411e5-229">Z. B. eine *Additive_expression* besteht aus einer Sequenz von *Multiplicative_expression*s getrennt durch `+` oder `-` Operatoren, wodurch die `+` und `-` Operatoren niedrigere Priorität als die `*`, `/`, und `%` Operatoren.</span><span class="sxs-lookup"><span data-stu-id="411e5-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="411e5-230">In der folgende Tabelle sind alle Operatoren in der Reihenfolge der Rangfolge von oben nach unten zusammengefasst:</span><span class="sxs-lookup"><span data-stu-id="411e5-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="411e5-231">__Bereich__</span><span class="sxs-lookup"><span data-stu-id="411e5-231">__Section__</span></span>                                                                                   | <span data-ttu-id="411e5-232">__Kategorie__</span><span class="sxs-lookup"><span data-stu-id="411e5-232">__Category__</span></span>                | <span data-ttu-id="411e5-233">__Operatoren__</span><span class="sxs-lookup"><span data-stu-id="411e5-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="411e5-234">Primary expressions (Primäre Ausdrücke)</span><span class="sxs-lookup"><span data-stu-id="411e5-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="411e5-235">Primär</span><span class="sxs-lookup"><span data-stu-id="411e5-235">Primary</span></span>                     | <span data-ttu-id="411e5-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="411e5-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="411e5-237">Unary operators (Unäre Operatoren)</span><span class="sxs-lookup"><span data-stu-id="411e5-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="411e5-238">Unär</span><span class="sxs-lookup"><span data-stu-id="411e5-238">Unary</span></span>                       | <span data-ttu-id="411e5-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="411e5-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="411e5-240">Arithmetic operators (Arithmetische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="411e5-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="411e5-241">Multiplikativ</span><span class="sxs-lookup"><span data-stu-id="411e5-241">Multiplicative</span></span>              | <span data-ttu-id="411e5-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="411e5-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="411e5-243">Arithmetic operators (Arithmetische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="411e5-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="411e5-244">Additiv</span><span class="sxs-lookup"><span data-stu-id="411e5-244">Additive</span></span>                    | <span data-ttu-id="411e5-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="411e5-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="411e5-246">Shift operators (Schiebeoperatoren)</span><span class="sxs-lookup"><span data-stu-id="411e5-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="411e5-247">Shift</span><span class="sxs-lookup"><span data-stu-id="411e5-247">Shift</span></span>                       | <span data-ttu-id="411e5-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="411e5-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="411e5-249">Relational and type-testing operators (Relationale und Typtestoperatoren)</span><span class="sxs-lookup"><span data-stu-id="411e5-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="411e5-250">Relational und Typtest</span><span class="sxs-lookup"><span data-stu-id="411e5-250">Relational and type testing</span></span> | <span data-ttu-id="411e5-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="411e5-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="411e5-252">Relational and type-testing operators (Relationale und Typtestoperatoren)</span><span class="sxs-lookup"><span data-stu-id="411e5-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="411e5-253">Gleichheit</span><span class="sxs-lookup"><span data-stu-id="411e5-253">Equality</span></span>                    | <span data-ttu-id="411e5-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="411e5-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="411e5-255">Logical operators (Logische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="411e5-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="411e5-256">Logisches AND</span><span class="sxs-lookup"><span data-stu-id="411e5-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="411e5-257">Logical operators (Logische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="411e5-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="411e5-258">Logisches XOR</span><span class="sxs-lookup"><span data-stu-id="411e5-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="411e5-259">Logical operators (Logische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="411e5-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="411e5-260">Logisches OR</span><span class="sxs-lookup"><span data-stu-id="411e5-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="411e5-261">Conditional logical operators (Bedingte logische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="411e5-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="411e5-262">Bedingtes AND</span><span class="sxs-lookup"><span data-stu-id="411e5-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="411e5-263">Conditional logical operators (Bedingte logische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="411e5-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="411e5-264">Bedingtes OR</span><span class="sxs-lookup"><span data-stu-id="411e5-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="411e5-265">The null coalescing operator (Der NULL-Sammeloperator)</span><span class="sxs-lookup"><span data-stu-id="411e5-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="411e5-266">NULL-Sammeloperator</span><span class="sxs-lookup"><span data-stu-id="411e5-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="411e5-267">Conditional operator (Bedingte Operatoren)</span><span class="sxs-lookup"><span data-stu-id="411e5-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="411e5-268">Bedingt</span><span class="sxs-lookup"><span data-stu-id="411e5-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="411e5-269">[Zuweisungsoperatoren](expressions.md#assignment-operators), [anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="411e5-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="411e5-270">Zuweisungs- und Lambda-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-270">Assignment and lambda expression</span></span> | <span data-ttu-id="411e5-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="411e5-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="411e5-272">Tritt ein Operand zwischen zwei Operatoren mit gleicher Rangfolge auf, steuert die Assoziativität der Operatoren die Reihenfolge, in der die Vorgänge ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="411e5-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="411e5-273">Mit Ausnahme der Zuweisungsoperatoren und der null-Sammeloperator, alle binären Operatoren sind ***linksassoziativ***, was bedeutet, dass Vorgänge von links nach rechts ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="411e5-274">`x + y + z` wird beispielsweise als `(x + y) + z` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="411e5-275">Die Zuweisungsoperatoren, die null-Sammeloperator und der bedingte Operator (`?:`) sind ***rechtsassoziativ***, was bedeutet, dass Vorgänge, die von rechts nach links ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="411e5-276">`x = y = z` wird beispielsweise als `x = (y = z)` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="411e5-277">Rangfolge und Assoziativität können mit Klammern gesteuert werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="411e5-278">In `x + y * z` wird beispielsweise zuerst `y` mit `z` multipliziert und dann das Ergebnis zu `x` addiert, aber in `(x + y) * z` werden zunächst `x` und `y` addiert, und dann wird das Ergebnis mit `z` multipliziert.</span><span class="sxs-lookup"><span data-stu-id="411e5-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="411e5-279">Überladen von Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-279">Operator overloading</span></span>

<span data-ttu-id="411e5-280">Alle unären und binären Operatoren sind Implementierungen vordefiniert, die in jedem Ausdruck automatisch verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="411e5-281">Zusätzlich zu den vordefinierten Implementierungen können benutzerdefinierte Implementierungen dazu eingeleitet werden `operator` Deklarationen in Klassen und Strukturen ([Operatoren](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="411e5-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="411e5-282">Benutzerdefinierte Implementierungen haben immer Vorrang vor vordefinierte operatorimplementierungen: Nur bei der keine Implementierungen der entsprechenden benutzerdefinierten Operator wird vorhanden sind die vordefinierten operatorimplementierungen berücksichtigt werden wie in beschrieben, [unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution) und [binärer Operator-Überladung Auflösung](expressions.md#binary-operator-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="411e5-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="411e5-283">Die ***überladbarer Unäroperatoren*** sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="411e5-284">Obwohl `true` und `false` nicht explizit in Ausdrücken verwendet werden (und sind daher nicht in der Rangfolgentabelle in enthalten [Operatorrangfolge und Assoziativität](expressions.md#operator-precedence-and-associativity)), diese werden als Operatoren betrachtet, da sie sich befinden in mehrere ausdruckskontexten aufgerufen: boolesche Ausdrücke ([boolesche Ausdrücke](expressions.md#boolean-expressions)) und im Zusammenhang mit der bedingten Ausdrücke ([Bedingungsoperator](expressions.md#conditional-operator)), und bedingte logische Operatoren ([bedingten logischen Operatoren](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="411e5-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="411e5-285">Die ***Überladbare binäre Operatoren*** sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="411e5-286">Nur die oben aufgeführten Operatoren können überladen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="411e5-287">Insbesondere ist es nicht möglich, Memberzugriff, Methodenaufruf zu überladen oder `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, und `is` Operatoren.</span><span class="sxs-lookup"><span data-stu-id="411e5-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="411e5-288">Wenn ein binärer Operator überladen ist, wird der zugehörige Zuweisungsoperator, sofern er vorhanden ist, auch implizit überladen.</span><span class="sxs-lookup"><span data-stu-id="411e5-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="411e5-289">Angenommen, eine Überladung des Operators `*` ist auch eine Überladung des Operators `*=`.</span><span class="sxs-lookup"><span data-stu-id="411e5-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="411e5-290">Dies wird weiter in [Verbundzuweisung](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="411e5-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="411e5-291">Beachten Sie, dass der Zuweisungsoperator selbst (`=`) kann nicht überladen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="411e5-292">Eine Zuweisung immer führt eine einfache bitweise Kopie eines Werts in einer Variablen an.</span><span class="sxs-lookup"><span data-stu-id="411e5-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="411e5-293">Wandeln Sie die Vorgänge, z. B. `(T)x`, werden durch die Bereitstellung von benutzerdefinierten Konvertierungen überladen ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="411e5-294">Element zuzugreifen, z. B. `a[x]`, einen überladbaren Operators wird nicht berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="411e5-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="411e5-295">Benutzerdefinierte Indizierung wird jedoch unterstützt über Indexer ([Indexer](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="411e5-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="411e5-296">In Ausdrücken werden Operatoren mithilfe der Operatornotation und in Deklarationen, Operatoren sind verwiesen funktionale Notation verwenden.</span><span class="sxs-lookup"><span data-stu-id="411e5-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="411e5-297">Die folgende Tabelle zeigt die Beziehung zwischen den Operator und funktionalen Notationen für unären und binären Operatoren.</span><span class="sxs-lookup"><span data-stu-id="411e5-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="411e5-298">In den ersten Eintrag *Op* kennzeichnet alle überladbarer unärer Präfix-Operator.</span><span class="sxs-lookup"><span data-stu-id="411e5-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="411e5-299">In den zweiten Eintrag *Op* kennzeichnet das Postfix unäre `++` und `--` Operatoren.</span><span class="sxs-lookup"><span data-stu-id="411e5-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="411e5-300">Im dritten Eintrag *Op* kennzeichnet alle überladbaren binären Operator.</span><span class="sxs-lookup"><span data-stu-id="411e5-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="411e5-301">__Operator-notation__</span><span class="sxs-lookup"><span data-stu-id="411e5-301">__Operator notation__</span></span> | <span data-ttu-id="411e5-302">__Funktionale notation__</span><span class="sxs-lookup"><span data-stu-id="411e5-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="411e5-303">Eine benutzerdefinierte Operatordeklarationen erfordern immer mit mindestens einer der Parameter des Typs Klasse oder Struktur sein, die sich die Operatordeklaration enthält.</span><span class="sxs-lookup"><span data-stu-id="411e5-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="411e5-304">Daher ist es nicht möglich, für einen benutzerdefinierten Operator auf die gleiche Signatur wie einen vordefinierten Operator haben.</span><span class="sxs-lookup"><span data-stu-id="411e5-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="411e5-305">Benutzerdefinierte Deklarationen können nicht der Syntax, die Rangfolge oder die Assoziativität von einem Operator geändert werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="411e5-306">Z. B. die `/` -Operators ist immer ein binärer Operator, immer wurde die rangfolgenebene angegeben [Operatorrangfolge und Assoziativität](expressions.md#operator-precedence-and-associativity), und ist immer linksassoziativ.</span><span class="sxs-lookup"><span data-stu-id="411e5-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="411e5-307">Es ist, zwar möglich, dass ein benutzerdefinierter Operator eine Berechnung ausführen, die es pleases sind Implementierungen, die Ergebnisse nicht zu erzeugen, die intuitiv erwartet werden dringend abgeraten.</span><span class="sxs-lookup"><span data-stu-id="411e5-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="411e5-308">Zum Beispiel eine Implementierung von `operator ==` sollten die beiden Operanden hinsichtlich ihrer Gleichheit und Zurückgeben eines entsprechenden `bool` Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="411e5-309">Die Beschreibungen der einzelnen Operatoren in [primärausdrücke](expressions.md#primary-expressions) über [bedingten logischen Operatoren](expressions.md#conditional-logical-operators) Geben Sie die vordefinierte Implementierungen von Operatoren und alle zusätzlichen Regeln, die angewendet werden zu jedem Operator.</span><span class="sxs-lookup"><span data-stu-id="411e5-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="411e5-310">Stellen Sie die Beschreibungen der Begriffe verwenden ***überladungsauflösung für unäroperator***, ***binärer Operator überladungsauflösung***, und ***numerische heraufstufung***, Definitionen sind finden Sie in den folgenden Abschnitten.</span><span class="sxs-lookup"><span data-stu-id="411e5-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="411e5-311">Überladungsauflösung für Unären operator</span><span class="sxs-lookup"><span data-stu-id="411e5-311">Unary operator overload resolution</span></span>

<span data-ttu-id="411e5-312">Ein Vorgang des Formulars `op x` oder `x op`, wobei `op` ein überladbarer unärer Operator, und `x` ist ein Ausdruck vom Typ `X`, wird wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="411e5-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="411e5-313">Der Satz von Kandidaten benutzerdefinierten Operatoren, die mit der gebotenen `X` für den Vorgang `operator op(x)` wird bestimmt, wobei die Regeln der [Candidate benutzerdefinierte Operatoren](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="411e5-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="411e5-314">Wenn der Satz von Kandidaten benutzerdefinierte Operatoren nicht leer ist, wird dies die Gruppe der Kandidaten-Operatoren für den Vorgang.</span><span class="sxs-lookup"><span data-stu-id="411e5-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="411e5-315">Andernfalls die vordefinierten unären `operator op` Implementierungen, einschließlich deren angehobene Forms, werden der Satz von Kandidaten-Operatoren für den Vorgang.</span><span class="sxs-lookup"><span data-stu-id="411e5-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="411e5-316">Die vordefinierte Implementierung eines bestimmten Operators werden in der Beschreibung des Operators angegeben ([primärausdrücke](expressions.md#primary-expressions) und [unäre Operatoren](expressions.md#unary-operators)).</span><span class="sxs-lookup"><span data-stu-id="411e5-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="411e5-317">Die Regeln der überladungsauflösung des [Überladungsauflösung](expressions.md#overload-resolution) gelten für den Satz von Kandidaten-Operatoren, um die beste Operator in Bezug auf die Argumentliste auszuwählen `(x)`, und dieser Operator wird das Ergebnis der Überladung der Auflösungsvorgang.</span><span class="sxs-lookup"><span data-stu-id="411e5-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="411e5-318">Überladungsauflösung keinen einzelnen bewährte Operator auswählen, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="411e5-319">Binärer Operator Auflösung von funktionsüberladungen</span><span class="sxs-lookup"><span data-stu-id="411e5-319">Binary operator overload resolution</span></span>

<span data-ttu-id="411e5-320">Ein Vorgang des Formulars `x op y`, wobei `op` ist ein Überladbarer binärer Operator `x` ist ein Ausdruck vom Typ `X`, und `y` ist ein Ausdruck vom Typ `Y`, wird wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="411e5-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="411e5-321">Der Satz von Kandidaten benutzerdefinierten Operatoren, die mit der gebotenen `X` und `Y` für den Vorgang `operator op(x,y)` bestimmt ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="411e5-322">Der Satz besteht aus der Union der Kandidat Operatoren gebotenen `X` und die Candidate-Operatoren, die von bereitgestellte `Y`, jeweils festgelegt, wobei die Regeln der [Candidate benutzerdefinierte Operatoren](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="411e5-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="411e5-323">Wenn `X` und `Y` denselben Typ aufweisen, oder wenn `X` und `Y` über einen gemeinsamen Basistyp abgeleitet werden, und klicken Sie dann gemeinsam genutzte potentielle Operatoren nur einmal in der kombinierten Gruppe vorkommen.</span><span class="sxs-lookup"><span data-stu-id="411e5-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="411e5-324">Wenn der Satz von Kandidaten benutzerdefinierte Operatoren nicht leer ist, wird dies die Gruppe der Kandidaten-Operatoren für den Vorgang.</span><span class="sxs-lookup"><span data-stu-id="411e5-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="411e5-325">Andernfalls die vordefinierten Binärdatei `operator op` Implementierungen, einschließlich deren angehobene Forms, werden der Satz von Kandidaten-Operatoren für den Vorgang.</span><span class="sxs-lookup"><span data-stu-id="411e5-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="411e5-326">Die vordefinierte Implementierung eines bestimmten Operators werden in der Beschreibung des Operators angegeben ([arithmetische Operatoren](expressions.md#arithmetic-operators) über [bedingten logischen Operatoren](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="411e5-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="411e5-327">Für vordefinierte für Enumerationen und Delegaten-Operatoren sind Operatoren die nur berücksichtigt die durch eine Enumeration oder Delegat-Typ, der der Bindung-Time-Typ, der einer der Operanden ist definierten.</span><span class="sxs-lookup"><span data-stu-id="411e5-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="411e5-328">Die Regeln der überladungsauflösung des [Überladungsauflösung](expressions.md#overload-resolution) gelten für den Satz von Kandidaten-Operatoren, um die beste Operator in Bezug auf die Argumentliste auszuwählen `(x,y)`, und dieser Operator wird das Ergebnis der Überladung der Auflösungsvorgang.</span><span class="sxs-lookup"><span data-stu-id="411e5-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="411e5-329">Überladungsauflösung keinen einzelnen bewährte Operator auswählen, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="411e5-330">Kandidat benutzerdefinierte Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-330">Candidate user-defined operators</span></span>

<span data-ttu-id="411e5-331">Bei einem vorgegebenen Typ `T` und einen Vorgang `operator op(A)`, wobei `op` ist ein überladbaren Operators und `A` ist eine Argumentliste, die den Satz von Kandidaten benutzerdefinierten Operatoren, die von bereitgestellte `T` für `operator op(A)` richtet sich wie folgt:</span><span class="sxs-lookup"><span data-stu-id="411e5-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="411e5-332">Bestimmen Sie den Typ `T0`.</span><span class="sxs-lookup"><span data-stu-id="411e5-332">Determine the type `T0`.</span></span> <span data-ttu-id="411e5-333">Wenn `T` ist ein NULL-Werte zulässt, `T0` ist die zugrunde liegenden Typ und andernfalls `T0` gleich `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="411e5-334">Für alle `operator op` Deklarationen in `T0` und alle Arten von solcher Operatoren, aufgehoben wird, wenn mindestens ein Operator anwendbar ist ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)) in Bezug auf die Argumentliste `A`, klicken Sie dann den Satz von Kandidat Operatoren besteht aus solchen entsprechenden Operatoren in `T0`.</span><span class="sxs-lookup"><span data-stu-id="411e5-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="411e5-335">Andernfalls gilt: Wenn `T0` ist `object`, der Satz von Kandidaten-Operatoren ist leer.</span><span class="sxs-lookup"><span data-stu-id="411e5-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="411e5-336">Andernfalls der Satz von Kandidaten Standardabfrageoperatoren gebotenen `T0` ist ein Satz von Kandidaten-Operatoren, die durch die direkte Basisklasse bereitgestellt `T0`, oder die effektive Basisklasse `T0` Wenn `T0` ein Parameter vom Typ ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="411e5-337">Numerische heraufstufungen</span><span class="sxs-lookup"><span data-stu-id="411e5-337">Numeric promotions</span></span>

<span data-ttu-id="411e5-338">Numerische heraufstufung besteht aus automatisch bestimmte implizite Konvertierungen von Operanden der unären und binären numerischen Operatoren für Sie ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="411e5-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="411e5-339">Numerische heraufstufung ist nicht auf, eine unterschiedliche Methode, sondern vielmehr eine Auswirkung der Auflösung von funktionsüberladungen auf die vordefinierten Operatoren angewendet.</span><span class="sxs-lookup"><span data-stu-id="411e5-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="411e5-340">Numerische heraufstufung wirkt speziell Auswertung von benutzerdefinierten Operatoren, sich nicht auch benutzerdefinierte Operatoren weisen ähnliche Auswirkungen implementiert werden können.</span><span class="sxs-lookup"><span data-stu-id="411e5-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="411e5-341">Ein Beispiel für numerische heraufstufung, sollten Sie die vordefinierte Implementierungen der Binärdatei `*` Operator:</span><span class="sxs-lookup"><span data-stu-id="411e5-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="411e5-342">Wenn Regeln zur überladungsauflösung ([Auflösen der Überladung](expressions.md#overload-resolution)) gelten für diesen Satz von Operatoren, der Effekt ist, die Operandentypen auswählen, das erste der Operatoren für die implizite Konvertierungen vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="411e5-343">Z. B. für den Vorgang `b * s`, wobei `b` ist eine `byte` und `s` ist eine `short`, überladen Sie die Lösung wählt `operator *(int,int)` als bester Operator.</span><span class="sxs-lookup"><span data-stu-id="411e5-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="411e5-344">Daher das wird also `b` und `s` werden in konvertiert `int`, und der Typ des Ergebnisses ist `int`.</span><span class="sxs-lookup"><span data-stu-id="411e5-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="411e5-345">Ebenso für den Vorgang `i * d`, wobei `i` ist ein `int` und `d` ist eine `double`, überladen Sie die Lösung wählt `operator *(double,double)` als bester Operator.</span><span class="sxs-lookup"><span data-stu-id="411e5-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="411e5-346">Unäre numerischen heraufstufungen</span><span class="sxs-lookup"><span data-stu-id="411e5-346">Unary numeric promotions</span></span>

<span data-ttu-id="411e5-347">Unäre numerische heraufstufung tritt auf, für den Operanden des vordefinierten `+`, `-`, und `~` unäre Operatoren.</span><span class="sxs-lookup"><span data-stu-id="411e5-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="411e5-348">Unäre numerische heraufstufung besteht nur aus der Konvertierung von Operanden des Typs `sbyte`, `byte`, `short`, `ushort`, oder `char` eingeben `int`.</span><span class="sxs-lookup"><span data-stu-id="411e5-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="411e5-349">Darüber hinaus für den unären `-` Operator, Unäre numerische heraufstufung konvertiert Operanden vom Typ `uint` eingeben `long`.</span><span class="sxs-lookup"><span data-stu-id="411e5-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="411e5-350">Binäre numerische heraufstufungen</span><span class="sxs-lookup"><span data-stu-id="411e5-350">Binary numeric promotions</span></span>

<span data-ttu-id="411e5-351">Binäre numerische heraufstufung tritt auf, für den Operanden des vordefinierten `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, und `<=` binären Operatoren.</span><span class="sxs-lookup"><span data-stu-id="411e5-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="411e5-352">Binäre numerische heraufstufung konvertiert beide Operanden implizit in einen gemeinsamen Typ, der bei der nicht-relationalen Operatoren der Ergebnistyp des Vorgangs wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="411e5-353">Binäre numerische heraufstufung besteht aus der Anwendung der folgenden Regeln, in der Reihenfolge, die sie hier angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="411e5-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="411e5-354">Wenn ein Operand vom Typ `decimal`, der andere Operand wird in den Typ konvertiert `decimal`, oder es sich bei ein Fehler während der Datenbindung tritt auf, wenn der andere Operand vom Typ `float` oder `double`.</span><span class="sxs-lookup"><span data-stu-id="411e5-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="411e5-355">Andernfalls, wenn ein Operand vom Typ `double`, der andere Operand wird in den Typ konvertiert `double`.</span><span class="sxs-lookup"><span data-stu-id="411e5-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="411e5-356">Andernfalls, wenn ein Operand vom Typ `float`, der andere Operand wird in den Typ konvertiert `float`.</span><span class="sxs-lookup"><span data-stu-id="411e5-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="411e5-357">Andernfalls, wenn ein Operand vom Typ `ulong`, der andere Operand wird in den Typ konvertiert `ulong`, oder es sich bei ein Fehler während der Datenbindung tritt auf, wenn der andere Operand vom Typ `sbyte`, `short`, `int`, oder `long`.</span><span class="sxs-lookup"><span data-stu-id="411e5-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="411e5-358">Andernfalls, wenn ein Operand vom Typ `long`, der andere Operand wird in den Typ konvertiert `long`.</span><span class="sxs-lookup"><span data-stu-id="411e5-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="411e5-359">Andernfalls, wenn ein Operand vom Typ `uint` und der andere Operand ist vom Typ `sbyte`, `short`, oder `int`, beide Operanden sind in den Typ konvertiert `long`.</span><span class="sxs-lookup"><span data-stu-id="411e5-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="411e5-360">Andernfalls, wenn ein Operand vom Typ `uint`, der andere Operand wird in den Typ konvertiert `uint`.</span><span class="sxs-lookup"><span data-stu-id="411e5-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="411e5-361">Andernfalls werden beide Operanden Typ konvertiert `int`.</span><span class="sxs-lookup"><span data-stu-id="411e5-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="411e5-362">Beachten Sie, dass die erste Regel keine Vorgänge lässt nicht zu, die Mischen der `decimal` Geben Sie mit der `double` und `float` Typen.</span><span class="sxs-lookup"><span data-stu-id="411e5-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="411e5-363">Die Regel ergibt sich aus der Tatsache, die es keine impliziten Konvertierungen zwischen gibt den `decimal` Typ und die `double` und `float` Typen.</span><span class="sxs-lookup"><span data-stu-id="411e5-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="411e5-364">Beachten Sie außerdem, dass es nicht möglich, dass ein Operand vom Typ `ulong` bei der andere Operand ist, der einen ganzzahligen Typ mit Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="411e5-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="411e5-365">Der Grund ist, dass kein ganzzahliger Typ, der vorhanden ist kann das gesamte Spektrum darstellen `ulong` sowie die ganzzahligen Typen mit Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="411e5-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="411e5-366">In beiden oben genannten Fällen kann ein Cast-Ausdruck verwendet werden, zu einer der Operanden explizit in einen Typ zu konvertieren, die mit der andere Operand kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="411e5-367">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="411e5-368">ein Fehler während der Datenbindung tritt auf, weil eine `decimal` kann nicht multipliziert eine `double`.</span><span class="sxs-lookup"><span data-stu-id="411e5-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="411e5-369">Der Fehler wurde behoben, durch explizite Konvertierung der zweite Operand `decimal`wie folgt:</span><span class="sxs-lookup"><span data-stu-id="411e5-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="411e5-370">Transformierten Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-370">Lifted operators</span></span>

<span data-ttu-id="411e5-371">***Operatoren angehoben*** zulassen von vordefinierten und benutzerdefinierten Operatoren, die Vorgänge an nicht auf NULL festlegbare Werttypen auch mit NULL-Werte zulassen Forms dieser Typen verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="411e5-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="411e5-372">Transformierten Operatoren bestehen aus vordefinierten und benutzerdefinierten Operatoren, die Anforderungen erfüllen, wie im folgenden beschrieben:</span><span class="sxs-lookup"><span data-stu-id="411e5-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="411e5-373">Für die Unäroperatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="411e5-374">eine angehobene Form eines Operators ist vorhanden, wenn die Operanden und Ergebnis beide nicht auf NULL festlegbare Werttypen sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="411e5-375">Die transformierten Form wird durch Hinzufügen eines einzelnen erstellt `?` Modifizierer, um die Typen von Operanden und Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="411e5-376">Der transformierten Operator erzeugt einen null-Wert, wenn der Operand null ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="411e5-377">Der transformierten Operator hingegen entpackt der Operand, der zugrunde liegenden-Operator angewendet und bindet das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="411e5-378">Für die binären Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="411e5-379">eine angehobene Form eines Operators ist vorhanden, wenn die Operanden und Ergebnis alle nicht auf NULL festlegbare Werttypen sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="411e5-380">Die transformierten Form wird durch Hinzufügen eines einzelnen erstellt `?` Modifizierer für jeden Operanden und Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="411e5-381">Der transformierten Operator erzeugt einen null-Wert, wenn eine oder beide Operanden null sind (eine Ausnahme wird die `&` und `|` Operatoren mit den `bool?` eingeben, wie in beschrieben [Boolesche logische Operatoren](expressions.md#boolean-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="411e5-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="411e5-382">Andernfalls der transformierten Operator entpackt die Operanden der zugrunde liegenden-Operator angewendet und dient als Wrapper für das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="411e5-383">Für die Gleichheitsoperatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="411e5-384">eine angehobene Form der Bediener vorhanden ist, wenn die Operandentypen sind nicht auf NULL festlegbare Werttypen und ist der Ergebnistyp `bool`.</span><span class="sxs-lookup"><span data-stu-id="411e5-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="411e5-385">Die transformierten Form wird durch Hinzufügen eines einzelnen erstellt `?` Modifizierer für die einzelnen Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="411e5-386">Der transformierten Operator betrachtet, zwei null-Werte identisch sind, und ein null-Wert auf einen beliebigen nicht-Null-Wert ungleich.</span><span class="sxs-lookup"><span data-stu-id="411e5-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="411e5-387">Wenn beide Operanden ungleich Null sind, wird der transformierten-Operator entpackt die Operanden und wendet den zugrunde liegenden Operator zum Erzeugen der `bool` Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="411e5-388">Für die relationalen Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="411e5-389">eine angehobene Form der Bediener vorhanden ist, wenn die Operandentypen sind nicht auf NULL festlegbare Werttypen und ist der Ergebnistyp `bool`.</span><span class="sxs-lookup"><span data-stu-id="411e5-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="411e5-390">Die transformierten Form wird durch Hinzufügen eines einzelnen erstellt `?` Modifizierer für die einzelnen Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="411e5-391">Die transformierten Operator gibt den Wert `false` Wenn einer oder beide der Operanden null sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="411e5-392">Andernfalls den transformierten Operator entpackt die Operanden und gilt den zugrunde liegenden Operator erzeugt die `bool` Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="411e5-393">Suche nach Membern</span><span class="sxs-lookup"><span data-stu-id="411e5-393">Member lookup</span></span>

<span data-ttu-id="411e5-394">Eine Suche nach Membern ist der Prozess, bei dem die Bedeutung eines Namens im Kontext eines Typs bestimmt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="411e5-395">Eine Suche nach Membern kann auftreten, als Teil der Auswertung einer *Simple_name* ([einfache Namen](expressions.md#simple-names)) oder ein *Member_access* ([Memberzugriff](expressions.md#member-access)) in ein -Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="411e5-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="411e5-396">Wenn die *Simple_name* oder *Member_access* tritt ein, wenn die *Primary_expression* von einem *Invocation_expression* ([ Methodenaufrufe](expressions.md#method-invocations)), gilt der Member aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="411e5-397">Wenn ein Member eine Methode oder ein Ereignis ist, oder wenn es sich um Konstanten, Felder oder Eigenschaften entweder einen Delegattyp handelt ([Delegaten](delegates.md)) oder den Typ `dynamic` ([der dynamische Typ](types.md#the-dynamic-type)), und klicken Sie dann das Element wird als bezeichnet *aufrufbare*.</span><span class="sxs-lookup"><span data-stu-id="411e5-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="411e5-398">Suche nach Membern berücksichtigt nicht nur den Namen der ein Element, sondern auch die Anzahl von Typparametern, die das Element und gibt an, ob der Member zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="411e5-399">Für die Zwecke der Suche nach Membern generische Methoden und geschachtelten generischen Typen haben die Anzahl von Typparametern, die in ihren jeweiligen Deklarationen angegeben, und alle anderen Elemente haben keine Typparameter.</span><span class="sxs-lookup"><span data-stu-id="411e5-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="411e5-400">Eine Suche nach Membern des Namens eines `N` mit `K`  Typ von Parametern in einem Typ `T` wird wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="411e5-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="411e5-401">Zuerst, eine Reihe von zugängliche Member, die mit dem Namen `N` bestimmt wird:</span><span class="sxs-lookup"><span data-stu-id="411e5-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="411e5-402">Wenn `T` Typparameter ist ein, und klicken Sie dann die Gruppe der Union der Sätze von zugängliche Member, die mit dem Namen ist `N` in jedem der Typen, die als sekundäre Einschränkung oder primary-Einschränkung angegeben ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)) für  `T`, sowie die zugängliche Member, die mit dem Namen `N` in `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="411e5-403">Andernfalls der Satz besteht aus allen zugegriffen werden kann ([Memberzugriff](basic-concepts.md#member-access)) Elemente, die mit dem Namen `N` in `T`, einschließlich der geerbte Member und den zugängliche Member mit dem Namen `N` in `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="411e5-404">Wenn `T` ein konstruierter Typ ist, wird die Menge der Elemente wird abgerufen, indem Typargumente ersetzen, wie in beschrieben [Mitglieder der konstruierte Typen](classes.md#members-of-constructed-types).</span><span class="sxs-lookup"><span data-stu-id="411e5-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="411e5-405">Elemente, die enthalten eine `override` Modifizierer aus der Gruppe ausgeschlossen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="411e5-406">Als Nächstes If `K` ist 0 (null), alle geschachtelten Typen, deren Deklarationen enthalten die Typparameter, werden entfernt.</span><span class="sxs-lookup"><span data-stu-id="411e5-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="411e5-407">Wenn `K` nicht gleich NULL ist, alle Elemente mit einer anderen Anzahl von Parametern entfernt werden, Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="411e5-408">Hinweis: Wenn `K` ist NULL, Methoden mit einem Typ, die Parameter nicht werden, da durch die Herleitung Typ entfernt werden ([Typrückschluss](expressions.md#type-inference)) möglicherweise die Typargumente abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="411e5-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="411e5-409">Weiter, wenn der Member ist *aufgerufen*, allen nicht-*aufrufbare* Mitglieder aus dem Satz entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="411e5-410">Als Nächstes werden die Elemente, die durch andere Member ausgeblendet werden aus der Gruppe entfernt.</span><span class="sxs-lookup"><span data-stu-id="411e5-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="411e5-411">Für jedes Element `S.M` in der Gruppe, in denen `S` ist der Typ, in dem das Element `M` deklariert ist, werden die folgenden Regeln angewendet:</span><span class="sxs-lookup"><span data-stu-id="411e5-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="411e5-412">Wenn `M` ist eine Konstante, ein Feld, Eigenschaft, Ereignis oder -Enumerationsmember, alle Elemente in dem Basistyp deklariert `S` aus der Gruppe entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="411e5-413">Wenn `M` ist eine Typdeklaration, dann alle nicht-Typen, die deklariert in einem Basistyp von `S` werden aus der Gruppe, entfernt und alle Typdeklarationen sind mit der gleichen Anzahl von Typparametern als `M` , die in dem Basistyp deklariert `S` werden entfernt aus dem Satz.</span><span class="sxs-lookup"><span data-stu-id="411e5-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="411e5-414">Wenn `M` ist eine Methode, alle Nichtmethode-Member in dem Basistyp deklariert `S` aus der Gruppe entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="411e5-415">Als Nächstes werden Schnittstellenmember, die von der Klasse, Elemente ausgeblendet werden aus dem Satz entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="411e5-416">Dieser Schritt ist nur wirksam, wenn `T` Typparameter und `T` hat sowohl eine effektive Basisklasse außer `object` und eine effektive nicht leere-Schnittstelle, die festgelegt ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="411e5-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="411e5-417">Für jedes Element `S.M` in der Gruppe, in denen `S` ist der Typ, in dem das Element `M` deklariert ist, werden die folgenden Regeln angewendet werden, wenn `S` ist eine Deklaration der Klasse nicht `object`:</span><span class="sxs-lookup"><span data-stu-id="411e5-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="411e5-418">Wenn `M` ist eine Konstante, ein Feld, Eigenschaft, Ereignis, Enumerationsmember oder Typdeklaration, und klicken Sie dann alle Member, die in der Schnittstellendeklaration einer deklariert, die aus dem Satz entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="411e5-419">Wenn `M` ist eine Methode, alle nicht-Method-Elemente, die in der Schnittstellendeklaration einer deklariert entfernt werden, aus dem Satz und alle Methoden, mit der gleichen Signatur wie `M` deklariert in einer Schnittstelle Deklaration aus dem Satz entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="411e5-420">Dass werden ausgeblendete Elemente entfernt, wird schließlich das Ergebnis der Suche bestimmt werden:</span><span class="sxs-lookup"><span data-stu-id="411e5-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="411e5-421">Wenn die Gruppe ein einzelnes Element, die keine Methode ist besteht, dann ist dieser Member das Ergebnis der Suche.</span><span class="sxs-lookup"><span data-stu-id="411e5-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="411e5-422">Andernfalls, wenn der Satz nur Methoden enthält, ist diese Gruppe von Methoden das Ergebnis der Suche.</span><span class="sxs-lookup"><span data-stu-id="411e5-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="411e5-423">Andernfalls die Suche ist nicht eindeutig, und beim Auftreten eines Laufzeitfehlers-Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="411e5-424">Für Member-Suchvorgänge in andere Typen als Parameter vom Typ und die Schnittstellen und Elementsuchvorgänge in Schnittstellen, die ausschließlich mit einfacher Vererbung (jede Schnittstelle in der Vererbungskette weist genau Null oder eine direkte Basisschnittstelle), wird die Suche nach Regeln einfach abgeleitet, die Member ausblenden Basis Member mit demselben Namen bzw. derselben Signatur.</span><span class="sxs-lookup"><span data-stu-id="411e5-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="411e5-425">Diese Suchvorgänge mit einfacher Vererbung sind stets eindeutig.</span><span class="sxs-lookup"><span data-stu-id="411e5-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="411e5-426">Die Mehrdeutigkeiten, der aus Element-Suchvorgängen in mehrere Vererbungen Schnittstellen möglicherweise auftreten können, werden in beschrieben [Schnittstelle Memberzugriff](interfaces.md#interface-member-access).</span><span class="sxs-lookup"><span data-stu-id="411e5-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="411e5-427">Basistypen</span><span class="sxs-lookup"><span data-stu-id="411e5-427">Base types</span></span>

<span data-ttu-id="411e5-428">Zum Zweck der Suche nach Membern, einen Typ `T` gilt folgenden Basistypen haben:</span><span class="sxs-lookup"><span data-stu-id="411e5-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="411e5-429">Wenn `T` ist `object`, klicken Sie dann `T` keinen Basistyp hat.</span><span class="sxs-lookup"><span data-stu-id="411e5-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="411e5-430">Wenn `T` ist ein *Enum_type*, der Basistypen des `T` Klassentypen sind `System.Enum`, `System.ValueType`, und `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="411e5-431">Wenn `T` ist eine *Struct_type*, der Basistypen des `T` Klassentypen sind `System.ValueType` und `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="411e5-432">Wenn `T` ist eine *Class_type*, der Basistypen des `T` sind die Basisklassen von `T`, einschließlich des Klassentyps `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="411e5-433">Wenn `T` ist ein *Interface_type*, der Basistypen des `T` sind die Basisschnittstellen von `T` und den Klassentyp `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="411e5-434">Wenn `T` ist ein *Array_type*, der Basistypen des `T` Klassentypen sind `System.Array` und `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="411e5-435">Wenn `T` ist eine *Delegate_type*, der Basistypen des `T` Klassentypen sind `System.Delegate` und `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="411e5-436">Funktionsmember</span><span class="sxs-lookup"><span data-stu-id="411e5-436">Function members</span></span>

<span data-ttu-id="411e5-437">Funktionsmember sind Elemente, die ausführbaren Anweisungen enthalten.</span><span class="sxs-lookup"><span data-stu-id="411e5-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="411e5-438">Funktionsmember sind immer Member von Typen und Member von Namespaces nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="411e5-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="411e5-439">C# -Code definiert die folgenden Kategorien von Funktionsmembern:</span><span class="sxs-lookup"><span data-stu-id="411e5-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="411e5-440">Methoden</span><span class="sxs-lookup"><span data-stu-id="411e5-440">Methods</span></span>
*  <span data-ttu-id="411e5-441">Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="411e5-441">Properties</span></span>
*  <span data-ttu-id="411e5-442">Ereignisse</span><span class="sxs-lookup"><span data-stu-id="411e5-442">Events</span></span>
*  <span data-ttu-id="411e5-443">Indexer</span><span class="sxs-lookup"><span data-stu-id="411e5-443">Indexers</span></span>
*  <span data-ttu-id="411e5-444">Benutzerdefinierte Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-444">User-defined operators</span></span>
*  <span data-ttu-id="411e5-445">Instanzkonstruktoren</span><span class="sxs-lookup"><span data-stu-id="411e5-445">Instance constructors</span></span>
*  <span data-ttu-id="411e5-446">Statische Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="411e5-446">Static constructors</span></span>
*  <span data-ttu-id="411e5-447">Destruktoren</span><span class="sxs-lookup"><span data-stu-id="411e5-447">Destructors</span></span>

<span data-ttu-id="411e5-448">Mit Ausnahme von Destruktoren und statische Konstruktoren (die explizit nicht aufgerufen werden können), werden die Anweisungen im Funktionsmember über Funktionsaufrufe-Element ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="411e5-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="411e5-449">Die eigentliche Syntax für das Schreiben von einem Funktionsaufruf für das Element abhängig ist, auf die Memberkategorie bestimmte Funktion.</span><span class="sxs-lookup"><span data-stu-id="411e5-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="411e5-450">Die Argumentliste ([Argumentlisten](expressions.md#argument-lists)) eines Elements für die Funktion bietet Aufruf tatsächliche Werte oder Variablenverweise für die Parameter, der das Funktionselement.</span><span class="sxs-lookup"><span data-stu-id="411e5-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="411e5-451">Aufrufe von generischen Methoden können es sich um den Typrückschluss, um zu bestimmen, den Satz der Argumente des Typs der Methode zu übergebenden einsetzen.</span><span class="sxs-lookup"><span data-stu-id="411e5-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="411e5-452">Dieser Prozess wird hier beschrieben [Typrückschluss](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="411e5-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="411e5-453">Aufrufe von Methoden, Indexer, Operatoren und Instanzkonstruktoren nutzen die Auflösung von funktionsüberladungen ermitteln, welche verschiedenen möglichen Funktionsmember aufrufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="411e5-454">Dieser Prozess wird hier beschrieben [Überladungsauflösung](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="411e5-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="411e5-455">Nachdem eine bestimmte Funktionsmember zum Zeitpunkt der Bindung identifiziert wurde, möglicherweise über die Auflösung von funktionsüberladungen, der tatsächliche Laufzeit-Prozess des Aufrufs der Funktionsmember der Member wird beschrieben in [Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="411e5-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="411e5-456">Die folgende Tabelle enthält die Verarbeitung, die findet in Konstrukten, die im Zusammenhang mit den sechs Kategorien Funktionsmember, die explizit aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="411e5-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="411e5-457">In der Tabelle `e`, `x`, `y`, und `value` klassifizierte Ausdrücke als Variablen oder Werte, `T` gibt einen Ausdruck, der als Typ klassifiziert `F` ist der einfache Name einer Methode, und klicken Sie auf `P` ist der einfache Name einer Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="411e5-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="411e5-458">__Erstellen__</span><span class="sxs-lookup"><span data-stu-id="411e5-458">__Construct__</span></span>     | <span data-ttu-id="411e5-459">__Beispiel__</span><span class="sxs-lookup"><span data-stu-id="411e5-459">__Example__</span></span>    | <span data-ttu-id="411e5-460">__Beschreibung__</span><span class="sxs-lookup"><span data-stu-id="411e5-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="411e5-461">Methodenaufruf</span><span class="sxs-lookup"><span data-stu-id="411e5-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="411e5-462">Überladungsauflösung wird angewendet, um die beste Methode wählen `F` in der enthaltenden Klasse oder Struktur.</span><span class="sxs-lookup"><span data-stu-id="411e5-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="411e5-463">Die Methode wird aufgerufen, mit der Argumentliste `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="411e5-464">Wenn die Methode nicht `static`, ist der Instanzausdruck `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="411e5-465">Überladungsauflösung wird angewendet, um die beste Methode wählen `F` in der Klasse oder Struktur `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="411e5-466">Ein Fehler während der Datenbindung tritt auf, wenn die Methode nicht `static`.</span><span class="sxs-lookup"><span data-stu-id="411e5-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="411e5-467">Die Methode wird aufgerufen, mit der Argumentliste `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="411e5-468">Überladungsauflösung wird angewendet, um die beste Methode F wählen Sie die Klasse, Struktur oder Schnittstelle, die durch den Typ des angegebenen `e`.</span><span class="sxs-lookup"><span data-stu-id="411e5-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="411e5-469">Ein Fehler während der Datenbindung tritt auf, wenn die Methode `static`.</span><span class="sxs-lookup"><span data-stu-id="411e5-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="411e5-470">Die Methode wird aufgerufen, mit dem Instanzausdruck `e` und deren Argumentliste `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="411e5-471">Eigenschaftenzugriff</span><span class="sxs-lookup"><span data-stu-id="411e5-471">Property access</span></span>   | `P`            | <span data-ttu-id="411e5-472">Die `get` -Accessor der Eigenschaft `P` in der enthaltenden Klasse oder Struktur wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="411e5-473">Ein Fehler während der Kompilierung tritt auf, wenn `P` ist lesegeschützt.</span><span class="sxs-lookup"><span data-stu-id="411e5-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="411e5-474">Wenn `P` nicht `static`, ist der Instanzausdruck `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="411e5-475">Die `set` -Accessor der Eigenschaft `P` in der enthaltenden Klasse oder Struktur wird mit der Argumentliste aufgerufen `(value)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="411e5-476">Ein Fehler während der Kompilierung tritt auf, wenn `P` ist schreibgeschützt.</span><span class="sxs-lookup"><span data-stu-id="411e5-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="411e5-477">Wenn `P` nicht `static`, ist der Instanzausdruck `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="411e5-478">Die `get` -Accessor der Eigenschaft `P` in der Klasse oder Struktur `T` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="411e5-479">Ein Fehler während der Kompilierung tritt auf, wenn `P` nicht `static` oder, wenn `P` ist lesegeschützt.</span><span class="sxs-lookup"><span data-stu-id="411e5-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="411e5-480">Die `set` -Accessor der Eigenschaft `P` in der Klasse oder Struktur `T` wird aufgerufen, mit der Argumentliste `(value)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="411e5-481">Ein Fehler während der Kompilierung tritt auf, wenn `P` nicht `static` oder, wenn `P` ist schreibgeschützt.</span><span class="sxs-lookup"><span data-stu-id="411e5-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="411e5-482">Die `get` -Accessor der Eigenschaft `P` in der Klasse, Struktur oder Schnittstelle, die durch den Typ des angegebenen `e` wird aufgerufen, mit dem Instanzausdruck `e`.</span><span class="sxs-lookup"><span data-stu-id="411e5-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="411e5-483">Ein Fehler während der Datenbindung tritt auf, wenn `P` ist `static` oder, wenn `P` ist lesegeschützt.</span><span class="sxs-lookup"><span data-stu-id="411e5-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="411e5-484">Die `set` -Accessor der Eigenschaft `P` in der Klasse, Struktur oder Schnittstelle, die durch den Typ des angegebenen `e` wird aufgerufen, mit dem Instanzausdruck `e` und deren Argumentliste `(value)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="411e5-485">Ein Fehler während der Datenbindung tritt auf, wenn `P` ist `static` oder, wenn `P` ist schreibgeschützt.</span><span class="sxs-lookup"><span data-stu-id="411e5-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="411e5-486">Ereigniszugriff</span><span class="sxs-lookup"><span data-stu-id="411e5-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="411e5-487">Die `add` Accessor des Ereignisses `E` in der enthaltenden Klasse oder Struktur wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="411e5-488">Wenn `E` ist nicht statisch ist, der Instanzausdruck gibt `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="411e5-489">Die `remove` Accessor des Ereignisses `E` in der enthaltenden Klasse oder Struktur wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="411e5-490">Wenn `E` ist nicht statisch ist, der Instanzausdruck gibt `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="411e5-491">Die `add` Accessor des Ereignisses `E` in der Klasse oder Struktur `T` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="411e5-492">Ein Fehler während der Datenbindung tritt auf, wenn `E` ist nicht statisch.</span><span class="sxs-lookup"><span data-stu-id="411e5-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="411e5-493">Die `remove` Accessor des Ereignisses `E` in der Klasse oder Struktur `T` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="411e5-494">Ein Fehler während der Datenbindung tritt auf, wenn `E` ist nicht statisch.</span><span class="sxs-lookup"><span data-stu-id="411e5-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="411e5-495">Die `add` Accessor des Ereignisses `E` in der Klasse, Struktur oder Schnittstelle, die durch den Typ des angegebenen `e` wird aufgerufen, mit dem Instanzausdruck `e`.</span><span class="sxs-lookup"><span data-stu-id="411e5-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="411e5-496">Ein Fehler während der Datenbindung tritt auf, wenn `E` ist statisch.</span><span class="sxs-lookup"><span data-stu-id="411e5-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="411e5-497">Die `remove` Accessor des Ereignisses `E` in der Klasse, Struktur oder Schnittstelle, die durch den Typ des angegebenen `e` wird aufgerufen, mit dem Instanzausdruck `e`.</span><span class="sxs-lookup"><span data-stu-id="411e5-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="411e5-498">Ein Fehler während der Datenbindung tritt auf, wenn `E` ist statisch.</span><span class="sxs-lookup"><span data-stu-id="411e5-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="411e5-499">Indexerzugriff</span><span class="sxs-lookup"><span data-stu-id="411e5-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="411e5-500">Auflösung von funktionsüberladungen wird angewendet, um den besten Indexer in der-Klasse, Struktur oder Schnittstelle erhalten durch den Typ des e auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="411e5-501">Die `get` Accessor des Indexers aufgerufen, mit dem Instanzausdruck `e` und deren Argumentliste `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="411e5-502">Ein Fehler während der Datenbindung tritt auf, wenn der Indexer nur Schreibzugriff ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="411e5-503">Auflösung von funktionsüberladungen zur angewendet wird, wählen Sie den besten Indexer in der Klasse, Struktur oder Schnittstelle, die durch den Typ des angegebenen `e`.</span><span class="sxs-lookup"><span data-stu-id="411e5-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="411e5-504">Die `set` Accessor des Indexers aufgerufen, mit dem Instanzausdruck `e` und deren Argumentliste `(x,y,value)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="411e5-505">Ein Fehler während der Datenbindung tritt auf, wenn der Indexer schreibgeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="411e5-506">Operator-Aufruf</span><span class="sxs-lookup"><span data-stu-id="411e5-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="411e5-507">Auflösung von funktionsüberladungen zur angewendet wird, wählen Sie den besten unäroperator in der Klasse oder Struktur, die durch den Typ des angegebenen `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="411e5-508">Der ausgewählte Operator wird aufgerufen, mit der Argumentliste `(x)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="411e5-509">Auflösung von funktionsüberladungen zur angewendet wird, wählen Sie den am besten binären Operator in Klassen oder Strukturen, die von den Typen der angegebenen `x` und `y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="411e5-510">Der ausgewählte Operator wird aufgerufen, mit der Argumentliste `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="411e5-511">Instanz-Konstruktoraufruf</span><span class="sxs-lookup"><span data-stu-id="411e5-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="411e5-512">Überladungsauflösung wird angewendet, um den besten Instanzkonstruktor wählen Sie in der Klasse oder Struktur `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="411e5-513">Der Instanzkonstruktor wird aufgerufen, mit der Argumentliste `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="411e5-514">Argumentlisten</span><span class="sxs-lookup"><span data-stu-id="411e5-514">Argument lists</span></span>

<span data-ttu-id="411e5-515">Jede Funktion Member und Delegataufruf enthält eine Argumentliste, die tatsächlichen Werte oder Variablenverweise für die Parameter, der das Funktionselement bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="411e5-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="411e5-516">Die Syntax zum Angeben der Argumentliste eines Aufrufs der Funktion Mitglied hängt davon ab, auf die Memberkategorie Funktion:</span><span class="sxs-lookup"><span data-stu-id="411e5-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="411e5-517">Z. B. Konstruktoren, Methoden, Indexer und Delegaten, die Argumente angegeben werden als eine *Argument_list*wie unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="411e5-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="411e5-518">Für Indexer können beim Aufrufen der `set` -Accessor, die Argumentliste darüber hinaus enthält den Ausdruck, der als der Rechte Operand des Zuweisungsoperators angegeben.</span><span class="sxs-lookup"><span data-stu-id="411e5-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="411e5-519">Informationen zu Eigenschaften, die Argumentliste ist leer, beim Aufrufen der `get` Accessor und besteht aus den Ausdruck, der als der Rechte Operand des Zuweisungsoperators angegeben wird, beim Aufrufen der `set` Accessor.</span><span class="sxs-lookup"><span data-stu-id="411e5-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="411e5-520">Für Ereignisse, die Argumentliste des als der Rechte Operand des angegebenen Ausdrucks besteht aus den `+=` oder `-=` Operator.</span><span class="sxs-lookup"><span data-stu-id="411e5-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="411e5-521">Für benutzerdefinierte Operatoren besteht aus die Argumentliste den einzigen Operanden des unären Operators oder beiden Operanden des binären Operators.</span><span class="sxs-lookup"><span data-stu-id="411e5-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="411e5-522">Die Argumente von Eigenschaften ([Eigenschaften](classes.md#properties)), Ereignisse ([Ereignisse](classes.md#events)), und benutzerdefinierte Operatoren ([Operatoren](classes.md#operators)) werden immer als Parameter übergeben ([ -Wertparameter](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="411e5-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="411e5-523">Die Argumente von Indexern ([Indexer](classes.md#indexers)) werden immer als Parameter übergeben ([-Wertparameter](classes.md#value-parameters)) oder Parameterarrays ([Parameterarrays](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="411e5-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="411e5-524">Referenz und Ausgabeparameter werden für diese Kategorien Funktionsmember nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="411e5-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="411e5-525">Die Argumente für einen Instanz-Konstruktor, Methode, Indexer oder Delegat Aufruf angegeben werden, als ein *Argument_list*:</span><span class="sxs-lookup"><span data-stu-id="411e5-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

<span data-ttu-id="411e5-526">Ein *Argument_list* besteht aus einem oder mehreren *Argument*s, die durch Kommas getrennt.</span><span class="sxs-lookup"><span data-stu-id="411e5-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="411e5-527">Jedes Argument besteht aus einem optionalen *Argument_name* gefolgt von einem *Argument_value*.</span><span class="sxs-lookup"><span data-stu-id="411e5-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="411e5-528">Ein *Argument* mit einer *Argument_name* wird als bezeichnet ein ***benanntes Argument***, während ein *Argument* ohne eine  *Argument_name* ist eine ***Positionsargument***.</span><span class="sxs-lookup"><span data-stu-id="411e5-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="411e5-529">Es ist ein Fehler für positionelle Argumente nach der ein benanntes Argument in angezeigt werden ein *Argument_list*.</span><span class="sxs-lookup"><span data-stu-id="411e5-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="411e5-530">Die *Argument_value* kann einen der folgenden Formen annehmen:</span><span class="sxs-lookup"><span data-stu-id="411e5-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="411e5-531">Ein *Ausdruck*, der angibt, der das Argument als ein Value-Parameter übergeben wird ([-Wertparameter](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="411e5-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="411e5-532">Das Schlüsselwort `ref` gefolgt von einem *Variable_reference* ([Variablenverweise](variables.md#variable-references)), der angibt, der das Argument als Verweisparameter übergeben wird ([Verweisparameter ](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="411e5-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="411e5-533">Eine Variable definitiv zugewiesen werden muss ([definitive Zuweisung](variables.md#definite-assignment)), bevor es als Verweisparameter übergeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="411e5-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="411e5-534">Das Schlüsselwort `out` gefolgt von einem *Variable_reference* ([Variablenverweise](variables.md#variable-references)), der angibt, der das Argument als Output-Parameter übergeben wird ([Ausgabeparameter](classes.md#output-parameters)).</span><span class="sxs-lookup"><span data-stu-id="411e5-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="411e5-535">Gilt als eine Variable definitiv zugewiesen ([definitive Zuweisung](variables.md#definite-assignment)) nach einem Member-Funktionsaufruf, in dem die Variable als Output-Parameter übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="411e5-536">Entsprechenden Parameter</span><span class="sxs-lookup"><span data-stu-id="411e5-536">Corresponding parameters</span></span>

<span data-ttu-id="411e5-537">Für jedes Argument in einer Argumentliste es muss ein entsprechender Parameter in der Funktionsmember der Member oder der Delegat aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="411e5-538">Die Parameterliste, die in der folgenden verwendet wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="411e5-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="411e5-539">Virtuelle Methoden und Indexer, die in Klassen definiert die Parameterliste wird aus der Deklaration der spezifischste ausgewählt oder außer Kraft setzen, der das Funktionselement, beginnend mit den statischen Typ des Empfängers, und ihre Basisklassen zu durchsuchen.</span><span class="sxs-lookup"><span data-stu-id="411e5-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="411e5-540">Für Schnittstellenmethoden und Indexer die Liste ausgewählt ist die spezifischste Definition des Elements, beginnen mit dem Schnittstellentyp und das Durchsuchen von Basisschnittstellen bilden.</span><span class="sxs-lookup"><span data-stu-id="411e5-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="411e5-541">Wenn keine unique-Parameter-Liste gefunden wird, wird eine Parameterliste kann nicht zugegriffen werden Namen und keine optionalen Parameter erstellt wird, sodass Aufrufe nicht benannte Parameter verwenden oder optionale Argumente auslassen.</span><span class="sxs-lookup"><span data-stu-id="411e5-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="411e5-542">Für partielle Methoden wird die Liste der definierenden Deklaration der partiellen Methode Parameter verwendet.</span><span class="sxs-lookup"><span data-stu-id="411e5-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="411e5-543">Ist es nur eine einzelner Parameter-Liste, die verwendet wird, für alle anderen Funktionsmember und Delegaten.</span><span class="sxs-lookup"><span data-stu-id="411e5-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="411e5-544">Die Position des Arguments oder Parameter ist als die Anzahl der Argumente oder Parameter in der Argumentliste oder Parameterliste vorhergehenden definiert.</span><span class="sxs-lookup"><span data-stu-id="411e5-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="411e5-545">Die entsprechenden Parameter für die Argumente der Funktion Mitglied werden wie folgt festgelegt:</span><span class="sxs-lookup"><span data-stu-id="411e5-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="411e5-546">Argumente in der *Argument_list* Instanzkonstruktoren, Methoden, Indexer und Delegaten:</span><span class="sxs-lookup"><span data-stu-id="411e5-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="411e5-547">Dieser Parameter entspricht ein Positionsargument, in ein fixed-Parameter an der gleichen Position in der Parameterliste auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="411e5-548">Positionelle Argumente eines Members der Funktion mit einem Parameterarray, das aufgerufen wird, in seiner normalen Form entspricht das Parameterarray, die an der gleichen Position in der Parameterliste erfolgen muss.</span><span class="sxs-lookup"><span data-stu-id="411e5-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="411e5-549">Positionelle Argumente eines Members der Funktion mit einem Parameterarray, das aufgerufen wird, in der erweiterten Form, in denen keine festen Parameter an der gleichen Position in der Parameterliste auftritt, entspricht einem Element im Parameterarray.</span><span class="sxs-lookup"><span data-stu-id="411e5-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="411e5-550">Ein benanntes Argument entspricht der Parameter mit dem gleichen Namen in der Parameterliste.</span><span class="sxs-lookup"><span data-stu-id="411e5-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="411e5-551">Für Indexer können beim Aufrufen der `set` -Accessor, der Ausdruck angegeben, wie der Rechte Operand des Zuweisungsoperators impliziter entspricht `value` Parameter, der die `set` Zugriffsmethoden-Deklaration.</span><span class="sxs-lookup"><span data-stu-id="411e5-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="411e5-552">Informationen zu Eigenschaften, die beim Aufrufen der `get` Accessor gibt es keine Argumente sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="411e5-553">Beim Aufrufen der `set` -Accessor, der Ausdruck angegeben, wie der Rechte Operand des Zuweisungsoperators impliziter entspricht `value` Parameter, der die `set` Zugriffsmethoden-Deklaration.</span><span class="sxs-lookup"><span data-stu-id="411e5-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="411e5-554">Der einzige Parameter der Operatordeklaration entspricht der einzelne Operanden aus, für den benutzerdefinierten unäre Operatoren (einschließlich Konvertierungen).</span><span class="sxs-lookup"><span data-stu-id="411e5-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="411e5-555">Für eine benutzerdefinierte binäre Operatoren der linke Operand entspricht des ersten Parameters, und der Rechte Operand des zweiten Parameters der Operator-Deklaration entspricht.</span><span class="sxs-lookup"><span data-stu-id="411e5-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="411e5-556">Laufzeitauswertung von Argumentenlisten</span><span class="sxs-lookup"><span data-stu-id="411e5-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="411e5-557">Bei der Verarbeitung zur Laufzeit eine Member-Funktionsaufruf ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), die Ausdrücke oder Variablenverweise einer Argumentliste werden in der Reihenfolge von links nach rechts ausgewertet, als Die folgende:</span><span class="sxs-lookup"><span data-stu-id="411e5-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="411e5-558">Für eine Value-Parameter der Argumentausdruck ausgewertet und eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) an den entsprechenden Parameter ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="411e5-559">Der resultierende Wert wird der Anfangswert des Value-Parameters in der Aufruf an.</span><span class="sxs-lookup"><span data-stu-id="411e5-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="411e5-560">Für einen Verweis oder Ausgabe-Parameter der Variable Verweis ausgewertet wird und der daraus resultierende Speicherort der Speicherort, der durch den Parameter im Funktionsaufruf Element dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="411e5-561">Der Variablenverweis als ein Verweis oder Ausgabe-Parameter ist ein Arrayelement einen *Reference_type*, eine laufzeitüberprüfung ausgeführt, um sicherzustellen, dass der Elementtyp des Arrays in den Typ des Parameters identisch ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="411e5-562">Wenn diese Überprüfung fehlschlägt, eine `System.ArrayTypeMismatchException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="411e5-563">Methoden, Indexer und Instanzkonstruktoren deklarieren, können der am weitesten rechts befindlichen-Parameter ein Parameterarray sein ([Parameterarrays](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="411e5-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="411e5-564">Solche Funktionsmember werden aufgerufen, entweder in ihrer normalen Form oder in ihre erweiterte Form, die je nach geltenden ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)):</span><span class="sxs-lookup"><span data-stu-id="411e5-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="411e5-565">Wenn ein Funktionsmember mit einem Parameterarray in seiner normalen Form aufgerufen wird, muss das Argument für das Parameterarray erhält einen einzelnen Ausdruck, der implizit konvertiert werden kann ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den Parameter-Arraytyp.</span><span class="sxs-lookup"><span data-stu-id="411e5-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="411e5-566">In diesem Fall verhält sich das Parameterarray genau wie ein Value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="411e5-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="411e5-567">Wenn ein Funktionsmember mit einem Parameterarray in erweiterter Form aufgerufen wird, muss der Aufruf NULL oder mehr positionelle Argumente für das Parameterarray, wobei jedes Argument ist ein Ausdruck, der implizit konvertiert werden angeben ([implizite Konvertierungen](conversions.md#implicit-conversions)), die den Elementtyp des Parameterarrays.</span><span class="sxs-lookup"><span data-stu-id="411e5-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="411e5-568">In diesem Fall der Aufruf erstellt eine Instanz des Parameterarraytyps mit einer Länge, die Anzahl von Argumenten entspricht, initialisiert die Elemente der Arrayinstanz mit den Werten des angegebenen Arguments und verwendet die neu erstellte Array-Instanz als die tatsächliche Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="411e5-569">Die Ausdrücke einer Argumentliste werden immer in der Reihenfolge ausgewertet, die sie geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="411e5-570">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-570">Thus, the example</span></span>
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
<span data-ttu-id="411e5-571">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="411e5-571">produces the output</span></span>
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="411e5-572">Die Array-Co Varianz-Regeln ([Array-Kovarianz](arrays.md#array-covariance)) zulassen den Wert eines Arraytyps `A[]` sollen einen Verweis auf eine Instanz des Arraytyps `B[]`, sofern eine implizite verweiskonvertierung von vorhanden ist `B` zu `A`.</span><span class="sxs-lookup"><span data-stu-id="411e5-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="411e5-573">Aufgrund dieser Regeln, wenn ein Arrayelement einen *Reference_type* übergeben als Verweis oder Ausgabe, wird eine laufzeitüberprüfung erforderlich, um sicherzustellen, dass der tatsächliche Elementtyp des Arrays, des Parameters entspricht.</span><span class="sxs-lookup"><span data-stu-id="411e5-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="411e5-574">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-574">In the example</span></span>
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
<span data-ttu-id="411e5-575">der zweite Aufruf von `F` bewirkt, dass eine `System.ArrayTypeMismatchException` ausgelöst, weil der tatsächliche Elementtyp des `b` ist `string` und nicht `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="411e5-576">Wenn ein Funktionsmember mit einem Parameterarray in erweiterter Form aufgerufen wird, wird der Aufruf verarbeitet, als ob ein Ausdruck zur Arrayerstellung mit einem Arrayinitialisierer ([Array-Ausdrücke für die Arrayerstellung](expressions.md#array-creation-expressions)) eingefügt wurde, um die Erweiterte Parameter.</span><span class="sxs-lookup"><span data-stu-id="411e5-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="411e5-577">Z. B. im Falle folgender Deklaration</span><span class="sxs-lookup"><span data-stu-id="411e5-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="411e5-578">die folgenden Aufrufe, der die erweiterte Form der Methode</span><span class="sxs-lookup"><span data-stu-id="411e5-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="411e5-579">genau überein mit</span><span class="sxs-lookup"><span data-stu-id="411e5-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="411e5-580">Beachten Sie insbesondere, dass ein leeres Array erstellt wird, wenn keine Argumente angegeben, für das Parameterarray vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="411e5-581">Wenn ein Funktionsmember mit entsprechenden optionalen Parametern Argumente ausgelassen werden, werden die Standardargumente von der Members-Funktionsdeklaration implizit übergeben.</span><span class="sxs-lookup"><span data-stu-id="411e5-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="411e5-582">Da diese immer konstant sind, wirkt sich ihrer Auswertung nicht die Auswertungsreihenfolge der restlichen Argumente.</span><span class="sxs-lookup"><span data-stu-id="411e5-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="411e5-583">Typrückschluss</span><span class="sxs-lookup"><span data-stu-id="411e5-583">Type inference</span></span>

<span data-ttu-id="411e5-584">Wenn eine generische Methode aufgerufen wird, ohne Angabe von Typargumenten, eine ***Typrückschluss*** Prozess versucht, die Typargumente für den Aufruf abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="411e5-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="411e5-585">Das Vorhandensein des Typrückschlusses können eine komfortablere Syntax zum Aufrufen einer generischen Methode verwendet werden soll, und ermöglicht es den Programmierer, vermeiden Sie redundante Informationen angeben.</span><span class="sxs-lookup"><span data-stu-id="411e5-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="411e5-586">Betrachten Sie z. B. die Deklaration der Methode ein:</span><span class="sxs-lookup"><span data-stu-id="411e5-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="411e5-587">Es ist möglich, rufen Sie die `Choose` Methode ohne explizite Angabe ein Argument vom Typ:</span><span class="sxs-lookup"><span data-stu-id="411e5-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="411e5-588">Mithilfe des Typrückschlusses die Typargumente `int` und `string` werden aus den Argumenten der Methode bestimmt.</span><span class="sxs-lookup"><span data-stu-id="411e5-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="411e5-589">Der Typrückschluss tritt als Teil der Bindung-Time-Verarbeitung eines Methodenaufrufs ([Methodenaufrufe](expressions.md#method-invocations)) und erfolgt vor dem Schritt zur überladungsauflösung des Aufrufs.</span><span class="sxs-lookup"><span data-stu-id="411e5-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="411e5-590">Wenn eine bestimmte Methode, die Gruppe in einem Methodenaufruf angegeben ist und keine Typargumente werden als Teil des Methodenaufrufs angegeben, wird ein Typrückschluss auf jede generische Methode in der Methodengruppe angewendet.</span><span class="sxs-lookup"><span data-stu-id="411e5-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="411e5-591">Wenn Typrückschluss erfolgreich ist, werden die abgeleiteten Typargumente verwendet, um die Typen der Argumente für die nachfolgenden überladungsauflösung zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="411e5-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="411e5-592">Wenn der Auflösung von funktionsüberladungen eine generische Methode wie das aufzurufende auswählt, werden dann die abgeleiteten Typargumente als die tatsächliche Typargumente für den Aufruf verwendet.</span><span class="sxs-lookup"><span data-stu-id="411e5-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="411e5-593">Wenn Typrückschluss für eine bestimmte Methode fehlschlägt, diese Methode an der überladungsauflösung nicht teilnehmen.</span><span class="sxs-lookup"><span data-stu-id="411e5-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="411e5-594">Der Ausfall des Typrückschlusses an und für sich selbst, bewirkt einen Fehler während der Bindung nicht.</span><span class="sxs-lookup"><span data-stu-id="411e5-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="411e5-595">Allerdings führt es häufig zu einem Fehler während der Bindung bei der Auflösung von funktionsüberladungen funktioniert nicht mehr alle entsprechenden Methoden zu finden.</span><span class="sxs-lookup"><span data-stu-id="411e5-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="411e5-596">Wenn die angegebene Anzahl von Argumenten anders als die Anzahl von Parametern in der Methode ist, klicken Sie dann Typrückschluss schlägt sofort fehl.</span><span class="sxs-lookup"><span data-stu-id="411e5-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="411e5-597">Andernfalls wird angenommen Sie, dass die generische Methode die folgende Signatur:</span><span class="sxs-lookup"><span data-stu-id="411e5-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="411e5-598">Durch einen Methodenaufruf des Formulars `M(E1...Em)` die Aufgabe des Typrückschlusses besteht darin, eindeutige Typargumente finden `S1...Sn` für jeden der Typparameter `X1...Xn` , damit der Aufruf `M<S1...Sn>(E1...Em)` gültig wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="411e5-599">Bei der Ableitung der einzelnen Typparameter `Xi` ist entweder *festen* auf einen bestimmten Typ `Si` oder *unfixed* mit einem zugeordneten Satz *Grenzen*.</span><span class="sxs-lookup"><span data-stu-id="411e5-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="411e5-600">Jede der Grenzen ist eine Art `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="411e5-601">Zunächst jede Variable vom Typ `Xi` ist mit einem leeren Satz von Grenzen unfixed.</span><span class="sxs-lookup"><span data-stu-id="411e5-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="411e5-602">Typrückschluss erfolgt in mehreren Phasen.</span><span class="sxs-lookup"><span data-stu-id="411e5-602">Type inference takes place in phases.</span></span> <span data-ttu-id="411e5-603">Jede Phase versucht, die Typargumente für weitere basierten auf die Ergebnisse der vorherigen Phase Variablen abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="411e5-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="411e5-604">In der erste Phase macht einige anfängliche Rückschlüsse ziehen, der den angegebenen Begrenzungen, während die zweite Phase Typvariablen auf bestimmte Typen korrigiert und weiter Grenzen leitet.</span><span class="sxs-lookup"><span data-stu-id="411e5-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="411e5-605">Die zweite Phase möglicherweise werden eine Anzahl von Malen wiederholt.</span><span class="sxs-lookup"><span data-stu-id="411e5-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="411e5-606">*Hinweis*: Typ erfolgt Typrückschluss nicht nur, wenn eine generische Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="411e5-607">Typrückschluss für die Konvertierung von Methodengruppen finden Sie im [Typrückschluss für die Konvertierung von Methodengruppen](expressions.md#type-inference-for-conversion-of-method-groups) und suchen den besten common-Typ, der einen Satz von Ausdrücken finden Sie im [Suchen des beste allgemeine Typs einer Gruppe Ausdrücke](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span><span class="sxs-lookup"><span data-stu-id="411e5-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="411e5-608">Die erste phase</span><span class="sxs-lookup"><span data-stu-id="411e5-608">The first phase</span></span>

<span data-ttu-id="411e5-609">Für jede der Methodenargumente `Ei`:</span><span class="sxs-lookup"><span data-stu-id="411e5-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="411e5-610">Wenn `Ei` ist eine anonyme Funktion, ein *expliziter Parameter Typrückschluss* ([expliziter Parameter Typrückschlüsse](expressions.md#explicit-parameter-type-inferences)) besteht aus `Ei` auf `Ti`</span><span class="sxs-lookup"><span data-stu-id="411e5-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="411e5-611">Andernfalls gilt: Wenn `Ei` weist einen Typ `U` und `xi` ist ein Werteparameter ein *untere Begrenzung Typrückschluss* erfolgt *aus* `U` *zu* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="411e5-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="411e5-612">Andernfalls gilt: Wenn `Ei` weist einen Typ `U` und `xi` ist eine `ref` oder `out` Parameter ein *genauer Typrückschluss* erfolgt *aus* `U` *zu* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="411e5-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="411e5-613">Andernfalls erfolgt kein Rückschluss für dieses Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="411e5-614">Die zweite phase</span><span class="sxs-lookup"><span data-stu-id="411e5-614">The second phase</span></span>

<span data-ttu-id="411e5-615">In der zweite Phase wird wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="411e5-616">Alle *unfixed* Variablen des Typs `Xi` der nicht *richten sich nach* ([Abhängigkeit](expressions.md#dependence)) alle `Xj` haben eine feste ([Festsetzung von](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="411e5-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="411e5-617">Wenn kein solcher Typvariablen vorhanden sind, alle *unfixed* Variablen des Typs `Xi` sind *festen* für die alle der folgenden enthalten:</span><span class="sxs-lookup"><span data-stu-id="411e5-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="411e5-618">Es ist mindestens ein Typvariable `Xj` das hängt vom `Xi`</span><span class="sxs-lookup"><span data-stu-id="411e5-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="411e5-619">`Xi` verfügt über ein nicht leerer Satz von Grenzen</span><span class="sxs-lookup"><span data-stu-id="411e5-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="411e5-620">Wenn kein solcher Typvariablen vorhanden sind und es immer noch gibt *unfixed* Variablen des Typs, geben der Typrückschluss schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="411e5-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="411e5-621">Andernfalls, wenn keine weiteren *unfixed* Variablen vom Typ vorhanden ist, wird ein Typrückschluss erfolgreich ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="411e5-622">Andernfalls für alle Argumente `Ei` mit entsprechenden Parametertyps `Ti` , in denen die *Ausgabetypen* ([Ausgabetypen](expressions.md#output-types)) enthalten *unfixed* Variablen vom Typ `Xj` jedoch *Eingabetypen* ([Eingabetypen](expressions.md#input-types)) ist dies kein *Ausgabe Typrückschluss* ([Typrückschlüsse Ausgabe ](expressions.md#output-type-inferences)) erfolgt *aus* `Ei` *zu* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="411e5-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="411e5-623">Anschließend wird die zweite Phase wiederholt.</span><span class="sxs-lookup"><span data-stu-id="411e5-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="411e5-624">Eingabetypen</span><span class="sxs-lookup"><span data-stu-id="411e5-624">Input types</span></span>

<span data-ttu-id="411e5-625">Wenn `E` ist eine Methodengruppe oder anonyme Funktion von implizit typisierten und `T` ist ein Delegat, Typ oder Typ für die Ausdrucksbaumstruktur und die Parametertypen der `T` sind *Eingabetypen* von `E` *mit Typ* `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="411e5-626">Ausgabetypen</span><span class="sxs-lookup"><span data-stu-id="411e5-626">Output types</span></span>

<span data-ttu-id="411e5-627">Wenn `E` ist einer Methodengruppe oder einer anonymen Funktion und `T` ist ein Delegat, Typ oder Typ für die Ausdrucksbaumstruktur und den Rückgabetyp der `T` ist ein *Ausgabetyp der* `E` *mit Typ*  `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="411e5-628">Abhängigkeit</span><span class="sxs-lookup"><span data-stu-id="411e5-628">Dependence</span></span>

<span data-ttu-id="411e5-629">Ein *unfixed* Variablen vom Typ `Xi` *hängt direkt von* eine Variable unfixed vom Typ `Xj` If für einige Argument `Ek` mit Typ `Tk` `Xj` Tritt auf, eine *Eingabetyp* von `Ek` mit Typ `Tk` und `Xi` tritt auf, eine *Ausgabetyp* von `Ek` mit Typ `Tk`.</span><span class="sxs-lookup"><span data-stu-id="411e5-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="411e5-630">`Xj` *hängt von* `Xi` Wenn `Xj` *hängt direkt von* `Xi` oder, wenn `Xi` *hängt direkt von* `Xk` und `Xk` *hängt* `Xj`.</span><span class="sxs-lookup"><span data-stu-id="411e5-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="411e5-631">Daher ist "hängt" transitiv, aber keine reflexive Abschluss von "hängt direkt von".</span><span class="sxs-lookup"><span data-stu-id="411e5-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="411e5-632">Ausgabe Typrückschlüsse</span><span class="sxs-lookup"><span data-stu-id="411e5-632">Output type inferences</span></span>

<span data-ttu-id="411e5-633">Ein *Ausgabe Typrückschluss* erfolgt *aus* eines Ausdrucks `E` *zu* ein `T` auf folgende Weise:</span><span class="sxs-lookup"><span data-stu-id="411e5-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="411e5-634">Wenn `E` ist eine anonyme Funktion hergeleiteten Rückgabetyp `U` ([abgeleiteter Rückgabetyp](expressions.md#inferred-return-type)) und `T` ist ein Delegat oder Typ für die Ausdrucksbaumstruktur mit Rückgabetyp `Tb`, klicken Sie dann eine *untere Begrenzung Typrückschluss* ([untere Begrenzung Rückschlüsse](expressions.md#lower-bound-inferences)) erfolgt *aus* `U` *zu* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="411e5-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="411e5-635">Andernfalls gilt: Wenn `E` ist eine Methodengruppe und `T` ist ein Delegat oder Typ mit den Parametertypen für die Ausdrucksbaumstruktur `T1...Tk` und den Rückgabetyp `Tb`, und die überladungsauflösung von `E` mit den Typen `T1...Tk` ergibt eine einzige Methode, mit dem Rückgabetyp `U`, ein *untere Begrenzung Typrückschluss* erfolgt *aus* `U` *zu* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="411e5-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="411e5-636">Andernfalls gilt: Wenn `E` ist ein Ausdruck mit Typ `U`, ein *untere Begrenzung Typrückschluss* erfolgt *aus* `U` *zu* `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="411e5-637">Andernfalls werden keine Rückschlüsse vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="411e5-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="411e5-638">Expliziter Parameter Typrückschlüsse</span><span class="sxs-lookup"><span data-stu-id="411e5-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="411e5-639">Ein *expliziter Parameter Typrückschluss* erfolgt *aus* eines Ausdrucks `E` *zu* ein `T` auf folgende Weise:</span><span class="sxs-lookup"><span data-stu-id="411e5-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="411e5-640">Wenn `E` ist eine explizit typisierte anonyme Funktion mit den Parametertypen `U1...Uk` und `T` ist ein Delegat oder Typ mit den Parametertypen für die Ausdrucksbaumstruktur `V1...Vk` klicken Sie dann für jeden `Ui` ein *genaue Typrückschluss* ([genauer Rückschlüsse](expressions.md#exact-inferences)) erfolgt *aus* `Ui` *zu* entsprechenden `Vi`.</span><span class="sxs-lookup"><span data-stu-id="411e5-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="411e5-641">Genaue Rückschlüsse</span><span class="sxs-lookup"><span data-stu-id="411e5-641">Exact inferences</span></span>

<span data-ttu-id="411e5-642">Ein *genauer Typrückschluss* *aus* ein `U` *zu* ein `V` erfolgt wie folgt:</span><span class="sxs-lookup"><span data-stu-id="411e5-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="411e5-643">Wenn `V` ist eines der der *unfixed* `Xi` dann `U` hinzugefügt wird, auf den Satz der genauen Grenzen für `Xi`.</span><span class="sxs-lookup"><span data-stu-id="411e5-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="411e5-644">Andernfalls legt `V1...Vk` und `U1...Uk` werden bestimmt, indem Sie überprüfen, ob eine der folgenden Fälle zutrifft:</span><span class="sxs-lookup"><span data-stu-id="411e5-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="411e5-645">`V` ist ein Arraytyp `V1[...]` und `U` ist ein Arraytyp `U1[...]` von den gleichen Rang</span><span class="sxs-lookup"><span data-stu-id="411e5-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="411e5-646">`V` Der Typ `V1?` und `U` ist der Typ `U1?`</span><span class="sxs-lookup"><span data-stu-id="411e5-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="411e5-647">`V` wird von ein konstruierter Typ `C<V1...Vk>`und `U` ein konstruierter Typ ist `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="411e5-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="411e5-648">Wenn es sich bei jedem dieser Fälle wenden Sie dann eine *genauer Typrückschluss* erfolgt *aus* jedes `Ui` *zu* der entsprechenden `Vi`.</span><span class="sxs-lookup"><span data-stu-id="411e5-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="411e5-649">Andernfalls werden keine Rückschlüsse vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="411e5-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="411e5-650">Untere Begrenzung Rückschlüsse</span><span class="sxs-lookup"><span data-stu-id="411e5-650">Lower-bound inferences</span></span>

<span data-ttu-id="411e5-651">Ein *untere Begrenzung Typrückschluss* *aus* ein `U` *zu* ein `V` erfolgt wie folgt:</span><span class="sxs-lookup"><span data-stu-id="411e5-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="411e5-652">Wenn `V` ist eines der der *unfixed* `Xi` dann `U` hinzugefügt wird, auf die unteren Grenzen `Xi`.</span><span class="sxs-lookup"><span data-stu-id="411e5-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="411e5-653">Andernfalls gilt: Wenn `V` ist der Typ `V1?`und `U` ist der Typ `U1?` eine untere Grenze Rückschluss aus erfolgt `U1` zu `V1`.</span><span class="sxs-lookup"><span data-stu-id="411e5-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="411e5-654">Andernfalls legt `U1...Uk` und `V1...Vk` werden bestimmt, indem Sie überprüfen, ob eine der folgenden Fälle zutrifft:</span><span class="sxs-lookup"><span data-stu-id="411e5-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="411e5-655">`V` ist ein Arraytyp `V1[...]` und `U` ist ein Arraytyp `U1[...]` (oder ein Typparameter, dessen effektive Basistyp ist `U1[...]`) von den gleichen Rang</span><span class="sxs-lookup"><span data-stu-id="411e5-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="411e5-656">`V` ist eine der `IEnumerable<V1>`, `ICollection<V1>` oder `IList<V1>` und `U` ist ein eindimensionales Array `U1[]`(oder ein Typparameter, dessen effektive Basistyp ist `U1[]`)</span><span class="sxs-lookup"><span data-stu-id="411e5-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="411e5-657">`V` ist ein konstruierter Typ der Klasse, Struktur, Schnittstelle oder Delegaten `C<V1...Vk>` und es ist ein eindeutiger Typ `C<U1...Uk>` so, dass `U` (oder, wenn Sie `U` ist ein Typparameter, effektive Basisklasse oder ein Mitglied der effektive Satz) ist mit dieser identisch (direkt oder indirekt) erbt, oder implementiert (direkt oder indirekt) `C<U1...Uk>`.</span><span class="sxs-lookup"><span data-stu-id="411e5-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="411e5-658">(Die Einschränkung "Eindeutigkeit" bedeutet, dass in der Groß-/Kleinschreibung Schnittstelle `C<T> {} class U: C<X>, C<Y> {}`, erfolgt kein Rückschluss beim Ableiten von `U` zu `C<T>` da `U1` möglicherweise `X` oder `Y`.)</span><span class="sxs-lookup"><span data-stu-id="411e5-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="411e5-659">Wenn eine dieser Fälle zutrifft, ein Typrückschluss erfolgt *aus* jedes `Ui` *zu* entsprechenden `Vi` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="411e5-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="411e5-660">Wenn `Ui` ist ein Verweistyp ist nicht bekannt wird eine *genauer Typrückschluss* erfolgt</span><span class="sxs-lookup"><span data-stu-id="411e5-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="411e5-661">Andernfalls gilt: Wenn `U` ist ein Arraytyp wird eine *untere Begrenzung Typrückschluss* erfolgt</span><span class="sxs-lookup"><span data-stu-id="411e5-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="411e5-662">Andernfalls gilt: Wenn `V` ist `C<V1...Vk>` und klicken Sie dann die i-th-Typparameter der Rückschluss abhängt `C`:</span><span class="sxs-lookup"><span data-stu-id="411e5-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="411e5-663">Wenn es kovariant ist ein *untere Begrenzung Typrückschluss* erfolgt.</span><span class="sxs-lookup"><span data-stu-id="411e5-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="411e5-664">Wenn es sich um kontravariant ist ein *Obergrenze Typrückschluss* erfolgt.</span><span class="sxs-lookup"><span data-stu-id="411e5-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="411e5-665">Wenn er unveränderlich ist ein *genauer Typrückschluss* erfolgt.</span><span class="sxs-lookup"><span data-stu-id="411e5-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="411e5-666">Andernfalls werden keine Rückschlüsse vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="411e5-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="411e5-667">Obere Grenze Rückschlüsse</span><span class="sxs-lookup"><span data-stu-id="411e5-667">Upper-bound inferences</span></span>

<span data-ttu-id="411e5-668">Ein *Obergrenze Typrückschluss* *aus* ein `U` *zu* ein `V` erfolgt wie folgt:</span><span class="sxs-lookup"><span data-stu-id="411e5-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="411e5-669">Wenn `V` ist eines der der *unfixed* `Xi` dann `U` hinzugefügt wird, auf die oberen Grenzen `Xi`.</span><span class="sxs-lookup"><span data-stu-id="411e5-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="411e5-670">Andernfalls legt `V1...Vk` und `U1...Uk` werden bestimmt, indem Sie überprüfen, ob eine der folgenden Fälle zutrifft:</span><span class="sxs-lookup"><span data-stu-id="411e5-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="411e5-671">`U` ist ein Arraytyp `U1[...]` und `V` ist ein Arraytyp `V1[...]` von den gleichen Rang</span><span class="sxs-lookup"><span data-stu-id="411e5-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="411e5-672">`U` ist eine der `IEnumerable<Ue>`, `ICollection<Ue>` oder `IList<Ue>` und `V` ist ein eindimensionales Array-Typ `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="411e5-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="411e5-673">`U` Der Typ `U1?` und `V` ist der Typ `V1?`</span><span class="sxs-lookup"><span data-stu-id="411e5-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="411e5-674">`U` ist der konstruierten Klasse, Struktur, Schnittstelle oder ein Delegattyp `C<U1...Uk>` und `V` wird von einer Klasse, Struktur, Schnittstellen oder Delegate-Typ, der mit erbt (direkt oder indirekt) identisch ist oder implementiert (direkt oder indirekt) ein eindeutiger Typ `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="411e5-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="411e5-675">(Die Einschränkung "Eindeutigkeit" bedeutet, dass bei wir `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, erfolgt kein Rückschluss beim Ableiten von `C<U1>` zu `V<Q>`.</span><span class="sxs-lookup"><span data-stu-id="411e5-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="411e5-676">Rückschlüsse erfolgen keine `U1` entweder `X<Q>` oder `Y<Q>`.)</span><span class="sxs-lookup"><span data-stu-id="411e5-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="411e5-677">Wenn eine dieser Fälle zutrifft, ein Typrückschluss erfolgt *aus* jedes `Ui` *zu* entsprechenden `Vi` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="411e5-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="411e5-678">Wenn `Ui` ist ein Verweistyp ist nicht bekannt wird eine *genauer Typrückschluss* erfolgt</span><span class="sxs-lookup"><span data-stu-id="411e5-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="411e5-679">Andernfalls gilt: Wenn `V` ist ein Arraytyp wird eine *Obergrenze Typrückschluss* erfolgt</span><span class="sxs-lookup"><span data-stu-id="411e5-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="411e5-680">Andernfalls gilt: Wenn `U` ist `C<U1...Uk>` und klicken Sie dann die i-th-Typparameter der Rückschluss abhängt `C`:</span><span class="sxs-lookup"><span data-stu-id="411e5-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="411e5-681">Wenn es kovariant ist ein *Obergrenze Typrückschluss* erfolgt.</span><span class="sxs-lookup"><span data-stu-id="411e5-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="411e5-682">Wenn es sich um kontravariant ist ein *untere Begrenzung Typrückschluss* erfolgt.</span><span class="sxs-lookup"><span data-stu-id="411e5-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="411e5-683">Wenn er unveränderlich ist ein *genauer Typrückschluss* erfolgt.</span><span class="sxs-lookup"><span data-stu-id="411e5-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="411e5-684">Andernfalls werden keine Rückschlüsse vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="411e5-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="411e5-685">Beheben von</span><span class="sxs-lookup"><span data-stu-id="411e5-685">Fixing</span></span>

<span data-ttu-id="411e5-686">Ein *unfixed* Variablen vom Typ `Xi` ist mit einem Satz von Grenzen *festen* wie folgt:</span><span class="sxs-lookup"><span data-stu-id="411e5-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="411e5-687">Der Satz von *potenzieller Typen* `Uj` als Satz aller Typen in den Satz von Grenzen für beginnt `Xi`.</span><span class="sxs-lookup"><span data-stu-id="411e5-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="411e5-688">Klicken Sie dann untersuchen wir jede Grenze für `Xi` wiederum: Für jede genau gebundene `U` von `Xi` alle Typen `Uj` die sind nicht identisch mit `U` werden aus dem Satz von Kandidaten entfernt.</span><span class="sxs-lookup"><span data-stu-id="411e5-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="411e5-689">Für jede untere Grenze `U` von `Xi` alle Typen `Uj` ist auf die dort *nicht* eine implizite Konvertierung von `U` werden aus dem Satz von Kandidaten entfernt.</span><span class="sxs-lookup"><span data-stu-id="411e5-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="411e5-690">Für jede Obergrenze `U` von `Xi` alle Typen `Uj` wird die dort *nicht* eine implizite Konvertierung in `U` werden aus dem Satz von Kandidaten entfernt.</span><span class="sxs-lookup"><span data-stu-id="411e5-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="411e5-691">If zu den Typen der verbleibenden Candidate `Uj` es ist ein eindeutiger Typ `V` von dem es eine implizite Konvertierung auf alle anderen möglichen Typen, klicken Sie dann `Xi` ist fest in `V`.</span><span class="sxs-lookup"><span data-stu-id="411e5-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="411e5-692">Andernfalls schlägt die Typrückschluss fehl.</span><span class="sxs-lookup"><span data-stu-id="411e5-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="411e5-693">Abgeleiteter Rückgabetyp</span><span class="sxs-lookup"><span data-stu-id="411e5-693">Inferred return type</span></span>

<span data-ttu-id="411e5-694">Die hergeleiteten Rückgabetyp von einer anonymen Funktion `F` wird während der typauflösung inferenz und die Überladung verwendet.</span><span class="sxs-lookup"><span data-stu-id="411e5-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="411e5-695">Der hergeleitete Rückgabetyp kann nur für eine anonyme Funktion, in denen alle Parameter, den Typen bekannt sind, entweder weil sie explizit angegeben sind, über eine anonyme Funktion Konvertierung bereitgestellt oder abgeleitet wird, während der Typrückschluss in einer einschließenden generischen, ermittelt werden Methodenaufruf.</span><span class="sxs-lookup"><span data-stu-id="411e5-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="411e5-696">Die ***abgeleitet Ergebnistyp*** wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="411e5-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="411e5-697">Wenn der Text der `F` ist ein *Ausdruck* , bei dem ein Typ, und klicken Sie dann auf die hergeleiteten Rückgabetyp des `F` ist der Typ dieses Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="411e5-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="411e5-698">Wenn der Text der `F` ist eine *Block* und der Satz von Ausdrücken im Block `return` -Anweisungen verfügt auch über einen bewährten gemeinsamen Typ `T` ([Suchen nach den besten common-Typ, der einen Satz von Ausdrücken](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), klicken Sie dann die hergeleiteten Rückgabetyp des `F` ist `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="411e5-699">Andernfalls kann kein Ergebnistyp abgeleitet werden, für die `F`.</span><span class="sxs-lookup"><span data-stu-id="411e5-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="411e5-700">Die ***Rückgabetyp abgeleitet*** wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="411e5-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="411e5-701">Wenn `F` ist asynchron und der Text der `F` ist entweder ein Ausdruck als "nothing" klassifiziert sind ([ausdrucksklassifizierungen](expressions.md#expression-classifications)), oder einen Anweisungsblock, die keine return-Anweisungen, in dem Ausdrücke haben, die hergeleiteten Rückgabetyp ist `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="411e5-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="411e5-702">Wenn `F` Async, verfügt über eine abgeleitete Ergebnistyp `T`, die hergeleiteten Rückgabetyp ist `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="411e5-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="411e5-703">Wenn `F` nicht asynchronen, verfügt über eine abgeleitete Ergebnistyp `T`, die hergeleiteten Rückgabetyp ist `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="411e5-704">Andernfalls kann kein Rückgabetyp abgeleitet werden, für die `F`.</span><span class="sxs-lookup"><span data-stu-id="411e5-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="411e5-705">Betrachten Sie als ein Beispiel für den Typrückschluss, die im Zusammenhang mit anonymen Funktionen, die `Select` Erweiterungsmethode deklariert wird, der `System.Linq.Enumerable` Klasse:</span><span class="sxs-lookup"><span data-stu-id="411e5-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

<span data-ttu-id="411e5-706">Vorausgesetzt die `System.Linq` Namespace importiert wurde, mit einer `using` -Klausel, und erhalten Sie eine Klasse `Customer` mit eine `Name` Eigenschaft vom Typ `string`, `Select` Methode kann verwendet werden, um die Namen der eine Liste der Kunden auswählen:</span><span class="sxs-lookup"><span data-stu-id="411e5-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="411e5-707">Der Aufruf der Erweiterungsmethode ([Erweiterung Methodenaufrufe](expressions.md#extension-method-invocations)) der `Select` verarbeitet wird, durch den Aufruf zu Aufruf einer statischen Methode umschreiben:</span><span class="sxs-lookup"><span data-stu-id="411e5-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="411e5-708">Da Sie nicht explizit Typargumente angegeben wurden, werden den Typrückschluss die Typargumente abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="411e5-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="411e5-709">Zuerst die `customers` Argument bezieht sich auf die `source` Parameter Ableiten von `T` sein `Customer`.</span><span class="sxs-lookup"><span data-stu-id="411e5-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="411e5-710">Mithilfe der anonymen Funktion geben Sie dann oben beschriebenen Rückschlussprozess `c` erhält den Typ `Customer`, und der Ausdruck `c.Name` bezieht sich auf den Rückgabetyp der `selector` Parameter Ableiten von `S` sein`string`.</span><span class="sxs-lookup"><span data-stu-id="411e5-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="411e5-711">Daher ist der Aufruf entspricht</span><span class="sxs-lookup"><span data-stu-id="411e5-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="411e5-712">und das Ergebnis ist vom Typ `IEnumerable<string>`.</span><span class="sxs-lookup"><span data-stu-id="411e5-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="411e5-713">Im folgende Beispiel wird veranschaulicht, wie anonyme Funktionstyp Typrückschluss ermöglicht die Typinformationen für "flow zwischen Argumenten im Aufruf einer generischen Methode".</span><span class="sxs-lookup"><span data-stu-id="411e5-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="411e5-714">Betrachten Sie die Methode ein:</span><span class="sxs-lookup"><span data-stu-id="411e5-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="411e5-715">Der Typrückschluss für den Aufruf:</span><span class="sxs-lookup"><span data-stu-id="411e5-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="411e5-716">wird wie folgt: Zunächst wird das Argument `"1:15:30"` bezieht sich auf die `value` Parameter Ableiten von `X` sein `string`.</span><span class="sxs-lookup"><span data-stu-id="411e5-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="411e5-717">Klicken Sie dann, den Parameter der ersten anonymen Funktion `s`, erhält den abgeleiteten Typ `string`, und der Ausdruck `TimeSpan.Parse(s)` bezieht sich auf den Rückgabetyp der `f1`, ableiten von `Y` sein `System.TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="411e5-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="411e5-718">Abschließend den Parameter der zweiten anonyme Funktion `t`, erhält den abgeleiteten Typ `System.TimeSpan`, und der Ausdruck `t.TotalSeconds` bezieht sich auf den Rückgabetyp der `f2`, ableiten von `Z` sein `double`.</span><span class="sxs-lookup"><span data-stu-id="411e5-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="411e5-719">Daher ist das Ergebnis des Aufrufs, vom Typ `double`.</span><span class="sxs-lookup"><span data-stu-id="411e5-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="411e5-720">Typrückschluss für die Konvertierung von Methodengruppen</span><span class="sxs-lookup"><span data-stu-id="411e5-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="411e5-721">Ähnlich wie Aufrufe der Methoden, Typrückschluss muss auch angewendet, wenn eine Methodengruppe `M` einer generischen Methode mit einem bestimmten Delegattyp konvertiert `D` ([Gruppe Konvertierungen](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="411e5-722">Wenn eine Methode</span><span class="sxs-lookup"><span data-stu-id="411e5-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="411e5-723">und die Methodengruppe `M` zugewiesen wird, in den Delegattyp `D` die Aufgabe des Typrückschlusses besteht darin, finden Sie Typargumente `S1...Sn` , damit der Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="411e5-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="411e5-724">kompatibel ist ([delegieren Deklarationen](delegates.md#delegate-declarations)) mit `D`.</span><span class="sxs-lookup"><span data-stu-id="411e5-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="411e5-725">Im Gegensatz zum Typ Typrückschluss für generische Methodenaufrufen, in diesem Fall stehen nur Argument *Typen*, kein Argument *Ausdrücke*.</span><span class="sxs-lookup"><span data-stu-id="411e5-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="411e5-726">Vor allem keine anonymen Funktionen vorhanden sind und daher keine Notwendigkeit, mehrere Phasen der Typrückschluss.</span><span class="sxs-lookup"><span data-stu-id="411e5-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="411e5-727">Stattdessen alle `Xi` gelten *unfixed*, und ein *untere Begrenzung Typrückschluss* erfolgt *aus* jeder Argumenttyp `Uj` von `D` *zu* entsprechenden Parametertyps `Tj` von `M`.</span><span class="sxs-lookup"><span data-stu-id="411e5-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="411e5-728">If für eines der `Xi` keine Grenzen gefunden, der Typrückschluss schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="411e5-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="411e5-729">Andernfalls alle `Xi` sind *behoben* entsprechenden `Si`, die das Ergebnis des Typrückschlusses sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="411e5-730">Suchen den besten common-Typ, der einen Satz von Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="411e5-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="411e5-731">In einigen Fällen muss kein gemeinsamer Typ für einen Satz von Ausdrücken abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="411e5-732">Insbesondere die Elementtypen eines implizit typisierte Arrays und die Rückgabetypen der anonymen Funktionen mit *Block* Stellen auf diese Weise gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="411e5-733">Intuitiv anhand eines Satzes von Ausdrücken `E1...Em` dieser Rückschluss muss dem Aufruf einer Methode</span><span class="sxs-lookup"><span data-stu-id="411e5-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="411e5-734">mit der `Ei` als Argumente.</span><span class="sxs-lookup"><span data-stu-id="411e5-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="411e5-735">Genauer gesagt: die Ableitung beginnt mit einem *unfixed* Variablen vom Typ `X`.</span><span class="sxs-lookup"><span data-stu-id="411e5-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="411e5-736">*Ausgabe Typrückschlüsse* werden durchgeführt, *aus* jedes `Ei` *zu* `X`.</span><span class="sxs-lookup"><span data-stu-id="411e5-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="411e5-737">Zum Schluss `X` ist *festen* und geben Sie bei erfolgreicher Ausführung der resultierenden `S` ist der resultierende bewährte gemeinsame Typ für die Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="411e5-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="411e5-738">Wenn keine solche `S` vorhanden ist, die Ausdrücke besitzen kein optimaler Typ für die allgemeine.</span><span class="sxs-lookup"><span data-stu-id="411e5-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="411e5-739">Auflösung von funktionsüberladungen</span><span class="sxs-lookup"><span data-stu-id="411e5-739">Overload resolution</span></span>

<span data-ttu-id="411e5-740">Überladungsauflösung ist eine Bindung-Time-Mechanismus für die Auswahl der am besten geeigneten Funktionsmembers angegebenen Argumentliste und einen Satz von möglichen Funktionsmembern aufrufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="411e5-741">Wählt die überladungsauflösung die Funktionsmember der Member in den folgenden unterschiedlichen Kontexten in C# -Code aufrufen:</span><span class="sxs-lookup"><span data-stu-id="411e5-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="411e5-742">Aufruf einer Methode, die mit dem Namen in einem *Invocation_expression* ([Methodenaufrufe](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="411e5-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="411e5-743">Aufruf eines Instanzkonstruktors mit dem Namen in einem *Object_creation_expression* ([Erstellung Objektausdrücke](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="411e5-744">Aufruf eines Indexeraccessors über eine *Element_access* ([Elementzugriff](expressions.md#element-access)).</span><span class="sxs-lookup"><span data-stu-id="411e5-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="411e5-745">Aufruf eines vordefinierten oder benutzerdefinierten Operators in einem Ausdruck verwiesen wird ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution) und [binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="411e5-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="411e5-746">Alle diese Kontexte definiert den Satz von möglichen Funktionsmembern und die Liste der Argumente in ihre eigenen Methoden, wie in den oben aufgeführten Abschnitten ausführlich beschrieben.</span><span class="sxs-lookup"><span data-stu-id="411e5-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="411e5-747">Der Satz von Kandidaten für einen Methodenaufruf enthält z. B. keine markierte Methoden `override` ([Membersuche](expressions.md#member-lookup)), und Methoden in einer Basisklasse sind nicht Kandidaten, wenn es sich bei einer beliebigen Methode in einer abgeleiteten Klasse gilt ([ Methodenaufrufe](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="411e5-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="411e5-748">Nach der möglichen Funktionsmembern und deren Argumentliste identifiziert wurden, ist die Auswahl der am besten geeigneten Funktionsmembers in allen Fällen identisch:</span><span class="sxs-lookup"><span data-stu-id="411e5-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="411e5-749">Die am besten, die den Satz von anwendbaren möglichen Funktionsmembern wird angegeben, Funktion die Member, Satz befindet.</span><span class="sxs-lookup"><span data-stu-id="411e5-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="411e5-750">Wenn der Satz nur einen Funktionsmember enthält, ist dieser Funktionsmember die am besten geeigneten Funktionsmembers.</span><span class="sxs-lookup"><span data-stu-id="411e5-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="411e5-751">Andernfalls ist die am besten geeigneten Funktionsmembers Funktionsmember, das besser als andere Funktionsmember hinsichtlich der angegebenen Argumentliste, vorausgesetzt, dass jeder Funktionsmember verglichen wird, für alle Mitglieder anderer Funktion mit den Regeln in [ Geeigneterer Funktionsmember](expressions.md#better-function-member).</span><span class="sxs-lookup"><span data-stu-id="411e5-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="411e5-752">Wenn nicht genau ein Funktionsmember, das besser als andere Funktionsmember vorhanden ist, klicken Sie dann der Element-Funktionsaufruf ist mehrdeutig, und beim Auftreten eines Laufzeitfehlers-Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="411e5-753">Die folgenden Abschnitte definieren die genauen Bedeutungen der Begriffe ***anwendbarer Funktionsmember*** und ***geeigneterer Funktionsmember***.</span><span class="sxs-lookup"><span data-stu-id="411e5-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="411e5-754">Anwendbarer Funktionsmember</span><span class="sxs-lookup"><span data-stu-id="411e5-754">Applicable function member</span></span>

<span data-ttu-id="411e5-755">Ein Funktionsmember gilt eine ***anwendbarer Funktionsmember*** in Bezug auf eine Argumentliste `A` wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="411e5-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="411e5-756">Jedes Argument im `A` entspricht einem Parameter in der Funktionsmemberdeklaration Siehe [entsprechende Parameter](expressions.md#corresponding-parameters), und alle Parameter, der kein Argument entspricht, ist ein optionaler Parameter.</span><span class="sxs-lookup"><span data-stu-id="411e5-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="411e5-757">Für jedes Argument im `A`, den Modus des Arguments für die Parameterübergabe (d. h. Wert `ref`, oder `out`) ist identisch mit dem Parameter übergebenden Modus des entsprechenden Parameters und</span><span class="sxs-lookup"><span data-stu-id="411e5-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="411e5-758">für eine Value-Parameter oder ein Parameterarray, eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) aus dem Argument vorhanden ist, in den Typ des entsprechenden Parameters oder</span><span class="sxs-lookup"><span data-stu-id="411e5-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="411e5-759">für eine `ref` oder `out` Parameter, der der Typ des Arguments ist identisch mit den Typ des entsprechenden Parameters.</span><span class="sxs-lookup"><span data-stu-id="411e5-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="411e5-760">Schließlich eine `ref` oder `out` Parameter ist ein Alias für das übergebene Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="411e5-761">Für eine Funktionselement, das ein Parameterarray enthält, wenn der Funktionsmember anhand der obigen Regeln gilt, gilt in angewendet werden. die ***Normalform***.</span><span class="sxs-lookup"><span data-stu-id="411e5-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="411e5-762">Ist ein Funktionselement, das ein Parameterarray enthält nicht anwendbar, in seiner normalen Form, das Funktionselement kann stattdessen fallen in die ***erweitert Formular***:</span><span class="sxs-lookup"><span data-stu-id="411e5-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="411e5-763">Diese erweiterte Form wird erstellt, indem Sie ersetzt das Parameterarray in der Funktionsmemberdeklaration mit 0 (null) oder ein array, z. B. weitere Parameter der Wert des Elementtyps des Parameters, die Anzahl der Argumente in der Argumentliste `A` entspricht die Summe Anzahl von Parametern.</span><span class="sxs-lookup"><span data-stu-id="411e5-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="411e5-764">Wenn `A` weniger Argumente als die Anzahl der festen Parameter in der Funktionsmemberdeklaration enthält, ist die erweiterte Form der Funktionsmember der Member kann nicht erstellt werden und ist somit nicht verwendbar.</span><span class="sxs-lookup"><span data-stu-id="411e5-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="411e5-765">Andernfalls ist diese erweiterte Form anwendbar, wenn Sie für jedes Argument im `A` den Parameter übergebenden Modus des Arguments ist identisch mit dem Parameter übergebenden Modus des entsprechenden Parameters und</span><span class="sxs-lookup"><span data-stu-id="411e5-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="411e5-766">für einen festen Wert-Parameter oder eine Value-Parameter, die durch die Erweiterung, die eine implizite Konvertierung erzeugt ([implizite Konvertierungen](conversions.md#implicit-conversions)) vorhanden ist aus dem Typ des Arguments in den Typ des entsprechenden Parameters oder</span><span class="sxs-lookup"><span data-stu-id="411e5-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="411e5-767">für eine `ref` oder `out` Parameter, der der Typ des Arguments ist identisch mit den Typ des entsprechenden Parameters.</span><span class="sxs-lookup"><span data-stu-id="411e5-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="411e5-768">Geeigneterer Funktionsmember</span><span class="sxs-lookup"><span data-stu-id="411e5-768">Better function member</span></span>

<span data-ttu-id="411e5-769">Im Rahmen den geeignetere Funktionsmember festlegen wird eine reduzierte Argumentliste A erstellt, enthält nur die Argumentausdrücken selbst in der Reihenfolge, die sie in der ursprünglichen Argumentliste angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="411e5-770">Parameterlisten für jeden die Candidate-Funktion, die Mitglieder auf folgende Weise erstellt werden:</span><span class="sxs-lookup"><span data-stu-id="411e5-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="411e5-771">Diese erweiterte Form wird verwendet, wenn das Funktionselement betrifft nur die erweiterte Form wurde.</span><span class="sxs-lookup"><span data-stu-id="411e5-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="411e5-772">Optionale Parameter ohne entsprechenden Argumente werden aus der Liste entfernt.</span><span class="sxs-lookup"><span data-stu-id="411e5-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="411e5-773">Die Parameter sind neu angeordnet, damit sie an der gleichen Position wie das entsprechende Argument in der Argumentliste auftreten.</span><span class="sxs-lookup"><span data-stu-id="411e5-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="411e5-774">Eine Argumentliste `A` mit einem Satz von Argumentausdrücken `{E1, E2, ..., En}` und zwei anwendbaren Funktionsmembern `Mp` und `Mq` mit Parametertypen `{P1, P2, ..., Pn}` und `{Q1, Q2, ..., Qn}`, `Mp` definiert eine ***geeigneterer Funktionsmember*** als `Mq` Wenn</span><span class="sxs-lookup"><span data-stu-id="411e5-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="411e5-775">für jedes Argument, das die implizite Konvertierung von `Ex` zu `Qx` ist nicht besser als die implizite Konvertierung von `Ex` zu `Px`, und</span><span class="sxs-lookup"><span data-stu-id="411e5-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="411e5-776">für mindestens ein Argument, um die Konvertierung von `Ex` zu `Px` ist besser als die Konvertierung von `Ex` zu `Qx`.</span><span class="sxs-lookup"><span data-stu-id="411e5-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="411e5-777">Beim Ausführen dieser Evaluierung Wenn `Mp` oder `Mq` gilt in der erweiterten Form dann `Px` oder `Qx` bezieht sich auf einen Parameter in die erweiterte Form der Parameterliste.</span><span class="sxs-lookup"><span data-stu-id="411e5-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="411e5-778">Für den Fall, dass der Typparameter Sequenzen `{P1, P2, ..., Pn}` und `{Q1, Q2, ..., Qn}` äquivalent sind (d. h. jede `Pi` verfügt über eine identitätskonvertierung in den entsprechenden `Qi`), die folgenden Regeln für die aktuelle Verbindung angewendet werden, in der Reihenfolge, um zu bestimmen, desto besser Funktionsmember.</span><span class="sxs-lookup"><span data-stu-id="411e5-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="411e5-779">Wenn `Mp` ist eine nicht generische Methode und `Mq` ist eine generische Methode `Mp` ist besser als `Mq`.</span><span class="sxs-lookup"><span data-stu-id="411e5-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="411e5-780">Andernfalls gilt: Wenn `Mp` gilt in seiner normalen Form und `Mq` verfügt über eine `params` array und gilt nur in der erweiterten Form dann `Mp` ist besser als `Mq`.</span><span class="sxs-lookup"><span data-stu-id="411e5-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="411e5-781">Andernfalls gilt: Wenn `Mp` verfügt über mehrere deklarierte Parameter als `Mq`, klicken Sie dann `Mp` ist besser als `Mq`.</span><span class="sxs-lookup"><span data-stu-id="411e5-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="411e5-782">Dies kann auftreten, wenn beide Methoden haben `params` Arrays und sind nur in ihre erweiterte Formate angewendet.</span><span class="sxs-lookup"><span data-stu-id="411e5-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="411e5-783">Andernfalls Wenn alle Parameter der `Mp` ein entsprechenden Arguments haben, während die Standardargumente für mindestens ein optionaler Parameter in ersetzt werden müssen `Mq` dann `Mp` ist besser als `Mq`.</span><span class="sxs-lookup"><span data-stu-id="411e5-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="411e5-784">Andernfalls gilt: Wenn `Mp` hat spezifische Parametertypen als `Mq`, klicken Sie dann `Mp` ist besser als `Mq`.</span><span class="sxs-lookup"><span data-stu-id="411e5-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="411e5-785">Lassen Sie `{R1, R2, ..., Rn}` und `{S1, S2, ..., Sn}` die nicht instanziierte und nicht erweiterten Parametertypen darstellen `Mp` und `Mq`.</span><span class="sxs-lookup"><span data-stu-id="411e5-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="411e5-786">`Mp`die Parametertypen sind präziser als `Mq`des If, für jeden Parameter `Rx` ist nicht als weniger spezifischen `Sx`, und für mindestens ein Parameter `Rx` sind präziser als `Sx`:</span><span class="sxs-lookup"><span data-stu-id="411e5-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="411e5-787">Ein Typparameter ist weniger spezifisch als Nichttyp-Parameter.</span><span class="sxs-lookup"><span data-stu-id="411e5-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="411e5-788">Rekursiv, bezieht sich ein konstruierter Typ mehr als einen anderen konstruierten Typ (mit derselben Anzahl von Typargumenten), wenn mindestens eine Typargument spezifischere ist und keine Typargument weniger spezifisch als das entsprechende Typargument in der anderen ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="411e5-789">Ein Arraytyp ist spezifischer als einen anderen Arraytyp (mit derselben Anzahl von Dimensionen), wenn der Typ des Elements der ersten spezifischer als der Elementtyp der zweiten ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="411e5-790">Andernfalls ist, wenn ein Element ein Operator nicht aufgehoben ist, und die andere ein transformierten Operator ist, die nicht transformiert eine besser.</span><span class="sxs-lookup"><span data-stu-id="411e5-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="411e5-791">Andernfalls ist keine Funktionsmember besser.</span><span class="sxs-lookup"><span data-stu-id="411e5-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="411e5-792">Bessere Konvertierung von Ausdruck</span><span class="sxs-lookup"><span data-stu-id="411e5-792">Better conversion from expression</span></span>

<span data-ttu-id="411e5-793">Erhalten eine implizite Konvertierung `C1` , der von einem Ausdruck konvertiert `E` auf einen Typ `T1`, und eine implizite Konvertierung `C2` , der von einem Ausdruck konvertiert `E` auf einen Typ `T2`, `C1` ist eine ***bessere Konvertierung*** als `C2` Wenn `E` entspricht nicht genau `T2` und mindestens eine der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="411e5-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="411e5-794">`E` genau übereinstimmt `T1` ([genau übereinstimmenden Ausdrucks](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="411e5-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="411e5-795">`T1` ist eine bessere Konvertierung Ziel als `T2` ([besser Konvertierung ausrichten](expressions.md#better-conversion-target))</span><span class="sxs-lookup"><span data-stu-id="411e5-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="411e5-796">-Ausdruck genau entsprechen.</span><span class="sxs-lookup"><span data-stu-id="411e5-796">Exactly matching Expression</span></span>

<span data-ttu-id="411e5-797">Erhält einen Ausdruck `E` und einen Typ `T`, `E` genau Übereinstimmungen `T` , wenn eine der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="411e5-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="411e5-798">`E` weist einen Typ `S`, und eine identitätskonvertierung vorhanden ist, von `S` auf `T`</span><span class="sxs-lookup"><span data-stu-id="411e5-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="411e5-799">`E` ist eine anonyme Funktion, `T` ist entweder ein Delegattyp `D` oder ein Typ für die Ausdrucksbaumstruktur `Expression<D>` und eine der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="411e5-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="411e5-800">Einem hergeleiteten Rückgabetyp `X` vorhanden ist, für die `E` im Rahmen der Liste der Parameter `D` ([abgeleiteter Rückgabetyp](expressions.md#inferred-return-type)), und eine identitätskonvertierung vorhanden ist, von `X` in den Rückgabetyp der `D`</span><span class="sxs-lookup"><span data-stu-id="411e5-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="411e5-801">Entweder `E` ist nicht asynchronen und `D` hat einen Rückgabetyp `Y` oder `E` ist asynchron und `D` hat einen Rückgabetyp `Task<Y>`, und eine der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="411e5-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="411e5-802">Der Text der `E` ist ein Ausdruck, die genau übereinstimmt `Y`</span><span class="sxs-lookup"><span data-stu-id="411e5-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="411e5-803">Der Text der `E` ist ein Anweisungsblock, in dem alle return-Anweisung gibt einen Ausdruck, die genau übereinstimmt zurück, `Y`</span><span class="sxs-lookup"><span data-stu-id="411e5-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="411e5-804">Bessere Konvertierung-Ziel</span><span class="sxs-lookup"><span data-stu-id="411e5-804">Better conversion target</span></span>

<span data-ttu-id="411e5-805">Erhalten zwei verschiedene Arten `T1` und `T2`, `T1` ist eine bessere Konvertierung Ziel als `T2` Wenn keine implizite Konvertierung von `T2` zu `T1` vorhanden ist, und mindestens eine der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="411e5-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="411e5-806">Eine implizite Konvertierung von `T1` zu `T2` vorhanden ist</span><span class="sxs-lookup"><span data-stu-id="411e5-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="411e5-807">`T1` ist entweder ein Delegattyp `D1` oder ein Typ für die Ausdrucksbaumstruktur `Expression<D1>`, `T2` ist entweder ein Delegattyp `D2` oder ein Typ für die Ausdrucksbaumstruktur `Expression<D2>`, `D1` hat einen Rückgabetyp `S1` und eines der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="411e5-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="411e5-808">`D2` ist die "void" zurückgeben</span><span class="sxs-lookup"><span data-stu-id="411e5-808">`D2` is void returning</span></span>
   * <span data-ttu-id="411e5-809">`D2` hat einen Rückgabetyp `S2`, und `S1` ist eine bessere Konvertierung Ziel als `S2`</span><span class="sxs-lookup"><span data-stu-id="411e5-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="411e5-810">`T1` ist `Task<S1>`, `T2` ist `Task<S2>`, und `S1` ist eine bessere Konvertierung Ziel als `S2`</span><span class="sxs-lookup"><span data-stu-id="411e5-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="411e5-811">`T1` ist `S1` oder `S1?` , in denen `S1` ist ein ganzzahliger Typ mit Vorzeichen, und `T2` ist `S2` oder `S2?` , in denen `S2` ein Integraltyp ohne Vorzeichen ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="411e5-812">Dies gilt insbesondere in folgenden Fällen:</span><span class="sxs-lookup"><span data-stu-id="411e5-812">Specifically:</span></span>
   * <span data-ttu-id="411e5-813">`S1` ist `sbyte` und `S2` ist `byte`, `ushort`, `uint`, oder `ulong`</span><span class="sxs-lookup"><span data-stu-id="411e5-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="411e5-814">`S1` ist `short` und `S2` ist `ushort`, `uint`, oder `ulong`</span><span class="sxs-lookup"><span data-stu-id="411e5-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="411e5-815">`S1` ist `int` und `S2` ist `uint`, oder `ulong`</span><span class="sxs-lookup"><span data-stu-id="411e5-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="411e5-816">`S1` ist `long` und `S2` ist `ulong`</span><span class="sxs-lookup"><span data-stu-id="411e5-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="411e5-817">In der generischen Klassen überladen</span><span class="sxs-lookup"><span data-stu-id="411e5-817">Overloading in generic classes</span></span>

<span data-ttu-id="411e5-818">Während Signaturen, die gemäß der Deklaration eindeutig und sein müssen ist es möglich, dass die Ersetzung der Argumente des Typs zu identischen Signaturen führt.</span><span class="sxs-lookup"><span data-stu-id="411e5-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="411e5-819">In solchen Fällen werden die aktuelle Tie Regeln der überladungsauflösung, die oben genannten das spezifischste Element auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="411e5-820">Die folgenden Beispiele zeigen die Überladungen, die gültige und ungültige nach dieser Regel sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="411e5-821">Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung</span><span class="sxs-lookup"><span data-stu-id="411e5-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="411e5-822">Für die meisten dynamisch gebundene Vorgänge ist der Satz von möglichen Kandidaten für die Auflösung zum Zeitpunkt der Kompilierung unbekannt.</span><span class="sxs-lookup"><span data-stu-id="411e5-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="411e5-823">In bestimmten Fällen ist jedoch der Satz von Kandidaten zum Zeitpunkt der Kompilierung bekannt:</span><span class="sxs-lookup"><span data-stu-id="411e5-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="411e5-824">Aufrufen statischer Methoden, mit dem dynamische Argumente</span><span class="sxs-lookup"><span data-stu-id="411e5-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="411e5-825">Instanz-Methodenaufrufen, in denen der Empfänger keine dynamischen Ausdruck ist</span><span class="sxs-lookup"><span data-stu-id="411e5-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="411e5-826">Indexer-Aufrufe, in denen der Empfänger keine dynamischen Ausdruck ist</span><span class="sxs-lookup"><span data-stu-id="411e5-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="411e5-827">Ruft der Konstruktor mit dem dynamische Argumente</span><span class="sxs-lookup"><span data-stu-id="411e5-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="411e5-828">In diesen Fällen wird eine begrenzte Überprüfung der während der Kompilierung ausgeführt, bei jedem Kandidaten aus, um festzustellen, ob diese möglicherweise zur Laufzeit anwenden könnten. Diese Überprüfung umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="411e5-829">Partielle Typrückschluss: Jeder Typ Argument, das nicht direkt oder indirekt auf ein Argument vom Typ abhängt `dynamic` abgeleitet wird, wobei die Regeln der [Typrückschluss](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="411e5-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="411e5-830">Die verbleibenden Typargumente sind unbekannt.</span><span class="sxs-lookup"><span data-stu-id="411e5-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="411e5-831">Partielle anwendbarkeitsprüfung: Anwendbarkeit wird überprüft, gemäß [Anwendbarer Funktionsmember](expressions.md#applicable-function-member), aber der Parameter, deren Typen unbekannt sind, werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="411e5-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="411e5-832">Wenn kein Kandidat dieser Test erfolgreich ist, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="411e5-833">Member-Funktionsaufruf</span><span class="sxs-lookup"><span data-stu-id="411e5-833">Function member invocation</span></span>

<span data-ttu-id="411e5-834">Dieser Abschnitt beschreibt den Prozess, der erfolgt zur Laufzeit eine bestimmte Funktionsmember aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="411e5-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="411e5-835">Es wird vorausgesetzt, dass ein Bindung-Time-Prozess bereits das bestimmte Element um aufzurufen, möglicherweise durch Anwenden der überladungsauflösung, um einen Satz von möglichen Funktionsmembern festgestellt hat.</span><span class="sxs-lookup"><span data-stu-id="411e5-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="411e5-836">Rahmen, den Aufrufvorgang beschreiben sind Funktionsmember in zwei Kategorien unterteilt:</span><span class="sxs-lookup"><span data-stu-id="411e5-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="411e5-837">Mitglieder der statische Funktion.</span><span class="sxs-lookup"><span data-stu-id="411e5-837">Static function members.</span></span> <span data-ttu-id="411e5-838">Dies sind Instanzkonstruktoren, statische Methoden, statische Eigenschaftenaccessoren und benutzerdefinierten Operatoren.</span><span class="sxs-lookup"><span data-stu-id="411e5-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="411e5-839">Statische Funktionsmember sind immer nicht virtuell.</span><span class="sxs-lookup"><span data-stu-id="411e5-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="411e5-840">Instanzmember-Funktion.</span><span class="sxs-lookup"><span data-stu-id="411e5-840">Instance function members.</span></span> <span data-ttu-id="411e5-841">Dies sind Instanzmethoden, Instanz-Eigenschaftenaccessoren und Indexeraccessoren.</span><span class="sxs-lookup"><span data-stu-id="411e5-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="411e5-842">Instanzmember-Funktion sind entweder nicht virtuell oder virtuell sein, und Sie werden immer in einer bestimmten Instanz aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="411e5-843">Die Instanz wird berechnet, indem ein Instanzenausdruck, und es wird innerhalb der Funktionsmember der Member als zugegriffen werden `this` ([diesen Zugriff](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="411e5-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="411e5-844">Die laufzeitverarbeitung der einen Funktionsaufruf für das Element besteht aus den folgenden Schritten, in denen `M` ist das Funktionselement und, falls `M` ist ein Instanzmember `E` ist die Instanzausdruck:</span><span class="sxs-lookup"><span data-stu-id="411e5-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="411e5-845">Wenn `M` ist eine statische Funktion-Element:</span><span class="sxs-lookup"><span data-stu-id="411e5-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="411e5-846">Die Argumentliste ausgewertet wird, wie in beschrieben [Argumentlisten](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="411e5-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="411e5-847">`M` wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-847">`M` is invoked.</span></span>

*  <span data-ttu-id="411e5-848">Wenn `M` wird in ein Instanzmember der Funktion deklariert eine *Value_type*:</span><span class="sxs-lookup"><span data-stu-id="411e5-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="411e5-849">`E` wird ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-849">`E` is evaluated.</span></span> <span data-ttu-id="411e5-850">Wenn diese Evaluierungsversion auf eine Ausnahme auslöst, werden dann keine weiteren Schritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="411e5-851">Wenn `E` wird nicht als eine Variable, und klicken Sie dann auf eine temporäre lokale Variable vom klassifiziert `E`Typ erstellt wird und der Wert der `E` diese Variable zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="411e5-852">`E` als ein Verweis auf diese temporäre lokale Variable wird dann neu klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="411e5-853">Die temporäre Variable ist als `this` in `M`, aber nicht auf andere Weise.</span><span class="sxs-lookup"><span data-stu-id="411e5-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="411e5-854">Daher nur, wenn `E` ist eine Variable "true" ist es möglich, dass der Aufrufer die Änderungen zu prüfen, `M` stellt `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="411e5-855">Die Argumentliste ausgewertet wird, wie in beschrieben [Argumentlisten](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="411e5-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="411e5-856">`M` wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-856">`M` is invoked.</span></span> <span data-ttu-id="411e5-857">Die Variable verweist `E` wird die Variable verweist `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="411e5-858">Wenn `M` wird in ein Instanzmember der Funktion deklariert eine *Reference_type*:</span><span class="sxs-lookup"><span data-stu-id="411e5-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="411e5-859">`E` wird ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-859">`E` is evaluated.</span></span> <span data-ttu-id="411e5-860">Wenn diese Evaluierungsversion auf eine Ausnahme auslöst, werden dann keine weiteren Schritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="411e5-861">Die Argumentliste ausgewertet wird, wie in beschrieben [Argumentlisten](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="411e5-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="411e5-862">Wenn der Typ des `E` ist eine *Value_type*, einer Boxing-Konvertierung ([Boxing-Konvertierung](types.md#boxing-conversions)) ausgeführt wird, um konvertieren `E` eingeben `object`, und `E` gilt Der Typ `object` in den folgenden Schritten.</span><span class="sxs-lookup"><span data-stu-id="411e5-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="411e5-863">In diesem Fall `M` möglicherweise nur ein Mitglied `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="411e5-864">Der Wert des `E` wird überprüft, um gültig sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="411e5-865">Wenn der Wert des `E` ist `null`, `System.NullReferenceException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="411e5-866">Die Implementierung des Schnittstellenmembers Funktion aufrufen, wird bestimmt:</span><span class="sxs-lookup"><span data-stu-id="411e5-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="411e5-867">Wenn der Typ der Bindung-Time `E` ist eine Schnittstelle, der aufzurufenden Funktionsmember ist die Implementierung der `M` bereitgestellt, die von der Run-Time-Typ der Instanz verweist `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="411e5-868">Diese Funktionsmember richtet sich nach Anwenden der Standardregeln für Schnittstellen-Zuordnung ([schnittstellenzuordnung](interfaces.md#interface-mapping)) um zu bestimmen, die Implementierung der `M` bereitgestellt, die von der Run-Time-Typ der Instanz verweist `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="411e5-869">Andernfalls gilt: Wenn `M` gehört eine virtuelle Funktion, der aufzurufenden Funktionsmember ist die Implementierung der `M` bereitgestellt, die von der Run-Time-Typ der Instanz verweist `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="411e5-870">Dieser Funktionsmember richtet sich nach Anwenden der Regeln für die Bestimmung der am weitesten abgeleiteten Implementierung ([virtuelle Methoden](classes.md#virtual-methods)) der `M` in Bezug auf die Run-Time-Typ der Instanz verweist `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="411e5-871">Andernfalls `M` nicht virtuelle Funktion angehört, und wird von der aufzurufenden Funktionsmember `M` selbst.</span><span class="sxs-lookup"><span data-stu-id="411e5-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="411e5-872">Wird aufgerufen, die Implementierung des Schnittstellenmembers Funktion im vorherigen Schritt bestimmt.</span><span class="sxs-lookup"><span data-stu-id="411e5-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="411e5-873">Das Objekt, das auf `E` wird das Objekt, das auf `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="411e5-874">Aufrufe in geschachtelten Instanzen</span><span class="sxs-lookup"><span data-stu-id="411e5-874">Invocations on boxed instances</span></span>

<span data-ttu-id="411e5-875">Ein Funktionsmember implementiert eine *Value_type* kann über eine geschachtelte Instanz, die aufgerufen werden *Value_type* in den folgenden Situationen:</span><span class="sxs-lookup"><span data-stu-id="411e5-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="411e5-876">Wenn der Funktionsmember der Member ist ein `override` einer Methode geerbt von Typ `object` und wird durch einen Instanzausdruck des Typs aufgerufen `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="411e5-877">Wenn der Funktionsmember der Member ist eine Implementierung eines Schnittstellenmembers-Funktion und wird aufgerufen, durch einen Instanzausdruck, der eine *Interface_type*.</span><span class="sxs-lookup"><span data-stu-id="411e5-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="411e5-878">Wenn der Funktionsmember der Member über einen Delegaten aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="411e5-879">In diesen Fällen gilt eine Variable enthält die geschachtelte Instanz die *Value_type*, und diese Variable wird die Variable verweist `this` innerhalb von der Members-Funktionsaufruf.</span><span class="sxs-lookup"><span data-stu-id="411e5-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="411e5-880">Insbesondere bedeutet dies, dass wenn ein Funktionsmember für eine geschachtelte Instanz aufgerufen wird, für den Funktionsmember "zum Ändern des Werts in die geschachtelte Instanz enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="411e5-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="411e5-881">Primäre Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-881">Primary expressions</span></span>

<span data-ttu-id="411e5-882">Primärausdrücke umfassen die einfachsten Formen von Ausdrücken.</span><span class="sxs-lookup"><span data-stu-id="411e5-882">Primary expressions include the simplest forms of expressions.</span></span>

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

<span data-ttu-id="411e5-883">Ausdrücke (primär) aufgeteilt sind *Array_creation_expression*s und *Primary_no_array_creation_expression*s.</span><span class="sxs-lookup"><span data-stu-id="411e5-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="411e5-884">Zum Behandeln von Arrayerstellungsausdruck – auf diese Weise, anstatt sie zusammen mit anderen einfachen Ausdruck Formulare auflisten ermöglicht es, die die Grammatik, die möglicherweise irreführende Code wie z. B. zu unterbinden</span><span class="sxs-lookup"><span data-stu-id="411e5-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="411e5-885">die andernfalls als interpretiert werden sollen</span><span class="sxs-lookup"><span data-stu-id="411e5-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="411e5-886">Literale</span><span class="sxs-lookup"><span data-stu-id="411e5-886">Literals</span></span>

<span data-ttu-id="411e5-887">Ein *Primary_expression* besteht aus einem *literal* ([Literale](lexical-structure.md#literals)) als Wert klassifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="411e5-888">Interpolierte Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="411e5-888">Interpolated strings</span></span>

<span data-ttu-id="411e5-889">Ein *Interpolated_string_expression* besteht aus einem `$` anmelden, gefolgt von einer regulären oder wörtliche Zeichenfolge, die Literale, bei dem Löcher, getrennt durch `{` und `}`, setzen Sie Ausdrücke und Formatierung Spezifikationen.</span><span class="sxs-lookup"><span data-stu-id="411e5-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="411e5-890">Ein Ausdruck eine interpolierte Zeichenfolge ist das Ergebnis einer *Interpolated_string_literal* , hat wurde unterteilt in einzelne Token, wie in beschrieben [interpolierte Zeichenfolgenliterale](lexical-structure.md#interpolated-string-literals).</span><span class="sxs-lookup"><span data-stu-id="411e5-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

<span data-ttu-id="411e5-891">Die *Constant_expression* eine Interpolation benötigen eine implizite Konvertierung in `int`.</span><span class="sxs-lookup"><span data-stu-id="411e5-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="411e5-892">Ein *Interpolated_string_expression* wird als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="411e5-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="411e5-893">Wenn sie sofort in konvertiert wird `System.IFormattable` oder `System.FormattableString` mit eine implizite interpolierte Zeichenfolge-Konvertierung ([implizite interpolierte zeichenfolgenkonvertierungen](conversions.md#implicit-interpolated-string-conversions)), des interpolierten Zeichenfolgenausdrucks Typ verfügt.</span><span class="sxs-lookup"><span data-stu-id="411e5-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="411e5-894">Andernfalls ist es den Typ `string`.</span><span class="sxs-lookup"><span data-stu-id="411e5-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="411e5-895">Wenn der Typ einer interpolierten Zeichenfolge `System.IFormattable` oder `System.FormattableString`, die Bedeutung ist, einen Aufruf von `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="411e5-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="411e5-896">Wenn der Typ ist `string`, die Bedeutung des Ausdrucks ist ein Aufruf `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="411e5-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="411e5-897">In beiden Fällen besteht aus die Argumentliste des Aufrufs ein Formatzeichenfolgenliteral mit Platzhalter für jede Interpolation, und ein Argument für die einzelnen Ausdrücke, die Platzhalter entspricht.</span><span class="sxs-lookup"><span data-stu-id="411e5-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="411e5-898">Das Formatzeichenfolgenliteral wie folgt erstellt wird, in denen `N` ist die Anzahl von Interpolationen in die *Interpolated_string_expression*:</span><span class="sxs-lookup"><span data-stu-id="411e5-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="411e5-899">Wenn ein *Interpolated_regular_string_whole* oder *Interpolated_verbatim_string_whole* folgt die `$` anmelden, und klicken Sie dann das Format-Zeichenfolgenliteral, das Token ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="411e5-900">Andernfalls besteht aus den Formatzeichenfolgenliteral:</span><span class="sxs-lookup"><span data-stu-id="411e5-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="411e5-901">Erste die *Interpolated_regular_string_start* oder *Interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="411e5-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="411e5-902">Klicken Sie dann für jede Zahl `I` aus `0` zu `N-1`:</span><span class="sxs-lookup"><span data-stu-id="411e5-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="411e5-903">Die dezimale Darstellung `I`</span><span class="sxs-lookup"><span data-stu-id="411e5-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="411e5-904">Wenn danach der entsprechenden *Interpolation* verfügt über eine *Constant_expression*, `,` (Komma) gefolgt von der dezimale Darstellung des Werts der *Constant_expression*</span><span class="sxs-lookup"><span data-stu-id="411e5-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="411e5-905">Die *Interpolated_regular_string_mid*, *Interpolated_regular_string_end*, *Interpolated_verbatim_string_mid* oder *Interpolated_ Verbatim_string_end* unmittelbar nach der entsprechenden Interpolation.</span><span class="sxs-lookup"><span data-stu-id="411e5-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="411e5-906">Die nachfolgende Argumente werden einfach die *Ausdrücke* aus der *Interpolationen* (sofern vorhanden), in der Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="411e5-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="411e5-907">TODO: Beispiele.</span><span class="sxs-lookup"><span data-stu-id="411e5-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="411e5-908">Einfache Namen</span><span class="sxs-lookup"><span data-stu-id="411e5-908">Simple names</span></span>

<span data-ttu-id="411e5-909">Ein *Simple_name* eines Bezeichners steht, optional gefolgt von einer Liste der Typargumente besteht aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="411e5-910">Ein *Simple_name* ist entweder der Form `I` oder des Formulars `I<A1,...,Ak>`, wobei `I` ist eine einmalige Kennung und `<A1,...,Ak>` ist eine optionale *Type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="411e5-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="411e5-911">Wenn kein *Type_argument_list* wird angegeben, sollten Sie `K` 0 (null).</span><span class="sxs-lookup"><span data-stu-id="411e5-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="411e5-912">Die *Simple_name* ausgewertet und wie folgt klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="411e5-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="411e5-913">Wenn `K` ist 0 (null) und die *Simple_name* Teil einer *Block* und, wenn die *Block*des (oder einer einschließenden *Block*des) lokalen Variablendeklaration-Speicherplatz ([Deklarationen](basic-concepts.md#declarations)) enthält lokale Variablen, Parameter oder Konstante mit dem Namen `I`, und klicken Sie dann die *Simple_name* bezieht sich auf dieser lokalen Variablen verwenden, Parameter oder Konstanten und wird als eine Variable oder einem Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="411e5-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="411e5-914">Wenn `K` ist 0 (null) und die *Simple_name* erscheint im Text der Deklaration einer generischen Methode und wenn dieser Deklaration einen Typparameter mit dem Namen enthält `I`, und klicken Sie dann die *Simple_name*bezieht sich auf den Typparameter.</span><span class="sxs-lookup"><span data-stu-id="411e5-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="411e5-915">Für jeden Instanztyp andernfalls `T` ([den Instanztyp](classes.md#the-instance-type)), beginnend mit dem Instanztyp der Deklaration des unmittelbar einschließenden und mit dem Instanztyp jeden einschließenden Klasse oder Struktur Deklaration (sofern vorhanden):</span><span class="sxs-lookup"><span data-stu-id="411e5-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="411e5-916">Wenn `K` ist 0 (null) und die Deklaration von `T` enthält einen Typparameter mit dem Namen `I`, und klicken Sie dann die *Simple_name* bezieht sich auf den Typparameter.</span><span class="sxs-lookup"><span data-stu-id="411e5-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="411e5-917">Andernfalls, wenn eine Suche nach Membern ([Membersuche](expressions.md#member-lookup)) der `I` in `T` mit `K`  Typargumente eine Übereinstimmung ergibt:</span><span class="sxs-lookup"><span data-stu-id="411e5-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="411e5-918">Wenn `T` ist die Instanztyp des unmittelbar umschließende Klasse oder Struktur und die Suche nach identifiziert einen oder mehrere Methoden, die das Ergebnis ist eine Methodengruppe mit einem zugeordneten Instanzausdruck von `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="411e5-919">Wenn eine Liste der Typargumente angegeben wurde, wird es verwendet eine generische Methode aufzurufen ([Methodenaufrufe](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="411e5-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="411e5-920">Andernfalls gilt: Wenn `T` der Instanztyp der unmittelbar umschließende Klasse oder Struktur-Typ ist, wenn die Suche auf einen Instanzmember identifiziert und der Verweis innerhalb eines Texts der Instanzkonstruktor, eine Instanzmethode oder einem Instanzaccessor auftritt, die Ergebnis ist identisch mit Memberzugriff ([Memberzugriff](expressions.md#member-access)) des Formulars `this.I`.</span><span class="sxs-lookup"><span data-stu-id="411e5-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="411e5-921">Dies kann nur, wenn `K` ist 0 (null).</span><span class="sxs-lookup"><span data-stu-id="411e5-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="411e5-922">Andernfalls das Ergebnis ist identisch mit Memberzugriff ([Memberzugriff](expressions.md#member-access)) des Formulars `T.I` oder `T.I<A1,...,Ak>`.</span><span class="sxs-lookup"><span data-stu-id="411e5-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="411e5-923">In diesem Fall es ist ein Fehler während der Bindung für die *Simple_name* zum Verweisen auf ein Instanzmember.</span><span class="sxs-lookup"><span data-stu-id="411e5-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="411e5-924">Für jeden Namespace, andernfalls `N`, beginnend mit dem Namespace, in dem die *Simple_name* wird mit jeder einschließenden Namespace (sofern vorhanden) und bis hin zu den globalen Namespace, weiterhin werden die folgenden Schritte aus ausgewertet, bis eine Entität befindet:</span><span class="sxs-lookup"><span data-stu-id="411e5-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="411e5-925">Wenn `K` ist 0 (null) und `I` ist der Name eines Namespace im `N`, klicken Sie dann:</span><span class="sxs-lookup"><span data-stu-id="411e5-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="411e5-926">Wenn der Speicherort, in dem die *Simple_name* tritt auf, steht eine Namespace-Deklaration für `N` und die Namespacedeklaration enthält eine *Extern_alias_directive* oder  *Using_alias_directive* , die den Namen zuordnet `I` mit einem Namespace oder Typ, der *Simple_name* ist mehrdeutig und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="411e5-927">Andernfalls die *Simple_name* verweist auf den Namespace mit dem Namen `I` in `N`.</span><span class="sxs-lookup"><span data-stu-id="411e5-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="411e5-928">Andernfalls gilt: Wenn `N` enthält einen zugreifbarer Typ, der mit Namen `I` und `K`  Typparameter, klicken Sie dann:</span><span class="sxs-lookup"><span data-stu-id="411e5-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="411e5-929">Wenn `K` ist 0 (null) und den Speicherort, in denen die *Simple_name* tritt auf, wird durch eine Namespacedeklaration zur eingeschlossen `N` und die Namespacedeklaration enthält eine *Extern_alias_directive*oder *Using_alias_directive* , die den Namen zuordnet `I` mit einem Namespace oder Typ, der *Simple_name* ist mehrdeutig und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="411e5-930">Andernfalls die *Namespace_or_type_name* verweist auf den Typ mit den Argumenten angegebenen Typs erstellt.</span><span class="sxs-lookup"><span data-stu-id="411e5-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="411e5-931">Andernfalls gilt: Wenn der Speicherort, in dem die *Simple_name* tritt auf, wird durch eine Namespacedeklaration zur eingeschlossen `N`:</span><span class="sxs-lookup"><span data-stu-id="411e5-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="411e5-932">Wenn `K` ist 0 (null) und die Namespacedeklaration enthält eine *Extern_alias_directive* oder *Using_alias_directive* , die den Namen verknüpft `I` mit einem importierten Namespace oder Der Typ, und klicken Sie dann die *Simple_name* bezieht sich auf diesen Namespace oder Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="411e5-933">Andernfalls, wenn die Deklarationen für Namespaces und importiert die *Using_namespace_directive*s und *Using_static_directive*s der Namespacedeklaration enthalten genau ein zugreifbarer Typ oder ohne Erweiterung statischen Member mit Namen `I` und `K`  Typparameter, und klicken Sie dann die *Simple_name* bezieht sich auf den Typ oder Member, die mit den Argumenten angegebenen Typs erstellt.</span><span class="sxs-lookup"><span data-stu-id="411e5-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="411e5-934">Andernfalls, wenn die Namespaces und Typen von importiert die *Using_namespace_directive*s der Namespacedeklaration enthalten mehr als ein zugreifbarer Typ oder nicht-Erweiterungsmethode statischen Member mit Namen `I` und `K`  Typparameter, und klicken Sie dann die *Simple_name* ist mehrdeutig und ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="411e5-935">Beachten Sie, dass dieser gesamte Schritt genau mit dem entsprechenden Schritt bei der Verarbeitung von parallelen ist eine *Namespace_or_type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="411e5-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="411e5-936">Andernfalls die *Simple_name* ist nicht definiert und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="411e5-937">Ausdrücke in Klammern</span><span class="sxs-lookup"><span data-stu-id="411e5-937">Parenthesized expressions</span></span>

<span data-ttu-id="411e5-938">Ein *Parenthesized_expression* besteht aus einem *Ausdruck* in Klammern eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="411e5-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="411e5-939">Ein *Parenthesized_expression* wird ausgewertet, durch die Auswertung der *Ausdruck* innerhalb der Klammern.</span><span class="sxs-lookup"><span data-stu-id="411e5-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="411e5-940">Wenn die *Ausdruck* innerhalb der Klammern gibt an einen Namespace oder Typ, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="411e5-941">Andernfalls das Ergebnis der *Parenthesized_expression* ist das Ergebnis der Auswertung der enthaltenen *Ausdruck*.</span><span class="sxs-lookup"><span data-stu-id="411e5-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="411e5-942">Memberzugriff</span><span class="sxs-lookup"><span data-stu-id="411e5-942">Member access</span></span>

<span data-ttu-id="411e5-943">Ein *Member_access* besteht aus einem *Primary_expression*, *Predefined_type*, oder ein *Qualified_alias_member*, gefolgt von einem"`.`"token, gefolgt von einem *Bezeichner*, optional gefolgt von einem *Type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="411e5-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

<span data-ttu-id="411e5-944">Die *Qualified_alias_member* Produktion wird definiert, [Namespace Alias Qualifiers](namespaces.md#namespace-alias-qualifiers).</span><span class="sxs-lookup"><span data-stu-id="411e5-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="411e5-945">Ein *Member_access* ist entweder der Form `E.I` oder des Formulars `E.I<A1, ..., Ak>`, wobei `E` ist ein primärer Ausdruck, `I` ist eine einmalige Kennung und `<A1, ..., Ak>` ist eine optionale  *Type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="411e5-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="411e5-946">Wenn kein *Type_argument_list* wird angegeben, sollten Sie `K` 0 (null).</span><span class="sxs-lookup"><span data-stu-id="411e5-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="411e5-947">Ein *Member_access* mit einem *Primary_expression* des Typs `dynamic` dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="411e5-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="411e5-948">In diesem Fall der Compiler den Memberzugriff als Eigenschaftszugriff vom Typ klassifiziert `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="411e5-949">Den Regeln unten, um zu bestimmen, die Bedeutung von der *Member_access* gelten dann zur Laufzeit, der Kompilierzeittyp von der Laufzeit-Typinformationen anstelle der *Primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="411e5-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="411e5-950">Wenn diese Klassifizierung Laufzeit zu einer Methodengruppe führt, und klicken Sie dann der Memberzugriff muss die *Primary_expression* von einer *Invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="411e5-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="411e5-951">Die *Member_access* ausgewertet und wie folgt klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="411e5-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="411e5-952">Wenn `K` ist 0 (null) und `E` ist ein Namespace und `E` enthält einen geschachtelten Namespace mit dem Namen `I`, lautet das Ergebnis dieses Namespace.</span><span class="sxs-lookup"><span data-stu-id="411e5-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="411e5-953">Andernfalls gilt: Wenn `E` ist ein Namespace und `E` enthält einen zugreifbarer Typ, der mit Namen `I` und `K`  Typparameter, und klicken Sie dann das Ergebnis dieses Typs mit den Argumenten angegebenen Typs erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="411e5-954">Wenn `E` ist eine *Predefined_type* oder *Primary_expression* als Typ klassifiziert werden, wenn `E` ist es sich nicht um ein Typparameter, und wenn eine Suche nach Membern ([Membersuche](expressions.md#member-lookup)) der `I` in `E` mit `K`  Typparameter erzeugt eine Übereinstimmung, `E.I` ausgewertet und wie folgt klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="411e5-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="411e5-955">Wenn `I` identifiziert einen Typ, lautet das Ergebnis dieses Typs mit den Argumenten angegebenen Typs erstellt.</span><span class="sxs-lookup"><span data-stu-id="411e5-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="411e5-956">Wenn `I` eine oder mehrere Methoden, identifiziert, und klicken Sie dann das Ergebnis einer Methodengruppe ohne zugeordneten Instanzausdruck ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="411e5-957">Wenn eine Liste der Typargumente angegeben wurde, wird es verwendet eine generische Methode aufzurufen ([Methodenaufrufe](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="411e5-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="411e5-958">Wenn `I` identifiziert eine `static` -Eigenschaft, und klicken Sie dann auf das Ergebnis ist ein Eigenschaftenzugriff ohne zugeordneten Instanzausdruck.</span><span class="sxs-lookup"><span data-stu-id="411e5-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="411e5-959">Wenn `I` identifiziert eine `static` Feld:</span><span class="sxs-lookup"><span data-stu-id="411e5-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="411e5-960">Wenn das Feld `readonly` der Verweis findet im statischen Konstruktor der Klasse oder Struktur, die in der das Feld deklariert wird, und das Ergebnis ist ein Wert, d. h. der Wert des statischen Felds `I` in `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="411e5-961">Andernfalls ist das Ergebnis einer Variablen, d. h. das statische Feld `I` in `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="411e5-962">Wenn `I` identifiziert eine `static` Ereignis:</span><span class="sxs-lookup"><span data-stu-id="411e5-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="411e5-963">Im Falle der Verweis innerhalb der Klasse oder Struktur, die in der das Ereignis deklariert ist, und das Ereignis deklariert wurde, ohne *Event_accessor_declarations* ([Ereignisse](classes.md#events)), klicken Sie dann `E.I` genau verarbeitet wird als ob `I` wurden von ein statischen Feld.</span><span class="sxs-lookup"><span data-stu-id="411e5-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="411e5-964">Andernfalls ist das Ergebnis ein Ereigniszugriff ohne zugeordneten Instanzausdruck.</span><span class="sxs-lookup"><span data-stu-id="411e5-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="411e5-965">Wenn `I` eine Konstante ist, identifiziert, und klicken Sie dann das Ergebnis ein Wert, nämlich den Wert dieser Konstanten ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="411e5-966">Wenn `I` einen Enumerationsmember identifiziert, und klicken Sie dann das Ergebnis ein Wert, nämlich der Wert dieses Elements der Enumeration ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="411e5-967">Andernfalls `E.I` ist ein ungültiger Memberverweis, und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="411e5-968">Wenn `E` ist ein Zugriff auf Eigenschaften, Indexerzugriff, Variable oder -Wert, dessen Typ von `T`, und eine Suche nach Membern ([Membersuche](expressions.md#member-lookup)) der `I` in `T` mit `K`  Typargumente erzeugt eine Übereinstimmung, `E.I` ausgewertet und wie folgt klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="411e5-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="411e5-969">Wenn zunächst `E` ist eine Eigenschaft oder Indexerzugriff, und klicken Sie dann den Wert der Eigenschaft oder Indexerzugriff abgerufen ([Werte Ausdrücke](expressions.md#values-of-expressions)) und `E` ist, die als Wert neu klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="411e5-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="411e5-970">Wenn `I` eine oder mehrere Methoden, identifiziert, ist das Ergebnis einer Methodengruppe mit einem zugeordneten Instanzausdruck von `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="411e5-971">Wenn eine Liste der Typargumente angegeben wurde, wird es verwendet eine generische Methode aufzurufen ([Methodenaufrufe](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="411e5-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="411e5-972">Wenn `I` identifiziert eine Instance-Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="411e5-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="411e5-973">Wenn `E` ist `this`, `I` identifiziert eine automatisch implementierte Eigenschaft ([automatisch implementierten Eigenschaften](classes.md#automatically-implemented-properties)) ohne einen Setter und der Verweis erfolgt innerhalb eines Instanzkonstruktors für einen Klassen-oder Strukturtyp `T`, lautet das Ergebnis einer Variablen, d. h. das ausgeblendete Unterstützungsfeld für den Auto-Eigenschaft vom `I` in der Instanz von `T` von bestimmten `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="411e5-974">Andernfalls ist das Ergebnis ein Eigenschaftszugriff mit einem zugeordneten Instanzausdruck von `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="411e5-975">Wenn `T` ist eine *Class_type* und `I` identifiziert ein Instanzenfeld, *Class_type*:</span><span class="sxs-lookup"><span data-stu-id="411e5-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="411e5-976">Wenn der Wert des `E` ist `null`, ein `System.NullReferenceException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="411e5-977">Wenn das Feld ist, andernfalls `readonly` der Verweis findet keinen Instanzenkonstruktor der Klasse in der das Feld deklariert wird, und das Ergebnis ist ein Wert, nämlich der Wert des Felds `I` in das Objekt, das auf `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="411e5-978">Andernfalls ist das Ergebnis einer Variablen, d. h. das Feld `I` in das Objekt, das auf `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="411e5-979">Wenn `T` ist eine *Struct_type* und `I` identifiziert ein Instanzenfeld, *Struct_type*:</span><span class="sxs-lookup"><span data-stu-id="411e5-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="411e5-980">Wenn `E` ist ein Wert, oder wenn das Feld `readonly` der Verweis findet keinen Instanzenkonstruktor der Struktur in der das Feld deklariert wird, und das Ergebnis ist ein Wert, nämlich der Wert des Felds `I` in der Strukturinstanz, die vom  `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="411e5-981">Andernfalls ist das Ergebnis einer Variablen, d. h. das Feld `I` in der Strukturinstanz, die vom `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="411e5-982">Wenn `I` bezeichnet ein Instanzereignis:</span><span class="sxs-lookup"><span data-stu-id="411e5-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="411e5-983">Im Falle der Verweis innerhalb der Klasse oder Struktur, die in der das Ereignis deklariert ist, und das Ereignis deklariert wurde, ohne *Event_accessor_declarations* ([Ereignisse](classes.md#events)), und der Verweis findet nicht statt, als die linke Seite des eine `+=` oder `-=` Operator an, klicken Sie dann `E.I` verarbeitet wird, genau wie `I` wurde ein Instanzfeld.</span><span class="sxs-lookup"><span data-stu-id="411e5-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="411e5-984">Das Ergebnis ist, andernfalls ein Ereigniszugriff mit einem zugeordneten Instanzausdruck von `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="411e5-985">Es wird versucht, andernfalls verarbeitet `E.I` als ein Aufruf der Erweiterungsmethode ([Erweiterung Methodenaufrufe](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="411e5-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="411e5-986">Wenn dies fehlschlägt, `E.I` ist ein ungültiger Memberverweis, und beim Auftreten eines Laufzeitfehlers-Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="411e5-987">Identische einfache Namen und Typ</span><span class="sxs-lookup"><span data-stu-id="411e5-987">Identical simple names and type names</span></span>

<span data-ttu-id="411e5-988">In ein Memberzugriff des Formulars `E.I`, wenn `E` ist ein einzelner Bezeichner, und wenn die Bedeutung der `E` als eine *Simple_name* ([einfache Namen](expressions.md#simple-names)) ist eine Konstante, Feld, Eigenschaft lokale Variable oder Parameter mit den gleichen Typ wie die Bedeutung der `E` als eine *Type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)), und klicken Sie dann beide Bedeutungen von `E` sind zulässig.</span><span class="sxs-lookup"><span data-stu-id="411e5-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="411e5-989">Die zwei möglichen Bedeutungen der `E.I` sind nie mehrdeutig, da `I` muss unbedingt ein Member des Typs `E` in beiden Fällen.</span><span class="sxs-lookup"><span data-stu-id="411e5-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="411e5-990">Das heißt, die Regel einfach ermöglicht Zugriff auf die statische Member und geschachtelte Typen von `E` , in denen ein Fehler während der Kompilierung würde andernfalls aufgetreten.</span><span class="sxs-lookup"><span data-stu-id="411e5-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="411e5-991">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="411e5-991">For example:</span></span>
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a><span data-ttu-id="411e5-992">Grammatik Mehrdeutigkeiten</span><span class="sxs-lookup"><span data-stu-id="411e5-992">Grammar ambiguities</span></span>

<span data-ttu-id="411e5-993">Die Produktionen für *Simple_name* ([einfache Namen](expressions.md#simple-names)) und *Member_access* ([Memberzugriff](expressions.md#member-access)) erhalten, allerdings haben Mehrdeutigkeiten in Anstieg der Grammatik für Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="411e5-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="411e5-994">Beispielsweise ist die Anweisung:</span><span class="sxs-lookup"><span data-stu-id="411e5-994">For example, the statement:</span></span>
```
F(G<A,B>(7));
```
<span data-ttu-id="411e5-995">interpretiert werden konnte, wie ein Aufruf von `F` mit zwei Argumenten `G < A` und `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="411e5-996">Alternativ dazu, wie ein Aufruf von interpretiert werden konnte `F` mit einem Argument, das einen Aufruf einer generischen Methode ist `G` mit zwei Typargumente und eine reguläre Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="411e5-997">Wenn eine Folge von Token (in Kontext) kann als analysiert werden eine *Simple_name* ([einfache Namen](expressions.md#simple-names)), *Member_access* ([Memberzugriff](expressions.md#member-access)), oder *Pointer_member_access* ([Zeigermemberzugriff](unsafe-code.md#pointer-member-access)) endet mit einem *Type_argument_list* ([Typargumente](types.md#type-arguments)), wird die Token, die direkt nach dem schließenden `>` Token wird untersucht.</span><span class="sxs-lookup"><span data-stu-id="411e5-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="411e5-998">Wenn einer der</span><span class="sxs-lookup"><span data-stu-id="411e5-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="411e5-999">die *Type_argument_list* wird beibehalten, als Teil der *Simple_name*, *Member_access* oder *Pointer_member_access* sowie andere mögliche Analyse der Sequenz von Token wird verworfen.</span><span class="sxs-lookup"><span data-stu-id="411e5-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="411e5-1000">Andernfalls die *Type_argument_list* ist nicht als Teil der *Simple_name*, *Member_access* oder *Pointer_member_access*, auch wenn es keine andere mögliche Analyse der Sequenz von Token.</span><span class="sxs-lookup"><span data-stu-id="411e5-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="411e5-1001">Beachten Sie, dass diese Regeln nicht angewendet, bei der Analyse einer *Type_argument_list* in einem *Namespace_or_type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="411e5-1002">Die Anweisung</span><span class="sxs-lookup"><span data-stu-id="411e5-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="411e5-1003">wird nach dieser Regel interpretiert werden als Aufruf an `F` mit einem Argument, das einen Aufruf einer generischen Methode ist `G` mit zwei Typargumente und eine reguläre-Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="411e5-1004">Die Anweisungen</span><span class="sxs-lookup"><span data-stu-id="411e5-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="411e5-1005">Jede interpretiert als Aufruf an `F` mit zwei Argumenten.</span><span class="sxs-lookup"><span data-stu-id="411e5-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="411e5-1006">Die Anweisung</span><span class="sxs-lookup"><span data-stu-id="411e5-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="411e5-1007">wird als ein kleiner-als-Operator, der größer als-Operator, und unäre plus -Operator, interpretiert werden, als wäre die Anweisung geschrieben worden `x = (F < A) > (+y)`, anstatt als eine *Simple_name* mit einer *Type_argument_list* gefolgt von einem binären plus -Operator.</span><span class="sxs-lookup"><span data-stu-id="411e5-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="411e5-1008">In der Anweisung</span><span class="sxs-lookup"><span data-stu-id="411e5-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="411e5-1009">die Token `C<T>` interpretiert werden eine *Namespace_or_type_name* mit einem *Type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="411e5-1010">Aufrufausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-1010">Invocation expressions</span></span>

<span data-ttu-id="411e5-1011">Ein *Invocation_expression* wird verwendet, um eine Methode aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="411e5-1012">Ein *Invocation_expression* dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)), wenn mindestens eine der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="411e5-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="411e5-1013">Die *Primary_expression* weist den Typ der Kompilierzeit `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="411e5-1014">Mindestens ein Argument des optionalen *Argument_list* weist den Typ der Kompilierzeit `dynamic` und *Primary_expression* verfügt nicht über einen Delegattyp aufweisen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="411e5-1015">In diesem Fall der Compiler klassifiziert die *Invocation_expression* als ein Wert vom Typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="411e5-1016">Den Regeln unten, um zu bestimmen, die Bedeutung des das *Invocation_expression* gelten dann zur Laufzeit, der Kompilierzeittyp, der von der Laufzeit-Typinformationen anstelle der *Primary_expression* und Argumente, die den Zeitpunkt der Kompilierung Typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="411e5-1017">Wenn die *Primary_expression* hat keinen Kompilierzeittyp `dynamic`, und klicken Sie dann der Methodenaufruf eine begrenzte Kompilierung Zeit Überprüfung durchgeführt wird, wie in beschrieben [Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="411e5-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="411e5-1018">Die *Primary_expression* von einer *Invocation_expression* einer Methodengruppe oder der Wert muss eine *Delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="411e5-1019">Wenn die *Primary_expression* eine Methodengruppe wird die *Invocation_expression* ist der Aufruf einer Methode ([Methodenaufrufe](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="411e5-1020">Wenn die *Primary_expression* ist ein Wert, der eine *Delegate_type*, *Invocation_expression* einem Delegataufruf aufgerufen wird ([AufrufeDelegieren](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="411e5-1021">Wenn die *Primary_expression* ist weder eine Methodengruppe noch den Wert einer *Delegate_type*, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="411e5-1022">Der optionale *Argument_list* ([Argumentlisten](expressions.md#argument-lists)) Werte oder Variablenverweise für die Parameter der Methode enthält.</span><span class="sxs-lookup"><span data-stu-id="411e5-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="411e5-1023">Das Ergebnis der Auswertung einer *Invocation_expression* wird wie folgt klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="411e5-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="411e5-1024">Wenn die *Invocation_expression* Ruft eine Methode oder ein Delegat, der gibt `void`, das Ergebnis ist "nothing".</span><span class="sxs-lookup"><span data-stu-id="411e5-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="411e5-1025">Ein Ausdruck, der Klassifizierung als "nothing" nur im Kontext zulässig ist eine *Statement_expression* ([ausdrucksanweisungen](statements.md#expression-statements)) oder als Text einer *Lambda_expression*([Anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="411e5-1026">Andernfalls tritt ein Fehler während der Bindung auf.</span><span class="sxs-lookup"><span data-stu-id="411e5-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="411e5-1027">Andernfalls ist das Ergebnis den Wert der von der Methode oder Delegat zurückgegebene Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="411e5-1028">Methodenaufrufe</span><span class="sxs-lookup"><span data-stu-id="411e5-1028">Method invocations</span></span>

<span data-ttu-id="411e5-1029">Für den Aufruf einer Methode die *Primary_expression* von der *Invocation_expression* muss eine Methodengruppe sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="411e5-1030">Die Methodengruppe identifiziert, die eine aufzurufende Methode oder den Satz von überladenen Methoden zur Auswahl einer bestimmten Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="411e5-1031">Im letzteren Fall Bestimmung der bestimmten Methode aufzurufende wird auf Grundlage des Kontexts bereitgestellt, die von den Typen der Argumente in der *Argument_list*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="411e5-1032">Die Bindung-Time-Verarbeitung eines Methodenaufrufs des Formulars `M(A)`, wobei `M` ist eine Methodengruppe (möglicherweise auch eine *Type_argument_list*), und `A` ist eine optionale *Argument_ Liste*, umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="411e5-1033">Der Satz von Kandidaten-Methoden für den Methodenaufruf wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="411e5-1034">Für jede Methode `F` die Methodengruppe zugeordneten `M`:</span><span class="sxs-lookup"><span data-stu-id="411e5-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="411e5-1035">Wenn `F` ist nicht generisch, `F` ist ein Kandidat bei:</span><span class="sxs-lookup"><span data-stu-id="411e5-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="411e5-1036">`M` verfügt über keine Liste der Typargumente, und</span><span class="sxs-lookup"><span data-stu-id="411e5-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="411e5-1037">`F` gilt in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="411e5-1038">Wenn `F` ist generisch und `M` verfügt über keine Liste der Typargumente, `F` ist ein Kandidat bei:</span><span class="sxs-lookup"><span data-stu-id="411e5-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="411e5-1039">Typrückschluss ([Typrückschluss](expressions.md#type-inference)) erfolgreich ist, wird eine Liste der Typargumente für den Aufruf ableiten und</span><span class="sxs-lookup"><span data-stu-id="411e5-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="411e5-1040">Sobald die abgeleiteten Typargumente für die entsprechende Methode Typparameter ersetzt werden, erfüllen alle konstruierte Typen in der Parameterliste von F deren Einschränkungen ([Einschränkungen erfüllen](types.md#satisfying-constraints)), und der Parameterliste der `F` gilt in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="411e5-1041">Wenn `F` ist generisch und `M` enthält eine Liste der Typargumente, `F` ist ein Kandidat bei:</span><span class="sxs-lookup"><span data-stu-id="411e5-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="411e5-1042">`F` die gleiche Anzahl von Methodentypparametern aufweist, wie in der Liste der Typargumente, bereitgestellt wurden und</span><span class="sxs-lookup"><span data-stu-id="411e5-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="411e5-1043">Sobald die Typargumente für die entsprechende Methode Typparameter ersetzt werden, erfüllen alle konstruierte Typen in der Parameterliste von F deren Einschränkungen ([Einschränkungen erfüllen](types.md#satisfying-constraints)), und der Liste der Parameter `F` gilt in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="411e5-1044">Die Gruppe der möglichen Methoden werden reduziert, um nur die Methoden aus den am stärksten abgeleiteten Typen enthalten: Für jede Methode `C.F` in der Gruppe, in denen `C` ist der Typ, in dem die Methode `F` deklariert ist, werden alle Methoden deklariert werden, in dem Basistyp `C` aus der Gruppe entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="411e5-1045">Darüber hinaus Wenn `C` ist ein Klassentyp außer `object`, alle Methoden, die in einen Schnittstellentyp deklariert werden aus der Gruppe entfernt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="411e5-1046">(Diese zweite Regel hat nur Auswirkungen, wenn die Methodengruppe das Ergebnis einer Suche nach Membern für einen Typparameter mit einer effektiven Klasse als Objekt und eine effektive nicht leere-Schnittstelle festgelegt ist.)</span><span class="sxs-lookup"><span data-stu-id="411e5-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="411e5-1047">Wenn die resultierende Menge der möglichen Methoden leer ist, klicken Sie dann auf die folgenden Schritte aus weiteren Verarbeitung abgebrochen werden, und stattdessen wird versucht, den Aufruf als ein Aufruf der Erweiterungsmethode verarbeiten ([Erweiterung Methodenaufrufe](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="411e5-1048">Wenn dies fehlschlägt, klicken Sie dann keine anwendbaren Methoden vorhanden, und beim Auftreten eines Laufzeitfehlers-Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="411e5-1049">Die beste Methode, der den Satz von möglichen Methoden wird mit den Regeln der überladungsauflösung des identifiziert [Überladungsauflösung](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="411e5-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="411e5-1050">Wenn eine einzelne bewährten Methode kann nicht identifiziert werden, der Methodenaufruf ist mehrdeutig, und beim Auftreten eines Laufzeitfehlers-Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="411e5-1051">Bei der Auflösung von funktionsüberladungen durchführen zu können, die Parameter einer generischen Methode gelten nach und Ersetzen Sie dabei die Typargumente (angegeben oder abgeleitet) für die entsprechenden Typparameter der Methode.</span><span class="sxs-lookup"><span data-stu-id="411e5-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="411e5-1052">Abschließende Überprüfung von der gewählten bewährte Methode wird ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="411e5-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="411e5-1053">Die Methode wird im Rahmen der Methodengruppe überprüft: Wenn die beste Methode, eine statische Methode handelt, muss die Methodengruppe zurückzuführen sein, aus einer *Simple_name* oder ein *Member_access* über einen Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="411e5-1054">Wenn die beste Methode eine Instanzmethode ist, muss die Methodengruppe zurückzuführen sein, aus einer *Simple_name*, eine *Member_access* durch eine Variable oder ein Wert, oder ein *Base_access*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="411e5-1055">Wenn keine dieser Anforderungen auf "true" ist, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="411e5-1056">Wenn die bewährte Methode eine generische Methode ist, werden die Typargumente (angegeben oder abgeleitet) für die Einschränkungen überprüft ([Einschränkungen erfüllen](types.md#satisfying-constraints)) für die generische Methode deklariert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="411e5-1057">Wenn jedes Typargument der entsprechenden Einschränkung(en): für den Typparameter nicht erfüllt, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="411e5-1058">Sobald eine Methode ausgewählt und zum Zeitpunkt der Bindung durch die oben genannten Schritte überprüft wurde, wird der tatsächlichen Laufzeit-Aufruf verarbeitet, gemäß den Regeln von der Members-Funktionsaufruf in beschrieben [Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="411e5-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="411e5-1059">Die Auswirkung der oben beschriebenen Regeln intuitivere lautet wie folgt aus: Zum Suchen von bestimmten Methode durch Aufruf einer Methode aufgerufen, beginnen Sie mit der durch Aufruf der Methode angegebenen Typ und entlang der Vererbungskette fortgesetzt, bis mindestens ein anwendbar, zugegriffen werden kann, nicht-Override-Methodendeklaration gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="411e5-1060">Klicken Sie dann führen Sie den Typrückschluss, und der überladungsauflösung, auf den Satz von anwendbar, zugegriffen werden kann, nicht-Override-Methoden, die in diesem Typ deklariert, und rufen die Methode so ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="411e5-1061">Wenn keine Methode gefunden wurde, versuchen Sie es stattdessen den Aufruf als ein Aufruf der Erweiterungsmethode zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="411e5-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="411e5-1062">Extension-Methodenaufrufe</span><span class="sxs-lookup"><span data-stu-id="411e5-1062">Extension method invocations</span></span>

<span data-ttu-id="411e5-1063">In einem Methodenaufruf ([Aufrufe in geschachtelten Instanzen](expressions.md#invocations-on-boxed-instances)) von einem der Formate</span><span class="sxs-lookup"><span data-stu-id="411e5-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="411e5-1064">Wenn der normale Verarbeitung des Aufrufs keine anwendbaren Methoden findet, wird versucht, das Konstrukt als ein Aufruf der Erweiterungsmethode zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="411e5-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="411e5-1065">Wenn *Expr* oder eines der *Args* weist den Typ der Kompilierzeit `dynamic`, Erweiterungsmethoden werden nicht angewendet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="411e5-1066">Das Ziel besteht darin, mit der besten finden *Type_name* `C`, sodass die entsprechenden Aufruf der statischen Methode durchgeführt werden kann:</span><span class="sxs-lookup"><span data-stu-id="411e5-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="411e5-1067">Eine Erweiterungsmethode `Ci.Mj` ist ***berechtigte*** wenn:</span><span class="sxs-lookup"><span data-stu-id="411e5-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="411e5-1068">`Ci` ist eine nicht generische und nicht geschachtelten-Klasse</span><span class="sxs-lookup"><span data-stu-id="411e5-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="411e5-1069">Der Name des `Mj` ist *Bezeichner*</span><span class="sxs-lookup"><span data-stu-id="411e5-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="411e5-1070">`Mj` ist verfügbar und anwendbar, wenn auf die Argumente als statische Methode angewendet werden, wie oben gezeigt</span><span class="sxs-lookup"><span data-stu-id="411e5-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="411e5-1071">Eine implizite identitätsänderung, Verweis- oder Boxing-Konvertierung vorhanden ist, von *Expr* in den Typ des ersten Parameters von `Mj`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="411e5-1072">Die Suche nach `C` wird wie folgt:</span><span class="sxs-lookup"><span data-stu-id="411e5-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="411e5-1073">Beginnend mit der nächsten einschließenden Deklaration des Namespaces anhand jedes einschließenden Namespace-Deklaration und endet mit der enthaltenden Kompilierungseinheit, werden aufeinander folgende Versuche unternommen, um einen möglichen Satz von Erweiterungsmethoden ermitteln:</span><span class="sxs-lookup"><span data-stu-id="411e5-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="411e5-1074">Wenn der angegebene Namespace oder die Kompilierung Organizational Unit direkt nicht generische Typdeklarationen aus `Ci` mit berechtigten Erweiterungsmethoden `Mj`, und klicken Sie dann der Satz von dieser Erweiterungsmethoden die Candidate-Gruppe ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="411e5-1075">Wenn Typen `Ci` importiert *Using_static_declarations* und direkt im von importierten Namespaces deklariert *Using_namespace_directive*s in der angegebenen Namespace oder die Kompilierung Einheit direkt enthält Erweiterungsmethoden für berechtigte `Mj`, und klicken Sie dann der Satz von dieser Erweiterungsmethoden die Candidate-Gruppe ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="411e5-1076">Wenn kein Kandidat festgelegt, die in jeder einschließenden Namespace-Deklaration oder Kompilierung Einheit gefunden wird, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="411e5-1077">Andernfalls wird die Auflösung von funktionsüberladungen Kandidaten festlegen, wie in beschrieben angewendet ([Überladungsauflösung](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="411e5-1078">Wenn bewährte Methoden gefunden wird, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="411e5-1079">`C` ist der Typ, in dem die beste Methode als eine Erweiterungsmethode deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="411e5-1080">Mithilfe von `C` als Ziel, Aufruf der Methode dann als Aufruf einer statischen Methode verarbeitet ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="411e5-1081">Die vorangehenden Regeln bedeuten, dass es sich bei Instanzmethoden Vorrang vor Erweiterungsmethoden haben, dass Erweiterungsmethoden zur Verfügung, in der inneren Namespacedeklarationen gegenüber den Erweiterungsmethoden in den äußeren Namespace-Deklarationen und die Erweiterung verfügbar Vorrang Methoden, die direkt in einem Namespace deklariert haben Vorrang vor Erweiterungsmethoden, die in diesen gleichen Namespace mit einem importierten Namespace-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="411e5-1082">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="411e5-1082">For example:</span></span>
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

<span data-ttu-id="411e5-1083">Im Beispiel `B`Methode hat Vorrang vor die erste Erweiterungsmethode, und `C`Methode hat Vorrang vor beiden Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

<span data-ttu-id="411e5-1084">Die Ausgabe dieses Beispiels ist:</span><span class="sxs-lookup"><span data-stu-id="411e5-1084">The output of this example is:</span></span>
```
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="411e5-1085">`D.G` hat Vorrang vor `C.G`, und `E.F` hat Vorrang vor beide `D.F` und `C.F`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="411e5-1086">Delegieren von Aufrufen</span><span class="sxs-lookup"><span data-stu-id="411e5-1086">Delegate invocations</span></span>

<span data-ttu-id="411e5-1087">Für einen Delegataufruf der *Primary_expression* von der *Invocation_expression* muss sein Wert eine *Delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="411e5-1088">Darüber hinaus in Betracht ziehen die *Delegate_type* ein Funktionsmember mit derselben Parameterliste als sein die *Delegate_type*, wird die *Delegate_type* muss anwendbar ( [Anwendbarer Funktionsmember](expressions.md#applicable-function-member)) in Bezug auf die *Argument_list* von der *Invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="411e5-1089">Die Run-Time-Verarbeitung von einem Delegataufruf des Formulars `D(A)`, wobei `D` ist eine *Primary_expression* von einer *Delegate_type* und `A` ist eine optionale *Argument_list*, umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="411e5-1090">`D` wird ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1090">`D` is evaluated.</span></span> <span data-ttu-id="411e5-1091">Wenn diese Evaluierungsversion auf eine Ausnahme auslöst, werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="411e5-1092">Der Wert des `D` wird überprüft, um gültig sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="411e5-1093">Wenn der Wert des `D` ist `null`, `System.NullReferenceException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="411e5-1094">Andernfalls `D` ist ein Verweis auf eine Delegatinstanz.</span><span class="sxs-lookup"><span data-stu-id="411e5-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="411e5-1095">Member Funktionsaufrufen ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) jede der aufrufbaren Entitäten in der Aufrufliste des Delegaten ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="411e5-1096">Für aufrufbare Entitäten, die aus einer Instanz und eine Instanzmethode besteht ist die Instanz für den Aufruf der Instanz, die in die aufrufbare Entität enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="411e5-1097">Elementzugriff</span><span class="sxs-lookup"><span data-stu-id="411e5-1097">Element access</span></span>

<span data-ttu-id="411e5-1098">Ein *Element_access* besteht aus einer *Primary_no_array_creation_expression*, gefolgt von einer "`[`" token, gefolgt von ein *Argument_list*, gefolgt von einer " `]`"token.</span><span class="sxs-lookup"><span data-stu-id="411e5-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="411e5-1099">Die *Argument_list* besteht aus einem oder mehreren *Argument*s, die durch Kommas getrennt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="411e5-1100">Die *Argument_list* von einem *Element_access* darf nicht enthalten `ref` oder `out` Argumente.</span><span class="sxs-lookup"><span data-stu-id="411e5-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="411e5-1101">Ein *Element_access* dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)), wenn mindestens eine der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="411e5-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="411e5-1102">Die *Primary_no_array_creation_expression* weist den Typ der Kompilierzeit `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="411e5-1103">Mindestens ein Ausdruck, der die *Argument_list* weist den Typ der Kompilierzeit `dynamic` und *Primary_no_array_creation_expression* verfügt nicht über einen Arraytyp.</span><span class="sxs-lookup"><span data-stu-id="411e5-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="411e5-1104">In diesem Fall der Compiler klassifiziert die *Element_access* als ein Wert vom Typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="411e5-1105">Den Regeln unten, um zu bestimmen, die Bedeutung des das *Element_access* gelten dann zur Laufzeit, der Kompilierzeittyp, der von der Laufzeit-Typinformationen anstelle der *Primary_no_array_creation_expression*und *Argument_list* Ausdrücke mit der Kompilierzeittyp `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="411e5-1106">Wenn die *Primary_no_array_creation_expression* hat keinen Kompilierzeittyp `dynamic`, und klicken Sie dann den Elementzugriff eine begrenzte Kompilierung Zeit Überprüfung durchgeführt wird, wie in beschrieben [Überprüfungen zur Kompilierzeit dynamisch Auflösen der Überladung](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="411e5-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="411e5-1107">Wenn die *Primary_no_array_creation_expression* von eine *Element_access* ist ein Wert, der eine *Array_type*, wird die *Element_access* ist ein Array-Zugriff ([Array Zugriff](expressions.md#array-access)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="411e5-1108">Andernfalls die *Primary_no_array_creation_expression* muss eine Variable oder den Wert der Klasse, Struktur oder Schnittstellentyp, der einen oder mehrere Indexer-Member, in diesem Fall verfügt die *Element_access* ist ein Indexzugriff ([Indexerzugriff](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="411e5-1109">Arrayzugriff</span><span class="sxs-lookup"><span data-stu-id="411e5-1109">Array access</span></span>

<span data-ttu-id="411e5-1110">Für ein Arrayzugriff verfügt die *Primary_no_array_creation_expression* von der *Element_access* muss sein Wert eine *Array_type*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="411e5-1111">Darüber hinaus die *Argument_list* eines Arrays Zugriff ist nicht zulässig, benannte Argumente enthält. Die Anzahl der Ausdrücke in der *Argument_list* muss identisch mit den Rang der *Array_type*, und jeder Ausdruck muss vom Typ `int`, `uint`, `long`, `ulong`, oder muss implizit in eine oder mehrere der folgenden Typen sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="411e5-1112">Das Ergebnis der Auswertung ein Arrayzugriff verfügt, wird eine Variable vom Typ Elements des Arrays, nämlich dem Array-Element, das ausgewählt, indem die Werte der Ausdrücke in der *Argument_list*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="411e5-1113">Die Verarbeitung zur Laufzeit ein Arrayzugriff verfügt der Form `P[A]`, wobei `P` ist eine *Primary_no_array_creation_expression* von einer *Array_type* und `A` ist ein *Argument_list*, umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="411e5-1114">`P` wird ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1114">`P` is evaluated.</span></span> <span data-ttu-id="411e5-1115">Wenn diese Evaluierungsversion auf eine Ausnahme auslöst, werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="411e5-1116">Der Index von Ausdrücken für die *Argument_list* werden in der Reihenfolge von links nach rechts ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="411e5-1117">Befolgen Sie die Auswertung jedes Ausdrucks Index, eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) auf einen der folgenden Typen erfolgt: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="411e5-1118">Der erste Typ in dieser Liste, die für die eine implizite Konvertierung vorhanden ist, wird ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="411e5-1119">Z. B. wenn der Indexausdruck vom Typ `short` klicken Sie dann eine implizite Konvertierung in `int` ausgeführt, da implizite Konvertierungen aus `short` zu `int` und `short` zu `long` sind möglich.</span><span class="sxs-lookup"><span data-stu-id="411e5-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="411e5-1120">Wenn die Auswertung von Ausdruckswerten Index oder die nachfolgenden implizite Konvertierung eine Ausnahme verursacht hat, klicken Sie dann keine weiteren Indexausdrücke werden ausgewertet, und keine weiteren Schritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="411e5-1121">Der Wert des `P` wird überprüft, um gültig sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="411e5-1122">Wenn der Wert des `P` ist `null`, `System.NullReferenceException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="411e5-1123">Der Wert der einzelnen Ausdrücke aus der *Argument_list* aktiviert ist, für die eigentlichen Grenzen jeder Arraydimension der Arrayinstanz verweist `P`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="411e5-1124">Wenn eine oder mehrere Werte außerhalb des gültigen Bereichs, sind ein `System.IndexOutOfRangeException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="411e5-1125">Der Speicherort des Array-Elements vom Index-Ausdrücke wird berechnet, und Hier wird das Ergebnis der Arrayzugriff.</span><span class="sxs-lookup"><span data-stu-id="411e5-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="411e5-1126">Indexerzugriff</span><span class="sxs-lookup"><span data-stu-id="411e5-1126">Indexer access</span></span>

<span data-ttu-id="411e5-1127">Indexzugriff die *Primary_no_array_creation_expression* von der *Element_access* muss eine Variable oder ein Wert, der eine Klasse, Struktur oder der Schnittstellentyp, und dieser Typ muss eine oder mehrere implementieren Indexer, die in Bezug auf gelten die *Argument_list* von der *Element_access*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="411e5-1128">Die Bindung-Time-Verarbeitung der Indexzugriff des Formulars `P[A]`, wobei `P` ist eine *Primary_no_array_creation_expression* der Klasse, Struktur oder Schnittstellentyp `T`, und `A` ist ein *Argument_list*, umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="411e5-1129">Der Satz von Indexern, die von bereitgestellte `T` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="411e5-1130">Der Satz besteht aus allen in deklarierten Indexer `T` oder einem Basistyp von `T` , die sich nicht `override` Deklarationen und sind im aktuellen Kontext zugegriffen werden kann ([Memberzugriff](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="411e5-1131">Die Gruppe ist für diesen Indexer, die anwendbar und nicht versteckt sind von anderen Indexern reduziert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="411e5-1132">Die folgenden Regeln gelten, jeder Indexer `S.I` in der Gruppe, in denen `S` ist der Typ, in dem der Indexer `I` deklariert wird:</span><span class="sxs-lookup"><span data-stu-id="411e5-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="411e5-1133">Wenn `I` ist nicht anwendbar, in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)), klicken Sie dann `I` aus der Gruppe entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="411e5-1134">Wenn `I` gilt in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)), in dem Basistyp deklariert alle Indexer `S` aus der Gruppe entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="411e5-1135">Wenn `I` gilt in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)) und `S` ist ein Klassentyp außer `object`, alle in einer Schnittstelle deklarierten Indexer aus der Gruppe entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="411e5-1136">Wenn die resultierende Menge der Kandidat Indexer leer ist, klicken Sie dann keine anwendbaren Indexer vorhanden, und beim Auftreten eines Laufzeitfehlers-Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="411e5-1137">Der beste Indexer, der den Satz von Kandidaten Indexer wird mit den Regeln der überladungsauflösung des identifiziert [Überladungsauflösung](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="411e5-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="411e5-1138">Wenn ein einzelner bewährte Indexer kann nicht identifiziert werden, der Indexer ist nicht eindeutig, und beim Auftreten eines Laufzeitfehlers-Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="411e5-1139">Der Index von Ausdrücken für die *Argument_list* werden in der Reihenfolge von links nach rechts ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="411e5-1140">Das Ergebnis der Verarbeitung der Indexerzugriff ist ein Ausdruck, der als Indexzugriff klassifiziert sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="411e5-1141">Der Indexer Zugriffsausdruck verweist auf den Indexer, der im vorherigen Schritt bestimmt und verfügt über einen zugeordneten Instanzausdruck von `P` sowie eine Liste zugeordnete Argument `A`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="411e5-1142">Je nach Kontext, in denen es verwendet wird, verursacht das Indexzugriff Aufrufen des entweder die *get-Accessor* oder *set-Accessor* des Indexers.</span><span class="sxs-lookup"><span data-stu-id="411e5-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="411e5-1143">Wenn der Indexzugriff auf das Ziel einer Zuweisung, ist die *set-Accessor* wird aufgerufen, um einen neuen Wert zuzuweisen ([einfache Zuweisung](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="411e5-1144">In allen anderen Fällen die *get-Accessor* wird aufgerufen, um den aktuellen Wert zu erhalten ([Werte Ausdrücke](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="411e5-1145">Dieser Zugriff</span><span class="sxs-lookup"><span data-stu-id="411e5-1145">This access</span></span>

<span data-ttu-id="411e5-1146">Ein *This_access* besteht aus dem reservierten Wort `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="411e5-1147">Ein *This_access* ist zulässig, nur in der *Block* Instanzkonstruktor, eine Instanzmethode oder einen Instanzaccessor.</span><span class="sxs-lookup"><span data-stu-id="411e5-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="411e5-1148">Es verfügt über eine der folgenden Bedeutungen:</span><span class="sxs-lookup"><span data-stu-id="411e5-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="411e5-1149">Wenn `this` werden in einem *Primary_expression* im Instanzenkonstruktor eine einer Klasse, die sie als Wert klassifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="411e5-1150">Der Typ des Werts ist der Instanztyp ([den Instanztyp](classes.md#the-instance-type)) der Klasse in der ein Fehler auftritt, und der Wert ist ein Verweis auf das Objekt erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="411e5-1151">Wenn `this` werden in einem *Primary_expression* innerhalb einer Instanzenmethode oder Instanzaccessor einer Klasse, wird es als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="411e5-1152">Der Typ des Werts ist der Instanztyp ([den Instanztyp](classes.md#the-instance-type)) der Klasse in der ein Fehler auftritt, und der Wert ist ein Verweis auf das Objekt für die die Methode oder der Accessor aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="411e5-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="411e5-1153">Wenn `this` werden in einem *Primary_expression* innerhalb eines Instanzkonstruktors einer Struktur, wird er als Variable klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="411e5-1154">Der Typ der Variablen ist der Instanztyp ([den Instanztyp](classes.md#the-instance-type)) der Struktur klicken Sie in die Nutzung wird, und die Variable verweist auf die Struktur, die erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="411e5-1155">Die `this` Variable eines Instanzkonstruktors einer Struktur verhält sich genau wie ein `out` Parameter des Strukturtyps – insbesondere bedeutet dies, dass die Variable auf jeden Fall in allen Ausführungspfaden der Instanz zugewiesen werden muss Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="411e5-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="411e5-1156">Wenn `this` werden in einem *Primary_expression* innerhalb einer Instanzenmethode oder Instanzaccessor einer Struktur, wird er als Variable klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="411e5-1157">Der Typ der Variablen ist der Instanztyp ([den Instanztyp](classes.md#the-instance-type)) der Struktur, in dem sich die Verwendung stattfindet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="411e5-1158">Wenn die Methode oder der Accessor kein Iterator ist ([Iteratoren](classes.md#iterators)), wird die `this` Variable darstellt, die Struktur für die die Methode oder der Accessor wurde aufgerufen, und verhält sich genauso wie eine `ref` Parameter des Strukturtyps.</span><span class="sxs-lookup"><span data-stu-id="411e5-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="411e5-1159">Wenn die Methode oder der Accessor ein Iterator ist, der `this` Variable darstellt, eine Kopie der Struktur für die die Methode oder der Accessor wurde aufgerufen, und verhält sich genau wie ein Value-Parameter des Strukturtyps.</span><span class="sxs-lookup"><span data-stu-id="411e5-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="411e5-1160">Verwenden von `this` in einem *Primary_expression* in einem anderen Kontext als oben aufgeführt ist ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="411e5-1161">Insbesondere ist es nicht möglich, verweisen auf `this` in einer statischen Methode, eine statische Eigenschaftenaccessor oder in einem *Variable_initializer* einer Felddeklaration.</span><span class="sxs-lookup"><span data-stu-id="411e5-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="411e5-1162">Basiszugriff</span><span class="sxs-lookup"><span data-stu-id="411e5-1162">Base access</span></span>

<span data-ttu-id="411e5-1163">Ein *Base_access* besteht aus dem reservierten Wort `base` gefolgt von entweder eine "`.`" Zugriffstoken und ein Bezeichner oder ein *Argument_list* in eckige Klammern eingeschlossen:</span><span class="sxs-lookup"><span data-stu-id="411e5-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="411e5-1164">Ein *Base_access* wird verwendet, um der Member der Basisklasse zuzugreifen, die von den entsprechend benannten Elemente in der aktuellen Klasse oder Struktur ausgeblendet werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="411e5-1165">Ein *Base_access* ist zulässig, nur in der *Block* Instanzkonstruktor, eine Instanzmethode oder einen Instanzaccessor.</span><span class="sxs-lookup"><span data-stu-id="411e5-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="411e5-1166">Wenn `base.I` tritt auf, in einer Klasse oder Struktur `I` muss ein Member der Basisklasse der Klasse oder Struktur zu kennzeichnen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="411e5-1167">Auch wenn `base[E]` tritt auf, in einer Klasse ein passender Indexer muss vorhanden sein, in der Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="411e5-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="411e5-1168">Zum Zeitpunkt der Bindung *Base_access* Ausdrücken der Form `base.I` und `base[E]` werden ausgewertet, als ob sie geschrieben wurden `((B)this).I` und `((B)this)[E]`, wobei `B` ist die Basisklasse der Klasse oder eine Struktur, die in dem das Konstrukt auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="411e5-1169">Daher `base.I` und `base[E]` entsprechen `this.I` und `this[E]`, mit Ausnahme von `this` als eine Instanz der Basisklasse angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="411e5-1170">Wenn eine *Base_access* verweist auf ein virtuelle Funktion-Element (eine Methode, Eigenschaft, oder Indexer), die Bestimmung der Funktion zur Laufzeit aufzurufenden Member ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung ](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) geändert wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="411e5-1171">Der Funktionsmember der Member, die aufgerufen wird, richtet sich nach der Suche nach der am weitesten abgeleiteten Implementierung ([virtuelle Methoden](classes.md#virtual-methods)) des Funktionsmembers in Bezug auf `B` (anstelle von in Bezug auf den Laufzeittyp der `this`, wie üblich in einer nicht-Base Zugriff wäre).</span><span class="sxs-lookup"><span data-stu-id="411e5-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="411e5-1172">Daher innerhalb eine `override` von einer `virtual` Funktionsmember der Member, eine *Base_access* kann zum Aufrufen der geerbten Implementierung von der Funktionsmember der Member verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="411e5-1173">Wenn das Funktionselement verweist eine *Base_access* ist eine abstrakte tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="411e5-1174">Postfix-Inkrementoperator und Postfix-Dekrementoperator</span><span class="sxs-lookup"><span data-stu-id="411e5-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="411e5-1175">Der Operand eines Postfixinkrement oder Dekrement Operation muss ein Ausdruck, der als eine Variable, einen Eigenschaftenzugriff oder Indexzugriff klassifiziert sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="411e5-1176">Das Ergebnis des Vorgangs ist ein Wert des gleichen Typs als Operand.</span><span class="sxs-lookup"><span data-stu-id="411e5-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="411e5-1177">Wenn die *Primary_expression* weist den Typ der Kompilierzeit `dynamic` und klicken Sie dann der Operator dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)), wird die *Post_increment_expression*oder *Post_decrement_expression* weist den Typ der Kompilierzeit `dynamic` und die folgenden Regeln gelten zur Laufzeit mit dem Laufzeit-Typ, der die *Primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="411e5-1178">Wenn der Operand einem Postfix erhöhen oder dekrementvorgang ist eine Eigenschaft oder Zugriffs auf Indexer, Eigenschaft oder des Indexers benötigen Sie Folgendes ein `get` und `set` Accessor.</span><span class="sxs-lookup"><span data-stu-id="411e5-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="411e5-1179">Wenn dies nicht der Fall ist, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="411e5-1180">Unäroperator der überladungsauflösung ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-1181">Vordefinierte `++` und `--` Operatoren vorhanden, für die folgenden Typen: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, und alle Enumerationstypen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="411e5-1182">Die vordefinierten `++` Operatoren zurückgegeben, den von den Operanden und den vordefinierten 1 hinzugefügt erzeugten Wert `--` Operatoren geben einen Wert durch Subtrahieren der Operand 1 zurück.</span><span class="sxs-lookup"><span data-stu-id="411e5-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="411e5-1183">In einem `checked` Kontext, wenn das Ergebnis dieser Addition oder Subtraktion außerhalb des Bereichs des Ergebnistyps ist und der Ergebnistyp ein ganzzahliger Typ oder ein Enumerationstyp ist, eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="411e5-1184">Die Verarbeitung zur Laufzeit ein Postfixinkrement oder dekrementvorgang des Formulars `x++` oder `x--` umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="411e5-1185">Wenn `x` wird als Variable klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="411e5-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="411e5-1186">`x` wird ausgewertet, um die Variable zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="411e5-1187">Der Wert des `x` gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="411e5-1188">Der ausgewählte Operator wird aufgerufen, mit dem gespeicherten Wert von `x` als Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="411e5-1189">Der Wert, der vom Operator zurückgegebenen befindet sich in den Speicherort, durch die Auswertung von `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="411e5-1190">Der gespeicherte Wert des `x` wird das Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="411e5-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="411e5-1191">Wenn `x` wird als eine Eigenschaft oder der Indexer Zugriff klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="411e5-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="411e5-1192">Der Instanzausdruck (Wenn `x` ist nicht `static`) und die Argumentliste (Wenn `x` Indexzugriff ist) zugeordneten `x` werden ausgewertet, und die Ergebnisse werden in der nachfolgenden verwendet `get` und `set` Accessor-Aufrufe.</span><span class="sxs-lookup"><span data-stu-id="411e5-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="411e5-1193">Die `get` Accessor `x` wird aufgerufen, und der zurückgegebene Wert gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="411e5-1194">Der ausgewählte Operator wird aufgerufen, mit dem gespeicherten Wert von `x` als Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="411e5-1195">Die `set` Accessor `x` wird aufgerufen, mit dem Wert, der zurückgegeben wird, durch den Operator als seine `value` Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="411e5-1196">Der gespeicherte Wert des `x` wird das Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="411e5-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="411e5-1197">Die `++` und `--` Operatoren unterstützt auch die Präfix-Notation ([Präfix-Inkrement und Dekrement-Operatoren](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="411e5-1198">In der Regel das Ergebnis des `x++` oder `x--` ist der Wert des `x` vor dem Vorgang, während das Ergebnis des `++x` oder `--x` ist der Wert des `x` nach Abschluss des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="411e5-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="411e5-1199">In beiden Fällen `x` selbst hat den gleichen Wert nach Abschluss des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="411e5-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="411e5-1200">Ein `operator ++` oder `operator --` Implementierung kann mithilfe der Präfix oder Postfix-Notation aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="411e5-1201">Es ist nicht möglich, separate Implementierungen für die beiden Notationen zu haben.</span><span class="sxs-lookup"><span data-stu-id="411e5-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="411e5-1202">Der new-Operator</span><span class="sxs-lookup"><span data-stu-id="411e5-1202">The new operator</span></span>

<span data-ttu-id="411e5-1203">Die `new` Operator wird verwendet, um neue Instanzen von Typen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="411e5-1204">Es gibt drei Arten von `new` Ausdrücke:</span><span class="sxs-lookup"><span data-stu-id="411e5-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="411e5-1205">Objektausdrücke-Erstellung werden verwendet, um neue Instanzen der Klasse und Werttypen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="411e5-1206">Array-erstellen-Ausdrücke werden verwendet, zum Erstellen neuer Instanzen von Arraytypen dar.</span><span class="sxs-lookup"><span data-stu-id="411e5-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="411e5-1207">Delegat erstellen Ausdrücke werden verwendet, um neue Instanzen von Delegaten erstellt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="411e5-1208">Die `new` Operator impliziert die Erstellung einer Instanz eines Typs, aber es ist nicht notwendigerweise, dass dynamische Zuordnung von Arbeitsspeicher.</span><span class="sxs-lookup"><span data-stu-id="411e5-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="411e5-1209">Insbesondere bei Instanzen von Werttypen benötigt keinen zusätzlichen Speicher über die Variablen in der sie sich befinden, und keine dynamischen Zuordnungen auftreten, wenn `new` wird verwendet, um Instanzen von Werttypen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="411e5-1210">Objektausdrücke-Erstellung</span><span class="sxs-lookup"><span data-stu-id="411e5-1210">Object creation expressions</span></span>

<span data-ttu-id="411e5-1211">Ein *Object_creation_expression* wird verwendet, um eine neue Instanz der Erstellen einer *Class_type* oder *Value_type*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

<span data-ttu-id="411e5-1212">Die *Typ* von einer *Object_creation_expression* muss eine *Class_type*, *Value_type* oder ein *Type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="411e5-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="411e5-1213">Die *Typ* nicht möglich, eine `abstract` *Class_type*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="411e5-1214">Der optionale *Argument_list* ([Argumentlisten](expressions.md#argument-lists)) ist nur zulässig, wenn die *Typ* ist eine *Class_type* oder *Struct_ Typ*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="411e5-1215">Ein Objekterstellungsausdruck kann die Liste des Konstruktorarguments weglassen und einschließende Klammern angegeben, dass es sich um ein Objektinitialisierer oder Auflistungsinitialisierer enthält.</span><span class="sxs-lookup"><span data-stu-id="411e5-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="411e5-1216">Die Liste des Konstruktorarguments auslassen und durch Einschließen von Klammern ist gleichbedeutend mit der Angabe einer leeren Argumentliste.</span><span class="sxs-lookup"><span data-stu-id="411e5-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="411e5-1217">Verarbeitung von einer Objekterstellungsausdruck, der einen Objektinitialisierer oder Auflistungsinitialisierer enthält besteht aus den Instanzkonstruktor zuerst zu verarbeiten und wird bei der Verarbeitung der Member oder Element Initialisierungen, die durch den Objektinitialisierer (angegeben[ Objektinitialisierer](expressions.md#object-initializers)) oder einem Auflistungsinitialisierer ([Auflistungsinitialisierer](expressions.md#collection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="411e5-1218">Wenn eines der Argumente in der optionalen *Argument_list* weist den Typ der Kompilierzeit `dynamic` die *Object_creation_expression* dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)) und die folgenden Regeln gelten zur Laufzeit mit dem Laufzeit-Typ, der diese Argumente von der *Argument_list* Kompilierzeittyp deren `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="411e5-1219">Allerdings wird die objekterstellung überprüfen eine begrenzte Kompilieren der Zeit vorgenommen, wie in beschrieben [Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="411e5-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="411e5-1220">Die laufzeitverarbeitung der Bindung einer *Object_creation_expression* des Formulars `new T(A)`, wobei `T` ist eine *Class_type* oder *Value_type* und `A` ist eine optionale *Argument_list*, umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="411e5-1221">Wenn `T` ist eine *Value_type* und `A` ist nicht vorhanden:</span><span class="sxs-lookup"><span data-stu-id="411e5-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="411e5-1222">Die *Object_creation_expression* ist ein Standard-Konstruktor-Funktionsaufruf.</span><span class="sxs-lookup"><span data-stu-id="411e5-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="411e5-1223">Das Ergebnis der *Object_creation_expression* ist ein Wert vom Typ `T`, d. h. der Standardwert für `T` gemäß [die System.ValueType-Typ](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="411e5-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="411e5-1224">Andernfalls gilt: Wenn `T` ist eine *Type_parameter* und `A` ist nicht vorhanden:</span><span class="sxs-lookup"><span data-stu-id="411e5-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="411e5-1225">Wenn kein werttypeinschränkung oder Konstruktoreinschränkung ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)) wurde angegeben `T`, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="411e5-1226">Das Ergebnis der *Object_creation_expression* ist ein Wert von der Run-Time-Typ, der der Type-Parameter gebunden wurde, d. h. das Ergebnis von Aufrufen des Standardkonstruktors des Typs.</span><span class="sxs-lookup"><span data-stu-id="411e5-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="411e5-1227">Der Laufzeit-Typ kann es sich um ein Verweistyp oder ein Werttyp sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="411e5-1228">Andernfalls gilt: Wenn `T` ist eine *Class_type* oder *Struct_type*:</span><span class="sxs-lookup"><span data-stu-id="411e5-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="411e5-1229">Wenn `T` ist ein `abstract` *Class_type*, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="411e5-1230">Die Instanzkonstruktor aufrufen wird bestimmt, mit den Regeln der überladungsauflösung des [Überladungsauflösung](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="411e5-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="411e5-1231">Der Satz von Kandidaten Instanzkonstruktoren besteht aus allen verfügbaren Instanzkonstruktoren in deklarierten `T` der gelten in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="411e5-1232">Wenn der Satz von Kandidaten Instanzkonstruktoren leer ist, oder ein einzelnen bewährte Instanzenkonstruktor nicht identifiziert werden kann, tritt auf, ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="411e5-1233">Das Ergebnis der *Object_creation_expression* ist ein Wert vom Typ `T`, d. h. der Wert erzeugt, durch Aufrufen des Instanzkonstruktors im vorherigen Schritt bestimmt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="411e5-1234">Andernfalls die *Object_creation_expression* ist ungültig, und ein Fehler während der Bindung erfolgt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="411e5-1235">Auch wenn die *Object_creation_expression* dynamisch gebunden, der während der Kompilierung-Typ ist immer noch `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="411e5-1236">Die Verarbeitung zur Laufzeit eine *Object_creation_expression* des Formulars `new T(A)`, wobei `T` ist *Class_type* oder *Struct_type* und `A` ist eine optionale *Argument_list*, umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="411e5-1237">Wenn `T` ist eine *Class_type*:</span><span class="sxs-lookup"><span data-stu-id="411e5-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="411e5-1238">Eine neue Instanz der Klasse `T` zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="411e5-1239">Wenn nicht genügend Arbeitsspeicher verfügbar, um die neue Instanz einer `System.OutOfMemoryException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="411e5-1240">Alle Felder der neuen Instanz werden auf ihre Standardwerte initialisiert ([Standardwerte](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="411e5-1241">Der Instanzkonstruktor wird aufgerufen, gemäß den Regeln von der Members-Funktionsaufruf ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="411e5-1242">Ein Verweis auf das neu zugeordnete Instanz automatisch an die Instanzkonstruktor übergeben wird und die Instanz kann zugegriffen werden aus diesen Konstruktor als `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="411e5-1243">Wenn `T` ist eine *Struct_type*:</span><span class="sxs-lookup"><span data-stu-id="411e5-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="411e5-1244">Eine Instanz des Typs `T` wird erstellt, indem Sie eine temporäre lokale Variable zuordnen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="411e5-1245">Seit einem Instanzenkonstruktor der ein *Struct_type* ist erforderlich, um auf jeden Fall wird jedes Feld der-Instanz erstellt wird, wird keine Initialisierung der temporären Variable muss einen Wert zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="411e5-1246">Der Instanzkonstruktor wird aufgerufen, gemäß den Regeln von der Members-Funktionsaufruf ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="411e5-1247">Ein Verweis auf das neu zugeordnete Instanz automatisch an die Instanzkonstruktor übergeben wird und die Instanz kann zugegriffen werden aus diesen Konstruktor als `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="411e5-1248">Objektinitialisierer</span><span class="sxs-lookup"><span data-stu-id="411e5-1248">Object initializers</span></span>

<span data-ttu-id="411e5-1249">Ein ***Objektinitialisierer*** gibt Werte für NULL oder mehr Felder, Eigenschaften oder indizierte Elemente eines Objekts.</span><span class="sxs-lookup"><span data-stu-id="411e5-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

<span data-ttu-id="411e5-1250">Ein Objektinitialisierer besteht aus einer Sequenz der Standardmember-Initialisierer, eingeschlossene `{` und `}` Token und durch Kommas getrennt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="411e5-1251">Jede *Member_initializer* kennzeichnet ein Ziel für die Initialisierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="411e5-1252">Ein *Bezeichner* müssen den Namen einer zugänglichen Feld oder -Eigenschaft des Objekts initialisiert wird, während ein *Argument_list* eingeschlossen in eckigen Klammern müssen die Argumente für einen Indexer zugegriffen werden kann angeben, auf die das Objekt initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="411e5-1253">Es ist ein Fehler für einen Objektinitialisierer, um mehr als ein Member-Initialisierer für das gleiche Feld oder Eigenschaft enthalten.</span><span class="sxs-lookup"><span data-stu-id="411e5-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="411e5-1254">Jede *Initializer_target* ist, gefolgt von einem Gleichheitszeichen und entweder ein Ausdruck, einen Objektinitialisierer oder einem Auflistungsinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="411e5-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="411e5-1255">Es ist nicht möglich, für die Ausdrücke in den Objektinitialisierer, um auf das neu erstellte Objekt zu verweisen, die es initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="411e5-1256">Ein Member-Initialisierer, der einen Ausdruck angibt, nach die Gleichheitszeichen auf die gleiche Weise wie eine Zuordnung verarbeitet werden ([einfache Zuweisung](expressions.md#simple-assignment)) an das Ziel.</span><span class="sxs-lookup"><span data-stu-id="411e5-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="411e5-1257">Ein Member-Initialisierer, der einen Objektinitialisierer angibt, nach die Gleichheitszeichen ist eine ***geschachtelte Objektinitialisierer***, d. h. eine Initialisierung eines eingebetteten Objekts.</span><span class="sxs-lookup"><span data-stu-id="411e5-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="411e5-1258">Anstatt das Feld oder die Eigenschaft einen neuen Wert zuzuweisen, werden die Zuweisungen aus dem geschachtelten Objektinitialisierer als Zuweisungen auf Member des Felds oder der Eigenschaft behandelt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="411e5-1259">Geschachtelte Objektinitialisierer können nicht angewendet werden, um Eigenschaften mit einem Werttyp oder schreibgeschützte Felder mit einem Werttyp.</span><span class="sxs-lookup"><span data-stu-id="411e5-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="411e5-1260">Ein Member-Initialisierer, der angibt, einen Auflistungsinitialisierer nach dem Gleichheitszeichen ist eine Initialisierung einer eingebetteten Auflistung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="411e5-1261">Anstatt das Feld "Ziel", Eigenschaft oder der Indexer eine neue Sammlung zuzuweisen, werden die Elemente im Initialisierer erhält der Auflistung, die auf das Ziel hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="411e5-1262">Das Ziel eines Auflistungstyps, das die in angegebenen Anforderungen erfüllt sein muss [Auflistungsinitialisierer](expressions.md#collection-initializers).</span><span class="sxs-lookup"><span data-stu-id="411e5-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="411e5-1263">Die Argumente für einen Index Initialisierer werden immer genau einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="411e5-1264">Also auch wenn die Argumente am Ende niemals verwendet (z. B. aufgrund eines leeren geschachtelten Initialisierers), werden sie für ihre Nebeneffekte ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="411e5-1265">Die folgende Klasse stellt zwei Koordinaten dar:</span><span class="sxs-lookup"><span data-stu-id="411e5-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="411e5-1266">Eine Instanz von `Point` erstellt und wie folgt initialisiert werden:</span><span class="sxs-lookup"><span data-stu-id="411e5-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="411e5-1267">Das hat dieselbe Wirkung wie das</span><span class="sxs-lookup"><span data-stu-id="411e5-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="411e5-1268">wo `__a` ist eine temporäre Variable andernfalls nicht sichtbare und kann nicht zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="411e5-1269">Die folgende Klasse stellt ein Rechteck, das aus zwei Punkten erstellt:</span><span class="sxs-lookup"><span data-stu-id="411e5-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="411e5-1270">Eine Instanz von `Rectangle` erstellt und wie folgt initialisiert werden:</span><span class="sxs-lookup"><span data-stu-id="411e5-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="411e5-1271">Das hat dieselbe Wirkung wie das</span><span class="sxs-lookup"><span data-stu-id="411e5-1271">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
<span data-ttu-id="411e5-1272">wo `__r`, `__p1` und `__p2` werden temporäre Variablen, die andernfalls nicht sichtbar und kann nicht zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="411e5-1273">Wenn `Rectangle`Konstruktor weist die beiden eingebettet `Point` Instanzen</span><span class="sxs-lookup"><span data-stu-id="411e5-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="411e5-1274">Das folgende Konstrukt verwendet werden kann, um das eingebettete initialisieren `Point` Instanzen, anstatt neue Instanzen:</span><span class="sxs-lookup"><span data-stu-id="411e5-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="411e5-1275">Das hat dieselbe Wirkung wie das</span><span class="sxs-lookup"><span data-stu-id="411e5-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="411e5-1276">Betrachten Sie eine Definition von C, im folgenden Beispiel an:</span><span class="sxs-lookup"><span data-stu-id="411e5-1276">Given an appropriate definition of C, the following example:</span></span>
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
<span data-ttu-id="411e5-1277">entspricht diese Reihe von Zuweisungen:</span><span class="sxs-lookup"><span data-stu-id="411e5-1277">is equivalent to this series of assignments:</span></span>
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
<span data-ttu-id="411e5-1278">wo `__c`usw. werden generierte Variablen, die nicht sichtbar und für den Quellcode nicht zugänglich sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="411e5-1279">Beachten Sie, dass die Argumente für `[0,0]` sind nur ein Mal ausgewertet, und die Argumente für `[1,2]` einmal ausgewertet werden, auch wenn sie nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="411e5-1280">Auflistungsinitialisierer</span><span class="sxs-lookup"><span data-stu-id="411e5-1280">Collection initializers</span></span>

<span data-ttu-id="411e5-1281">Ein Auflistungsinitialisierer gibt an, die Elemente einer Auflistung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1281">A collection initializer specifies the elements of a collection.</span></span>

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

<span data-ttu-id="411e5-1282">Ein Auflistungsinitialisierer besteht aus einer Folge von elementinitialisierern, eingeschlossene `{` und `}` Token und durch Kommas getrennt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="411e5-1283">Jeder Elementinitialisierer gibt ein Element, das initialisierte Objekt hinzugefügt werden, und besteht aus einer Liste von Ausdrücken, die vom eingeschlossen `{` und `}` Token und durch Kommas getrennt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="411e5-1284">Einen einzelnen Ausdrücken Elementinitialisierer ohne geschweifte Klammern geschrieben werden kann, jedoch nicht klicken Sie dann ein Zuweisungsausdruck, um Mehrdeutigkeiten zu vermeiden, mit der Standardmember-Initialisierer.</span><span class="sxs-lookup"><span data-stu-id="411e5-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="411e5-1285">Die *Non_assignment_expression* Produktion wird definiert, [Ausdruck](expressions.md#expression).</span><span class="sxs-lookup"><span data-stu-id="411e5-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="411e5-1286">Im folgenden finden ein Beispiel für eine Objekterstellungsausdruck, die einen auflistungs-bzw. Arrayinitialisierers enthält:</span><span class="sxs-lookup"><span data-stu-id="411e5-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="411e5-1287">Das Auflistungsobjekt, das für die ein Auflistungsinitialisierer gilt muss von einem Typ sein, die implementiert `System.Collections.IEnumerable` oder ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="411e5-1288">Für jedes Element in der Reihenfolge angegeben, wird der Auflistungsinitialisierer Ruft eine `Add` Methode auf der Ziel-Objekt mit der Liste mit Ausdrücken des elementinitialisierers als Argumentliste normalen Membersuche anwenden und für jeden Aufruf der überladungsauflösung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="411e5-1289">Daher müssen das Objekt eine anwendbare Instanz- oder Erweiterungsmethode-Methode mit dem Namen `Add` für jedes Elementinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="411e5-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="411e5-1290">Die folgende Klasse stellt einen Kontakt mit einem Namen und eine Liste der Telefonnummern dar:</span><span class="sxs-lookup"><span data-stu-id="411e5-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="411e5-1291">Ein `List<Contact>` erstellt und wie folgt initialisiert werden:</span><span class="sxs-lookup"><span data-stu-id="411e5-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
<span data-ttu-id="411e5-1292">Das hat dieselbe Wirkung wie das</span><span class="sxs-lookup"><span data-stu-id="411e5-1292">which has the same effect as</span></span>
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
<span data-ttu-id="411e5-1293">wo `__clist`, `__c1` und `__c2` werden temporäre Variablen, die andernfalls nicht sichtbar und kann nicht zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="411e5-1294">Arrayausdrücke-Erstellung</span><span class="sxs-lookup"><span data-stu-id="411e5-1294">Array creation expressions</span></span>

<span data-ttu-id="411e5-1295">Ein *Array_creation_expression* wird verwendet, um eine neue Instanz der Erstellen einer *Array_type*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="411e5-1296">Ein Arrayausdruck für die Erstellung der ersten Form weist eine Arrayinstanz des Typs, das Löschen der einzelnen Ausdrücke aus der Liste mit Ausdrücken ergibt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="411e5-1297">Z. B. die Arrayerstellungsausdruck `new int[10,20]` erzeugt eine Arrayinstanz des Typs `int[,]`, und der Ausdruck zur Arrayerstellung `new int[10][,]` erzeugt ein Array vom Typ `int[][,]`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="411e5-1298">Jeder Ausdruck in der Liste mit Ausdrücken muss vom Typ `int`, `uint`, `long`, oder `ulong`, oder implizit in eine oder mehrere dieser Typen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="411e5-1299">Der Wert jedes Ausdrucks bestimmt die Länge der entsprechenden Dimension in der neu zugewiesenen Arrayinstanz fest.</span><span class="sxs-lookup"><span data-stu-id="411e5-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="411e5-1300">Da die Länge einer Dimension des Arrays nicht negativ sein muss, ist es ein Fehler während der Kompilierung, damit eine *Constant_expression* mit einem negativen Wert in der Liste mit Ausdrücken.</span><span class="sxs-lookup"><span data-stu-id="411e5-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="411e5-1301">Mit Ausnahme der in einem unsicheren Kontext ([nicht sicheren Kontexten](unsafe-code.md#unsafe-contexts)), das Layout des Arrays ist nicht angegeben.</span><span class="sxs-lookup"><span data-stu-id="411e5-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="411e5-1302">Wenn einem Arrayerstellungsausdruck der ersten Form einen Arrayinitialisierer enthält, wird jeder Ausdruck in der Liste mit Ausdrücken muss eine Konstante sein, und der Rang jede Dimension angegebenen Länge durch die Liste der Ausdrücke angegeben, müssen mit denen des Arrayinitialisierers übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="411e5-1303">In einem Arrayerstellungsausdruck der zweite oder dritte Form muss es sich bei der Rang des dem angegebenen Array-Typ oder Rang Bezeichner mit des Arrayinitialisierers übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="411e5-1304">Die Längen der einzelnen Dimension werden von der Anzahl der Elemente in jedem von der entsprechenden Schachtelungsebenen des Arrayinitialisierers hergeleitet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="411e5-1305">Daher wird, ist der Ausdruck</span><span class="sxs-lookup"><span data-stu-id="411e5-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="411e5-1306">genau entspricht.</span><span class="sxs-lookup"><span data-stu-id="411e5-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="411e5-1307">Die dritte Form einem Arrayerstellungsausdruck wird als bezeichnet ein ***implizit typisierten Ausdruck zur Arrayerstellung***.</span><span class="sxs-lookup"><span data-stu-id="411e5-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="411e5-1308">Es ähnelt der zweiten Form, mit dem Unterschied, dass der Elementtyp des Arrays nicht explizit jedoch als der am besten verwendete Typ bestimmt angegeben wird, ([Suchen nach den besten common-Typ, der einen Satz von Ausdrücken](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) des Satzes von Ausdrücken im Array Initialisierer.</span><span class="sxs-lookup"><span data-stu-id="411e5-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="411e5-1309">Für ein mehrdimensionales Array d. h. eine Where der *Rank_specifier* enthält mindestens ein Komma, dieser Satz besteht aus allen *Ausdruck*s finden Sie in geschachtelten *Array_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="411e5-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="411e5-1310">Arrayinitialisierer sind unter [Array Initializers](arrays.md#array-initializers).</span><span class="sxs-lookup"><span data-stu-id="411e5-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="411e5-1311">Das Ergebnis der Auswertung von einem Arrayerstellungsausdruck wird als ein Wert, d. h. einen Verweis auf die neu zugewiesene Arrayinstanz klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="411e5-1312">Die Verarbeitung zur Laufzeit von einem Arrayerstellungsausdruck umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="411e5-1313">Der Dimension Länge von Ausdrücken für die *Expression_list* werden in der Reihenfolge von links nach rechts ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="411e5-1314">Befolgen Sie die Auswertung der einzelnen Ausdrücke, die eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) auf einen der folgenden Typen erfolgt: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="411e5-1315">Der erste Typ in dieser Liste, die für die eine implizite Konvertierung vorhanden ist, wird ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="411e5-1316">Wenn die Auswertung eines Ausdrucks oder einer nachfolgenden impliziten Konvertierung eine Ausnahme auslöst, klicken Sie dann keine weitere Ausdrücke werden ausgewertet, und keine weiteren Schritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="411e5-1317">Die Werte für die Längen der Dimension werden wie folgt überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="411e5-1318">Wenn eine oder mehrere der Werte sind kleiner als 0 (null), eine `System.OverflowException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="411e5-1319">Es wird eine Arrayinstanz mit Längen der angegebenen Dimension zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="411e5-1320">Wenn nicht genügend Arbeitsspeicher verfügbar, um die neue Instanz einer `System.OutOfMemoryException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="411e5-1321">Alle Elemente der neuen Arrayinstanz werden auf ihre Standardwerte initialisiert ([Standardwerte](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="411e5-1322">Wenn der Ausdruck zur Arrayerstellung einen Arrayinitialisierer enthält, wird jeder Ausdruck im Arrayinitialisierer ausgewertet und das entsprechende Arrayelement zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="411e5-1323">Die auswertungen und Zuweisungen erfolgen in der Reihenfolge der Ausdrücke im Arrayinitialisierer geschrieben werden, in anderen Worten: Elemente werden in aufsteigender Indexreihenfolge, mit die Dimension ganz rechts erhöhen zuerst initialisiert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="411e5-1324">Wenn die Auswertung eines angegebenen Ausdrucks oder der nachfolgenden Zuweisung zu dem entsprechenden Array-Element eine Ausnahme verursacht hat, klicken Sie dann keine weiteren Elemente initialisiert werden (und die übrigen Elemente werden daher Standardwerte haben).</span><span class="sxs-lookup"><span data-stu-id="411e5-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="411e5-1325">Ein Ausdruck zur Arrayerstellung ermöglicht die Instanziierung eines Arrays mit Elementen eines Arraytyps, aber die Elemente dieses Arrays müssen manuell initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="411e5-1326">Beispielsweise wird die Anweisung</span><span class="sxs-lookup"><span data-stu-id="411e5-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="411e5-1327">erstellt ein eindimensionales Array mit 100 Elementen des Typs `int[]`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="411e5-1328">Der anfängliche Wert jedes Elements ist `null`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="411e5-1329">Es ist nicht möglich, für die gleiche Arrayerstellungsausdruck Teilarrays aus, und die Anweisung auch instanziieren</span><span class="sxs-lookup"><span data-stu-id="411e5-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="411e5-1330">führt zu einem Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1330">results in a compile-time error.</span></span> <span data-ttu-id="411e5-1331">Die Instanziierung der Teilarrays muss stattdessen manuell, wie im ausgeführt werden</span><span class="sxs-lookup"><span data-stu-id="411e5-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="411e5-1332">Wenn ein Array von Arrays mit eine Form "rechteckige", die d. h. untergeordneten Arrays gleicher Länge sind aufweist, ist es effizienter, ein mehrdimensionales Array verwenden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="411e5-1333">Im obigen Beispiel erstellt die Instanziierung des Arrays von Arrays 101 Objekte – eine äußere Array und 100 Teilarrays.</span><span class="sxs-lookup"><span data-stu-id="411e5-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="411e5-1334">Im Gegensatz dazu sind</span><span class="sxs-lookup"><span data-stu-id="411e5-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="411e5-1335">erstellt nur ein einzelnes Objekt, ein zweidimensionales Array, und die Zuweisung in einer einzelnen Anweisung erreicht.</span><span class="sxs-lookup"><span data-stu-id="411e5-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="411e5-1336">Es folgen Beispiele für Ausdrücke für implizit typisierte Arrays erstellen:</span><span class="sxs-lookup"><span data-stu-id="411e5-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="411e5-1337">Der letzte Ausdruck verursacht einen Kompilierungsfehler, da weder `int` noch `string` implizit in den anderen, und es kommt nicht am besten geben.</span><span class="sxs-lookup"><span data-stu-id="411e5-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="411e5-1338">Ein explizit typisierte Ausdruck zur Arrayerstellung muss verwendet werden in diesem Fall z. B. Angabe des Typs werden `object[]`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="411e5-1339">Eines der Elemente kann auch in einen gemeinsamen Basistyp umgewandelt werden die klicken Sie dann den abgeleitete Elementtyp aufweisen würden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="411e5-1340">Implizit typisiertes Array erstellen Ausdrücke mit anonyme Objektinitialisierer kombiniert werden können ([anonyme Erstellung Objektausdrücke](expressions.md#anonymous-object-creation-expressions)) zum Erstellen anonym typisierte Datenstrukturen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="411e5-1341">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="411e5-1341">For example:</span></span>
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="411e5-1342">Delegieren Sie die Erstellung von Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="411e5-1342">Delegate creation expressions</span></span>

<span data-ttu-id="411e5-1343">Ein *Delegate_creation_expression* wird verwendet, um eine neue Instanz der Erstellen einer *Delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="411e5-1344">Das Argument ein Delegatausdruck für die Erstellung muss eine Methodengruppe, eine anonyme Funktion oder einen Wert, der entweder den Kompilierzeittyp `dynamic` oder *Delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="411e5-1345">Wenn das Argument eine Methodengruppe ist, kennzeichnet es die Methode und eine Instanzmethode, die das Objekt für die Erstellung ein Delegaten.</span><span class="sxs-lookup"><span data-stu-id="411e5-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="411e5-1346">Wenn das Argument eine anonyme Funktion, die sie direkt die Parameter und den Methodentext, der das Delegatziel definiert ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="411e5-1347">Wenn das Argument ein Wert identifiziert eine Delegatinstanz, von dem eine Kopie erstellt ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="411e5-1348">Wenn die *Ausdruck* weist den Typ der Kompilierzeit `dynamic`, *Delegate_creation_expression* dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)), und die folgenden Regeln gelten zur Laufzeit mit dem Laufzeit-Typ, der die *Ausdruck*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="411e5-1349">Andernfalls gelten die Regeln zum Zeitpunkt der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="411e5-1350">Die laufzeitverarbeitung der Bindung einer *Delegate_creation_expression* des Formulars `new D(E)`, wobei `D` ist eine *Delegate_type* und `E` ist ein *Ausdruck* , umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="411e5-1351">Wenn `E` eine Methodengruppe wird der Delegaterstellungsausdruck in die gleiche Weise wie eine Methode gruppenkonvertierung verarbeitet wird ([Gruppe Konvertierungen](conversions.md#method-group-conversions)) von `E` zu `D`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="411e5-1352">Wenn `E` ist eine anonyme Funktion, der Delegaterstellungsausdruck wird verarbeitet, in die gleiche Weise wie eine anonyme Funktion-Konvertierung ([anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions)) von `E` zu `D`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="411e5-1353">Wenn `E` ist ein Wert, `E` kompatibel sein muss ([delegieren Deklarationen](delegates.md#delegate-declarations)) mit `D`, und das Ergebnis ist ein Verweis auf einen neu erstellten Delegaten vom Typ `D` , verweist auf den gleichen Aufruf als Liste `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="411e5-1354">Wenn `E` ist nicht kompatibel mit `D`, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="411e5-1355">Die Verarbeitung zur Laufzeit eine *Delegate_creation_expression* des Formulars `new D(E)`, wobei `D` ist eine *Delegate_type* und `E` ist ein *Ausdruck* , umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="411e5-1356">Wenn `E` eine Methodengruppe wird der Delegaterstellungsausdruck wird als eine Methode gruppenkonvertierung ausgewertet ([Gruppe Konvertierungen](conversions.md#method-group-conversions)) von `E` zu `D`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="411e5-1357">Wenn `E` ist eine anonyme Funktion, die delegaterstellung wird ausgewertet, wie eine anonyme Funktion Konvertierung von `E` zu `D` ([anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="411e5-1358">Wenn `E` ist ein Wert, der eine *Delegate_type*:</span><span class="sxs-lookup"><span data-stu-id="411e5-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="411e5-1359">`E` wird ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1359">`E` is evaluated.</span></span> <span data-ttu-id="411e5-1360">Wenn diese Evaluierungsversion auf eine Ausnahme auslöst, werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="411e5-1361">Wenn der Wert des `E` ist `null`, `System.NullReferenceException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="411e5-1362">Eine neue Instanz des Delegattyps `D` zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="411e5-1363">Wenn nicht genügend Arbeitsspeicher verfügbar, um die neue Instanz einer `System.OutOfMemoryException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="411e5-1364">Die neue Delegatinstanz wird mit der gleichen Aufrufliste als die Delegatinstanz, die vom initialisiert `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="411e5-1365">Die Aufrufliste eines Delegaten wird bestimmt, wenn der Delegat instanziiert wird, und klicken Sie dann für die gesamte Lebensdauer des Delegaten unverändert bleibt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="411e5-1366">Das heißt, ist es nicht möglich, die Ziel um aufrufbare Entitäten eines Delegaten zu ändern, nachdem es erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="411e5-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="411e5-1367">Wenn zwei Delegaten kombiniert werden, oder eine von einem anderen entfernt wird ([delegieren Deklarationen](delegates.md#delegate-declarations)), dazu, dass ein neuer Delegat; keine bestehende Delegat hat seinen Inhalt geändert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="411e5-1368">Es ist nicht möglich, einen Delegaten zu erstellen, der auf eine Eigenschaft, Indexer, benutzerdefinierten Operator, Instanzenkonstruktor, Destruktor oder statischen Konstruktor verweist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="411e5-1369">Wie oben beschrieben, wenn ein Delegat erstellt wird aus einer Methodengruppe, die Liste der formalen Parameter und Rückgabetyp des Delegaten bestimmen, welche der überladenen Methoden auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="411e5-1370">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-1370">In the example</span></span>
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
<span data-ttu-id="411e5-1371">die `A.f` Feld wird initialisiert, indem ein Delegat, der auf den zweiten verweist `Square` Methode, da diese Methode genau mit der Liste formaler Parameter und Rückgabetyp übereinstimmt `DoubleFunc`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="411e5-1372">Haben Sie die zweite `Square` -Methode nicht vorhanden war, ein Fehler während der Kompilierung würde aufgetreten.</span><span class="sxs-lookup"><span data-stu-id="411e5-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="411e5-1373">Anonyme Erstellung Objektausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="411e5-1374">Ein *Anonymous_object_creation_expression* wird verwendet, um ein Objekt eines anonymen Typs zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

<span data-ttu-id="411e5-1375">Ein anonymer Objektinitialisierer deklariert einen anonymen Typ und gibt eine Instanz dieses Typs zurück.</span><span class="sxs-lookup"><span data-stu-id="411e5-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="411e5-1376">Ein anonymer Typ ist ein namenloses Klassentyp, der direkt erbt `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="411e5-1377">Die Member eines anonymen Typs sind, eine Sequenz von schreibgeschützten Eigenschaften, die von der anonymer Objektinitialisierer, die zum Erstellen einer Instanz des Typs abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="411e5-1378">Insbesondere ein anonymer Objektinitialisierer des Formulars</span><span class="sxs-lookup"><span data-stu-id="411e5-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="411e5-1379">deklariert einen anonymen Typ des Formulars</span><span class="sxs-lookup"><span data-stu-id="411e5-1379">declares an anonymous type of the form</span></span>
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
<span data-ttu-id="411e5-1380">in dem jede `Tx` ist der Typ des der entsprechende Ausdruck `ex`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="411e5-1381">Der Ausdruck verwendet eine *Member_declarator* muss einen Typ haben.</span><span class="sxs-lookup"><span data-stu-id="411e5-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="411e5-1382">Daher wird ein Fehler während der Kompilierung für einen Ausdruck in einem *Member_declarator* Null oder eine anonyme Funktion sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="411e5-1383">Es ist auch ein Fehler während der Kompilierung für den Ausdruck zu einen unsicheren Typ haben.</span><span class="sxs-lookup"><span data-stu-id="411e5-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="411e5-1384">Die Namen eines anonymen Typs und des Parameters, der die `Equals` Methode werden automatisch vom Compiler generiert und kann nicht in den Programmtext verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="411e5-1385">Innerhalb desselben Programms erzeugt zwei anonyme Objektinitialisierer, die eine Sequenz von Eigenschaften den gleichen Namen und die während der Kompilierung-Typen in derselben Reihenfolge angeben, dass Instanzen desselben anonymen Typs.</span><span class="sxs-lookup"><span data-stu-id="411e5-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="411e5-1386">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="411e5-1387">die Zuweisung in der letzten Zeile ist zulässig, weil `p1` und `p2` desselben anonymen Typs sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="411e5-1388">Die `Equals` und `GetHashcode` Methoden von anonymen Typen Überschreiben von geerbten Methoden `object`, und werden in Form von definiert die `Equals` und `GetHashcode` -Eigenschaften, so, dass zwei Instanzen desselben anonymen Typs gleich sind. wenn und nur dann, wenn alle Eigenschaften gleich sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="411e5-1389">Ein Member-Declarator kann abgekürzt werden, um einen einfachen Namen ([Typrückschluss](expressions.md#type-inference)), Memberzugriff ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), Basiszugriff ([Zugriffbasieren](expressions.md#base-access)) oder ein Null-bedingte Memberzugriff ([bedingten Null-Ausdrücke als Projektionsinitialisierer](expressions.md#null-conditional-expressions-as-projection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="411e5-1390">Dies wird als bezeichnet ein ***Projektion Initialisierer*** und ist die Kurzform für eine Deklaration und Zuweisung zu einer Eigenschaft mit dem gleichen Namen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="411e5-1391">Insbesondere memberdeklaratoren der Formen</span><span class="sxs-lookup"><span data-stu-id="411e5-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="411e5-1392">sind bzw. genaue Entsprechung der folgenden:</span><span class="sxs-lookup"><span data-stu-id="411e5-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="411e5-1393">Daher in einer Projektion Initialisierung der *Bezeichner* wählt den Wert und an das Feld oder die Eigenschaft, der der Wert zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="411e5-1394">Intuitiv projiziert ein Initialisierer für die Projektion, nicht nur ein Wert, sondern auch den Namen des Werts.</span><span class="sxs-lookup"><span data-stu-id="411e5-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="411e5-1395">Der Typeof-operator</span><span class="sxs-lookup"><span data-stu-id="411e5-1395">The typeof operator</span></span>

<span data-ttu-id="411e5-1396">Die `typeof` Operator wird zum Abrufen der `System.Type` Objekt für einen Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

<span data-ttu-id="411e5-1397">Die erste Form der *Typeof_expression* besteht aus einem `typeof` Schlüsselwort, gefolgt von einer in Klammern gesetzte *Typ*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="411e5-1398">Das Ergebnis eines Ausdrucks, der diese Form ist die `System.Type` -Objekt für den angegebenen Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="411e5-1399">Es gibt nur ein `System.Type` Objekt für jeden angegebenen Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="411e5-1400">Dies bedeutet, dass für einen Typ `T`, `typeof(T) == typeof(T)` ist immer "true".</span><span class="sxs-lookup"><span data-stu-id="411e5-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="411e5-1401">Die *Typ* nicht `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="411e5-1402">Die zweite Form der *Typeof_expression* besteht aus einem `typeof` Schlüsselwort, gefolgt von einer in Klammern gesetzte *Unbound_type_name*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="411e5-1403">Ein *Unbound_type_name* ähnelt sehr einer *Type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)) mit dem Unterschied, dass ein *Unbound_type_name* enthält *Generic_dimension_specifier*s, in denen eine *Type_name* enthält *Type_argument_list*s.</span><span class="sxs-lookup"><span data-stu-id="411e5-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="411e5-1404">Wenn der Operand des eine *Typeof_expression* ist eine Folge von Token, das die Grammatiken beider erfüllt *Unbound_type_name* und *Type_name*, es nämlich Wenn enthält weder ein *Generic_dimension_specifier* noch ein *Type_argument_list*, die Abfolge der Token gilt ein *Type_name*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="411e5-1405">Die Bedeutung des einen *Unbound_type_name* wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="411e5-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="411e5-1406">Konvertieren Sie die Sequenz von Token, die eine *Type_name* durch Ersetzen jedes *Generic_dimension_specifier* mit einem *Type_argument_list* müssen die gleiche Anzahl von Kommas und die Schlüsselwort `object` während jedes *Type_argument*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="411e5-1407">Bewerten Sie die resultierende *Type_name*, und ignoriert alle Einschränkungen für Typparameter.</span><span class="sxs-lookup"><span data-stu-id="411e5-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="411e5-1408">Die *Unbound_type_name* aufgelöst, die für den ungebundenen generischen Typ, der den resultierenden konstruierten Typ zugeordnet wird ([gebunden und Typen von nicht gebundenen](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="411e5-1409">Das Ergebnis der *Typeof_expression* ist die `System.Type` -Objekt für generische Typ der resultierenden aufgehoben werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="411e5-1410">Die dritte Form der *Typeof_expression* besteht aus einem `typeof` Schlüsselwort, gefolgt von einer in Klammern gesetzte `void` Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="411e5-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="411e5-1411">Das Ergebnis eines Ausdrucks, der diese Form ist die `System.Type` -Objekt, das Fehlen eines Typs darstellt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="411e5-1412">Das Typobjekt, das von zurückgegebene `typeof(void)` unterscheidet sich von das Typobjekt für den beliebigen Typs zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="411e5-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="411e5-1413">Dieses besondere Art-Objekt ist nützlich in Klassenbibliotheken, mit denen Reflektion für Methoden in der Sprache, in dem diese Methoden haben eine Methode zur Darstellung des Rückgabetyp einer Methode, einschließlich der void-Methoden, mit einer Instanz von möchten `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="411e5-1414">Die `typeof` -Operator kann für einen Typparameter verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="411e5-1415">Das Ergebnis ist die `System.Type` -Objekt für den Laufzeittyp, der dem Typparameter gebunden war.</span><span class="sxs-lookup"><span data-stu-id="411e5-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="411e5-1416">Die `typeof` Operator kann auch auf einen konstruierten Typ oder einen ungebundenen generischen Typ verwendet werden ([gebunden und Typen von nicht gebundenen](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="411e5-1417">Die `System.Type` Objekt für ein ungebundener generischer Typ ist nicht identisch mit der `System.Type` Objekt des Instanztyps.</span><span class="sxs-lookup"><span data-stu-id="411e5-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="411e5-1418">Der Instanztyp ist also immer ein geschlossener konstruierter Typ zur Laufzeit die `System.Type` Objekt abhängig ist für die Laufzeit-Typargumente verwendet werden, während die ungebundenen generischen Typs keine Typargumente aufweist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="411e5-1419">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-1419">The example</span></span>
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
<span data-ttu-id="411e5-1420">erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="411e5-1420">produces the following output:</span></span>
```
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

<span data-ttu-id="411e5-1421">Beachten Sie, dass `int` und `System.Int32` denselben Typ aufweisen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="411e5-1422">Beachten Sie, dass das Ergebnis des `typeof(X<>)` hängt nicht das Typargument, aber das Ergebnis des `typeof(X<T>)` ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="411e5-1423">Checked- und Unchecked-Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="411e5-1424">Die `checked` und `unchecked` Operatoren werden verwendet, um zu steuern die ***Kontext der überlaufprüfung*** für arithmetische Operationen für ganzzahlige Typen und Konvertierungen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="411e5-1425">Die `checked` -Operator wertet den enthaltenen Ausdruck in einem überprüften Kontext und die `unchecked` -Operator wertet die enthaltenen Ausdruck in einem ungeprüften Kontext.</span><span class="sxs-lookup"><span data-stu-id="411e5-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="411e5-1426">Ein *Checked_expression* oder *Unchecked_expression* entspricht genau einer *Parenthesized_expression* ([Ausdrücke in Klammern](expressions.md#parenthesized-expressions)), außer dass der enthaltene Ausdruck in der angegebenen Kontext der überlaufprüfung ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="411e5-1427">Kontext der überlaufprüfung kann auch über gesteuert werden die `checked` und `unchecked` Anweisungen ([die checked und unchecked Anweisungen](statements.md#the-checked-and-unchecked-statements)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="411e5-1428">Die folgenden Vorgänge werden von hergestellt, indem Kontext der überlaufprüfung beeinflusst die `checked` und `unchecked` Operatoren und Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="411e5-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="411e5-1429">Die vordefinierten `++` und `--` unäre Operatoren ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators) und [Präfix-Inkrement und Dekrement-Operatoren](expressions.md#prefix-increment-and-decrement-operators)), wenn der Operand ein ganzzahliger ist Geben Sie ein.</span><span class="sxs-lookup"><span data-stu-id="411e5-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="411e5-1430">Die vordefinierten `-` unäroperator ([unäre Operator minus](expressions.md#unary-minus-operator)), wenn der Operand ein ganzzahliger Typ ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="411e5-1431">Die vordefinierten `+`, `-`, `*`, und `/` binäre Operatoren ([arithmetische Operatoren](expressions.md#arithmetic-operators)), wenn beide Operanden Ganzzahltypen sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="411e5-1432">Explizite numerische Konvertierungen ([explizite numerische Konvertierungen](conversions.md#explicit-numeric-conversions)) von einem ganzzahligen Typ in einen anderen ganzzahligen Typ oder von `float` oder `double` in einen ganzzahligen Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="411e5-1433">Wenn eine der oben genannten Vorgänge erzeugen kann ein Ergebnis, das ist zu groß, um die Darstellung in den Zieltyp, der den Kontext, in dem der Vorgang ausgeführt Steuerelemente ist, des resultierenden Verhaltens:</span><span class="sxs-lookup"><span data-stu-id="411e5-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="411e5-1434">In einem `checked` Kontext, wenn der Vorgang ein konstanter Ausdruck ist ([Konstante Ausdrücke](expressions.md#constant-expressions)), ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="411e5-1435">Wenn der Vorgang zur Laufzeit, ausgeführt wird, andernfalls eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="411e5-1436">In einer `unchecked` Kontext ist das Ergebnis abgeschnitten, indem alle höherwertigen Bits, die nicht in den Zieltyp passen verworfen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="411e5-1437">Für nicht Konstante Ausdrücke (Expressions, die zur Laufzeit ausgewertet werden), die von einer nicht eingeschlossen sind `checked` oder `unchecked` Operatoren oder -Anweisungen, wird der Kontext der standardmäßigen überlaufprüfung `unchecked` , wenn die externen (z. B. Compiler Faktoren Switches und Konfiguration der ausführungsumgebung) rufen Sie für `checked` Auswertung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="411e5-1438">Für Konstante Ausdrücke (Expressions, die zum Zeitpunkt der Kompilierung vollständig ausgewertet werden können), ist der Standard-überlaufprüfung-Kontext immer `checked`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="411e5-1439">Es sei denn, Sie befindet sich ein konstanter Ausdruck explizit in ein `unchecked` Kontext Überläufe, die auftreten, während der Kompilierung Auswertung des Ausdrucks immer Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="411e5-1440">Der Text einer anonymen Funktion ist nicht betroffen von `checked` oder `unchecked` Kontexte, in dem die anonyme Funktion auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="411e5-1441">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-1441">In the example</span></span>
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
<span data-ttu-id="411e5-1442">keine Kompilierungsfehler werden gemeldet, da keiner der Ausdrücke zur Kompilierzeit ausgewertet werden kann.</span><span class="sxs-lookup"><span data-stu-id="411e5-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="411e5-1443">Zur Laufzeit die `F` -Methode löst eine `System.OverflowException`, und die `G` Methodenrückgabe-727379968 (die unteren 32 Bits des Ergebnisses wird außerhalb des gültigen Bereichs).</span><span class="sxs-lookup"><span data-stu-id="411e5-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="411e5-1444">Das Verhalten der `H` Methode hängt von der Standard-überlaufprüfung-Kontext für die Kompilierung, aber es ist entweder dasselbe `F` oder gleich `G`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="411e5-1445">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-1445">In the example</span></span>
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
<span data-ttu-id="411e5-1446">Beim Auswerten der Konstanten Ausdrücke in auftretenden `F` und `H` verursachen Kompilierzeitfehler gemeldet werden, da die Ausdrücke, in ausgewertet werden einem `checked` Kontext.</span><span class="sxs-lookup"><span data-stu-id="411e5-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="411e5-1447">Ein Überlauf tritt auch auf, wenn es sich bei der Auswertung des konstanten Ausdrucks in `G`, aber da die Auswertung in erfolgt einem `unchecked` Kontext, der Überlauf wird nicht gemeldet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="411e5-1448">Die `checked` und `unchecked` Operatoren wirken sich nur auf der überlaufprüfung Kontext für diese Vorgänge, die textlich innerhalb der "`(`"und"`)`" Token.</span><span class="sxs-lookup"><span data-stu-id="411e5-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="411e5-1449">Die Operatoren haben keine Auswirkungen auf die Funktionsmember, die aufgerufen werden, als Ergebnis der Auswertung des Ausdrucks enthalten.</span><span class="sxs-lookup"><span data-stu-id="411e5-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="411e5-1450">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-1450">In the example</span></span>
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
<span data-ttu-id="411e5-1451">die Verwendung von `checked` in `F` wirkt sich nicht auf die Auswertung von `x * y` in `Multiply`, sodass `x * y` wird in der Standardeinstellung überlaufprüfung-Kontext ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="411e5-1452">Die `unchecked` Operator eignet sich, beim Schreiben von Konstanten der ganzzahligen Typen mit Vorzeichen in hexadezimaler Schreibweise.</span><span class="sxs-lookup"><span data-stu-id="411e5-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="411e5-1453">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="411e5-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="411e5-1454">Sowohl der oben genannten hexadezimalen Konstanten sind vom Typ `uint`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="411e5-1455">Da sind die Konstanten außerhalb der `int` liegen, ohne die `unchecked` Operator, die Umwandlungen in `int` Fehler während der Kompilierung erzeugt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="411e5-1456">Die `checked` und `unchecked` Operatoren und Anweisungen ermöglichen es Programmierern, um bestimmte Aspekte der einige numerischen Berechnungen zu steuern.</span><span class="sxs-lookup"><span data-stu-id="411e5-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="411e5-1457">Das Verhalten einiger numerischen Operatoren hängt jedoch von ihrer Operanden des Bereichsoperators-Datentypen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="411e5-1458">Z. B. immer Multiplikation von zwei Dezimalstellen löst eine Ausnahme bei einem Überlauf sogar innerhalb einer explizit `unchecked` zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="411e5-1459">Auf ähnliche Weise Multiplizieren zweier gleitet nie Ergebnisse zu einer Ausnahme bei einem Überlauf sogar innerhalb einer explizit `checked` zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="411e5-1460">Andere Operatoren sind außerdem nicht betroffen, durch den Modus der Überprüfung, ob der Standard- oder explizit.</span><span class="sxs-lookup"><span data-stu-id="411e5-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="411e5-1461">Ausdrücke mit Standardwert</span><span class="sxs-lookup"><span data-stu-id="411e5-1461">Default value expressions</span></span>

<span data-ttu-id="411e5-1462">Ein Standardwertausdruck verwendet, um den Standardwert abzurufen ([Standardwerte](variables.md#default-values)) eines Typs.</span><span class="sxs-lookup"><span data-stu-id="411e5-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="411e5-1463">Ein Standardwertausdruck wird normalerweise für Typparameter verwendet werden, da nicht bekannt sein kann, wenn der Typparameter einen Werttyp oder ein Verweistyp ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="411e5-1464">(Keine Konvertierung vorhanden ist, aus der `null` Zeichenfolgenliteral auf einen Typparameter, wenn der Typparameter bekannt ist, dass ein Verweistyp sein.)</span><span class="sxs-lookup"><span data-stu-id="411e5-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="411e5-1465">Wenn die *Typ* in einem *Default_value_expression* wertet zur Laufzeit in einen Verweistyp, der das Ergebnis ist `null` auf diesen Typ konvertiert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="411e5-1466">Wenn die *Typ* in eine *Default_value_expression* wertet zur Laufzeit in einen Werttyp, der das Ergebnis ist der *Value_type*des default-Wert ([Standard Konstruktoren](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="411e5-1467">Ein *Default_value_expression* ist ein konstanter Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)) Wenn der Typ ist ein Verweistyp oder ein Typparameter, der bekannt ist, dass ein Verweistyp sein ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="411e5-1468">Darüber hinaus eine *Default_value_expression* ist ein konstanter Ausdruck, wenn der Typ eine der folgenden Werttypen: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, oder ein Enumerationstyp.</span><span class="sxs-lookup"><span data-stu-id="411e5-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="411e5-1469">Nameof-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-1469">Nameof expressions</span></span>

<span data-ttu-id="411e5-1470">Ein *Nameof_expression* verwendet, um den Namen des eine Programmentität als eine Konstante Zeichenfolge abzurufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

<span data-ttu-id="411e5-1471">Grammatisch gesehen die *Named_entity* Operand ist immer ein Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="411e5-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="411e5-1472">Da `nameof` ist kein reserviertes Schlüsselwort, ein Nameof-Ausdruck ist immer mit einem Aufruf der der einfache Name syntaktisch mehrdeutig `nameof`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="411e5-1473">Aus Kompatibilitätsgründen, wenn einer Namenssuche ([einfache Namen](expressions.md#simple-names)) mit dem Namen `nameof` erfolgreich ist, wird der Ausdruck als behandelt eine *Invocation_expression* – unabhängig davon, ob der Aufruf ist Rechtlich.</span><span class="sxs-lookup"><span data-stu-id="411e5-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="411e5-1474">Andernfalls ist es eine *Nameof_expression*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="411e5-1475">Die Bedeutung von der *Named_entity* von eine *Nameof_expression* ändert sich die Bedeutung des Zertifikats als ein Ausdruck, also entweder als eine *Simple_name*, *Base_access*  oder *Member_access*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="411e5-1476">Jedoch, in dem die Suche in beschrieben [einfache Namen](expressions.md#simple-names) und [Memberzugriff](expressions.md#member-access) führt zu einem Fehler, da ein Instanzmember in einem statischen Kontext gefunden wurde ein *Nameof_expression*keine derartigen Fehler erzeugt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="411e5-1477">Es ist ein Fehler während der Kompilierung für eine *Named_entity* Festlegen einer Methodengruppe haben eine *Type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="411e5-1478">Es ist ein Kompilierzeitfehler für eine *Named_entity_target* der Typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="411e5-1479">Ein *Nameof_expression* ist ein konstanter Ausdruck des Typs `string`, und hat keine Auswirkungen zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="411e5-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="411e5-1480">Insbesondere die *Named_entity* wird nicht ausgewertet, und wird ignoriert, für die Zwecke der Untersuchung der Zuweisung ([allgemeinen Regeln für einfache Ausdrücke](variables.md#general-rules-for-simple-expressions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="411e5-1481">Der Wert ist der letzte Bezeichner, der die *Named_entity* vor dem letzten optionalen *Type_argument_list*, transformierten auf folgende Weise:</span><span class="sxs-lookup"><span data-stu-id="411e5-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="411e5-1482">Das Präfix "`@`", wenn verwendet, wird entfernt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="411e5-1483">Jede *Unicode_escape_sequence* wird umgewandelt in das entsprechende Unicode-Zeichen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="411e5-1484">Alle *Formatting_characters* werden entfernt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="411e5-1485">Hierbei handelt es sich um die gleichen Transformationen angewendet [Bezeichner](lexical-structure.md#identifiers) beim Testen der Gleichheit zwischen Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="411e5-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="411e5-1486">TODO: Beispiele</span><span class="sxs-lookup"><span data-stu-id="411e5-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="411e5-1487">Anonyme Methode Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-1487">Anonymous method expressions</span></span>

<span data-ttu-id="411e5-1488">Ein *Anonymous_method_expression* ist eines der zwei Möglichkeiten, eine anonyme Funktion definieren.</span><span class="sxs-lookup"><span data-stu-id="411e5-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="411e5-1489">Diese werden weiter beschrieben [anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions).</span><span class="sxs-lookup"><span data-stu-id="411e5-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="411e5-1490">Unäre Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-1490">Unary operators</span></span>

<span data-ttu-id="411e5-1491">Die `?`, `+`, `-`, `!`, `~`, `++`, `--`, umgewandelt, und `await` Operatoren werden als die Unäroperatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

<span data-ttu-id="411e5-1492">Wenn der Operand ein *Unary_expression* weist den Typ der Kompilierzeit `dynamic`, dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="411e5-1493">In diesem Fall geben Sie die während der Kompilierung von der *Unary_expression* ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit mit der Run-Time-Typ des Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="411e5-1494">NULL-bedingten operator</span><span class="sxs-lookup"><span data-stu-id="411e5-1494">Null-conditional operator</span></span>

<span data-ttu-id="411e5-1495">Der nullbedingungsoperator gilt Operanden eine Liste der Vorgänge nur, wenn dieser Operand ungleich Null ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="411e5-1496">Das Ergebnis der Anwendung des Operators ist andernfalls `null`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1496">Otherwise the result of applying the operator is `null`.</span></span>

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="411e5-1497">Memberzugriff und Element-Zugriffsvorgänge (der selbst bedingten Null-sein können) als auch aufrufen, kann die Liste der Vorgänge enthalten.</span><span class="sxs-lookup"><span data-stu-id="411e5-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="411e5-1498">Beispiel: der Ausdruck `a.b?[0]?.c()` ist eine *Null_conditional_expression* mit einer *Primary_expression* `a.b` und *Null_conditional_operations* `?[0]` (bedingte Null-Elementzugriff), `?.c` (bedingte Null-Memberzugriff) und `()` (Aufruf).</span><span class="sxs-lookup"><span data-stu-id="411e5-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="411e5-1499">Für eine *Null_conditional_expression* `E` mit einem *Primary_expression* `P`ermöglichen `E0` des Ausdrucks durch entfernen das vorangestellte textlichabgerufenwerden`?`von jedem der *Null_conditional_operations* von `E` , die über eines verfügen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="411e5-1500">Im Prinzip `E0` ist der Ausdruck, der ausgewertet wird, wenn keines der null-Überprüfungen durch dargestellt die `?`s finde einen `null`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="411e5-1501">Darüber hinaus können `E1` des Ausdrucks durch Entfernen Sie die führende textlich abgerufen werden `?` aus nur die zuerst die *Null_conditional_operations* in `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="411e5-1502">Kann dies zu einem *primären Ausdruck* (wenn gab es nur eine `?`) oder einem anderen *Null_conditional_expression*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="411e5-1503">Z. B. wenn `E` ist der Ausdruck `a.b?[0]?.c()`, klicken Sie dann `E0` ist der Ausdruck `a.b[0].c()` und `E1` ist der Ausdruck `a.b[0]?.c()`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="411e5-1504">Wenn `E0` wird dann als nichts, klassifiziert `E` wird als "nothing" klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="411e5-1505">Andernfalls wird E als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="411e5-1506">`E0` und `E1` werden verwendet, um zu bestimmen, die Bedeutung der `E`:</span><span class="sxs-lookup"><span data-stu-id="411e5-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="411e5-1507">Wenn `E` tritt ein, wenn eine *Statement_expression* die Bedeutung der `E` ist identisch mit der Anweisung</span><span class="sxs-lookup"><span data-stu-id="411e5-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="411e5-1508">mit dem Unterschied, dass P nur einmal ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="411e5-1509">Andernfalls gilt: Wenn `E0` wird klassifiziert als "nothing" ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="411e5-1510">Andernfalls können `T0` den Typ der `E0`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="411e5-1511">Wenn `T0` ist ein Typparameter, die ein Verweistyp oder ein NULL-Werte werden tritt ein Fehler während der Kompilierung nicht bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="411e5-1512">Wenn `T0` ist ein NULL-Werte, und klicken Sie dann auf den Typ des `E` ist `T0?`, und die Bedeutung der `E` ist identisch mit</span><span class="sxs-lookup"><span data-stu-id="411e5-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="411e5-1513">mit dem Unterschied, dass `P` wird nur einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="411e5-1514">Andernfalls ist des Typs von E T0, und die Bedeutung von E ist identisch mit</span><span class="sxs-lookup"><span data-stu-id="411e5-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="411e5-1515">mit dem Unterschied, dass `P` wird nur einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="411e5-1516">Wenn `E1` ist selbst ein *Null_conditional_expression*, klicken Sie dann diese Regeln werden angewendet, in diesem Fall die Tests für die Schachtelung `null` bis keine weiteren `?`des, und der Ausdruck wurde reduziert ganz nach unten auf dem primären Ausdruck `E0`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="411e5-1517">Z. B. wenn der Ausdruck `a.b?[0]?.c()` tritt ein, wenn eine Anweisung-Ausdruck, wie die Anweisung:</span><span class="sxs-lookup"><span data-stu-id="411e5-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="411e5-1518">die Bedeutung ist äquivalent zu:</span><span class="sxs-lookup"><span data-stu-id="411e5-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="411e5-1519">Dies entspricht erneut dem:</span><span class="sxs-lookup"><span data-stu-id="411e5-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="411e5-1520">Mit dem Unterschied, dass `a.b` und `a.b[0]` werden nur einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="411e5-1521">Wenn sie in einem Kontext tritt auf, in dem sein Wert, wie in verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="411e5-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="411e5-1522">und vorausgesetzt, dass der Typ des letzten Aufrufs kein Typ NULL-Werte ist, entspricht die Bedeutung:</span><span class="sxs-lookup"><span data-stu-id="411e5-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="411e5-1523">Mit dem Unterschied, dass `a.b` und `a.b[0]` werden nur einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="411e5-1524">NULL-bedingte Ausdrücke als Projektionsinitialisierer</span><span class="sxs-lookup"><span data-stu-id="411e5-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="411e5-1525">Ein Null-bedingten Ausdruck ist nur zulässig, wie eine *Member_declarator* in einer *Anonymous_object_creation_expression* ([anonyme Erstellung Objektausdrücke](expressions.md#anonymous-object-creation-expressions)) Wenn Es endet mit ein (optional auch nullbedingte) Memberzugriff.</span><span class="sxs-lookup"><span data-stu-id="411e5-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="411e5-1526">Grammatisch, kann diese Anforderung ausgedrückt werden:</span><span class="sxs-lookup"><span data-stu-id="411e5-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="411e5-1527">Dies ist ein Sonderfall der Grammatik für *Null_conditional_expression* oben.</span><span class="sxs-lookup"><span data-stu-id="411e5-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="411e5-1528">Die Produktion *Member_declarator* in [anonyme Erstellung Objektausdrücke](expressions.md#anonymous-object-creation-expressions) dann schließt nur *Null_conditional_member_access*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="411e5-1529">NULL-bedingte Ausdrücke als Anweisungsausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="411e5-1530">Ein Null-bedingten Ausdruck ist nur zulässig, als eine *Statement_expression* ([ausdrucksanweisungen](statements.md#expression-statements)) Wenn sie mit einem Aufruf beendet wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="411e5-1531">Grammatisch, kann diese Anforderung ausgedrückt werden:</span><span class="sxs-lookup"><span data-stu-id="411e5-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="411e5-1532">Dies ist ein Sonderfall der Grammatik für *Null_conditional_expression* oben.</span><span class="sxs-lookup"><span data-stu-id="411e5-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="411e5-1533">Die Produktion *Statement_expression* in [ausdrucksanweisungen](statements.md#expression-statements) dann schließt nur *Null_conditional_invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="411e5-1534">Unärer Plus-Operator</span><span class="sxs-lookup"><span data-stu-id="411e5-1534">Unary plus operator</span></span>

<span data-ttu-id="411e5-1535">Für einen Vorgang des Formulars `+x`, Auflösen der Überladung unäroperator ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-1536">Der Operand wird in der Parametertyp des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="411e5-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="411e5-1537">Die vordefinierten unären plus -Operatoren sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="411e5-1538">Für die einzelnen Operatoren ist das Ergebnis einfach der Wert des Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="411e5-1539">Unärer Minusoperator</span><span class="sxs-lookup"><span data-stu-id="411e5-1539">Unary minus operator</span></span>

<span data-ttu-id="411e5-1540">Für einen Vorgang des Formulars `-x`, Auflösen der Überladung unäroperator ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-1541">Der Operand wird in der Parametertyp des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="411e5-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="411e5-1542">Die vordefinierten Negationsoperatoren sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="411e5-1543">Ganze Zahl Negation:</span><span class="sxs-lookup"><span data-stu-id="411e5-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="411e5-1544">Das Ergebnis wird durch das Subtrahieren berechnet `x` 0 (null).</span><span class="sxs-lookup"><span data-stu-id="411e5-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="411e5-1545">Wenn der Wert des `x` ist die kleinste darstellbare Wert des Typs Operand (-2 ^ 31 für `int` oder-2 ^ 63 für `long`), klicken Sie dann die mathematische Negation des `x` ist nicht in den Operandentyp dargestellt werden kann.</span><span class="sxs-lookup"><span data-stu-id="411e5-1545">If the value of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="411e5-1546">Wenn dies in erfolgt eine `checked` Kontext eine `System.OverflowException` ausgelöst; wenn es erfolgt eine `unchecked` Kontext das Ergebnis ist der Wert des Operanden und der Überlauf wird nicht gemeldet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="411e5-1547">Wenn der Operand der Negationsoperator vom Typ `uint`, es ist in den Typ konvertiert `long`, und der Typ des Ergebnisses ist `long`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="411e5-1548">Eine Ausnahme ist die Regel, die `int` Wert von – 2147483648 (-2 ^ 31) als ein Ganzzahlliteral im Dezimalformat geschrieben werden ([Integer-Literale](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="411e5-1549">Wenn der Operand der Negationsoperator vom Typ `ulong`, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="411e5-1550">Eine Ausnahme ist die Regel, die `long` Wert-9223372036854775808 (-2 ^ 63) als ein Ganzzahlliteral im Dezimalformat geschrieben werden ([Integer-Literale](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="411e5-1551">Gleitkomma Negation:</span><span class="sxs-lookup"><span data-stu-id="411e5-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="411e5-1552">Das Ergebnis ist der Wert des `x` mit umgekehrtem Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="411e5-1553">Wenn `x` NaN ist, ist das Ergebnis ist ebenfalls NaN.</span><span class="sxs-lookup"><span data-stu-id="411e5-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="411e5-1554">Decimal Negation:</span><span class="sxs-lookup"><span data-stu-id="411e5-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="411e5-1555">Das Ergebnis wird durch das Subtrahieren berechnet `x` 0 (null).</span><span class="sxs-lookup"><span data-stu-id="411e5-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="411e5-1556">Decimal Negation ist äquivalent zur Verwendung der unäre Minusoperator des Typs `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="411e5-1557">Logischer Negationsoperator</span><span class="sxs-lookup"><span data-stu-id="411e5-1557">Logical negation operator</span></span>

<span data-ttu-id="411e5-1558">Für einen Vorgang des Formulars `!x`, Auflösen der Überladung unäroperator ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-1559">Der Operand wird in der Parametertyp des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="411e5-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="411e5-1560">Nur eine vordefinierte Logischer Negationsoperator vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="411e5-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="411e5-1561">Dieser Operator wird die logische Negation des Operanden berechnet: Wenn der Operand ist `true`, das Ergebnis ist `false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="411e5-1562">Wenn der Operand ist `false`, das Ergebnis ist `true`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="411e5-1563">Bitweiser Komplementoperator</span><span class="sxs-lookup"><span data-stu-id="411e5-1563">Bitwise complement operator</span></span>

<span data-ttu-id="411e5-1564">Für einen Vorgang des Formulars `~x`, Auflösen der Überladung unäroperator ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-1565">Der Operand wird in der Parametertyp des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="411e5-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="411e5-1566">Die vordefinierten bitweiser Komplementoperatoren sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="411e5-1567">Für die einzelnen Operatoren, ist das Ergebnis des Vorgangs das bitweise Komplement des `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="411e5-1568">Jeder Enumerationstyp `E` implizit bietet die folgenden bitweiser Komplementoperator:</span><span class="sxs-lookup"><span data-stu-id="411e5-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="411e5-1569">Das Ergebnis der Auswertung `~x`, wobei `x` ist ein Ausdruck eines Enumerationstyps `E` mit einem zugrunde liegenden Typ `U`, entspricht genau dem Auswerten von `(E)(~(U)x)`, außer dass die Konvertierung in `E` ist als immer ausgeführt. sofern es sich um ein `unchecked` Kontext ([checked und unchecked Operatoren](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="411e5-1570">Präfix-Inkrementoperator und Präfix-Dekrementoperator</span><span class="sxs-lookup"><span data-stu-id="411e5-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="411e5-1571">Der Operand eines Präfix Inkrement oder Dekrement Operation muss ein Ausdruck, der als eine Variable, einen Eigenschaftenzugriff oder Indexzugriff klassifiziert sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="411e5-1572">Das Ergebnis des Vorgangs ist ein Wert des gleichen Typs als Operand.</span><span class="sxs-lookup"><span data-stu-id="411e5-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="411e5-1573">Wenn der Operand eines Präfixes erhöhen oder dekrementvorgang ist eine Eigenschaft oder Zugriffs auf Indexer, Eigenschaft oder des Indexers benötigen Sie Folgendes ein `get` und `set` Accessor.</span><span class="sxs-lookup"><span data-stu-id="411e5-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="411e5-1574">Wenn dies nicht der Fall ist, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="411e5-1575">Unäroperator der überladungsauflösung ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-1576">Vordefinierte `++` und `--` Operatoren vorhanden, für die folgenden Typen: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, und alle Enumerationstypen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="411e5-1577">Die vordefinierten `++` Operatoren zurückgegeben, den von den Operanden und den vordefinierten 1 hinzugefügt erzeugten Wert `--` Operatoren geben einen Wert durch Subtrahieren der Operand 1 zurück.</span><span class="sxs-lookup"><span data-stu-id="411e5-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="411e5-1578">In einem `checked` Kontext, wenn das Ergebnis dieser Addition oder Subtraktion außerhalb des Bereichs des Ergebnistyps ist und der Ergebnistyp ein ganzzahliger Typ oder ein Enumerationstyp ist, eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="411e5-1579">Die Verarbeitung zur Laufzeit eine "Präfix"-Inkrementoperator oder dekrementvorgang des Formulars `++x` oder `--x` umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="411e5-1580">Wenn `x` wird als Variable klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="411e5-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="411e5-1581">`x` wird ausgewertet, um die Variable zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="411e5-1582">Der ausgewählte Operator wird aufgerufen, mit dem Wert des `x` als Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="411e5-1583">Der Wert, der vom Operator zurückgegebenen befindet sich in den Speicherort, durch die Auswertung von `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="411e5-1584">Der Wert, der vom Operator zurückgegeben wird das Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="411e5-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="411e5-1585">Wenn `x` wird als eine Eigenschaft oder der Indexer Zugriff klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="411e5-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="411e5-1586">Der Instanzausdruck (Wenn `x` ist nicht `static`) und die Argumentliste (Wenn `x` Indexzugriff ist) zugeordneten `x` werden ausgewertet, und die Ergebnisse werden in der nachfolgenden verwendet `get` und `set` Accessor-Aufrufe.</span><span class="sxs-lookup"><span data-stu-id="411e5-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="411e5-1587">Die `get` Accessor `x` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="411e5-1588">Der ausgewählte Operator wird aufgerufen, mit den Rückgabewert von der `get` Accessor als Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="411e5-1589">Die `set` Accessor `x` wird aufgerufen, mit dem Wert, der zurückgegeben wird, durch den Operator als seine `value` Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="411e5-1590">Der Wert, der vom Operator zurückgegeben wird das Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="411e5-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="411e5-1591">Die `++` und `--` Operatoren unterstützt auch Postfix Notation ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="411e5-1592">In der Regel das Ergebnis des `x++` oder `x--` ist der Wert des `x` vor dem Vorgang, während das Ergebnis des `++x` oder `--x` ist der Wert des `x` nach Abschluss des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="411e5-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="411e5-1593">In beiden Fällen `x` selbst hat den gleichen Wert nach Abschluss des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="411e5-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="411e5-1594">Ein `operator++` oder `operator--` Implementierung kann mithilfe der Präfix oder Postfix-Notation aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="411e5-1595">Es ist nicht möglich, separate Implementierungen für die beiden Notationen zu haben.</span><span class="sxs-lookup"><span data-stu-id="411e5-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="411e5-1596">CAST-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-1596">Cast expressions</span></span>

<span data-ttu-id="411e5-1597">Ein *Cast_expression* wird verwendet, um einen Ausdruck explizit in einen angegebenen Typ zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="411e5-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="411e5-1598">Ein *Cast_expression* des Formulars `(T)E`, wobei `T` ist eine *Typ* und `E` ist eine *Unary_expression*, führt eine explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-conversions)) des Werts der `E` eingeben `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="411e5-1599">Wenn keine explizite Konvertierung von vorhanden ist `E` zu `T`, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="411e5-1600">Andernfalls ist das Ergebnis der durch die explizite Konvertierung erzeugte Wert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="411e5-1601">Das Ergebnis wird immer als Wert klassifiziert, wenn `E` steht für eine Variable.</span><span class="sxs-lookup"><span data-stu-id="411e5-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="411e5-1602">Die Grammatik für eine *Cast_expression* führt zu einer bestimmten syntaktischen Mehrdeutigkeiten.</span><span class="sxs-lookup"><span data-stu-id="411e5-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="411e5-1603">Beispielsweise der Ausdruck `(x)-y` entweder als interpretiert werden konnte eine *Cast_expression* (eine Umwandlung von `-y` eingeben `x`) oder als ein *Additive_expression* in Kombination mit einer *Parenthesized_expression* (die den Wert berechnet `x - y)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="411e5-1604">Auflösen *Cast_expression* Mehrdeutigkeiten, die folgende Regel vorhanden ist: Eine Sequenz einer oder mehreren *token*s ([Leerraum](lexical-structure.md#white-space)) eingeschlossen in Klammern wird als Anfang betrachtet eine *Cast_expression* nur dann, wenn mindestens eine der folgenden erfüllt sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="411e5-1605">Die Sequenz von Token ist richtig Grammatik für eine *Typ*, aber nicht für eine *Ausdruck*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="411e5-1606">Die Sequenz von Token ist richtig Grammatik für eine *Typ*, und das Token direkt nach der schließenden Klammer des Tokens "`~`", das Token "`!`", das Token "`(`", wird eine  *Bezeichner* ([Unicode-Escape-Zeichensequenzen](lexical-structure.md#unicode-character-escape-sequences)), ein *literal* ([Literale](lexical-structure.md#literals)), oder ein beliebiges *Schlüsselwort*([Schlüsselwörter](lexical-structure.md#keywords)) mit Ausnahme von `as` und `is`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="411e5-1607">Der Begriff "korrekte Grammatik" bedeutet, dass nur, die, die die Folge von Token, die bestimmten grammatische Produktion entsprechen muss.</span><span class="sxs-lookup"><span data-stu-id="411e5-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="411e5-1608">Dabei wird insbesondere die eigentliche Bedeutung der einzelnen Bezeichner nicht berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="411e5-1609">Z. B. wenn `x` und `y` Bezeichner sind, klicken Sie dann `x.y` entspricht der korrekten Grammatik für einen Typ, selbst wenn `x.y` nicht tatsächlich einen Typ anzugeben.</span><span class="sxs-lookup"><span data-stu-id="411e5-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="411e5-1610">Von der Mehrdeutigkeit Regel folgt, dass, wenn `x` und `y` Bezeichner sind, `(x)y`, `(x)(y)`, und `(x)(-y)` sind *Cast_expression*s, aber `(x)-y` nicht, selbst wenn `x` identifiziert einen Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="411e5-1611">Jedoch wenn `x` ist ein Schlüsselwort, das einen vordefinierten Typ angibt (wie z. B. `int`), dann sind alle vier Formen *Cast_expression*s (da solches Schlüsselwort nicht möglicherweise einen Ausdruck selbst handeln kann).</span><span class="sxs-lookup"><span data-stu-id="411e5-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="411e5-1612">Await-Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="411e5-1612">Await expressions</span></span>

<span data-ttu-id="411e5-1613">Der Await-Operator wird verwendet, die einschließenden Async-Funktion angehalten, bis zum Abschluss der asynchronen Vorgangs, durch den Operanden dargestellt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="411e5-1614">Ein *Await_expression* ist nur im Text einer asynchronen Funktion zulässig ([Iteratoren](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="411e5-1615">In der nächsten einschließenden Async-Funktion, ein *Await_expression* kann nicht an diesen Orten auftreten:</span><span class="sxs-lookup"><span data-stu-id="411e5-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="411e5-1616">In einer anonymen Funktion für geschachtelte (nicht-Async)</span><span class="sxs-lookup"><span data-stu-id="411e5-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="411e5-1617">Innerhalb des Blocks, der eine *Lock_statement*</span><span class="sxs-lookup"><span data-stu-id="411e5-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="411e5-1618">In einem unsicheren Kontext</span><span class="sxs-lookup"><span data-stu-id="411e5-1618">In an unsafe context</span></span>

<span data-ttu-id="411e5-1619">Beachten Sie, dass ein *Await_expression* kann nicht in den meisten Stellen im Auftreten einer *Query_expression*, da die syntaktisch transformiert werden, um nicht-Async-Lambda-Ausdrücke verwenden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="411e5-1620">Innerhalb einer Async-Funktion `await` kann nicht als Bezeichner verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="411e5-1621">Daher besteht keine syntaktischen Mehrdeutigkeit zwischen await-Ausdrücken und verschiedene Ausdrücke, die im Zusammenhang mit Bezeichnern.</span><span class="sxs-lookup"><span data-stu-id="411e5-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="411e5-1622">Außerhalb von asynchronen Funktionen `await` fungiert als ein normaler Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="411e5-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="411e5-1623">Der Operand ein *Await_expression* heißt die ***Aufgabe***.</span><span class="sxs-lookup"><span data-stu-id="411e5-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="411e5-1624">Es stellt einen asynchronen Vorgang, die möglicherweise oder ist möglicherweise unvollständig zum Zeitpunkt der *Await_expression* ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="411e5-1625">Der Zweck des Await-Operators werden von der einschließenden Funktion für die asynchrone Ausführung anzuhalten, bis die erwartete Aufgabe abgeschlossen ist, und klicken Sie dann ihr Ergebnis zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="411e5-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="411e5-1626">Awaitable-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-1626">Awaitable expressions</span></span>

<span data-ttu-id="411e5-1627">Die Aufgabe, ein Await-Ausdruck ist erforderlich, um werden ***awaitable***.</span><span class="sxs-lookup"><span data-stu-id="411e5-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="411e5-1628">Ein Ausdruck `t` ist "awaitable", wenn eine der folgenden enthält:</span><span class="sxs-lookup"><span data-stu-id="411e5-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="411e5-1629">`t` ist vom Kompilierzeittyp `dynamic`</span><span class="sxs-lookup"><span data-stu-id="411e5-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="411e5-1630">`t` verfügt über eine zugegriffen werden kann oder Erweiterungsmethode-Methode wird aufgerufen, `GetAwaiter` ohne Parameter und keine Typparameter und einen Rückgabetyp `A` für die alle der folgenden enthalten:</span><span class="sxs-lookup"><span data-stu-id="411e5-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="411e5-1631">`A` implementiert die Schnittstelle `System.Runtime.CompilerServices.INotifyCompletion` (im folgenden genannten `INotifyCompletion` aus Gründen der Übersichtlichkeit)</span><span class="sxs-lookup"><span data-stu-id="411e5-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="411e5-1632">`A` verfügt über eine Instanzeigenschaft zugegriffen werden kann, lesbaren `IsCompleted` des Typs `bool`</span><span class="sxs-lookup"><span data-stu-id="411e5-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="411e5-1633">`A` verfügt über eine zugängliche Instanzmethode Methode `GetResult` ohne Parameter und keine Typparameter</span><span class="sxs-lookup"><span data-stu-id="411e5-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="411e5-1634">Der Zweck der `GetAwaiter` Methode besteht darin, erhalten eine ***"awaiter"*** für den Task.</span><span class="sxs-lookup"><span data-stu-id="411e5-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="411e5-1635">Der Typ `A` heißt die ***"awaiter"-Typ*** für den Ausdruck "await".</span><span class="sxs-lookup"><span data-stu-id="411e5-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="411e5-1636">Der Zweck der `IsCompleted` Eigenschaft ist, um festzustellen, ob die Aufgabe bereits abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="411e5-1637">Wenn dies der Fall ist, besteht keine Notwendigkeit, Auswertung anzuhalten.</span><span class="sxs-lookup"><span data-stu-id="411e5-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="411e5-1638">Der Zweck der `INotifyCompletion.OnCompleted` Methode besteht darin, registrieren eine "fortsetzungs" an die Aufgabe, d. h. einen Delegaten (des Typs `System.Action`), wird aufgerufen, nachdem die Aufgabe abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="411e5-1639">Der Zweck der `GetResult` Methode besteht darin, das Ergebnis des Vorgangs zu erhalten, sobald sie abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="411e5-1640">Dieses Ergebnis möglicherweise den erfolgreichen Abschluss, möglicherweise mit einem Ergebniswert, oder es kann sein, eine Ausnahme, indem ausgelöst wird die `GetResult` Methode.</span><span class="sxs-lookup"><span data-stu-id="411e5-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="411e5-1641">Klassifizierung des await-Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="411e5-1641">Classification of await expressions</span></span>

<span data-ttu-id="411e5-1642">Der Ausdruck `await t` wird die gleiche Weise wie der Ausdruck klassifiziert `(t).GetAwaiter().GetResult()`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="411e5-1643">Daher, wenn der Rückgabetyp der `GetResult` ist `void`, *Await_expression* wird als "nothing" klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="411e5-1644">Wenn sie einen nicht-Void-Rückgabetyp verfügt `T`, *Await_expression* wird als ein Wert vom Typ klassifiziert `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="411e5-1645">Runtime-Auswertung von await-Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="411e5-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="411e5-1646">Zur Laufzeit wird der Ausdruck `await t` wird wie folgt ausgewertet:</span><span class="sxs-lookup"><span data-stu-id="411e5-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="411e5-1647">Ein awaiter-Element `a` erhalten Sie durch die Auswertung des Ausdrucks `(t).GetAwaiter()`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="411e5-1648">Ein `bool` `b` erhalten Sie durch die Auswertung des Ausdrucks `(a).IsCompleted`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="411e5-1649">Wenn `b` ist `false` und Auswertung, ob abhängt `a` implementiert die Schnittstelle `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (im folgenden genannten `ICriticalNotifyCompletion` aus Gründen der Übersichtlichkeit).</span><span class="sxs-lookup"><span data-stu-id="411e5-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="411e5-1650">Diese Überprüfung wird durchgeführt, auf die Bindungszeit; zur Laufzeit z. B. wenn `a` hat den Kompilierzeittyp `dynamic`, und zum Zeitpunkt der Kompilierung andernfalls.</span><span class="sxs-lookup"><span data-stu-id="411e5-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="411e5-1651">Lassen Sie `r` Wiederaufnahmedelegat zu kennzeichnen ([Iteratoren](classes.md#iterators)):</span><span class="sxs-lookup"><span data-stu-id="411e5-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="411e5-1652">Wenn `a` implementiert nicht `ICriticalNotifyCompletion`, klicken Sie dann den Ausdruck `(a as (INotifyCompletion)).OnCompleted(r)` ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="411e5-1653">Wenn `a` implementiert `ICriticalNotifyCompletion`, klicken Sie dann den Ausdruck `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="411e5-1654">Auswertung wird angehalten, und die Kontrolle an den aktuellen Aufrufer der Async-Funktion zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="411e5-1655">Entweder unmittelbar auf (Wenn `b` wurde `true`), oder bei späteren Aufruf der Wiederaufnahmedelegat (Wenn `b` wurde `false`), der Ausdruck `(a).GetResult()` ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="411e5-1656">Wenn sie einen Wert zurückgibt, ist dieser Wert das Ergebnis der *Await_expression*.</span><span class="sxs-lookup"><span data-stu-id="411e5-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="411e5-1657">Andernfalls ist das Ergebnis mit "nothing".</span><span class="sxs-lookup"><span data-stu-id="411e5-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="411e5-1658">Ein awaiter-Element-Implementierung der Schnittstellenmethoden `INotifyCompletion.OnCompleted` und `ICriticalNotifyCompletion.UnsafeOnCompleted` sollten dazu führen, dass der Delegat `r` höchstens einmal aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="411e5-1659">Andernfalls ist das Verhalten der einschließenden Async-Funktion nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="411e5-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="411e5-1660">Arithmetische operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-1660">Arithmetic operators</span></span>

<span data-ttu-id="411e5-1661">Die `*`, `/`, `%`, `+`, und `-` Operatoren werden die arithmetischen Operatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

<span data-ttu-id="411e5-1662">Wenn ein Operand eines arithmetischen Operators der Kompilierzeit-Typ hat `dynamic`, und klicken Sie dann der Ausdruck dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="411e5-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="411e5-1663">In diesem Fall den Kompilierzeittyp des Ausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit mit dem Laufzeit-Typ, der diese Operanden mit der Kompilierzeittyp `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="411e5-1664">Multiplikationsoperator</span><span class="sxs-lookup"><span data-stu-id="411e5-1664">Multiplication operator</span></span>

<span data-ttu-id="411e5-1665">Für einen Vorgang des Formulars `x * y`, binärer Operator der überladungsauflösung ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-1666">Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="411e5-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="411e5-1667">Der vordefinierte Multiplikationsoperatoren sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="411e5-1668">Alle Operatoren ermittelt das Produkt der `x` und `y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="411e5-1669">Multiplikation:</span><span class="sxs-lookup"><span data-stu-id="411e5-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="411e5-1670">In einer `checked` Kontext, wenn außerhalb des Bereichs des Ergebnistyps, das Produkt ist eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="411e5-1671">In einer `unchecked` Kontext Überläufe werden nicht gemeldet, und alle signifikanten höherwertigen Bits außerhalb des Bereichs des Ergebnistyps werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="411e5-1672">Multiplikation mit Gleitkommawerten:</span><span class="sxs-lookup"><span data-stu-id="411e5-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="411e5-1673">Das Produkt wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="411e5-1674">Die folgende Tabelle enthält die Ergebnisse der alle möglichen Kombinationen von endlichen Werten ungleich NULL, Nullen, unendliche und NaN.</span><span class="sxs-lookup"><span data-stu-id="411e5-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="411e5-1675">In der Tabelle `x` und `y` positive endliche Werten sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="411e5-1676">`z` ist das Ergebnis des `x * y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="411e5-1677">Wenn das Ergebnis zu groß für den Zieltyp `z` unendlich ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="411e5-1678">Wenn das Ergebnis für den Zieltyp zu klein ist `z` ist 0 (null).</span><span class="sxs-lookup"><span data-stu-id="411e5-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="411e5-1679">+y</span><span class="sxs-lookup"><span data-stu-id="411e5-1679">+y</span></span>   | <span data-ttu-id="411e5-1680">-y</span><span class="sxs-lookup"><span data-stu-id="411e5-1680">-y</span></span>   | <span data-ttu-id="411e5-1681">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1681">+0</span></span>  | <span data-ttu-id="411e5-1682">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1682">-0</span></span>  | <span data-ttu-id="411e5-1683">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1683">+inf</span></span> | <span data-ttu-id="411e5-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1684">-inf</span></span> | <span data-ttu-id="411e5-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1685">NaN</span></span> | 
   | <span data-ttu-id="411e5-1686">+x</span><span class="sxs-lookup"><span data-stu-id="411e5-1686">+x</span></span>   | <span data-ttu-id="411e5-1687">+z</span><span class="sxs-lookup"><span data-stu-id="411e5-1687">+z</span></span>   | <span data-ttu-id="411e5-1688">-z</span><span class="sxs-lookup"><span data-stu-id="411e5-1688">-z</span></span>   | <span data-ttu-id="411e5-1689">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1689">+0</span></span>  | <span data-ttu-id="411e5-1690">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1690">-0</span></span>  | <span data-ttu-id="411e5-1691">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1691">+inf</span></span> | <span data-ttu-id="411e5-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1692">-inf</span></span> | <span data-ttu-id="411e5-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1693">NaN</span></span> | 
   | <span data-ttu-id="411e5-1694">-x</span><span class="sxs-lookup"><span data-stu-id="411e5-1694">-x</span></span>   | <span data-ttu-id="411e5-1695">-z</span><span class="sxs-lookup"><span data-stu-id="411e5-1695">-z</span></span>   | <span data-ttu-id="411e5-1696">+z</span><span class="sxs-lookup"><span data-stu-id="411e5-1696">+z</span></span>   | <span data-ttu-id="411e5-1697">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1697">-0</span></span>  | <span data-ttu-id="411e5-1698">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1698">+0</span></span>  | <span data-ttu-id="411e5-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1699">-inf</span></span> | <span data-ttu-id="411e5-1700">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1700">+inf</span></span> | <span data-ttu-id="411e5-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1701">NaN</span></span> | 
   | <span data-ttu-id="411e5-1702">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1702">+0</span></span>   | <span data-ttu-id="411e5-1703">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1703">+0</span></span>   | <span data-ttu-id="411e5-1704">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1704">-0</span></span>   | <span data-ttu-id="411e5-1705">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1705">+0</span></span>  | <span data-ttu-id="411e5-1706">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1706">-0</span></span>  | <span data-ttu-id="411e5-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1707">NaN</span></span>  | <span data-ttu-id="411e5-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1708">NaN</span></span>  | <span data-ttu-id="411e5-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1709">NaN</span></span> | 
   | <span data-ttu-id="411e5-1710">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1710">-0</span></span>   | <span data-ttu-id="411e5-1711">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1711">-0</span></span>   | <span data-ttu-id="411e5-1712">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1712">+0</span></span>   | <span data-ttu-id="411e5-1713">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1713">-0</span></span>  | <span data-ttu-id="411e5-1714">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1714">+0</span></span>  | <span data-ttu-id="411e5-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1715">NaN</span></span>  | <span data-ttu-id="411e5-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1716">NaN</span></span>  | <span data-ttu-id="411e5-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1717">NaN</span></span> | 
   | <span data-ttu-id="411e5-1718">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1718">+inf</span></span> | <span data-ttu-id="411e5-1719">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1719">+inf</span></span> | <span data-ttu-id="411e5-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1720">-inf</span></span> | <span data-ttu-id="411e5-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1721">NaN</span></span> | <span data-ttu-id="411e5-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1722">NaN</span></span> | <span data-ttu-id="411e5-1723">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1723">+inf</span></span> | <span data-ttu-id="411e5-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1724">-inf</span></span> | <span data-ttu-id="411e5-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1725">NaN</span></span> | 
   | <span data-ttu-id="411e5-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1726">-inf</span></span> | <span data-ttu-id="411e5-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1727">-inf</span></span> | <span data-ttu-id="411e5-1728">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1728">+inf</span></span> | <span data-ttu-id="411e5-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1729">NaN</span></span> | <span data-ttu-id="411e5-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1730">NaN</span></span> | <span data-ttu-id="411e5-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1731">-inf</span></span> | <span data-ttu-id="411e5-1732">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1732">+inf</span></span> | <span data-ttu-id="411e5-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1733">NaN</span></span> | 
   | <span data-ttu-id="411e5-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1734">NaN</span></span>  | <span data-ttu-id="411e5-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1735">NaN</span></span>  | <span data-ttu-id="411e5-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1736">NaN</span></span>  | <span data-ttu-id="411e5-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1737">NaN</span></span> | <span data-ttu-id="411e5-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1738">NaN</span></span> | <span data-ttu-id="411e5-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1739">NaN</span></span>  | <span data-ttu-id="411e5-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1740">NaN</span></span>  | <span data-ttu-id="411e5-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1741">NaN</span></span> | 

*  <span data-ttu-id="411e5-1742">Dezimale Multiplikation:</span><span class="sxs-lookup"><span data-stu-id="411e5-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="411e5-1743">Ist der resultierende Wert zu groß, um die Darstellung in der `decimal` Format eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="411e5-1744">Ist der Ergebniswert zu klein, um das Darstellen der `decimal` Format, das Ergebnis ist 0 (null).</span><span class="sxs-lookup"><span data-stu-id="411e5-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="411e5-1745">Die Dezimalstellen des Ergebnisses wird vor der Rundung, ist die Summe aus der Skalierung der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="411e5-1746">Decimal Multiplikation ist äquivalent zur Verwendung des Multiplikationsoperators des Typs `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="411e5-1747">Divisionsoperator</span><span class="sxs-lookup"><span data-stu-id="411e5-1747">Division operator</span></span>

<span data-ttu-id="411e5-1748">Für einen Vorgang des Formulars `x / y`, binärer Operator der überladungsauflösung ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-1749">Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="411e5-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="411e5-1750">Der vordefinierte Divisionsoperatoren sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="411e5-1751">Alle Operatoren berechnen den Quotienten `x` und `y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="411e5-1752">Division ganzer Zahlen kommt:</span><span class="sxs-lookup"><span data-stu-id="411e5-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="411e5-1753">Wenn der Wert des rechten Operanden NULL ist, ist eine `System.DivideByZeroException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="411e5-1754">Die Division rundet das Ergebnis in Richtung 0 (null).</span><span class="sxs-lookup"><span data-stu-id="411e5-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="411e5-1755">Daher ist der Absolute Wert des Ergebnisses wird die größte ganze Zahl möglich, die kleiner oder gleich dem absoluten Wert des Quotienten aus zwei Operanden ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="411e5-1756">Das Ergebnis ist 0 (null) oder positiv, wenn die beiden Operanden der gleichen Anmeldebenutzeroberfläche und 0 (null) oder negativ, wenn die beiden Operanden entgegengesetzten signiert haben.</span><span class="sxs-lookup"><span data-stu-id="411e5-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="411e5-1757">Wenn der linke Operand der kleinste darstellbare ist `int` oder `long` Wert und der Rechte Operand ist `-1`, einem Überlauf kommt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="411e5-1758">In einem `checked` Kontext Dies bewirkt, dass eine `System.ArithmeticException` (oder eine Unterklasse davon) ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="411e5-1759">In einer `unchecked` Kontext es wird durch die Implementierung definiert, ob eine `System.ArithmeticException` (oder eine Unterklasse davon) wird ausgelöst, oder der Überlauf wird mit den sich ergebenden Wert, der der linke Operand wird nicht gemeldet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="411e5-1760">Gleitkommadivision:</span><span class="sxs-lookup"><span data-stu-id="411e5-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="411e5-1761">Der Quotient ist gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="411e5-1762">Die folgende Tabelle enthält die Ergebnisse der alle möglichen Kombinationen von endlichen Werten ungleich NULL, Nullen, unendliche und NaN.</span><span class="sxs-lookup"><span data-stu-id="411e5-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="411e5-1763">In der Tabelle `x` und `y` positive endliche Werten sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="411e5-1764">`z` ist das Ergebnis des `x / y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="411e5-1765">Wenn das Ergebnis zu groß für den Zieltyp `z` unendlich ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="411e5-1766">Wenn das Ergebnis für den Zieltyp zu klein ist `z` ist 0 (null).</span><span class="sxs-lookup"><span data-stu-id="411e5-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="411e5-1767">+y</span><span class="sxs-lookup"><span data-stu-id="411e5-1767">+y</span></span>   | <span data-ttu-id="411e5-1768">-y</span><span class="sxs-lookup"><span data-stu-id="411e5-1768">-y</span></span>   | <span data-ttu-id="411e5-1769">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1769">+0</span></span>   | <span data-ttu-id="411e5-1770">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1770">-0</span></span>   | <span data-ttu-id="411e5-1771">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1771">+inf</span></span> | <span data-ttu-id="411e5-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1772">-inf</span></span> | <span data-ttu-id="411e5-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1773">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1774">+x</span><span class="sxs-lookup"><span data-stu-id="411e5-1774">+x</span></span>   | <span data-ttu-id="411e5-1775">+z</span><span class="sxs-lookup"><span data-stu-id="411e5-1775">+z</span></span>   | <span data-ttu-id="411e5-1776">-z</span><span class="sxs-lookup"><span data-stu-id="411e5-1776">-z</span></span>   | <span data-ttu-id="411e5-1777">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1777">+inf</span></span> | <span data-ttu-id="411e5-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1778">-inf</span></span> | <span data-ttu-id="411e5-1779">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1779">+0</span></span>   | <span data-ttu-id="411e5-1780">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1780">-0</span></span>   | <span data-ttu-id="411e5-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1781">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1782">-x</span><span class="sxs-lookup"><span data-stu-id="411e5-1782">-x</span></span>   | <span data-ttu-id="411e5-1783">-z</span><span class="sxs-lookup"><span data-stu-id="411e5-1783">-z</span></span>   | <span data-ttu-id="411e5-1784">+z</span><span class="sxs-lookup"><span data-stu-id="411e5-1784">+z</span></span>   | <span data-ttu-id="411e5-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1785">-inf</span></span> | <span data-ttu-id="411e5-1786">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1786">+inf</span></span> | <span data-ttu-id="411e5-1787">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1787">-0</span></span>   | <span data-ttu-id="411e5-1788">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1788">+0</span></span>   | <span data-ttu-id="411e5-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1789">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1790">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1790">+0</span></span>   | <span data-ttu-id="411e5-1791">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1791">+0</span></span>   | <span data-ttu-id="411e5-1792">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1792">-0</span></span>   | <span data-ttu-id="411e5-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1793">NaN</span></span>  | <span data-ttu-id="411e5-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1794">NaN</span></span>  | <span data-ttu-id="411e5-1795">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1795">+0</span></span>   | <span data-ttu-id="411e5-1796">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1796">-0</span></span>   | <span data-ttu-id="411e5-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1797">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1798">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1798">-0</span></span>   | <span data-ttu-id="411e5-1799">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1799">-0</span></span>   | <span data-ttu-id="411e5-1800">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1800">+0</span></span>   | <span data-ttu-id="411e5-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1801">NaN</span></span>  | <span data-ttu-id="411e5-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1802">NaN</span></span>  | <span data-ttu-id="411e5-1803">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1803">-0</span></span>   | <span data-ttu-id="411e5-1804">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1804">+0</span></span>   | <span data-ttu-id="411e5-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1805">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1806">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1806">+inf</span></span> | <span data-ttu-id="411e5-1807">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1807">+inf</span></span> | <span data-ttu-id="411e5-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1808">-inf</span></span> | <span data-ttu-id="411e5-1809">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1809">+inf</span></span> | <span data-ttu-id="411e5-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1810">-inf</span></span> | <span data-ttu-id="411e5-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1811">NaN</span></span>  | <span data-ttu-id="411e5-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1812">NaN</span></span>  | <span data-ttu-id="411e5-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1813">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1814">-inf</span></span> | <span data-ttu-id="411e5-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1815">-inf</span></span> | <span data-ttu-id="411e5-1816">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1816">+inf</span></span> | <span data-ttu-id="411e5-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1817">-inf</span></span> | <span data-ttu-id="411e5-1818">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1818">+inf</span></span> | <span data-ttu-id="411e5-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1819">NaN</span></span>  | <span data-ttu-id="411e5-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1820">NaN</span></span>  | <span data-ttu-id="411e5-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1821">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1822">NaN</span></span>  | <span data-ttu-id="411e5-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1823">NaN</span></span>  | <span data-ttu-id="411e5-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1824">NaN</span></span>  | <span data-ttu-id="411e5-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1825">NaN</span></span>  | <span data-ttu-id="411e5-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1826">NaN</span></span>  | <span data-ttu-id="411e5-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1827">NaN</span></span>  | <span data-ttu-id="411e5-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1828">NaN</span></span>  | <span data-ttu-id="411e5-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1829">NaN</span></span>  | 

*  <span data-ttu-id="411e5-1830">Dezimale Division:</span><span class="sxs-lookup"><span data-stu-id="411e5-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="411e5-1831">Wenn der Wert des rechten Operanden NULL ist, ist eine `System.DivideByZeroException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="411e5-1832">Ist der resultierende Wert zu groß, um die Darstellung in der `decimal` Format eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="411e5-1833">Ist der Ergebniswert zu klein, um das Darstellen der `decimal` Format, das Ergebnis ist 0 (null).</span><span class="sxs-lookup"><span data-stu-id="411e5-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="411e5-1834">Die Dezimalstellen des Ergebnisses ist der kleinste Skala, die ein Ergebnis gleich beibehalten werden das am nächsten darstellbaren decimal-Wert auf "true" mathematische Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="411e5-1835">Dezimale Division ist äquivalent zur Verwendung des Divisionsoperators des Typs `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="411e5-1836">Restoperator</span><span class="sxs-lookup"><span data-stu-id="411e5-1836">Remainder operator</span></span>

<span data-ttu-id="411e5-1837">Für einen Vorgang des Formulars `x % y`, binärer Operator der überladungsauflösung ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-1838">Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="411e5-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="411e5-1839">Der vordefinierte restoperatoren sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="411e5-1840">Alle Operatoren berechnen den Rest der Division zwischen `x` und `y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="411e5-1841">Ganzzahligen Rest:</span><span class="sxs-lookup"><span data-stu-id="411e5-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="411e5-1842">Das Ergebnis des `x % y` wird durch der Wert erzeugt `x - (x / y) * y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="411e5-1843">Wenn `y` ist 0 (null), eine `System.DivideByZeroException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="411e5-1844">Wenn der linke Operand der kleinsten ist `int` oder `long` Wert und der Rechte Operand ist `-1`, `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="411e5-1845">Ist in keinem Fall `x % y` löst eine Ausnahme aus, in denen `x / y` wird keine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="411e5-1846">Gleitkommarest:</span><span class="sxs-lookup"><span data-stu-id="411e5-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="411e5-1847">Die folgende Tabelle enthält die Ergebnisse der alle möglichen Kombinationen von endlichen Werten ungleich NULL, Nullen, unendliche und NaN.</span><span class="sxs-lookup"><span data-stu-id="411e5-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="411e5-1848">In der Tabelle `x` und `y` positive endliche Werten sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="411e5-1849">`z` ist das Ergebnis des `x % y` und wird berechnet als `x - n * y`, wobei `n` ist die größte ganze Zahl mögliche, die kleiner als oder gleich `x / y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="411e5-1850">Diese Methode, der Berechnung des Rests ist analog zu, die für ganzzahlige Operanden verwendet, unterscheidet sich jedoch die IEEE 754-Definition (in der `n` ist eine ganze Zahl, die am nächsten `x / y`).</span><span class="sxs-lookup"><span data-stu-id="411e5-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="411e5-1851">+y</span><span class="sxs-lookup"><span data-stu-id="411e5-1851">+y</span></span>   | <span data-ttu-id="411e5-1852">-y</span><span class="sxs-lookup"><span data-stu-id="411e5-1852">-y</span></span>   | <span data-ttu-id="411e5-1853">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1853">+0</span></span>   | <span data-ttu-id="411e5-1854">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1854">-0</span></span>   | <span data-ttu-id="411e5-1855">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1855">+inf</span></span> | <span data-ttu-id="411e5-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1856">-inf</span></span> | <span data-ttu-id="411e5-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1857">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1858">+x</span><span class="sxs-lookup"><span data-stu-id="411e5-1858">+x</span></span>   | <span data-ttu-id="411e5-1859">+z</span><span class="sxs-lookup"><span data-stu-id="411e5-1859">+z</span></span>   | <span data-ttu-id="411e5-1860">+z</span><span class="sxs-lookup"><span data-stu-id="411e5-1860">+z</span></span>   | <span data-ttu-id="411e5-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1861">NaN</span></span>  | <span data-ttu-id="411e5-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1862">NaN</span></span>  | <span data-ttu-id="411e5-1863">w</span><span class="sxs-lookup"><span data-stu-id="411e5-1863">x</span></span>    | <span data-ttu-id="411e5-1864">w</span><span class="sxs-lookup"><span data-stu-id="411e5-1864">x</span></span>    | <span data-ttu-id="411e5-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1865">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1866">-x</span><span class="sxs-lookup"><span data-stu-id="411e5-1866">-x</span></span>   | <span data-ttu-id="411e5-1867">-z</span><span class="sxs-lookup"><span data-stu-id="411e5-1867">-z</span></span>   | <span data-ttu-id="411e5-1868">-z</span><span class="sxs-lookup"><span data-stu-id="411e5-1868">-z</span></span>   | <span data-ttu-id="411e5-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1869">NaN</span></span>  | <span data-ttu-id="411e5-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1870">NaN</span></span>  | <span data-ttu-id="411e5-1871">-x</span><span class="sxs-lookup"><span data-stu-id="411e5-1871">-x</span></span>   | <span data-ttu-id="411e5-1872">-x</span><span class="sxs-lookup"><span data-stu-id="411e5-1872">-x</span></span>   | <span data-ttu-id="411e5-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1873">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1874">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1874">+0</span></span>   | <span data-ttu-id="411e5-1875">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1875">+0</span></span>   | <span data-ttu-id="411e5-1876">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1876">+0</span></span>   | <span data-ttu-id="411e5-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1877">NaN</span></span>  | <span data-ttu-id="411e5-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1878">NaN</span></span>  | <span data-ttu-id="411e5-1879">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1879">+0</span></span>   | <span data-ttu-id="411e5-1880">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1880">+0</span></span>   | <span data-ttu-id="411e5-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1881">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1882">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1882">-0</span></span>   | <span data-ttu-id="411e5-1883">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1883">-0</span></span>   | <span data-ttu-id="411e5-1884">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1884">-0</span></span>   | <span data-ttu-id="411e5-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1885">NaN</span></span>  | <span data-ttu-id="411e5-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1886">NaN</span></span>  | <span data-ttu-id="411e5-1887">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1887">-0</span></span>   | <span data-ttu-id="411e5-1888">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1888">-0</span></span>   | <span data-ttu-id="411e5-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1889">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1890">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1890">+inf</span></span> | <span data-ttu-id="411e5-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1891">NaN</span></span>  | <span data-ttu-id="411e5-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1892">NaN</span></span>  | <span data-ttu-id="411e5-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1893">NaN</span></span>  | <span data-ttu-id="411e5-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1894">NaN</span></span>  | <span data-ttu-id="411e5-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1895">NaN</span></span>  | <span data-ttu-id="411e5-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1896">NaN</span></span>  | <span data-ttu-id="411e5-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1897">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1898">-inf</span></span> | <span data-ttu-id="411e5-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1899">NaN</span></span>  | <span data-ttu-id="411e5-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1900">NaN</span></span>  | <span data-ttu-id="411e5-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1901">NaN</span></span>  | <span data-ttu-id="411e5-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1902">NaN</span></span>  | <span data-ttu-id="411e5-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1903">NaN</span></span>  | <span data-ttu-id="411e5-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1904">NaN</span></span>  | <span data-ttu-id="411e5-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1905">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1906">NaN</span></span>  | <span data-ttu-id="411e5-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1907">NaN</span></span>  | <span data-ttu-id="411e5-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1908">NaN</span></span>  | <span data-ttu-id="411e5-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1909">NaN</span></span>  | <span data-ttu-id="411e5-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1910">NaN</span></span>  | <span data-ttu-id="411e5-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1911">NaN</span></span>  | <span data-ttu-id="411e5-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1912">NaN</span></span>  | <span data-ttu-id="411e5-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1913">NaN</span></span>  | 

*  <span data-ttu-id="411e5-1914">Decimal Rest:</span><span class="sxs-lookup"><span data-stu-id="411e5-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="411e5-1915">Wenn der Wert des rechten Operanden NULL ist, ist eine `System.DivideByZeroException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="411e5-1916">Die Dezimalstellen des Ergebnisses wird vor der Rundung, ist der jeweils größere der Skalen der beiden Operanden aus, und die Vorzeichen des Ergebnisses ausgeführt, wenn ungleich NULL ist, entspricht der `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="411e5-1917">Decimal Rest ist äquivalent zur Verwendung der Restoperator vom Typ `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="411e5-1918">Additionsoperator</span><span class="sxs-lookup"><span data-stu-id="411e5-1918">Addition operator</span></span>

<span data-ttu-id="411e5-1919">Für einen Vorgang des Formulars `x + y`, binärer Operator der überladungsauflösung ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-1920">Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="411e5-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="411e5-1921">Die vordefinierten Additionsoperatoren sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="411e5-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="411e5-1922">Für die numerischen Typen und Enumerationstypen berechnen die vordefinierten Additionsoperatoren die Summe der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="411e5-1923">Wenn einer oder beide Operanden vom Typzeichenfolge sind, verketten Sie die vordefinierten Additionsoperatoren eine Zeichenfolgendarstellung der Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="411e5-1924">Ganze Zahl hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="411e5-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="411e5-1925">In einem `checked` Kontext, wenn die Summe außerhalb des Bereichs des Ergebnistyps, ist eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="411e5-1926">In einer `unchecked` Kontext Überläufe werden nicht gemeldet, und alle signifikanten höherwertigen Bits außerhalb des Bereichs des Ergebnistyps werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="411e5-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="411e5-1927">Gleitkommaaddition:</span><span class="sxs-lookup"><span data-stu-id="411e5-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="411e5-1928">Die Summe wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="411e5-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="411e5-1929">Die folgende Tabelle enthält die Ergebnisse der alle möglichen Kombinationen von endlichen Werten ungleich NULL, Nullen, unendliche und NaN.</span><span class="sxs-lookup"><span data-stu-id="411e5-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="411e5-1930">In der Tabelle `x` und `y` sind begrenzte Werte ungleich NULL, und `z` ist das Ergebnis des `x + y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="411e5-1931">Wenn `x` und `y` dieselbe Größenordnung haben, aber umgekehrte meldet, `z` plus null ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="411e5-1932">Wenn `x + y` ist zu groß für den Zieltyp darstellen `z` unendlich und hat das gleichen Vorzeichen wie `x + y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="411e5-1933">y</span><span class="sxs-lookup"><span data-stu-id="411e5-1933">y</span></span>    | <span data-ttu-id="411e5-1934">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1934">+0</span></span>   | <span data-ttu-id="411e5-1935">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1935">-0</span></span>   | <span data-ttu-id="411e5-1936">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1936">+inf</span></span> | <span data-ttu-id="411e5-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1937">-inf</span></span> | <span data-ttu-id="411e5-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1938">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1939">w</span><span class="sxs-lookup"><span data-stu-id="411e5-1939">x</span></span>    | <span data-ttu-id="411e5-1940">z</span><span class="sxs-lookup"><span data-stu-id="411e5-1940">z</span></span>    | <span data-ttu-id="411e5-1941">w</span><span class="sxs-lookup"><span data-stu-id="411e5-1941">x</span></span>    | <span data-ttu-id="411e5-1942">w</span><span class="sxs-lookup"><span data-stu-id="411e5-1942">x</span></span>    | <span data-ttu-id="411e5-1943">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1943">+inf</span></span> | <span data-ttu-id="411e5-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1944">-inf</span></span> | <span data-ttu-id="411e5-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1945">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1946">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1946">+0</span></span>   | <span data-ttu-id="411e5-1947">y</span><span class="sxs-lookup"><span data-stu-id="411e5-1947">y</span></span>    | <span data-ttu-id="411e5-1948">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1948">+0</span></span>   | <span data-ttu-id="411e5-1949">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1949">+0</span></span>   | <span data-ttu-id="411e5-1950">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1950">+inf</span></span> | <span data-ttu-id="411e5-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1951">-inf</span></span> | <span data-ttu-id="411e5-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1952">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1953">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1953">-0</span></span>   | <span data-ttu-id="411e5-1954">y</span><span class="sxs-lookup"><span data-stu-id="411e5-1954">y</span></span>    | <span data-ttu-id="411e5-1955">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-1955">+0</span></span>   | <span data-ttu-id="411e5-1956">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-1956">-0</span></span>   | <span data-ttu-id="411e5-1957">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1957">+inf</span></span> | <span data-ttu-id="411e5-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1958">-inf</span></span> | <span data-ttu-id="411e5-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1959">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1960">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1960">+inf</span></span> | <span data-ttu-id="411e5-1961">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1961">+inf</span></span> | <span data-ttu-id="411e5-1962">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1962">+inf</span></span> | <span data-ttu-id="411e5-1963">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1963">+inf</span></span> | <span data-ttu-id="411e5-1964">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1964">+inf</span></span> | <span data-ttu-id="411e5-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1965">NaN</span></span>  | <span data-ttu-id="411e5-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1966">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1967">-inf</span></span> | <span data-ttu-id="411e5-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1968">-inf</span></span> | <span data-ttu-id="411e5-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1969">-inf</span></span> | <span data-ttu-id="411e5-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1970">-inf</span></span> | <span data-ttu-id="411e5-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1971">NaN</span></span>  | <span data-ttu-id="411e5-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-1972">-inf</span></span> | <span data-ttu-id="411e5-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1973">NaN</span></span>  | 
   | <span data-ttu-id="411e5-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1974">NaN</span></span>  | <span data-ttu-id="411e5-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1975">NaN</span></span>  | <span data-ttu-id="411e5-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1976">NaN</span></span>  | <span data-ttu-id="411e5-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1977">NaN</span></span>  | <span data-ttu-id="411e5-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1978">NaN</span></span>  | <span data-ttu-id="411e5-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1979">NaN</span></span>  | <span data-ttu-id="411e5-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-1980">NaN</span></span>  | 

*  <span data-ttu-id="411e5-1981">Dezimalstellen hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="411e5-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="411e5-1982">Ist der resultierende Wert zu groß, um die Darstellung in der `decimal` Format eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="411e5-1983">Die Dezimalstellen des Ergebnisses wird vor der Rundung, ist der jeweils größere der Skalen der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="411e5-1984">Dezimale Addition ist äquivalent zur Verwendung des Addition-Operators vom Typ `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="411e5-1985">Hinzufügen der Enumeration.</span><span class="sxs-lookup"><span data-stu-id="411e5-1985">Enumeration addition.</span></span> <span data-ttu-id="411e5-1986">Jeder Enumerationstyp implizit bietet die folgenden vordefinierten Operatoren, in denen `E` ist der Enumerationstyp und `U` ist der zugrunde liegenden Typ des `E`:</span><span class="sxs-lookup"><span data-stu-id="411e5-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="411e5-1987">Zur Laufzeit diese Operatoren ausgewertet werden, genau wie `(E)((U)x + (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="411e5-1988">Verketten von Zeichenfolgen:</span><span class="sxs-lookup"><span data-stu-id="411e5-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="411e5-1989">Diese Überladungen der Binärdatei `+` Operator führen eine zeichenfolgenverkettung aus.</span><span class="sxs-lookup"><span data-stu-id="411e5-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="411e5-1990">Wenn ein Operand vom zeichenfolgenverkettung wird `null`, eine leere Zeichenfolge ersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="411e5-1991">Andernfalls jedes Argument keine Zeichenfolge durch Aufrufen der virtuellen in seine Zeichenfolgendarstellung konvertiert ist `ToString` Methode geerbt vom Typ `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="411e5-1992">Wenn `ToString` gibt `null`, eine leere Zeichenfolge ersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   Das Ergebnis der Operator für zeichenfolgenverkettungen ist eine Zeichenfolge, die aus der Zeichen des linken Operanden gefolgt von den Zeichen des rechten Operanden. Gibt zurück, der Operator für zeichenfolgenverkettung nie eine `null` Wert. <span data-ttu-id="411e5-1995">Ein `System.OutOfMemoryException` möglicherweise ausgelöst, wenn nicht genügend Arbeitsspeicher verfügbar, um die resultierende Zeichenfolge steht.</span><span class="sxs-lookup"><span data-stu-id="411e5-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="411e5-1996">Delegieren Sie die Kombination aus.</span><span class="sxs-lookup"><span data-stu-id="411e5-1996">Delegate combination.</span></span> <span data-ttu-id="411e5-1997">Alle Delegattypen implizit bietet die folgenden vordefinierten Operator, in denen `D` ist der Delegattyp:</span><span class="sxs-lookup"><span data-stu-id="411e5-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="411e5-1998">Die Binärdatei `+` Operator führt Delegatkombination aus, wenn beide Operanden vom Delegattyp sind `D`.</span><span class="sxs-lookup"><span data-stu-id="411e5-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="411e5-1999">(Wenn die Operanden unterschiedliche Delegattypen aufweisen, tritt ein Fehler während der Bindung.) Wenn der erste Operand `null`, das Ergebnis des Vorgangs ist der Wert des zweiten Operanden (selbst wenn das auch ist `null`).</span><span class="sxs-lookup"><span data-stu-id="411e5-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="411e5-2000">Wenn der zweite Operand ist, andernfalls `null`, lautet das Ergebnis des Vorgangs der Wert des ersten Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="411e5-2001">Andernfalls ist das Ergebnis des Vorgangs ein neuer Delegaten Instanz, die, wenn aufgerufen wird, ruft der erste Operand und ruft dann den zweiten Operanden auf.</span><span class="sxs-lookup"><span data-stu-id="411e5-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="411e5-2002">Beispiele für Delegatkombination, finden Sie unter [Subtraktionsoperator](expressions.md#subtraction-operator) und [Delegataufruf](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="411e5-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="411e5-2003">Da `System.Delegate` ist es sich nicht um ein Delegattyp, `operator`  `+` ist nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="411e5-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="411e5-2004">Subtraktionsoperator</span><span class="sxs-lookup"><span data-stu-id="411e5-2004">Subtraction operator</span></span>

<span data-ttu-id="411e5-2005">Für einen Vorgang des Formulars `x - y`, binärer Operator der überladungsauflösung ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-2006">Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="411e5-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="411e5-2007">Die vordefinierten Subtraktionsoperatoren sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="411e5-2008">Die Operatoren, die alle subtrahieren `y` aus `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="411e5-2009">Ganze Zahl Subtraktion:</span><span class="sxs-lookup"><span data-stu-id="411e5-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="411e5-2010">In einem `checked` Kontext, wenn außerhalb des Bereichs des Ergebnistyps, der Unterschied ist eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="411e5-2011">In einer `unchecked` Kontext Überläufe werden nicht gemeldet, und alle signifikanten höherwertigen Bits außerhalb des Bereichs des Ergebnistyps werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="411e5-2012">Subtraktion von Gleitkommazahlen:</span><span class="sxs-lookup"><span data-stu-id="411e5-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="411e5-2013">Der Unterschied wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="411e5-2014">Die folgende Tabelle enthält die Ergebnisse der alle möglichen Kombinationen von endlichen Werten ungleich NULL, Nullen, unendliche und NaN-Werte.</span><span class="sxs-lookup"><span data-stu-id="411e5-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="411e5-2015">In der Tabelle `x` und `y` sind begrenzte Werte ungleich NULL, und `z` ist das Ergebnis des `x - y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="411e5-2016">Wenn `x` und `y` gleich sind, `z` plus null ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="411e5-2017">Wenn `x - y` ist zu groß für den Zieltyp darstellen `z` unendlich und hat das gleichen Vorzeichen wie `x - y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | <span data-ttu-id="411e5-2018">y</span><span class="sxs-lookup"><span data-stu-id="411e5-2018">y</span></span>    | <span data-ttu-id="411e5-2019">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-2019">+0</span></span>   | <span data-ttu-id="411e5-2020">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-2020">-0</span></span>   | <span data-ttu-id="411e5-2021">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2021">+inf</span></span> | <span data-ttu-id="411e5-2022">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2022">-inf</span></span> | <span data-ttu-id="411e5-2023">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2023">NaN</span></span> | 
   | <span data-ttu-id="411e5-2024">w</span><span class="sxs-lookup"><span data-stu-id="411e5-2024">x</span></span>    | <span data-ttu-id="411e5-2025">z</span><span class="sxs-lookup"><span data-stu-id="411e5-2025">z</span></span>    | <span data-ttu-id="411e5-2026">w</span><span class="sxs-lookup"><span data-stu-id="411e5-2026">x</span></span>    | <span data-ttu-id="411e5-2027">w</span><span class="sxs-lookup"><span data-stu-id="411e5-2027">x</span></span>    | <span data-ttu-id="411e5-2028">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2028">-inf</span></span> | <span data-ttu-id="411e5-2029">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2029">+inf</span></span> | <span data-ttu-id="411e5-2030">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2030">NaN</span></span> | 
   | <span data-ttu-id="411e5-2031">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-2031">+0</span></span>   | <span data-ttu-id="411e5-2032">-y</span><span class="sxs-lookup"><span data-stu-id="411e5-2032">-y</span></span>   | <span data-ttu-id="411e5-2033">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-2033">+0</span></span>   | <span data-ttu-id="411e5-2034">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-2034">+0</span></span>   | <span data-ttu-id="411e5-2035">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2035">-inf</span></span> | <span data-ttu-id="411e5-2036">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2036">+inf</span></span> | <span data-ttu-id="411e5-2037">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2037">NaN</span></span> | 
   | <span data-ttu-id="411e5-2038">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-2038">-0</span></span>   | <span data-ttu-id="411e5-2039">-y</span><span class="sxs-lookup"><span data-stu-id="411e5-2039">-y</span></span>   | <span data-ttu-id="411e5-2040">-0</span><span class="sxs-lookup"><span data-stu-id="411e5-2040">-0</span></span>   | <span data-ttu-id="411e5-2041">+0</span><span class="sxs-lookup"><span data-stu-id="411e5-2041">+0</span></span>   | <span data-ttu-id="411e5-2042">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2042">-inf</span></span> | <span data-ttu-id="411e5-2043">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2043">+inf</span></span> | <span data-ttu-id="411e5-2044">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2044">NaN</span></span> | 
   | <span data-ttu-id="411e5-2045">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2045">+inf</span></span> | <span data-ttu-id="411e5-2046">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2046">+inf</span></span> | <span data-ttu-id="411e5-2047">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2047">+inf</span></span> | <span data-ttu-id="411e5-2048">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2048">+inf</span></span> | <span data-ttu-id="411e5-2049">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2049">NaN</span></span>  | <span data-ttu-id="411e5-2050">+inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2050">+inf</span></span> | <span data-ttu-id="411e5-2051">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2051">NaN</span></span> | 
   | <span data-ttu-id="411e5-2052">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2052">-inf</span></span> | <span data-ttu-id="411e5-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2053">-inf</span></span> | <span data-ttu-id="411e5-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2054">-inf</span></span> | <span data-ttu-id="411e5-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2055">-inf</span></span> | <span data-ttu-id="411e5-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="411e5-2056">-inf</span></span> | <span data-ttu-id="411e5-2057">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2057">NaN</span></span>  | <span data-ttu-id="411e5-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2058">NaN</span></span> | 
   | <span data-ttu-id="411e5-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2059">NaN</span></span>  | <span data-ttu-id="411e5-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2060">NaN</span></span>  | <span data-ttu-id="411e5-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2061">NaN</span></span>  | <span data-ttu-id="411e5-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2062">NaN</span></span>  | <span data-ttu-id="411e5-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2063">NaN</span></span>  | <span data-ttu-id="411e5-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2064">NaN</span></span>  | <span data-ttu-id="411e5-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="411e5-2065">NaN</span></span> | 

*  <span data-ttu-id="411e5-2066">Dezimale Subtraktion:</span><span class="sxs-lookup"><span data-stu-id="411e5-2066">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="411e5-2067">Ist der resultierende Wert zu groß, um die Darstellung in der `decimal` Format eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-2067">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="411e5-2068">Die Dezimalstellen des Ergebnisses wird vor der Rundung, ist der jeweils größere der Skalen der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2068">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="411e5-2069">Dezimale Subtraktion ist äquivalent zur Verwendung des Subtraktionsoperators des Typs `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2069">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="411e5-2070">Subtraktion der Enumeration.</span><span class="sxs-lookup"><span data-stu-id="411e5-2070">Enumeration subtraction.</span></span> <span data-ttu-id="411e5-2071">Jeder Enumerationstyp enthält implizit den folgenden vordefinierten-Operator, in denen `E` der Enum-Typs ist und `U` ist der zugrunde liegenden Typ des `E`:</span><span class="sxs-lookup"><span data-stu-id="411e5-2071">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="411e5-2072">Dieser Operator wird ausgewertet, genau wie `(U)((U)x - (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2072">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="411e5-2073">Das heißt, der Operator berechnet den Unterschied zwischen der Ordinalwerte der `x` und `y`, und der Typ des Ergebnisses ist der zugrunde liegende Typ der Enumeration.</span><span class="sxs-lookup"><span data-stu-id="411e5-2073">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="411e5-2074">Dieser Operator wird ausgewertet, genau wie `(E)((U)x - y)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2074">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="411e5-2075">Das heißt, subtrahiert der Operator einen Wert aus der zugrunde liegende Typ der Enumeration, dies ergibt den Wert der Enumeration.</span><span class="sxs-lookup"><span data-stu-id="411e5-2075">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="411e5-2076">Delegieren Sie zum Entfernen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2076">Delegate removal.</span></span> <span data-ttu-id="411e5-2077">Alle Delegattypen implizit bietet die folgenden vordefinierten Operator, in denen `D` ist der Delegattyp:</span><span class="sxs-lookup"><span data-stu-id="411e5-2077">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="411e5-2078">Die Binärdatei `-` Operator führt delegatentfernung aus, wenn beide Operanden vom Delegattyp sind `D`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2078">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="411e5-2079">Wenn die Operanden unterschiedliche Delegattypen haben, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2079">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="411e5-2080">Wenn der erste Operand `null`, ist das Ergebnis des Vorgangs `null`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2080">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="411e5-2081">Wenn der zweite Operand ist, andernfalls `null`, lautet das Ergebnis des Vorgangs der Wert des ersten Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2081">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="411e5-2082">Beide Operanden darstellen, andernfalls Aufruflisten ([delegieren Deklarationen](delegates.md#delegate-declarations)) ein oder mehrere Einträge, und das Ergebnis ist eine neue Aufrufliste besteht aus den ersten Operanden Liste mit den zweiten Operanden Einträge daraus Diese Liste mit den zweiten Operanden angegeben ist eine ordnungsgemäße zusammenhängenden Unterliste der des ersten.</span><span class="sxs-lookup"><span data-stu-id="411e5-2082">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="411e5-2083">(Um Unterliste auf Gleichheit zu bestimmen, werden entsprechende Einträge verglichen, wie für den Delegaten Equality-Operator ([delegieren Gleichheitsoperatoren](expressions.md#delegate-equality-operators)).) Andernfalls ist das Ergebnis den Wert des linken Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2083">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="411e5-2084">Keiner der Operanden des Bereichsoperators Listen wird im Prozess geändert.</span><span class="sxs-lookup"><span data-stu-id="411e5-2084">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="411e5-2085">Wenn der zweite Operand Liste mehrerer Unterlisten zusammenhängende Einträge aus den ersten Operanden übereinstimmt, wird die am weitesten rechts befindlichen übereinstimmende Unterliste zusammenhängende Einträge entfernt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2085">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="411e5-2086">Wenn die Entfernung in einer leeren Liste führt, wird das Ergebnis ist `null`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2086">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="411e5-2087">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="411e5-2087">For example:</span></span>

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a><span data-ttu-id="411e5-2088">Schiebeoperatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2088">Shift operators</span></span>

<span data-ttu-id="411e5-2089">Die `<<` und `>>` Operatoren werden verwendet, um wechselnden Bit-Vorgänge ausführen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2089">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="411e5-2090">Wenn ein Operand vom eine *Shift_expression* weist den Typ der Kompilierzeit `dynamic`, und klicken Sie dann der Ausdruck dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2090">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="411e5-2091">In diesem Fall den Kompilierzeittyp des Ausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit mit dem Laufzeit-Typ, der diese Operanden mit der Kompilierzeittyp `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2091">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="411e5-2092">Für einen Vorgang des Formulars `x << count` oder `x >> count`, binärer Operator der überladungsauflösung ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2092">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-2093">Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="411e5-2093">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="411e5-2094">Beim Deklarieren eines überladenen Schiebeoperators der Typ des ersten Operanden muss immer der Klasse oder Struktur, die die Operatordeklaration enthält, und der Typ des zweiten Operanden muss immer `int`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2094">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="411e5-2095">Die vordefinierten Shift-Operatoren sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2095">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="411e5-2096">Shift-Left:</span><span class="sxs-lookup"><span data-stu-id="411e5-2096">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="411e5-2097">Die `<<` verschiebt der Operator `x` um eine Anzahl von Bits nach links berechnet werden, wie unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2097">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="411e5-2098">Die höherwertigen Bits außerhalb des Bereichs des Ergebnistyps der `x` werden verworfen, die verbleibenden Bits nach links verschoben, und die niederwertigen leeren Bitpositionen werden auf 0 (null) festgelegt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2098">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="411e5-2099">Verschiebung nach rechts:</span><span class="sxs-lookup"><span data-stu-id="411e5-2099">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="411e5-2100">Die `>>` verschiebt der Operator `x` rechts um eine Anzahl von Bits berechnet, wie unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2100">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="411e5-2101">Wenn `x` ist vom Typ `int` oder `long`, die niederwertigen Bits von `x` werden verworfen, die verbleibenden Bits nach rechts verschoben werden, und die höherwertige leere Bitpositionen werden auf NULL, wenn festgelegt `x` ist negativ und auf, wenn ein `x` ist negativ.</span><span class="sxs-lookup"><span data-stu-id="411e5-2101">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="411e5-2102">Wenn `x` ist vom Typ `uint` oder `ulong`, die niederwertigen Bits von `x` werden verworfen, die verbleibenden Bits werden nach rechts verschoben, und die höherwertige leere Bitpositionen werden auf 0 (null) festgelegt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2102">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="411e5-2103">Die Anzahl der zu verschiebenden Bits wird in der für die vordefinierten Operatoren wird wie folgt berechnet:</span><span class="sxs-lookup"><span data-stu-id="411e5-2103">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="411e5-2104">Wenn der Typ des `x` ist `int` oder `uint`, die Umschaltanzahl durch die niederwertigen fünf Bits des `count`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2104">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="411e5-2105">Das heißt, wird die Umschaltanzahl aus berechnet `count & 0x1F`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2105">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="411e5-2106">Wenn der Typ des `x` ist `long` oder `ulong`, die Umschaltanzahl durch die niederwertigen sechs Bits des `count`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2106">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="411e5-2107">Das heißt, wird die Umschaltanzahl aus berechnet `count & 0x3F`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2107">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="411e5-2108">Ist die UMSCHALT-Rekursionsergebnis 0 (null), die Schiebeoperatoren einfach den Wert des zurückgeben `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2108">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="411e5-2109">Verschiebevorgänge nie lösen Überläufe und erzeugen identische Ergebnisse im `checked` und `unchecked` Kontexte.</span><span class="sxs-lookup"><span data-stu-id="411e5-2109">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="411e5-2110">Wenn der linke Operand des der `>>` Operator einen ganzzahligen Typ mit Vorzeichen, der Operator führt eine arithmetische Verschiebung nach rechts in dem der Wert, der das höchstwertige Bit (das signierte Bit) des Operanden weitergegeben wird, das höherwertige leere Bitpositionen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2110">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="411e5-2111">Wenn der linke Operand des der `>>` Operator ein Integraltyp ohne Vorzeichen, der Operator führt eine logische Verschiebung nach rechts in der höherwertige leere Bitpositionen immer auf 0 (null) festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2111">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="411e5-2112">Um den umgekehrten Vorgang aus, die von der Operandentyp abgeleitet auszuführen, können die explizite Umwandlungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2112">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="411e5-2113">Z. B. wenn `x` ist eine Variable vom Typ `int`, den Vorgang `unchecked((int)((uint)x >> y))` führt eine logische Verschiebung rechts `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2113">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="411e5-2114">Relationale und Typtestoperatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2114">Relational and type-testing operators</span></span>

<span data-ttu-id="411e5-2115">Die `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` und `as` Operatoren werden als relationalen und Typtest Operatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2115">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

<span data-ttu-id="411e5-2116">Die `is` Operator finden Sie im [der is-Operator](expressions.md#the-is-operator) und `as` Operator finden Sie im [den Operator](expressions.md#the-as-operator).</span><span class="sxs-lookup"><span data-stu-id="411e5-2116">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="411e5-2117">Die `==`, `!=`, `<`, `>`, `<=` und `>=` Operatoren sind ***Vergleichsoperatoren***.</span><span class="sxs-lookup"><span data-stu-id="411e5-2117">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="411e5-2118">Wenn ein Operand eines Vergleichsoperators der Kompilierzeit-Typ hat `dynamic`, und klicken Sie dann der Ausdruck dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2118">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="411e5-2119">In diesem Fall den Kompilierzeittyp des Ausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit mit dem Laufzeit-Typ, der diese Operanden mit der Kompilierzeittyp `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2119">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="411e5-2120">Für einen Vorgang des Formulars `x` *Op* `y`, wobei *Op* ein Vergleichsoperator, Auflösung von funktionsüberladungen ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2120">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-2121">Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="411e5-2121">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="411e5-2122">Die vordefinierten Vergleichsoperatoren werden in den folgenden Abschnitten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2122">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="411e5-2123">Alle vordefinierten Vergleichsoperatoren zurückgeben ein Ergebnisses vom Typ `bool`, wie in der folgenden Tabelle beschrieben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2123">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="411e5-2124">__Vorgang__</span><span class="sxs-lookup"><span data-stu-id="411e5-2124">__Operation__</span></span> | <span data-ttu-id="411e5-2125">__Ergebnis__</span><span class="sxs-lookup"><span data-stu-id="411e5-2125">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="411e5-2126">`true` Wenn `x` gleich `y`, `false` andernfalls</span><span class="sxs-lookup"><span data-stu-id="411e5-2126">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="411e5-2127">`true` Wenn `x` ist nicht gleich `y`, `false` andernfalls</span><span class="sxs-lookup"><span data-stu-id="411e5-2127">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="411e5-2128">`true` Wenn `x` ist kleiner als `y`, `false` andernfalls</span><span class="sxs-lookup"><span data-stu-id="411e5-2128">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="411e5-2129">`true` Wenn `x` ist größer als `y`, `false` andernfalls</span><span class="sxs-lookup"><span data-stu-id="411e5-2129">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="411e5-2130">`true` Wenn `x` ist kleiner als oder gleich `y`, `false` andernfalls</span><span class="sxs-lookup"><span data-stu-id="411e5-2130">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="411e5-2131">`true` Wenn `x` ist größer als oder gleich `y`, `false` andernfalls</span><span class="sxs-lookup"><span data-stu-id="411e5-2131">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="411e5-2132">Integer-Vergleichsoperatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2132">Integer comparison operators</span></span>

<span data-ttu-id="411e5-2133">Die vordefinierten Integer-Vergleichsoperatoren sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-2133">The predefined integer comparison operators are:</span></span>
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

<span data-ttu-id="411e5-2134">Die einzelnen Operatoren vergleicht die numerischen Werte der zwei ganzzahlige Operanden und gibt eine `bool` Wert, der angibt, ob die jeweilige Beziehung ist `true` oder `false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2134">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="411e5-2135">Gleitkomma-Vergleichsoperatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2135">Floating-point comparison operators</span></span>

<span data-ttu-id="411e5-2136">Die vordefinierten Gleitkomma-Vergleichsoperatoren sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-2136">The predefined floating-point comparison operators are:</span></span>
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

<span data-ttu-id="411e5-2137">Die Operatoren vergleichen die Operanden gemäß den Regeln der IEEE 754-standard:</span><span class="sxs-lookup"><span data-stu-id="411e5-2137">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="411e5-2138">Wenn einer der Operanden NaN ist, wird das Ergebnis ist `false` für alle Operatoren außer `!=`, für die das Ergebnis ist `true`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2138">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="411e5-2139">Für alle zwei Operanden `x != y` erzeugt immer das gleiche Ergebnis wie `!(x == y)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2139">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="411e5-2140">Wenn einer oder beide Operanden sind jedoch NaN ist, die `<`, `>`, `<=`, und `>=` Operatoren erzeugen die gleichen Ergebnisse wie die logische Negation des den entgegengesetzten Operator nicht.</span><span class="sxs-lookup"><span data-stu-id="411e5-2140">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="411e5-2141">Z. B., wenn entweder der `x` und `y` ist NaN, `x < y` ist `false`, aber `!(x >= y)` ist `true`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2141">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="411e5-2142">Wenn keiner der Operanden NaN ist, vergleichen Sie die Operatoren die Werte von zwei Gleitkommazahlen Operanden in Bezug auf die Reihenfolge</span><span class="sxs-lookup"><span data-stu-id="411e5-2142">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="411e5-2143">wo `min` und `max` sind die kleinstmöglichen und größtmöglichen positiven endlichen Werten, die in der angegebenen Gleitkomma-Format dargestellt werden können.</span><span class="sxs-lookup"><span data-stu-id="411e5-2143">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="411e5-2144">Diese Sortierung wesentliche Auswirkungen sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-2144">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="411e5-2145">Negative und positive Nullen werden als gleich betrachtet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2145">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="411e5-2146">Eine negative Unendlichkeit gilt als kleiner als alle anderen Werte, aber gleich einem anderen minus unendlich.</span><span class="sxs-lookup"><span data-stu-id="411e5-2146">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="411e5-2147">Eine positive Unendlichkeit gilt als größer als alle anderen Werte, aber gleich einem anderen plus unendlich.</span><span class="sxs-lookup"><span data-stu-id="411e5-2147">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="411e5-2148">Decimal-Vergleichsoperatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2148">Decimal comparison operators</span></span>

<span data-ttu-id="411e5-2149">Die vordefinierten decimal Vergleichsoperatoren sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-2149">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="411e5-2150">Die einzelnen Operatoren vergleicht die numerischen Werte der beiden decimal Operanden und gibt eine `bool` Wert, der angibt, ob die jeweilige Beziehung ist `true` oder `false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2150">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="411e5-2151">Jeder dezimale Vergleich ist äquivalent zur Verwendung des entsprechenden relationalen oder Equality-Operator vom Typ `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2151">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="411e5-2152">Boolesche Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2152">Boolean equality operators</span></span>

<span data-ttu-id="411e5-2153">Die vordefinierten booleschen Operatoren sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-2153">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="411e5-2154">Das Ergebnis des `==` ist `true` sowohl `x` und `y` sind `true` oder wenn beide `x` und `y` sind `false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2154">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="411e5-2155">Andernfalls ist das Ergebnis `false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2155">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="411e5-2156">Das Ergebnis des `!=` ist `false` sowohl `x` und `y` sind `true` oder wenn beide `x` und `y` sind `false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2156">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="411e5-2157">Andernfalls ist das Ergebnis `true`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2157">Otherwise, the result is `true`.</span></span> <span data-ttu-id="411e5-2158">Wenn die Operanden sind vom Typ `bool`, `!=` Operator erzeugt das gleiche Ergebnis wie die `^` Operator.</span><span class="sxs-lookup"><span data-stu-id="411e5-2158">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="411e5-2159">Enumeration-Vergleichsoperatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2159">Enumeration comparison operators</span></span>

<span data-ttu-id="411e5-2160">Jeder Enumerationstyp bietet implizit die folgenden vordefinierten Vergleichsoperatoren:</span><span class="sxs-lookup"><span data-stu-id="411e5-2160">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="411e5-2161">Das Ergebnis der Auswertung `x op y`, wobei `x` und `y` sind Ausdrücke eines Enumerationstyps `E` mit einem zugrunde liegenden Typ `U`, und `op` ist eine der folgenden Vergleichsoperatoren, entspricht genau dem Auswerten von `((U)x) op ((U)y)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2161">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="411e5-2162">Das heißt, vergleichen die Enumeration-Typ-Vergleichsoperatoren einfach die zugrunde liegenden ganzzahligen Werte der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2162">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="411e5-2163">Gleichheitsoperatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2163">Reference type equality operators</span></span>

<span data-ttu-id="411e5-2164">Die Gleichheitsoperatoren vordefinierten Typ sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-2164">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="411e5-2165">Die Operatoren geben das Ergebnis des Vergleichs die beiden Verweise auf Gleichheit oder wertungleichheit zurück.</span><span class="sxs-lookup"><span data-stu-id="411e5-2165">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="411e5-2166">Da die vordefinierten Typ Gleichheitsoperatoren Operanden vom Typ akzeptieren `object`, gelten für alle Typen, die keinen entsprechenden deklarieren `operator ==` und `operator !=` Member.</span><span class="sxs-lookup"><span data-stu-id="411e5-2166">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="411e5-2167">Im Gegensatz dazu ausblenden, alle anwendbaren benutzerdefinierte Operatoren effektiv die Gleichheitsoperatoren vordefinierten Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-2167">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="411e5-2168">Die Gleichheitsoperatoren vordefinierten Typ erfordert einen der folgenden:</span><span class="sxs-lookup"><span data-stu-id="411e5-2168">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="411e5-2169">Beide Operanden sind, einen Wert eines Typs, die bekanntermaßen eine *Reference_type* oder das Literal `null`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2169">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="411e5-2170">Darüber hinaus eine explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-reference-conversions)) aus dem Typ der Operanden in den Typ des anderen Operanden vorhanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2170">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="411e5-2171">Einer der Operanden ist ein Wert vom Typ `T` , in denen `T` ist eine *Type_parameter* und der andere Operand ist das Literal `null`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2171">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="411e5-2172">Außerdem `T` verfügt nicht über die werteinschränkung-Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-2172">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="411e5-2173">Wenn eine der folgenden Bedingungen erfüllt sind, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2173">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="411e5-2174">Wesentliche Auswirkungen auf die diese Regeln sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-2174">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="411e5-2175">Es ist ein Fehler während der Bindung, die Gleichheitsoperatoren vordefinierten Typ zu verwenden, um zwei Verweise zu vergleichen, die bekannt sind, zum Zeitpunkt der Bindung unterschiedlich sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-2175">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="411e5-2176">Wenn die Bindung-Time-Typen der Operanden zwei Klassentypen sind z. B. `A` und `B`, und wenn weder `A` noch `B` von der anderen abgeleitet ist, und es unmöglich, dass die beiden Operanden wäre auf dasselbe Objekt verweisen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2176">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="411e5-2177">Daher gilt der Vorgang einen Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2177">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="411e5-2178">Die vordefinierten Typ Gleichheitsoperatoren lassen nicht Wert Operanden vom Typ zu vergleichende.</span><span class="sxs-lookup"><span data-stu-id="411e5-2178">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="411e5-2179">Es sei denn, eine eigene Operatoren ein Strukturtyp deklariert wird, ist es daher nicht möglich, zum Vergleichen von Werten dieses Strukturtyps.</span><span class="sxs-lookup"><span data-stu-id="411e5-2179">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="411e5-2180">Die vordefinierten Typ Gleichheitsoperatoren verursachen nie Boxing-Vorgänge für ihre Operanden sicher auftreten.</span><span class="sxs-lookup"><span data-stu-id="411e5-2180">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="411e5-2181">Es wäre ohne Bedeutung für solche Boxing-Vorgänge ausführen, da Verweise auf das neu zugeordneten geschachtelten Instanzen unbedingt von allen anderen verweisen voneinander abweichen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2181">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="411e5-2182">Wenn ein Operand vom Typ Parametertyp `T` im Vergleich zu `null`, und welche Laufzeit `T` ein Werttyp ist, ist das Ergebnis des Vergleichs `false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2182">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="411e5-2183">Das folgende Beispiel überprüft, ob ein Argument des Typs einer beschränkten Typ-Parameter ist `null`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2183">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="411e5-2184">Die `x == null` Konstrukt ist zulässig, obwohl `T` könnte einen Werttyp darstellen, und das Ergebnis wird lediglich definiert `false` beim `T` ein Werttyp ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2184">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="411e5-2185">Für einen Vorgang des Formulars `x == y` oder `x != y`ggf. alle `operator ==` oder `operator !=` vorhanden ist, handelt es sich bei der überladungsauflösung für Operator ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) Regeln auswählen, werden Operator anstelle der vordefinierten Gleichheitsoperator.</span><span class="sxs-lookup"><span data-stu-id="411e5-2185">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="411e5-2186">Es ist jedoch immer möglich, wählen Sie die vordefinierten Gleichheitsoperator, indem Sie explizit umwandeln, eine oder beide der Operanden in den Typ `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2186">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="411e5-2187">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2187">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
<span data-ttu-id="411e5-2188">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="411e5-2188">produces the output</span></span>
```
True
False
False
False
```

<span data-ttu-id="411e5-2189">Die `s` und `t` Variablen verweisen auf zwei unterschiedliche `string` Instanzen, die dieselben Zeichen enthält.</span><span class="sxs-lookup"><span data-stu-id="411e5-2189">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="411e5-2190">Der erste Vergleich gibt `True` da die vordefinierte Zeichenfolge Equality-Operator ([Zeichenfolge Gleichheitsoperatoren](expressions.md#string-equality-operators)) wird aktiviert, wenn beide Operanden vom Typ sind `string`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2190">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="411e5-2191">Die verbleibenden Vergleiche, die alle Ausgabe `False` , da die vordefinierte Gleichheitsoperator ausgewählt ist, wenn eine oder beide der Operanden des Typs sind `object`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2191">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="411e5-2192">Beachten Sie, dass die oben genannten Technik nicht für Werttypen von Bedeutung ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2192">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="411e5-2193">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2193">The example</span></span>
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
<span data-ttu-id="411e5-2194">Gibt `False` geschachtelt, da die Umwandlungen Verweise auf zwei separate Instanzen des erstellen `int` Werte.</span><span class="sxs-lookup"><span data-stu-id="411e5-2194">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="411e5-2195">Zeichenfolge-Gleichheitsoperatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2195">String equality operators</span></span>

<span data-ttu-id="411e5-2196">Die Gleichheitsoperatoren vordefinierte Zeichenfolge sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-2196">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="411e5-2197">Zwei `string` Werte werden als gleich betrachtet, wenn eine der folgenden Bedingungen zutrifft:</span><span class="sxs-lookup"><span data-stu-id="411e5-2197">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="411e5-2198">Beide Werte sind `null`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2198">Both values are `null`.</span></span>
*  <span data-ttu-id="411e5-2199">Beide Werte sind nicht-Null-Verweise auf String-Instanzen, die dieselbe Länge haben und die identische Zeichen in jeder Position des Zeichens.</span><span class="sxs-lookup"><span data-stu-id="411e5-2199">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="411e5-2200">Die Gleichheitsoperatoren vergleichen Zeichenfolgenwerte anstelle der Zeichenfolge verweisen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2200">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="411e5-2201">Wenn zwei unterschiedliche Instanzen genaue dieselbe Sequenz von Zeichen enthalten, die Werte der Zeichenfolgen gleich sind, aber die Verweise unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2201">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="411e5-2202">Siehe [Gleichheitsoperatoren](expressions.md#reference-type-equality-operators), die Gleichheitsoperatoren können verwendet werden, um Verweise auf Zeichenfolgen anstelle von String-Werten verglichen werden soll.</span><span class="sxs-lookup"><span data-stu-id="411e5-2202">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="411e5-2203">Delegieren von Gleichheitsoperatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2203">Delegate equality operators</span></span>

<span data-ttu-id="411e5-2204">Alle Delegattypen bietet implizit die folgenden vordefinierten Vergleichsoperatoren:</span><span class="sxs-lookup"><span data-stu-id="411e5-2204">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="411e5-2205">Zwei Delegatinstanzen wie folgt gelten als gleich:</span><span class="sxs-lookup"><span data-stu-id="411e5-2205">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="411e5-2206">Wenn eine Delegatinstanz `null`, sie gleich sind, wenn beide `null`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2206">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="411e5-2207">Wenn die Delegaten unterschiedliche Laufzeit-Typinformationen, die sie nicht gleich sind haben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2207">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="411e5-2208">Wenn beide Delegatinstanzen eine Aufrufliste haben ([delegieren Deklarationen](delegates.md#delegate-declarations)), die Instanzen gleich sind, wenn ihre Aufruflisten die gleiche Länge sind, und jeder Eintrag in der Aufrufliste gleich ist (wie unten definiert) mit dem entsprechenden Eintrag in der Reihenfolge, in die andere Aufrufliste.</span><span class="sxs-lookup"><span data-stu-id="411e5-2208">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="411e5-2209">Die folgenden Regeln bestimmen die Gleichheit von Listeneinträgen Aufruf:</span><span class="sxs-lookup"><span data-stu-id="411e5-2209">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="411e5-2210">Wenn zwei Aufruf Listeneinträge sowohl auf die gleiche statische finden Sie unter sind Methode und dann die Einträge gleich.</span><span class="sxs-lookup"><span data-stu-id="411e5-2210">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="411e5-2211">Wenn zwei Aufruf Listeneinträge beide auf dieselbe nicht statische Methode auf dem gleichen Zielobjekt verweisen (wie durch die Gleichheitsoperatoren definiert) klicken Sie dann sind die Einträge gleich.</span><span class="sxs-lookup"><span data-stu-id="411e5-2211">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="411e5-2212">Aufruf Einträge erstellt, bei der Auswertung des semantisch identisch *Anonymous_method_expression*s oder *Lambda_expression*mit den gleichen Satz erfassten äußere Variable (möglicherweise leere) Instanzen sind zulässig (jedoch nicht erforderlich) gleich sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-2212">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="411e5-2213">Operatoren und null</span><span class="sxs-lookup"><span data-stu-id="411e5-2213">Equality operators and null</span></span>

<span data-ttu-id="411e5-2214">Die `==` und `!=` Operatoren ermöglichen einer der Operanden ein als Wert für einen nullable-Typ und die andere, werden die `null` literal, selbst wenn keine vordefinierte oder benutzerdefinierte-Operator (in unlifted oder Formular aufgehoben) für den Vorgang vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2214">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="411e5-2215">Für einen Vorgang von einem der Formate</span><span class="sxs-lookup"><span data-stu-id="411e5-2215">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="411e5-2216">in denen `x` ist ein Ausdruck, der einen nullable-Typ, wenn überladungsauflösung für Operator ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) ein Fehler auftritt, finden Sie einen entsprechenden Operator das Ergebnis wird stattdessen aus berechnet die `HasValue` Eigenschaft des `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2216">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="411e5-2217">Insbesondere werden die ersten beiden Formulare in übersetzt `!x.HasValue`, und die letzten zwei Formen werden in übersetzt `x.HasValue`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2217">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="411e5-2218">Der is-Operator</span><span class="sxs-lookup"><span data-stu-id="411e5-2218">The is operator</span></span>

<span data-ttu-id="411e5-2219">Die `is` Operator wird verwendet, um dynamisch zu überprüfen, ob der Laufzeittyp eines Objekts mit einem angegebenen Typ kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2219">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="411e5-2220">Das Ergebnis des Vorgangs `E is T`, wobei `E` ist ein Ausdruck und `T` ist ein Typ, ist ein boolescher Wert, der angibt, der Wert, ob `E` erfolgreich Typ konvertiert werden kann `T` von einer verweiskonvertierung, einer Boxing Konvertierung oder einer unboxing-Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2220">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="411e5-2221">Die Operation wird wie folgt ausgewertet, nachdem für alle Typparameter Typargumente ersetzt wurden:</span><span class="sxs-lookup"><span data-stu-id="411e5-2221">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="411e5-2222">Wenn `E` wird eine anonyme Funktion, ein Fehler während der Kompilierung tritt ein,</span><span class="sxs-lookup"><span data-stu-id="411e5-2222">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="411e5-2223">Wenn `E` ist eine Methodengruppe oder `null` Zeichenliteral zurück, wenn der Typ des `E` ist ein Verweistyp oder einem nullable-Typ und den Wert der `E` ist null das Ergebnis ist "false".</span><span class="sxs-lookup"><span data-stu-id="411e5-2223">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="411e5-2224">Andernfalls können `D` darstellen den dynamischen Typ der `E` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="411e5-2224">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="411e5-2225">Wenn der Typ des `E` ist ein Verweistyp, `D` ist der Run-Time-Typ, der den Instanzverweis von `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2225">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="411e5-2226">Wenn der Typ des `E` ist ein NULL-Werte zulässt, `D` ist der zugrunde liegende Typ dieses nullable-Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-2226">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="411e5-2227">Wenn der Typ des `E` ist ein NULL-Werttyp, `D` ist der Typ des `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2227">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="411e5-2228">Das Ergebnis des Vorgangs hängt `D` und `T` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="411e5-2228">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="411e5-2229">Wenn `T` ein Verweistyp ist das Ergebnis ist "true" Wenn `D` und `T` desselben Typs sind, wenn `D` ist ein Verweistyp und eine implizite verweiskonvertierung von `D` zu `T` vorhanden ist, oder wenn `D` ist ein Werttyp und eine Boxingkonvertierung von `D` zu `T` vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2229">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="411e5-2230">Wenn `T` ein nullable-Typ ist das Ergebnis ist "true" Wenn `D` ist der zugrunde liegenden Typ des `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2230">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="411e5-2231">Wenn `T` ist ein NULL-Werttyp, das Ergebnis ist True, wenn `D` und `T` denselben Typ aufweisen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2231">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="411e5-2232">Das Ergebnis ist, andernfalls "false".</span><span class="sxs-lookup"><span data-stu-id="411e5-2232">Otherwise, the result is false.</span></span>

<span data-ttu-id="411e5-2233">Beachten Sie, dass benutzerdefinierte Konvertierungen werden nicht berücksichtigt, indem die `is` Operator.</span><span class="sxs-lookup"><span data-stu-id="411e5-2233">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="411e5-2234">Den Operator</span><span class="sxs-lookup"><span data-stu-id="411e5-2234">The as operator</span></span>

<span data-ttu-id="411e5-2235">Die `as` Operator wird verwendet, um explizit einen Wert in einen angegebenen Referenztyp oder ein nullable-Typ zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="411e5-2235">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="411e5-2236">Im Gegensatz zu Cast-Ausdruck ([Umwandlungsausdrücke](expressions.md#cast-expressions)), wird die `as` Operator niemals eine Ausnahme auslöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-2236">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="411e5-2237">Stattdessen ist die angegebene Konvertierung nicht möglich, die der resultierende Wert ist `null`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2237">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="411e5-2238">In einem Vorgang des Formulars `E as T`, `E` muss ein Ausdruck sein und `T` muss ein Verweistyp, ein Typparameter ein Verweistyp oder einem nullable-Typ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2238">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="411e5-2239">Darüber hinaus muss mindestens eine der folgenden "true" sein, oder andernfalls ein Kompilierungsfehler tritt auf:</span><span class="sxs-lookup"><span data-stu-id="411e5-2239">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="411e5-2240">Eine Identität ([identitätskonvertierung](conversions.md#identity-conversion)), implizite NULL-Werte zulässt ([implizite NULL-Werte zulassen Konvertierungen](conversions.md#implicit-nullable-conversions)), ein impliziter Verweis ([implizite Verweis-](conversions.md#implicit-reference-conversions)), Boxing ([ Boxing-Konvertierung](conversions.md#boxing-conversions)), explizite NULL-Werte zulässt ([explizite NULL-Werte zulassen Konvertierungen](conversions.md#explicit-nullable-conversions)), expliziten Verweis ([explizite Konvertierungen](conversions.md#explicit-reference-conversions)), oder durch unboxing ([Unboxing-Konvertierungen](conversions.md#unboxing-conversions)) Konvertierung vorhanden ist, von `E` zu `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2240">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="411e5-2241">Der Typ des `E` oder `T` ist ein offener Typ.</span><span class="sxs-lookup"><span data-stu-id="411e5-2241">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="411e5-2242">`E` ist die `null` literal.</span><span class="sxs-lookup"><span data-stu-id="411e5-2242">`E` is the `null` literal.</span></span>

<span data-ttu-id="411e5-2243">Wenn die Kompilierung von eingeben `E` nicht `dynamic`, den Vorgang `E as T` führt zum gleiche Ergebnis wie</span><span class="sxs-lookup"><span data-stu-id="411e5-2243">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="411e5-2244">außer dass `E` nur einmal überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-2244">except that `E` is only evaluated once.</span></span> <span data-ttu-id="411e5-2245">Der Compiler zur Optimierung erwarten `E as T` höchstens einen Typ "dynamic" Überprüfung statt zwei dynamischen Typprüfungen impliziert durch die Erweiterung, die oben genannten ausführen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2245">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="411e5-2246">Wenn die Kompilierung von eingeben `E` ist `dynamic`, anders als bei der Cast-Operator der `as` Operator ist nicht dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2246">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="411e5-2247">Aus diesem Grund ist die Erweiterung in diesem Fall ein:</span><span class="sxs-lookup"><span data-stu-id="411e5-2247">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="411e5-2248">Beachten Sie, dass einige Konvertierungen wie die benutzerdefinierte Konvertierungen nicht zulässig sind die `as` Operator und stattdessen mithilfe der Cast-Ausdrücke ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="411e5-2248">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="411e5-2249">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2249">In the example</span></span>
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
<span data-ttu-id="411e5-2250">der Typparameter `T` von `G` wird ein Verweistyp sein bezeichnet, da sie die klasseneinschränkung verfügt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2250">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="411e5-2251">Der Typparameter `U` von `H` ist jedoch; nicht daher die Verwendung von der `as` -Operator in `H` ist nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="411e5-2251">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="411e5-2252">Logische Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2252">Logical operators</span></span>

<span data-ttu-id="411e5-2253">Die `&`, `^`, und `|` Operatoren werden als die logischen Operatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2253">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

<span data-ttu-id="411e5-2254">Wenn ein Operand vom ein logischer Operator, der während der Kompilierung-Typ hat `dynamic`, und klicken Sie dann der Ausdruck dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2254">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="411e5-2255">In diesem Fall den Kompilierzeittyp des Ausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit mit dem Laufzeit-Typ, der diese Operanden mit der Kompilierzeittyp `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2255">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="411e5-2256">Für einen Vorgang des Formulars `x op y`, wobei `op` ist eine logische Operatoren, Auflösung von funktionsüberladungen ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2256">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="411e5-2257">Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="411e5-2257">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="411e5-2258">Die vordefinierten logische Operatoren werden in den folgenden Abschnitten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2258">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="411e5-2259">Logische Operatoren für ganze Zahlen</span><span class="sxs-lookup"><span data-stu-id="411e5-2259">Integer logical operators</span></span>

<span data-ttu-id="411e5-2260">Die vordefinierten Ganzzahl logischen Operatoren sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-2260">The predefined integer logical operators are:</span></span>
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

<span data-ttu-id="411e5-2261">Die `&` -Operator berechnet das bitweise logische `AND` der zwei Operanden, die `|` -Operator berechnet das bitweise logische `OR` der zwei Operanden, und die `^` -Operator berechnet das bitweise logische XOR `OR` der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2261">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="411e5-2262">Keine Überläufe sind von diesen Vorgängen möglich.</span><span class="sxs-lookup"><span data-stu-id="411e5-2262">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="411e5-2263">Logische Enumerationsoperatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2263">Enumeration logical operators</span></span>

<span data-ttu-id="411e5-2264">Jeder Enumerationstyp `E` implizit bietet die folgenden vordefinierten logische Operatoren:</span><span class="sxs-lookup"><span data-stu-id="411e5-2264">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="411e5-2265">Das Ergebnis der Auswertung `x op y`, wobei `x` und `y` sind Ausdrücke eines Enumerationstyps `E` mit einem zugrunde liegenden Typ `U`, und `op` ist einer der logischen Operatoren, entspricht genau dem Auswerten von `(E)((U)x op (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2265">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="411e5-2266">Das heißt, führen Sie die logischen Operatoren für die Enumeration einfach die logische Operation mit den zugrunde liegenden Typ, der zwei Operanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2266">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="411e5-2267">Logische boolesche Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2267">Boolean logical operators</span></span>

<span data-ttu-id="411e5-2268">Die vordefinierten booleschen logischen Operatoren sind:</span><span class="sxs-lookup"><span data-stu-id="411e5-2268">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="411e5-2269">Das Ergebnis von `x & y` ist `true`, wenn sowohl `x` als auch `y` zu `true` ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2269">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="411e5-2270">Andernfalls ist das Ergebnis `false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2270">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="411e5-2271">Das Ergebnis des `x | y` ist `true` Wenn `x` oder `y` ist `true`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2271">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="411e5-2272">Andernfalls ist das Ergebnis `false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2272">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="411e5-2273">Das Ergebnis des `x ^ y` ist `true` Wenn `x` ist `true` und `y` ist `false`, oder `x` ist `false` und `y` ist `true`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2273">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="411e5-2274">Andernfalls ist das Ergebnis `false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2274">Otherwise, the result is `false`.</span></span> <span data-ttu-id="411e5-2275">Wenn die Operanden sind vom Typ `bool`, `^` -Operator berechnet das gleiche Ergebnis wie die `!=` Operator.</span><span class="sxs-lookup"><span data-stu-id="411e5-2275">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="411e5-2276">Auf NULL festlegbaren booleschen logischen Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2276">Nullable boolean logical operators</span></span>

<span data-ttu-id="411e5-2277">Der auf NULL festlegbaren booleschen Typ `bool?` kann drei Werte darstellen `true`, `false`, und `null`, und ist konzeptionell identisch mit den drei Werten-Typ für boolesche Ausdrücke, die in SQL verwendet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2277">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="411e5-2278">Um sicherzustellen, dass die Ergebnisse von den Ergebnissen der `&` und `|` Operatoren für `bool?` Operanden sind konsistent mit der SQL dreiwertige Logik, die folgenden vordefinierten Operatoren werden bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="411e5-2278">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="411e5-2279">Die folgende Tabelle enthält die Ergebnisse dieser Operatoren für den Werten aller Kombinationen von `true`, `false`, und `null`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2279">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a><span data-ttu-id="411e5-2280">Bedingte logische Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2280">Conditional logical operators</span></span>

<span data-ttu-id="411e5-2281">Die `&&` und `||` Operatoren werden als die bedingten logischen Operatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2281">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="411e5-2282">Sie werden auch die "verkürzte" logischen Operatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2282">They are also called the "short-circuiting" logical operators.</span></span>

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

<span data-ttu-id="411e5-2283">Die `&&` und `||` Operatoren sind bedingte Versionen der `&` und `|` Operatoren:</span><span class="sxs-lookup"><span data-stu-id="411e5-2283">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="411e5-2284">Der Vorgang `x && y` entspricht dem Vorgang `x & y`, außer dass `y` nur ausgewertet, wenn `x` nicht `false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2284">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="411e5-2285">Der Vorgang `x || y` entspricht dem Vorgang `x | y`, außer dass `y` nur ausgewertet, wenn `x` nicht `true`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2285">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="411e5-2286">Wenn ein Operand eines bedingten logischen Operators der Kompilierzeit-Typ hat `dynamic`, und klicken Sie dann der Ausdruck dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2286">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="411e5-2287">In diesem Fall den Kompilierzeittyp des Ausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit mit dem Laufzeit-Typ, der diese Operanden mit der Kompilierzeittyp `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2287">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="411e5-2288">Ein Vorgang des Formulars `x && y` oder `x || y` wird durch Anwenden der Auflösung von funktionsüberladungen verarbeitet ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)), als ob der Vorgang geschrieben wurde `x & y` oder `x | y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2288">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="411e5-2289">Klicken Sie dann</span><span class="sxs-lookup"><span data-stu-id="411e5-2289">Then,</span></span>

*  <span data-ttu-id="411e5-2290">Wenn Auflösung von funktionsüberladungen finden einen einzelnen bewährte Operator oder wählt die überladungsauflösung eine der vordefinierten Ganzzahl logischen Operatoren, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2290">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="411e5-2291">Andernfalls, wenn der ausgewählte Operator, eine der vordefinierten booleschen logische Operatoren ist ([Boolesche logische Operatoren](expressions.md#boolean-logical-operators)) oder auf NULL festlegbaren booleschen logischen Operatoren ([auf NULL festlegbaren booleschen logischen Operatoren](expressions.md#nullable-boolean-logical-operators)), wird die Vorgang wird verarbeitet, wie in beschrieben [booleschen bedingten logischen Operatoren](expressions.md#boolean-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="411e5-2291">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="411e5-2292">Andernfalls der ausgewählte Operator ist ein benutzerdefinierter Operator und der Vorgang verarbeitet wird, wie in beschrieben [bedingten logischen Operatoren eine benutzerdefinierte](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="411e5-2292">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="411e5-2293">Es ist nicht möglich, direkt die bedingten logischen Operatoren überladen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2293">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="411e5-2294">Aber da die bedingten logischen Operatoren in Bezug auf die regulären logischen Operatoren ausgewertet werden, gelten Überladungen der regulären logischen Operatoren mit gewissen Einschränkungen auch Überladungen der bedingten logischen Operatoren als.</span><span class="sxs-lookup"><span data-stu-id="411e5-2294">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="411e5-2295">Dies wird weiter in [bedingten logischen Operatoren eine benutzerdefinierte](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="411e5-2295">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="411e5-2296">Boolesche bedingte logische Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2296">Boolean conditional logical operators</span></span>

<span data-ttu-id="411e5-2297">Wenn die Operanden des `&&` oder `||` sind vom Typ `bool`, oder wenn die Operanden sind Typen, die keinem zutreffenden definieren `operator &` oder `operator |`, aber implizite Konvertierungen in definieren `bool`, der Vorgang ist wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="411e5-2297">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="411e5-2298">Der Vorgang `x && y` wird als ausgewertet, `x ? y : false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2298">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="411e5-2299">Das heißt, `x` wird zuerst ausgewertet und in den Typ konvertiert `bool`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2299">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="411e5-2300">Wenn danach `x` ist `true`, `y` ausgewertet und in den Typ konvertiert `bool`, und dies ist das Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="411e5-2300">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="411e5-2301">Andernfalls ist das Ergebnis des Vorgangs `false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2301">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="411e5-2302">Der Vorgang `x || y` wird als ausgewertet, `x ? true : y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2302">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="411e5-2303">Das heißt, `x` wird zuerst ausgewertet und in den Typ konvertiert `bool`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2303">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="411e5-2304">Wenn danach `x` ist `true`, ist das Ergebnis des Vorgangs `true`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2304">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="411e5-2305">Andernfalls `y` ausgewertet und in den Typ konvertiert `bool`, und dies ist das Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="411e5-2305">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="411e5-2306">Eine benutzerdefinierte bedingten logischen Operatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2306">User-defined conditional logical operators</span></span>

<span data-ttu-id="411e5-2307">Wenn die Operanden des `&&` oder `||` sind Typen, die dem zutreffenden deklarieren Sie eine benutzerdefinierte `operator &` oder `operator |`, die folgenden beiden muss "true", in denen `T` ist der Typ, in dem der ausgewählte Operator deklariert wird:</span><span class="sxs-lookup"><span data-stu-id="411e5-2307">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="411e5-2308">Der Rückgabetyp und den Typ jedes Parameters, der den ausgewählten Operator muss `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2308">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="411e5-2309">Das heißt, muss der Operator die logische compute `AND` oder die logische `OR` der beiden Operanden vom Typ `T`, und ein Ergebnis vom Typ zurückgeben müssen `T`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2309">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="411e5-2310">`T` Deklarationen von darf `operator true` und `operator false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2310">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="411e5-2311">Ein Fehler während der Bindung tritt auf, wenn der Anforderungen nicht erfüllt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-2311">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="411e5-2312">Andernfalls die `&&` oder `||` Operation wird ausgewertet, durch die Kombination der benutzerdefinierte `operator true` oder `operator false` mit den ausgewählten benutzerdefinierten Operator:</span><span class="sxs-lookup"><span data-stu-id="411e5-2312">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="411e5-2313">Der Vorgang `x && y` wird als ausgewertet, `T.false(x) ? x : T.&(x, y)`, wobei `T.false(x)` ist ein Aufruf von der `operator false` in deklarierten `T`, und `T.&(x, y)` ist ein Aufruf des ausgewählten `operator &`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2313">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="411e5-2314">Das heißt, `x` wird zuerst ausgewertet und `operator false` wird aufgerufen, auf das Ergebnis aus, um zu bestimmen, ob `x` ist definitiv "false".</span><span class="sxs-lookup"><span data-stu-id="411e5-2314">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="411e5-2315">Wenn danach `x` ist definitiv "false" werden, das Ergebnis des Vorgangs ist der Wert, der zuvor für berechneten `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2315">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="411e5-2316">Andernfalls `y` ausgewertet wird, und der ausgewählten `operator &` wird aufgerufen, für die zuvor für `x` und den berechneten Wert für `y` um das Ergebnis des Vorgangs zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2316">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="411e5-2317">Der Vorgang `x || y` wird als ausgewertet, `T.true(x) ? x : T.|(x, y)`, wobei `T.true(x)` ist ein Aufruf von der `operator true` in deklarierten `T`, und `T.|(x,y)` ist ein Aufruf des ausgewählten `operator|`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2317">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="411e5-2318">Das heißt, `x` wird zuerst ausgewertet und `operator true` wird aufgerufen, auf das Ergebnis aus, um zu bestimmen, ob `x` auf jeden Fall ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2318">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="411e5-2319">Wenn danach `x` auf jeden Fall true ist, das Ergebnis des Vorgangs ist der Wert, der zuvor für berechneten `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2319">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="411e5-2320">Andernfalls `y` ausgewertet wird, und der ausgewählten `operator |` wird aufgerufen, für die zuvor für `x` und den berechneten Wert für `y` um das Ergebnis des Vorgangs zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2320">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="411e5-2321">In einer dieser Vorgänge, die der angegebene Ausdruck `x` ist nur einmal ausgewertet, und der angegebene Ausdruck `y` wird nicht ausgewertet oder genau einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2321">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="411e5-2322">Ein Beispiel für einen Typ, der implementiert `operator true` und `operator false`, finden Sie unter [Datenbank booleschen Typ](structs.md#database-boolean-type).</span><span class="sxs-lookup"><span data-stu-id="411e5-2322">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="411e5-2323">Die null-Sammeloperator</span><span class="sxs-lookup"><span data-stu-id="411e5-2323">The null coalescing operator</span></span>

<span data-ttu-id="411e5-2324">Die `??` Operator, der null-Sammeloperator aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-2324">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="411e5-2325">Ein null-sammelausdrücken im Format `a ?? b` erfordert `a` vom einen Nullable-Typ oder Verweis-Typ sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-2325">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="411e5-2326">Wenn `a` ungleich Null sind, ist das Ergebnis des `a ?? b` ist `a`ist, andernfalls ist das Ergebnis `b`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2326">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="411e5-2327">Der Vorgang ermittelt `b` nur, wenn `a` ist null.</span><span class="sxs-lookup"><span data-stu-id="411e5-2327">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="411e5-2328">Die null-Sammeloperator ist rechtsassoziativ, was bedeutet, dass Vorgänge, die von rechts nach links gruppiert werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2328">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="411e5-2329">Z. B. ein Ausdruck der Form `a ?? b ?? c` wird als ausgewertet, `a ?? (b ?? c)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2329">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="411e5-2330">Im allgemeinen Begriffe, einen Ausdruck der Form `E1 ?? E2 ?? ... ?? En` gibt den ersten Operanden, der ist ungleich Null verwendet werden soll, oder null, wenn alle Operanden null sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-2330">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="411e5-2331">Der Typ des Ausdrucks `a ?? b` hängt von der impliziten Konvertierungen auf die Operanden verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-2331">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="411e5-2332">In der Reihenfolge ihrer Priorität, die den Typ des `a ?? b` ist `A0`, `A`, oder `B`, wobei `A` ist der Typ des `a` (vorausgesetzt, dass `a` verfügt über einen Typ), `B` ist der Typ des `b` () vorausgesetzt, dass `b` verfügt über einen Typ), und `A0` ist der zugrunde liegenden Typ des `A` Wenn `A` ist ein NULL-Werte zulässt, oder `A` andernfalls.</span><span class="sxs-lookup"><span data-stu-id="411e5-2332">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="411e5-2333">Insbesondere `a ?? b` wird wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="411e5-2333">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="411e5-2334">Wenn `A` vorhanden ist und keinen nullable-Typ oder ein Verweistyp, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2334">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="411e5-2335">Wenn `b` ein dynamisches Ausdrucks, ist der Ergebnistyp ist `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2335">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="411e5-2336">Zur Laufzeit `a` wird zuerst ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2336">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="411e5-2337">Wenn `a` ist nicht null, `a` wird konvertiert in einen dynamischen, und dies ist das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-2337">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="411e5-2338">Andernfalls `b` ausgewertet wird, und dies ist das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-2338">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="411e5-2339">Andernfalls gilt: Wenn `A` vorhanden ist und ein nullable-Typ und eine implizite Konvertierung vorhanden ist aus `b` zu `A0`, der Ergebnistyp ist `A0`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2339">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="411e5-2340">Zur Laufzeit `a` wird zuerst ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2340">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="411e5-2341">Wenn `a` ist nicht null, `a` wird entpackt, um geben `A0`, und dies ist das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-2341">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="411e5-2342">Andernfalls `b` ausgewertet und in den Typ konvertiert `A0`, und dies ist das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-2342">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="411e5-2343">Andernfalls gilt: Wenn `A` vorhanden ist und eine implizite Konvertierung von `b` zu `A`, ist der Ergebnistyp `A`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2343">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="411e5-2344">Zur Laufzeit `a` wird zuerst ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2344">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="411e5-2345">Wenn `a` ist nicht null, `a` wird das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-2345">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="411e5-2346">Andernfalls `b` ausgewertet und in den Typ konvertiert `A`, und dies ist das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-2346">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="411e5-2347">Andernfalls gilt: Wenn `b` weist einen Typ `B` und eine implizite Konvertierung von `a` zu `B`, der Ergebnistyp ist `B`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2347">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="411e5-2348">Zur Laufzeit `a` wird zuerst ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2348">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="411e5-2349">Wenn `a` ist nicht null, `a` wird entpackt, um geben `A0` (Wenn `A` vorhanden ist, und NULL-Werte zulässt) und in den Typ konvertiert `B`, und dies ist das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-2349">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="411e5-2350">Andernfalls `b` ausgewertet wird und das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-2350">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="411e5-2351">Andernfalls `a` und `b` nicht kompatibel sind und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2351">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="411e5-2352">Bedingter Operator</span><span class="sxs-lookup"><span data-stu-id="411e5-2352">Conditional operator</span></span>

<span data-ttu-id="411e5-2353">Die `?:` Operator wird den bedingten Operator aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2353">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="411e5-2354">Sie wird gelegentlich auch den ternären Operator aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2354">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="411e5-2355">Ein bedingter Ausdruck der Form `b ? x : y` zuerst wertet die Bedingung `b`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2355">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="411e5-2356">Wenn danach `b` ist `true`, `x` ausgewertet wird und das Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="411e5-2356">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="411e5-2357">Andernfalls `y` ausgewertet wird und das Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="411e5-2357">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="411e5-2358">Ein bedingter Ausdruck ist nie ergibt, beide `x` und `y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2358">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="411e5-2359">Der bedingte Operator ist rechtsassoziativ, was bedeutet, dass Vorgänge, die von rechts nach links gruppiert werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2359">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="411e5-2360">Z. B. ein Ausdruck der Form `a ? b : c ? d : e` wird als ausgewertet, `a ? b : (c ? d : e)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2360">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="411e5-2361">Der erste Operand der `?:` Operator muss ein Ausdruck, der implizit in konvertiert werden kann `bool`, oder ein Ausdruck, der ein Typ, der implementiert `operator true`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2361">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="411e5-2362">Wenn keine dieser Anforderungen nicht erfüllt ist, tritt ein Fehler während der Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2362">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="411e5-2363">Die zweiten und dritten Operanden, `x` und `y`, der die `?:` -Operators bestimmen den Typ des bedingten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="411e5-2363">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="411e5-2364">Wenn `x` weist den Typ `X` und `y` weist den Typ `Y` dann</span><span class="sxs-lookup"><span data-stu-id="411e5-2364">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="411e5-2365">Wenn eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) vorhanden ist, von `X` zu `Y`, jedoch nicht von `Y` zu `X`, klicken Sie dann `Y` ist der Typ des bedingten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="411e5-2365">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="411e5-2366">Wenn eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) vorhanden ist, von `Y` zu `X`, jedoch nicht von `X` zu `Y`, klicken Sie dann `X` ist der Typ des bedingten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="411e5-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="411e5-2367">Andernfalls keine Ausdruckstyp bestimmt werden kann, und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2367">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="411e5-2368">Wenn nur eine der `x` und `y` verfügt über einen Typ und beide `x` und `y`, der implizit in diesen Typ konvertiert werden, dann wird der Typ des bedingten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="411e5-2368">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="411e5-2369">Andernfalls keine Ausdruckstyp bestimmt werden kann, und ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2369">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="411e5-2370">Die Run-Time-Verarbeitung von einem bedingten Ausdruck der Form `b ? x : y` umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-2370">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="411e5-2371">Zuerst `b` ausgewertet wird, und die `bool` Wert `b` bestimmt wird:</span><span class="sxs-lookup"><span data-stu-id="411e5-2371">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="411e5-2372">Wenn eine implizite Konvertierung vom Typ des `b` zu `bool` vorhanden ist, und klicken Sie dann diese implizite Konvertierung ausgeführt wird, erzeugt eine `bool` Wert.</span><span class="sxs-lookup"><span data-stu-id="411e5-2372">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="411e5-2373">Andernfalls die `operator true` definiert durch den Typ der `b` wird aufgerufen, um zu erzeugen eine `bool` Wert.</span><span class="sxs-lookup"><span data-stu-id="411e5-2373">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="411e5-2374">Wenn die `bool` ist der Wert, der vom vorherigen Schritt generiert `true`, klicken Sie dann `x` ausgewertet und konvertiert Sie in den Typ des bedingten Ausdrucks, und dies ist das Ergebnis des bedingten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="411e5-2374">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="411e5-2375">Andernfalls `y` ausgewertet und konvertiert Sie in den Typ des bedingten Ausdrucks, und dies ist das Ergebnis des bedingten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="411e5-2375">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="411e5-2376">Anonyme Funktionsausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-2376">Anonymous function expressions</span></span>

<span data-ttu-id="411e5-2377">Ein ***anonyme Funktion*** ist ein Ausdruck, der eine "inline" Methodendefinition darstellt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2377">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="411e5-2378">Eine anonyme Funktion verfügt nicht über einen Wert oder einen Typ in und von sich selbst, aber in einen kompatiblen Delegat- oder ausdrucksbaumtyp konvertierbar ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2378">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="411e5-2379">Die Auswertung einer anonymen Funktion Konvertierung hängt von den Zieltyp der Konvertierung ab: Wenn sie einen Delegattyp ist, ergibt die Konvertierung eines Delegatwerts verweisen auf die Methode, die die anonyme Funktion definiert.</span><span class="sxs-lookup"><span data-stu-id="411e5-2379">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="411e5-2380">Wenn sie einen Typ für die Ausdrucksbaumstruktur ist, ergibt sich die Konvertierung in eine Ausdrucksbaumstruktur, die die Struktur der Methode, wie eine Objektstruktur darstellt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2380">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="411e5-2381">Aus Verlaufsgründen es gibt zwei syntaktische Varianten von anonymen Funktionen, d. h. *Lambda_expression*s und *Anonymous_method_expression*s.</span><span class="sxs-lookup"><span data-stu-id="411e5-2381">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="411e5-2382">Fast alle Gründen *Lambda_expression*s sind präziser und ausdrucksbasierte als *Anonymous_method_expression*s, die in der Sprache für die Abwärtskompatibilität Kompatibilität bleiben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2382">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

<span data-ttu-id="411e5-2383">Der Operator `=>` verfügt über die gleiche Rangfolge wie die Zuweisung (`=`) und ist rechtsassoziativ.</span><span class="sxs-lookup"><span data-stu-id="411e5-2383">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="411e5-2384">Eine anonyme Funktion mit dem `async` -Modifizierer ist eine asynchrone Funktion und die in beschriebenen Regeln folgt [Iteratoren](classes.md#iterators).</span><span class="sxs-lookup"><span data-stu-id="411e5-2384">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="411e5-2385">Die Parameter einer anonymen Funktion in Form einer *Lambda_expression* können explizit oder implizit typisiert werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2385">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="411e5-2386">In der Parameterliste eine explizit typisierte ist der Typ jedes Parameters explizit angegeben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2386">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="411e5-2387">In einer implizit typisierten Parameterliste, die Typen der Parameter aus dem Kontext, in dem die anonyme Funktion tritt auf, abgeleitet werden – insbesondere, wenn die anonyme Funktion konvertiert wird auf einen kompatiblen Delegaten-Typ oder den Typ für die Ausdrucksbaumstruktur, den bereitstellt die Parametertypen ([anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2387">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="411e5-2388">In einer anonymen Funktion mit einem einzigen, implizit typisierte Parameter können die Klammern aus der Liste ausgelassen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2388">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="411e5-2389">Das heißt, eine anonyme Funktion des Formulars</span><span class="sxs-lookup"><span data-stu-id="411e5-2389">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="411e5-2390">kann abgekürzt werden zu</span><span class="sxs-lookup"><span data-stu-id="411e5-2390">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="411e5-2391">Der Parameterliste eine anonyme Funktion in Form einer *Anonymous_method_expression* ist optional.</span><span class="sxs-lookup"><span data-stu-id="411e5-2391">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="411e5-2392">Wenn angegeben, müssen die Parameter explizit typisiert sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-2392">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="411e5-2393">Wenn nicht der Fall, die anonyme Funktion konvertiert werden kann, um einen Delegaten mit einer der Parameter ist aufgelistet, die nicht enthalten `out` Parameter.</span><span class="sxs-lookup"><span data-stu-id="411e5-2393">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="411e5-2394">Ein *Block* Text einer anonymen Funktion erreichbar ist ([Endpunkte und Erreichbarkeit](statements.md#end-points-and-reachability)), wenn die anonyme Funktion in einer nicht erreichbar-Anweisung auftritt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2394">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="411e5-2395">Einige Beispiele für anonyme Funktionen führen Sie die folgenden:</span><span class="sxs-lookup"><span data-stu-id="411e5-2395">Some examples of anonymous functions follow below:</span></span>

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

<span data-ttu-id="411e5-2396">Das Verhalten der *Lambda_expression*s und *Anonymous_method_expression*s ist gleichermaßen, weisen die folgenden Punkte:</span><span class="sxs-lookup"><span data-stu-id="411e5-2396">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="411e5-2397">*Anonymous_method_expression*s zulassen die Parameterliste komplett weggelassen werden, stellt die Konvertierung zu einer Liste von Wertparameter Delegattypen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2397">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="411e5-2398">*Lambda_expression*s zulassen, Parametertypen, die nicht angegeben werden, und abgeleitet werden, während *Anonymous_method_expression*erfordern Parametertypen explizit angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2398">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="411e5-2399">Der Text der ein *Lambda_expression* kann ein Ausdruck oder einen Anweisungsblock sein werden, während der Text der ein *Anonymous_method_expression* muss ein Anweisungsblock sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-2399">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="411e5-2400">Nur *Lambda_expression*s haben Konvertierungen in kompatibel ausdrucksbaumstrukturtypen ([ausdrucksbaumstrukturtypen](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2400">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="411e5-2401">Anonyme Funktionssignaturen</span><span class="sxs-lookup"><span data-stu-id="411e5-2401">Anonymous function signatures</span></span>

<span data-ttu-id="411e5-2402">Der optionale *Anonymous_function_signature* eine anonyme Funktion definiert, die Namen und optional auch die Typen der formalen Parameter für die anonyme Funktion.</span><span class="sxs-lookup"><span data-stu-id="411e5-2402">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="411e5-2403">Der Bereich der Parameter für die anonyme Funktion ist die *Anonymous_function_body*.</span><span class="sxs-lookup"><span data-stu-id="411e5-2403">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="411e5-2404">([Bereiche](basic-concepts.md#scopes)) zusammen mit der Parameterliste (sofern vorhanden) bildet die anonyme-Methode-Body einen Deklarationsabschnitt ([Deklarationen](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2404">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="411e5-2405">Es ist daher ein Fehler während der Kompilierung für den Namen eines Parameters, der anonyme Funktion, die mit dem Namen des eine lokale Variable, die lokale Konstante oder die Parameter, deren Bereich umfasst, die *Anonymous_method_expression* oder *Lambda_ Ausdruck*.</span><span class="sxs-lookup"><span data-stu-id="411e5-2405">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="411e5-2406">Wenn eine anonyme Funktion verfügt über eine *Explicit_anonymous_function_signature*, und klicken Sie dann der Satz von kompatible Delegattypen und ausdrucksbaumstrukturtypen auf diejenigen beschränkt ist, die den gleichen Parametertypen und -Modifizierern in der gleichen Reihenfolge aufweisen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2406">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="411e5-2407">Im Gegensatz zu Group Konvertierungen ([Gruppe Konvertierungen](conversions.md#method-group-conversions)), Kontravarianz von Parametertypen der anonymen Funktion wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2407">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="411e5-2408">Wenn eine anonyme Funktion kein *Anonymous_function_signature*, und klicken Sie dann der Satz von kompatible Delegattypen und ausdrucksbaumstrukturtypen auf diejenigen beschränkt ist, die keine `out` Parameter.</span><span class="sxs-lookup"><span data-stu-id="411e5-2408">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="411e5-2409">Beachten Sie, dass ein *Anonymous_function_signature* darf keine Attribute oder ein Parameterarray.</span><span class="sxs-lookup"><span data-stu-id="411e5-2409">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="411e5-2410">Dennoch eine *Anonymous_function_signature* kann mit einem Delegattyp, dessen Parameterliste ein Parameterarray enthält, kompatibel sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-2410">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="411e5-2411">Beachten Sie auch diese Konvertierung in einen Typ für die Ausdrucksbaumstruktur, selbst wenn kompatibel, kann immer noch ausfallen, zum Zeitpunkt der Kompilierung ([ausdrucksbaumstrukturtypen](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2411">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="411e5-2412">Anonyme funktionsrümpfe</span><span class="sxs-lookup"><span data-stu-id="411e5-2412">Anonymous function bodies</span></span>

<span data-ttu-id="411e5-2413">Der Text (*Ausdruck* oder *Block*) eine anonyme Funktion unterliegt den folgenden Regeln:</span><span class="sxs-lookup"><span data-stu-id="411e5-2413">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="411e5-2414">Wenn die anonyme Funktion eine Signatur enthält, sind die Parameter, die in der Signatur angegeben im Text zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2414">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="411e5-2415">Wenn die anonyme Funktion keine Signatur hat es konvertiert werden kann einem Delegattyp bzw. den Ausdruckstyp Parametern ([anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions)), aber der Parameter können nicht zugegriffen werden, im Text.</span><span class="sxs-lookup"><span data-stu-id="411e5-2415">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="411e5-2416">Mit Ausnahme von `ref` oder `out` Parameter angegeben wird, in der Signatur (sofern vorhanden) der nächsten einschließenden anonyme Funktion wird ein Fehler während der Kompilierung für den Text, den Zugriff auf eine `ref` oder `out` Parameter.</span><span class="sxs-lookup"><span data-stu-id="411e5-2416">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="411e5-2417">Wenn der Typ des `this` ist ein Strukturtyp, es ist ein Fehler während der Kompilierung für den Text, den Zugriff auf `this`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2417">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="411e5-2418">Dies ist "true" gibt an, ob der Zugriff explizit ist (wie in `this.x`) oder implizit (wie in `x` , in denen `x` ist ein Instanzmember der Struktur).</span><span class="sxs-lookup"><span data-stu-id="411e5-2418">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="411e5-2419">Diese Regel verhindert, dass dieser Zugriff einfach und wirkt sich nicht, ob die Suche nach Membern in ein Member der Struktur ergibt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2419">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="411e5-2420">Der Text hat Zugriff auf die äußeren Variablen ([äußeren Variablen](expressions.md#outer-variables)) der anonymen Funktion.</span><span class="sxs-lookup"><span data-stu-id="411e5-2420">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="411e5-2421">Zugriff auf eine äußere Variable verweist auf die Instanz der Variablen, die zu dem Zeitpunkt aktiv ist die *Lambda_expression* oder *Anonymous_method_expression* ausgewertet wird ([Auswertung anonyme Funktionsausdrücke](expressions.md#evaluation-of-anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2421">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="411e5-2422">Es ist ein Fehler während der Kompilierung für den Textkörper enthält ein `goto` -Anweisung `break` -Anweisung oder `continue` -Anweisung ein, deren Ziel außerhalb des Texts oder im Text einer enthaltenen anonymen Funktion.</span><span class="sxs-lookup"><span data-stu-id="411e5-2422">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="411e5-2423">Ein `return` -Anweisung im Hauptteil übergibt die Kontrolle über einen Aufruf der nächsten einschließenden anonyme Funktion, die nicht aus der einschließenden Funktionsmember.</span><span class="sxs-lookup"><span data-stu-id="411e5-2423">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="411e5-2424">Ein im angegebenen Ausdruck eine `return` -Anweisung muss implizit in den Rückgabetyp des Delegattyps oder Typ, der für die Ausdrucksbaumstruktur sein der nächsten einschließenden *Lambda_expression* oder *Anonymous_ Method_expression* konvertiert wird ([anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2424">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="411e5-2425">Ist nicht explizit angegeben, ob es eine Möglichkeit gibt, führen Sie den Block, eine anonyme Funktion außer durch Auswertung und der Aufruf der *Lambda_expression* oder *Anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="411e5-2425">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="411e5-2426">Insbesondere kann der Compiler so implementieren Sie eine anonyme Funktion synthetisieren eine oder mehrere benannte Methoden und Typen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2426">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="411e5-2427">Die Namen dieser alle synthetischen Elemente müssen eines Formulars, das für die Verwendung durch den Compiler reserviert sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-2427">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="411e5-2428">Auflösung von funktionsüberladungen und anonymen Funktionen</span><span class="sxs-lookup"><span data-stu-id="411e5-2428">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="411e5-2429">Anonyme Funktionen in einer Argumentliste Typrückschluss teilnehmen und Auflösen der Überladung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2429">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="411e5-2430">Näheres [Typrückschluss](expressions.md#type-inference) und [Überladungsauflösung](expressions.md#overload-resolution) die genauen Regeln.</span><span class="sxs-lookup"><span data-stu-id="411e5-2430">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="411e5-2431">Das folgende Beispiel veranschaulicht die Auswirkungen von anonymen Funktionen auf überladungsauflösung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2431">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

<span data-ttu-id="411e5-2432">Die `ItemList<T>` -Klasse verfügt über zwei `Sum` Methoden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2432">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="411e5-2433">Jede hat eine `selector` -Argument, das der Wert auf Summe von einem Listenelement extrahiert.</span><span class="sxs-lookup"><span data-stu-id="411e5-2433">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="411e5-2434">Der extrahierte Wert kann entweder ein `int` oder `double` und die resultierende Summe ist ebenso entweder ein `int` oder ein `double`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2434">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="411e5-2435">Die `Sum` Methoden können z. B. zum Berechnen von Summen aus einer Liste der Detailzeilen in einer Reihenfolge verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2435">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

<span data-ttu-id="411e5-2436">In den ersten Aufruf des `orderDetails.Sum`, beide `Sum` Methoden gelten. da die anonyme Funktion `d => d. UnitCount` ist kompatibel mit sowohl `Func<Detail,int>` und `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2436">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="411e5-2437">Auflösung von funktionsüberladungen wählt jedoch die erste `Sum` Methode da die Konvertierung in `Func<Detail,int>` ist besser als die Konvertierung zu `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2437">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="411e5-2438">Im zweiten Aufruf von `orderDetails.Sum`, nur der zweite `Sum` Methode gilt. da die anonyme Funktion `d => d.UnitPrice * d.UnitCount` erzeugt einen Wert vom Typ `double`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2438">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="411e5-2439">Daher überladen Auflösung auswählt, die zweite `Sum` Methode für diesen Aufruf.</span><span class="sxs-lookup"><span data-stu-id="411e5-2439">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="411e5-2440">Anonyme Funktionen und dynamische Bindung</span><span class="sxs-lookup"><span data-stu-id="411e5-2440">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="411e5-2441">Eine anonyme Funktion darf nicht über einen Empfänger, Argument oder zum Operand eine dynamisch gebundene Vorgang sein.</span><span class="sxs-lookup"><span data-stu-id="411e5-2441">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="411e5-2442">Äußere Variablen</span><span class="sxs-lookup"><span data-stu-id="411e5-2442">Outer variables</span></span>

<span data-ttu-id="411e5-2443">Alle lokalen Variablen, "Value"-Parameter oder Parameterarray, deren Bereich umfasst, die *Lambda_expression* oder *Anonymous_method_expression* heißt ein ***äußere Variable*** der anonyme Funktion.</span><span class="sxs-lookup"><span data-stu-id="411e5-2443">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="411e5-2444">In einer Instanz Funktionsmember der Member einer Klasse die `this` Wert gilt, dass einen Value-Parameter, und eine äußere Variable von jeder anonyme Funktion, die in der Funktionsmember enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-2444">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="411e5-2445">Äußere Variablen erfasst</span><span class="sxs-lookup"><span data-stu-id="411e5-2445">Captured outer variables</span></span>

<span data-ttu-id="411e5-2446">Wenn eine äußere Variable, die von einer anonymen Funktion verwiesen wird, gilt die äußere Variable wurden als ***erfasst*** von der anonymen Funktion.</span><span class="sxs-lookup"><span data-stu-id="411e5-2446">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="411e5-2447">Die Lebensdauer einer lokalen Variablen ist normalerweise begrenzt, für die Ausführung von Block oder der Anweisung, mit denen sie zugeordnet ist ([lokale Variablen](variables.md#local-variables)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2447">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="411e5-2448">Jedoch die Lebensdauer einer erfassten äußeren Variablen wird mindestens erweitert, bis der Delegat oder Ausdrucksbaumstruktur, die aus der anonymen Funktion erstellt wurde, wird für die Garbagecollection.</span><span class="sxs-lookup"><span data-stu-id="411e5-2448">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="411e5-2449">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2449">In the example</span></span>
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
<span data-ttu-id="411e5-2450">die lokale Variable `x` wird erfasst, indem Sie die anonyme Funktion und die Lebensdauer des `x` mindestens erweitert wurde, bis der Delegat, der von zurückgegebene `F` wird zum Kandidaten für die Garbagecollection (die erst ganz am Ende nicht der Fall ist das Programm).</span><span class="sxs-lookup"><span data-stu-id="411e5-2450">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="411e5-2451">Da der gleichen Instanz von jedem Aufruf der anonymen Funktion arbeitet `x`, die Ausgabe des Beispiels ist:</span><span class="sxs-lookup"><span data-stu-id="411e5-2451">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```
1
2
3
```

<span data-ttu-id="411e5-2452">Wenn eine lokale Variable oder ein Value-Parameter, die von einer anonymen Funktion erfasst sind, die lokale Variable oder Parameter wird nicht mehr als eine feste Variable sein ([für feste und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)), aber stattdessen gilt eine Moveable Variable.</span><span class="sxs-lookup"><span data-stu-id="411e5-2452">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="411e5-2453">Daher alle `unsafe` Code, der die Adresse der erfassten äußere Variable ist, muss zunächst mithilfe der `fixed` Anweisung, um die Variable zu beheben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2453">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="411e5-2454">Beachten Sie, dass im Gegensatz zu einer Variable uncaptured eine aufgezeichnete lokale Variable gleichzeitig für mehrere Ausführungsthreads verfügbar gemacht werden kann.</span><span class="sxs-lookup"><span data-stu-id="411e5-2454">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="411e5-2455">Instanziierung von lokalen Variablen</span><span class="sxs-lookup"><span data-stu-id="411e5-2455">Instantiation of local variables</span></span>

<span data-ttu-id="411e5-2456">Eine lokale Variable gilt ***instanziiert*** bei Ausführung des Gültigkeitsbereichs der Variablen wechselt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2456">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="411e5-2457">Z. B. wenn die folgende Methode aufgerufen wird, die lokale Variable `x` instanziiert und initialisiert drei Mal – einmal für jede Iteration der Schleife.</span><span class="sxs-lookup"><span data-stu-id="411e5-2457">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="411e5-2458">Verschieben Sie jedoch die Deklaration von `x` außerhalb der Schleife Ergebnisse in eine einzelne Instanziierung `x`:</span><span class="sxs-lookup"><span data-stu-id="411e5-2458">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="411e5-2459">Wenn nicht erfasst werden, besteht keine Möglichkeit, beachten Sie, wie oft eine lokale Variable instanziiert wird, da die Lebensdauer der die Instanziierungen disjunkt sind, kann für alle Instanziierungen einfach am gleichen Speicherort verwenden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2459">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="411e5-2460">Wenn eine anonyme Funktion eine lokale Variable erhält, werden die Auswirkungen der Instanziierung jedoch offensichtlich.</span><span class="sxs-lookup"><span data-stu-id="411e5-2460">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="411e5-2461">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2461">The example</span></span>
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
<span data-ttu-id="411e5-2462">erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="411e5-2462">produces the output:</span></span>
```
1
3
5
```

<span data-ttu-id="411e5-2463">Aber wenn die Deklaration von `x` wird außerhalb der Schleife verschoben:</span><span class="sxs-lookup"><span data-stu-id="411e5-2463">However, when the declaration of `x` is moved outside the loop:</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
<span data-ttu-id="411e5-2464">Die Ausgabe lautet:</span><span class="sxs-lookup"><span data-stu-id="411e5-2464">the output is:</span></span>
```
5
5
5
```

<span data-ttu-id="411e5-2465">Wenn eine Iterationsvariable in eine for-Schleife deklariert wird, gilt die Variable selbst außerhalb der Schleife deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2465">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="411e5-2466">Also, wenn das Beispiel so geändert wird, um die Iterationsvariable selbst zu erfassen:</span><span class="sxs-lookup"><span data-stu-id="411e5-2466">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="411e5-2467">nur eine Instanz der Iterationsvariablen wird erfasst, das die Ausgabe generiert:</span><span class="sxs-lookup"><span data-stu-id="411e5-2467">only one instance of the iteration variable is captured, which produces the output:</span></span>
```
3
3
3
```

<span data-ttu-id="411e5-2468">Es ist möglich, dass anonyme Funktion freigeben einige erfassten Variablen noch verfügen über separate Instanzen von anderen Delegaten.</span><span class="sxs-lookup"><span data-stu-id="411e5-2468">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="411e5-2469">Z. B. wenn `F` in geändert wird</span><span class="sxs-lookup"><span data-stu-id="411e5-2469">For example, if `F` is changed to</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
<span data-ttu-id="411e5-2470">die drei Delegaten erfassen die gleiche Instanz von `x` jedoch separaten Instanzen von `y`, und die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="411e5-2470">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```
1 1
2 1
3 1
```

<span data-ttu-id="411e5-2471">Separate anonyme Funktionen können mit die gleiche Instanz von eine äußere Variable erfassen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2471">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="411e5-2472">Im folgenden Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2472">In the example:</span></span>
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
<span data-ttu-id="411e5-2473">die beiden anonymen Funktionen erfassen die gleiche Instanz von der lokalen Variablen `x`, und sie können daher "kommunizieren" über diese Variable.</span><span class="sxs-lookup"><span data-stu-id="411e5-2473">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="411e5-2474">Die Ausgabe des Beispiels ist:</span><span class="sxs-lookup"><span data-stu-id="411e5-2474">The output of the example is:</span></span>
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="411e5-2475">Auswertung von Ausdrücken für anonyme Funktion</span><span class="sxs-lookup"><span data-stu-id="411e5-2475">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="411e5-2476">Eine anonyme Funktion `F` muss immer in einen Delegattyp konvertiert werden `D` oder ein Typ für die Ausdrucksbaumstruktur `E`, entweder direkt oder über die Ausführung von Delegaterstellungsausdrücke `new D(F)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2476">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="411e5-2477">Diese Konvertierung bestimmt das Ergebnis der anonymen Hauptfunktion unter [anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions).</span><span class="sxs-lookup"><span data-stu-id="411e5-2477">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="411e5-2478">Abfrageausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-2478">Query expressions</span></span>

<span data-ttu-id="411e5-2479">***Abfrageausdrücke*** bieten eine integrierte Sprachsyntax für Abfragen, die relationale und hierarchische Abfragesprachen wie SQL oder XQuery ähnelt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2479">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

<span data-ttu-id="411e5-2480">Ein Abfrageausdruck beginnt mit einer `from` -Klausel und endet mit einem `select` oder `group` Klausel.</span><span class="sxs-lookup"><span data-stu-id="411e5-2480">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="411e5-2481">Der ursprüngliche `from` Klausel gefolgt von NULL oder mehr `from`, `let`, `where`, `join` oder `orderby` Klauseln.</span><span class="sxs-lookup"><span data-stu-id="411e5-2481">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="411e5-2482">Jede `from` -Klausel ist ein Generator Einführung in eine ***Bereichsvariable*** der Bereiche, über die Elemente einer ***Sequenz***.</span><span class="sxs-lookup"><span data-stu-id="411e5-2482">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="411e5-2483">Jede `let` Klausel führt eine Bereichsvariable, der ein über den vorherigen Bereichsvariablen berechnet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2483">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="411e5-2484">Jede `where` -Klausel ist ein Filter, der Elemente aus dem Ergebnis ausgeschlossen sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-2484">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="411e5-2485">Jede `join` -Klausel vergleicht die angegebenen Schlüssel der Quellsequenz mit Schlüsseln von einer anderen Sequenz, die übereinstimmende Paare Rückgabe.</span><span class="sxs-lookup"><span data-stu-id="411e5-2485">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="411e5-2486">Jede `orderby` Klausel sortiert die Elemente gemäß den angegebenen Kriterien. Die endgültige `select` oder `group` -Klausel gibt die Form des Ergebnisses in Bezug auf die Bereichsvariablen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2486">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="411e5-2487">Zum Schluss eine `into` -Klausel kann verwendet werden, für die Abfragen "Zusammenführung", indem Sie die Ergebnisse einer Abfrage als einen Generator in einer nachfolgenden Abfrage zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="411e5-2487">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="411e5-2488">Allerdings haben Mehrdeutigkeiten in Abfrageausdrücken</span><span class="sxs-lookup"><span data-stu-id="411e5-2488">Ambiguities in query expressions</span></span>

<span data-ttu-id="411e5-2489">Abfrageausdrücke enthalten eine Anzahl von "kontextabhängige Schlüsselwörter", d. h. die Bezeichner, die in einem bestimmten Kontext eine besondere Bedeutung haben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2489">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="411e5-2490">Insbesondere Hierbei handelt es sich `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` und `by`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2490">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="411e5-2491">Um Mehrdeutigkeiten in Abfrageausdrücken aufgrund gemischter Verwendung dieser Bezeichner als Schlüsselwörter oder einfache Namen zu vermeiden, gelten diese Bezeichner Schlüsselwörter, wenn eine beliebige Stelle in einem Abfrageausdruck stattfinden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2491">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="411e5-2492">Zu diesem Zweck ein Abfrageausdruck ist ein Ausdruck, der mit "`from identifier`"gefolgt von jedes Token, mit Ausnahme von"`;`","`=`"oder"`,`".</span><span class="sxs-lookup"><span data-stu-id="411e5-2492">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="411e5-2493">Um diese Wörter als Bezeichner in einem Abfrageausdruck verwenden, sie können dem Präfix "`@`" ([Bezeichner](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2493">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="411e5-2494">Übersetzung Abfrageausdrucks</span><span class="sxs-lookup"><span data-stu-id="411e5-2494">Query expression translation</span></span>

<span data-ttu-id="411e5-2495">Die C#-Sprache gibt nicht die Ausführungssemantik von Abfrageausdrücken an.</span><span class="sxs-lookup"><span data-stu-id="411e5-2495">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="411e5-2496">Stattdessen Abfrageausdrücke werden in übersetzt, Aufrufe von Methoden, die folgen, der *Abfrageausdrucksmuster* ([das Abfrageausdrucksmuster](expressions.md#the-query-expression-pattern)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2496">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="411e5-2497">Insbesondere werden die Abfrageausdrücke in Aufrufe von Methoden, die mit dem Namen übersetzt `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, und `Cast`. Diese Methoden werden voraussichtlich bestimmter Signaturen und Ergebnistypen, siehe [das Abfrageausdrucksmuster](expressions.md#the-query-expression-pattern).</span><span class="sxs-lookup"><span data-stu-id="411e5-2497">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="411e5-2498">Diese Methoden können sein, Instanzmethoden des abgefragten Objekts oder von Erweiterungsmethoden, die auf das Objekt extern sind, und implementieren sie die eigentliche Ausführung der Abfrage.</span><span class="sxs-lookup"><span data-stu-id="411e5-2498">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="411e5-2499">Die Übersetzung von Abfrageausdrücken Methodenaufrufe ist eine syntaktische Zuordnung, die vor jeder typbindung oder die Auflösung von funktionsüberladungen ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="411e5-2499">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="411e5-2500">Die Übersetzung wird garantiert, syntaktisch korrekt zu sein, aber nicht notwendigerweise um semantisch richtige C#-Code zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2500">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="411e5-2501">Nach der Übersetzung von Abfrageausdrücken, die sich ergebende Methodenaufrufe werden als reguläre Methodenaufrufe verarbeitet, und dies kann wiederum Fehler, z. B. aufzudecken, wenn die Methoden nicht vorhanden sind, wenn Argumente falsch aufweisen, oder die Methoden generisch sind und Typrückschluss schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="411e5-2501">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="411e5-2502">Ein Abfrageausdruck wird verarbeitet, durch die folgenden Übersetzungen wiederholtes anwenden, bis keine weiteren Reduzierung möglich sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-2502">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="411e5-2503">Die Übersetzungen sind in der Reihenfolge der Anwendung aufgeführt: jeder Abschnitt wird davon ausgegangen, dass die Übersetzungen in den vorherigen Abschnitten wurden vollständig ausgeführt, und nachdem erreicht, ein Abschnitt nicht höher sein bei der Verarbeitung der selben Abfrageausdruck Revisited wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-2503">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="411e5-2504">Zuweisung zu einer Bereichsvariablen darf nicht in Abfrageausdrücken.</span><span class="sxs-lookup"><span data-stu-id="411e5-2504">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="411e5-2505">Eine C#-Implementierung darf jedoch nicht immer dieser Einschränkung zu erzwingen, da dies in einigen Fällen möglicherweise nicht mit dem hier vorgestellten syntaktische Übersetzung-Schema werden kann.</span><span class="sxs-lookup"><span data-stu-id="411e5-2505">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="411e5-2506">Bestimmte Übersetzungen injizieren Bereichsvariablen mit transparenter Bezeichner gekennzeichnet durch `*`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2506">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="411e5-2507">Die speziellen Eigenschaften transparenter Bezeichner werden erläutert. im weiteren [transparenter Bezeichner](expressions.md#transparent-identifiers).</span><span class="sxs-lookup"><span data-stu-id="411e5-2507">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="411e5-2508">Wählen Sie "und" Groupby-Klausel mit Fortsetzungen</span><span class="sxs-lookup"><span data-stu-id="411e5-2508">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="411e5-2509">Ein Abfrageausdruck mit einer Fortsetzung</span><span class="sxs-lookup"><span data-stu-id="411e5-2509">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="411e5-2510">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2510">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="411e5-2511">Die Übersetzungen in den folgenden Abschnitten wird davon ausgegangen, dass Abfragen keine `into` Fortsetzungen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2511">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="411e5-2512">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2512">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="411e5-2513">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2513">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="411e5-2514">die endgültige Übersetzung der ist</span><span class="sxs-lookup"><span data-stu-id="411e5-2514">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="411e5-2515">Explizite Bereich Variablentypen</span><span class="sxs-lookup"><span data-stu-id="411e5-2515">Explicit range variable types</span></span>

<span data-ttu-id="411e5-2516">Ein `from` -Klausel, die einen Bereichsvariablentyp gibt explizit an</span><span class="sxs-lookup"><span data-stu-id="411e5-2516">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="411e5-2517">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2517">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="411e5-2518">Ein `join` -Klausel, die einen Bereichsvariablentyp gibt explizit an</span><span class="sxs-lookup"><span data-stu-id="411e5-2518">A `join` clause that explicitly specifies a range variable type</span></span>
```
join T x in e on k1 equals k2
```
<span data-ttu-id="411e5-2519">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2519">is translated into</span></span>
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="411e5-2520">Die Übersetzungen in den folgenden Abschnitten wird davon ausgegangen, dass Abfragen keine expliziten Bereich Variablentypen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2520">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="411e5-2521">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2521">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="411e5-2522">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2522">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="411e5-2523">die endgültige Übersetzung der ist</span><span class="sxs-lookup"><span data-stu-id="411e5-2523">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="411e5-2524">Variablentypen expliziten Bereich sind nützlich zum Abfragen von Auflistungen bereit, die nicht generische implementieren `IEnumerable` -Schnittstelle, aber nicht die generische `IEnumerable<T>` Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="411e5-2524">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="411e5-2525">Im obigen Beispiel wäre dies der Fall, wenn `customers` wurden vom Typ `ArrayList`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2525">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="411e5-2526">Degenerierte-Abfrageausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-2526">Degenerate query expressions</span></span>

<span data-ttu-id="411e5-2527">Ein Abfrageausdruck den Wert des Formulars</span><span class="sxs-lookup"><span data-stu-id="411e5-2527">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="411e5-2528">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2528">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="411e5-2529">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2529">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="411e5-2530">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2530">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="411e5-2531">Ein degenerierter Abfrageausdruck ist eine, die einfach die Elemente der Quelle auswählt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2531">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="411e5-2532">Eine spätere Phase der Übersetzung entfernt degenerierte Abfragen durch andere Übersetzungsschritten, indem diese zu ihrer Quelle ersetzt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2532">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="411e5-2533">Es ist wichtig, jedoch um sicherzustellen, dass das Ergebnis einer Abfrage Ausdruck nie das Quellobjekt selbst ist, wie die, die den Typ und die Identität der Quelle an den Client der Abfrage anzeigen würden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2533">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="411e5-2534">Aus diesem Grund schützt dadurch degenerierte Abfragen, die direkt im Quellcode geschrieben wird, durch explizites Aufrufen von `Select` auf dem Quellcomputer.</span><span class="sxs-lookup"><span data-stu-id="411e5-2534">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="411e5-2535">Es ist Ihre Aufgabe die Implementierer von `Select` und andere Abfrageoperatoren, um sicherzustellen, dass diese Methoden geben das Quellobjekt selbst nie zurück.</span><span class="sxs-lookup"><span data-stu-id="411e5-2535">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="411e5-2536">Zu ermöglichen, where, Join und Orderby-Klausel</span><span class="sxs-lookup"><span data-stu-id="411e5-2536">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="411e5-2537">Ein Abfrageausdruck mit einer Sekunde `from` Klausel gefolgt von einem `select` Klausel</span><span class="sxs-lookup"><span data-stu-id="411e5-2537">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="411e5-2538">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2538">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="411e5-2539">Ein Abfrageausdruck mit einer Sekunde `from` Klausel gefolgt von etwas anders als eine `select` Klausel:</span><span class="sxs-lookup"><span data-stu-id="411e5-2539">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="411e5-2540">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2540">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="411e5-2541">Ein Abfrageausdruck mit einer `let` Klausel</span><span class="sxs-lookup"><span data-stu-id="411e5-2541">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="411e5-2542">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2542">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="411e5-2543">Ein Abfrageausdruck mit einer `where` Klausel</span><span class="sxs-lookup"><span data-stu-id="411e5-2543">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="411e5-2544">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2544">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="411e5-2545">Einen Abfrageausdruck mit einer `join` -Klausel ohne eine `into` gefolgt von einem `select` Klausel</span><span class="sxs-lookup"><span data-stu-id="411e5-2545">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="411e5-2546">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2546">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="411e5-2547">Einen Abfrageausdruck mit einer `join` -Klausel ohne eine `into` gefolgt von etwas anders als eine `select` Klausel</span><span class="sxs-lookup"><span data-stu-id="411e5-2547">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="411e5-2548">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2548">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="411e5-2549">Einen Abfrageausdruck mit einer `join` -Klausel mit einer `into` gefolgt von einer `select` Klausel</span><span class="sxs-lookup"><span data-stu-id="411e5-2549">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="411e5-2550">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2550">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="411e5-2551">Einen Abfrageausdruck mit einer `join` -Klausel mit einer `into` gefolgt von etwas anders als eine `select` Klausel</span><span class="sxs-lookup"><span data-stu-id="411e5-2551">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="411e5-2552">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2552">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="411e5-2553">Ein Abfrageausdruck mit einer `orderby` Klausel</span><span class="sxs-lookup"><span data-stu-id="411e5-2553">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="411e5-2554">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2554">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="411e5-2555">-Klausel gibt an, wenn eine Sortierung, die eine `descending` Richtung-Symbol, einen Aufruf der `OrderByDescending` oder `ThenByDescending` stattdessen erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-2555">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="411e5-2556">Die folgenden Übersetzungen wird davon ausgegangen, dass es sind keine `let`, `where`, `join` oder `orderby` -Klauseln und nicht mehr als einen ersten `from` -Klausel in jedem Abfrageausdruck.</span><span class="sxs-lookup"><span data-stu-id="411e5-2556">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="411e5-2557">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2557">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="411e5-2558">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2558">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="411e5-2559">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2559">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="411e5-2560">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2560">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="411e5-2561">die endgültige Übersetzung der ist</span><span class="sxs-lookup"><span data-stu-id="411e5-2561">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="411e5-2562">wo `x` ist ein vom Compiler generierten Bezeichner, der ansonsten nicht sichtbar und kann nicht zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2562">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="411e5-2563">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2563">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="411e5-2564">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2564">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="411e5-2565">die endgültige Übersetzung der ist</span><span class="sxs-lookup"><span data-stu-id="411e5-2565">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="411e5-2566">wo `x` ist ein vom Compiler generierten Bezeichner, der ansonsten nicht sichtbar und kann nicht zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2566">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="411e5-2567">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2567">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="411e5-2568">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2568">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="411e5-2569">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2569">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="411e5-2570">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2570">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="411e5-2571">die endgültige Übersetzung der ist</span><span class="sxs-lookup"><span data-stu-id="411e5-2571">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="411e5-2572">wo `x` und `y` sind vom Compiler generierten Bezeichner, die andernfalls nicht sichtbar und kann nicht zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2572">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="411e5-2573">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2573">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="411e5-2574">hat die letzte Übersetzung</span><span class="sxs-lookup"><span data-stu-id="411e5-2574">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="411e5-2575">Select-Klauseln</span><span class="sxs-lookup"><span data-stu-id="411e5-2575">Select clauses</span></span>

<span data-ttu-id="411e5-2576">Ein Abfrageausdruck den Wert des Formulars</span><span class="sxs-lookup"><span data-stu-id="411e5-2576">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="411e5-2577">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2577">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="411e5-2578">es sei denn, V den Bezeichner X, die Übersetzung ist einfach</span><span class="sxs-lookup"><span data-stu-id="411e5-2578">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="411e5-2579">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="411e5-2579">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="411e5-2580">In wird einfach übersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2580">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="411e5-2581">GroupBy-Klausel</span><span class="sxs-lookup"><span data-stu-id="411e5-2581">Groupby clauses</span></span>

<span data-ttu-id="411e5-2582">Ein Abfrageausdruck den Wert des Formulars</span><span class="sxs-lookup"><span data-stu-id="411e5-2582">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="411e5-2583">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2583">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="411e5-2584">es sei denn, V den Bezeichner X, die Übersetzung ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2584">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="411e5-2585">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2585">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="411e5-2586">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2586">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="411e5-2587">Transparenter Bezeichner</span><span class="sxs-lookup"><span data-stu-id="411e5-2587">Transparent identifiers</span></span>

<span data-ttu-id="411e5-2588">Bestimmte Übersetzungen einfügen Bereichsvariablen mit ***transparenter Bezeichner*** gekennzeichnet durch `*`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2588">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="411e5-2589">Transparente-IDs sind keine geeignete Sprache-Funktion. Sie sind nur als ein Zwischenschritt bei der Abfrage Ausdruck Übersetzungsprozess vorhanden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2589">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="411e5-2590">Wenn eine abfrageübersetzung einen transparenten Bezeichner einbettet sollten, weiter weitergegeben Übersetzungsschritten den transparenten Bezeichner in anonymen Funktionen und anonyme Objektinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="411e5-2590">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="411e5-2591">In diesen Kontexten weisen transparenter Bezeichner folgendes Verhalten auf:</span><span class="sxs-lookup"><span data-stu-id="411e5-2591">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="411e5-2592">Wenn ein transparenter Bezeichner als Parameter in einer anonymen Funktion auftritt, werden die Member des anonymen Typs zugeordnet automatisch im Bereich des Texts der anonymen Funktion.</span><span class="sxs-lookup"><span data-stu-id="411e5-2592">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="411e5-2593">Wenn ein Element mit einem transparenten Bezeichner im Gültigkeitsbereich befindet, sind im Bereich auch die Elemente dieses Elements.</span><span class="sxs-lookup"><span data-stu-id="411e5-2593">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="411e5-2594">Tritt ein transparenter Bezeichner als ein Member-Declarator in einem anonyme Objektinitialisierer, führt er einen Member mit einem transparenten Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="411e5-2594">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="411e5-2595">In den oben beschriebenen Übersetzungsschritten werden transparent Bezeichner immer zusammen mit anonymen Typen, mit der Absicht an mehrere Bereichsvariablen als Mitglieder eines einzelnen Objekts erfassen eingeführt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2595">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="411e5-2596">Eine Implementierung von c# ist zulässig, einen anderen Mechanismus als anonyme Typen zu verwenden, um mehrere Bereichsvariablen zu gruppieren.</span><span class="sxs-lookup"><span data-stu-id="411e5-2596">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="411e5-2597">In den folgenden Beispielen für die Übersetzung wird davon ausgegangen, dass anonyme Typen werden verwendet, und wie transparent Bezeichner zeigen sofort übersetzt werden kann.</span><span class="sxs-lookup"><span data-stu-id="411e5-2597">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="411e5-2598">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2598">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="411e5-2599">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2599">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="411e5-2600">die übersetzt weiteren in</span><span class="sxs-lookup"><span data-stu-id="411e5-2600">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="411e5-2601">die transparente Bezeichner gelöscht werden, entspricht</span><span class="sxs-lookup"><span data-stu-id="411e5-2601">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="411e5-2602">wo `x` ist ein vom Compiler generierten Bezeichner, der ansonsten nicht sichtbar und kann nicht zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2602">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="411e5-2603">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2603">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="411e5-2604">wird übersetzt</span><span class="sxs-lookup"><span data-stu-id="411e5-2604">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="411e5-2605">die weiteren wird nach einer Verkleinerung auf</span><span class="sxs-lookup"><span data-stu-id="411e5-2605">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="411e5-2606">die endgültige Übersetzung der ist</span><span class="sxs-lookup"><span data-stu-id="411e5-2606">the final translation of which is</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
<span data-ttu-id="411e5-2607">wo `x`, `y`, und `z` sind vom Compiler generierten Bezeichner, die andernfalls nicht sichtbar und kann nicht zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2607">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="411e5-2608">Das Abfrageausdrucksmuster</span><span class="sxs-lookup"><span data-stu-id="411e5-2608">The query expression pattern</span></span>

<span data-ttu-id="411e5-2609">Die ***Abfrageausdrucksmuster*** richtet ein Muster von Methoden, Typen implementiert werden können, um Abfrageausdrücke unterstützen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2609">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="411e5-2610">Da durch eine syntaktische Zuordnung Abfrageausdrücke Methodenaufrufe übersetzt werden, haben Typen beträchtliche Flexibilität bei, wie sie das Abfrageausdrucksmuster implementieren.</span><span class="sxs-lookup"><span data-stu-id="411e5-2610">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="411e5-2611">Beispielsweise können die Methoden des Musters als Instanzmethoden oder als Erweiterungsmethoden implementiert werden, da die beiden die gleiche Aufrufsyntax haben und die Methoden Delegaten oder Ausdrucksbaumstrukturen anfordern können, da anonyme Funktionen sowohl konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2611">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="411e5-2612">Die empfohlene Form eines generischen Typs `C<T>` , unterstützt Abfragemusters Ausdruck unten dargestellt ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2612">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="411e5-2613">Ein generischer Typ wird verwendet, um die richtigen Beziehungen zwischen Parameter und Ergebnis zu veranschaulichen, aber es ist möglich, die das Muster für nicht generische Typen ebenfalls implementieren.</span><span class="sxs-lookup"><span data-stu-id="411e5-2613">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

<span data-ttu-id="411e5-2614">Die oben genannten Methoden verwenden die generische Delegattypen `Func<T1,R>` und `Func<T1,T2,R>`, aber sie ebenso gut hätten auch andere Typen für Delegat- oder Struktur mit den gleichen Beziehungen in Parameter und Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="411e5-2614">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="411e5-2615">Beachten Sie, dass die empfohlene Beziehung zwischen `C<T>` und `O<T>` wird sichergestellt, dass die `ThenBy` und `ThenByDescending` Methoden stehen nur auf das Ergebnis einer `OrderBy` oder `OrderByDescending`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2615">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="411e5-2616">Beachten Sie auch die empfohlene Form des Ergebnisses `GroupBy` – eine Sequenz von Sequenzen, die jeder innere Sequenz verfügt, in denen über einen zusätzlichen `Key` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="411e5-2616">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="411e5-2617">Die `System.Linq` Namespace stellt eine Implementierung des Abfragemusters Operator für jeden Typ, der implementiert die `System.Collections.Generic.IEnumerable<T>` Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="411e5-2617">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="411e5-2618">Zuweisungsoperatoren</span><span class="sxs-lookup"><span data-stu-id="411e5-2618">Assignment operators</span></span>

<span data-ttu-id="411e5-2619">Die Zuweisungsoperatoren zuweisen eine Variable, eine Eigenschaft, ein Ereignis oder ein Indexerelement einen neuen Wert.</span><span class="sxs-lookup"><span data-stu-id="411e5-2619">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

<span data-ttu-id="411e5-2620">Der linke Operand einer Zuweisung muss ein Ausdruck, der als eine Variable, einen Eigenschaftenzugriff, Indexzugriff oder ein Ereigniszugriff klassifiziert sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-2620">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="411e5-2621">Die `=` Operator wird aufgerufen, die ***einfacher Zuweisungsoperator***.</span><span class="sxs-lookup"><span data-stu-id="411e5-2621">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="411e5-2622">Es weist den Wert des rechten Operanden, auf die Variable, Eigenschaft oder der Indexer-Element, durch den linken Operanden angegeben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2622">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="411e5-2623">Der linke Operand des der einfache Zuweisungsoperator möglicherweise kein Event-Zugriff (mit Ausnahme beschriebener in [Feldähnliche Ereignisse](classes.md#field-like-events)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2623">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="411e5-2624">Der einfache Zuweisungsoperator finden Sie im [einfache Zuweisung](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="411e5-2624">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="411e5-2625">Die Zuweisungsoperatoren außer der `=` Operator heißen die ***zusammensetzungszuweisungsoperatoren***.</span><span class="sxs-lookup"><span data-stu-id="411e5-2625">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="411e5-2626">Diese Operatoren den angegebenen Vorgang für die beiden Operanden ausführen, und klicken Sie dann den resultierenden Wert zuweisen, auf die Variable, Eigenschaft oder der Indexer-Element, durch den linken Operanden angegeben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2626">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="411e5-2627">Die zusammengesetzten Zuweisungsoperatoren werden in beschrieben [Verbundzuweisung](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="411e5-2627">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="411e5-2628">Die `+=` und `-=` Operatoren mit einem Event-Access-Ausdruck wie der linke Operand heißen die *Ereignis Zuweisungsoperatoren*.</span><span class="sxs-lookup"><span data-stu-id="411e5-2628">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="411e5-2629">Keine anderen Zuweisungsoperator ist wie der linke Operand ein Ereigniszugriff gültig.</span><span class="sxs-lookup"><span data-stu-id="411e5-2629">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="411e5-2630">Die Ereignis-Zuweisungsoperatoren werden in beschrieben [Ereignis Zuweisung](expressions.md#event-assignment).</span><span class="sxs-lookup"><span data-stu-id="411e5-2630">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="411e5-2631">Die Zuweisungsoperatoren sind rechtsassoziativ, was bedeutet, dass Vorgänge, die von rechts nach links gruppiert werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2631">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="411e5-2632">Z. B. ein Ausdruck der Form `a = b = c` wird als ausgewertet, `a = (b = c)`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2632">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="411e5-2633">Einfache Zuweisung</span><span class="sxs-lookup"><span data-stu-id="411e5-2633">Simple assignment</span></span>

<span data-ttu-id="411e5-2634">Die `=` Operator wird den einfache Zuweisungsoperator aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2634">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="411e5-2635">Wenn der linke Operand, der eine einfache Zuweisung des Formulars ist `E.P` oder `E[Ei]` , in denen `E` weist den Typ der Kompilierzeit `dynamic`, und klicken Sie dann die Zuweisung dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2635">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="411e5-2636">In diesem Fall den Kompilierzeittyp des Zuweisungsausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit basierend auf den Laufzeittyp der `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2636">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="411e5-2637">In einer einfachen Zuweisung muss der Rechte Operand ein Ausdruck sein, der implizit in den Typ des linken Operanden ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2637">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="411e5-2638">Der Vorgang weist den Wert des rechten Operanden auf die Variable, Eigenschaft oder der Indexer-Element, durch den linken Operanden angegeben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2638">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="411e5-2639">Das Ergebnis eines Ausdrucks für die einfache Zuweisung wird der Wert, der linke Operand zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2639">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="411e5-2640">Das Ergebnis weist den gleichen Typ wie der linke Operand, und es wird immer als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="411e5-2640">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="411e5-2641">Wenn der linke Operand eine Eigenschaft oder der Indexer-Zugriff ist, Eigenschaft oder des Indexers benötigen eine `set` Accessor.</span><span class="sxs-lookup"><span data-stu-id="411e5-2641">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="411e5-2642">Wenn dies nicht der Fall ist, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2642">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="411e5-2643">Die Verarbeitung zur Laufzeit eine einfache Zuweisung des Formulars `x = y` umfasst die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="411e5-2643">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="411e5-2644">Wenn `x` wird als Variable klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="411e5-2644">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="411e5-2645">`x` wird ausgewertet, um die Variable zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2645">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="411e5-2646">`y` wird ausgewertet und, falls erforderlich, konvertiert in den Typ des `x` über eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2646">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="411e5-2647">Wenn die Variable, die vom `x` ist ein Arrayelement einen *Reference_type*, eine laufzeitüberprüfung ausgeführt, um sicherzustellen, dass der Wert für berechnet `y` ist kompatibel mit der Arrayinstanz, von denen `x` ist ein Element.</span><span class="sxs-lookup"><span data-stu-id="411e5-2647">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="411e5-2648">Die Überprüfung ist erfolgreich, wenn `y` ist `null`, oder wenn implizite verweiskonvertierung ([implizite Verweis-](conversions.md#implicit-reference-conversions)) vorhanden ist, aus dem tatsächlichen Typ der Instanz verweist `y` in den tatsächlichen Elementtyp der Instanz für das Array mit `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2648">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="411e5-2649">Andernfalls wird eine `System.ArrayTypeMismatchException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="411e5-2649">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="411e5-2650">Der Wert der Auswertung und Konvertierung von `y` befindet sich in den Speicherort durch die Auswertung von `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2650">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="411e5-2651">Wenn `x` wird als eine Eigenschaft oder der Indexer Zugriff klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="411e5-2651">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="411e5-2652">Der Instanzausdruck (Wenn `x` ist nicht `static`) und die Argumentliste (Wenn `x` Indexzugriff ist) zugeordneten `x` werden ausgewertet, und die Ergebnisse werden in der nachfolgenden verwendet `set` Accessor-Aufruf.</span><span class="sxs-lookup"><span data-stu-id="411e5-2652">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="411e5-2653">`y` wird ausgewertet und, falls erforderlich, konvertiert in den Typ des `x` über eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2653">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="411e5-2654">Die `set` Accessor `x` wird aufgerufen, mit dem Wert für berechnet `y` als seine `value` Argument.</span><span class="sxs-lookup"><span data-stu-id="411e5-2654">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="411e5-2655">Die Array-Co Varianz-Regeln ([Array-Kovarianz](arrays.md#array-covariance)) zulassen den Wert eines Arraytyps `A[]` sollen einen Verweis auf eine Instanz des Arraytyps `B[]`, sofern eine implizite verweiskonvertierung von vorhanden ist `B` zu `A`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2655">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="411e5-2656">Aufgrund dieser Regeln, die Zuweisung an ein Arrayelement einen *Reference_type* erfordert eine laufzeitüberprüfung aus, um sicherzustellen, dass der zugewiesene Wert mit dem Array-Instanz kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2656">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="411e5-2657">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2657">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="411e5-2658">bewirkt, dass die letzte Zuweisung einer `System.ArrayTypeMismatchException` ausgelöst werden, da eine Instanz von `ArrayList` kann nicht gespeichert werden, in einem Element des eine `string[]`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2658">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="411e5-2659">Wenn eine Eigenschaft oder der Indexer in deklariert eine *Struct_type* ist das Ziel einer Zuweisung, die den Instanzausdruck, die mit der Eigenschaft verknüpften oder Indexerzugriff muss als Variable klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="411e5-2659">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="411e5-2660">Wenn der Instanzausdruck als Wert klassifiziert ist, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2660">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="411e5-2661">Aufgrund der [Memberzugriff](expressions.md#member-access), die gleiche Regel gilt auch für Felder.</span><span class="sxs-lookup"><span data-stu-id="411e5-2661">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="411e5-2662">Betrachten Sie die Deklarationen:</span><span class="sxs-lookup"><span data-stu-id="411e5-2662">Given the declarations:</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
<span data-ttu-id="411e5-2663">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2663">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="411e5-2664">die Zuweisungen zu `p.X`, `p.Y`, `r.A`, und `r.B` sind zulässig, da `p` und `r` Variablen sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-2664">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="411e5-2665">Im Beispiel allerdings</span><span class="sxs-lookup"><span data-stu-id="411e5-2665">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="411e5-2666">die Zuweisungen sind ungültig, da `r.A` und `r.B` sind keine Variablen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2666">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="411e5-2667">Verbundzuweisung</span><span class="sxs-lookup"><span data-stu-id="411e5-2667">Compound assignment</span></span>

<span data-ttu-id="411e5-2668">Wenn der linke Operand des eine zusammengesetzte Zuweisung des Formulars ist `E.P` oder `E[Ei]` , in denen `E` weist den Typ während der Kompilierung `dynamic`, und klicken Sie dann die Zuweisung dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2668">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="411e5-2669">In diesem Fall den Kompilierzeittyp des Zuweisungsausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit basierend auf den Laufzeittyp der `E`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2669">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="411e5-2670">Ein Vorgang des Formulars `x op= y` wird durch Anwenden der Auflösung von funktionsüberladungen binärer Operator verarbeitet ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)), als ob der Vorgang geschrieben wurde `x op y`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2670">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="411e5-2671">Klicken Sie dann</span><span class="sxs-lookup"><span data-stu-id="411e5-2671">Then,</span></span>

*  <span data-ttu-id="411e5-2672">Wenn der Rückgabetyp des ausgewählten Operators implizit in den Typ des `x`, der Vorgang wird als ausgewertet `x = x op y`, außer dass `x` wird nur einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2672">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="411e5-2673">Andernfalls, wenn der ausgewählte Operator einen vordefinierten Operator ist, ist der Rückgabetyp von den ausgewählten Operator explizit konvertiert werden kann, in den Typ des `x`, und wenn `y` wird implizit in den Typ des `x` oder der Operator ist ein Shift-Operator, und klicken Sie dann der Vorgang als ausgewertet wird `x = (T)(x op y)`, wobei `T` ist der Typ des `x`, außer dass `x` wird nur einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2673">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="411e5-2674">Andernfalls die zusammengesetzte Zuweisung ist ungültig, und beim Auftreten eines Laufzeitfehlers-Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2674">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="411e5-2675">Der Begriff "nur einmal ausgewertet" bedeutet, dass bei der Auswertung `x op y`, die Ergebnisse der enthaltenen Ausdrücke von `x` vorübergehend gespeichert und dann wiederverwendet werden, bei der Zuweisung zu `x`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2675">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="411e5-2676">Z. B. in der Zuweisung `A()[B()] += C()`, wobei `A` ist eine Methode zurückgeben `int[]`, und `B` und `C` sind Methoden, die zurückgeben `int`, die Methoden werden nur einmal aufgerufen, in der Reihenfolge `A`, `B`, `C`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2676">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="411e5-2677">Wenn der linke Operand des eine zusammengesetzte Zuweisung einer Eigenschaft oder Indexzugriff ist, Eigenschaft oder des Indexers benötigen Sie Folgendes ein `get` Accessor und einen `set` Accessor.</span><span class="sxs-lookup"><span data-stu-id="411e5-2677">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="411e5-2678">Wenn dies nicht der Fall ist, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2678">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="411e5-2679">Die zweite Regel oben ermöglicht `x op= y` als auszuwertenden `x = (T)(x op y)` in bestimmten Kontexten.</span><span class="sxs-lookup"><span data-stu-id="411e5-2679">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="411e5-2680">Die Regel vorhanden ist, dass die vordefinierten Operatoren an, die als zusammengesetzte Operatoren verwendet werden können, wenn der linke Operand vom Typ `sbyte`, `byte`, `short`, `ushort`, oder `char`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2680">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="411e5-2681">Auch wenn beide Argumente einer dieser Typen sind, die vordefinierten Operatoren erzeugt ein Ergebnis vom Typ `int`, wie in beschrieben [binären numerischen heraufstufungen](expressions.md#binary-numeric-promotions).</span><span class="sxs-lookup"><span data-stu-id="411e5-2681">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="411e5-2682">Daher ohne eine Umwandlung wäre nicht möglich, einem der linke Operand das Ergebnis zuzuweisen es.</span><span class="sxs-lookup"><span data-stu-id="411e5-2682">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="411e5-2683">Die intuitive Auswirkung der Regel für die vordefinierten Operatoren ist einfach, `x op= y` ist zulässig, wenn sowohl der `x op y` und `x = y` sind zulässig.</span><span class="sxs-lookup"><span data-stu-id="411e5-2683">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="411e5-2684">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2684">In the example</span></span>
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
<span data-ttu-id="411e5-2685">die Fehlerursache für jeden Fehler ist, dass eine entsprechende einfache Zuweisung auch ein Fehler gewesen wäre.</span><span class="sxs-lookup"><span data-stu-id="411e5-2685">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="411e5-2686">Dies bedeutet auch, dass die verbundzuweisung, in die Vorgänge unterstützen Vorgänge aufgehoben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2686">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="411e5-2687">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="411e5-2687">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="411e5-2688">der transformierten Operator `+(int?,int?)` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-2688">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="411e5-2689">Ereignis-Zuweisung</span><span class="sxs-lookup"><span data-stu-id="411e5-2689">Event assignment</span></span>

<span data-ttu-id="411e5-2690">Wenn der linke Operand des eine `+=` oder `-=` Operator wird als Ereigniszugriff klassifiziert, und der Ausdruck wird wie folgt ausgewertet:</span><span class="sxs-lookup"><span data-stu-id="411e5-2690">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="411e5-2691">Der Instanzausdruck wird ggf. von der Ereigniszugriff ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2691">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="411e5-2692">Der Rechte Operand des der `+=` oder `-=` Operator wird ausgewertet, und, falls erforderlich, konvertiert in den Typ des linken Operanden über eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2692">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="411e5-2693">Ein Ereignisaccessor des Ereignisses aufgerufen wird, mit der Argumentliste, die mit dem rechten Operanden ist, nach der Auswertung und, falls erforderlich, Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2693">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="411e5-2694">Wenn der Operator war `+=`, `add` -Accessor wird aufgerufen, wenn der Operator war `-=`, `remove` -Accessor wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2694">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="411e5-2695">Zuweisungsausdruck Ereignis ergibt sich nicht auf einen Wert aus.</span><span class="sxs-lookup"><span data-stu-id="411e5-2695">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="411e5-2696">Daher Zuweisungsausdruck Ereignis nur im Kontext gültig ist eine *Statement_expression* ([ausdrucksanweisungen](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2696">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="411e5-2697">Ausdruck</span><span class="sxs-lookup"><span data-stu-id="411e5-2697">Expression</span></span>

<span data-ttu-id="411e5-2698">Ein *Ausdruck* ist entweder ein *Non_assignment_expression* oder *Zuweisung*.</span><span class="sxs-lookup"><span data-stu-id="411e5-2698">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a><span data-ttu-id="411e5-2699">Konstante Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-2699">Constant expressions</span></span>

<span data-ttu-id="411e5-2700">Ein *Constant_expression* ist ein Ausdruck, der zum Zeitpunkt der Kompilierung vollständig ausgewertet werden kann.</span><span class="sxs-lookup"><span data-stu-id="411e5-2700">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="411e5-2701">Es muss ein konstanter Ausdruck sein. die `null` Literal oder einen Wert mit einem der folgenden Typen: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, `bool`, `object`, `string`, oder ein Enumerationstyp.</span><span class="sxs-lookup"><span data-stu-id="411e5-2701">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="411e5-2702">Nur die folgenden Konstrukte sind in Konstanten Ausdrücken zulässig:</span><span class="sxs-lookup"><span data-stu-id="411e5-2702">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="411e5-2703">Literale (einschließlich der `null` literal).</span><span class="sxs-lookup"><span data-stu-id="411e5-2703">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="411e5-2704">Verweise auf `const` Member der Klasse und Strukturtypen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2704">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="411e5-2705">Verweise auf Member von Enumerationstypen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2705">References to members of enumeration types.</span></span>
*  <span data-ttu-id="411e5-2706">Verweise auf `const` Parameter oder lokale Variablen</span><span class="sxs-lookup"><span data-stu-id="411e5-2706">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="411e5-2707">Untergeordnete Ausdrücke in Klammern, die Konstante Ausdrücke sind.</span><span class="sxs-lookup"><span data-stu-id="411e5-2707">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="411e5-2708">CAST-Ausdrücke, die den Typ des bereitgestellten ist einer der oben aufgeführten Typen.</span><span class="sxs-lookup"><span data-stu-id="411e5-2708">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="411e5-2709">`checked` und `unchecked` Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-2709">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="411e5-2710">Ausdrücke mit Standardwert</span><span class="sxs-lookup"><span data-stu-id="411e5-2710">Default value expressions</span></span>
*  <span data-ttu-id="411e5-2711">Nameof-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-2711">Nameof expressions</span></span>
*  <span data-ttu-id="411e5-2712">Die vordefinierten `+`, `-`, `!`, und `~` unäre Operatoren.</span><span class="sxs-lookup"><span data-stu-id="411e5-2712">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="411e5-2713">Die vordefinierten `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, und `>=` binäre Operatoren bereitgestellten jeder Operand ist ein Typ, der oben aufgeführten.</span><span class="sxs-lookup"><span data-stu-id="411e5-2713">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="411e5-2714">Die `?:` bedingter Operator.</span><span class="sxs-lookup"><span data-stu-id="411e5-2714">The `?:` conditional operator.</span></span>

<span data-ttu-id="411e5-2715">Die folgenden Konvertierungen sind in Konstanten Ausdrücken zulässig:</span><span class="sxs-lookup"><span data-stu-id="411e5-2715">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="411e5-2716">Identity-Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="411e5-2716">Identity conversions</span></span>
*  <span data-ttu-id="411e5-2717">Numerische Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="411e5-2717">Numeric conversions</span></span>
*  <span data-ttu-id="411e5-2718">Enumeration-Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="411e5-2718">Enumeration conversions</span></span>
*  <span data-ttu-id="411e5-2719">Konstanter Ausdruck-Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="411e5-2719">Constant expression conversions</span></span>
*  <span data-ttu-id="411e5-2720">Implizite und explizite Verweis-, vorausgesetzt, dass die Quelle der Konvertierungen ein konstanter Ausdruck ist, der den null-Wert ergibt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2720">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="411e5-2721">Andere Konvertierungen, einschließlich Boxing, sind unboxing und implizite Verweis-, der nicht-Null-Werte in Konstanten Ausdrücken nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="411e5-2721">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="411e5-2722">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="411e5-2722">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="411e5-2723">die Initialisierung von i ist ein Fehler auf, da eine Boxing-Konvertierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2723">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="411e5-2724">Die Initialisierung von str ist ein Fehler auf, da implizite verweiskonvertierung aus einem Wert ungleich Null erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="411e5-2724">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="411e5-2725">Wenn ein Ausdruck die oben aufgeführten Anforderungen erfüllt, wird der Ausdruck zur Kompilierzeit ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2725">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="411e5-2726">Dies gilt auch, wenn der Ausdruck einen Unterausdruck eines umfassenderen Ausdrucks ist, die nicht Konstante Konstrukte enthält.</span><span class="sxs-lookup"><span data-stu-id="411e5-2726">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="411e5-2727">Die Auswertung der Kompilierzeit konstanter Ausdrücke verwendet die gleichen Regeln als laufzeitauswertung von Ausdrücken für nicht-Konstante, außer dass bewirkt, einen Fehler während der Kompilierung auftreten dass, in denen laufzeitauswertung ausgelöst worden wäre, eine Ausnahme, die kompilierzeitauswertung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2727">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="411e5-2728">Es sei denn, Sie befindet sich ein konstanter Ausdruck explizit in ein `unchecked` Kontext, während der Kompilierung Auswertung des Ausdrucks immer arithmetische Operationen für ganzzahlige Typen und Konvertierungen auftretenden verursachen Kompilierzeitfehler ([Konstante Ausdrücke](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2728">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="411e5-2729">Konstante Ausdrücke werden in den folgenden Kontexten auftreten.</span><span class="sxs-lookup"><span data-stu-id="411e5-2729">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="411e5-2730">In diesen Kontexten und tritt auf, ein Fehler während der Kompilierung, wenn ein Ausdruck zum Zeitpunkt der Kompilierung vollständig ausgewertet werden kann.</span><span class="sxs-lookup"><span data-stu-id="411e5-2730">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="411e5-2731">Konstante Deklarationen ([Konstanten](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2731">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="411e5-2732">Member-Enumerationsdeklarationen ([Enumerationsmember](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2732">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="411e5-2733">Standardargumente Listen der formalen Parameter ([Methodenparameter](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="411e5-2733">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="411e5-2734">`case` Bezeichnungen von einem `switch` Anweisung ([der Switch-Anweisung](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2734">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="411e5-2735">`goto case` Anweisungen ([Goto-Anweisung](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2735">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="411e5-2736">Länge in einem Arrayerstellungsausdruck Dimension ([Array-Ausdrücke für die Arrayerstellung](expressions.md#array-creation-expressions)), die einen Initialisierer enthält.</span><span class="sxs-lookup"><span data-stu-id="411e5-2736">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="411e5-2737">Attribute ([Attribute](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="411e5-2737">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="411e5-2738">Eine implizite Konstantenausdruck-Konvertierung ([Konstantenausdruck für implizite Konvertierungen](conversions.md#implicit-constant-expression-conversions)) ermöglicht einen konstanten Ausdruck des Typs `int` zu konvertierenden `sbyte`, `byte`, `short`, `ushort`, `uint`, oder `ulong`, sofern der Wert des konstanten Ausdrucks innerhalb des Bereichs des Zieltyps liegt.</span><span class="sxs-lookup"><span data-stu-id="411e5-2738">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="411e5-2739">Boolesche Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="411e5-2739">Boolean expressions</span></span>

<span data-ttu-id="411e5-2740">Ein *Boolean_expression* ist ein Ausdruck ein Ergebnis vom Typ ergibt `bool`– entweder direkt oder durch Anwenden von `operator true` in bestimmten Kontexten wie im folgenden angegeben.</span><span class="sxs-lookup"><span data-stu-id="411e5-2740">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="411e5-2741">Der bedingte kontrollausdruck einer *If_statement* ([If Anweisung](statements.md#the-if-statement)), *While_statement* ([die while-Anweisung](statements.md#the-while-statement)), *Do_statement* ([die Do-Anweisung](statements.md#the-do-statement)), oder *For_statement* ([der für die Anweisung](statements.md#the-for-statement)) ist eine *Boolean_ Ausdruck*.</span><span class="sxs-lookup"><span data-stu-id="411e5-2741">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="411e5-2742">Der steuernde bedingten Ausdruck die `?:` Operator ([Bedingungsoperator](expressions.md#conditional-operator)) folgt den gleichen Regeln wie eine *Boolean_expression*, aber aus Gründen der Operator wird die Rangfolge klassifiziert als eine *Conditional_or_expression*.</span><span class="sxs-lookup"><span data-stu-id="411e5-2742">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="411e5-2743">Ein *Boolean_expression* `E` ist erforderlich, um einen Wert vom Typ zu erzeugen können `bool`wie folgt:</span><span class="sxs-lookup"><span data-stu-id="411e5-2743">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="411e5-2744">Wenn `E` wird implizit in `bool` und dann zur Laufzeit diese implizite Konvertierung angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="411e5-2744">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="411e5-2745">Andernfalls unäroperator überladungsauflösung ([überladungsauflösung für unäroperator](expressions.md#unary-operator-overload-resolution)) wird verwendet, um eine eindeutige best-Implementierung des Operators finden `true` auf `E`, und diese Implementierung wird zur Laufzeit angewendet.</span><span class="sxs-lookup"><span data-stu-id="411e5-2745">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="411e5-2746">Wenn es wurde kein Operator gefunden wird, tritt ein Fehler während der Bindung.</span><span class="sxs-lookup"><span data-stu-id="411e5-2746">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="411e5-2747">Die `DBBool` Strukturtyp in [Datenbank booleschen Typ](structs.md#database-boolean-type) enthält ein Beispiel für einen Typ, der implementiert `operator true` und `operator false`.</span><span class="sxs-lookup"><span data-stu-id="411e5-2747">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
