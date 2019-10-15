---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704089"
---
# <a name="expressions"></a><span data-ttu-id="20699-101">Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-101">Expressions</span></span>

<span data-ttu-id="20699-102">Ein Ausdruck ist eine Sequenz von Operatoren und Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="20699-103">In diesem Kapitel werden die Syntax, die Reihenfolge der Auswertung von Operanden und Operatoren sowie die Bedeutung von Ausdrücken definiert.</span><span class="sxs-lookup"><span data-stu-id="20699-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="20699-104">Ausdrucks Klassifizierungen</span><span class="sxs-lookup"><span data-stu-id="20699-104">Expression classifications</span></span>

<span data-ttu-id="20699-105">Ein Ausdruck ist eines der folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="20699-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="20699-106">Ein Wert.</span><span class="sxs-lookup"><span data-stu-id="20699-106">A value.</span></span> <span data-ttu-id="20699-107">Jeder Wert verfügt über einen zugeordneten Typ.</span><span class="sxs-lookup"><span data-stu-id="20699-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="20699-108">Eine Variable.</span><span class="sxs-lookup"><span data-stu-id="20699-108">A variable.</span></span> <span data-ttu-id="20699-109">Jede Variable verfügt über einen zugeordneten Typ, nämlich den deklarierten Typ der Variablen.</span><span class="sxs-lookup"><span data-stu-id="20699-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="20699-110">Ein Namespace.</span><span class="sxs-lookup"><span data-stu-id="20699-110">A namespace.</span></span> <span data-ttu-id="20699-111">Ein Ausdruck mit dieser Klassifizierung kann nur auf der linken Seite eines *member_access* ([Member Access](expressions.md#member-access)) angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="20699-112">In jedem anderen Kontext verursacht ein Ausdruck, der als Namespace klassifiziert ist, einen Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="20699-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="20699-113">Ein Typ.</span><span class="sxs-lookup"><span data-stu-id="20699-113">A type.</span></span> <span data-ttu-id="20699-114">Ein Ausdruck mit dieser Klassifizierung kann nur auf der linken Seite eines *member_access* ([Member Access](expressions.md#member-access)) oder als Operand für den `as`-Operator ([den as-Operator](expressions.md#the-as-operator)), den `is`-Operator ([der is-Operator](expressions.md#the-is-operator)) oder den @No __t-6-Operator ([der typeof-Operator](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="20699-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="20699-115">In jedem anderen Kontext verursacht ein Ausdruck, der als Typ klassifiziert ist, einen Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="20699-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="20699-116">Eine Methoden Gruppe, bei der es sich um eine Reihe von überladenen Methoden handelt, die sich aus einer Element Suche ([Member Suche](expressions.md#member-lookup)) ergeben.</span><span class="sxs-lookup"><span data-stu-id="20699-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="20699-117">Eine Methoden Gruppe kann über einen zugeordneten Instanzausdruck und eine zugeordnete Typargument Liste verfügen.</span><span class="sxs-lookup"><span data-stu-id="20699-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="20699-118">Wenn eine Instanzmethode aufgerufen wird, wird das Ergebnis der Auswertung des Instanzausdrucks zur Instanz, die durch `this` ([dieser Zugriff](expressions.md#this-access)) dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="20699-119">Eine Methoden Gruppe ist in einem *invocation_expression* ([Aufruf Ausdrücke](expressions.md#invocation-expressions)), einem *delegate_creation_expression* ([delegaterstellungs Ausdruck](expressions.md#delegate-creation-expressions)) und als linke Seite eines is-Operators zulässig und kann implizit konvertiert in einen kompatiblen Delegattyp ([Methoden Gruppen Konvertierungen](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="20699-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="20699-120">In jedem anderen Kontext verursacht ein Ausdruck, der als Methoden Gruppe klassifiziert ist, einen Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="20699-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="20699-121">Ein NULL-Literale.</span><span class="sxs-lookup"><span data-stu-id="20699-121">A null literal.</span></span> <span data-ttu-id="20699-122">Ein Ausdruck mit dieser Klassifizierung kann implizit in einen Verweistyp oder einen Werte zulässt-Typ konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="20699-123">Eine anonyme Funktion.</span><span class="sxs-lookup"><span data-stu-id="20699-123">An anonymous function.</span></span> <span data-ttu-id="20699-124">Ein Ausdruck mit dieser Klassifizierung kann implizit in einen kompatiblen Delegattyp oder Ausdrucks Strukturtyp konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="20699-125">Ein Eigenschaften Zugriff.</span><span class="sxs-lookup"><span data-stu-id="20699-125">A property access.</span></span> <span data-ttu-id="20699-126">Jeder Eigenschaften Zugriff verfügt über einen zugeordneten Typ, nämlich den Typ der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="20699-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="20699-127">Außerdem kann ein Eigenschaften Zugriff über einen zugeordneten Instanzausdruck verfügen.</span><span class="sxs-lookup"><span data-stu-id="20699-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="20699-128">Wenn ein Accessor (der `get`-oder `set`-Block) eines Instanzeigenschaftenzugriffs aufgerufen wird, wird das Ergebnis der Auswertung des Instanzausdrucks zu der durch `this` ([dieser Zugriff](expressions.md#this-access)) dargestellten Instanz.</span><span class="sxs-lookup"><span data-stu-id="20699-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="20699-129">Ein Ereignis Zugriff.</span><span class="sxs-lookup"><span data-stu-id="20699-129">An event access.</span></span> <span data-ttu-id="20699-130">Jedem Ereignis Zugriff ist ein Typ zugeordnet, nämlich der Typ des Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="20699-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="20699-131">Außerdem kann ein Ereignis Zugriff über einen zugeordneten Instanzausdruck verfügen.</span><span class="sxs-lookup"><span data-stu-id="20699-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="20699-132">Ein Ereignis Zugriff kann als Linker Operand des Operators "`+=`" und "`-=`" ([Ereignis Zuweisung](expressions.md#event-assignment)) angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="20699-133">In jedem anderen Kontext verursacht ein Ausdruck, der als Ereignis Zugriff klassifiziert ist, einen Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="20699-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="20699-134">Ein Indexer-Zugriff.</span><span class="sxs-lookup"><span data-stu-id="20699-134">An indexer access.</span></span> <span data-ttu-id="20699-135">Jedem Indexer-Zugriff ist ein Typ zugeordnet, nämlich der Elementtyp des Indexers.</span><span class="sxs-lookup"><span data-stu-id="20699-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="20699-136">Außerdem verfügt ein Indexer-Zugriff über einen zugeordneten Instanzausdruck und eine zugeordnete Argumentliste.</span><span class="sxs-lookup"><span data-stu-id="20699-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="20699-137">Wenn ein Accessor (der `get`-oder `set`-Block) eines Indexer-Zugriffs aufgerufen wird, wird das Ergebnis der Auswertung des Instanzausdrucks zur durch `this` ([diesem Zugriff](expressions.md#this-access)) dargestellten Instanz, und das Ergebnis der Auswertung der Argumentliste wird zum Parameterliste für den Aufruf.</span><span class="sxs-lookup"><span data-stu-id="20699-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="20699-138">Schweigen.</span><span class="sxs-lookup"><span data-stu-id="20699-138">Nothing.</span></span> <span data-ttu-id="20699-139">Dies tritt auf, wenn der Ausdruck ein Aufruf einer Methode mit dem Rückgabetyp `void` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="20699-140">Ein Ausdruck, der als Nothing klassifiziert ist, ist nur im Kontext eines *statement_expression* ([Ausdrucks Anweisungen](statements.md#expression-statements)) gültig.</span><span class="sxs-lookup"><span data-stu-id="20699-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="20699-141">Das Endergebnis eines Ausdrucks ist nie ein Namespace, ein Typ, eine Methoden Gruppe oder ein Ereignis Zugriff.</span><span class="sxs-lookup"><span data-stu-id="20699-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="20699-142">Wie oben bereits erwähnt, sind diese Kategorien von Ausdrücken zwischenkonstrukte, die nur in bestimmten Kontexten zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="20699-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="20699-143">Ein Eigenschaften Zugriff oder Indexer-Zugriff wird immer als Wert neu klassifiziert, indem ein Aufruf des *Get-Accessor* oder des *set*-Accessors ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="20699-144">Der jeweilige Accessor wird durch den Kontext der Eigenschaft oder des Indexer-Zugriffs bestimmt: Wenn der Zugriff das Ziel einer Zuweisung ist, wird der *Set-Accessor* aufgerufen, um einen neuen Wert zuzuweisen ([einfache Zuweisung](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="20699-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="20699-145">Andernfalls wird der *Get-Accessor* aufgerufen, um den aktuellen Wert zu erhalten ([Werte von Ausdrücken](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="20699-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="20699-146">Werte von Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="20699-146">Values of expressions</span></span>

<span data-ttu-id="20699-147">Die meisten Konstrukte, die einen Ausdruck einschließen, erfordern letztendlich, dass der Ausdruck einen ***Wert***angibt.</span><span class="sxs-lookup"><span data-stu-id="20699-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="20699-148">In solchen Fällen tritt ein Kompilierzeitfehler auf, wenn der tatsächliche Ausdruck einen Namespace, einen Typ, eine Methoden Gruppe oder nichts angibt.</span><span class="sxs-lookup"><span data-stu-id="20699-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="20699-149">Wenn der Ausdruck jedoch einen Eigenschaften Zugriff, einen Indexer-Zugriff oder eine Variable angibt, wird der Wert der Eigenschaft, des Indexers oder der Variablen implizit ersetzt:</span><span class="sxs-lookup"><span data-stu-id="20699-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="20699-150">Der Wert einer Variablen ist einfach der Wert, der zurzeit an dem von der Variablen identifizierten Speicherort gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="20699-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="20699-151">Eine Variable muss als definitiv zugewiesen ([definitive Zuweisung](variables.md#definite-assignment)) betrachtet werden, bevor ihr Wert abgerufen werden kann. andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="20699-152">Der Wert eines Eigenschafts Zugriffs Ausdrucks wird abgerufen, indem der *Get-Accessor* der Eigenschaft aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="20699-153">Wenn die Eigenschaft keinen *Get-Accessor*aufweist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="20699-154">Andernfalls wird ein Funktionsmember-Aufruf ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) ausgeführt, und das Ergebnis des aufzurufenden Ausdrucks wird zum Wert des Eigenschafts Zugriffs Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="20699-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="20699-155">Der Wert eines indexerzugriffsausdrucks wird abgerufen, indem der *Get-Accessor* des Indexers aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="20699-156">Wenn der Indexer keinen *Get-Accessor*aufweist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="20699-157">Andernfalls wird ein Funktionsmember-Aufruf ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) mit der Argumentliste ausgeführt, die dem indexerzugriffsausdruck zugeordnet ist, und das Ergebnis des aufzurufenden Werts wird zum Wert des Indexer-Zugriffs. Begriff.</span><span class="sxs-lookup"><span data-stu-id="20699-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="20699-158">Statische und dynamische Bindung</span><span class="sxs-lookup"><span data-stu-id="20699-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="20699-159">Der Prozess der Bestimmung der Bedeutung eines Vorgangs basierend auf dem Typ oder Wert von konstituierenden Ausdrücken (Argumente, Operanden, Empfänger) wird häufig als ***Bindung***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="20699-160">Zum Beispiel wird die Bedeutung eines Methoden Aufrufes basierend auf dem Typ des Empfängers und der Argumente bestimmt.</span><span class="sxs-lookup"><span data-stu-id="20699-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="20699-161">Die Bedeutung eines Operators wird basierend auf dem Typ seiner Operanden bestimmt.</span><span class="sxs-lookup"><span data-stu-id="20699-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="20699-162">In C# der Bedeutung eines Vorgangs wird in der Regel zur Kompilierzeit bestimmt, basierend auf dem Kompilier Zeittyp der enthaltenen Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="20699-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="20699-163">Wenn ein Ausdruck einen Fehler enthält, wird der Fehler auch vom Compiler erkannt und gemeldet.</span><span class="sxs-lookup"><span data-stu-id="20699-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="20699-164">Diese Vorgehensweise wird als ***statische Bindung***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="20699-165">Wenn ein Ausdruck jedoch ein dynamischer Ausdruck ist (d. h. den Typ `dynamic`), gibt dies an, dass jede Bindung, an der Sie beteiligt ist, auf dem Lauf Zeittyp (d. h. dem tatsächlichen Typ des Objekts, das Sie zur Laufzeit bezeichnet) und nicht auf dem Typ basiert, auf dem Sie sich befindet. Kompilierzeit.</span><span class="sxs-lookup"><span data-stu-id="20699-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="20699-166">Die Bindung eines solchen Vorgangs wird daher bis zu der Zeit verzögert, in der der Vorgang während der Ausführung des Programms ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="20699-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="20699-167">Dies wird als ***dynamische Bindung***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="20699-168">Wenn ein Vorgang dynamisch gebunden ist, wird vom Compiler nur eine kleine oder keine Überprüfung durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="20699-169">Wenn die Lauf Zeitbindung fehlschlägt, werden Fehler zur Laufzeit als Ausnahmen gemeldet.</span><span class="sxs-lookup"><span data-stu-id="20699-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="20699-170">Die folgenden Vorgänge in C# unterliegen der Bindung:</span><span class="sxs-lookup"><span data-stu-id="20699-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="20699-171">Member-Zugriff: `e.M`</span><span class="sxs-lookup"><span data-stu-id="20699-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="20699-172">Methodenaufruf: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="20699-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="20699-173">Delegataufruf: `e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="20699-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="20699-174">Element Zugriff: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="20699-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="20699-175">Objekt Erstellung: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="20699-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="20699-176">Überladene unäre Operatoren: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span><span class="sxs-lookup"><span data-stu-id="20699-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="20699-177">Überladene binäre Operatoren: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, 0, 1, 2, 3, 4, 5, 6, 7, 8</span><span class="sxs-lookup"><span data-stu-id="20699-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="20699-178">Zuweisungs Operatoren: "`=`", "`+=`", "`-=`", "`*=`", "`/=`", "`%=`", "`&=`", "`|=`", "`^=`" @no__t</span><span class="sxs-lookup"><span data-stu-id="20699-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="20699-179">Implizite und explizite Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="20699-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="20699-180">Wenn keine dynamischen Ausdrücke beteiligt sind, C# wird standardmäßig die statische Bindung verwendet. Dies bedeutet, dass die Kompilierzeit Typen von konstituierenden Ausdrücken im Auswahl Vorgang verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="20699-181">Wenn jedoch einer der in den oben aufgeführten Vorgängen aufgeführten Ausdrücke ein dynamischer Ausdruck ist, wird der Vorgang stattdessen dynamisch gebunden.</span><span class="sxs-lookup"><span data-stu-id="20699-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="20699-182">Bindungs Zeit</span><span class="sxs-lookup"><span data-stu-id="20699-182">Binding-time</span></span>

<span data-ttu-id="20699-183">Die statische Bindung erfolgt zur Kompilierzeit, während die dynamische Bindung zur Laufzeit stattfindet.</span><span class="sxs-lookup"><span data-stu-id="20699-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="20699-184">In den folgenden Abschnitten bezieht sich der Begriff " ***Bindungs Zeit*** " entweder auf die Kompilierzeit oder die Laufzeit, je nachdem, wann die Bindung stattfindet.</span><span class="sxs-lookup"><span data-stu-id="20699-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="20699-185">Das folgende Beispiel veranschaulicht die Begriffe der statischen und dynamischen Bindung und der Bindungs Zeit:</span><span class="sxs-lookup"><span data-stu-id="20699-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="20699-186">Die ersten beiden Aufrufe sind statisch gebunden: die Überladung von `Console.WriteLine` wird basierend auf dem Kompilier Zeittyp ihres Arguments ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="20699-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="20699-187">Folglich ist die Bindungs Zeit die Kompilierzeit.</span><span class="sxs-lookup"><span data-stu-id="20699-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="20699-188">Der dritte Aufruf ist dynamisch gebunden: die Überladung von `Console.WriteLine` wird basierend auf dem Lauf Zeittyp des Arguments ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="20699-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="20699-189">Dies liegt daran, dass es sich bei dem Argument um einen dynamischen Ausdruck handelt, dessen Kompilier Zeittyp `dynamic` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="20699-190">Folglich ist die Bindungs Zeit für den dritten Aufruf Lauf Zeit.</span><span class="sxs-lookup"><span data-stu-id="20699-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="20699-191">Dynamische Bindung</span><span class="sxs-lookup"><span data-stu-id="20699-191">Dynamic binding</span></span>

<span data-ttu-id="20699-192">Der Zweck der dynamischen Bindung besteht darin, C# den Programmen die Interaktion mit ***dynamischen Objekten***zu ermöglichen, d. h. Objekten, die nicht den C# normalen Regeln des Typsystems entsprechen.</span><span class="sxs-lookup"><span data-stu-id="20699-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="20699-193">Dynamische Objekte können Objekte aus anderen Programmiersprachen mit unterschiedlichen Typen Systemen sein, oder es handelt sich um Objekte, die Programm gesteuert eingerichtet werden, um Ihre eigene Bindungs Semantik für verschiedene Vorgänge zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="20699-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="20699-194">Der Mechanismus, mit dem ein dynamisches Objekt seine eigene Semantik implementiert, ist die Implementierung definiert.</span><span class="sxs-lookup"><span data-stu-id="20699-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="20699-195">Eine bestimmte Schnittstelle, die wieder implementiert wird, wird von dynamischen Objekten implementiert, um der C# Laufzeit zu signalisieren, dass Sie über eine besondere Semantik verfügen.</span><span class="sxs-lookup"><span data-stu-id="20699-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="20699-196">Wenn also Vorgänge für ein dynamisches Objekt dynamisch gebunden werden, übernehmen Sie die eigene Bindungs Semantik anstelle der C# in diesem Dokument angegebenen.</span><span class="sxs-lookup"><span data-stu-id="20699-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="20699-197">Der Zweck der dynamischen Bindung besteht darin, die Interoperabilität mit dynamischen Objekten zuzulassen, C# aber die dynamische Bindung für alle Objekte, unabhängig davon, ob Sie dynamisch sind oder nicht.</span><span class="sxs-lookup"><span data-stu-id="20699-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="20699-198">Dies ermöglicht eine reibungslosere Integration dynamischer Objekte, da die Ergebnisse von Vorgängen für diese nicht selbst dynamische Objekte sein können, aber immer noch ein Typ ist, der dem Programmierer zur Kompilierzeit unbekannt ist.</span><span class="sxs-lookup"><span data-stu-id="20699-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="20699-199">Außerdem kann die dynamische Bindung helfen, fehleranfälligen reflektionsbasierten Code zu eliminieren, auch wenn keine Objekte an dynamischen Objekten beteiligt sind.</span><span class="sxs-lookup"><span data-stu-id="20699-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="20699-200">In den folgenden Abschnitten wird für jedes Konstrukt in der Sprache exakt beschrieben, wann die dynamische Bindung angewendet wird, welche Kompilierzeit Überprüfung (sofern vorhanden) angewendet wird und wie das Ergebnis und die Ausdrucks Klassifizierung der Kompilierung ist.</span><span class="sxs-lookup"><span data-stu-id="20699-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="20699-201">Typen von konstituierenden Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="20699-201">Types of constituent expressions</span></span>

<span data-ttu-id="20699-202">Wenn ein Vorgang statisch gebunden ist, wird der Typ eines einzelnen Ausdrucks (z. b. ein Empfänger, ein Argument, ein Index oder ein Operand) immer als der Kompilier Zeittyp dieses Ausdrucks betrachtet.</span><span class="sxs-lookup"><span data-stu-id="20699-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="20699-203">Wenn ein Vorgang dynamisch gebunden wird, wird der Typ eines einzelnen Ausdrucks basierend auf dem Kompilier Zeittyp des konstituierenden Ausdrucks auf unterschiedliche Weise bestimmt:</span><span class="sxs-lookup"><span data-stu-id="20699-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="20699-204">Ein konstituierender Ausdruck des Kompilierzeit Typs `dynamic` wird als der Typ des tatsächlichen Werts betrachtet, den der Ausdruck zur Laufzeit auswertet.</span><span class="sxs-lookup"><span data-stu-id="20699-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="20699-205">Ein konstituierender Ausdruck, dessen Kompilier Zeittyp ein Typparameter ist, wird als Typ betrachtet, an den der Typparameter zur Laufzeit gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="20699-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="20699-206">Andernfalls wird der entsprechende Kompilier Zeittyp für den enthaltenen Ausdruck berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="20699-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="20699-207">Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-207">Operators</span></span>

<span data-ttu-id="20699-208">Ausdrücke werden aus ***Operanden*** und ***Operatoren***erstellt.</span><span class="sxs-lookup"><span data-stu-id="20699-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="20699-209">Die Operatoren eines Ausdrucks geben an, welche Operationen auf die Operanden angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="20699-210">Beispiele für Operatoren sind `+`, `-`, `*`, `/` und `new`.</span><span class="sxs-lookup"><span data-stu-id="20699-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="20699-211">Beispiele für Operanden sind Literale, Felder, lokale Variablen und Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="20699-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="20699-212">Es gibt drei Arten von Operatoren:</span><span class="sxs-lookup"><span data-stu-id="20699-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="20699-213">Unäre Operatoren.</span><span class="sxs-lookup"><span data-stu-id="20699-213">Unary operators.</span></span> <span data-ttu-id="20699-214">Die unären Operatoren nehmen einen der Operanden an und verwenden entweder eine Präfix Notation (z. b. `--x`) oder eine Postfix Notation (z. b. `x++`).</span><span class="sxs-lookup"><span data-stu-id="20699-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="20699-215">Binäre Operatoren.</span><span class="sxs-lookup"><span data-stu-id="20699-215">Binary operators.</span></span> <span data-ttu-id="20699-216">Die binären Operatoren nehmen zwei Operanden an und verwenden die Infix-Notation (z. b. `x + y`).</span><span class="sxs-lookup"><span data-stu-id="20699-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="20699-217">Ternärer Operator.</span><span class="sxs-lookup"><span data-stu-id="20699-217">Ternary operator.</span></span> <span data-ttu-id="20699-218">Es ist nur ein ternärer Operator (`?:`) vorhanden. Es werden drei Operanden benötigt und die Infix-Notation (`c ? x : y`) verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="20699-219">Die Reihenfolge der Auswertung von Operatoren in einem Ausdruck wird durch die ***Rangfolge*** und ***Assoziativität*** der Operatoren ([Operator Rangfolge und Assoziativität](expressions.md#operator-precedence-and-associativity)) bestimmt.</span><span class="sxs-lookup"><span data-stu-id="20699-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="20699-220">Operanden in einem Ausdruck werden von links nach rechts ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="20699-221">Beispielsweise wird in `F(i) + G(i++) * H(i)` die Methode `F` mit dem alten Wert von `i` aufgerufen. Anschließend wird Method `G` mit dem alten Wert von `i` aufgerufen, und schließlich wird die Methode `H` mit dem neuen Wert `i` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="20699-222">Dies ist getrennt von und nicht mit der Operator Rangfolge.</span><span class="sxs-lookup"><span data-stu-id="20699-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="20699-223">Bestimmte Operatoren können ***überladen*** werden.</span><span class="sxs-lookup"><span data-stu-id="20699-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="20699-224">Die Operator Überladung ermöglicht das Angeben von benutzerdefinierten Operator Implementierungen für Vorgänge, bei denen einer oder beide der Operanden eine benutzerdefinierte Klasse oder ein Strukturtyp sind ([Operator Überladung](expressions.md#operator-overloading)).</span><span class="sxs-lookup"><span data-stu-id="20699-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="20699-225">Operatorrangfolge und Assoziativität</span><span class="sxs-lookup"><span data-stu-id="20699-225">Operator precedence and associativity</span></span>

<span data-ttu-id="20699-226">Wenn ein Ausdruck mehrere Operatoren enthält, bestimmt die ***Rangfolge*** der Operatoren die Reihenfolge, in der die einzelnen Operatoren ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="20699-227">Beispielsweise wird der Ausdruck `x + y * z` als `x + (y * z)` ausgewertet, da der `*`-Operator eine höhere Rangfolge aufweist als der binäre `+`-Operator.</span><span class="sxs-lookup"><span data-stu-id="20699-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="20699-228">Die Rangfolge eines Operators wird durch die Definition der zugehörigen Grammatik Produktion festgelegt.</span><span class="sxs-lookup"><span data-stu-id="20699-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="20699-229">Ein *additive_expression* besteht z. b. aus einer Sequenz von *multiplicative_expression*s, die durch `+`-oder `-`-Operatoren getrennt sind, sodass der `+`-und `-`-Operatoren eine niedrigere Rangfolge hat als die `*`, `/` und @no__ t-8-Operatoren.</span><span class="sxs-lookup"><span data-stu-id="20699-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="20699-230">In der folgenden Tabelle werden alle Operatoren in der Rangfolge von der höchsten zur niedrigsten aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="20699-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="20699-231">__Bereich__</span><span class="sxs-lookup"><span data-stu-id="20699-231">__Section__</span></span>                                                                                   | <span data-ttu-id="20699-232">__Kategorie__</span><span class="sxs-lookup"><span data-stu-id="20699-232">__Category__</span></span>                | <span data-ttu-id="20699-233">__Operatoren__</span><span class="sxs-lookup"><span data-stu-id="20699-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="20699-234">Primary expressions (Primäre Ausdrücke)</span><span class="sxs-lookup"><span data-stu-id="20699-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="20699-235">Primär</span><span class="sxs-lookup"><span data-stu-id="20699-235">Primary</span></span>                     | <span data-ttu-id="20699-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="20699-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="20699-237">Unary operators (Unäre Operatoren)</span><span class="sxs-lookup"><span data-stu-id="20699-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="20699-238">Unär</span><span class="sxs-lookup"><span data-stu-id="20699-238">Unary</span></span>                       | <span data-ttu-id="20699-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="20699-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="20699-240">Arithmetic operators (Arithmetische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="20699-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="20699-241">Multiplikativ</span><span class="sxs-lookup"><span data-stu-id="20699-241">Multiplicative</span></span>              | <span data-ttu-id="20699-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="20699-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="20699-243">Arithmetic operators (Arithmetische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="20699-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="20699-244">Additiv</span><span class="sxs-lookup"><span data-stu-id="20699-244">Additive</span></span>                    | <span data-ttu-id="20699-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="20699-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="20699-246">Shift operators (Schiebeoperatoren)</span><span class="sxs-lookup"><span data-stu-id="20699-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="20699-247">Shift</span><span class="sxs-lookup"><span data-stu-id="20699-247">Shift</span></span>                       | <span data-ttu-id="20699-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="20699-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="20699-249">Relational and type-testing operators (Relationale und Typtestoperatoren)</span><span class="sxs-lookup"><span data-stu-id="20699-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="20699-250">Relational und Typtest</span><span class="sxs-lookup"><span data-stu-id="20699-250">Relational and type testing</span></span> | <span data-ttu-id="20699-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="20699-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="20699-252">Relational and type-testing operators (Relationale und Typtestoperatoren)</span><span class="sxs-lookup"><span data-stu-id="20699-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="20699-253">Gleichheit</span><span class="sxs-lookup"><span data-stu-id="20699-253">Equality</span></span>                    | <span data-ttu-id="20699-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="20699-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="20699-255">Logical operators (Logische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="20699-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="20699-256">Logisches AND</span><span class="sxs-lookup"><span data-stu-id="20699-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="20699-257">Logical operators (Logische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="20699-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="20699-258">Logisches XOR</span><span class="sxs-lookup"><span data-stu-id="20699-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="20699-259">Logical operators (Logische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="20699-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="20699-260">Logisches OR</span><span class="sxs-lookup"><span data-stu-id="20699-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="20699-261">Conditional logical operators (Bedingte logische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="20699-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="20699-262">Bedingtes AND</span><span class="sxs-lookup"><span data-stu-id="20699-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="20699-263">Conditional logical operators (Bedingte logische Operatoren)</span><span class="sxs-lookup"><span data-stu-id="20699-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="20699-264">Bedingtes OR</span><span class="sxs-lookup"><span data-stu-id="20699-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="20699-265">The null coalescing operator (Der NULL-Sammeloperator)</span><span class="sxs-lookup"><span data-stu-id="20699-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="20699-266">NULL-Sammeloperator</span><span class="sxs-lookup"><span data-stu-id="20699-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="20699-267">Conditional operator (Bedingte Operatoren)</span><span class="sxs-lookup"><span data-stu-id="20699-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="20699-268">Bedingt</span><span class="sxs-lookup"><span data-stu-id="20699-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="20699-269">[Zuweisungs Operatoren](expressions.md#assignment-operators), [Anonyme Funktions Ausdrücke](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="20699-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="20699-270">Zuweisungs- und Lambda-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-270">Assignment and lambda expression</span></span> | <span data-ttu-id="20699-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="20699-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="20699-272">Wenn ein Operand zwischen zwei Operatoren mit der gleichen Rangfolge auftritt, steuert die Assoziativität der Operatoren die Reihenfolge, in der die Vorgänge ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="20699-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="20699-273">Mit Ausnahme der Zuweisungs Operatoren und des NULL-Sammel Operators sind alle binären Operatoren ***Links***bündig. Dies bedeutet, dass Vorgänge von links nach rechts ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="20699-274">`x + y + z` wird beispielsweise als `(x + y) + z` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="20699-275">Die Zuweisungs Operatoren, der NULL-Sammel Operator und der bedingte Operator (`?:`) sind ***Rechts assoziativ***, was bedeutet, dass Vorgänge von rechts nach links ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="20699-276">`x = y = z` wird beispielsweise als `x = (y = z)` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="20699-277">Rangfolge und Assoziativität können mit Klammern gesteuert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="20699-278">In `x + y * z` wird beispielsweise zuerst `y` mit `z` multipliziert und dann das Ergebnis zu `x` addiert, aber in `(x + y) * z` werden zunächst `x` und `y` addiert, und dann wird das Ergebnis mit `z` multipliziert.</span><span class="sxs-lookup"><span data-stu-id="20699-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="20699-279">Überladen von Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-279">Operator overloading</span></span>

<span data-ttu-id="20699-280">Alle unären und binären Operatoren verfügen über vordefinierte Implementierungen, die automatisch in jedem Ausdruck verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="20699-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="20699-281">Zusätzlich zu den vordefinierten Implementierungen können benutzerdefinierte Implementierungen durch Einschließen von `operator`-Deklarationen in Klassen und Strukturen ([Operatoren](classes.md#operators)) eingeführt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="20699-282">Implementierungen von benutzerdefinierten Operatoren haben immer Vorrang vor vordefinierten Operator Implementierungen: Nur wenn keine anwendbaren benutzerdefinierten Operator Implementierungen vorhanden sind, werden die vordefinierten Operator Implementierungen in Erwägung gezogen, wie unter [unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution) und [binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="20699-283">Die ***über ladbaren unären Operatoren*** sind:</span><span class="sxs-lookup"><span data-stu-id="20699-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="20699-284">Obwohl `true` und `false` nicht explizit in Ausdrücken verwendet werden (und daher nicht in der Rang folgen Tabelle in der [Operator Rangfolge und Assoziativität](expressions.md#operator-precedence-and-associativity)enthalten sind), werden Sie als Operatoren angesehen, da Sie in mehreren Ausdrücken aufgerufen werden. Kontexte: boolesche Ausdrücke ([boolesche Ausdrücke](expressions.md#boolean-expressions)) und Ausdrücke, die den bedingten ([bedingten Operator](expressions.md#conditional-operator)) und bedingte logische Operatoren ([bedingte logische Operatoren](expressions.md#conditional-logical-operators)) einbeziehen.</span><span class="sxs-lookup"><span data-stu-id="20699-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="20699-285">Die ***über ladbaren binären Operatoren*** sind:</span><span class="sxs-lookup"><span data-stu-id="20699-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="20699-286">Nur die oben aufgeführten Operatoren können überladen werden.</span><span class="sxs-lookup"><span data-stu-id="20699-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="20699-287">Insbesondere ist es nicht möglich, Element Zugriffe, Methodenaufrufe oder die Operatoren "`=`", "`&&`", "`||`", "`??`", "`?:`", "`=>`", "`checked`", "`typeof`", "0" und "1" zu überladen.</span><span class="sxs-lookup"><span data-stu-id="20699-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="20699-288">Wenn ein binärer Operator überladen ist, wird der zugehörige Zuweisungsoperator, sofern er vorhanden ist, auch implizit überladen.</span><span class="sxs-lookup"><span data-stu-id="20699-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="20699-289">Beispielsweise ist eine Überladung des Operators `*` auch eine Überladung des Operators `*=`.</span><span class="sxs-lookup"><span data-stu-id="20699-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="20699-290">Dies wird in der [Verbund Zuweisung](expressions.md#compound-assignment)weiter unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="20699-291">Beachten Sie, dass der Zuweisungs Operator selbst (`=`) nicht überladen werden kann.</span><span class="sxs-lookup"><span data-stu-id="20699-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="20699-292">Eine Zuweisung führt immer eine einfache bitweise Kopie eines Werts in eine Variable aus.</span><span class="sxs-lookup"><span data-stu-id="20699-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="20699-293">Umwandlungs Vorgänge, wie z. b. `(T)x`, werden durch die Bereitstellung von benutzerdefinierten Konvertierungen ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)) überladen.</span><span class="sxs-lookup"><span data-stu-id="20699-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="20699-294">Der Element Zugriff, wie z. b. `a[x]`, wird nicht als über ladbarer Operator angesehen.</span><span class="sxs-lookup"><span data-stu-id="20699-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="20699-295">Stattdessen wird die benutzerdefinierte Indizierung durch Indexer ([Indexer](classes.md#indexers)) unterstützt.</span><span class="sxs-lookup"><span data-stu-id="20699-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="20699-296">In Ausdrücken wird mithilfe der Operator Notation auf Operatoren verwiesen, und in Deklarationen wird auf Operatoren mithilfe der funktionalen Notation verwiesen.</span><span class="sxs-lookup"><span data-stu-id="20699-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="20699-297">In der folgenden Tabelle wird die Beziehung zwischen Operator-und Funktions Notizen für unäre und binäre Operatoren veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="20699-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="20699-298">Im ersten Eintrag bezeichnet *op* alle über ladbaren unären Präfix Operatoren.</span><span class="sxs-lookup"><span data-stu-id="20699-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="20699-299">Im zweiten Eintrag bezeichnet *op* den unären postfix `++`-und `--`-Operatoren.</span><span class="sxs-lookup"><span data-stu-id="20699-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="20699-300">Im dritten Eintrag bezeichnet *op* jeden über ladbaren binären Operator.</span><span class="sxs-lookup"><span data-stu-id="20699-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="20699-301">__Operator Notation__</span><span class="sxs-lookup"><span data-stu-id="20699-301">__Operator notation__</span></span> | <span data-ttu-id="20699-302">__Funktionale Notation__</span><span class="sxs-lookup"><span data-stu-id="20699-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="20699-303">Benutzerdefinierte Operator Deklarationen erfordern immer, dass mindestens einer der Parameter vom Klassen-oder Strukturtyp ist, der die Operator Deklaration enthält.</span><span class="sxs-lookup"><span data-stu-id="20699-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="20699-304">Daher ist es nicht möglich, dass ein benutzerdefinierter Operator dieselbe Signatur wie ein vordefinierter Operator hat.</span><span class="sxs-lookup"><span data-stu-id="20699-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="20699-305">Benutzerdefinierte Operator Deklarationen können die Syntax, Rangfolge oder Assoziativität eines Operators nicht ändern.</span><span class="sxs-lookup"><span data-stu-id="20699-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="20699-306">Beispielsweise ist der `/`-Operator immer ein binärer Operator, verfügt immer über die in [Operator Rangfolge und Assoziativität](expressions.md#operator-precedence-and-associativity)angegebene Rang folgen Ebene und ist immer links assoziativ.</span><span class="sxs-lookup"><span data-stu-id="20699-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="20699-307">Obwohl es möglich ist, dass ein benutzerdefinierter Operator jede beliebige Berechnung durchführt, wird dringend davon abgeraten, Implementierungen, die andere Ergebnisse als diejenigen ergeben, die intuitiv erwartet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="20699-308">Beispielsweise sollte eine Implementierung von `operator ==` die beiden Operanden auf Gleichheit vergleichen und ein entsprechendes `bool`-Ergebnis zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="20699-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="20699-309">Die Beschreibungen der einzelnen Operatoren in [Primary Expressions](expressions.md#primary-expressions) durch [bedingte logische Operatoren](expressions.md#conditional-logical-operators) geben die vordefinierten Implementierungen der Operatoren sowie alle zusätzlichen Regeln an, die für die einzelnen Operatoren gelten.</span><span class="sxs-lookup"><span data-stu-id="20699-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="20699-310">In den Beschreibungen werden die Begriffe ***unäre Operator Überladungs Auflösung***, ***binäre Operator Überladungs Auflösung***und ***numerische***herauf Stufung verwendet. diese Definitionen finden Sie in den folgenden Abschnitten.</span><span class="sxs-lookup"><span data-stu-id="20699-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="20699-311">Überladungs Auflösung für unären Operator</span><span class="sxs-lookup"><span data-stu-id="20699-311">Unary operator overload resolution</span></span>

<span data-ttu-id="20699-312">Ein Vorgang in der Form `op x` oder `x op`, bei dem `op` ein über ladbarer unärer Operator ist, und `x` ein Ausdruck des Typs `X` ist, wird wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="20699-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="20699-313">Der Satz von benutzerdefinierten Operatoren, die von `X` für den Vorgang `operator op(x)` bereitgestellt werden, wird mithilfe der Regeln von [benutzerdefinierten Operatoren des Kandidaten](expressions.md#candidate-user-defined-operators)bestimmt.</span><span class="sxs-lookup"><span data-stu-id="20699-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="20699-314">Wenn der Satz von benutzerdefinierten Operatoren des Kandidaten nicht leer ist, wird dies zur Gruppe der Kandidaten Operatoren für den Vorgang.</span><span class="sxs-lookup"><span data-stu-id="20699-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="20699-315">Andernfalls werden die vordefinierten unären `operator op`-Implementierungen, einschließlich ihrer angehobenen Formulare, zur Gruppe der Kandidaten Operatoren für den Vorgang.</span><span class="sxs-lookup"><span data-stu-id="20699-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="20699-316">Die vordefinierten Implementierungen eines angegebenen Operators werden in der Beschreibung des Operators ([primär Ausdrücke](expressions.md#primary-expressions) und [unäre Operatoren](expressions.md#unary-operators)) angegeben.</span><span class="sxs-lookup"><span data-stu-id="20699-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="20699-317">Die Regeln für die Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution) werden auf den Satz von Kandidaten Operatoren angewendet, um den besten Operator in Bezug auf die Argumentliste `(x)` auszuwählen, und dieser Operator wird zum Ergebnis der Überladungs Auflösung.</span><span class="sxs-lookup"><span data-stu-id="20699-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="20699-318">Wenn bei der Überladungs Auflösung nicht der einzige beste Operator ausgewählt werden kann, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="20699-319">Binäre Operator Überladungs Auflösung</span><span class="sxs-lookup"><span data-stu-id="20699-319">Binary operator overload resolution</span></span>

<span data-ttu-id="20699-320">Ein Vorgang der Form `x op y`, wobei `op` ein über ladbarer binärer Operator ist, `x` ein Ausdruck vom Typ `X` und `y` ein Ausdruck des Typs `Y` ist, wird wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="20699-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="20699-321">Der Satz von benutzerdefinierten Operatoren, die von `X` und `Y` für den Vorgang `operator op(x,y)` bereitgestellt werden, wird bestimmt.</span><span class="sxs-lookup"><span data-stu-id="20699-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="20699-322">Der Satz besteht aus der Vereinigung der Kandidaten Operatoren, die von `X` bereitgestellt werden, und den Kandidaten Operatoren, die von `Y` bereitgestellt werden, wobei jede mithilfe der Regeln von [benutzerdefinierten Operatoren des Kandidaten](expressions.md#candidate-user-defined-operators)bestimmt wird</span><span class="sxs-lookup"><span data-stu-id="20699-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="20699-323">Wenn `X` und `Y` denselben Typ haben oder wenn `X` und `Y` von einem gemeinsamen Basistyp abgeleitet sind, treten freigegebene Kandidaten Operatoren nur einmal in der kombinierten Menge auf.</span><span class="sxs-lookup"><span data-stu-id="20699-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="20699-324">Wenn der Satz von benutzerdefinierten Operatoren des Kandidaten nicht leer ist, wird dies zur Gruppe der Kandidaten Operatoren für den Vorgang.</span><span class="sxs-lookup"><span data-stu-id="20699-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="20699-325">Andernfalls werden die vordefinierten binären `operator op`-Implementierungen, einschließlich ihrer angehobenen Formulare, zur Gruppe der Kandidaten Operatoren für den Vorgang.</span><span class="sxs-lookup"><span data-stu-id="20699-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="20699-326">Die vordefinierten Implementierungen eines angegebenen Operators werden in der Beschreibung des Operators ([arithmetische Operatoren](expressions.md#arithmetic-operators) durch [bedingte logische Operatoren](expressions.md#conditional-logical-operators)) angegeben.</span><span class="sxs-lookup"><span data-stu-id="20699-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="20699-327">Bei vordefinierten Aufzählungs-und delegatoperatoren sind die einzigen Operatoren, die von einem Aufzählungs-oder Delegattyp definiert werden, der der Bindungstyp von einem der Operanden ist.</span><span class="sxs-lookup"><span data-stu-id="20699-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="20699-328">Die Regeln für die Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution) werden auf den Satz von Kandidaten Operatoren angewendet, um den besten Operator in Bezug auf die Argumentliste `(x,y)` auszuwählen, und dieser Operator wird zum Ergebnis der Überladungs Auflösung.</span><span class="sxs-lookup"><span data-stu-id="20699-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="20699-329">Wenn bei der Überladungs Auflösung nicht der einzige beste Operator ausgewählt werden kann, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="20699-330">Benutzerdefinierte Operatoren für Kandidaten</span><span class="sxs-lookup"><span data-stu-id="20699-330">Candidate user-defined operators</span></span>

<span data-ttu-id="20699-331">Bei Angabe eines Typs `T` und eines Vorgangs `operator op(A)`, wobei `op` ein über ladbarer Operator und `A` eine Argumentliste ist, wird der von `T` für `operator op(A)` bereitgestellte Satz von Kandidaten benutzerdefinierten Operatoren wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="20699-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="20699-332">Bestimmen Sie den Typ `T0`.</span><span class="sxs-lookup"><span data-stu-id="20699-332">Determine the type `T0`.</span></span> <span data-ttu-id="20699-333">Wenn `T` ein Typ ist, der NULL-Werte zulässt, ist `T0` der zugrunde liegende Typ, andernfalls ist `T0` gleich `T`.</span><span class="sxs-lookup"><span data-stu-id="20699-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="20699-334">Wenn mindestens ein Operator ([anwendbarer Funktionsmember](expressions.md#applicable-function-member)) in Bezug auf die Argumentliste `A` für alle `operator op`-Deklarationen in `T0` und alle von diesen Operatoren aufgelegten Formen dieser Operatoren anwendbar ist, besteht der Satz der Kandidaten Operatoren aus allen solchen anwendbare Operatoren in `T0`.</span><span class="sxs-lookup"><span data-stu-id="20699-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="20699-335">Andernfalls ist der Satz von Kandidaten Operatoren leer, wenn `T0` `object` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="20699-336">Andernfalls ist der von `T0` bereitgestellte Satz von Kandidaten Operatoren der Satz von Kandidaten Operatoren, die von der direkten Basisklasse `T0` bereitgestellt werden, oder die effektive Basisklasse von `T0`, wenn `T0` ein Typparameter ist.</span><span class="sxs-lookup"><span data-stu-id="20699-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="20699-337">Numerische Aktionen</span><span class="sxs-lookup"><span data-stu-id="20699-337">Numeric promotions</span></span>

<span data-ttu-id="20699-338">Die numerische herauf Stufung besteht aus dem automatischen Ausführen bestimmter impliziter Konvertierungen der Operanden der vordefinierten unären und binären numerischen Operatoren.</span><span class="sxs-lookup"><span data-stu-id="20699-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="20699-339">Die numerische herauf Stufung ist kein eindeutiger Mechanismus, sondern wirkt sich eher auf die Anwendung der Überladungs Auflösung auf die vordefinierten Operatoren aus.</span><span class="sxs-lookup"><span data-stu-id="20699-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="20699-340">Die numerische herauf Stufung wirkt sich nicht auf die Auswertung von benutzerdefinierten Operatoren aus, obwohl benutzerdefinierte Operatoren implementiert werden können, um ähnliche Effekte zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="20699-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="20699-341">Als Beispiel für die numerische herauf Stufung sollten Sie die vordefinierten Implementierungen des binären `*`-Operators beachten:</span><span class="sxs-lookup"><span data-stu-id="20699-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="20699-342">Wenn Regeln zur Überladungs Auflösung ([Überladungs](expressions.md#overload-resolution)Auflösung) auf diese Gruppe von Operatoren angewendet werden, besteht der Effekt darin, den ersten Operator auszuwählen, für den implizite Konvertierungen aus den Operanden Typen vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="20699-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="20699-343">Beispiel: für den Vorgang `b * s`, wobei `b` ein `byte` und `s` ein `short` ist, wählt die Überladungs Auflösung `operator *(int,int)` als optimalen Operator aus.</span><span class="sxs-lookup"><span data-stu-id="20699-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="20699-344">Folglich ist der Effekt, dass `b` und `s` in `int` konvertiert werden und der Ergebnistyp `int` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="20699-345">Entsprechend wählt die Überladungs Auflösung für den Vorgang `i * d`, wobei `i` ein `int` und `d` ein `double` ist, `operator *(double,double)` als besten Operator aus.</span><span class="sxs-lookup"><span data-stu-id="20699-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="20699-346">Unäre numerische Aktionen</span><span class="sxs-lookup"><span data-stu-id="20699-346">Unary numeric promotions</span></span>

<span data-ttu-id="20699-347">Unäre numerische herauf Stufung tritt für die Operanden der vordefinierten `+`-, `-`-und `~`-Operatoren auf.</span><span class="sxs-lookup"><span data-stu-id="20699-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="20699-348">Bei der unären numerischen herauf Stufung werden nur die Operanden des Typs `sbyte`, `byte`, `short`, `ushort` oder `char` in den Typ "`int`" umgerechnet.</span><span class="sxs-lookup"><span data-stu-id="20699-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="20699-349">Außerdem konvertiert die unäre `-`-Operator Operanden vom Typ `uint` in den Typ `long`.</span><span class="sxs-lookup"><span data-stu-id="20699-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="20699-350">Binäre numerische Aktionen</span><span class="sxs-lookup"><span data-stu-id="20699-350">Binary numeric promotions</span></span>

<span data-ttu-id="20699-351">Binäre numerische herauf Stufung tritt für die Operanden der vordefinierten `+`-, `-`-, `*`-, `/`-, `%`-, `&`-, `|`-, `^`-, `==`-, `!=`-, 0-, 1-und 3-Binär Operatoren auf</span><span class="sxs-lookup"><span data-stu-id="20699-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="20699-352">Die binäre numerische herauf Stufung konvertiert beide Operanden implizit in einen gemeinsamen Typ, der bei nicht relationalen Operatoren auch zum Ergebnistyp des Vorgangs wird.</span><span class="sxs-lookup"><span data-stu-id="20699-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="20699-353">Die binäre numerische herauf Stufung besteht aus der Anwendung der folgenden Regeln in der Reihenfolge, in der Sie angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="20699-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="20699-354">Wenn einer der beiden Operanden vom Typ `decimal` ist, wird der andere Operand in den Typ `decimal` konvertiert, oder es tritt ein Bindungs Fehler auf, wenn der andere Operand vom Typ `float` oder `double` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="20699-355">Andernfalls, wenn einer der beiden Operanden vom Typ `double` ist, wird der andere Operand in den Typ `double` konvertiert.</span><span class="sxs-lookup"><span data-stu-id="20699-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="20699-356">Andernfalls, wenn einer der beiden Operanden vom Typ `float` ist, wird der andere Operand in den Typ `float` konvertiert.</span><span class="sxs-lookup"><span data-stu-id="20699-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="20699-357">Andernfalls, wenn einer der beiden Operanden vom Typ `ulong` ist, wird der andere Operand in den Typ `ulong` konvertiert, oder es tritt ein Bindungs Fehler auf, wenn der andere Operand vom Typ `sbyte`, `short`, `int` oder `long` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="20699-358">Andernfalls, wenn einer der beiden Operanden vom Typ `long` ist, wird der andere Operand in den Typ `long` konvertiert.</span><span class="sxs-lookup"><span data-stu-id="20699-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="20699-359">Andernfalls, wenn einer der beiden Operanden vom Typ `uint` und der andere Operand vom Typ `sbyte`, `short` oder `int` ist, werden beide Operanden in den Typ `long` konvertiert.</span><span class="sxs-lookup"><span data-stu-id="20699-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="20699-360">Andernfalls, wenn einer der beiden Operanden vom Typ `uint` ist, wird der andere Operand in den Typ `uint` konvertiert.</span><span class="sxs-lookup"><span data-stu-id="20699-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="20699-361">Andernfalls werden beide Operanden in den Typ `int` konvertiert.</span><span class="sxs-lookup"><span data-stu-id="20699-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="20699-362">Beachten Sie, dass die erste Regel Vorgänge, die den `decimal`-Typ mit den Typen `double` und `float` mischen, nicht zulässt.</span><span class="sxs-lookup"><span data-stu-id="20699-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="20699-363">Die Regel folgt der Tatsache, dass keine impliziten Konvertierungen zwischen dem `decimal`-Typ und den Typen `double` und `float` vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="20699-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="20699-364">Beachten Sie außerdem, dass es nicht möglich ist, dass ein Operand vom Typ `ulong` ist, wenn der andere Operand einen ganzzahligen Typ mit Vorzeichen hat.</span><span class="sxs-lookup"><span data-stu-id="20699-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="20699-365">Der Grund dafür ist, dass kein ganzzahliger Typ vorhanden ist, der den vollständigen Bereich von `ulong` und die ganzzahligen Typen mit Vorzeichen darstellen kann.</span><span class="sxs-lookup"><span data-stu-id="20699-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="20699-366">In beiden oben genannten Fällen kann ein Umwandlungs Ausdruck verwendet werden, um einen Operanden explizit in einen Typ zu konvertieren, der mit dem anderen Operanden kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="20699-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="20699-367">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="20699-368">ein Fehler bei der Bindungs Zeit tritt auf, weil ein `decimal` nicht mit einem `double` multipliziert werden kann.</span><span class="sxs-lookup"><span data-stu-id="20699-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="20699-369">Der Fehler wird behoben, indem der zweite Operand wie folgt explizit in `decimal`-Wert umgerechnet wird:</span><span class="sxs-lookup"><span data-stu-id="20699-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="20699-370">Gesteigerte Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-370">Lifted operators</span></span>

<span data-ttu-id="20699-371">Mithilfe von aufzurufenden ***Operatoren*** können vordefinierte und benutzerdefinierte Operatoren, die nicht auf NULL festleg Bare Werttypen angewendet werden, auch mit null-fähigen Formularen dieser Typen verwendet werden</span><span class="sxs-lookup"><span data-stu-id="20699-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="20699-372">Gesteigerte Operatoren werden aus vordefinierten und benutzerdefinierten Operatoren erstellt, die bestimmte Anforderungen erfüllen, wie im folgenden beschrieben:</span><span class="sxs-lookup"><span data-stu-id="20699-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="20699-373">Für die unären Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="20699-374">eine angehobene Form eines Operators ist vorhanden, wenn der Operand und die Ergebnistypen beide Werttypen sind, die keine NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="20699-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="20699-375">Das angefügte Formular wird erstellt, indem ein einzelner `?`-Modifizierer zu den Operanden-und Ergebnistypen hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="20699-376">Der Operator "angehoben" erzeugt einen NULL-Wert, wenn der Operand NULL ist.</span><span class="sxs-lookup"><span data-stu-id="20699-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="20699-377">Andernfalls entpackt der angehobene Operator den Operanden, wendet den zugrunde liegenden Operator an und umschließt das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="20699-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="20699-378">Für die binären Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="20699-379">ein angezeigter Typ eines Operators ist vorhanden, wenn der Operand und die Ergebnistypen alle Werttypen darstellen, die keine NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="20699-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="20699-380">Das angefügte Formular wird erstellt, indem jedem Operand und Ergebnistyp ein einzelner `?`-Modifizierer hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="20699-381">Der gesteigerte Operator erzeugt einen NULL-Wert, wenn ein oder beide Operanden NULL sind (eine Ausnahme ist die `&`-und `|`-Operatoren des `bool?`-Typs, wie in [booleschen logischen Operatoren](expressions.md#boolean-logical-operators)beschrieben).</span><span class="sxs-lookup"><span data-stu-id="20699-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="20699-382">Andernfalls entpackt der angehobene Operator die Operanden, wendet den zugrunde liegenden Operator an und umschließt das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="20699-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="20699-383">Für die Gleichheits Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="20699-384">eine angehobene Form eines Operators ist vorhanden, wenn die Operanden Typen sowohl nicht auf NULL festleg Bare Werttypen als auch, wenn der Ergebnistyp `bool` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="20699-385">Das angefügte Formular wird erstellt, indem jedem Operanden ein einzelner `?`-Modifizierer hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="20699-386">Der Operator "angehoben" berücksichtigt zwei NULL-Werte gleich, und ein NULL-Wert entspricht keinem Wert, der ungleich NULL ist.</span><span class="sxs-lookup"><span data-stu-id="20699-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="20699-387">Wenn beide Operanden ungleich NULL sind, entpackt der angehobene Operator die Operanden und wendet den zugrunde liegenden Operator an, um das Ergebnis "`bool`" zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="20699-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="20699-388">Für die relationalen Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="20699-389">eine angehobene Form eines Operators ist vorhanden, wenn die Operanden Typen sowohl nicht auf NULL festleg Bare Werttypen als auch, wenn der Ergebnistyp `bool` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="20699-390">Das angefügte Formular wird erstellt, indem jedem Operanden ein einzelner `?`-Modifizierer hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="20699-391">Der Operator "angehoben" erzeugt den Wert `false`, wenn ein oder beide Operanden NULL sind.</span><span class="sxs-lookup"><span data-stu-id="20699-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="20699-392">Andernfalls entpackt der angehobene Operator die Operanden und wendet den zugrunde liegenden Operator an, um das Ergebnis "`bool`" zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="20699-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="20699-393">Mitglieder Suche</span><span class="sxs-lookup"><span data-stu-id="20699-393">Member lookup</span></span>

<span data-ttu-id="20699-394">Bei der Suche nach Membern handelt es sich um den Prozess, bei dem die Bedeutung eines Namens im Kontext eines Typs bestimmt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="20699-395">Eine Member-Suche kann als Teil der Auswertung eines *Simple_name* ([simple names](expressions.md#simple-names)) oder eines *member_access* ([Member Access](expressions.md#member-access)) in einem Ausdruck auftreten.</span><span class="sxs-lookup"><span data-stu-id="20699-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="20699-396">Wenn *Simple_name* oder *member_access* als *primary_expression* einer *invocation_expression* -Methode ([Methodenaufrufe](expressions.md#method-invocations)) auftritt, wird der Member als aufgerufen bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="20699-397">Wenn ein Member eine Methode oder ein Ereignis ist, oder wenn es sich um eine Konstante, ein Feld oder eine Eigenschaft eines Delegattyps (Delegaten) oder des Typs @no__t-[1 (](delegates.md)[der dynamische Typ](types.md#the-dynamic-type)) handelt, wird der Member als *Aufruf Kabel*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="20699-398">Die Element Suche berücksichtigt nicht nur den Namen eines Members, sondern auch die Anzahl der Typparameter, die der Member hat und ob auf den Member zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="20699-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="20699-399">Für die Suche nach Membern verfügen generische Methoden und generische generische Typen über die Anzahl der Typparameter, die in den jeweiligen Deklarationen angegeben sind, und alle anderen Member haben keine Typparameter.</span><span class="sxs-lookup"><span data-stu-id="20699-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="20699-400">Eine Member-Suche mit dem Namen @ no__t-0 mit den Parametern `K` @ no__t-2type in einem Typ @ no__t-3 wird wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="20699-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="20699-401">Zuerst wird ein Satz barrierefreier Member mit dem Namen @ no__t-0 bestimmt:</span><span class="sxs-lookup"><span data-stu-id="20699-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="20699-402">Wenn `T` ein Typparameter ist, ist die Menge die Menge der zugreif baren Member mit dem Namen @ no__t-1 in jedem der Typen, die als primäre Einschränkung oder sekundäre Einschränkung ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) für @ no__t-3 angegeben sind, zusammen mit dem Satz von Barrierefreie Member mit dem Namen @ no__t-4 in `object`.</span><span class="sxs-lookup"><span data-stu-id="20699-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="20699-403">Andernfalls besteht der Satz aus allen zugänglichen Membern ([Member Access](basic-concepts.md#member-access)) mit dem Namen @ no__t-1 in @ no__t-2, einschließlich geerbten Membern und der zugänglichen Member mit dem Namen @ no__t-3 in `object`.</span><span class="sxs-lookup"><span data-stu-id="20699-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="20699-404">Wenn `T` ein konstruierter Typ ist, wird der Satz von Membern durch Ersetzen von Typargumenten abgerufen, wie in [Members von konstruierten Typen](classes.md#members-of-constructed-types)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="20699-405">Member, die einen `override`-Modifizierer einschließen, werden aus dem Satz ausgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="20699-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="20699-406">Wenn `K` 0 (null) ist, werden alle Typen, deren Deklarationen Typparameter enthalten, entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="20699-407">Wenn `K` nicht 0 (null) ist, werden alle Elemente mit einer anderen Anzahl von Typparametern entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="20699-408">Beachten Sie Folgendes: Wenn `K` 0 (null) ist, werden Methoden mit Typparametern nicht entfernt, da derTyprückschluss-Prozess ([Typrückschluss](expressions.md#type-inference)) möglicherweise die Typargumente ableiten kann.</span><span class="sxs-lookup"><span data-stu-id="20699-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="20699-409">Wenn der Member *aufgerufen*wird, werden alle nicht*Aufruf* baren Member aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="20699-410">Als nächstes werden Elemente, die von anderen Membern ausgeblendet werden, aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="20699-411">Für jedes Element `S.M` im Satz, wobei `S` der Typ ist, in dem der Member @ no__t-2 deklariert ist, werden die folgenden Regeln angewendet:</span><span class="sxs-lookup"><span data-stu-id="20699-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="20699-412">Wenn `M` eine Konstante, ein Feld, eine Eigenschaft, ein Ereignis oder ein Enumerationsmember ist, werden alle Member, die in einem Basistyp von `S` deklariert sind, aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="20699-413">Wenn `M` eine Typdeklaration ist, werden alle nicht-Typen, die in einem Basistyp von `S` deklariert sind, aus dem Satz entfernt, und alle Typdeklarationen mit der gleichen Anzahl von Typparametern wie `M`, die in einem Basistyp von `S` deklariert sind, werden aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="20699-414">Wenn `M` eine Methode ist, werden alle nicht-Methoden Elemente, die in einem Basistyp von `S` deklariert sind, aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="20699-415">Als nächstes werden Schnittstellenmember, die von Klassenmembern ausgeblendet werden, aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="20699-416">Dieser Schritt hat nur Auswirkungen, wenn `T` ein Typparameter ist und `T` sowohl eine effektive Basisklasse als `object` als auch eine nicht leere effektive Schnittstellen Menge aufweist ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="20699-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="20699-417">Für jedes Element `S.M` im Satz, wobei `S` der Typ ist, in dem der Member `M` deklariert ist, werden die folgenden Regeln angewendet, wenn `S` eine Klassen Deklaration außer `object` ist:</span><span class="sxs-lookup"><span data-stu-id="20699-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="20699-418">Wenn `M` eine Konstante, ein Feld, eine Eigenschaft, ein Ereignis, ein Enumerationsmember oder eine Typdeklaration ist, werden alle in einer Schnittstellen Deklaration deklarierten Member aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="20699-419">Wenn `M` eine Methode ist, werden alle nicht-Methoden Member, die in einer Schnittstellen Deklaration deklariert sind, aus dem Satz entfernt, und alle Methoden mit derselben Signatur wie `M`, die in einer Schnittstellen Deklaration deklariert sind, werden aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="20699-420">Schließlich wird das Ergebnis der Suche bestimmt, wenn verborgene Member entfernt wurden:</span><span class="sxs-lookup"><span data-stu-id="20699-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="20699-421">Wenn der Satz aus einem einzelnen Member besteht, der keine Methode ist, dann ist dieser Member das Ergebnis der Suche.</span><span class="sxs-lookup"><span data-stu-id="20699-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="20699-422">Andernfalls ist diese Gruppe von Methoden das Ergebnis der Suche, wenn der Satz nur Methoden enthält.</span><span class="sxs-lookup"><span data-stu-id="20699-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="20699-423">Andernfalls ist die Suche mehrdeutig, und es tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="20699-424">Für Member-suchen in anderen Typen als Typparameter und Schnittstellen und Member-suchen in Schnittstellen, die nur eine einzige Vererbung sind (jede Schnittstelle in der Vererbungs Kette weist genau null oder eine direkte Basisschnittstelle auf), hat die Auswirkung der Such Regeln den Wert einfach, dass abgeleitete Member Basiselemente mit dem gleichen Namen oder der gleichen Signatur ausblenden.</span><span class="sxs-lookup"><span data-stu-id="20699-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="20699-425">Solche Suchvorgänge mit einer einzelnen Vererbung sind nie mehrdeutig.</span><span class="sxs-lookup"><span data-stu-id="20699-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="20699-426">Die Mehrdeutigkeiten, die möglicherweise von Member-suchen in Schnittstellen mit mehreren Vererbungen auftreten können, werden unter Zugreifen auf die [Benutzeroberfläche](interfaces.md#interface-member-access)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="20699-427">Basis Typen</span><span class="sxs-lookup"><span data-stu-id="20699-427">Base types</span></span>

<span data-ttu-id="20699-428">Für Zwecke der Element Suche wird ein Typ `T` als die folgenden Basis Typen betrachtet:</span><span class="sxs-lookup"><span data-stu-id="20699-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="20699-429">Wenn `T` `object` ist, hat `T` keinen Basistyp.</span><span class="sxs-lookup"><span data-stu-id="20699-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="20699-430">Wenn `T` ein *enum_type*ist, sind die Basis Typen von `T` die Klassentypen `System.Enum`, `System.ValueType` und `object`.</span><span class="sxs-lookup"><span data-stu-id="20699-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="20699-431">Wenn `T` ein *struct_type*ist, sind die Basis Typen von `T` die Klassentypen `System.ValueType` und `object`.</span><span class="sxs-lookup"><span data-stu-id="20699-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="20699-432">Wenn `T` ein *class_type*ist, sind die Basis Typen von `T` die Basisklassen von `T`, einschließlich des Klassen Typs `object`.</span><span class="sxs-lookup"><span data-stu-id="20699-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="20699-433">Wenn `T` ein *INTERFACE_TYPE*ist, sind die Basis Typen von `T` die Basis Schnittstellen von `T` und der Klassentyp `object`.</span><span class="sxs-lookup"><span data-stu-id="20699-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="20699-434">Wenn `T` ein *array_type*ist, sind die Basis Typen von `T` die Klassentypen `System.Array` und `object`.</span><span class="sxs-lookup"><span data-stu-id="20699-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="20699-435">Wenn `T` ein *delegate_type*ist, sind die Basis Typen von `T` die Klassentypen `System.Delegate` und `object`.</span><span class="sxs-lookup"><span data-stu-id="20699-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="20699-436">Funktionsmember</span><span class="sxs-lookup"><span data-stu-id="20699-436">Function members</span></span>

<span data-ttu-id="20699-437">Funktionsmember sind Elemente, die ausführbare Anweisungen enthalten.</span><span class="sxs-lookup"><span data-stu-id="20699-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="20699-438">Funktionsmember sind immer Member von Typen und können keine Member von Namespaces sein.</span><span class="sxs-lookup"><span data-stu-id="20699-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="20699-439">C#definiert die folgenden Kategorien von Funktionsmembern:</span><span class="sxs-lookup"><span data-stu-id="20699-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="20699-440">Methoden</span><span class="sxs-lookup"><span data-stu-id="20699-440">Methods</span></span>
*  <span data-ttu-id="20699-441">Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="20699-441">Properties</span></span>
*  <span data-ttu-id="20699-442">Ereignisse</span><span class="sxs-lookup"><span data-stu-id="20699-442">Events</span></span>
*  <span data-ttu-id="20699-443">Indexer</span><span class="sxs-lookup"><span data-stu-id="20699-443">Indexers</span></span>
*  <span data-ttu-id="20699-444">Benutzerdefinierte Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-444">User-defined operators</span></span>
*  <span data-ttu-id="20699-445">Instanzkonstruktoren</span><span class="sxs-lookup"><span data-stu-id="20699-445">Instance constructors</span></span>
*  <span data-ttu-id="20699-446">Statische Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="20699-446">Static constructors</span></span>
*  <span data-ttu-id="20699-447">Destruktoren</span><span class="sxs-lookup"><span data-stu-id="20699-447">Destructors</span></span>

<span data-ttu-id="20699-448">Mit Ausnahme der destrukturtoren und statischer Konstruktoren (die nicht explizit aufgerufen werden können) werden die in Funktionsmembern enthaltenen Anweisungen durch Funktionselement Aufrufe ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="20699-449">Die tatsächliche Syntax zum Schreiben eines Funktionsmember-aufzurufenden ist von der jeweiligen Funktionsmember-Kategorie abhängig.</span><span class="sxs-lookup"><span data-stu-id="20699-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="20699-450">Die Argumentliste ([Argumentlisten](expressions.md#argument-lists)) eines Funktionsmember-aufzurufenden enthält tatsächliche Werte oder Variablen Verweise für die Parameter des Funktionsmembers.</span><span class="sxs-lookup"><span data-stu-id="20699-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="20699-451">Aufrufe generischer Methoden können den Typrückschluss verwenden, um den Satz von Typargumenten zu ermitteln, die an die Methode übergeben werden sollen.</span><span class="sxs-lookup"><span data-stu-id="20699-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="20699-452">Dieser Prozess wird unter [Typrückschluss](expressions.md#type-inference)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="20699-453">Aufrufe von Methoden, Indexern, Operatoren und Instanzkonstruktoren verwenden die Überladungs Auflösung, um zu bestimmen, welcher Satz von Funktions Membern aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="20699-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="20699-454">Dieser Prozess wird in der [Überladungs Auflösung](expressions.md#overload-resolution)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="20699-455">Sobald ein bestimmter Funktionsmember zur Bindungs Zeit (möglicherweise durch Überladungs Auflösung) identifiziert wurde, wird der tatsächliche Lauf Zeit Prozess des Aufrufs des Funktionsmembers in der [Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="20699-456">In der folgenden Tabelle wird die Verarbeitung zusammengefasst, die in-Konstrukten mit den sechs Kategorien von Funktionsmembern stattfindet, die explizit aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="20699-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="20699-457">In der Tabelle sind `e`, `x`, `y` und `value` Ausdrücke, die als Variablen oder Werte klassifiziert sind, `T` ein Ausdruck, der als Typ klassifiziert ist, `F` der einfache Name einer Methode und `P` der einfache Name einer Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="20699-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="20699-458">__Erstellen__</span><span class="sxs-lookup"><span data-stu-id="20699-458">__Construct__</span></span>     | <span data-ttu-id="20699-459">__Beispiel__</span><span class="sxs-lookup"><span data-stu-id="20699-459">__Example__</span></span>    | <span data-ttu-id="20699-460">__Beschreibung__</span><span class="sxs-lookup"><span data-stu-id="20699-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="20699-461">Methodenaufruf</span><span class="sxs-lookup"><span data-stu-id="20699-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="20699-462">Die Überladungs Auflösung wird angewendet, um die beste Methode `F` in der enthaltenden Klasse oder Struktur auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="20699-463">Die-Methode wird mit der Argumentliste `(x,y)` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="20699-464">Wenn die Methode nicht `static` ist, ist der Instanzausdruck `this`.</span><span class="sxs-lookup"><span data-stu-id="20699-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="20699-465">Die Überladungs Auflösung wird angewendet, um die beste Methode `F` in der Klasse oder Struktur `T` auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="20699-466">Ein Fehler bei der Bindungs Zeit tritt auf, wenn die Methode nicht `static` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="20699-467">Die-Methode wird mit der Argumentliste `(x,y)` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="20699-468">Die Überladungs Auflösung wird angewendet, um die beste Methode F in der Klasse, Struktur oder Schnittstelle auszuwählen, die durch den Typ von `e` angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="20699-469">Ein Fehler bei der Bindungs Zeit tritt auf, wenn die Methode `static` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="20699-470">Die-Methode wird mit dem Instanzausdruck `e` und der Argumentliste `(x,y)` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="20699-471">Eigenschaftenzugriff</span><span class="sxs-lookup"><span data-stu-id="20699-471">Property access</span></span>   | `P`            | <span data-ttu-id="20699-472">Der `get`-Accessor der Eigenschaft `P` in der enthaltenden Klasse oder Struktur wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="20699-473">Ein Kompilierzeitfehler tritt auf, wenn `P` schreibgeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="20699-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="20699-474">Wenn `P` nicht `static` ist, ist der Instanzausdruck `this`.</span><span class="sxs-lookup"><span data-stu-id="20699-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="20699-475">Der `set`-Accessor der Eigenschaft `P` in der enthaltenden Klasse oder Struktur wird mit der Argumentliste `(value)` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="20699-476">Ein Kompilierzeitfehler tritt auf, wenn `P` schreibgeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="20699-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="20699-477">Wenn `P` nicht `static` ist, ist der Instanzausdruck `this`.</span><span class="sxs-lookup"><span data-stu-id="20699-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="20699-478">Der `get`-Accessor der Eigenschaft `P` in der Klasse oder Struktur, `T` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="20699-479">Ein Kompilierzeitfehler tritt auf, wenn `P` nicht `static` ist oder wenn `P` schreibgeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="20699-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="20699-480">Der `set`-Accessor der Eigenschaft `P` in der Klasse oder Struktur `T` wird mit der Argumentliste `(value)` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="20699-481">Ein Kompilierzeitfehler tritt auf, wenn `P` nicht `static` ist oder wenn `P` schreibgeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="20699-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="20699-482">Der `get`-Accessor der Eigenschaft `P` in der Klasse, Struktur oder Schnittstelle, die vom Typ von `e` angegeben wird, wird mit dem Instanzausdruck `e` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="20699-483">Ein Fehler bei der Bindungs Zeit tritt auf, wenn `P` `static` ist oder wenn `P` schreibgeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="20699-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="20699-484">Der `set`-Accessor der Eigenschaft `P` in der Klasse, Struktur oder Schnittstelle, die vom Typ von `e` angegeben wird, wird mit dem Instanzausdruck `e` und der Argumentliste `(value)` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="20699-485">Ein Fehler bei der Bindungs Zeit tritt auf, wenn `P` `static` ist oder wenn `P` schreibgeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="20699-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="20699-486">Ereignis Zugriff</span><span class="sxs-lookup"><span data-stu-id="20699-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="20699-487">Der `add`-Accessor des Ereignisses, `E` in der enthaltenden Klasse oder Struktur aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="20699-488">Wenn `E` nicht statisch ist, ist der Instanzausdruck `this`.</span><span class="sxs-lookup"><span data-stu-id="20699-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="20699-489">Der `remove`-Accessor des Ereignisses, `E` in der enthaltenden Klasse oder Struktur aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="20699-490">Wenn `E` nicht statisch ist, ist der Instanzausdruck `this`.</span><span class="sxs-lookup"><span data-stu-id="20699-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="20699-491">Der `add`-Accessor des Ereignisses `E` in der Klasse oder Struktur, `T` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="20699-492">Ein Fehler bei der Bindungs Zeit tritt auf, wenn `E` nicht statisch ist.</span><span class="sxs-lookup"><span data-stu-id="20699-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="20699-493">Der `remove`-Accessor des Ereignisses `E` in der Klasse oder Struktur, `T` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="20699-494">Ein Fehler bei der Bindungs Zeit tritt auf, wenn `E` nicht statisch ist.</span><span class="sxs-lookup"><span data-stu-id="20699-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="20699-495">Der `add`-Accessor des Ereignisses `E` in der Klasse, Struktur oder Schnittstelle, die vom Typ von `e` angegeben wird, wird mit dem Instanzausdruck `e` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="20699-496">Ein Fehler bei der Bindungs Zeit tritt auf, wenn `E` statisch ist.</span><span class="sxs-lookup"><span data-stu-id="20699-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="20699-497">Der `remove`-Accessor des Ereignisses `E` in der Klasse, Struktur oder Schnittstelle, die vom Typ von `e` angegeben wird, wird mit dem Instanzausdruck `e` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="20699-498">Ein Fehler bei der Bindungs Zeit tritt auf, wenn `E` statisch ist.</span><span class="sxs-lookup"><span data-stu-id="20699-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="20699-499">Indexerzugriff</span><span class="sxs-lookup"><span data-stu-id="20699-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="20699-500">Die Überladungs Auflösung wird angewendet, um den besten Indexer in der Klasse, Struktur oder Schnittstelle auszuwählen, die durch den Typ von e angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="20699-501">Der `get`-Accessor des Indexers wird mit dem Instanzausdruck `e` und der Argumentliste `(x,y)` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="20699-502">Ein Bindungs Zeit Fehler tritt auf, wenn der Indexer schreibgeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="20699-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="20699-503">Die Überladungs Auflösung wird angewendet, um den besten Indexer in der Klasse, Struktur oder Schnittstelle auszuwählen, die durch den Typ von `e` angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="20699-504">Der `set`-Accessor des Indexers wird mit dem Instanzausdruck `e` und der Argumentliste `(x,y,value)` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="20699-505">Ein Bindungs Zeit Fehler tritt auf, wenn der Indexer schreibgeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="20699-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="20699-506">Operator Aufruf</span><span class="sxs-lookup"><span data-stu-id="20699-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="20699-507">Die Überladungs Auflösung wird angewendet, um den besten unären Operator in der Klasse oder Struktur auszuwählen, die durch den Typ von `x` angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="20699-508">Der ausgewählte Operator wird mit der Argumentliste `(x)` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="20699-509">Die Überladungs Auflösung wird angewendet, um den besten binären Operator in den Klassen oder Strukturen auszuwählen, die von den Typen von `x` und `y` angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="20699-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="20699-510">Der ausgewählte Operator wird mit der Argumentliste `(x,y)` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="20699-511">Instanzkonstruktoraufruf</span><span class="sxs-lookup"><span data-stu-id="20699-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="20699-512">Die Überladungs Auflösung wird angewendet, um den besten Instanzkonstruktor in der Klasse oder Struktur auszuwählen `T`.</span><span class="sxs-lookup"><span data-stu-id="20699-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="20699-513">Der Instanzkonstruktor wird mit der Argumentliste `(x,y)` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="20699-514">Argument Listen</span><span class="sxs-lookup"><span data-stu-id="20699-514">Argument lists</span></span>

<span data-ttu-id="20699-515">Jeder Funktionsmember und Delegataufruf enthält eine Argumentliste, die tatsächliche Werte oder Variablen Verweise für die Parameter des Funktionsmembers bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="20699-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="20699-516">Die Syntax zum Angeben der Argumentliste eines Funktionsmember-aufzurufenden ist von der Funktionsmember-Kategorie abhängig:</span><span class="sxs-lookup"><span data-stu-id="20699-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="20699-517">Bei Instanzkonstruktoren, Methoden, Indexern und Delegaten werden die Argumente als *argument_list*angegeben, wie unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="20699-518">Bei Indexers, wenn der `set`-Accessor aufgerufen wird, enthält die Argumentliste zusätzlich den Ausdruck, der als rechter Operand des Zuweisungs Operators angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="20699-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="20699-519">Bei Eigenschaften ist die Argumentliste leer, wenn der `get`-Accessor aufgerufen wird, und besteht aus dem Ausdruck, der als rechter Operand des Zuweisungs Operators angegeben ist, wenn der `set`-Accessor aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="20699-520">Bei Ereignissen besteht die Argumentliste aus dem Ausdruck, der als rechter Operand des Operators "`+=`" oder "`-=`" angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="20699-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="20699-521">Bei benutzerdefinierten Operatoren besteht die Argumentliste aus dem einzelnen Operanden des unären Operators oder den beiden Operanden des binären Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="20699-522">Die Argumente von Eigenschaften ([Eigenschaften](classes.md#properties)), Ereignissen ([Ereignissen](classes.md#events)) und benutzerdefinierten Operatoren ([Operatoren](classes.md#operators)) werden immer als Wert Parameter ([value-Parameter](classes.md#value-parameters)) übermittelt.</span><span class="sxs-lookup"><span data-stu-id="20699-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="20699-523">Die Argumente von Indexer ([Indexer](classes.md#indexers)) werden immer als Wert Parameter ([Wert Parameter](classes.md#value-parameters)) oder Parameter Arrays ([Parameter Arrays](classes.md#parameter-arrays)) übergeben.</span><span class="sxs-lookup"><span data-stu-id="20699-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="20699-524">Verweis-und Ausgabeparameter werden für diese Kategorien von Funktionsmembern nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="20699-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="20699-525">Die Argumente eines Instanzkonstruktors, einer Methode, eines Indexers oder eines delegataufrufers werden als *argument_list*angegeben:</span><span class="sxs-lookup"><span data-stu-id="20699-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

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

<span data-ttu-id="20699-526">Ein *argument_list* besteht aus einem oder mehreren *Argumenten*, getrennt durch Kommas.</span><span class="sxs-lookup"><span data-stu-id="20699-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="20699-527">Jedes Argument besteht aus einem optionalen *argument_name* , gefolgt von einem *argument_value*.</span><span class="sxs-lookup"><span data-stu-id="20699-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="20699-528">Ein *Argument* mit einem *argument_name* wird als ***benanntes Argument***bezeichnet, wohingegen ein *Argument* ohne *argument_name* ein ***Positions Argument***ist.</span><span class="sxs-lookup"><span data-stu-id="20699-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="20699-529">Es ist ein Fehler für ein Positions Argument, das nach einem benannten Argument in einem *argument_list*angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="20699-530">*Argument_value* kann eine der folgenden Formen annehmen:</span><span class="sxs-lookup"><span data-stu-id="20699-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="20699-531">Ein *Ausdruck*, der angibt, dass das Argument als Wert Parameter ([Wert Parameter](classes.md#value-parameters)) übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="20699-532">Das Schlüsselwort `ref` gefolgt von einem *variable_reference* ([Variablen Verweise](variables.md#variable-references)), das angibt, dass das Argument als Verweis Parameter übergeben wird ([Verweis Parameter](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="20699-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="20699-533">Eine Variable muss definitiv zugewiesen werden ([definitive Zuweisung](variables.md#definite-assignment)), bevor Sie als Verweis Parameter übergeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="20699-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="20699-534">Das Schlüsselwort `out` gefolgt von einem *variable_reference* ([Variablen Verweise](variables.md#variable-references)), das angibt, dass das Argument als Ausgabeparameter ([Ausgabeparameter](classes.md#output-parameters)) übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="20699-535">Eine Variable wird als definitiv zugewiesen ([definitive Zuweisung](variables.md#definite-assignment)) nach einem Funktionselement Aufruf, bei dem die Variable als Output-Parameter übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="20699-536">Entsprechende Parameter</span><span class="sxs-lookup"><span data-stu-id="20699-536">Corresponding parameters</span></span>

<span data-ttu-id="20699-537">Für jedes Argument in einer Argumentliste muss ein entsprechender Parameter im Funktions Member oder Delegaten vorhanden sein, der aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="20699-538">Die im folgenden verwendete Parameterliste wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="20699-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="20699-539">Bei virtuellen Methoden und indexatoren, die in Klassen definiert sind, wird die Parameterliste aus der spezifischsten Deklaration oder Überschreibung des Funktionsmembers ausgewählt, beginnend mit dem statischen Typ des Empfängers und Durchsuchen der zugehörigen Basisklassen.</span><span class="sxs-lookup"><span data-stu-id="20699-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="20699-540">Für Schnittstellen Methoden und Indexer wird die Parameterliste aus der spezifischsten Definition des Members ausgewählt, beginnend mit dem Schnittstellentyp und Durchsuchen der Basis Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="20699-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="20699-541">Wenn keine eindeutige Parameterliste gefunden wird, wird eine Parameterliste mit nicht zugänglichen Namen und optionalen Parametern erstellt, sodass Aufrufe keine benannten Parameter verwenden können oder keine optionalen Argumente weglassen.</span><span class="sxs-lookup"><span data-stu-id="20699-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="20699-542">Bei partiellen Methoden wird die Parameterliste der definierenden partiellen Methoden Deklaration verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="20699-543">Für alle anderen Funktionsmember und Delegaten gibt es nur eine einzige Parameterliste, die verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="20699-544">Die Position eines Arguments oder Parameters wird als die Anzahl der Argumente oder Parameter definiert, die in der Argumentliste oder Parameterliste vorangestellt sind.</span><span class="sxs-lookup"><span data-stu-id="20699-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="20699-545">Die entsprechenden Parameter für Funktionsmember-Argumente werden wie folgt festgelegt:</span><span class="sxs-lookup"><span data-stu-id="20699-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="20699-546">Argumente im *argument_list* von Instanzkonstruktoren, Methoden, Indexern und Delegaten:</span><span class="sxs-lookup"><span data-stu-id="20699-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="20699-547">Ein Positions Argument, bei dem ein fester Parameter an derselben Position in der Parameterliste auftritt, entspricht diesem Parameter.</span><span class="sxs-lookup"><span data-stu-id="20699-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="20699-548">Ein Positions Argument eines Funktionsmembers mit einem Parameter Array, das in der normalen Form aufgerufen wird, entspricht dem Parameter Array, das an derselben Position in der Parameterliste vorkommen muss.</span><span class="sxs-lookup"><span data-stu-id="20699-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="20699-549">Ein Positions Argument eines Funktionsmembers mit einem Parameter Array, das in der erweiterten Form aufgerufen wird, wobei kein fester Parameter an derselben Position in der Parameterliste auftritt, entspricht einem Element im Parameter Array.</span><span class="sxs-lookup"><span data-stu-id="20699-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="20699-550">Ein benanntes Argument entspricht dem-Parameter mit dem gleichen Namen in der Parameterliste.</span><span class="sxs-lookup"><span data-stu-id="20699-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="20699-551">Bei Indexers entspricht der als rechter Operand des Zuweisungs Operators angegebene Ausdruck dem impliziten `value`-Parameter der `set`-Accessor-Deklaration, wenn der `set`-Accessor aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="20699-552">Bei-Eigenschaften gibt es keine Argumente, wenn der `get`-Accessor aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="20699-553">Wenn der `set`-Accessor aufgerufen wird, entspricht der als rechter Operand des Zuweisungs Operators angegebene Ausdruck dem impliziten `value`-Parameter der `set`-Accessor-Deklaration.</span><span class="sxs-lookup"><span data-stu-id="20699-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="20699-554">Bei benutzerdefinierten unären Operatoren (einschließlich Konvertierungen) entspricht der einzelne Operand dem einzelnen Parameter der Operator Deklaration.</span><span class="sxs-lookup"><span data-stu-id="20699-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="20699-555">Für benutzerdefinierte binäre Operatoren entspricht der linke Operand dem ersten Parameter, und der rechte Operand entspricht dem zweiten Parameter der Operator Deklaration.</span><span class="sxs-lookup"><span data-stu-id="20699-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="20699-556">Lauf Zeit Auswertung von Argumentlisten</span><span class="sxs-lookup"><span data-stu-id="20699-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="20699-557">Während der Lauf Zeit Verarbeitung eines Funktionsmember-aufzurufenden ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) werden die Ausdrücke oder Variablen Verweise einer Argumentliste in der Reihenfolge von links nach rechts ausgewertet, wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="20699-558">Bei einem value-Parameter wird der Argument Ausdruck ausgewertet und eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den entsprechenden Parametertyp durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="20699-559">Der resultierende Wert wird zum Anfangswert des value-Parameters im Funktionselement Aufruf.</span><span class="sxs-lookup"><span data-stu-id="20699-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="20699-560">Bei einem Verweis-oder Ausgabeparameter wird der Variablen Verweis ausgewertet, und der resultierende Speicherort wird zu dem Speicherort, der durch den-Parameter im Aufruf der Funktionselemente dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="20699-561">Wenn der als Verweis oder Ausgabeparameter angegebene Variablen Verweis ein Array Element eines *reference_type*ist, wird eine Lauf Zeit Überprüfung durchgeführt, um sicherzustellen, dass der Elementtyp des Arrays mit dem Parametertyp identisch ist.</span><span class="sxs-lookup"><span data-stu-id="20699-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="20699-562">Wenn diese Überprüfung fehlschlägt, wird eine `System.ArrayTypeMismatchException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="20699-563">Methoden, Indexer und Instanzkonstruktoren können Ihren äußersten ganz rechts Parameter als Parameter Array deklarieren ([Parameter Arrays](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="20699-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="20699-564">Solche Funktionsmember werden entweder in ihrer normalen Form oder in der erweiterten Form aufgerufen, je nachdem, welche anwendbar ist ([anwendbares Funktionsmember](expressions.md#applicable-function-member)):</span><span class="sxs-lookup"><span data-stu-id="20699-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="20699-565">Wenn ein Funktionsmember mit einem Parameter Array in der normalen Form aufgerufen wird, muss das für das Parameter Array angegebene Argument ein einzelner Ausdruck sein, der implizit konvertierbar ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den Typ des Parameter Arrays ist.</span><span class="sxs-lookup"><span data-stu-id="20699-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="20699-566">In diesem Fall verhält sich das Parameter Array genau wie ein value-Parameter.</span><span class="sxs-lookup"><span data-stu-id="20699-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="20699-567">Wenn ein Funktionsmember mit einem Parameter Array in der erweiterten Form aufgerufen wird, muss der Aufruf NULL oder mehr Positions Argumente für das Parameter Array angeben, wobei jedes Argument ein Ausdruck ist, der implizit konvertierbar ist ([implizite Konvertierungen ](conversions.md#implicit-conversions)) zum Elementtyp des Parameter Arrays.</span><span class="sxs-lookup"><span data-stu-id="20699-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="20699-568">In diesem Fall erstellt der Aufruf eine Instanz des Parameter Array Typs mit einer Länge, die der Anzahl der Argumente entspricht, initialisiert die Elemente der Array Instanz mit den angegebenen Argument Werten und verwendet die neu erstellte Array Instanz als die tatsächliche gestritten.</span><span class="sxs-lookup"><span data-stu-id="20699-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="20699-569">Die Ausdrücke einer Argumentliste werden immer in der Reihenfolge ausgewertet, in der Sie geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="20699-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="20699-570">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-570">Thus, the example</span></span>
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
<span data-ttu-id="20699-571">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="20699-571">produces the output</span></span>
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="20699-572">Die Array-Co-Varianz Regeln ([Array-Kovarianz](arrays.md#array-covariance)) erlauben, dass ein Wert eines Arraytyps `A[]` ein Verweis auf eine Instanz eines Arraytyps `B[]` ist, vorausgesetzt, dass eine implizite Verweis Konvertierung von `B` in `A` vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="20699-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="20699-573">Aufgrund dieser Regeln ist eine Lauf Zeit Überprüfung erforderlich, wenn ein Array Element eines *reference_type* als Verweis-oder Ausgabeparameter übergeben wird, um sicherzustellen, dass der tatsächliche Elementtyp des Arrays mit dem des Parameters identisch ist.</span><span class="sxs-lookup"><span data-stu-id="20699-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="20699-574">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-574">In the example</span></span>
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
<span data-ttu-id="20699-575">der zweite Aufruf von `F` bewirkt, dass eine `System.ArrayTypeMismatchException` ausgelöst wird, da der tatsächliche Elementtyp von `b` `string` und nicht `object` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="20699-576">Wenn ein Funktionsmember mit einem Parameter Array in der erweiterten Form aufgerufen wird, wird der Aufruf genau so verarbeitet, als ob ein Array Erstellungs Ausdruck mit einem Arrayinitialisierer ([Array Erstellungs Ausdrücke](expressions.md#array-creation-expressions)) um die erweiterten Parameter eingefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="20699-577">Beispielsweise mit der Deklaration</span><span class="sxs-lookup"><span data-stu-id="20699-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="20699-578">die folgenden Aufrufe der erweiterten Form der Methode</span><span class="sxs-lookup"><span data-stu-id="20699-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="20699-579">genau entsprechen</span><span class="sxs-lookup"><span data-stu-id="20699-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="20699-580">Beachten Sie insbesondere, dass ein leeres Array erstellt wird, wenn keine Argumente für das Parameter Array angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="20699-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="20699-581">Wenn Argumente von einem Funktionsmember mit entsprechenden optionalen Parametern ausgelassen werden, werden die Standardargumente der Deklaration des Funktions Members implizit übermittelt.</span><span class="sxs-lookup"><span data-stu-id="20699-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="20699-582">Da diese immer konstant sind, wirkt sich Ihre Auswertung nicht auf die Auswertungs Reihenfolge der restlichen Argumente aus.</span><span class="sxs-lookup"><span data-stu-id="20699-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="20699-583">Typrückschluss</span><span class="sxs-lookup"><span data-stu-id="20699-583">Type inference</span></span>

<span data-ttu-id="20699-584">Wenn eine generische Methode ohne Angabe von Typargumenten aufgerufen wird, versucht ein ***Typrückschluss*** -Prozess, Typargumente für den Aufruf abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="20699-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="20699-585">Das vorhanden sein des Typrückschlusses ermöglicht die Verwendung einer bequemeren Syntax zum Aufrufen einer generischen Methode und ermöglicht dem Programmierer, das Angeben von redundanten Typinformationen zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="20699-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="20699-586">Beispielsweise mit der Methoden Deklaration:</span><span class="sxs-lookup"><span data-stu-id="20699-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="20699-587">Es ist möglich, die `Choose`-Methode aufzurufen, ohne explizit ein Typargument anzugeben:</span><span class="sxs-lookup"><span data-stu-id="20699-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="20699-588">Durch den Typrückschluss werden die Typargumente `int` und `string` aus den Argumenten der Methode ermittelt.</span><span class="sxs-lookup"><span data-stu-id="20699-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="20699-589">Der Typrückschluss tritt als Teil der Bindungs Zeit Verarbeitung eines Methoden Aufrufs ([Methodenaufrufe](expressions.md#method-invocations)) auf und findet vor dem Schritt zur Überladungs Auflösung des Aufrufs statt.</span><span class="sxs-lookup"><span data-stu-id="20699-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="20699-590">Wenn eine bestimmte Methoden Gruppe in einem Methodenaufruf angegeben wird und im Rahmen des Methoden aufruschlusses keine Typargumente angegeben werden, wird der Typrückschluss auf jede generische Methode in der Methoden Gruppe angewendet.</span><span class="sxs-lookup"><span data-stu-id="20699-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="20699-591">Wenn der Typrückschluss erfolgreich ist, werden die Typargumente abgeleitet, um die Argument Typen für die nachfolgende Überladungs Auflösung zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="20699-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="20699-592">Wenn die Überladungs Auflösung eine generische Methode als die aufzurufende Methode auswählt, werden die abgerufenen Typargumente als tatsächliche Typargumente für den Aufruf verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="20699-593">Wenn der Typrückschluss für eine bestimmte Methode fehlschlägt, wird diese Methode nicht an der Überladungs Auflösung beteiligt.</span><span class="sxs-lookup"><span data-stu-id="20699-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="20699-594">Der Fehler beim Typrückschluss in und von sich führt nicht zu einem Bindungs Fehler.</span><span class="sxs-lookup"><span data-stu-id="20699-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="20699-595">Allerdings führt dies häufig zu einem Bindungs Fehler, wenn die Überladungs Auflösung dann keine anwendbaren Methoden findet.</span><span class="sxs-lookup"><span data-stu-id="20699-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="20699-596">Wenn sich die angegebene Anzahl von Argumenten von der Anzahl der Parameter in der Methode unterscheidet, schlägt die Ableitung sofort fehl.</span><span class="sxs-lookup"><span data-stu-id="20699-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="20699-597">Angenommen, die generische Methode hat die folgende Signatur:</span><span class="sxs-lookup"><span data-stu-id="20699-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="20699-598">Bei einem Methoden aufzurufenden Formular `M(E1...Em)` ist die Aufgabe des Typrückschlusses das Suchen nach eindeutigen Typargumenten `S1...Sn` für jeden Typparameter `X1...Xn`, sodass der-Befehl `M<S1...Sn>(E1...Em)` gültig wird.</span><span class="sxs-lookup"><span data-stu-id="20699-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="20699-599">Während des *Abschlusses* wird jeder Typparameter `Xi` entweder mit einem bestimmten Typ *korrigiert* `Si` oder mit einem zugeordneten Satz von *Begrenzungen*.</span><span class="sxs-lookup"><span data-stu-id="20699-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="20699-600">Jede der Begrenzungen ist ein Typ `T`.</span><span class="sxs-lookup"><span data-stu-id="20699-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="20699-601">Anfänglich wird jede Typvariable `Xi` mit einem leeren Satz von Begrenzungen nicht korrigiert.</span><span class="sxs-lookup"><span data-stu-id="20699-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="20699-602">Der Typrückschluss findet in Phasen statt.</span><span class="sxs-lookup"><span data-stu-id="20699-602">Type inference takes place in phases.</span></span> <span data-ttu-id="20699-603">In jeder Phase wird versucht, Typargumente für weitere Typvariablen basierend auf den Ergebnissen der vorherigen Phase abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="20699-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="20699-604">In der ersten Phase werden einige anfängliche Rückschlüsse von Begrenzungen erstellt, während in der zweiten Phase Typvariablen für bestimmte Typen korrigiert und weitere Begrenzungen abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="20699-605">Die zweite Phase muss möglicherweise mehrmals wiederholt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="20699-606">*Hinweis*: Der Typrückschluss findet nicht nur statt, wenn eine generische Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="20699-607">Der Typrückschluss für die Konvertierung von Methoden Gruppen wird unter [Typrückschluss für die Konvertierung von Methoden Gruppen](expressions.md#type-inference-for-conversion-of-method-groups) beschrieben und der beste allgemeine Typ eines Satzes von Ausdrücken finden Sie unter [Ermitteln des besten allgemeinen Typs eines Satzes von Ausdrücken](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span><span class="sxs-lookup"><span data-stu-id="20699-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="20699-608">Die erste Phase</span><span class="sxs-lookup"><span data-stu-id="20699-608">The first phase</span></span>

<span data-ttu-id="20699-609">Für jedes der Methodenargumente `Ei`:</span><span class="sxs-lookup"><span data-stu-id="20699-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="20699-610">Wenn `Ei` eine anonyme Funktion ist, wird ein *expliziter parametertyprückschluss* ([explizite parametertyprückschluss](expressions.md#explicit-parameter-type-inferences)) von `Ei` bis `Ti` vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="20699-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="20699-611">Andernfalls, wenn `Ei` einen Typ aufweist `U` und `xi` ein Wert Parameter ist, wird ein *niedrigerer gebundener* Rückschluss *von* `U` *bis* `Ti` vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="20699-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="20699-612">Andernfalls, wenn `Ei` einen Typ aufweist `U` und `xi` ein `ref`-oder `out`-Parameter ist, wird ein *genauer Rückschluss* *von* `U` *bis* `Ti` vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="20699-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="20699-613">Andernfalls wird kein Rückschluss für dieses Argument gemacht.</span><span class="sxs-lookup"><span data-stu-id="20699-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="20699-614">Die zweite Phase</span><span class="sxs-lookup"><span data-stu-id="20699-614">The second phase</span></span>

<span data-ttu-id="20699-615">Die zweite Phase verläuft wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="20699-616">Alle nicht *fixierten* Typvariablen `Xi`, die nicht *abhängig* sind ([Abhängigkeit](expressions.md#dependence)), `Xj` sind fixiert ([Korrektur](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="20699-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="20699-617">Wenn keine derartigen Typvariablen vorhanden sind, werden alle *nicht fixierten* Typvariablen `Xi` *korrigiert* , für die Folgendes gilt:</span><span class="sxs-lookup"><span data-stu-id="20699-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="20699-618">Es ist mindestens eine Typvariable `Xj` vorhanden, die von `Xi` abhängt.</span><span class="sxs-lookup"><span data-stu-id="20699-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="20699-619">`Xi` weist einen nicht leeren Satz von Begrenzungen auf.</span><span class="sxs-lookup"><span data-stu-id="20699-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="20699-620">Wenn keine Typvariablen vorhanden sind und noch immer *unfixe* Typvariablen vorhanden sind, schlägt der Typrückschluss fehl.</span><span class="sxs-lookup"><span data-stu-id="20699-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="20699-621">Andernfalls ist der Typrückschluss erfolgreich, wenn keine weiteren *unfixed* -Typvariablen vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="20699-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="20699-622">Andernfalls gilt für alle Argumente `Ei` mit dem entsprechenden Parametertyp `Ti`, wobei die *Ausgabetypen* ([Ausgabetypen](expressions.md#output-types)) *unfixed* Typvariablen enthalten `Xj`, aber die *Eingabetypen* ([Eingabetypen](expressions.md#input-types)) nicht. der *Ausgabetyp Rückschluss* ([Ausschlüsse auf Ausgabetyp](expressions.md#output-type-inferences)) erfolgt *von* `Ei` *bis* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="20699-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="20699-623">Die zweite Phase wird wiederholt.</span><span class="sxs-lookup"><span data-stu-id="20699-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="20699-624">Eingabetypen</span><span class="sxs-lookup"><span data-stu-id="20699-624">Input types</span></span>

<span data-ttu-id="20699-625">Wenn `E` eine Methoden Gruppe oder implizit typisierte anonyme Funktion ist und `T` ein Delegattyp oder ein Ausdrucks Strukturtyp ist, sind alle Parametertypen von `T` *Eingabetypen* von `E` *mit dem Typ* `T`.</span><span class="sxs-lookup"><span data-stu-id="20699-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="20699-626">Ausgabetypen</span><span class="sxs-lookup"><span data-stu-id="20699-626">Output types</span></span>

<span data-ttu-id="20699-627">Wenn `E` eine Methoden Gruppe oder eine anonyme Funktion ist und `T` ein Delegattyp oder ein Ausdrucks bauentyp ist, ist der Rückgabetyp von `T` ein *Ausgabetyp von* `E` *mit dem Typ* `T`.</span><span class="sxs-lookup"><span data-stu-id="20699-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="20699-628">Hän</span><span class="sxs-lookup"><span data-stu-id="20699-628">Dependence</span></span>

<span data-ttu-id="20699-629">Eine *unfixed* -Typvariable `Xi` ist *direkt* von einer Variablen mit dem Typ "unfixed" `Xj` abhängig, wenn für ein Argument `Ek` mit dem Typ `Tk` `Xj` in einem *Eingabetyp* von `Ek` mit dem Typ `Tk` und 0 in einem  *der Ausgabetyp* von 2 mit dem Typ 3.</span><span class="sxs-lookup"><span data-stu-id="20699-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="20699-630">`Xj` ist von `Xi` *abhängig* , wenn `Xj` *direkt* von `Xi` abhängt oder wenn `Xi` *direkt auf* `Xk` und @no__t- *9 von* 1 abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="20699-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="20699-631">Daher ist "hängt von" der transitiv, aber nicht reflexive Abschluss von "ist direkt abhängig".</span><span class="sxs-lookup"><span data-stu-id="20699-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="20699-632">Ausschlüsse auf Ausgabetyp</span><span class="sxs-lookup"><span data-stu-id="20699-632">Output type inferences</span></span>

<span data-ttu-id="20699-633">Ein *ausgabetyprückschluss* wird *von* einem Ausdruck `E` *zu* einem Typ `T` auf folgende Weise vorgenommen:</span><span class="sxs-lookup"><span data-stu-id="20699-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="20699-634">Wenn `E` eine anonyme Funktion mit einem abzurufenden Rückgabetyp ist `U` ([Rückgabetyp abgeleitet](expressions.md#inferred-return-type)) und `T` ein Delegattyp oder ein Ausdrucks Strukturtyp mit dem Rückgabetyp `Tb` ist, dann ein *niedrigerer gebundener Rückschluss* ([unten gebundene Rückschlüsse](expressions.md#lower-bound-inferences)). wird *von* `U` *bis* 0 hergestellt.</span><span class="sxs-lookup"><span data-stu-id="20699-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="20699-635">Andernfalls gilt: Wenn `E` eine Methoden Gruppe und `T` ein Delegattyp oder ein Ausdrucks Strukturtyp mit Parametertypen `T1...Tk` und der Rückgabetyp `Tb` ist und die Überladungs Auflösung von `E` mit den Typen `T1...Tk` ergibt, wird eine einzelne Methode mit dem Rückgabetyp `U` erzeugt. , wird *von* `U` *bis* 1 ein Rück *Schluss* ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="20699-636">Andernfalls, wenn `E` ein Ausdruck mit dem Typ `U` ist, wird ein Rück *Schluss* *von* `U` *bis* `T` vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="20699-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="20699-637">Andernfalls werden keine Rückschlüsse gemacht.</span><span class="sxs-lookup"><span data-stu-id="20699-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="20699-638">Explizite Parametertyp Rückschlüsse</span><span class="sxs-lookup"><span data-stu-id="20699-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="20699-639">Ein *expliziter Parameter-Typrückschluss* wird *von* einem Ausdruck `E` *zu* einem Typ `T` auf folgende Weise vorgenommen:</span><span class="sxs-lookup"><span data-stu-id="20699-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="20699-640">Wenn `E` eine explizit typisierte anonyme Funktion mit Parametertypen ist `U1...Uk` und `T` ein Delegattyp oder ein Ausdrucks Strukturtyp mit Parametertypen `V1...Vk` ist, wird für jeden `Ui` ein *genauer Rückschluss* ([exakte Rückschlüsse](expressions.md#exact-inferences)) vorgenommen *. von* `Ui` *bis* zum entsprechenden 0.</span><span class="sxs-lookup"><span data-stu-id="20699-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="20699-641">Exakte Rückschlüsse</span><span class="sxs-lookup"><span data-stu-id="20699-641">Exact inferences</span></span>

<span data-ttu-id="20699-642">Eine *genaue Ableitung* *von* einem Typ `U` *zu* einem Typ `V` erfolgt wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="20699-643">Wenn `V` eines der *unfixed* -`Xi` ist, wird `U` dem Satz der exakten Begrenzungen für `Xi` hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="20699-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="20699-644">Andernfalls werden die `V1...Vk` und `U1...Uk` festgelegt, indem überprüft wird, ob eines der folgenden Fälle zutrifft:</span><span class="sxs-lookup"><span data-stu-id="20699-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="20699-645">`V` ist ein Arraytyp `V1[...]`, und `U` ist ein Arraytyp `U1[...]` desselben Rangs.</span><span class="sxs-lookup"><span data-stu-id="20699-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="20699-646">`V` ist der Typ `V1?`, und `U` ist der Typ `U1?`</span><span class="sxs-lookup"><span data-stu-id="20699-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="20699-647">`V` ist ein konstruierter Typ `C<V1...Vk>`und `U` ein konstruierter Typ `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="20699-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="20699-648">Wenn einer dieser Fälle zutrifft, wird ein *genauer Rückschluss* *von* jeder `Ui` *bis* zum entsprechenden `Vi` vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="20699-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="20699-649">Andernfalls werden keine Rückschlüsse gemacht.</span><span class="sxs-lookup"><span data-stu-id="20699-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="20699-650">Unten gebundene Rückschlüsse</span><span class="sxs-lookup"><span data-stu-id="20699-650">Lower-bound inferences</span></span>

<span data-ttu-id="20699-651">Der Rück *Schluss* *von* einem Typ `U` *zu* einem Typ `V` erfolgt wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="20699-652">Wenn `V` eines der *unfixed* -`Xi` ist, wird `U` dem Satz der unteren Grenzen für `Xi` hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="20699-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="20699-653">Andernfalls, wenn `V` der Typ ist `V1?`und `U` der Typ `U1?` ist, wird ein niedrigerer gebundener Rückschluss von `U1` bis `V1` vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="20699-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="20699-654">Andernfalls werden die `U1...Uk` und `V1...Vk` festgelegt, indem überprüft wird, ob eines der folgenden Fälle zutrifft:</span><span class="sxs-lookup"><span data-stu-id="20699-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="20699-655">`V` ist ein Arraytyp `V1[...]`, und `U` ist ein Arraytyp `U1[...]` (oder ein Typparameter, dessen effektiver Basistyp `U1[...]` ist) desselben Rang.</span><span class="sxs-lookup"><span data-stu-id="20699-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="20699-656">`V` ist eine der `IEnumerable<V1>`, `ICollection<V1>` oder `IList<V1>` und `U` ist ein eindimensionaler Arraytyp `U1[]` (oder ein Typparameter, dessen effektiver Basistyp `U1[]` ist).</span><span class="sxs-lookup"><span data-stu-id="20699-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="20699-657">`V` ist eine konstruierte Klasse, Struktur, Schnittstelle oder Delegattyp `C<V1...Vk>`, und es gibt einen eindeutigen Typ `C<U1...Uk>`, sodass `U` (oder, wenn `U` ein Typparameter ist, die effektive Basisklasse oder ein beliebiger Member des effektiven Schnittstellen Satzes) mit identisch ist. , erbt von (direkt oder indirekt) oder implementiert (direkt oder indirekt) `C<U1...Uk>`.</span><span class="sxs-lookup"><span data-stu-id="20699-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="20699-658">(Die "Eindeutigkeits Einschränkung" bedeutet, dass bei der Fall Schnittstelle `C<T> {} class U: C<X>, C<Y> {}` kein Rückschluss erfolgt, wenn von `U` zu `C<T>` abgeleitet wird, da `U1` `X` oder `Y` sein könnte.)</span><span class="sxs-lookup"><span data-stu-id="20699-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="20699-659">Wenn einer dieser Fälle zutrifft, erfolgt ein Rückschluss *von* jeder `Ui` *bis* zum entsprechenden `Vi` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="20699-660">Wenn `Ui` nicht bekannt ist, dass es sich um einen Verweistyp handelt, wird ein *genauer Rückschluss* gemacht.</span><span class="sxs-lookup"><span data-stu-id="20699-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="20699-661">Andernfalls, wenn `U` ein Arraytyp ist, wird ein Rück *Schluss mit niedrigerer Bindung* erstellt.</span><span class="sxs-lookup"><span data-stu-id="20699-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="20699-662">Andernfalls, wenn `V` `C<V1...Vk>` ist, hängt der Rückschluss vom i-th Type-Parameter von `C` ab:</span><span class="sxs-lookup"><span data-stu-id="20699-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="20699-663">Wenn Sie kovariant ist, wird ein Rück *Schluss* ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="20699-664">Wenn Sie kontra Variant ist, wird eine *obere gebundene Ableitung* vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="20699-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="20699-665">Wenn Sie invariante ist, wird ein *genauer Rückschluss* gemacht.</span><span class="sxs-lookup"><span data-stu-id="20699-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="20699-666">Andernfalls werden keine Rückschlüsse gemacht.</span><span class="sxs-lookup"><span data-stu-id="20699-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="20699-667">Obere gebundene Rückschlüsse</span><span class="sxs-lookup"><span data-stu-id="20699-667">Upper-bound inferences</span></span>

<span data-ttu-id="20699-668">Eine *obere gebundene Ableitung* *von* einem Typ `U` *zu* einem Typ `V` erfolgt wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="20699-669">Wenn `V` eines der *unfixed* -`Xi` ist, wird `U` dem Satz von oberen Begrenzungen für `Xi` hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="20699-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="20699-670">Andernfalls werden die `V1...Vk` und `U1...Uk` festgelegt, indem überprüft wird, ob eines der folgenden Fälle zutrifft:</span><span class="sxs-lookup"><span data-stu-id="20699-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="20699-671">`U` ist ein Arraytyp `U1[...]`, und `V` ist ein Arraytyp `V1[...]` desselben Rangs.</span><span class="sxs-lookup"><span data-stu-id="20699-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="20699-672">`U` ist eine der `IEnumerable<Ue>`, `ICollection<Ue>` oder `IList<Ue>` und `V` ist ein eindimensionaler Arraytyp `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="20699-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="20699-673">`U` ist der Typ `U1?`, und `V` ist der Typ `V1?`</span><span class="sxs-lookup"><span data-stu-id="20699-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="20699-674">`U` ist eine Klasse, Struktur, Schnittstelle oder Delegattyp `C<U1...Uk>`, und `V` ist eine Klasse, Struktur, Schnittstelle oder Delegattyp, die mit identisch ist, von (direkt oder indirekt) oder (direkt oder indirekt) einen eindeutigen Typ implementiert `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="20699-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="20699-675">(Die "Eindeutigkeits Einschränkung" bedeutet, dass bei `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}` kein Rückschluss erfolgt, wenn von `C<U1>` zu `V<Q>` abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="20699-676">Rückschlüsse werden nicht von `U1` bis `X<Q>` oder `Y<Q>` vorgenommen.)</span><span class="sxs-lookup"><span data-stu-id="20699-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="20699-677">Wenn einer dieser Fälle zutrifft, erfolgt ein Rückschluss *von* jeder `Ui` *bis* zum entsprechenden `Vi` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="20699-678">Wenn `Ui` nicht bekannt ist, dass es sich um einen Verweistyp handelt, wird ein *genauer Rückschluss* gemacht.</span><span class="sxs-lookup"><span data-stu-id="20699-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="20699-679">Andernfalls, wenn `V` ein Arraytyp ist, wird eine *obere gebundene Ableitung* vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="20699-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="20699-680">Andernfalls, wenn `U` `C<U1...Uk>` ist, hängt der Rückschluss vom i-th Type-Parameter von `C` ab:</span><span class="sxs-lookup"><span data-stu-id="20699-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="20699-681">Wenn Sie kovariant ist, wird eine *obere gebundene Ableitung* vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="20699-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="20699-682">Wenn Sie kontra Variant ist, wird ein Rück *Schluss* festgestellt.</span><span class="sxs-lookup"><span data-stu-id="20699-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="20699-683">Wenn Sie invariante ist, wird ein *genauer Rückschluss* gemacht.</span><span class="sxs-lookup"><span data-stu-id="20699-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="20699-684">Andernfalls werden keine Rückschlüsse gemacht.</span><span class="sxs-lookup"><span data-stu-id="20699-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="20699-685">Festnahme</span><span class="sxs-lookup"><span data-stu-id="20699-685">Fixing</span></span>

<span data-ttu-id="20699-686">Eine Variable vom Typ " *unfixed* " `Xi` mit einem Satz von Begrenzungen ist wie folgt *festgelegt* :</span><span class="sxs-lookup"><span data-stu-id="20699-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="20699-687">Der Satz von *Kandidaten Typen* `Uj` beginnt als Satz aller Typen im Satz von Begrenzungen für `Xi`.</span><span class="sxs-lookup"><span data-stu-id="20699-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="20699-688">Anschließend untersuchen wir jede gebundene Grenze für `Xi`: Für jede exakte gebundene `U` von `Xi` alle Typen `Uj`, die nicht mit `U` identisch sind, aus dem Kandidaten Satz entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="20699-689">Für jede untere Grenze `U` von `Xi` alle Typen `Uj`, für die *keine* implizite Konvertierung von `U` vorhanden ist, aus dem Kandidaten Satz entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="20699-690">Für jede obere Grenze `U` von `Xi` alle Typen `Uj`, von denen *keine* implizite Konvertierung in `U` erfolgt, werden aus dem Kandidaten Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="20699-691">Wenn zwischen den verbleibenden Kandidaten Typen `Uj` ein eindeutiger Typ `V` vorhanden ist, von dem eine implizite Konvertierung in alle anderen Kandidaten Typen erfolgt, wird `Xi` auf `V` festgesetzt.</span><span class="sxs-lookup"><span data-stu-id="20699-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="20699-692">Andernfalls schlägt der Typrückschluss fehl.</span><span class="sxs-lookup"><span data-stu-id="20699-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="20699-693">Rückgabetyp abgeleitet</span><span class="sxs-lookup"><span data-stu-id="20699-693">Inferred return type</span></span>

<span data-ttu-id="20699-694">Der abzurufende Rückgabetyp einer anonymen Funktion `F` wird bei der Typrückschluss-und Überladungs Auflösung verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="20699-695">Der zurückgegebene Rückgabetyp kann nur für eine anonyme Funktion bestimmt werden, in der alle Parametertypen bekannt sind, da Sie entweder explizit angegeben werden, durch eine anonyme Funktions Konvertierung bereitgestellt oder während des Typrückschlusses auf einem einschließenden generischen abgeleitet werden. Methodenaufruf.</span><span class="sxs-lookup"><span data-stu-id="20699-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="20699-696">Der abhergelegte ***Ergebnistyp*** wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="20699-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="20699-697">Wenn der Text von `F` ein *Ausdruck* ist, der über einen-Typ verfügt, ist der abherausgestellte Ergebnistyp von `F` der Typ dieses Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="20699-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="20699-698">Wenn der Text von `F` ein- *Block* ist und der Satz von Ausdrücken in den `return`-Anweisungen des Blocks einen am besten verbreiteten Typ `T` aufweist (suchen[nach dem besten allgemeinen Typ eines Satzes von Ausdrücken](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), ist der Rückschluss Ergebnistyp `F` `T`.</span><span class="sxs-lookup"><span data-stu-id="20699-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="20699-699">Andernfalls kann ein Ergebnistyp nicht für `F` abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="20699-700">Der ***zurückgegebene Rückgabetyp*** wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="20699-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="20699-701">Wenn `F` Async ist und der Text von `F` entweder ein Ausdruck ist, der als Nothing ([Ausdrucks Klassifizierungen](expressions.md#expression-classifications)) klassifiziert ist, oder ein Anweisungsblock, bei dem keine Return-Anweisungen über Ausdrücke verfügen, ist der abherleitet Rückgabetyp `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="20699-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="20699-702">Wenn `F` Async ist und einen abhergelegten Ergebnistyp `T` aufweist, ist der zurückgegebene Rückgabetyp `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="20699-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="20699-703">Wenn `F` nicht Async ist und einen abhergelegten Ergebnistyp `T` aufweist, ist der zurückgegebene Rückgabetyp `T`.</span><span class="sxs-lookup"><span data-stu-id="20699-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="20699-704">Andernfalls kann ein Rückgabetyp nicht für `F` abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="20699-705">Sehen Sie sich als Beispiel für den Typrückschluss mit anonymen Funktionen die in der `System.Linq.Enumerable`-Klasse deklarierte `Select`-Erweiterungsmethode an:</span><span class="sxs-lookup"><span data-stu-id="20699-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
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

<span data-ttu-id="20699-706">Angenommen, der `System.Linq`-Namespace wurde mit einer `using`-Klausel importiert, und eine Klasse `Customer` mit einer `Name`-Eigenschaft vom Typ `string`, kann die `Select`-Methode verwendet werden, um die Namen einer Kundenliste auszuwählen:</span><span class="sxs-lookup"><span data-stu-id="20699-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="20699-707">Der Aufruf der Erweiterungsmethode ([Erweiterungs Methodenaufrufe](expressions.md#extension-method-invocations)) von `Select` wird verarbeitet, indem der Aufruf in einen statischen Methodenaufruf umgeschrieben wird:</span><span class="sxs-lookup"><span data-stu-id="20699-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="20699-708">Da Typargumente nicht explizit angegeben wurden, wird der Typrückschluss verwendet, um die Typargumente abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="20699-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="20699-709">Zuerst steht das Argument "`customers`" im Zusammenhang mit dem Parameter "`source`", wobei `T` als `Customer` abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="20699-710">Anschließend wird mit dem oben beschriebenen Prozess der anonymen Funktionstyp Ableitung `c` den Typ `Customer` und der Ausdruck `c.Name` mit dem Rückgabetyp des `selector`-Parameters verknüpft, wobei `S` als `string` abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="20699-711">Daher entspricht der Aufruf dem</span><span class="sxs-lookup"><span data-stu-id="20699-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="20699-712">und das Ergebnis ist vom Typ `IEnumerable<string>`.</span><span class="sxs-lookup"><span data-stu-id="20699-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="20699-713">Im folgenden Beispiel wird veranschaulicht, wie der anonyme Funktionstyp Rückschluss das Eingeben von Typinformationen zwischen Argumenten in einem generischen Methodenaufruf zulässt.</span><span class="sxs-lookup"><span data-stu-id="20699-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="20699-714">Bei Angabe der-Methode:</span><span class="sxs-lookup"><span data-stu-id="20699-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="20699-715">Typrückschluss für den Aufruf:</span><span class="sxs-lookup"><span data-stu-id="20699-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="20699-716">geht wie folgt vor: Zuerst ist das Argument `"1:15:30"` mit dem Parameter `value` verknüpft, wobei `X` als `string` abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="20699-717">Anschließend erhält der-Parameter der ersten anonymen Funktion, `s`, den abzurufenden Typ `string`, und der Ausdruck `TimeSpan.Parse(s)` ist mit dem Rückgabetyp `f1` verknüpft, wobei `Y` als `System.TimeSpan` abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="20699-718">Zum Schluss erhält der-Parameter der zweiten anonymen Funktion (`t`) den abzurufenden Typ `System.TimeSpan`, und der Ausdruck `t.TotalSeconds` ist mit dem Rückgabetyp `f2` verknüpft, wobei `Z` als `double` abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="20699-719">Daher ist das Ergebnis des auf-aufgabens vom Typ `double`.</span><span class="sxs-lookup"><span data-stu-id="20699-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="20699-720">Typrückschluss für die Konvertierung von Methoden Gruppen</span><span class="sxs-lookup"><span data-stu-id="20699-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="20699-721">Ähnlich wie bei Aufrufen von generischen Methoden muss der Typrückschluss auch angewendet werden, wenn eine Methoden Gruppe `M`, die eine generische Methode enthält, in einen angegebenen Delegattyp konvertiert wird `D` ([Methoden Gruppen Konvertierungen](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="20699-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="20699-722">Bei Angabe einer Methode</span><span class="sxs-lookup"><span data-stu-id="20699-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="20699-723">und die Methoden Gruppe `M` dem Delegattyp zugewiesen wird `D` ist die Aufgabe des Typrückschlusses das Suchen der Typargumente `S1...Sn`, sodass der Ausdruck:</span><span class="sxs-lookup"><span data-stu-id="20699-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="20699-724">wird mit `D` kompatibel ([Delegatdeklarationen](delegates.md#delegate-declarations)).</span><span class="sxs-lookup"><span data-stu-id="20699-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="20699-725">Im Gegensatz zum Typrückschluss-Algorithmus für generische Methodenaufrufe gibt es in diesem Fall nur Argument *Typen*, keine Argument *Ausdrücke*.</span><span class="sxs-lookup"><span data-stu-id="20699-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="20699-726">Insbesondere gibt es keine anonymen Funktionen und daher keinen Bedarf an mehreren Phasen des Inferenz.</span><span class="sxs-lookup"><span data-stu-id="20699-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="20699-727">Stattdessen werden alle `Xi` als *nicht korrigiert*betrachtet, und ein untergeordneter Daten *Rückschluss* wird *von* jedem Argumenttyp `Uj` von `D` *bis* zum entsprechenden Parametertyp `Tj` von `M` erstellt.</span><span class="sxs-lookup"><span data-stu-id="20699-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="20699-728">Wenn für eine der `Xi` keine Begrenzungen gefunden wurden, schlägt der Typrückschluss fehl.</span><span class="sxs-lookup"><span data-stu-id="20699-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="20699-729">Andernfalls werden alle `Xi` an die entsprechenden `Si` *korrigiert* , die das Ergebnis des Typrückschlusses sind.</span><span class="sxs-lookup"><span data-stu-id="20699-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="20699-730">Ermitteln des am häufigsten verbreiteten Typs eines Satzes von Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="20699-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="20699-731">In einigen Fällen muss ein gemeinsamer Typ für einen Satz von Ausdrücken abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="20699-732">Insbesondere werden die Elementtypen von implizit typisierten Arrays und die Rückgabe Typen anonymer Funktionen mit *Block* Text auf diese Weise gefunden.</span><span class="sxs-lookup"><span data-stu-id="20699-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="20699-733">Intuitiv, wenn ein Satz von Ausdrücken `E1...Em`, sollte dieser Rückschluss dem Aufrufen einer Methode entsprechen.</span><span class="sxs-lookup"><span data-stu-id="20699-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="20699-734">mit dem `Ei` als Argumente.</span><span class="sxs-lookup"><span data-stu-id="20699-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="20699-735">Genauer ist, beginnt der Rückschluss mit einer Variablen vom Typ " *unfixed* " `X`.</span><span class="sxs-lookup"><span data-stu-id="20699-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="20699-736">Die *Ausschlüsse auf Ausgabetyp* werden dann *von* den einzelnen `Ei` *bis* `X` vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="20699-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="20699-737">Schließlich wird `X` *korrigiert* , und wenn der Vorgang erfolgreich ist, ist der resultierende Typ `S` der resultierende am besten geeignete Typ für die Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="20699-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="20699-738">Wenn keine dieser `S` vorhanden ist, haben die Ausdrücke keinen besten allgemeinen Typ.</span><span class="sxs-lookup"><span data-stu-id="20699-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="20699-739">Überladungsauflösung</span><span class="sxs-lookup"><span data-stu-id="20699-739">Overload resolution</span></span>

<span data-ttu-id="20699-740">Bei der Überladungs Auflösung handelt es sich um einen Bindungs zeitmechanismus zum Auswählen des besten Funktionsmembers, der beim Aufrufen einer Argumentliste und eines Satzes von Kandidaten Funktions Membern aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="20699-741">Die Überladungs Auflösung wählt den Funktions Member aus, der in den folgenden C#unterschiedlichen Kontexten in aufgerufen werden soll:</span><span class="sxs-lookup"><span data-stu-id="20699-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="20699-742">Aufrufen einer Methode mit dem Namen in einem *invocation_expression* -Aufruf ([Methodenaufrufe](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="20699-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="20699-743">Aufruf eines Instanzkonstruktors namens in einem *object_creation_expression* -[Objekt (Objekt Erstellungs Ausdrücke](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="20699-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="20699-744">Aufruf eines Indexeraccessors über einen *element_access* ([Element Zugriff](expressions.md#element-access)).</span><span class="sxs-lookup"><span data-stu-id="20699-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="20699-745">Aufruf eines vordefinierten oder benutzerdefinierten Operators, auf den in einem Ausdruck verwiesen wird ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution) und [binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="20699-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="20699-746">Jeder dieser Kontexte definiert den Satz von Kandidaten Funktionsmembern und die Liste der Argumente in der eigenen, eindeutigen Weise, wie in den oben aufgeführten Abschnitten ausführlich beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="20699-747">Beispielsweise enthält der Satz von Kandidaten für einen Methodenaufruf keine Methoden, die `override` ([Member Suche](expressions.md#member-lookup)) gekennzeichnet sind, und Methoden in einer Basisklasse sind keine Kandidaten, wenn eine Methode in einer abgeleiteten Klasse anwendbar ist (Methoden[Aufrufe](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="20699-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="20699-748">Nachdem die Kandidaten Funktions Member und die Argumentliste identifiziert wurden, ist die Auswahl des besten Funktionsmembers in allen Fällen identisch:</span><span class="sxs-lookup"><span data-stu-id="20699-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="20699-749">In Anbetracht der Menge der anwendbaren Kandidaten Funktionsmember befindet sich das beste Funktionsmember in dieser Gruppe.</span><span class="sxs-lookup"><span data-stu-id="20699-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="20699-750">Wenn die Menge nur ein Funktionsmember enthält, ist dieses Funktionsmember das beste Funktionsmember.</span><span class="sxs-lookup"><span data-stu-id="20699-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="20699-751">Andernfalls ist das beste Funktionsmember der ein Funktionsmember, der besser als alle anderen Funktionsmember in Bezug auf die angegebene Argumentliste ist, vorausgesetzt, dass jeder Funktionsmember mit allen anderen Funktionsmembern verglichen wird, wobei die Regeln in besserer Funktion verwendet werden. [ -Member](expressions.md#better-function-member).</span><span class="sxs-lookup"><span data-stu-id="20699-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="20699-752">Wenn nicht genau ein Funktionsmember vorhanden ist, der besser als alle anderen Funktionsmember ist, dann ist der Funktionselement Aufruf mehrdeutig, und ein Bindungs Fehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="20699-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="20699-753">In den folgenden Abschnitten wird die genaue Bedeutung des ***anwendbaren Funktionsmembers*** und des ***besseren Funktionsmembers***definiert.</span><span class="sxs-lookup"><span data-stu-id="20699-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="20699-754">Anwendbares Funktionsmember</span><span class="sxs-lookup"><span data-stu-id="20699-754">Applicable function member</span></span>

<span data-ttu-id="20699-755">Ein Funktionsmember ist ein ***anwendbares Funktionsmember*** in Bezug auf eine Argumentliste `A`, wenn alle der folgenden Aussagen zutreffen:</span><span class="sxs-lookup"><span data-stu-id="20699-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="20699-756">Jedes Argument in `A` entspricht einem Parameter in der Deklaration des Funktions Members, wie in den [entsprechenden Parametern](expressions.md#corresponding-parameters)beschrieben, und jeder Parameter, dem kein Argument entspricht, ist ein optionaler Parameter.</span><span class="sxs-lookup"><span data-stu-id="20699-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="20699-757">Für jedes Argument in `A` ist der Parameter Übergabe Modus des Arguments (d. h. Value, `ref` oder `out`) mit dem Parameter Übergabe Modus des entsprechenden Parameters identisch.</span><span class="sxs-lookup"><span data-stu-id="20699-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="20699-758">für einen value-Parameter oder ein Parameter Array ist eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) vom-Argument in den Typ des entsprechenden Parameters vorhanden, oder</span><span class="sxs-lookup"><span data-stu-id="20699-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="20699-759">bei einem `ref`-oder `out`-Parameter ist der Typ des Arguments mit dem Typ des entsprechenden Parameters identisch.</span><span class="sxs-lookup"><span data-stu-id="20699-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="20699-760">Schließlich handelt es sich bei dem Parameter "`ref`" oder "`out`" um einen Alias für das übergebenen Argument.</span><span class="sxs-lookup"><span data-stu-id="20699-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="20699-761">Wenn ein Funktionsmember, der ein Parameter Array enthält, das Funktionsmember durch die oben genannten Regeln anwendbar ist, wird es in der ***normalen Form***als anwendbar bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="20699-762">Wenn ein Funktionsmember, der ein Parameter Array enthält, nicht in der normalen Form anwendbar ist, kann der Funktions Member stattdessen in der ***erweiterten Form***angewendet werden:</span><span class="sxs-lookup"><span data-stu-id="20699-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="20699-763">Das erweiterte Formular wird erstellt, indem das Parameter Array in der Funktionsmember-Deklaration durch Null oder mehr Wert Parameter des Elementtyps des Parameter Arrays ersetzt wird, sodass die Anzahl der Argumente in der Argumentliste `A` mit der Gesamtzahl übereinstimmt. von Parametern.</span><span class="sxs-lookup"><span data-stu-id="20699-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="20699-764">Wenn `A` weniger Argumente aufweist als die Anzahl fester Parameter in der Deklaration des Funktions Members, kann die erweiterte Form des Funktionsmembers nicht erstellt werden und ist daher nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="20699-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="20699-765">Andernfalls ist das erweiterte Formular anwendbar, wenn für jedes Argument in `A` der Parameter Übergabe Modus des Arguments mit dem Parameter Übergabe Modus des entsprechenden Parameters identisch ist, und</span><span class="sxs-lookup"><span data-stu-id="20699-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="20699-766">bei einem Parameter mit festem Wert oder einem Wert Parameter, der durch die Erweiterung erstellt wurde, ist eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) vom Typ des Arguments in den Typ des entsprechenden Parameters vorhanden.</span><span class="sxs-lookup"><span data-stu-id="20699-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="20699-767">bei einem `ref`-oder `out`-Parameter ist der Typ des Arguments mit dem Typ des entsprechenden Parameters identisch.</span><span class="sxs-lookup"><span data-stu-id="20699-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="20699-768">Besseres Funktionsmember</span><span class="sxs-lookup"><span data-stu-id="20699-768">Better function member</span></span>

<span data-ttu-id="20699-769">Zum Ermitteln des besseren Funktionsmembers wird eine Liste der aus dem ausgebauten Argument enthaltenen Elemente erstellt, die nur die Argument Ausdrücke selbst in der Reihenfolge enthält, in der Sie in der ursprünglichen Argumentliste angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="20699-770">Parameter Listen für jeden der Kandidaten Funktionsmember werden wie folgt erstellt:</span><span class="sxs-lookup"><span data-stu-id="20699-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="20699-771">Das erweiterte Formular wird verwendet, wenn das Funktionsmember nur in der erweiterten Form anwendbar ist.</span><span class="sxs-lookup"><span data-stu-id="20699-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="20699-772">Optionale Parameter ohne entsprechende Argumente werden aus der Parameterliste entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="20699-773">Die Parameter werden neu angeordnet, sodass Sie an derselben Position wie das entsprechende Argument in der Argumentliste auftreten.</span><span class="sxs-lookup"><span data-stu-id="20699-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="20699-774">Wenn eine Argumentliste `A` mit einem Satz von Argument Ausdrücken `{E1, E2, ..., En}` und zwei anwendbaren Funktionsmembern `Mp` und `Mq` mit den Parametertypen `{P1, P2, ..., Pn}` und `{Q1, Q2, ..., Qn}` ist, wird `Mp` als ein ***besseres Funktionsmember*** definiert als `Mq`, wenn</span><span class="sxs-lookup"><span data-stu-id="20699-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="20699-775">für jedes Argument ist die implizite Konvertierung von `Ex` in `Qx` nicht besser als die implizite Konvertierung von `Ex` in `Px` und</span><span class="sxs-lookup"><span data-stu-id="20699-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="20699-776">für mindestens ein Argument ist die Konvertierung von `Ex` in `Px` besser als die Konvertierung von `Ex` in `Qx`.</span><span class="sxs-lookup"><span data-stu-id="20699-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="20699-777">Wenn `Mp` oder `Mq` in der erweiterten Form anwendbar ist, verweist `Px` oder `Qx` auf einen Parameter in der erweiterten Form der Parameterliste.</span><span class="sxs-lookup"><span data-stu-id="20699-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="20699-778">Falls der Parametertyp Sequenzen @ no__t-0 und `{Q1, Q2, ..., Qn}` äquivalent sind (d. h., jede `Pi` weist eine Identitäts Konvertierung in den entsprechenden `Qi` auf), werden die folgenden Regeln zum Durchbrechen von Regeln angewendet, um den besseren Funktionsmember zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="20699-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="20699-779">Wenn `Mp` eine nicht generische Methode und `Mq` eine generische Methode ist, ist `Mp` besser als `Mq`.</span><span class="sxs-lookup"><span data-stu-id="20699-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="20699-780">Wenn `Mp` in der normalen Form anwendbar ist und `Mq` ein `params`-Array hat und nur in der erweiterten Form anwendbar ist, ist `Mp` besser als `Mq`.</span><span class="sxs-lookup"><span data-stu-id="20699-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="20699-781">Wenn `Mp` mehr deklarierte Parameter als `Mq` aufweist, ist `Mp` besser als `Mq`.</span><span class="sxs-lookup"><span data-stu-id="20699-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="20699-782">Dies kann vorkommen, wenn beide Methoden `params`-Arrays aufweisen und nur in ihren erweiterten Formularen anwendbar sind.</span><span class="sxs-lookup"><span data-stu-id="20699-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="20699-783">Andernfalls, wenn alle Parameter von `Mp` ein entsprechendes Argument aufweisen, während Standardargumente mindestens einen optionalen Parameter in `Mq` ersetzen müssen, ist `Mp` besser als `Mq`.</span><span class="sxs-lookup"><span data-stu-id="20699-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="20699-784">Andernfalls ist `Mp` besser als `Mq`, wenn `Mp` spezifischere Parametertypen als `Mq` aufweist.</span><span class="sxs-lookup"><span data-stu-id="20699-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="20699-785">Let `{R1, R2, ..., Rn}` und `{S1, S2, ..., Sn}` stellen die nicht instanziierten und nicht erweiterten Parametertypen `Mp` und `Mq` dar.</span><span class="sxs-lookup"><span data-stu-id="20699-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="20699-786">die Parametertypen von `Mp` sind spezifischer als `Mq`, wenn für jeden Parameter `Rx` nicht weniger spezifisch als `Sx` ist, und für mindestens einen Parameter ist `Rx` spezifischer als `Sx`:</span><span class="sxs-lookup"><span data-stu-id="20699-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="20699-787">Ein Typparameter ist weniger spezifisch als ein nicht-Typparameter.</span><span class="sxs-lookup"><span data-stu-id="20699-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="20699-788">Ein konstruierter Typ ist rekursiv spezifischer als ein anderer konstruierter Typ (mit der gleichen Anzahl von Typargumenten), wenn mindestens ein Typargument spezifischer ist und kein Typargument weniger spezifisch als das entsprechende Typargument in der anderen ist.</span><span class="sxs-lookup"><span data-stu-id="20699-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="20699-789">Ein Arraytyp ist spezifischer als ein anderer Arraytyp (mit der gleichen Anzahl von Dimensionen), wenn der Elementtyp des ersten spezifischeren ist als der Elementtyp der zweiten.</span><span class="sxs-lookup"><span data-stu-id="20699-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="20699-790">Wenn ein Member ein nicht gesperrter Operator und der andere ein gesperrter Operator ist, ist der nicht--gesteigerte-Operator besser.</span><span class="sxs-lookup"><span data-stu-id="20699-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="20699-791">Andernfalls ist keines der Funktionsmember besser.</span><span class="sxs-lookup"><span data-stu-id="20699-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="20699-792">Bessere Konvertierung von Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="20699-792">Better conversion from expression</span></span>

<span data-ttu-id="20699-793">Bei einer impliziten Konvertierung `C1`, die von einem Ausdruck `E` in einen Typ `T1` konvertiert, und einer impliziten Konvertierung `C2`, die von einem Ausdruck `E` in einen Typ `T2` konvertiert, ist `C1` eine ***bessere Konvertierung*** als `C2`, wenn @no__ t-9 stimmt nicht genau mit 0 überein, und mindestens einer der folgenden Punkte enthält Folgendes:</span><span class="sxs-lookup"><span data-stu-id="20699-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="20699-794">`E` entspricht genau `T1` ([exakt übereinstimmenden Ausdruck](expressions.md#exactly-matching-expression)).</span><span class="sxs-lookup"><span data-stu-id="20699-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="20699-795">`T1` ist ein besseres Konvertierungs Ziel als `T2` ([besseres Konvertierungs Ziel](expressions.md#better-conversion-target)).</span><span class="sxs-lookup"><span data-stu-id="20699-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="20699-796">Exakt übereinstimmender Ausdruck</span><span class="sxs-lookup"><span data-stu-id="20699-796">Exactly matching Expression</span></span>

<span data-ttu-id="20699-797">Bei Angabe eines Ausdrucks `E` und eines Typs `T` entspricht `E` exakt `T`, wenn eine der folgenden Punkte enthält:</span><span class="sxs-lookup"><span data-stu-id="20699-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="20699-798">`E` weist einen Typ `S` auf, und es ist eine Identitäts Konvertierung von `S` in `T` vorhanden.</span><span class="sxs-lookup"><span data-stu-id="20699-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="20699-799">`E` ist eine anonyme Funktion, `T` entweder ein Delegattyp ist `D` oder ein Ausdrucks bauentyp `Expression<D>` und einer der folgenden Punkte:</span><span class="sxs-lookup"><span data-stu-id="20699-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="20699-800">Ein abzurufende Rückgabetyp `X` ist für `E` im Kontext der Parameterliste von `D` (abzurufenden[Rückgabetyp](expressions.md#inferred-return-type)) vorhanden, und eine Identitäts Konvertierung von `X` in den Rückgabetyp `D`</span><span class="sxs-lookup"><span data-stu-id="20699-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="20699-801">@No__t-0 ist nicht Async, und `D` hat einen Rückgabetyp `Y`, oder `E` ist async, und `D` hat einen Rückgabetyp `Task<Y>` und eine der folgenden Punkte:</span><span class="sxs-lookup"><span data-stu-id="20699-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="20699-802">Der Text von `E` ist ein Ausdruck, der exakt mit `Y` übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="20699-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="20699-803">Der Text von `E` ist ein Anweisungsblock, bei dem jede Return-Anweisung einen Ausdruck zurückgibt, der exakt mit `Y` übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="20699-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="20699-804">Besseres Konvertierungs Ziel</span><span class="sxs-lookup"><span data-stu-id="20699-804">Better conversion target</span></span>

<span data-ttu-id="20699-805">Bei zwei verschiedenen Typen `T1` und `T2` ist `T1` ein besseres Konvertierungs Ziel als `T2`, wenn keine implizite Konvertierung von `T2` in `T1` vorhanden ist und mindestens eine der folgenden Punkte enthält:</span><span class="sxs-lookup"><span data-stu-id="20699-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="20699-806">Eine implizite Konvertierung von `T1` in `T2` ist vorhanden.</span><span class="sxs-lookup"><span data-stu-id="20699-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="20699-807">`T1` ist entweder ein Delegattyp `D1` oder ein Ausdrucks bauentyp `Expression<D1>`, `T2` ist entweder ein Delegattyp `D2` oder ein Ausdrucks bauentyp `Expression<D2>`, `D1` hat einen Rückgabetyp `S1` und eine der folgenden Punkte :</span><span class="sxs-lookup"><span data-stu-id="20699-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="20699-808">die Rückgabe von `D2` ist ungültig.</span><span class="sxs-lookup"><span data-stu-id="20699-808">`D2` is void returning</span></span>
   * <span data-ttu-id="20699-809">`D2` hat einen Rückgabetyp `S2`, und `S1` ist ein besseres Konvertierungs Ziel als `S2`</span><span class="sxs-lookup"><span data-stu-id="20699-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="20699-810">`T1` ist `Task<S1>`, `T2` `Task<S2>` und `S1` ein besseres Konvertierungs Ziel als `S2`</span><span class="sxs-lookup"><span data-stu-id="20699-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="20699-811">`T1` ist `S1` oder `S1?`, wobei `S1` ein ganzzahliger Typ mit Vorzeichen und `T2` `S2` oder `S2?` ist, wobei `S2` ein ganzzahliger Typ ohne Vorzeichen ist.</span><span class="sxs-lookup"><span data-stu-id="20699-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="20699-812">Dies gilt insbesondere in folgenden Fällen:</span><span class="sxs-lookup"><span data-stu-id="20699-812">Specifically:</span></span>
   * <span data-ttu-id="20699-813">`S1` ist `sbyte` und `S2` `byte`, `ushort`, `uint` oder `ulong`</span><span class="sxs-lookup"><span data-stu-id="20699-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="20699-814">`S1` ist `short` und `S2` `ushort`, `uint` oder `ulong`</span><span class="sxs-lookup"><span data-stu-id="20699-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="20699-815">`S1` ist `int` und `S2` `uint` oder `ulong`</span><span class="sxs-lookup"><span data-stu-id="20699-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="20699-816">`S1` ist `long` und `S2` `ulong`</span><span class="sxs-lookup"><span data-stu-id="20699-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="20699-817">Überladen in generischen Klassen</span><span class="sxs-lookup"><span data-stu-id="20699-817">Overloading in generic classes</span></span>

<span data-ttu-id="20699-818">Obwohl Signaturen wie deklariert eindeutig sein müssen, ist es möglich, dass die Ersetzung von Typargumenten zu identischen Signaturen führt.</span><span class="sxs-lookup"><span data-stu-id="20699-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="20699-819">In solchen Fällen werden die oben genannten Regeln der Überladungs Auflösung über den spezifischsten Member ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="20699-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="20699-820">Die folgenden Beispiele zeigen über Ladungen, die gemäß dieser Regel gültig und ungültig sind:</span><span class="sxs-lookup"><span data-stu-id="20699-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="20699-821">Kompilierzeit Überprüfung der Auflösung dynamischer Überladungen</span><span class="sxs-lookup"><span data-stu-id="20699-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="20699-822">Bei den meisten dynamisch gebundenen Vorgängen ist der Satz möglicher Kandidaten für die Auflösung zur Kompilierzeit nicht bekannt.</span><span class="sxs-lookup"><span data-stu-id="20699-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="20699-823">In bestimmten Fällen ist der Kandidaten Satz jedoch zum Zeitpunkt der Kompilierung bekannt:</span><span class="sxs-lookup"><span data-stu-id="20699-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="20699-824">Statische Methodenaufrufe mit dynamischen Argumenten</span><span class="sxs-lookup"><span data-stu-id="20699-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="20699-825">Instanzmethodenaufrufe, bei denen der Empfänger kein dynamischer Ausdruck ist</span><span class="sxs-lookup"><span data-stu-id="20699-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="20699-826">Indexer-Aufrufe, bei denen der Empfänger kein dynamischer Ausdruck ist</span><span class="sxs-lookup"><span data-stu-id="20699-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="20699-827">Konstruktoraufrufe mit dynamischen Argumenten</span><span class="sxs-lookup"><span data-stu-id="20699-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="20699-828">In diesen Fällen wird für jeden Kandidaten eine beschränkte Kompilierzeit Überprüfung durchgeführt, um festzustellen, ob eine dieser Vorgänge möglicherweise zur Laufzeit angewendet werden könnte. Diese Überprüfung umfasst die folgenden Schritte:</span><span class="sxs-lookup"><span data-stu-id="20699-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="20699-829">Partieller Typrückschluss: Alle Typargumente, die nicht direkt oder indirekt auf ein Argument vom Typ "`dynamic`" angewiesen sind, werden mithilfe der Regeln des [Typrückschlusses](expressions.md#type-inference) abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="20699-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="20699-830">Die übrigen Typargumente sind unbekannt.</span><span class="sxs-lookup"><span data-stu-id="20699-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="20699-831">Partielle anwendbarkeits Überprüfung: Die Anwendbarkeit wird gemäß dem [anwendbaren Funktionsmember](expressions.md#applicable-function-member)überprüft, wobei Parameter ignoriert werden, deren Typen unbekannt sind.</span><span class="sxs-lookup"><span data-stu-id="20699-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="20699-832">Wenn kein Kandidat diesen Test durchläuft, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="20699-833">Funktionselement Aufruf</span><span class="sxs-lookup"><span data-stu-id="20699-833">Function member invocation</span></span>

<span data-ttu-id="20699-834">In diesem Abschnitt wird der Prozess beschrieben, der zur Laufzeit ausgeführt wird, um einen bestimmten Funktionsmember aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="20699-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="20699-835">Es wird davon ausgegangen, dass ein Bindungs Zeit Prozess bereits den aufzurufenden Member ermittelt hat, möglicherweise durch Anwenden der Überladungs Auflösung auf einen Satz von Kandidaten Funktionsmembern.</span><span class="sxs-lookup"><span data-stu-id="20699-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="20699-836">Um den Aufrufprozess zu beschreiben, werden Funktionsmember in zwei Kategorien unterteilt:</span><span class="sxs-lookup"><span data-stu-id="20699-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="20699-837">Statische Funktionsmember.</span><span class="sxs-lookup"><span data-stu-id="20699-837">Static function members.</span></span> <span data-ttu-id="20699-838">Dabei handelt es sich um Instanzkonstruktoren, statische Methoden, statische Eigenschaftenaccessoren und benutzerdefinierte Operatoren.</span><span class="sxs-lookup"><span data-stu-id="20699-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="20699-839">Statische Funktionsmember sind immer nicht virtuell.</span><span class="sxs-lookup"><span data-stu-id="20699-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="20699-840">Instanzfunktionsmember.</span><span class="sxs-lookup"><span data-stu-id="20699-840">Instance function members.</span></span> <span data-ttu-id="20699-841">Dabei handelt es sich um Instanzmethoden, Accessoren für Instanzeigenschaften und Indexer-Accessoren.</span><span class="sxs-lookup"><span data-stu-id="20699-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="20699-842">Instanzfunktionsmember sind entweder nicht virtuell oder virtuell und werden immer für eine bestimmte Instanz aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="20699-843">Die Instanz wird durch einen Instanzausdruck berechnet und kann innerhalb des Funktionsmembers als `this` ([dieser Zugriff](expressions.md#this-access)) verfügbar sein.</span><span class="sxs-lookup"><span data-stu-id="20699-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="20699-844">Die Lauf Zeit Verarbeitung eines Funktionsmember-aufzurufenden besteht aus den folgenden Schritten, wobei `M` der Funktionsmember und, wenn `M` ein Instanzmember ist, `E` der Instanzausdruck ist:</span><span class="sxs-lookup"><span data-stu-id="20699-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="20699-845">Wenn `M` ein statisches Funktionsmember ist:</span><span class="sxs-lookup"><span data-stu-id="20699-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="20699-846">Die Argumentliste wird entsprechend der Beschreibung in den [Argumentlisten](expressions.md#argument-lists)ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="20699-847">`M` wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-847">`M` is invoked.</span></span>

*  <span data-ttu-id="20699-848">Wenn `M` ein Instanzfunktionsmember ist, der in einem *value_type*deklariert ist:</span><span class="sxs-lookup"><span data-stu-id="20699-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="20699-849">`E` wird ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-849">`E` is evaluated.</span></span> <span data-ttu-id="20699-850">Wenn diese Auswertung eine Ausnahme verursacht, werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="20699-851">Wenn `E` nicht als Variable klassifiziert ist, wird eine temporäre lokale Variable mit dem Typ `E` erstellt, und der Wert von `E` wird dieser Variablen zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="20699-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="20699-852">`E` wird dann als Verweis auf diese temporäre lokale Variable neu klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="20699-853">Auf die temporäre Variable kann in `M` als `this` zugegriffen werden, aber nicht auf andere Weise.</span><span class="sxs-lookup"><span data-stu-id="20699-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="20699-854">Daher kann der Aufrufer nur dann, wenn `E` eine echte Variable ist, die Änderungen beobachten, die `M` an `this` vornimmt.</span><span class="sxs-lookup"><span data-stu-id="20699-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="20699-855">Die Argumentliste wird entsprechend der Beschreibung in den [Argumentlisten](expressions.md#argument-lists)ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="20699-856">`M` wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-856">`M` is invoked.</span></span> <span data-ttu-id="20699-857">Die Variable, auf die `E` verweist, wird zur Variablen, auf die von `this` verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="20699-858">Wenn `M` ein Instanzfunktionsmember ist, der in einem *reference_type*deklariert ist:</span><span class="sxs-lookup"><span data-stu-id="20699-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="20699-859">`E` wird ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-859">`E` is evaluated.</span></span> <span data-ttu-id="20699-860">Wenn diese Auswertung eine Ausnahme verursacht, werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="20699-861">Die Argumentliste wird entsprechend der Beschreibung in den [Argumentlisten](expressions.md#argument-lists)ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="20699-862">Wenn der Typ `E` ein *value_type*ist, wird eine Boxing-Konvertierung ([Boxing-Konvertierungen](types.md#boxing-conversions)) ausgeführt, um `E` in den Typ `object` zu konvertieren, und `E` wird in den folgenden Schritten als Typ `object` angesehen.</span><span class="sxs-lookup"><span data-stu-id="20699-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="20699-863">In diesem Fall kann `M` nur ein Member von `System.Object` sein.</span><span class="sxs-lookup"><span data-stu-id="20699-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="20699-864">Der Wert von "`E`" wird als gültig geprüft.</span><span class="sxs-lookup"><span data-stu-id="20699-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="20699-865">Wenn der Wert `E` `null` ist, wird ein `System.NullReferenceException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="20699-866">Die aufzurufende Funktionsmember-Implementierung wird bestimmt:</span><span class="sxs-lookup"><span data-stu-id="20699-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="20699-867">Wenn der Bindungstyp `E` eine Schnittstelle ist, ist das aufzurufende Funktionsmember die Implementierung von `M`, die durch den Lauf Zeittyp der Instanz bereitgestellt wird, auf die von `E` verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="20699-868">Dieses Funktionsmember wird bestimmt, indem die Schnittstellen Zuordnungs Regeln ([Schnittstellen Zuordnung](interfaces.md#interface-mapping)) angewendet werden, um die Implementierung von `M` festzustellen, die vom Lauf Zeittyp der Instanz bereitgestellt wird, auf die von `E` verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="20699-869">Andernfalls ist das aufzurufende Funktionsmember die Implementierung von `M`, die durch den Lauf Zeittyp der Instanz bereitgestellt wird, auf die `E` verweist, wenn `M` ein virtuelles Funktionsmember ist.</span><span class="sxs-lookup"><span data-stu-id="20699-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="20699-870">Dieses Funktionsmember wird festgelegt, indem die Regeln zum Bestimmen der am weitesten abgeleiteten Implementierung ([virtuelle Methoden](classes.md#virtual-methods)) von `M` in Bezug auf den Lauf Zeittyp der Instanz, auf die von `E` verwiesen wird, angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="20699-871">Andernfalls ist `M` ein nicht virtuelles Funktionsmember, und der aufzurufende Funktionsmember ist `M` selbst.</span><span class="sxs-lookup"><span data-stu-id="20699-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="20699-872">Die im obigen Schritt festgelegte Funktionsmember-Implementierung wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="20699-873">Das Objekt, auf das `E` verweist, wird das Objekt, auf das von `this` verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="20699-874">Aufrufe für geboxte Instanzen</span><span class="sxs-lookup"><span data-stu-id="20699-874">Invocations on boxed instances</span></span>

<span data-ttu-id="20699-875">Ein Funktionsmember, der in einem *value_type* -Element implementiert ist, kann in den folgenden Situationen über eine geboxte Instanz von *value_type* aufgerufen werden:</span><span class="sxs-lookup"><span data-stu-id="20699-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="20699-876">Wenn das Funktionsmember eine `override` einer Methode ist, die vom Typ `object` geerbt wird und durch einen Instanzausdruck vom Typ `object` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="20699-877">Wenn das Funktionsmember eine Implementierung eines Schnittstellenfunktionsmembers ist und durch einen Instanzausdruck eines *INTERFACE_TYPE*aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="20699-878">Wenn der Funktionsmember durch einen Delegaten aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="20699-879">In diesen Fällen wird die geschachtelte Instanz als eine Variable des *value_type*-Elements betrachtet, und diese Variable wird zur Variablen, auf die von `this` innerhalb des Funktionselement aufzurufenden verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="20699-880">Dies bedeutet insbesondere, dass beim Aufrufen eines Funktionsmembers für eine geachtelte Instanz der Funktionsmember den in der geboxten Instanz enthaltenen Wert ändern kann.</span><span class="sxs-lookup"><span data-stu-id="20699-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="20699-881">Primäre Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-881">Primary expressions</span></span>

<span data-ttu-id="20699-882">Primäre Ausdrücke enthalten die einfachsten Formen von Ausdrücken.</span><span class="sxs-lookup"><span data-stu-id="20699-882">Primary expressions include the simplest forms of expressions.</span></span>

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

<span data-ttu-id="20699-883">Primäre Ausdrücke sind unterteilt zwischen *array_creation_expression*s und *primary_no_array_creation_expression*s.</span><span class="sxs-lookup"><span data-stu-id="20699-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="20699-884">Wenn Sie Array-Creation-Expression auf diese Weise behandeln, anstatt sie zusammen mit den anderen einfachen Ausdrucks Formularen aufzulisten, ermöglicht die Grammatik, potenziell verwirrenden Code, wie z. b.</span><span class="sxs-lookup"><span data-stu-id="20699-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="20699-885">der andernfalls interpretiert werden würde.</span><span class="sxs-lookup"><span data-stu-id="20699-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="20699-886">Literale</span><span class="sxs-lookup"><span data-stu-id="20699-886">Literals</span></span>

<span data-ttu-id="20699-887">Ein *primary_expression* , das aus einem Literalzeichen ([Literale](lexical-structure.md#literals)) besteht, wird als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="20699-888">Interpolierte Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="20699-888">Interpolated strings</span></span>

<span data-ttu-id="20699-889">Ein *interpolated_string_expression* besteht aus einem `$`-Vorzeichen, gefolgt von einem regulären oder ausführlichen Zeichenfolgenliteral, bei dem Lücken, getrennt durch `{` und `}`, Ausdrücke und Formatierungs Spezifikationen einschließen.</span><span class="sxs-lookup"><span data-stu-id="20699-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="20699-890">Ein interintererter Zeichen folgen Ausdruck ist das Ergebnis einer *interpolated_string_literal* , die in einzelne Token unterteilt wurde, wie in [interpoliert-Zeichenfolgenliteralen](lexical-structure.md#interpolated-string-literals)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

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

<span data-ttu-id="20699-891">Der *constant_expression* -in einer Interpolations muss eine implizite Konvertierung in `int` aufweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="20699-892">Ein *interpolated_string_expression* -Wert wird als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="20699-893">Wenn Sie sofort mit einer impliziten interdimensionalen Zeichen folgen Konvertierung in `System.IFormattable` oder `System.FormattableString` konvertiert wird ([implizite interkretionierte Zeichen folgen Konvertierungen](conversions.md#implicit-interpolated-string-conversions)), hat der interpoliert Zeichen folgen Ausdruck diesen Typ.</span><span class="sxs-lookup"><span data-stu-id="20699-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="20699-894">Andernfalls weist Sie den Typ "`string`" auf.</span><span class="sxs-lookup"><span data-stu-id="20699-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="20699-895">Wenn der Typ einer interinterierten Zeichenfolge `System.IFormattable` oder `System.FormattableString` ist, handelt es sich bei der Bedeutung um einen-Befehl `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="20699-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="20699-896">Wenn der Typ `string` ist, ist die Bedeutung des Ausdrucks ein aufrufender `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="20699-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="20699-897">In beiden Fällen besteht die Argumentliste des Aufrufes aus einem Format Zeichenfolgenliteralzeichen mit Platzhaltern für jede interpolung und einem Argument für jeden Ausdruck, der den Platzhaltern entspricht.</span><span class="sxs-lookup"><span data-stu-id="20699-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="20699-898">Der Format Zeichenfolgenliterale wird wie folgt erstellt, wobei `N` die Anzahl der Interpolationen in *interpolated_string_expression*ist:</span><span class="sxs-lookup"><span data-stu-id="20699-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="20699-899">Wenn ein *interpolated_regular_string_whole* oder ein *interpolated_verbatim_string_whole* auf das `$`-Zeichen folgt, ist das Format Zeichenfolgenliteral dieses Token.</span><span class="sxs-lookup"><span data-stu-id="20699-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="20699-900">Andernfalls besteht das Format Zeichenfolgenliterale aus folgendem:</span><span class="sxs-lookup"><span data-stu-id="20699-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="20699-901">Zuerst die *interpolated_regular_string_start* oder *interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="20699-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="20699-902">Dann für jede Zahl `I` von `0` bis `N-1`:</span><span class="sxs-lookup"><span data-stu-id="20699-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="20699-903">Die Dezimal Darstellung von `I`</span><span class="sxs-lookup"><span data-stu-id="20699-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="20699-904">Wenn die entsprechende *Interpolations* *constant_expression*, ein `,` (Komma), gefolgt von der Dezimal Darstellung des Werts des *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="20699-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="20699-905">Danach folgt *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* oder *interpolated_verbatim_string_end* direkt auf die entsprechende interpolung.</span><span class="sxs-lookup"><span data-stu-id="20699-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="20699-906">Die nachfolgenden Argumente sind einfach die *Ausdrücke* aus den *Interpolationen* (sofern vorhanden), in der richtigen Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="20699-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="20699-907">TODO: Beispiele.</span><span class="sxs-lookup"><span data-stu-id="20699-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="20699-908">Einfache Namen</span><span class="sxs-lookup"><span data-stu-id="20699-908">Simple names</span></span>

<span data-ttu-id="20699-909">Ein *Simple_name* besteht aus einem Bezeichner, optional gefolgt von einer Typargument Liste:</span><span class="sxs-lookup"><span data-stu-id="20699-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="20699-910">Ein *Simple_name* hat entweder das Format `I` oder die Form `I<A1,...,Ak>`, wobei `I` ein einzelner Bezeichner und `<A1,...,Ak>` ein optionales *type_argument_list*ist.</span><span class="sxs-lookup"><span data-stu-id="20699-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="20699-911">Wenn *type_argument_list* nicht angegeben ist, sollten Sie `K` NULL sein.</span><span class="sxs-lookup"><span data-stu-id="20699-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="20699-912">Der *Simple_name* wird wie folgt ausgewertet und klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="20699-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="20699-913">Wenn `K` 0 (null) ist und die *Simple_name* innerhalb eines- *Blocks* angezeigt wird und der *Block*(oder ein einschließender *Block*) der Deklaration der lokalen Variablen Deklaration ([Deklarationen](basic-concepts.md#declarations)) eine lokale Variable, einen Parameter oder eine Konstante mit enthält. Name @ no__t-6, dann verweist *Simple_name* auf diese lokale Variable, den Parameter oder die Konstante und wird als Variable oder Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="20699-914">Wenn `K` 0 (null) ist und *Simple_name* innerhalb des Texts einer generischen Methoden Deklaration angezeigt wird und diese Deklaration einen Typparameter mit dem Namen @ no__t-2 enthält, verweist *Simple_name* auf diesen Typparameter.</span><span class="sxs-lookup"><span data-stu-id="20699-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="20699-915">Andernfalls für jeden Instanztyp @ no__t-0 ([der Instanztyp](classes.md#the-instance-type)), beginnend mit dem Instanztyp der unmittelbar einschließenden Typdeklaration und fortsetzen mit dem Instanztyp jeder einschließenden Klasse oder Struktur Deklaration (sofern vorhanden):</span><span class="sxs-lookup"><span data-stu-id="20699-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="20699-916">Wenn `K` 0 (null) ist und die Deklaration von `T` einen Typparameter mit dem Namen @ no__t-2 enthält, verweist *Simple_name* auf diesen Typparameter.</span><span class="sxs-lookup"><span data-stu-id="20699-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="20699-917">Andernfalls ergibt eine Element Suche ([Member Suche](expressions.md#member-lookup)) von `I` in `T` mit `K` @ no__t-4type-Argumenten eine Entsprechung:</span><span class="sxs-lookup"><span data-stu-id="20699-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="20699-918">Wenn `T` der Instanztyp der unmittelbar einschließenden Klasse oder des Struktur Typs ist und die Suche eine oder mehrere Methoden identifiziert, ist das Ergebnis eine Methoden Gruppe mit einem zugeordneten Instanzausdruck `this`.</span><span class="sxs-lookup"><span data-stu-id="20699-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="20699-919">Wenn eine Typargument Liste angegeben wurde, wird Sie beim Aufrufen einer generischen Methode ([Methodenaufrufe](expressions.md#method-invocations)) verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="20699-920">Andernfalls, wenn `T` der Instanztyp der unmittelbar einschließenden Klasse oder des Struktur Typs ist, wenn die Suche einen Instanzmember identifiziert und der Verweis innerhalb des Texts eines Instanzkonstruktors, einer Instanzmethode oder eines Instanzaccessors auftritt, wird das Ergebnis ist identisch mit einem Member-Zugriff ([Member Access](expressions.md#member-access)) der Form `this.I`.</span><span class="sxs-lookup"><span data-stu-id="20699-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="20699-921">Dies kann nur vorkommen, wenn `K` 0 (null) ist.</span><span class="sxs-lookup"><span data-stu-id="20699-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="20699-922">Andernfalls ist das Ergebnis das gleiche wie ein Element Zugriff ([Member Access](expressions.md#member-access)) der Form `T.I` oder `T.I<A1,...,Ak>`.</span><span class="sxs-lookup"><span data-stu-id="20699-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="20699-923">In diesem Fall handelt es sich um einen Bindungs Zeit Fehler, damit *Simple_name* auf einen Instanzmember verweist.</span><span class="sxs-lookup"><span data-stu-id="20699-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="20699-924">Andernfalls werden die folgenden Schritte für jeden Namespace @ no__t-0, beginnend mit dem Namespace, in dem der *Simple_name* auftritt, mit den einzelnen einschließenden Namespaces (sofern vorhanden) und mit dem globalen Namespace ausgewertet, bis eine Entität gefunden wird:</span><span class="sxs-lookup"><span data-stu-id="20699-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="20699-925">Wenn `K` 0 (null) und `I` der Name eines Namespace in @ no__t-2 ist, dann gilt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="20699-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="20699-926">Wenn der Speicherort, an dem der *Simple_name* auftritt, von einer Namespace Deklaration für `N` eingeschlossen wird und die Namespace Deklaration ein *extern_alias_directive* oder *using_alias_directive* enthält, das den Namen @ no__t-4 einem Namespace oder Typ, dann ist *Simple_name* mehrdeutig, und ein Kompilierzeitfehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="20699-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="20699-927">Andernfalls verweist *Simple_name* auf den Namespace mit dem Namen "`I`" in `N`.</span><span class="sxs-lookup"><span data-stu-id="20699-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="20699-928">Wenn `N` einen zugänglichen Typ mit den Parametern "Name @ no__t-1" und "`K` @ no__t-3type" enthält, wird Folgendes angezeigt:</span><span class="sxs-lookup"><span data-stu-id="20699-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="20699-929">Wenn `K` 0 (null) ist und die Position, an der das *Simple_name* auftritt, von einer Namespace Deklaration für `N` eingeschlossen wird und die Namespace Deklaration ein *extern_alias_directive* oder *using_alias_directive* enthält, das den Name @ no__t-5 mit einem Namespace oder Typ, dann ist *Simple_name* mehrdeutig und ein Kompilierzeitfehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="20699-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="20699-930">Andernfalls verweist der *namespace_or_type_name* auf den Typ, der mit den angegebenen Typargumenten erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="20699-931">Andernfalls, wenn der Speicherort, an dem der *Simple_name* auftritt, von einer Namespace Deklaration für @ no__t-1 eingeschlossen wird:</span><span class="sxs-lookup"><span data-stu-id="20699-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="20699-932">Wenn `K` 0 (null) ist und die Namespace Deklaration ein *extern_alias_directive* -oder *using_alias_directive* -Wert enthält, der den Namen @ no__t-3 einem importierten Namespace oder Typ zuordnet, verweist *Simple_name* auf diesen Namespace. Sorte.</span><span class="sxs-lookup"><span data-stu-id="20699-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="20699-933">Andernfalls, wenn die von den *using_namespace_directive*s und *using_static_directive*s der Namespace Deklaration importierten Namespaces und Typdeklarationen genau einen zugreif baren Typ oder einen statischen Member ohne Erweiterung mit dem Namen @ no__ enthalten. t-2-und `K` @ no__t-4type-Parameter, dann verweist *Simple_name* auf diesen Typ oder Member, der mit den angegebenen Typargumenten erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="20699-934">Andernfalls, wenn die von den *using_namespace_directive*s der Namespace Deklaration importierten Namespaces und Typen mehr als einen zugreif baren Typ oder einen statischen Member mit dem Namen @ no__t-1 und `K` @ no__t-3type-Parameter enthalten. dann ist *Simple_name* mehrdeutig, und es tritt ein Fehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="20699-935">Beachten Sie, dass der gesamte Schritt genau mit dem entsprechenden Schritt bei der Verarbeitung eines *namespace_or_type_name* ([Namespace-und Typnamen](basic-concepts.md#namespace-and-type-names)) identisch ist.</span><span class="sxs-lookup"><span data-stu-id="20699-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="20699-936">Andernfalls ist *Simple_name* nicht definiert, und es tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="20699-937">Ausdrücke in Klammern</span><span class="sxs-lookup"><span data-stu-id="20699-937">Parenthesized expressions</span></span>

<span data-ttu-id="20699-938">Ein *parenthesized_expression* besteht aus einem *Ausdruck* , der in Klammern eingeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="20699-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="20699-939">Ein *parenthesized_expression* wird ausgewertet, indem der *Ausdruck* innerhalb der Klammern ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="20699-940">Wenn der *Ausdruck* innerhalb der Klammern einen Namespace oder Typ angibt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="20699-941">Andernfalls ist das Ergebnis der *parenthesized_expression* das Ergebnis der Auswertung des enthaltenen *Ausdrucks*.</span><span class="sxs-lookup"><span data-stu-id="20699-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="20699-942">Memberzugriff</span><span class="sxs-lookup"><span data-stu-id="20699-942">Member access</span></span>

<span data-ttu-id="20699-943">Ein *member_access* besteht aus einem *primary_expression*, einem *predefined_type*oder einem *qualified_alias_member*, gefolgt von einem "`.`"-Token, gefolgt von einem *Bezeichner*, optional gefolgt von einem *type_argument_list* .</span><span class="sxs-lookup"><span data-stu-id="20699-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

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

<span data-ttu-id="20699-944">Die *qualified_alias_member* -Produktion ist in [Namespace-alias Qualifizierern](namespaces.md#namespace-alias-qualifiers)definiert.</span><span class="sxs-lookup"><span data-stu-id="20699-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="20699-945">Ein *member_access* hat entweder das Format `E.I` oder die Form `E.I<A1, ..., Ak>`, wobei "`E`" ein primärer Ausdruck ist, `I` ein einzelner Bezeichner und `<A1, ..., Ak>` ein optionales *type_argument_list*ist.</span><span class="sxs-lookup"><span data-stu-id="20699-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="20699-946">Wenn *type_argument_list* nicht angegeben ist, sollten Sie `K` NULL sein.</span><span class="sxs-lookup"><span data-stu-id="20699-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="20699-947">Ein *member_access* mit einem *primary_expression* vom Typ `dynamic` ist dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="20699-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="20699-948">In diesem Fall klassifiziert der Compiler den Member Access als Eigenschaften Zugriff vom Typ `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="20699-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="20699-949">Die nachstehenden Regeln zum Bestimmen der Bedeutung des *member_access* werden dann zur Laufzeit angewendet und verwenden den Lauf Zeittyp anstelle des Kompilier Zeittyps der *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="20699-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="20699-950">Wenn diese Lauf Zeit Klassifizierung zu einer Methoden Gruppe führt, muss der Member Access *primary_expression* of a *invocation_expression*sein.</span><span class="sxs-lookup"><span data-stu-id="20699-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="20699-951">Der *member_access* wird wie folgt ausgewertet und klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="20699-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="20699-952">Wenn `K` 0 (null) und `E` ein Namespace und `E` einen schsted Namespace mit dem Namen @ no__t-3 enthält, ist das Ergebnis der Namespace.</span><span class="sxs-lookup"><span data-stu-id="20699-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="20699-953">Wenn `E` ein Namespace ist und `E` einen zugänglichen Typ mit dem Namen @ no__t-2 und `K` @ no__t-4type-Parameter enthält, ist das Ergebnis der Typ, der mit den angegebenen Typargumenten erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="20699-954">Wenn `E` ein *predefined_type* oder ein *primary_expression* ist, das als Typ klassifiziert ist, wenn `E` kein Typparameter ist, und wenn eine Member-Suche ([Member Suche](expressions.md#member-lookup)) von `I` in `E` mit `K` @ no__t-8type-Parametern eine Entsprechung erzeugt, Anschließend wird `E.I` wie folgt ausgewertet und klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="20699-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="20699-955">Wenn `I` einen Typ identifiziert, ist das Ergebnis der Typ, der mit den angegebenen Typargumenten erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="20699-956">Wenn `I` eine oder mehrere Methoden identifiziert, ist das Ergebnis eine Methoden Gruppe ohne zugeordneten Instanzausdruck.</span><span class="sxs-lookup"><span data-stu-id="20699-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="20699-957">Wenn eine Typargument Liste angegeben wurde, wird Sie beim Aufrufen einer generischen Methode ([Methodenaufrufe](expressions.md#method-invocations)) verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="20699-958">Wenn `I` eine Eigenschaft `static` identifiziert, ist das Ergebnis ein Eigenschaften Zugriff ohne zugeordneten Instanzausdruck.</span><span class="sxs-lookup"><span data-stu-id="20699-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="20699-959">Wenn `I` ein `static`-Feld angibt:</span><span class="sxs-lookup"><span data-stu-id="20699-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="20699-960">Wenn das Feld `readonly` ist und der Verweis außerhalb des statischen Konstruktors der Klasse oder Struktur auftritt, in der das Feld deklariert ist, dann ist das Ergebnis ein Wert, nämlich der Wert des statischen Felds @ no__t-1 in @ no__t-2.</span><span class="sxs-lookup"><span data-stu-id="20699-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="20699-961">Andernfalls ist das Ergebnis eine Variable, nämlich das statische Feld @ no__t-0 in @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="20699-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="20699-962">Wenn `I` ein `static`-Ereignis kennzeichnet:</span><span class="sxs-lookup"><span data-stu-id="20699-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="20699-963">Wenn der Verweis innerhalb der Klasse oder Struktur auftritt, in der das Ereignis deklariert ist, und das Ereignis ohne *event_accessor_declarations* ([Ereignisse](classes.md#events)) deklariert wurde, wird `E.I` genau so verarbeitet, als ob es sich bei `I` um ein statisches Feld handelt.</span><span class="sxs-lookup"><span data-stu-id="20699-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="20699-964">Andernfalls ist das Ergebnis ein Ereignis Zugriff ohne zugeordneten Instanzausdruck.</span><span class="sxs-lookup"><span data-stu-id="20699-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="20699-965">Wenn `I` eine Konstante identifiziert, ist das Ergebnis ein Wert, nämlich der Wert dieser Konstante.</span><span class="sxs-lookup"><span data-stu-id="20699-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="20699-966">Wenn `I` einen Enumerationsmember identifiziert, ist das Ergebnis ein Wert, nämlich der Wert dieses Enumerationsmembers.</span><span class="sxs-lookup"><span data-stu-id="20699-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="20699-967">Andernfalls ist `E.I` ein ungültiger Element Verweis, und es tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="20699-968">Wenn `E` ein Eigenschaften Zugriff, Indexer-Zugriff, Variable oder Wert ist, der Typ, der @ no__t-1 ist, und eine Member-Suche ([Member-Suche](expressions.md#member-lookup)) von `I` in `T` mit `K` @ no__t-6typargumenten eine Entsprechung erzeugt, dann wird `E.I` ausgewertet und wie folgt klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="20699-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="20699-969">Wenn `E` eine Eigenschaft oder ein Indexer-Zugriff ist, wird zuerst der Wert der Eigenschaft oder des Indexerzugriffs abgerufen ([Werte von Ausdrücken](expressions.md#values-of-expressions)), und `E` wird als Wert neu klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="20699-970">Wenn `I` eine oder mehrere Methoden identifiziert, ist das Ergebnis eine Methoden Gruppe mit einem zugeordneten Instanzausdruck `E`.</span><span class="sxs-lookup"><span data-stu-id="20699-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="20699-971">Wenn eine Typargument Liste angegeben wurde, wird Sie beim Aufrufen einer generischen Methode ([Methodenaufrufe](expressions.md#method-invocations)) verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="20699-972">Wenn `I` eine Instanzeigenschaft identifiziert,</span><span class="sxs-lookup"><span data-stu-id="20699-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="20699-973">Wenn `E` `this` ist, identifiziert `I` eine automatisch implementierte Eigenschaft ([automatisch implementierte Eigenschaften](classes.md#automatically-implemented-properties)) ohne Setter, und der Verweis tritt innerhalb eines Instanzkonstruktors für eine Klasse oder einen Strukturtyp auf `T`, dann das Ergebnis. ist eine Variable, d. h. das verborgene dahinter liegende Feld für die Auto-Eigenschaft, die von `I` in der Instanz von `T` von `this` angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="20699-974">Andernfalls ist das Ergebnis ein Eigenschaften Zugriff mit dem zugeordneten Instanzausdruck @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="20699-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="20699-975">Wenn `T` ein *class_type* ist und `I` ein Instanzfeld dieses *class_type*angibt:</span><span class="sxs-lookup"><span data-stu-id="20699-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="20699-976">Wenn der Wert `E` `null` ist, wird eine `System.NullReferenceException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="20699-977">Andernfalls ist das Ergebnis ein Wert, d. h. der Wert des Felds @ no__t-1 in dem Objekt, auf das von @ no__t-2 verwiesen wird, wenn das Feld `readonly` ist und der Verweis außerhalb eines Instanzkonstruktors der Klasse erfolgt, in der das Feld deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="20699-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="20699-978">Andernfalls ist das Ergebnis eine Variable, nämlich das Feld @ no__t-0 in dem Objekt, auf das von @ no__t-1 verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="20699-979">Wenn `T` ein *struct_type* ist und `I` ein Instanzfeld dieses *struct_type*angibt:</span><span class="sxs-lookup"><span data-stu-id="20699-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="20699-980">Wenn `E` ein Wert ist oder wenn das Feld `readonly` ist und der Verweis außerhalb eines Instanzkonstruktors der Struktur auftritt, in der das Feld deklariert ist, dann ist das Ergebnis ein Wert, d. h. der Wert des Felds @ no__t-2 in der Struktur Instanz, angegeben durch @ no__t-3.</span><span class="sxs-lookup"><span data-stu-id="20699-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="20699-981">Andernfalls ist das Ergebnis eine Variable, nämlich das Feld @ no__t-0 in der Struktur Instanz, angegeben durch @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="20699-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="20699-982">Wenn `I` ein Instanzereignis angibt:</span><span class="sxs-lookup"><span data-stu-id="20699-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="20699-983">, Wenn der Verweis innerhalb der Klasse oder Struktur auftritt, in der das Ereignis deklariert ist, und das Ereignis ohne *event_accessor_declarations* ([Events](classes.md#events)) deklariert wurde und der Verweis nicht als linke Seite eines `+=`-oder `-=`-Operators auftritt. , `E.I` wird genau so verarbeitet, als ob `I` ein Instanzfeld wäre.</span><span class="sxs-lookup"><span data-stu-id="20699-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="20699-984">Andernfalls ist das Ergebnis ein Ereignis Zugriff mit dem zugeordneten Instanzausdruck @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="20699-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="20699-985">Andernfalls wird versucht, `E.I` als Erweiterungs Methodenaufruf ([Erweiterungs Methodenaufrufe](expressions.md#extension-method-invocations)) zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="20699-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="20699-986">Wenn dies nicht möglich ist, ist `E.I` ein ungültiger Element Verweis, und es tritt ein Fehler bei der Bindung auf.</span><span class="sxs-lookup"><span data-stu-id="20699-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="20699-987">Identische einfache Namen und Typnamen</span><span class="sxs-lookup"><span data-stu-id="20699-987">Identical simple names and type names</span></span>

<span data-ttu-id="20699-988">Bei einem Element Zugriff auf das Formular `E.I`, wenn `E` ein einzelner Bezeichner ist und die Bedeutung von `E` als *Simple_name* ([einfache Namen](expressions.md#simple-names)) eine Konstante, ein Feld, eine Eigenschaft, eine lokale Variable oder ein Parameter mit demselben Typ ist wie `E` als *TYPE_NAME* ([Namespace-und Typnamen](basic-concepts.md#namespace-and-type-names)), dann sind beide möglichen Bedeutungen von `E` zulässig.</span><span class="sxs-lookup"><span data-stu-id="20699-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="20699-989">Die beiden möglichen Bedeutungen von `E.I` sind nie mehrdeutig, da `I` in beiden Fällen notwendigerweise ein Member des Typs `E` sein muss.</span><span class="sxs-lookup"><span data-stu-id="20699-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="20699-990">Mit anderen Worten: die Regel gestattet den Zugriff auf die statischen Member und die schsted Typen von `E`, wobei andernfalls ein Kompilierzeitfehler aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="20699-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="20699-991">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="20699-991">For example:</span></span>
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

#### <a name="grammar-ambiguities"></a><span data-ttu-id="20699-992">Grammatik Mehrdeutigkeiten</span><span class="sxs-lookup"><span data-stu-id="20699-992">Grammar ambiguities</span></span>

<span data-ttu-id="20699-993">Die-Produktionen für *Simple_name* ([simple names](expressions.md#simple-names)) und *member_access* ([Member Access](expressions.md#member-access)) können Mehrdeutigkeiten in der Grammatik für Ausdrücke auslösen.</span><span class="sxs-lookup"><span data-stu-id="20699-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="20699-994">Beispielsweise ist die-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="20699-994">For example, the statement:</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="20699-995">kann als `F`-Aufrufwert mit zwei Argumenten interpretiert werden, `G < A` und `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="20699-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="20699-996">Alternativ könnte Sie auch als `F`-aufgerufenen mit einem Argument interpretiert werden, bei dem es sich um einen Aufrufe einer generischen Methode @ no__t-1 mit zwei Typargumenten und einem regulären Argument handelt.</span><span class="sxs-lookup"><span data-stu-id="20699-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="20699-997">Wenn eine Sequenz von Token (im Kontext) als *Simple_name* ([simple names](expressions.md#simple-names)), *member_access* ([Member Access](expressions.md#member-access)) oder *pointer_member_access* ([Pointer Member Access](unsafe-code.md#pointer-member-access)) analysiert werden kann, die mit einem type_argument_ endet  *List* ([Typargumente](types.md#type-arguments)), das Token, das unmittelbar auf das schließende `>`-Token folgt, wird überprüft.</span><span class="sxs-lookup"><span data-stu-id="20699-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="20699-998">Wenn eine von</span><span class="sxs-lookup"><span data-stu-id="20699-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="20699-999">Anschließend wird *type_argument_list* als Teil der *Simple_name*, *member_access* oder *pointer_member_access* beibehalten, und jede andere mögliche Analyse der Sequenz von Token wird verworfen.</span><span class="sxs-lookup"><span data-stu-id="20699-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="20699-1000">Andernfalls wird *type_argument_list* nicht als Teil von *Simple_name*, *member_access* oder *pointer_member_access*angesehen, auch wenn es keine andere Möglichkeit gibt, die Sequenz von Token zu analysieren.</span><span class="sxs-lookup"><span data-stu-id="20699-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="20699-1001">Beachten Sie, dass diese Regeln nicht angewendet werden, wenn ein *type_argument_list* -Element in einem *namespace_or_type_name* -Element ([Namespace-und Typnamen](basic-concepts.md#namespace-and-type-names)) verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="20699-1002">Die Anweisung</span><span class="sxs-lookup"><span data-stu-id="20699-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="20699-1003">wird gemäß dieser Regel als ein aufrufungs `F` mit einem Argument interpretiert, bei dem es sich um einen Aufrufe einer generischen Methode `G` mit zwei Typargumenten und einem regulären Argument handelt.</span><span class="sxs-lookup"><span data-stu-id="20699-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="20699-1004">Die Anweisungen</span><span class="sxs-lookup"><span data-stu-id="20699-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="20699-1005">Jeder wird als `F`-Aufrufwert mit zwei Argumenten interpretiert.</span><span class="sxs-lookup"><span data-stu-id="20699-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="20699-1006">Die Anweisung</span><span class="sxs-lookup"><span data-stu-id="20699-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="20699-1007">wird als Operator kleiner als, größer als und Unärer Plus-Operator interpretiert, als ob die Anweisung `x = (F < A) > (+y)` geschrieben worden wäre, anstatt als *Simple_name* mit einem *type_argument_list* gefolgt von einem binären Plus-Operator.</span><span class="sxs-lookup"><span data-stu-id="20699-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="20699-1008">In der-Anweisung</span><span class="sxs-lookup"><span data-stu-id="20699-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="20699-1009">die Token `C<T>` werden als *namespace_or_type_name* mit einem *type_argument_list*interpretiert.</span><span class="sxs-lookup"><span data-stu-id="20699-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="20699-1010">Aufrufausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-1010">Invocation expressions</span></span>

<span data-ttu-id="20699-1011">Ein *invocation_expression* wird verwendet, um eine Methode aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="20699-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="20699-1012">Ein *invocation_expression* -Wert ist dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)), wenn mindestens einer der folgenden Punkte enthält:</span><span class="sxs-lookup"><span data-stu-id="20699-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="20699-1013">*Primary_expression* weist den Typ der Kompilierzeit `dynamic` auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="20699-1014">Mindestens ein Argument des optionalen *argument_list* weist den Kompilier Zeittyp `dynamic` und *primary_expression* keinen Delegattyp auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="20699-1015">In diesem Fall klassifiziert der Compiler den *invocation_expression* -Wert als Wert des Typs `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="20699-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="20699-1016">Die unten aufgeführten Regeln zum Bestimmen der Bedeutung des *invocation_expression* werden dann zur Laufzeit angewendet. dabei wird der Lauf Zeittyp anstelle des Kompilierzeit Typs der *primary_expression* -und-Argumente verwendet, die den Typ der Kompilierzeit aufweisen @no_ _T-2.</span><span class="sxs-lookup"><span data-stu-id="20699-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="20699-1017">Wenn *primary_expression* keinen Kompilier Zeittyp `dynamic` hat, wird der Methodenaufruf einer begrenzten Kompilierungszeit Überprüfung unterzogen, wie in der [Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="20699-1018">Der *primary_expression* eines *invocation_expression* muss eine Methoden Gruppe oder ein *delegate_type*-Wert sein.</span><span class="sxs-lookup"><span data-stu-id="20699-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="20699-1019">Wenn *primary_expression* eine Methoden Gruppe ist, handelt es sich bei *invocation_expression* um einen Methodenaufruf ([Methodenaufrufe](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="20699-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="20699-1020">Wenn *primary_expression* ein Wert von *delegate_type*ist, ist *invocation_expression* ein Delegataufruf ([Delegataufrufe](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="20699-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="20699-1021">Wenn *primary_expression* weder eine Methoden Gruppe noch ein Wert eines *delegate_type*ist, tritt ein Fehler bei der Bindung auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="20699-1022">Die optionalen *argument_list* ([Argumentlisten](expressions.md#argument-lists)) stellen Werte oder Variablen Verweise für die Parameter der Methode bereit.</span><span class="sxs-lookup"><span data-stu-id="20699-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="20699-1023">Das Ergebnis der Auswertung eines *invocation_expression* wird wie folgt klassifiziert:</span><span class="sxs-lookup"><span data-stu-id="20699-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="20699-1024">Wenn *invocation_expression* eine Methode oder einen Delegaten aufruft, die `void` zurückgibt, ist das Ergebnis "Nothing".</span><span class="sxs-lookup"><span data-stu-id="20699-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="20699-1025">Ein Ausdruck, der als "Nothing" klassifiziert wird, ist nur im Kontext eines *statement_expression* ([Ausdrucks Anweisungen](statements.md#expression-statements)) oder als Text eines *lambda_expression* ([Anonyme Funktions Ausdrücke](expressions.md#anonymous-function-expressions)) zulässig.</span><span class="sxs-lookup"><span data-stu-id="20699-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="20699-1026">Andernfalls tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="20699-1027">Andernfalls ist das Ergebnis ein Wert des Typs, der von der-Methode oder dem-Delegaten zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="20699-1028">Methodenaufrufe</span><span class="sxs-lookup"><span data-stu-id="20699-1028">Method invocations</span></span>

<span data-ttu-id="20699-1029">Bei einem Methodenaufruf muss der *primary_expression* des *invocation_expression* eine Methoden Gruppe sein.</span><span class="sxs-lookup"><span data-stu-id="20699-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="20699-1030">Die Methoden Gruppe identifiziert die eine Methode, die aufgerufen werden soll, oder den Satz überladener Methoden, von denen eine bestimmte aufzurufende Methode ausgewählt werden soll.</span><span class="sxs-lookup"><span data-stu-id="20699-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="20699-1031">Im letzteren Fall basiert die Bestimmung der aufzurufenden Methode auf dem Kontext, der von den Typen der Argumente in der *argument_list*bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="20699-1032">Die Bindungs Zeit Verarbeitung eines Methoden aufruens in der Form `M(A)`, wobei `M` eine Methoden Gruppe (möglicherweise auch ein *type_argument_list*) und `A` ein optionales *argument_list*ist, besteht aus den folgenden Schritten:</span><span class="sxs-lookup"><span data-stu-id="20699-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="20699-1033">Der Satz von Kandidaten Methoden für den Methodenaufruf wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="20699-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="20699-1034">Für jede Methode `F`, die der Methoden Gruppe zugeordnet ist `M`:</span><span class="sxs-lookup"><span data-stu-id="20699-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="20699-1035">Wenn `F` nicht generisch ist, ist `F` in folgenden den folgenden Kandidaten:</span><span class="sxs-lookup"><span data-stu-id="20699-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="20699-1036">`M` hat keine Typargument Liste, und</span><span class="sxs-lookup"><span data-stu-id="20699-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="20699-1037">`F` ist in Bezug auf `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) anwendbar.</span><span class="sxs-lookup"><span data-stu-id="20699-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="20699-1038">Wenn `F` generisch und `M` keine Typargument Liste aufweist, ist `F` in folgenden den folgenden Kandidaten:</span><span class="sxs-lookup"><span data-stu-id="20699-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="20699-1039">Der Typrückschluss ([Typrückschluss](expressions.md#type-inference)) ist erfolgreich, wobei eine Liste der Typargumente für den-Befehl abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="20699-1040">Nachdem die abhergenten Typargumente die entsprechenden Methodentypparameter ersetzt haben, erfüllen alle konstruierten Typen in der Parameterliste von F ihre Einschränkungen (die die[Einschränkungen erfüllen](types.md#satisfying-constraints)), und die Parameterliste von `F` gilt für Beachten Sie `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="20699-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="20699-1041">Wenn `F` generisch und `M` eine Typargument Liste enthält, ist `F` in folgenden den folgenden Kandidaten:</span><span class="sxs-lookup"><span data-stu-id="20699-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="20699-1042">`F` verfügt über die gleiche Anzahl von Methodentypparametern, die in der Typargument Liste angegeben wurden, und</span><span class="sxs-lookup"><span data-stu-id="20699-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="20699-1043">Nachdem die Typargumente die entsprechenden Methodentypparameter ersetzt haben, erfüllen alle konstruierten Typen in der Parameterliste von F ihre Einschränkungen (die die[Einschränkungen erfüllen](types.md#satisfying-constraints)), und die Parameterliste von `F` gilt in Bezug auf `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="20699-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="20699-1044">Der Satz von Kandidaten Methoden wird so reduziert, dass nur die Methoden der am weitesten abgeleiteten Typen enthalten sind: Für jede Methode `C.F` im Satz, wobei `C` der Typ ist, in dem die Methode `F` deklariert ist, werden alle Methoden, die in einem Basistyp von `C` deklariert sind, aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="20699-1045">Wenn `C` ein anderer Klassentyp als `object` ist, werden alle in einem Schnittstellentyp deklarierten Methoden aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="20699-1046">(Diese letztere Regel hat nur Auswirkungen, wenn die Methoden Gruppe das Ergebnis einer Member-Suche nach einem Typparameter ist, der eine effektive Basisklasse als Object und eine nicht leere effektive Schnittstellen Menge aufweist.)</span><span class="sxs-lookup"><span data-stu-id="20699-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="20699-1047">Wenn der resultierende Satz von Kandidaten Methoden leer ist, wird die weitere Verarbeitung der folgenden Schritte abgebrochen, und stattdessen wird versucht, den Aufruf als Erweiterungs Methodenaufruf ([Erweiterungs Methodenaufrufe](expressions.md#extension-method-invocations)) zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="20699-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="20699-1048">Wenn dies fehlschlägt, gibt es keine anwendbaren Methoden, und ein Bindungs Fehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="20699-1049">Die beste Methode des Satzes von Kandidaten Methoden wird mithilfe der Regeln zur Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution)identifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="20699-1050">Wenn eine einzige optimale Methode nicht identifiziert werden kann, ist der Methodenaufruf mehrdeutig, und es tritt ein Fehler bei der Bindung auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="20699-1051">Beim Durchführen der Überladungs Auflösung werden die Parameter einer generischen Methode berücksichtigt, nachdem die Typargumente (bereitgestellt oder abgeleitet) für die entsprechenden Methodentyp Parameter ersetzt wurden.</span><span class="sxs-lookup"><span data-stu-id="20699-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="20699-1052">Die endgültige Überprüfung der ausgewählten optimalen Methode wird ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="20699-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="20699-1053">Die-Methode wird im Kontext der-Methoden Gruppe überprüft: Wenn es sich bei der besten Methode um eine statische Methode handelt, muss die Methoden Gruppe durch einen-Typ aus einem *Simple_name* -oder einem *member_access* -Wert resultieren.</span><span class="sxs-lookup"><span data-stu-id="20699-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="20699-1054">Wenn die beste Methode eine Instanzmethode ist, muss die Methoden Gruppe aus einem *Simple_name*-, einem *member_access* -Wert durch eine Variable oder einem Wert oder einem *base_access*resultieren.</span><span class="sxs-lookup"><span data-stu-id="20699-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="20699-1055">Wenn keine dieser Anforderungen erfüllt ist, tritt ein Fehler bei der Bindung auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="20699-1056">Wenn es sich bei der besten Methode um eine generische Methode handelt, werden die Typargumente (bereitgestellt oder abgeleitet) mit den Einschränkungen (die sich durch die[Erfüllung der Einschränkungen](types.md#satisfying-constraints)in der generischen Methode) überprüfen</span><span class="sxs-lookup"><span data-stu-id="20699-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="20699-1057">Wenn ein Typargument die entsprechenden Einschränkungen für den Typparameter nicht erfüllt, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="20699-1058">Nachdem eine Methode mithilfe der obigen Schritte zur Bindungs Zeit ausgewählt und überprüft wurde, wird der tatsächliche Lauf Zeit Aufruf gemäß den Regeln für den Funktionselement Aufruf verarbeitet, der unter [Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)beschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="20699-1059">Die intuitiver Auswirkung der oben beschriebenen Auflösungs Regeln lautet wie folgt: Wenn Sie die von einem Methodenaufruf aufgerufene Methode suchen möchten, beginnen Sie mit dem Typ, der durch den Methodenaufruf angegeben wird, und fahren Sie die Vererbungs Kette fort, bis mindestens eine anwendbare, barrierefreie, nicht über schreibende Methoden Deklaration gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="20699-1060">Führen Sie dann den Typrückschluss und die Überladungs Auflösung für den Satz der anwendbaren, zugänglichen, nicht überschreibenden Methoden aus, die in diesem Typ deklariert sind, und rufen Sie die Methode auf</span><span class="sxs-lookup"><span data-stu-id="20699-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="20699-1061">Wenn keine Methode gefunden wurde, versuchen Sie stattdessen, den Aufruf als Erweiterungs Methodenaufruf zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="20699-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="20699-1062">Erweiterungs Methodenaufrufe</span><span class="sxs-lookup"><span data-stu-id="20699-1062">Extension method invocations</span></span>

<span data-ttu-id="20699-1063">In einem Methodenaufruf ([Aufrufe für geboxte Instanzen](expressions.md#invocations-on-boxed-instances)) eines der Formulare</span><span class="sxs-lookup"><span data-stu-id="20699-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="20699-1064">Wenn bei der normalen Verarbeitung des aufzurufenden-aufzurufenden keine anwendbaren Methoden gefunden werden, wird versucht, das Konstrukt als Erweiterungs Methodenaufruf zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="20699-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="20699-1065">Wenn *expr* oder eines der *args* den Kompilier Zeittyp `dynamic` aufweist, werden keine Erweiterungs Methoden angewendet.</span><span class="sxs-lookup"><span data-stu-id="20699-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="20699-1066">Ziel ist es, das beste *TYPE_NAME* -`C` zu finden, damit der entsprechende Aufruf der statischen Methode stattfinden kann:</span><span class="sxs-lookup"><span data-stu-id="20699-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="20699-1067">Eine Erweiterungsmethode `Ci.Mj` ist ***berechtigt*** , wenn Folgendes gilt:</span><span class="sxs-lookup"><span data-stu-id="20699-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="20699-1068">"`Ci`" ist eine nicht generische, nicht-nicht-Klassen-Klasse.</span><span class="sxs-lookup"><span data-stu-id="20699-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="20699-1069">Der Name `Mj` ist ein *Bezeichner* .</span><span class="sxs-lookup"><span data-stu-id="20699-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="20699-1070">`Mj` ist verfügbar und anwendbar, wenn Sie auf die Argumente als statische Methode angewendet wird, wie oben gezeigt.</span><span class="sxs-lookup"><span data-stu-id="20699-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="20699-1071">Eine implizite Identitäts-, Verweis-oder Boxingkonvertierung ist von *expr* zum Typ des ersten Parameters von `Mj` vorhanden.</span><span class="sxs-lookup"><span data-stu-id="20699-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="20699-1072">Die Suche nach `C` verläuft wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="20699-1073">Beginnend mit der nächstgelegenen, einschließenden Namespace Deklaration, der Fortsetzung der einschließenden Namespace Deklaration und dem Ende der enthaltenden Kompilierungseinheit werden nachfolgende Versuche unternommen, um einen Kandidaten Satz von Erweiterungs Methoden zu finden:</span><span class="sxs-lookup"><span data-stu-id="20699-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="20699-1074">Wenn der angegebene Namespace oder die Kompilierungseinheit direkt nicht generische Typdeklarationen `Ci` mit berechtigten Erweiterungs Methoden `Mj` enthält, ist der Satz dieser Erweiterungs Methoden der Kandidaten Satz.</span><span class="sxs-lookup"><span data-stu-id="20699-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="20699-1075">Wenn die Typen `Ci` von *using_static_declarations* importiert und direkt in Namespaces deklariert werden, die von *using_namespace_directive*s in der angegebenen Namespace-oder Kompilierungseinheit importiert wurden, enthalten `Mj` direkt geeignete Erweiterungs Methoden. der Satz dieser Erweiterungs Methoden ist der Kandidaten Satz.</span><span class="sxs-lookup"><span data-stu-id="20699-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="20699-1076">Wenn kein Kandidaten Satz in einer einschließenden Namespace Deklaration oder Kompilierungseinheit gefunden wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="20699-1077">Andernfalls wird die Überladungs Auflösung auf den Kandidaten Satz angewendet, wie in ([Überladungs Auflösung](expressions.md#overload-resolution)) beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="20699-1078">Wenn keine einzige optimale Methode gefunden wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="20699-1079">`C` ist der Typ, in dem die beste Methode als Erweiterungsmethode deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="20699-1080">Wenn `C` als Ziel verwendet wird, wird der Methodenaufruf als statischer Methodenaufruf verarbeitet ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="20699-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="20699-1081">Die vorstehenden Regeln bedeuten, dass Instanzmethoden Vorrang vor Erweiterungs Methoden haben, dass die in inneren Namespace Deklarationen verfügbaren Erweiterungs Methoden Vorrang vor Erweiterungs Methoden haben, die in äußeren Namespace Deklarationen verfügbar sind, und diese Erweiterung Methoden, die direkt in einem Namespace deklariert werden, haben Vorrang vor Erweiterungs Methoden, die in den gleichen Namespace mit einer using-namespace Direktive importiert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="20699-1082">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="20699-1082">For example:</span></span>
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

<span data-ttu-id="20699-1083">Im Beispiel hat die-Methode von `B` Vorrang vor der ersten Erweiterungsmethode, und `C`-Methode hat Vorrang vor beiden Erweiterungs Methoden.</span><span class="sxs-lookup"><span data-stu-id="20699-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

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

<span data-ttu-id="20699-1084">Die Ausgabe dieses Beispiels lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-1084">The output of this example is:</span></span>
```console
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="20699-1085">`D.G` hat Vorrang vor `C.G`, und `E.F` hat Vorrang vor `D.F` und `C.F`.</span><span class="sxs-lookup"><span data-stu-id="20699-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="20699-1086">Delegataufrufe</span><span class="sxs-lookup"><span data-stu-id="20699-1086">Delegate invocations</span></span>

<span data-ttu-id="20699-1087">Bei einem Delegataufruf muss der *primary_expression* des *invocation_expression* ein Wert von *delegate_type*sein.</span><span class="sxs-lookup"><span data-stu-id="20699-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="20699-1088">Wenn Sie den *delegate_type* in Erwägung ziehen, ein Funktionsmember mit derselben Parameterliste wie das *delegate_type*-Element zu sein, muss die *delegate_type* -Anwendung ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) in Bezug auf den *argument_ die Liste* der *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="20699-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="20699-1089">Die Lauf Zeit Verarbeitung eines delegataufruens in der Form `D(A)`, wobei `D` ein *primary_expression* eines *delegate_type* und `A` ein optionaler *argument_list*ist, besteht aus den folgenden Schritten:</span><span class="sxs-lookup"><span data-stu-id="20699-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="20699-1090">`D` wird ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-1090">`D` is evaluated.</span></span> <span data-ttu-id="20699-1091">Wenn diese Auswertung eine Ausnahme verursacht, werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="20699-1092">Der Wert von "`D`" wird als gültig geprüft.</span><span class="sxs-lookup"><span data-stu-id="20699-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="20699-1093">Wenn der Wert `D` `null` ist, wird ein `System.NullReferenceException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="20699-1094">Andernfalls ist `D` ein Verweis auf eine Delegatinstanz.</span><span class="sxs-lookup"><span data-stu-id="20699-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="20699-1095">Funktionsmember-Aufrufe ([Kompilierungszeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) werden für jede der Aufruf baren Entitäten in der Aufruf Liste des Delegaten ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="20699-1096">Bei Aufruf baren Entitäten, die aus einer Instanz-und Instanzmethode bestehen, ist die Instanz für den Aufruf die Instanz, die in der Aufruf baren Entität enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="20699-1097">Element Zugriff</span><span class="sxs-lookup"><span data-stu-id="20699-1097">Element access</span></span>

<span data-ttu-id="20699-1098">Ein *element_access* besteht aus einem *primary_no_array_creation_expression*, gefolgt von einem "`[`"-Token, gefolgt von einem *argument_list*, gefolgt von einem "`]`"-Token.</span><span class="sxs-lookup"><span data-stu-id="20699-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="20699-1099">Der *argument_list* besteht aus einem oder mehreren *Argumenten*, getrennt durch Kommas.</span><span class="sxs-lookup"><span data-stu-id="20699-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="20699-1100">Der *argument_list* eines *element_access* darf keine `ref`-oder `out`-Argumente enthalten.</span><span class="sxs-lookup"><span data-stu-id="20699-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="20699-1101">Ein *element_access* -Wert ist dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)), wenn mindestens einer der folgenden Punkte enthält:</span><span class="sxs-lookup"><span data-stu-id="20699-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="20699-1102">*Primary_no_array_creation_expression* weist den Typ der Kompilierzeit `dynamic` auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="20699-1103">Mindestens ein Ausdruck der *argument_list* weist den Kompilier Zeittyp `dynamic` und *primary_no_array_creation_expression* keinen Arraytyp auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="20699-1104">In diesem Fall klassifiziert der Compiler den *element_access* -Wert als Wert des Typs `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="20699-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="20699-1105">Die unten aufgeführten Regeln zum Bestimmen der Bedeutung des *element_access* werden dann zur Laufzeit angewendet. dabei wird der Lauf Zeittyp anstelle des Kompilierzeit Typs der *primary_no_array_creation_expression* und *argument_list* verwendet. Ausdrücke, die den Kompilier Zeittyp `dynamic` aufweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="20699-1106">Wenn *primary_no_array_creation_expression* keinen Kompilier Zeittyp `dynamic` hat, wird der Zugriff auf die Kompilierzeit durch den Element Zugriff eingeschränkt, wie in der [Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="20699-1107">Wenn der *primary_no_array_creation_expression* -Wert eines *element_access* -Werts ein *array_type*-Wert ist, ist *element_access* ein Array Zugriff ([Array Zugriff](expressions.md#array-access)).</span><span class="sxs-lookup"><span data-stu-id="20699-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="20699-1108">Andernfalls muss *primary_no_array_creation_expression* eine Variable oder ein Wert einer Klasse, Struktur oder Schnittstelle sein, die über mindestens ein Indexer-Member verfügt. in diesem Fall ist der *element_access* ein Indexer-Zugriff ([Indexer-Zugriff](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="20699-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="20699-1109">Arrayzugriff</span><span class="sxs-lookup"><span data-stu-id="20699-1109">Array access</span></span>

<span data-ttu-id="20699-1110">Bei einem Array Zugriff muss der *primary_no_array_creation_expression* des *element_access* -Werts ein *array_type*sein.</span><span class="sxs-lookup"><span data-stu-id="20699-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="20699-1111">Außerdem darf die *argument_list* eines Array Zugriffs keine benannten Argumente enthalten. Die Anzahl der Ausdrücke in *argument_list* muss mit dem Rang des *array_type*identisch sein, und jeder Ausdruck muss vom Typ `int`, `uint`, `long`, `ulong` oder implizit in einen oder mehrere dieser Typen konvertiert werden können.</span><span class="sxs-lookup"><span data-stu-id="20699-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="20699-1112">Das Ergebnis der Auswertung eines Array Zugriffs ist eine Variable des Elementtyps des Arrays, d. h. das Array Element, das von den Werten der Ausdrücke in der *argument_list*ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="20699-1113">Die Lauf Zeit Verarbeitung eines Array Zugriffs auf das Formular `P[A]`, wobei `P` ein *primary_no_array_creation_expression* eines *array_type* und `A` eine *argument_list*ist, besteht aus den folgenden Schritten:</span><span class="sxs-lookup"><span data-stu-id="20699-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="20699-1114">`P` wird ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-1114">`P` is evaluated.</span></span> <span data-ttu-id="20699-1115">Wenn diese Auswertung eine Ausnahme verursacht, werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="20699-1116">Die Index Ausdrücke der *argument_list* werden in der Reihenfolge von links nach rechts ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="20699-1117">Nach der Auswertung der einzelnen Index Ausdrücke wird eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) in einen der folgenden Typen ausgeführt: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="20699-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="20699-1118">Der erste Typ in dieser Liste, für den eine implizite Konvertierung vorhanden ist, wird ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="20699-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="20699-1119">Wenn der Index Ausdruck beispielsweise vom Typ `short` ist, wird eine implizite Konvertierung in `int` ausgeführt, da implizite Konvertierungen von `short` in `int` und von `short` in `long` möglich sind.</span><span class="sxs-lookup"><span data-stu-id="20699-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="20699-1120">Wenn die Auswertung eines Index Ausdrucks oder der nachfolgenden impliziten Konvertierung eine Ausnahme verursacht, werden keine weiteren Index Ausdrücke ausgewertet, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="20699-1121">Der Wert von "`P`" wird als gültig geprüft.</span><span class="sxs-lookup"><span data-stu-id="20699-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="20699-1122">Wenn der Wert `P` `null` ist, wird ein `System.NullReferenceException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="20699-1123">Der Wert jedes Ausdrucks in *argument_list* wird anhand der tatsächlichen Begrenzungen der einzelnen Dimensionen der Array Instanz überprüft, auf die von `P` verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="20699-1124">Wenn mindestens ein Wert außerhalb des gültigen Bereichs liegt, wird eine @no__t 0 (null) ausgelöst, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="20699-1125">Der Speicherort des Array Elements, das durch den Index Ausdruck (n) angegeben wird, wird berechnet, und dieser Speicherort wird zum Ergebnis des Array Zugriffs.</span><span class="sxs-lookup"><span data-stu-id="20699-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="20699-1126">Indexerzugriff</span><span class="sxs-lookup"><span data-stu-id="20699-1126">Indexer access</span></span>

<span data-ttu-id="20699-1127">Bei einem Indexerzugriff muss *primary_no_array_creation_expression* des *element_access* eine Variable oder ein Wert einer Klasse, Struktur oder eines Schnittstellen Typs sein, und dieser Typ muss mindestens einen Indexer implementieren, der in Bezug auf das *argument_list* des *element_access*.</span><span class="sxs-lookup"><span data-stu-id="20699-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="20699-1128">Die Bindungs Zeit Verarbeitung eines Indexer-Zugriffs auf das Formular `P[A]`, wobei `P` ein *primary_no_array_creation_expression* eines Klassen-, Struktur-oder Schnittstellen Typs `T` und `A` ein *argument_list*ist, besteht aus folgendem: nehmen</span><span class="sxs-lookup"><span data-stu-id="20699-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="20699-1129">Der von `T` bereitgestellte Indexer wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="20699-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="20699-1130">Der Satz besteht aus allen indexermembern, die in `T` oder einem Basistyp `T` deklariert werden, die keine `override`-Deklarationen sind und auf die im aktuellen Kontext zugegriffen werden kann ([Member Access](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="20699-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="20699-1131">Der Satz wird auf die Indexer reduziert, die anwendbar sind und von anderen Indexer nicht ausgeblendet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="20699-1132">Die folgenden Regeln werden auf jeden Indexer `S.I` im Satz angewendet, wobei `S` der Typ ist, in dem der Indexer `I` deklariert ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="20699-1133">Wenn `I` in Bezug auf `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) nicht anwendbar ist, wird `I` aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="20699-1134">Wenn `I` in Bezug auf `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) anwendbar ist, werden alle in einem Basistyp von `S` deklarierten Indexer aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="20699-1135">Wenn `I` in Bezug auf `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) anwendbar ist und `S` ein anderer Klassentyp als `object` ist, werden alle Indexer, die in einer Schnittstelle deklariert sind, aus dem Satz entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="20699-1136">Wenn der resultierende Satz von Kandidaten Indexer leer ist, sind keine anwendbaren Indexer vorhanden, und es tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="20699-1137">Der beste Indexer der Gruppe von Kandidaten Indexern wird mithilfe der Regeln zur Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution)identifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="20699-1138">Wenn ein einzelner optimaler Indexer nicht identifiziert werden kann, ist der Indexerzugriff mehrdeutig, und es tritt ein Fehler bei der Bindung auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="20699-1139">Die Index Ausdrücke der *argument_list* werden in der Reihenfolge von links nach rechts ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="20699-1140">Das Ergebnis der Verarbeitung des Indexer-Zugriffs ist ein Ausdruck, der als Indexer-Zugriff klassifiziert ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="20699-1141">Der indexerzugriffsausdruck verweist auf den Indexer, der im obigen Schritt festgelegt wurde, und verfügt über einen zugeordneten Instanzausdruck `P` und eine zugeordnete Argumentliste `A`.</span><span class="sxs-lookup"><span data-stu-id="20699-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="20699-1142">Abhängig vom Kontext, in dem Sie verwendet werden, bewirkt ein Indexer-Zugriff, dass entweder der *Get-Accessor* oder der *Set-Accessor* des Indexers aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="20699-1143">Wenn der Indexer-Zugriff das Ziel einer Zuweisung ist, wird der *Set-Accessor* aufgerufen, um einen neuen Wert zuzuweisen ([einfache Zuweisung](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="20699-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="20699-1144">In allen anderen Fällen wird der *Get-Accessor* aufgerufen, um den aktuellen Wert zu erhalten ([Werte von Ausdrücken](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="20699-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="20699-1145">Dieser Zugriff</span><span class="sxs-lookup"><span data-stu-id="20699-1145">This access</span></span>

<span data-ttu-id="20699-1146">Ein *this_access* besteht aus dem reservierten Wort `this`.</span><span class="sxs-lookup"><span data-stu-id="20699-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="20699-1147">Ein *this_access* ist nur im- *Block* eines Instanzkonstruktors, einer Instanzmethode oder eines Instanzaccessors zulässig.</span><span class="sxs-lookup"><span data-stu-id="20699-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="20699-1148">Dies hat eine der folgenden Bedeutungen:</span><span class="sxs-lookup"><span data-stu-id="20699-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="20699-1149">Wenn `this` in einem *primary_expression* innerhalb eines Instanzkonstruktors einer Klasse verwendet wird, wird es als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="20699-1150">Der Typ des Werts ist der Instanztyp ([der Instanztyp](classes.md#the-instance-type)) der Klasse, in der die Verwendung erfolgt, und der Wert ist ein Verweis auf das Objekt, das erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="20699-1151">Wenn `this` in einer *primary_expression* in einer Instanzmethode oder einem Instanzaccessor einer Klasse verwendet wird, wird Sie als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="20699-1152">Der Typ des Werts ist der Instanztyp ([der Instanztyp](classes.md#the-instance-type)) der Klasse, in der die Verwendung erfolgt, und der Wert ist ein Verweis auf das Objekt, für das die Methode oder der Accessor aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="20699-1153">Wenn `this` in einem *primary_expression* innerhalb eines Instanzkonstruktors einer Struktur verwendet wird, wird es als Variable klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="20699-1154">Der Typ der Variablen ist der Instanztyp ([der Instanztyp](classes.md#the-instance-type)) der Struktur, in der die Verwendung erfolgt, und die Variable stellt die Struktur dar, die erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="20699-1155">Die Variable "`this`" eines Instanzkonstruktors einer Struktur verhält sich genau wie ein `out`-Parameter des Struktur Typs – insbesondere bedeutet dies, dass die Variable definitiv in jedem Ausführungs Pfad des Instanzkonstruktors zugewiesen werden muss.</span><span class="sxs-lookup"><span data-stu-id="20699-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="20699-1156">Wenn `this` in einer *primary_expression* in einer Instanzmethode oder einem Instanzaccessor einer Struktur verwendet wird, wird Sie als Variable klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="20699-1157">Der Typ der Variablen ist der Instanztyp ([der Instanztyp](classes.md#the-instance-type)) der Struktur, in der die Verwendung erfolgt.</span><span class="sxs-lookup"><span data-stu-id="20699-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="20699-1158">Wenn die Methode oder der Accessor kein Iterator ([Iteratoren](classes.md#iterators)) ist, stellt die `this`-Variable die Struktur dar, für die die Methode oder der Accessor aufgerufen wurde, und verhält sich genau wie ein `ref`-Parameter des Struktur Typs.</span><span class="sxs-lookup"><span data-stu-id="20699-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="20699-1159">Wenn es sich bei der Methode oder dem Accessor um einen Iterator handelt, stellt die `this`-Variable eine Kopie der Struktur dar, für die die Methode oder der Accessor aufgerufen wurde, und verhält sich genau wie ein Wert Parameter des Struktur Typs.</span><span class="sxs-lookup"><span data-stu-id="20699-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="20699-1160">Die Verwendung von `this` in einem *primary_expression* -Wert in einem anderen als dem oben aufgeführten Kontext ist ein Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="20699-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="20699-1161">Insbesondere ist es nicht möglich, in einer statischen Methode, einem statischen Eigenschafts Accessor oder einem *variable_initializer* einer Feld Deklaration auf `this` zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="20699-1162">Basis Zugriff</span><span class="sxs-lookup"><span data-stu-id="20699-1162">Base access</span></span>

<span data-ttu-id="20699-1163">Ein *base_access* besteht aus dem reservierten Wort `base`, gefolgt von einem "`.`"-Token und einem Bezeichner oder einem *argument_list* , der in eckigen Klammern eingeschlossen ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="20699-1164">Ein *base_access* wird verwendet, um auf Basisklassenmember zuzugreifen, die durch ähnlich benannte Member in der aktuellen Klasse oder Struktur ausgeblendet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="20699-1165">Ein *base_access* ist nur im- *Block* eines Instanzkonstruktors, einer Instanzmethode oder eines Instanzaccessors zulässig.</span><span class="sxs-lookup"><span data-stu-id="20699-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="20699-1166">Wenn `base.I` in einer Klasse oder Struktur auftritt, muss `I` einen Member der Basisklasse dieser Klasse oder Struktur bezeichnen.</span><span class="sxs-lookup"><span data-stu-id="20699-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="20699-1167">Ebenso muss ein anwendbarer Indexer in der Basisklasse vorhanden sein, wenn `base[E]` in einer Klasse auftritt.</span><span class="sxs-lookup"><span data-stu-id="20699-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="20699-1168">Zur Bindungs Zeit werden *base_access* -Ausdrücke der Form `base.I` und `base[E]` genau so ausgewertet, als wären Sie `((B)this).I` und `((B)this)[E]` geschrieben worden, wobei `B` die Basisklasse der Klasse oder Struktur ist, in der das Konstrukt auftritt.</span><span class="sxs-lookup"><span data-stu-id="20699-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="20699-1169">Folglich entsprechen `base.I` und `base[E]` `this.I` und `this[E]`, mit dem Unterschied, dass `this` als Instanz der Basisklasse angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="20699-1170">Wenn ein *base_access* auf einen virtuellen Funktionsmember (eine Methode, eine Eigenschaft oder einen Indexer) verweist, wird bestimmt, welcher Funktionsmember zur Laufzeit aufgerufen werden soll ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="20699-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="20699-1171">Der aufgerufene Funktionsmember wird ermittelt, indem die am meisten abgeleitete Implementierung ([virtuelle Methoden](classes.md#virtual-methods)) des Funktionsmembers in Bezug auf `B` ermittelt wird (anstelle des Lauf Zeit Typs `this`, wie es bei einem nicht-Basis Zugriff üblich wäre). .</span><span class="sxs-lookup"><span data-stu-id="20699-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="20699-1172">Daher kann ein *base_access* innerhalb eines `override` eines Funktionsmembers der `virtual` verwendet werden, um die geerbte Implementierung des Funktionsmembers aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="20699-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="20699-1173">Wenn das von einem *base_access* referenzierte Funktionsmember abstrakt ist, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="20699-1174">Postfix-Inkrementoperator und Postfix-Dekrementoperator</span><span class="sxs-lookup"><span data-stu-id="20699-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="20699-1175">Der Operand eines Postfix-Inkrement-oder dekrementvorgangs muss ein Ausdruck sein, der als Variable, Eigenschafts Zugriff oder Indexerzugriff klassifiziert ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="20699-1176">Das Ergebnis des Vorgangs ist ein Wert desselben Typs wie der Operand.</span><span class="sxs-lookup"><span data-stu-id="20699-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="20699-1177">Wenn *primary_expression* den Kompilier Zeittyp `dynamic` aufweist, ist der Operator dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)), *post_increment_expression* oder *post_decrement_expression* hat den Kompilier Zeittyp `dynamic` die folgenden Regeln werden zur Laufzeit verwendet, wobei der Lauf Zeittyp der *primary_expression*verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="20699-1178">Wenn der Operand einer Postfix-Inkrement-oder Dekrementoperation eine Eigenschaft oder ein Indexer-Zugriff ist, muss die Eigenschaft oder der Indexer sowohl einen `get`-als auch einen `set`-Accessor haben.</span><span class="sxs-lookup"><span data-stu-id="20699-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="20699-1179">Wenn dies nicht der Fall ist, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="20699-1180">Unäre Operator Überladungs Auflösung ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-1181">Vordefinierte Operatoren `++` und `--` sind für die folgenden Typen vorhanden: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, 0, 1, 2, 3 und beliebiger Enumerationstyp.</span><span class="sxs-lookup"><span data-stu-id="20699-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="20699-1182">Der vordefinierte `++`-Operatoren gibt den Wert zurück, der beim Hinzufügen von 1 zum Operanden erzeugt wurde, und die vordefinierten `--`-Operatoren geben den Wert zurück, der durch die Subtraktion von 1 vom Operanden erzeugt</span><span class="sxs-lookup"><span data-stu-id="20699-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="20699-1183">Wenn in einem `checked`-Kontext das Ergebnis dieser Addition oder Subtraktion außerhalb des Bereichs des Ergebnis Typs liegt und der Ergebnistyp ein ganzzahliger Typ oder Enumeration-Typ ist, wird eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="20699-1184">Die Lauf Zeit Verarbeitung eines Postfix-Inkrement-oder dekrementvorgangs in der Form `x++` oder `x--` besteht aus den folgenden Schritten:</span><span class="sxs-lookup"><span data-stu-id="20699-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="20699-1185">Wenn `x` als Variable klassifiziert ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="20699-1186">`x` wird ausgewertet, um die Variable zu entwickeln.</span><span class="sxs-lookup"><span data-stu-id="20699-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="20699-1187">Der Wert `x` wird gespeichert.</span><span class="sxs-lookup"><span data-stu-id="20699-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="20699-1188">Der ausgewählte Operator wird mit dem gespeicherten Wert `x` als Argument aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="20699-1189">Der vom Operator zurückgegebene Wert wird an dem Speicherort gespeichert, der durch die Auswertung von `x` angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="20699-1190">Der gespeicherte Wert von `x` wird das Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="20699-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="20699-1191">Wenn `x` als Eigenschaft oder Indexer-Zugriff klassifiziert ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="20699-1192">Der Instanzausdruck (wenn `x` nicht `static` ist) und die Argumentliste (wenn `x` ein Indexer-Zugriff ist), die `x` zugeordnet ist, werden ausgewertet, und die Ergebnisse werden in den nachfolgenden `get`-und `set`-Accessor-Aufrufen verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="20699-1193">Der `get`-Accessor von `x` wird aufgerufen, und der zurückgegebene Wert wird gespeichert.</span><span class="sxs-lookup"><span data-stu-id="20699-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="20699-1194">Der ausgewählte Operator wird mit dem gespeicherten Wert `x` als Argument aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="20699-1195">Der `set`-Accessor von `x` wird mit dem Wert aufgerufen, der vom Operator als `value`-Argument zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="20699-1196">Der gespeicherte Wert von `x` wird das Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="20699-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="20699-1197">Die Operatoren "`++`" und "`--`" unterstützen auch Präfix Notation ([Präfix Inkrement-und Dekrementoperator](expressions.md#prefix-increment-and-decrement-operators))</span><span class="sxs-lookup"><span data-stu-id="20699-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="20699-1198">In der Regel ist das Ergebnis von `x++` oder `x--` der Wert von `x` vor dem Vorgang, wohingegen das Ergebnis von `++x` oder `--x` der Wert von `x` nach dem Vorgang ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="20699-1199">In beiden Fällen hat `x` selbst denselben Wert nach dem Vorgang.</span><span class="sxs-lookup"><span data-stu-id="20699-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="20699-1200">Eine `operator ++`-oder `operator --`-Implementierung kann entweder mithilfe der Postfix-oder Präfix Notation aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="20699-1201">Es ist nicht möglich, separate Operator Implementierungen für die beiden Notationen zu haben.</span><span class="sxs-lookup"><span data-stu-id="20699-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="20699-1202">Der new-Operator</span><span class="sxs-lookup"><span data-stu-id="20699-1202">The new operator</span></span>

<span data-ttu-id="20699-1203">Der `new`-Operator wird verwendet, um neue Instanzen von Typen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="20699-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="20699-1204">Es gibt drei Formen von `new`-ausdrücken:</span><span class="sxs-lookup"><span data-stu-id="20699-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="20699-1205">Objekt Erstellungs Ausdrücke werden verwendet, um neue Instanzen von Klassentypen und Werttypen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="20699-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="20699-1206">Array Erstellungs Ausdrücke werden verwendet, um neue Instanzen von Array Typen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="20699-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="20699-1207">Delegaterstellungs-Ausdrücke werden verwendet, um neue Instanzen von Delegattypen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="20699-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="20699-1208">Der `new`-Operator impliziert die Erstellung einer Instanz eines Typs, impliziert jedoch nicht notwendigerweise die dynamische Speicher Belegung.</span><span class="sxs-lookup"><span data-stu-id="20699-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="20699-1209">Vor allem sind für Instanzen von Werttypen außerhalb der Variablen, in denen Sie sich befinden, kein zusätzlicher Arbeitsspeicher erforderlich, und es treten keine dynamischen Zuordnungen auf, wenn `new` zum Erstellen von Instanzen von Werttypen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="20699-1210">Objekt Erstellungs Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-1210">Object creation expressions</span></span>

<span data-ttu-id="20699-1211">Ein *object_creation_expression* wird verwendet, um eine neue Instanz eines *class_type* oder eines *value_type*zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="20699-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

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

<span data-ttu-id="20699-1212">Der *Typ* eines *object_creation_expression* muss ein *class_type*, ein *value_type* oder ein *type_parameter*sein.</span><span class="sxs-lookup"><span data-stu-id="20699-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="20699-1213">Der *Typ* darf keine `abstract`- *class_type*sein.</span><span class="sxs-lookup"><span data-stu-id="20699-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="20699-1214">Die optionalen *argument_list* ([Argumentlisten](expressions.md#argument-lists)) sind nur zulässig, wenn der *Typ* ein *class_type* oder ein *struct_type*ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="20699-1215">Ein Objekt Erstellungs Ausdruck kann die Liste der Konstruktorargumente weglassen und einschließende Klammern einschließen, wenn er einen Objektinitialisierer oder einen Auflistungsinitialisierer enthält.</span><span class="sxs-lookup"><span data-stu-id="20699-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="20699-1216">Das Weglassen der Konstruktorargumentliste und das Einschließen von Klammern entspricht der Angabe einer leeren Argumentliste.</span><span class="sxs-lookup"><span data-stu-id="20699-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="20699-1217">Die Verarbeitung eines Ausdrucks zur Objekt Erstellung, der einen Objektinitialisierer oder Auflistungsinitialisierer enthält, besteht aus der ersten Verarbeitung des Instanzkonstruktors und der anschließenden Verarbeitung der Element-oder Element Initialisierungen, die vom Objektinitialisierer angegeben werden ([ Objektinitialisierer](expressions.md#object-initializers)) oder sammlungsinitialisierer[(](expressions.md#collection-initializers)Auflistungsinitialisierer).</span><span class="sxs-lookup"><span data-stu-id="20699-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="20699-1218">Wenn eines der Argumente im optionalen *argument_list* den Kompilier Zeittyp `dynamic` aufweist, wird das *object_creation_expression* dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)), und die folgenden Regeln werden zur Laufzeit mithilfe der Laufzeit angewendet. der Typ der Argumente der *argument_list* , die den Kompilier Zeittyp `dynamic` aufweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="20699-1219">Allerdings wird bei der Objekt Erstellung eine beschränkte Kompilierungszeit Überprüfung durchgeführt, wie in der [Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="20699-1220">Die Bindungs Zeit Verarbeitung eines *object_creation_expression* in der Form `new T(A)`, wobei `T` ein *class_type* oder ein *value_type* und `A` ein optionaler *argument_list*ist, besteht aus den folgenden Schritten:</span><span class="sxs-lookup"><span data-stu-id="20699-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="20699-1221">Wenn `T` ein *value_type* ist und `A` nicht vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="20699-1222">Der *object_creation_expression* ist ein standardkonstruktoraufruf.</span><span class="sxs-lookup"><span data-stu-id="20699-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="20699-1223">Das Ergebnis von *object_creation_expression* ist ein Wert vom Typ `T`, nämlich der Standardwert für `T`, wie im [System. ValueType-Typ](types.md#the-systemvaluetype-type)definiert.</span><span class="sxs-lookup"><span data-stu-id="20699-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="20699-1224">Andernfalls, wenn `T` ein *type_parameter* ist und `A` nicht vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="20699-1225">Wenn keine Werttyp Einschränkung oder Konstruktoreinschränkung ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) für `T` angegeben wurde, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="20699-1226">Das Ergebnis des *object_creation_expression* ist ein Wert des Lauf Zeit Typs, an den der Typparameter gebunden wurde, nämlich das Ergebnis des Aufrufs des Standardkonstruktors dieses Typs.</span><span class="sxs-lookup"><span data-stu-id="20699-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="20699-1227">Der Lauf Zeittyp kann ein Referenztyp oder ein Werttyp sein.</span><span class="sxs-lookup"><span data-stu-id="20699-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="20699-1228">Andernfalls, wenn `T` ein *class_type* oder ein *struct_type*ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="20699-1229">Wenn `T` ein `abstract`- *class_type*ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="20699-1230">Der aufzurufende Instanzkonstruktor wird mithilfe der Regeln zur Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution)bestimmt.</span><span class="sxs-lookup"><span data-stu-id="20699-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="20699-1231">Der Satz von Kandidaten Instanzkonstruktoren besteht aus allen zugänglichen Instanzkonstruktoren, die in `T` deklariert sind und in Bezug auf `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) anwendbar sind.</span><span class="sxs-lookup"><span data-stu-id="20699-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="20699-1232">Wenn der Satz von standardinstanzkonstruktoren leer ist, oder wenn ein einzelner Konstruktor mit der besten Instanz nicht identifiziert werden kann, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="20699-1233">Das Ergebnis des *object_creation_expression* ist ein Wert vom Typ `T`, nämlich der Wert, der durch Aufrufen des Instanzkonstruktors erzeugt wird, der im obigen Schritt festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="20699-1234">Andernfalls ist *object_creation_expression* ungültig, und ein Bindungs Fehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="20699-1235">Auch wenn die *object_creation_expression* dynamisch gebunden ist, ist der Kompilier Zeittyp immer noch `T`.</span><span class="sxs-lookup"><span data-stu-id="20699-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="20699-1236">Die Lauf Zeit Verarbeitung eines *object_creation_expression* im Format `new T(A)`, wobei `T` *class_type* oder ein *struct_type* und `A` ein optionales *argument_list*ist, besteht aus den folgenden Schritten:</span><span class="sxs-lookup"><span data-stu-id="20699-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="20699-1237">Wenn `T` ein *class_type*ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="20699-1238">Eine neue Instanz der-Klasse `T` ist zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="20699-1239">Wenn nicht genügend Arbeitsspeicher zum Zuordnen der neuen Instanz verfügbar ist, wird ein `System.OutOfMemoryException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="20699-1240">Alle Felder der neuen Instanz werden mit ihren Standardwerten initialisiert ([Standardwerte](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="20699-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="20699-1241">Der Instanzkonstruktor wird entsprechend den Regeln für den Funktionselement Aufruf ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="20699-1242">Ein Verweis auf die neu zugeordnete Instanz wird automatisch an den Instanzkonstruktor übergeben, und auf die Instanz kann innerhalb dieses Konstruktors als `this` zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="20699-1243">Wenn `T` ein *struct_type*ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="20699-1244">Eine Instanz vom Typ "`T`" wird durch Zuordnen einer temporären lokalen Variablen erstellt.</span><span class="sxs-lookup"><span data-stu-id="20699-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="20699-1245">Da ein Instanzkonstruktor eines *struct_type* erforderlich ist, um jedem Feld der erstellten Instanz einen Wert definitiv zuzuweisen, ist keine Initialisierung der temporären Variablen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="20699-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="20699-1246">Der Instanzkonstruktor wird entsprechend den Regeln für den Funktionselement Aufruf ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="20699-1247">Ein Verweis auf die neu zugeordnete Instanz wird automatisch an den Instanzkonstruktor übergeben, und auf die Instanz kann innerhalb dieses Konstruktors als `this` zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="20699-1248">Objektinitialisierer</span><span class="sxs-lookup"><span data-stu-id="20699-1248">Object initializers</span></span>

<span data-ttu-id="20699-1249">Ein ***Objektinitialisierer*** gibt Werte für NULL oder mehr Felder, Eigenschaften oder indizierte Elemente eines Objekts an.</span><span class="sxs-lookup"><span data-stu-id="20699-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

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

<span data-ttu-id="20699-1250">Ein Objektinitialisierer besteht aus einer Sequenz von Elementinitialisierern, die durch `{`-und `}`-Token eingeschlossen und durch Kommas getrennt sind.</span><span class="sxs-lookup"><span data-stu-id="20699-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="20699-1251">Jede *member_initializer* bezeichnet ein Ziel für die Initialisierung.</span><span class="sxs-lookup"><span data-stu-id="20699-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="20699-1252">Ein *Bezeichner* muss ein barrierefreies Feld oder eine Eigenschaft des Objekts benennen, das initialisiert wird, wohingegen ein in eckige Klammern eingeschlossenes *argument_list* Argumente für einen zugreif baren Indexer für das Objekt angeben muss, das initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="20699-1253">Es ist ein Fehler, wenn ein Objektinitialisierer mehr als einen Elementinitialisierer für das gleiche Feld oder die gleiche Eigenschaft einschließt.</span><span class="sxs-lookup"><span data-stu-id="20699-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="20699-1254">Auf jede *initializer_target* folgt ein Gleichheitszeichen und entweder ein Ausdruck, ein Objektinitialisierer oder ein Auflistungsinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="20699-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="20699-1255">Es ist nicht möglich, dass Ausdrücke im Objektinitialisierer auf das neu erstellte Objekt verweisen, das initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="20699-1256">Ein Member-Initialisierer, der einen Ausdruck angibt, nachdem das Gleichheitszeichen auf die gleiche Weise wie eine Zuweisung ([einfache Zuweisung](expressions.md#simple-assignment)) zum Ziel verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="20699-1257">Ein Member-Initialisierer, der einen Objektinitialisierer angibt, nachdem dasGleichheitszeichen ein instanzinitialisierer ist, d. h. eine Initialisierung eines eingebetteten Objekts.</span><span class="sxs-lookup"><span data-stu-id="20699-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="20699-1258">Anstatt dem Feld oder der Eigenschaft einen neuen Wert zuzuweisen, werden die Zuweisungen im Initialisierer für den initialisierten Objektinitialisierer als Zuweisungen zu Membern des Felds oder der Eigenschaft behandelt.</span><span class="sxs-lookup"><span data-stu-id="20699-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="20699-1259">Initialisierer für ein Objekt können nicht auf Eigenschaften mit einem Werttyp oder auf schreibgeschützte Felder mit einem Werttyp angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="20699-1260">Ein Member-Initialisierer, der einen Auflistungsinitialisierer angibt, nachdem das Gleichheitszeichen eine Initialisierung einer eingebetteten Auflistung ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="20699-1261">Anstatt dem Zielfeld, der Eigenschaft oder dem Indexer eine neue Auflistung zuzuweisen, werden die im Initialisierer angegebenen Elemente der Auflistung hinzugefügt, auf die das Ziel verweist.</span><span class="sxs-lookup"><span data-stu-id="20699-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="20699-1262">Das Ziel muss einen Sammlungstyp aufweisen, der die in [Auflistungsinitialisierer](expressions.md#collection-initializers) angegebenen Anforderungen erfüllt.</span><span class="sxs-lookup"><span data-stu-id="20699-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="20699-1263">Die Argumente für einen indexinitialisierer werden immer genau einmal ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="20699-1264">Folglich werden Sie, selbst wenn die Argumente am Ende nie verwendet werden (z. b. aufgrund eines leeren Initialisierers), auf Ihre Nebeneffekte ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="20699-1265">Die folgende Klasse stellt einen Punkt mit zwei Koordinaten dar:</span><span class="sxs-lookup"><span data-stu-id="20699-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="20699-1266">Eine Instanz von `Point` kann wie folgt erstellt und initialisiert werden:</span><span class="sxs-lookup"><span data-stu-id="20699-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="20699-1267">Dies hat die gleiche Wirkung wie</span><span class="sxs-lookup"><span data-stu-id="20699-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="20699-1268">Dabei ist `__a` eine anderweitig unsichtbare und nicht zugängliche temporäre Variable.</span><span class="sxs-lookup"><span data-stu-id="20699-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="20699-1269">Die folgende Klasse stellt ein Rechteck dar, das aus zwei Punkten erstellt wurde:</span><span class="sxs-lookup"><span data-stu-id="20699-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="20699-1270">Eine Instanz von `Rectangle` kann wie folgt erstellt und initialisiert werden:</span><span class="sxs-lookup"><span data-stu-id="20699-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="20699-1271">Dies hat die gleiche Wirkung wie</span><span class="sxs-lookup"><span data-stu-id="20699-1271">which has the same effect as</span></span>
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
<span data-ttu-id="20699-1272">Dabei sind `__p1` und `__p2` temporäre Variablen, die andernfalls unsichtbar und nicht zugänglich sind. @no__t</span><span class="sxs-lookup"><span data-stu-id="20699-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="20699-1273">Wenn `Rectangle`-Konstruktor die beiden eingebetteten `Point`-Instanzen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="20699-1274">Das folgende Konstrukt kann verwendet werden, um die eingebetteten `Point`-Instanzen zu initialisieren, anstatt neue Instanzen zuzuweisen:</span><span class="sxs-lookup"><span data-stu-id="20699-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="20699-1275">Dies hat die gleiche Wirkung wie</span><span class="sxs-lookup"><span data-stu-id="20699-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="20699-1276">Im folgenden Beispiel wird eine entsprechende Definition von C angegeben:</span><span class="sxs-lookup"><span data-stu-id="20699-1276">Given an appropriate definition of C, the following example:</span></span>
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
<span data-ttu-id="20699-1277">entspricht dieser Reihe von Zuweisungen:</span><span class="sxs-lookup"><span data-stu-id="20699-1277">is equivalent to this series of assignments:</span></span>
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
<span data-ttu-id="20699-1278">Dabei sind `__c` usw. generierte Variablen, die unsichtbar sind und für den Quellcode nicht zugänglich sind.</span><span class="sxs-lookup"><span data-stu-id="20699-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="20699-1279">Beachten Sie, dass die Argumente für `[0,0]` nur einmal ausgewertet werden und die Argumente für `[1,2]` einmal ausgewertet werden, auch wenn Sie nie verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="20699-1280">Auflistungsinitialisierer</span><span class="sxs-lookup"><span data-stu-id="20699-1280">Collection initializers</span></span>

<span data-ttu-id="20699-1281">Ein Auflistungsinitialisierer gibt die Elemente einer Auflistung an.</span><span class="sxs-lookup"><span data-stu-id="20699-1281">A collection initializer specifies the elements of a collection.</span></span>

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

<span data-ttu-id="20699-1282">Ein Auflistungsinitialisierer besteht aus einer Sequenz von Elementinitialisierern, die durch `{`-und `}`-Token eingeschlossen und durch Kommas getrennt sind.</span><span class="sxs-lookup"><span data-stu-id="20699-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="20699-1283">Jeder Elementinitialisierer gibt ein Element an, das dem Auflistungs Objekt hinzugefügt werden soll, das initialisiert wird, und besteht aus einer Liste von Ausdrücken, die von `{`-und `}`-Token eingeschlossen und durch Kommas getrennt sind.</span><span class="sxs-lookup"><span data-stu-id="20699-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="20699-1284">Ein Elementinitialisierer mit einem Ausdruck kann ohne geschweifte Klammern geschrieben werden, kann jedoch kein Zuweisungs Ausdruck sein, um Mehrdeutigkeit mit Member-Initialisierern zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="20699-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="20699-1285">Die *non_assignment_expression* -Produktion ist in [Expression](expressions.md#expression)definiert.</span><span class="sxs-lookup"><span data-stu-id="20699-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="20699-1286">Im folgenden finden Sie ein Beispiel für einen Objekt Erstellungs Ausdruck, der einen Auflistungsinitialisierer umfasst:</span><span class="sxs-lookup"><span data-stu-id="20699-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="20699-1287">Das Auflistungs Objekt, auf das ein Auflistungsinitialisierer angewendet wird, muss einen Typ aufweisen, der `System.Collections.IEnumerable` oder ein Kompilierzeitfehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="20699-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="20699-1288">Für jedes angegebene Element in der Reihenfolge ruft der sammlungsinitialisierer eine `Add`-Methode für das Zielobjekt mit der Ausdrucks Liste des elementinitialisierers als Argumentliste auf und wendet die normale Member-Suche-und Überladungs Auflösung für jeden Aufruf an.</span><span class="sxs-lookup"><span data-stu-id="20699-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="20699-1289">Folglich muss das Auflistungs Objekt über eine gültige Instanz-oder Erweiterungsmethode mit dem Namen `Add` für jeden Elementinitialisierer verfügen.</span><span class="sxs-lookup"><span data-stu-id="20699-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="20699-1290">Die folgende Klasse stellt einen Kontakt mit einem Namen und einer Liste von Telefonnummern dar:</span><span class="sxs-lookup"><span data-stu-id="20699-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="20699-1291">Ein `List<Contact>` kann wie folgt erstellt und initialisiert werden:</span><span class="sxs-lookup"><span data-stu-id="20699-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
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
<span data-ttu-id="20699-1292">Dies hat die gleiche Wirkung wie</span><span class="sxs-lookup"><span data-stu-id="20699-1292">which has the same effect as</span></span>
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
<span data-ttu-id="20699-1293">Dabei sind `__c1` und `__c2` temporäre Variablen, die andernfalls unsichtbar und nicht zugänglich sind. @no__t</span><span class="sxs-lookup"><span data-stu-id="20699-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="20699-1294">Ausdrücke zum Erstellen von Arrays</span><span class="sxs-lookup"><span data-stu-id="20699-1294">Array creation expressions</span></span>

<span data-ttu-id="20699-1295">Ein *array_creation_expression* wird zum Erstellen einer neuen Instanz eines *array_type*verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="20699-1296">Ein Array Erstellungs Ausdruck des ersten Formulars ordnet eine Array Instanz des Typs zu, der sich aus dem Löschen der einzelnen Ausdrücke aus der Ausdrucks Liste ergibt.</span><span class="sxs-lookup"><span data-stu-id="20699-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="20699-1297">Beispielsweise erzeugt der Ausdruck "Array Erstellung" `new int[10,20]` eine Array Instanz vom Typ "`int[,]`", und der Array Erstellungs Ausdruck "`new int[10][,]`" erzeugt ein Array vom Typ "`int[][,]`".</span><span class="sxs-lookup"><span data-stu-id="20699-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="20699-1298">Jeder Ausdruck in der Ausdrucks Liste muss den Typ "`int`", "`uint`", "`long`" oder "`ulong`" aufweisen oder implizit in einen oder mehrere dieser Typen konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="20699-1299">Der Wert jedes Ausdrucks bestimmt die Länge der entsprechenden Dimension in der neu zugeordneten Array Instanz.</span><span class="sxs-lookup"><span data-stu-id="20699-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="20699-1300">Da die Länge einer Array Dimension nicht negativ sein muss, ist dies ein Kompilierzeitfehler, bei dem ein *constant_expression* mit einem negativen Wert in der Ausdrucks Liste vorliegt.</span><span class="sxs-lookup"><span data-stu-id="20699-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="20699-1301">Das Layout von Arrays ist nicht festgelegt, es sei denn, es ist ein unsicherer Kontext ([unsichere Kontexte](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="20699-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="20699-1302">Wenn ein Array Erstellungs Ausdruck des ersten Formulars einen Arrayinitialisierer enthält, muss jeder Ausdruck in der Ausdrucks Liste eine Konstante sein, und die von der Ausdrucks Liste angegebenen Rang-und Dimensions Längen müssen mit denen des Arrayinitialisierers identisch sein.</span><span class="sxs-lookup"><span data-stu-id="20699-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="20699-1303">In einem Array Erstellungs Ausdruck des zweiten oder dritten Formulars muss der Rang des angegebenen Arraytyps oder rangspezifizierers mit dem des Arrayinitialisierers identisch sein.</span><span class="sxs-lookup"><span data-stu-id="20699-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="20699-1304">Die einzelnen Dimensions Längen werden von der Anzahl von Elementen in jeder der entsprechenden Schachtelungs Ebenen des Arrayinitialisierers abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="20699-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="20699-1305">Folglich ist der Ausdruck</span><span class="sxs-lookup"><span data-stu-id="20699-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="20699-1306">entspricht genau</span><span class="sxs-lookup"><span data-stu-id="20699-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="20699-1307">Ein Array Erstellungs Ausdruck des dritten Formulars wird als ***implizit typisierter Array Erstellungs Ausdruck***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="20699-1308">Sie ähnelt dem zweiten Formular, mit dem Unterschied, dass der Elementtyp des Arrays nicht explizit angegeben wird, sondern als der am besten geeignete Typ (suchen[des besten allgemeinen Typs eines Satzes von Ausdrücken](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) des Satzes von Ausdrücken im Arrayinitialisierer festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="20699-1309">Bei einem mehrdimensionalen Array, d. h. einer, bei dem das *rank_specifier* mindestens ein Komma enthält, besteht dieser Satz aus allen *Ausdrücken*, die in den naleten *array_initializer*s gefunden wurden.</span><span class="sxs-lookup"><span data-stu-id="20699-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="20699-1310">Arrayinitialisierer werden in [arrayinitialisierern](arrays.md#array-initializers)ausführlicher beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="20699-1311">Das Ergebnis der Auswertung eines Array Erstellungs Ausdrucks wird als Wert klassifiziert, nämlich als Verweis auf die neu zugeordnete Array Instanz.</span><span class="sxs-lookup"><span data-stu-id="20699-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="20699-1312">Die Lauf Zeit Verarbeitung eines Ausdrucks zur Array Erstellung besteht aus den folgenden Schritten:</span><span class="sxs-lookup"><span data-stu-id="20699-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="20699-1313">Die Dimensions Längen Ausdrücke der *expression_list* werden in der Reihenfolge von links nach rechts ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="20699-1314">Nach der Auswertung der einzelnen Ausdrücke wird eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) in einen der folgenden Typen ausgeführt: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="20699-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="20699-1315">Der erste Typ in dieser Liste, für den eine implizite Konvertierung vorhanden ist, wird ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="20699-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="20699-1316">Wenn die Auswertung eines Ausdrucks oder der nachfolgenden impliziten Konvertierung eine Ausnahme verursacht, werden keine weiteren Ausdrücke ausgewertet, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="20699-1317">Die berechneten Werte für die Dimensions Längen werden wie folgt überprüft.</span><span class="sxs-lookup"><span data-stu-id="20699-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="20699-1318">Wenn mindestens einer der-Werte kleiner als 0 (null) ist, wird ein @no__t 0 (null) ausgelöst, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="20699-1319">Eine Array Instanz mit den angegebenen Dimensions Längen ist zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="20699-1320">Wenn nicht genügend Arbeitsspeicher zum Zuordnen der neuen Instanz verfügbar ist, wird ein `System.OutOfMemoryException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="20699-1321">Alle Elemente der neuen Array Instanz werden mit ihren Standardwerten initialisiert ([Standardwerte](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="20699-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="20699-1322">Wenn der Ausdruck für die Array Erstellung einen Arrayinitialisierer enthält, wird jeder Ausdruck im Arrayinitialisierer ausgewertet und dem entsprechenden Array Element zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="20699-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="20699-1323">Die Auswertungen und Zuweisungen werden in der Reihenfolge ausgeführt, in der die Ausdrücke in den Arrayinitialisierer geschrieben werden – mit anderen Worten, Elemente werden in steigender Index Reihenfolge initialisiert, wobei die äußerste Rechte Dimension zuerst zunimmt.</span><span class="sxs-lookup"><span data-stu-id="20699-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="20699-1324">Wenn die Auswertung eines angegebenen Ausdrucks oder der nachfolgenden Zuweisung zum entsprechenden Array Element eine Ausnahme auslöst, werden keine weiteren Elemente initialisiert (und die restlichen Elemente haben daher ihre Standardwerte).</span><span class="sxs-lookup"><span data-stu-id="20699-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="20699-1325">Ein Array Erstellungs Ausdruck ermöglicht die Instanziierung eines Arrays mit Elementen eines Arraytyps, aber die Elemente eines solchen Arrays müssen manuell initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="20699-1326">Beispielsweise ist die-Anweisung</span><span class="sxs-lookup"><span data-stu-id="20699-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="20699-1327">erstellt ein eindimensionales Array mit 100 Elementen vom Typ `int[]`.</span><span class="sxs-lookup"><span data-stu-id="20699-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="20699-1328">Der Anfangswert jedes Elements ist `null`.</span><span class="sxs-lookup"><span data-stu-id="20699-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="20699-1329">Es ist nicht möglich, dass derselbe Array Erstellungs Ausdruck auch die Unterarrays und die-Anweisung instanziiert.</span><span class="sxs-lookup"><span data-stu-id="20699-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="20699-1330">führt zu einem Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="20699-1330">results in a compile-time error.</span></span> <span data-ttu-id="20699-1331">Die Instanziierung der Unterarrays muss stattdessen manuell ausgeführt werden, wie in</span><span class="sxs-lookup"><span data-stu-id="20699-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="20699-1332">Wenn ein Array von Arrays eine "rechteckige" Form hat, d. h., wenn die Teil Arrays die gleiche Länge haben, ist es effizienter, ein mehrdimensionales Array zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="20699-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="20699-1333">Im obigen Beispiel erstellt die Instanziierung des Arrays von Arrays 101 Objekte – ein äußeres Array und 100 Teil Arrays.</span><span class="sxs-lookup"><span data-stu-id="20699-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="20699-1334">Im Gegensatz dazu</span><span class="sxs-lookup"><span data-stu-id="20699-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="20699-1335">erstellt nur ein einzelnes-Objekt, ein zweidimensionales Array und erreicht die Zuordnung in einer einzelnen Anweisung.</span><span class="sxs-lookup"><span data-stu-id="20699-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="20699-1336">Im folgenden finden Sie Beispiele für implizit typisierte Array Erstellungs Ausdrücke:</span><span class="sxs-lookup"><span data-stu-id="20699-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="20699-1337">Der letzte Ausdruck verursacht einen Kompilierzeitfehler, da weder `int` noch `string` implizit in den anderen konvertiert werden kann, sodass es keinen optimalen allgemeinen Typ gibt.</span><span class="sxs-lookup"><span data-stu-id="20699-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="20699-1338">In diesem Fall muss ein explizit typisierter Array Erstellungs Ausdruck verwendet werden, z. b. die Angabe des Typs, der `object[]` lauten soll.</span><span class="sxs-lookup"><span data-stu-id="20699-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="20699-1339">Alternativ kann eines der Elemente in einen allgemeinen Basistyp umgewandelt werden, der dann zum abzurufenden Elementtyp wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="20699-1340">Implizit typisierte Array Erstellungs Ausdrücke können mit anonymen Objektinitialisierern ([anonymen Objekt Erstellungs Ausdrücken](expressions.md#anonymous-object-creation-expressions)) kombiniert werden, um anonym typisierte Datenstrukturen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="20699-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="20699-1341">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="20699-1341">For example:</span></span>
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

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="20699-1342">Delegatenerstellungs Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-1342">Delegate creation expressions</span></span>

<span data-ttu-id="20699-1343">Ein *delegate_creation_expression* wird zum Erstellen einer neuen Instanz eines *delegate_type*verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="20699-1344">Das Argument eines delegaterstellungs-Ausdrucks muss eine Methoden Gruppe, eine anonyme Funktion oder ein Wert entweder der Kompilier Zeittyp `dynamic` oder ein *delegate_type*sein.</span><span class="sxs-lookup"><span data-stu-id="20699-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="20699-1345">Wenn das Argument eine Methoden Gruppe ist, identifiziert es die-Methode und für eine Instanzmethode das-Objekt, für das ein Delegat erstellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="20699-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="20699-1346">Wenn das Argument eine anonyme Funktion ist, werden die Parameter und der Methoden Text des delegatziels direkt definiert.</span><span class="sxs-lookup"><span data-stu-id="20699-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="20699-1347">Wenn das-Argument ein Wert ist, wird eine Delegatinstanz identifiziert, von der eine Kopie erstellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="20699-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="20699-1348">Wenn der *Ausdruck* den Kompilier Zeittyp `dynamic` aufweist, wird das *delegate_creation_expression* dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)), und die unten aufgeführten Regeln werden zur Laufzeit mithilfe des Lauf Zeittyps des *Ausdrucks*angewendet.</span><span class="sxs-lookup"><span data-stu-id="20699-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="20699-1349">Andernfalls werden die Regeln zur Kompilierzeit angewendet.</span><span class="sxs-lookup"><span data-stu-id="20699-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="20699-1350">Die Bindungs Zeit Verarbeitung eines *delegate_creation_expression* in der Form `new D(E)`, wobei `D` ein *delegate_type* und `E` ein *Ausdruck*ist, besteht aus den folgenden Schritten:</span><span class="sxs-lookup"><span data-stu-id="20699-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="20699-1351">Wenn `E` eine Methoden Gruppe ist, wird der Delegat Erstellungs Ausdruck auf die gleiche Weise wie eine Methoden Gruppen Konvertierung ([Methoden Gruppen Konvertierungen](conversions.md#method-group-conversions)) von `E` in `D` verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="20699-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="20699-1352">Wenn `E` eine anonyme Funktion ist, wird der delegaterstellungs-Ausdruck auf die gleiche Weise wie eine anonyme Funktions Konvertierung ([Anonyme Funktions Konvertierungen](conversions.md#anonymous-function-conversions)) von `E` in `D` verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="20699-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="20699-1353">Wenn `E` ein Wert ist, muss `E` kompatibel ([Delegatdeklarationen](delegates.md#delegate-declarations)) mit `D` sein, und das Ergebnis ist ein Verweis auf einen neu erstellten Delegaten vom Typ `D`, der auf dieselbe Aufruf Liste wie `E` verweist.</span><span class="sxs-lookup"><span data-stu-id="20699-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="20699-1354">Wenn `E` nicht mit `D` kompatibel ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="20699-1355">Die Lauf Zeit Verarbeitung eines *delegate_creation_expression* in der Form `new D(E)`, wobei `D` ein *delegate_type* und `E` ein *Ausdruck*ist, besteht aus den folgenden Schritten:</span><span class="sxs-lookup"><span data-stu-id="20699-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="20699-1356">Wenn `E` eine Methoden Gruppe ist, wird der delegaterstellungs-Ausdruck als Methoden Gruppen Konvertierung ([Methoden Gruppen Konvertierungen](conversions.md#method-group-conversions)) von `E` in `D` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="20699-1357">Wenn `E` eine anonyme Funktion ist, wird die Delegaterstellung als anonyme Funktions Konvertierung von `E` in `D` ([Anonyme Funktions Konvertierungen](conversions.md#anonymous-function-conversions)) ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="20699-1358">Wenn `E` ein *delegate_type*-Wert ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="20699-1359">`E` wird ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-1359">`E` is evaluated.</span></span> <span data-ttu-id="20699-1360">Wenn diese Auswertung eine Ausnahme verursacht, werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="20699-1361">Wenn der Wert `E` `null` ist, wird ein `System.NullReferenceException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="20699-1362">Eine neue Instanz des Delegattyps `D` ist zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="20699-1363">Wenn nicht genügend Arbeitsspeicher zum Zuordnen der neuen Instanz verfügbar ist, wird ein `System.OutOfMemoryException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="20699-1364">Die neue Delegatinstanz wird mit der gleichen Aufruf Liste initialisiert wie die Delegatinstanz, die von `E` angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="20699-1365">Die Aufruf Liste eines Delegaten wird bestimmt, wenn der Delegat instanziiert wird, und wird dann für die gesamte Lebensdauer des Delegaten konstant bleiben.</span><span class="sxs-lookup"><span data-stu-id="20699-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="20699-1366">Anders ausgedrückt: Es ist nicht möglich, die Ziel Aufruf baren Entitäten eines Delegaten zu ändern, nachdem er erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="20699-1367">Wenn zwei Delegaten kombiniert werden oder eine aus einer anderen entfernt wird ([Delegatdeklarationen](delegates.md#delegate-declarations)), ergibt sich ein neuer Delegat. der Inhalt eines vorhandenen Delegaten wurde nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="20699-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="20699-1368">Es ist nicht möglich, einen Delegaten zu erstellen, der auf eine Eigenschaft, einen Indexer, einen benutzerdefinierten Operator, einen Instanzkonstruktor, einen Dekonstruktor oder einen statischen Konstruktor verweist.</span><span class="sxs-lookup"><span data-stu-id="20699-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="20699-1369">Wie oben beschrieben, wird beim Erstellen eines Delegaten aus einer Methoden Gruppe die Liste der formalen Parameter und der Rückgabetyp des Delegaten bestimmt, welche der überladenen Methoden ausgewählt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="20699-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="20699-1370">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-1370">In the example</span></span>
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
<span data-ttu-id="20699-1371">das Feld "`A.f`" wird mit einem Delegaten initialisiert, der auf die zweite Methode `Square` verweist, da diese Methode exakt mit der formalen Parameterliste und dem Rückgabetyp von `DoubleFunc` übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="20699-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="20699-1372">Wenn die zweite `Square`-Methode nicht vorhanden wäre, wäre ein Kompilierzeitfehler aufgetreten.</span><span class="sxs-lookup"><span data-stu-id="20699-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="20699-1373">Ausdrücke zum Erstellen anonymer Objekte</span><span class="sxs-lookup"><span data-stu-id="20699-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="20699-1374">Ein *anonymous_object_creation_expression* wird verwendet, um ein Objekt eines anonymen Typs zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="20699-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

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

<span data-ttu-id="20699-1375">Ein Anonymer Objektinitialisierer deklariert einen anonymen Typ und gibt eine Instanz dieses Typs zurück.</span><span class="sxs-lookup"><span data-stu-id="20699-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="20699-1376">Ein anonymer Typ ist ein namenloser Klassentyp, der direkt von `object` erbt.</span><span class="sxs-lookup"><span data-stu-id="20699-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="20699-1377">Die Member eines anonymen Typs sind eine Sequenz von schreibgeschützten Eigenschaften, die vom anonymen Objektinitialisierer abgeleitet werden, um eine Instanz des Typs zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="20699-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="20699-1378">Genauer gesagt: ein Anonymer Objektinitialisierer des Formulars</span><span class="sxs-lookup"><span data-stu-id="20699-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="20699-1379">deklariert einen anonymen Typ des Formulars.</span><span class="sxs-lookup"><span data-stu-id="20699-1379">declares an anonymous type of the form</span></span>
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
<span data-ttu-id="20699-1380">Dabei ist jede `Tx` der Typ des entsprechenden Ausdrucks `ex`.</span><span class="sxs-lookup"><span data-stu-id="20699-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="20699-1381">Der Ausdruck, der in einem *member_declarator* verwendet wird, muss einen Typ aufweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="20699-1382">Daher ist es ein Kompilierzeitfehler für einen Ausdruck in einem *member_declarator* -Wert, der NULL oder eine anonyme Funktion ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="20699-1383">Es ist auch ein Kompilierzeitfehler, wenn der Ausdruck einen unsicheren Typ hat.</span><span class="sxs-lookup"><span data-stu-id="20699-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="20699-1384">Die Namen eines anonymen Typs und des Parameters für die `Equals`-Methode werden vom Compiler automatisch generiert, und im Programmtext kann nicht auf Sie verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="20699-1385">Innerhalb desselben Programms werden zwei anonyme Objektinitialisierer, die eine Sequenz von Eigenschaften mit denselben Namen und Kompilierzeit Typen in derselben Reihenfolge angeben, Instanzen desselben anonymen Typs erstellen.</span><span class="sxs-lookup"><span data-stu-id="20699-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="20699-1386">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="20699-1387">die Zuweisung in der letzten Zeile ist zulässig, da `p1` und `p2` denselben anonymen Typ haben.</span><span class="sxs-lookup"><span data-stu-id="20699-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="20699-1388">Die Methoden `Equals` und `GetHashcode` für anonyme Typen überschreiben die von `object` geerbten Methoden und werden im Hinblick auf die `Equals` und `GetHashcode` der Eigenschaften definiert, sodass zwei Instanzen desselben anonymen Typs nur dann gleich sind, wenn alle ihre Eigenschaften hoch.</span><span class="sxs-lookup"><span data-stu-id="20699-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="20699-1389">Ein Element Deklarator kann mit einem einfachen Namen ([Typrückschluss](expressions.md#type-inference)), einem Element Zugriff ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), einem Basis Zugriff ([Basis Zugriff](expressions.md#base-access)) oder einem NULL bedingten Member-Zugriff ([ NULL bedingte Ausdrücke als Projektions Initialisierer](expressions.md#null-conditional-expressions-as-projection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="20699-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="20699-1390">Dies wird als ***Projektions Initialisierer*** bezeichnet und ist eine Abkürzung für eine Deklaration von und die Zuweisung zu einer Eigenschaft mit demselben Namen.</span><span class="sxs-lookup"><span data-stu-id="20699-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="20699-1391">Insbesondere Member-Deklaratoren der Formulare</span><span class="sxs-lookup"><span data-stu-id="20699-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="20699-1392">entsprechen genau den folgenden:</span><span class="sxs-lookup"><span data-stu-id="20699-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="20699-1393">Folglich wählt der *Bezeichner* in einem Projektions Initialisierer sowohl den Wert als auch das Feld oder die Eigenschaft aus, dem der Wert zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="20699-1394">Intuitiv projiziert ein Projektions Initialisierer nicht nur einen Wert, sondern auch den Namen des Werts.</span><span class="sxs-lookup"><span data-stu-id="20699-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="20699-1395">Der typeof-Operator</span><span class="sxs-lookup"><span data-stu-id="20699-1395">The typeof operator</span></span>

<span data-ttu-id="20699-1396">Der `typeof`-Operator wird zum Abrufen des `System.Type`-Objekts für einen Typ verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

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

<span data-ttu-id="20699-1397">Die erste Form von *typeof_expression* besteht aus einem `typeof`-Schlüsselwort, gefolgt von einem *Typ*in Klammern.</span><span class="sxs-lookup"><span data-stu-id="20699-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="20699-1398">Das Ergebnis eines Ausdrucks dieses Formulars ist das `System.Type`-Objekt für den bestimmten Typ.</span><span class="sxs-lookup"><span data-stu-id="20699-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="20699-1399">Es ist nur ein `System.Type`-Objekt für einen bestimmten Typ vorhanden.</span><span class="sxs-lookup"><span data-stu-id="20699-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="20699-1400">Dies bedeutet, dass bei einem Typ @ no__t-0 `typeof(T) == typeof(T)` immer true ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="20699-1401">Der *Typ* kann nicht `dynamic` sein.</span><span class="sxs-lookup"><span data-stu-id="20699-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="20699-1402">Die zweite Form von *typeof_expression* besteht aus einem `typeof`-Schlüsselwort, gefolgt von einer *unbound_type_name*in Klammern.</span><span class="sxs-lookup"><span data-stu-id="20699-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="20699-1403">Ein *unbound_type_name* ähnelt einem *TYPE_NAME* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)), mit dem Unterschied, dass ein *unbound_type_name* *generic_dimension_specifier*s enthält, in dem ein *TYPE_NAME* Type_ enthält.  *argument_list*s.</span><span class="sxs-lookup"><span data-stu-id="20699-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="20699-1404">Wenn der Operand einer *typeof_expression* eine Sequenz von Token ist, die die Grammatiken von *unbound_type_name* und *TYPE_NAME*erfüllt, d. h., wenn Sie weder einen *generic_dimension_specifier* noch einen type_argument enthält.  *_list*, die Sequenz von Token wird als *TYPE_NAME*betrachtet.</span><span class="sxs-lookup"><span data-stu-id="20699-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="20699-1405">Die Bedeutung eines *unbound_type_name* wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="20699-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="20699-1406">Konvertieren Sie die Sequenz von Token in *TYPE_NAME* , indem Sie jede *generic_dimension_specifier* durch eine *type_argument_list* ersetzen, die dieselbe Anzahl von Kommas und das Schlüsselwort `object` wie jede *type_argument*hat.</span><span class="sxs-lookup"><span data-stu-id="20699-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="20699-1407">Evaluieren Sie die resultierende *TYPE_NAME*, während alle Typparameter Einschränkungen ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="20699-1408">Der *unbound_type_name* wird in den ungebundenen generischen Typ aufgelöst, der dem resultierenden konstruierten Typ zugeordnet ist ([gebundene und ungebundene Typen](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="20699-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="20699-1409">Das Ergebnis des *typeof_expression* ist das `System.Type`-Objekt für den resultierenden ungebundenen generischen Typ.</span><span class="sxs-lookup"><span data-stu-id="20699-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="20699-1410">Die dritte Form von *typeof_expression* besteht aus einem `typeof`-Schlüsselwort, gefolgt von einem `void`-Schlüsselwort in Klammern.</span><span class="sxs-lookup"><span data-stu-id="20699-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="20699-1411">Das Ergebnis eines Ausdrucks dieser Form ist das `System.Type`-Objekt, das das Fehlen eines Typs darstellt.</span><span class="sxs-lookup"><span data-stu-id="20699-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="20699-1412">Das von `typeof(void)` zurückgegebene Typobjekt unterscheidet sich vom Typobjekt, das für einen beliebigen Typ zurückgegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="20699-1413">Dieses besondere Typobjekt eignet sich für Klassenbibliotheken, die die Reflektion auf Methoden in der Sprache zulassen, wobei diese Methoden eine Möglichkeit haben möchten, den Rückgabetyp jeder Methode, einschließlich void-Methoden, mit einer Instanz von `System.Type` darzustellen.</span><span class="sxs-lookup"><span data-stu-id="20699-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="20699-1414">Der `typeof`-Operator kann für einen Typparameter verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="20699-1415">Das Ergebnis ist das `System.Type`-Objekt für den Lauf Zeittyp, der an den Typparameter gebunden wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="20699-1416">Der `typeof`-Operator kann auch für einen konstruierten Typ oder einen ungebundenen generischen Typ ([gebundene und ungebundene Typen](types.md#bound-and-unbound-types)) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="20699-1417">Das `System.Type`-Objekt für einen ungebundenen generischen Typ ist nicht mit dem `System.Type`-Objekt des Instanztyps identisch.</span><span class="sxs-lookup"><span data-stu-id="20699-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="20699-1418">Der Instanztyp ist zur Laufzeit immer ein geschlossener konstruierter Typ, damit das `System.Type`-Objekt von den verwendeten Lauf Zeittyp Argumenten abhängt, während der ungebundene generische Typ keine Typargumente aufweist.</span><span class="sxs-lookup"><span data-stu-id="20699-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="20699-1419">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-1419">The example</span></span>
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
<span data-ttu-id="20699-1420">erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="20699-1420">produces the following output:</span></span>
```console
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

<span data-ttu-id="20699-1421">Beachten Sie, dass `int` und `System.Int32` denselben Typ haben.</span><span class="sxs-lookup"><span data-stu-id="20699-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="20699-1422">Beachten Sie auch, dass das Ergebnis von `typeof(X<>)` nicht vom Typargument abhängt, aber das Ergebnis von `typeof(X<T>)` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="20699-1423">Checked- und Unchecked-Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="20699-1424">Mit den Operatoren "`checked`" und "`unchecked`" wird der ***Überlauf Überprüfungs Kontext*** für arithmetische Operationen und Konvertierungen im integralen Typ gesteuert.</span><span class="sxs-lookup"><span data-stu-id="20699-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="20699-1425">Der `checked`-Operator wertet den enthaltenen Ausdruck in einem überprüften Kontext aus, und der Operator "`unchecked`" wertet den enthaltenen Ausdruck in einem nicht überprüften Kontext aus.</span><span class="sxs-lookup"><span data-stu-id="20699-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="20699-1426">Ein *checked_expression* -oder *unchecked_expression* -Wert entspricht genau einem *parenthesized_expression* -Ausdruck (in[Klammern](expressions.md#parenthesized-expressions)), mit dem Unterschied, dass der enthaltene Ausdruck im angegebenen Überlauf Überprüfungs Kontext ausgewertet wird. .</span><span class="sxs-lookup"><span data-stu-id="20699-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="20699-1427">Der Überlauf Überprüfungs Kontext kann auch über die Anweisungen `checked` und `unchecked` (die aktivierten und deaktivierten[Anweisungen](statements.md#the-checked-and-unchecked-statements)) gesteuert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="20699-1428">Die folgenden Vorgänge wirken sich auf den Überlauf Prüfungs Kontext aus, der von den Operatoren `checked` und `unchecked` und den-Anweisungen festgelegt wird:</span><span class="sxs-lookup"><span data-stu-id="20699-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="20699-1429">Die vordefinierten Operatoren "`++`" und "`--`" ([postfix-Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators) und [Präfix Inkrement-und Dekrementoperatoren](expressions.md#prefix-increment-and-decrement-operators)), wenn der Operand ein ganzzahliger Typ ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="20699-1430">Der vordefinierte unäre `-`-Operator ([unärer Minus Operator](expressions.md#unary-minus-operator)), wenn der Operand ein ganzzahliger Typ ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="20699-1431">Die vordefinierten `+`-, `-`-, `*`-und `/`-Binär Operatoren ([arithmetische Operatoren](expressions.md#arithmetic-operators)), wenn beide Operanden ganzzahlige Typen sind.</span><span class="sxs-lookup"><span data-stu-id="20699-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="20699-1432">Explizite numerische Konvertierungen ([explizite numerische Konvertierungen](conversions.md#explicit-numeric-conversions)) von einem ganzzahligen Typ in einen anderen ganzzahligen Typ oder von `float` oder `double` in einen ganzzahligen Typ.</span><span class="sxs-lookup"><span data-stu-id="20699-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="20699-1433">Wenn eine der obigen Vorgänge ein Ergebnis erzeugt, das zu groß für die Darstellung im Zieltyp ist, steuert der Kontext, in dem der Vorgang ausgeführt wird, das resultierende Verhalten:</span><span class="sxs-lookup"><span data-stu-id="20699-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="20699-1434">In einem `checked`-Kontext tritt ein Kompilierzeitfehler auf, wenn es sich bei dem Vorgang um einen konstanten Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)) handelt.</span><span class="sxs-lookup"><span data-stu-id="20699-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="20699-1435">Andernfalls wird, wenn der Vorgang zur Laufzeit ausgeführt wird, eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="20699-1436">In einem `unchecked`-Kontext wird das Ergebnis abgeschnitten, indem alle höherwertigen Bits verworfen werden, die nicht in den Zieltyp passen.</span><span class="sxs-lookup"><span data-stu-id="20699-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="20699-1437">Für nicht konstante Ausdrücke (Ausdrücke, die zur Laufzeit ausgewertet werden), die nicht von `checked`-oder `unchecked`-Operatoren oder-Anweisungen eingeschlossen werden, ist der Standardkontext der Überlauf Überprüfung `unchecked`, es sei denn, externe Faktoren (z. b. Compilerschalter und Konfiguration der Ausführungsumgebung) für die `checked`-Auswertung.</span><span class="sxs-lookup"><span data-stu-id="20699-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="20699-1438">Für konstante Ausdrücke (Ausdrücke, die zur Kompilierzeit vollständig ausgewertet werden können) ist der Standardkontext der Überlauf Überprüfung immer `checked`.</span><span class="sxs-lookup"><span data-stu-id="20699-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="20699-1439">Wenn ein konstanter Ausdruck nicht explizit in einem `unchecked`-Kontext abgelegt wird, verursachen über Flüsse, die während der Kompilierzeit Auswertung des Ausdrucks auftreten, immer Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="20699-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="20699-1440">Der Text einer anonymen Funktion wird nicht von `checked`-oder `unchecked`-Kontexten beeinflusst, in denen die anonyme Funktion auftritt.</span><span class="sxs-lookup"><span data-stu-id="20699-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="20699-1441">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-1441">In the example</span></span>
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
<span data-ttu-id="20699-1442">Es werden keine Kompilierzeitfehler gemeldet, da keiner der Ausdrücke zur Kompilierzeit ausgewertet werden kann.</span><span class="sxs-lookup"><span data-stu-id="20699-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="20699-1443">Zur Laufzeit löst die `F`-Methode eine `System.OverflowException` aus, und die `G`-Methode gibt-727379968 (die unteren 32 Bits des Ergebnisses außerhalb des gültigen Bereichs) zurück.</span><span class="sxs-lookup"><span data-stu-id="20699-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="20699-1444">Das Verhalten der `H`-Methode hängt vom Standardkontext der Überlauf Überprüfung für die Kompilierung ab, ist jedoch entweder identisch mit `F` oder identisch mit `G`.</span><span class="sxs-lookup"><span data-stu-id="20699-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="20699-1445">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-1445">In the example</span></span>
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
<span data-ttu-id="20699-1446">die über Flüsse, die beim Auswerten der Konstanten Ausdrücke in `F` und `H` auftreten, bewirken, dass Kompilierzeitfehler gemeldet werden, weil die Ausdrücke in einem `checked`-Kontext ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="20699-1447">Ein Überlauf tritt auch auf, wenn der Konstante Ausdruck in `G` ausgewertet wird. da die Auswertung jedoch in einem `unchecked`-Kontext stattfindet, wird der Überlauf nicht gemeldet.</span><span class="sxs-lookup"><span data-stu-id="20699-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="20699-1448">Die Operatoren "`checked`" und "`unchecked`" wirken sich nur auf den Überlauf Überprüfungs Kontext für die Vorgänge aus, die in den Token "`(`" und "`)`" textuell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="20699-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="20699-1449">Die Operatoren haben keine Auswirkung auf Funktionsmember, die als Ergebnis der Auswertung des enthaltenen Ausdrucks aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="20699-1450">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-1450">In the example</span></span>
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
<span data-ttu-id="20699-1451">die Verwendung von `checked` in `F` wirkt sich nicht auf die Auswertung von `x * y` in `Multiply` aus, sodass `x * y` im Standardkontext der Überlauf Überprüfung ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="20699-1452">Der `unchecked`-Operator ist praktisch, wenn Konstanten der ganzzahligen Typen mit Vorzeichen in hexadezimal Notation geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="20699-1453">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="20699-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="20699-1454">Beide hexadezimal Konstanten oben sind vom Typ `uint`.</span><span class="sxs-lookup"><span data-stu-id="20699-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="20699-1455">Da die Konstanten außerhalb des `int`-Bereichs liegen, ohne den `unchecked`-Operator, würden die Umwandlungen in `int` Kompilierzeitfehler verursachen.</span><span class="sxs-lookup"><span data-stu-id="20699-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="20699-1456">Mit den Operatoren "`checked`" und "`unchecked`" können Programmierer bestimmte Aspekte einiger numerischer Berechnungen steuern.</span><span class="sxs-lookup"><span data-stu-id="20699-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="20699-1457">Das Verhalten einiger numerischer Operatoren hängt jedoch von den Datentypen ihrer Operanden ab.</span><span class="sxs-lookup"><span data-stu-id="20699-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="20699-1458">Beispielsweise führt die Multiplikation von zwei Dezimalzahlen immer zu einer Ausnahme bei einem Überlauf, auch innerhalb eines expliziten `unchecked`-Konstrukts.</span><span class="sxs-lookup"><span data-stu-id="20699-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="20699-1459">Analog dazu führt das Multiplizieren von zwei Gleit Komma zahlen niemals zu einer Ausnahme bei einem Überlauf, auch innerhalb eines explizit `checked`-Konstrukts</span><span class="sxs-lookup"><span data-stu-id="20699-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="20699-1460">Außerdem sind andere Operatoren nie von dem Überprüfungs Modus betroffen, ob Standard oder explizit.</span><span class="sxs-lookup"><span data-stu-id="20699-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="20699-1461">Standardwert Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-1461">Default value expressions</span></span>

<span data-ttu-id="20699-1462">Ein Standardwert Ausdruck wird zum Abrufen des Standardwerts ([Standardwerte](variables.md#default-values)) eines Typs verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="20699-1463">In der Regel wird ein Standardwert Ausdruck für Typparameter verwendet, da möglicherweise nicht bekannt ist, ob der Typparameter ein Werttyp oder ein Verweistyp ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="20699-1464">(Es ist keine Konvertierung vom `null`-Literalen in einen Typparameter vorhanden, es sei denn, der Typparameter ist bekannt als Verweistyp.)</span><span class="sxs-lookup"><span data-stu-id="20699-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="20699-1465">Wenn der *Typ* in einer *default_value_expression* zur Laufzeit als Verweistyp ausgewertet wird, wird das Ergebnis `null` in diesen Typ konvertiert.</span><span class="sxs-lookup"><span data-stu-id="20699-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="20699-1466">Wenn der *Typ* in einem *default_value_expression* zur Laufzeit auf einen Werttyp ausgewertet wird, ist das Ergebnis der Standardwert des *value_type*([Standardkonstruktoren](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="20699-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="20699-1467">Bei einem *default_value_expression* handelt es sich um einen konstanten Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)), wenn der Typ ein Verweistyp oder ein Typparameter ist, der bekanntermaßen als Verweistyp ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="20699-1468">Außerdem ist ein *default_value_expression* ein konstanter Ausdruck, wenn der Typ einem der folgenden Werttypen entspricht: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3 , oder ein beliebiger Enumerationstyp.</span><span class="sxs-lookup"><span data-stu-id="20699-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="20699-1469">Nameof-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-1469">Nameof expressions</span></span>

<span data-ttu-id="20699-1470">Ein *nameof_expression* wird verwendet, um den Namen einer Programm Entität als Konstante Zeichenfolge zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="20699-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

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

<span data-ttu-id="20699-1471">Der *named_entity* -Operand ist immer ein Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="20699-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="20699-1472">Da `nameof` kein reserviertes Schlüsselwort ist, ist ein nameof-Ausdruck immer syntaktisch mehrdeutig, wenn ein Aufruf des einfachen namens `nameof` erfolgt.</span><span class="sxs-lookup"><span data-stu-id="20699-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="20699-1473">Aus Kompatibilitätsgründen, wenn eine Namenssuche ([simple names](expressions.md#simple-names)) mit dem Namen `nameof` erfolgreich ist, wird der Ausdruck als *invocation_expression* behandelt, unabhängig davon, ob der Aufruf zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="20699-1474">Andernfalls handelt es sich um einen *nameof_expression*-Wert.</span><span class="sxs-lookup"><span data-stu-id="20699-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="20699-1475">Die Bedeutung des *named_entity* eines *nameof_expression* ist die Bedeutung des Ausdrucks als Ausdruck. Das heißt, entweder als *Simple_name*, als *base_access* oder als *member_access*.</span><span class="sxs-lookup"><span data-stu-id="20699-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="20699-1476">Wenn die in [einfachen Namen](expressions.md#simple-names) und dem Element [Zugriff](expressions.md#member-access) beschriebene Suche jedoch zu einem Fehler führt, weil ein Instanzmember in einem statischen Kontext gefunden wurde, erzeugt ein *nameof_expression* keinen solchen Fehler.</span><span class="sxs-lookup"><span data-stu-id="20699-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="20699-1477">Es ist ein Kompilierzeitfehler für eine *named_entity* , die eine Methoden Gruppe mit einem *type_argument_list*-Wert festlegt.</span><span class="sxs-lookup"><span data-stu-id="20699-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="20699-1478">Es handelt sich um einen Kompilierzeitfehler für ein *named_entity_target* -Typ, der den Typ `dynamic` hat.</span><span class="sxs-lookup"><span data-stu-id="20699-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="20699-1479">Ein *nameof_expression* ist ein konstanter Ausdruck vom Typ `string`, der zur Laufzeit keine Auswirkung hat.</span><span class="sxs-lookup"><span data-stu-id="20699-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="20699-1480">Insbesondere wird die zugehörige *named_entity* nicht ausgewertet, und wird für die konkrete Zuweisungs Analyse ignoriert ([Allgemeine Regeln für einfache Ausdrücke](variables.md#general-rules-for-simple-expressions)).</span><span class="sxs-lookup"><span data-stu-id="20699-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="20699-1481">Der Wert ist der letzte Bezeichner der *named_entity* vor dem optionalen finalen *type_argument_list*, wie folgt transformiert:</span><span class="sxs-lookup"><span data-stu-id="20699-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="20699-1482">Wenn Sie verwendet`@`wird, wird das Präfix "" entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="20699-1483">Jede *unicode_escape_sequence* wird in das entsprechende Unicode-Zeichen transformiert.</span><span class="sxs-lookup"><span data-stu-id="20699-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="20699-1484">Alle *formatting_characters* werden entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="20699-1485">Dabei handelt es sich um dieselben Transformationen, die beim Testen der Gleichheit zwischen bezeichlern in [Bezeichner](lexical-structure.md#identifiers) angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="20699-1486">TODO: Beispiele</span><span class="sxs-lookup"><span data-stu-id="20699-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="20699-1487">Anonyme Methoden Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-1487">Anonymous method expressions</span></span>

<span data-ttu-id="20699-1488">Ein *anonymous_method_expression* -Wert ist eine von zwei Möglichkeiten, eine anonyme Funktion zu definieren.</span><span class="sxs-lookup"><span data-stu-id="20699-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="20699-1489">Diese werden in [Anonyme Funktions Ausdrücke](expressions.md#anonymous-function-expressions)weiter unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="20699-1490">Unäre Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-1490">Unary operators</span></span>

<span data-ttu-id="20699-1491">Die Operatoren `?`, `+`, `-`, `!`, `~`, `++`, `--`, Cast und `await` werden als unäre Operatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

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

<span data-ttu-id="20699-1492">Wenn der Operand eines *unary_expression* den Kompilier Zeittyp `dynamic` aufweist, wird er dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="20699-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="20699-1493">In diesem Fall ist der Kompilier Zeittyp des *unary_expression* `dynamic`, und die unten beschriebene Auflösung erfolgt zur Laufzeit mit dem Lauf Zeittyp des Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="20699-1494">NULL Bedingter Operator</span><span class="sxs-lookup"><span data-stu-id="20699-1494">Null-conditional operator</span></span>

<span data-ttu-id="20699-1495">Der NULL bedingte Operator wendet eine Liste von Vorgängen auf seinen Operanden nur an, wenn dieser Operand nicht NULL ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="20699-1496">Andernfalls ist das Ergebnis der Anwendung des Operators `null`.</span><span class="sxs-lookup"><span data-stu-id="20699-1496">Otherwise the result of applying the operator is `null`.</span></span>

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

<span data-ttu-id="20699-1497">Die Liste der Vorgänge kann Member-und Element Zugriffs Vorgänge (die selbst bedingt NULL sein können) sowie Aufrufe einschließen.</span><span class="sxs-lookup"><span data-stu-id="20699-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="20699-1498">Beispielsweise ist der Ausdruck `a.b?[0]?.c()` ein *null_conditional_expression* mit einem *primary_expression* -`a.b` und *null_conditional_operations* `?[0]` (null bedingter Element Zugriff), `?.c` (null bedingter Member Access) und `()` (Aufruf).</span><span class="sxs-lookup"><span data-stu-id="20699-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="20699-1499">Bei einem *null_conditional_expression* -`E` mit einem *primary_expression* `P` muss `E0` der Ausdruck sein, der durch das textumen Entfernen der führenden `?` aus den einzelnen *null_conditional_operations* von `E` abgerufen wird. haben Sie einen.</span><span class="sxs-lookup"><span data-stu-id="20699-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="20699-1500">Konzeptionell ist `E0` der Ausdruck, der ausgewertet wird, wenn keine der Null-Überprüfungen, die durch die `?`S dargestellt werden, eine `null` findet.</span><span class="sxs-lookup"><span data-stu-id="20699-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="20699-1501">Lassen Sie `E1` auch den Ausdruck aus, der durch das textumen Entfernen der führenden `?` aus nur dem ersten des *null_conditional_operations* in `E` abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="20699-1502">Dies kann zu einem *primären Ausdruck* (falls nur ein `?`) oder zu einem anderen *null_conditional_expression*führen.</span><span class="sxs-lookup"><span data-stu-id="20699-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="20699-1503">Wenn `E` z. b. der Ausdruck `a.b?[0]?.c()` ist, ist `E0` der Ausdruck `a.b[0].c()` und `E1` der Ausdruck `a.b[0]?.c()`.</span><span class="sxs-lookup"><span data-stu-id="20699-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="20699-1504">Wenn `E0` als "Nothing" klassifiziert ist, wird `E` als "Nothing" klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="20699-1505">Andernfalls wird E als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="20699-1506">`E0` und `E1` werden verwendet, um die Bedeutung von `E` zu ermitteln:</span><span class="sxs-lookup"><span data-stu-id="20699-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="20699-1507">Wenn `E` als *statement_expression* auftritt, ist die Bedeutung von `E` mit der Anweisung identisch.</span><span class="sxs-lookup"><span data-stu-id="20699-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="20699-1508">mit der Ausnahme, dass P nur einmal ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="20699-1509">Andernfalls, wenn `E0` als "Nothing" klassifiziert wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="20699-1510">Andernfalls soll `T0` der Typ `E0` sein.</span><span class="sxs-lookup"><span data-stu-id="20699-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="20699-1511">Wenn `T0` ein Typparameter ist, der nicht bekannt ist, dass es sich um einen Verweistyp oder einen nicht auf NULL festleg baren Werttyp handelt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="20699-1512">Wenn `T0` ein Werttyp ist, der keine NULL-Werte zulässt, ist der Typ von `E` `T0?`, und die Bedeutung von `E` ist identisch mit.</span><span class="sxs-lookup"><span data-stu-id="20699-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="20699-1513">mit der Ausnahme, dass "`P`" nur einmal ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="20699-1514">Andernfalls ist der Typ von e t0, und die Bedeutung von e ist identisch mit.</span><span class="sxs-lookup"><span data-stu-id="20699-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="20699-1515">mit der Ausnahme, dass "`P`" nur einmal ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="20699-1516">Wenn `E1` selbst ein *null_conditional_expression*ist, werden diese Regeln erneut angewendet, wobei die Tests für `null` geschachtelt werden, bis keine weiteren `?` vorhanden sind und der Ausdruck bis zum primären Ausdruck `E0` reduziert wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="20699-1517">Wenn z. b. der Ausdruck `a.b?[0]?.c()` als-Anweisungs Ausdruck auftritt, wie in der-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="20699-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="20699-1518">seine Bedeutung ist äquivalent zu:</span><span class="sxs-lookup"><span data-stu-id="20699-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="20699-1519">Das wiederum entspricht:</span><span class="sxs-lookup"><span data-stu-id="20699-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="20699-1520">Mit der Ausnahme, dass die `a.b` und `a.b[0]` nur einmal ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="20699-1521">Wenn Sie in einem Kontext auftritt, in dem ihr Wert verwendet wird, wie in:</span><span class="sxs-lookup"><span data-stu-id="20699-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="20699-1522">und vorausgesetzt, dass der Typ des letzten aufzurufenden aufzurufenden nicht auf NULL festleg baren Werttypen ist, entspricht seine Bedeutung folgenden Werten:</span><span class="sxs-lookup"><span data-stu-id="20699-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="20699-1523">mit der Ausnahme, dass die `a.b` und `a.b[0]` nur einmal ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="20699-1524">NULL bedingte Ausdrücke als Projektions Initialisierer</span><span class="sxs-lookup"><span data-stu-id="20699-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="20699-1525">Ein NULL bedingter Ausdruck ist nur als *member_declarator* in einer *anonymous_object_creation_expression* ([Anonyme Objekt Erstellungs Ausdrücke](expressions.md#anonymous-object-creation-expressions)) zulässig, wenn er mit einem (optional NULL bedingten) Element Zugriff endet.</span><span class="sxs-lookup"><span data-stu-id="20699-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="20699-1526">Diese Anforderung kann wie folgt ausgedrückt werden:</span><span class="sxs-lookup"><span data-stu-id="20699-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="20699-1527">Dies ist ein Sonderfall der obigen Grammatik für *null_conditional_expression* .</span><span class="sxs-lookup"><span data-stu-id="20699-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="20699-1528">Die Produktion für *member_declarator* in [Ausdrücken für die Erstellung anonymer Objekte](expressions.md#anonymous-object-creation-expressions) schließt dann nur *null_conditional_member_access*ein.</span><span class="sxs-lookup"><span data-stu-id="20699-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="20699-1529">NULL bedingte Ausdrücke als Anweisungs Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="20699-1530">Ein NULL bedingter Ausdruck ist nur als *statement_expression* ([Ausdrucks Anweisungen](statements.md#expression-statements)) zulässig, wenn er mit einem Aufruf endet.</span><span class="sxs-lookup"><span data-stu-id="20699-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="20699-1531">Diese Anforderung kann wie folgt ausgedrückt werden:</span><span class="sxs-lookup"><span data-stu-id="20699-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="20699-1532">Dies ist ein Sonderfall der obigen Grammatik für *null_conditional_expression* .</span><span class="sxs-lookup"><span data-stu-id="20699-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="20699-1533">Die Production for *statement_expression* in [Expression-Anweisungen](statements.md#expression-statements) schließt dann nur *null_conditional_invocation_expression*ein.</span><span class="sxs-lookup"><span data-stu-id="20699-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="20699-1534">Unärer Plus-Operator</span><span class="sxs-lookup"><span data-stu-id="20699-1534">Unary plus operator</span></span>

<span data-ttu-id="20699-1535">Bei einem Vorgang in der Form `+x` wird die Überladungs Auflösung des unären Operators ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-1536">Der Operand wird in den Parametertyp des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="20699-1537">Die vordefinierten unären plus Operatoren sind:</span><span class="sxs-lookup"><span data-stu-id="20699-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="20699-1538">Für jeden dieser Operatoren ist das Ergebnis einfach der Wert des Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="20699-1539">Unärer Minusoperator</span><span class="sxs-lookup"><span data-stu-id="20699-1539">Unary minus operator</span></span>

<span data-ttu-id="20699-1540">Bei einem Vorgang in der Form `-x` wird die Überladungs Auflösung des unären Operators ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-1541">Der Operand wird in den Parametertyp des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="20699-1542">Die vordefinierten Negations Operatoren lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="20699-1543">Ganzzahlige Negation:</span><span class="sxs-lookup"><span data-stu-id="20699-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="20699-1544">Das Ergebnis wird durch Subtrahieren von `x` von 0 (null) berechnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="20699-1545">Wenn der Wert von `x` der kleinste darstellbare Wert des Operanden Typs (-2 ^ 31 für `int` oder-2 ^ 63 für `long`) ist, kann die mathematische Negation von `x` innerhalb des Operanden Typs nicht dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1545">If the value of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="20699-1546">Wenn dies innerhalb eines `checked`-Kontexts auftritt, wird ein `System.OverflowException` ausgelöst. Wenn Sie in einem `unchecked`-Kontext auftritt, ist das Ergebnis der Wert des Operanden, und der Überlauf wird nicht gemeldet.</span><span class="sxs-lookup"><span data-stu-id="20699-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="20699-1547">Wenn der Operand des Negations Operators vom Typ `uint` ist, wird er in den Typ `long` konvertiert, und der Ergebnistyp ist `long`.</span><span class="sxs-lookup"><span data-stu-id="20699-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="20699-1548">Eine Ausnahme ist die Regel, die zulässt, dass der `int`-Wert-2147483648 (-2 ^ 31) als dezimales Ganzzahlliteral ([ganzzahlige Literale](lexical-structure.md#integer-literals)) geschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="20699-1549">Wenn der Operand des Negations Operators vom Typ `ulong` ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="20699-1550">Eine Ausnahme ist die Regel, die zulässt, dass der `long`-Wert-9.223.372.036.854.775.808 (-2 ^ 63) als dezimales Ganzzahlliteral ([ganzzahlige Literale](lexical-structure.md#integer-literals)) geschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="20699-1551">Gleit Komma Negation:</span><span class="sxs-lookup"><span data-stu-id="20699-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="20699-1552">Das Ergebnis ist der Wert von `x` mit dem zugehörigen Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="20699-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="20699-1553">Wenn `x` NaN ist, ist das Ergebnis ebenfalls NaN.</span><span class="sxs-lookup"><span data-stu-id="20699-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="20699-1554">Dezimale Negation:</span><span class="sxs-lookup"><span data-stu-id="20699-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="20699-1555">Das Ergebnis wird durch Subtrahieren von `x` von 0 (null) berechnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="20699-1556">Die Dezimale Negation entspricht der Verwendung des unären Minus Operators vom Typ `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="20699-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="20699-1557">Logischer Negationsoperator</span><span class="sxs-lookup"><span data-stu-id="20699-1557">Logical negation operator</span></span>

<span data-ttu-id="20699-1558">Bei einem Vorgang in der Form `!x` wird die Überladungs Auflösung des unären Operators ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-1559">Der Operand wird in den Parametertyp des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="20699-1560">Es ist nur ein vordefinierter logischer Negations Operator vorhanden:</span><span class="sxs-lookup"><span data-stu-id="20699-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="20699-1561">Dieser Operator berechnet die logische Negation des Operanden: Wenn der Operand `true` ist, ist das Ergebnis `false`.</span><span class="sxs-lookup"><span data-stu-id="20699-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="20699-1562">Wenn der Operand `false` ist, ist das Ergebnis `true`.</span><span class="sxs-lookup"><span data-stu-id="20699-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="20699-1563">Bitweiser Komplement Operator</span><span class="sxs-lookup"><span data-stu-id="20699-1563">Bitwise complement operator</span></span>

<span data-ttu-id="20699-1564">Bei einem Vorgang in der Form `~x` wird die Überladungs Auflösung des unären Operators ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-1565">Der Operand wird in den Parametertyp des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="20699-1566">Die vordefinierten bitweisen Komplement Operatoren sind:</span><span class="sxs-lookup"><span data-stu-id="20699-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="20699-1567">Für jeden dieser Operatoren ist das Ergebnis des Vorgangs das bitweise Komplement von `x`.</span><span class="sxs-lookup"><span data-stu-id="20699-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="20699-1568">Jeder Enumerationstyp `E` stellt implizit den folgenden bitweisen Komplement Operator bereit:</span><span class="sxs-lookup"><span data-stu-id="20699-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="20699-1569">Das Ergebnis der Auswertung von "`~x`", wobei "`x`" ein Ausdruck eines Enumerationstyps ist `E` mit einem zugrunde liegenden Typ "`U`" ist identisch mit dem Auswerten von "`(E)(~(U)x)`", mit dem Unterschied, dass die Konvertierung in `E` immer so ausgeführt wird, als ob es sich um einen @no__t Die aktivierten und deaktivierten [Operatoren](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="20699-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="20699-1570">Präfix-Inkrementoperator und Präfix-Dekrementoperator</span><span class="sxs-lookup"><span data-stu-id="20699-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="20699-1571">Der Operand eines Präfix Inkrement-oder dekrementvorgangs muss ein Ausdruck sein, der als Variable, Eigenschafts Zugriff oder Indexerzugriff klassifiziert ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="20699-1572">Das Ergebnis des Vorgangs ist ein Wert desselben Typs wie der Operand.</span><span class="sxs-lookup"><span data-stu-id="20699-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="20699-1573">Wenn der Operand einer Präfix Inkrement-oder Dekrementoperation eine Eigenschaft oder ein Indexer-Zugriff ist, muss die Eigenschaft oder der Indexer sowohl einen `get`-als auch einen `set`-Accessor haben.</span><span class="sxs-lookup"><span data-stu-id="20699-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="20699-1574">Wenn dies nicht der Fall ist, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="20699-1575">Unäre Operator Überladungs Auflösung ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-1576">Vordefinierte Operatoren `++` und `--` sind für die folgenden Typen vorhanden: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, 0, 1, 2, 3 und beliebiger Enumerationstyp.</span><span class="sxs-lookup"><span data-stu-id="20699-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="20699-1577">Der vordefinierte `++`-Operatoren gibt den Wert zurück, der beim Hinzufügen von 1 zum Operanden erzeugt wurde, und die vordefinierten `--`-Operatoren geben den Wert zurück, der durch die Subtraktion von 1 vom Operanden erzeugt</span><span class="sxs-lookup"><span data-stu-id="20699-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="20699-1578">Wenn in einem `checked`-Kontext das Ergebnis dieser Addition oder Subtraktion außerhalb des Bereichs des Ergebnis Typs liegt und der Ergebnistyp ein ganzzahliger Typ oder Enumeration-Typ ist, wird eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="20699-1579">Die Lauf Zeit Verarbeitung eines Präfix Inkrement-oder dekrementvorgangs in der Form `++x` oder `--x` besteht aus den folgenden Schritten:</span><span class="sxs-lookup"><span data-stu-id="20699-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="20699-1580">Wenn `x` als Variable klassifiziert ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="20699-1581">`x` wird ausgewertet, um die Variable zu entwickeln.</span><span class="sxs-lookup"><span data-stu-id="20699-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="20699-1582">Der ausgewählte Operator wird mit dem Wert `x` als Argument aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="20699-1583">Der vom Operator zurückgegebene Wert wird an dem Speicherort gespeichert, der durch die Auswertung von `x` angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="20699-1584">Der vom Operator zurückgegebene Wert wird zum Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="20699-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="20699-1585">Wenn `x` als Eigenschaft oder Indexer-Zugriff klassifiziert ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="20699-1586">Der Instanzausdruck (wenn `x` nicht `static` ist) und die Argumentliste (wenn `x` ein Indexer-Zugriff ist), die `x` zugeordnet ist, werden ausgewertet, und die Ergebnisse werden in den nachfolgenden `get`-und `set`-Accessor-Aufrufen verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="20699-1587">Der `get`-Accessor von `x` wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="20699-1588">Der ausgewählte Operator wird mit dem Wert aufgerufen, der vom `get`-Accessor als Argument zurückgegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="20699-1589">Der `set`-Accessor von `x` wird mit dem Wert aufgerufen, der vom Operator als `value`-Argument zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="20699-1590">Der vom Operator zurückgegebene Wert wird zum Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="20699-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="20699-1591">Die Operatoren `++` und `--` unterstützen auch Postfix-Notation ([postfix-Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="20699-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="20699-1592">In der Regel ist das Ergebnis von `x++` oder `x--` der Wert von `x` vor dem Vorgang, wohingegen das Ergebnis von `++x` oder `--x` der Wert von `x` nach dem Vorgang ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="20699-1593">In beiden Fällen hat `x` selbst denselben Wert nach dem Vorgang.</span><span class="sxs-lookup"><span data-stu-id="20699-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="20699-1594">Eine `operator++`-oder `operator--`-Implementierung kann entweder mithilfe der Postfix-oder Präfix Notation aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="20699-1595">Es ist nicht möglich, separate Operator Implementierungen für die beiden Notationen zu haben.</span><span class="sxs-lookup"><span data-stu-id="20699-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="20699-1596">Umwandlungs Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-1596">Cast expressions</span></span>

<span data-ttu-id="20699-1597">Ein *cast_expression* wird verwendet, um einen Ausdruck explizit in einen bestimmten Typ zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="20699-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="20699-1598">Ein *cast_expression* im Format `(T)E`, wobei `T` ein *Typ* und `E` ein *unary_expression*ist, führt eine explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-conversions)) des Werts `E` in den Typ `T` aus.</span><span class="sxs-lookup"><span data-stu-id="20699-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="20699-1599">Wenn keine explizite Konvertierung von `E` in `T` vorhanden ist, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="20699-1600">Andernfalls ist das Ergebnis der Wert, der von der expliziten Konvertierung erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="20699-1601">Das Ergebnis wird immer als Wert klassifiziert, auch wenn `E` eine Variable bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="20699-1602">Die Grammatik für ein *cast_expression* führt zu bestimmten syntaktischen Mehrdeutigkeiten.</span><span class="sxs-lookup"><span data-stu-id="20699-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="20699-1603">Beispielsweise könnte der Ausdruck `(x)-y` entweder als *cast_expression* (eine Umwandlung von `-y` in den Typ `x`) oder als *additive_expression* in Kombination mit einem *parenthesized_expression* (der den Wert `x - y)` berechnet) interpretiert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="20699-1604">Zum Auflösen von *cast_expression* Mehrdeutigkeiten ist die folgende Regel vorhanden: Eine Sequenz von einem oder mehreren *Token*s ([Leerraum](lexical-structure.md#white-space)), die in Klammern eingeschlossen ist, wird nur als Anfang eines *cast_expression* betrachtet, wenn mindestens einer der folgenden Punkte zutrifft:</span><span class="sxs-lookup"><span data-stu-id="20699-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="20699-1605">Die Reihenfolge der Token ist die korrekte Grammatik für einen *Typ*, jedoch nicht für einen *Ausdruck*.</span><span class="sxs-lookup"><span data-stu-id="20699-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="20699-1606">Die Reihenfolge der Token ist für einen *Typ*eine korrekte Grammatik, und das Token, das unmittelbar auf die schließenden Klammern folgt, ist das Token "`~`", das Token "`!`", das Token "`(`", ein *Bezeichner* ([Escapesequenzen für Unicode-Zeichen](lexical-structure.md#unicode-character-escape-sequences)). ), ein *Literalzeichen* ([Literale](lexical-structure.md#literals)) oder ein beliebiges *Schlüsselwort* ([Schlüsselwörter](lexical-structure.md#keywords)) außer `as` und `is`.</span><span class="sxs-lookup"><span data-stu-id="20699-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="20699-1607">Der Begriff "korrekte Grammatik" oben bedeutet nur, dass die Abfolge der Token der jeweiligen grammatikerischen Produktion entsprechen muss.</span><span class="sxs-lookup"><span data-stu-id="20699-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="20699-1608">Dabei wird insbesondere nicht die tatsächliche Bedeutung von einzelnen Bezeichner berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="20699-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="20699-1609">Wenn z. b. `x` und `y` Bezeichner sind, ist `x.y` korrekt für einen Typ, auch wenn `x.y` keinen Typ angibt.</span><span class="sxs-lookup"><span data-stu-id="20699-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="20699-1610">Aus der disambiguationrule folgt dies: Wenn `x` und `y` Bezeichner sind, sind `(x)y`, `(x)(y)` und `(x)(-y)` *cast_expression*s, aber `(x)-y` ist nicht, auch wenn `x` einen Typ identifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="20699-1611">Wenn `x` jedoch ein Schlüsselwort ist, das einen vordefinierten Typ identifiziert (z. b. `int`), dann sind alle vier Formen *cast_expression*s (da ein solches Schlüsselwort nicht möglicherweise ein Ausdruck allein sein kann).</span><span class="sxs-lookup"><span data-stu-id="20699-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="20699-1612">Erwartungs Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-1612">Await expressions</span></span>

<span data-ttu-id="20699-1613">Der Erwartungs Operator wird verwendet, um die Auswertung der einschließenden Async-Funktion anzuhalten, bis der asynchrone Vorgang, der durch den Operanden dargestellt wird, abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="20699-1614">Ein *await_expression* -Wert ist nur im Text einer Async-Funktion ([Iteratoren](classes.md#iterators)) zulässig.</span><span class="sxs-lookup"><span data-stu-id="20699-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="20699-1615">Innerhalb der nächsten einschließenden asynchronen Funktion tritt möglicherweise ein *await_expression* an diesen Stellen nicht auf:</span><span class="sxs-lookup"><span data-stu-id="20699-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="20699-1616">Innerhalb einer geschachtelten (nicht Async) anonymen Funktion</span><span class="sxs-lookup"><span data-stu-id="20699-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="20699-1617">Innerhalb des Blocks eines *lock_statement*</span><span class="sxs-lookup"><span data-stu-id="20699-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="20699-1618">In einem unsicheren Kontext</span><span class="sxs-lookup"><span data-stu-id="20699-1618">In an unsafe context</span></span>

<span data-ttu-id="20699-1619">Beachten Sie, dass ein *await_expression* nicht an den meisten Stellen innerhalb eines *query_expression*auftreten kann, da diese syntaktisch transformiert werden, um nicht asynchrone Lambda-Ausdrücke zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="20699-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="20699-1620">Innerhalb einer Async-Funktion kann `await` nicht als Bezeichner verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="20699-1621">Daher gibt es keine syntaktische Mehrdeutigkeit zwischen "Erwartung-Ausdrücke" und verschiedenen Ausdrücken mit bezeichmern.</span><span class="sxs-lookup"><span data-stu-id="20699-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="20699-1622">Außerhalb der Async-Funktionen fungiert `await` als normaler Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="20699-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="20699-1623">Der Operand eines *await_expression* wird als ***Aufgabe***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="20699-1624">Sie stellt einen asynchronen Vorgang dar, der zum Zeitpunkt der Auswertung der *await_expression* möglicherweise nicht ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="20699-1625">Der erwartete Operator besteht darin, die Ausführung der einschließenden Async-Funktion anzuhalten, bis die erwartete Aufgabe abgeschlossen ist, und dann das Ergebnis zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="20699-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="20699-1626">Awanutzbare Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-1626">Awaitable expressions</span></span>

<span data-ttu-id="20699-1627">Die Aufgabe eines Erwartungs ***Ausdrucks muss erwartet werden.***</span><span class="sxs-lookup"><span data-stu-id="20699-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="20699-1628">Ein Ausdruck `t` ist zu erwarten, wenn einer der folgenden Punkte Folgendes enthält:</span><span class="sxs-lookup"><span data-stu-id="20699-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="20699-1629">`t` weist den Kompilier Zeittyp `dynamic` auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="20699-1630">`t` verfügt über eine barrierefreie Instanz-oder Erweiterungsmethode, die `GetAwaiter` ohne Parameter und ohne Typparameter und einem Rückgabetyp `A` genannt wird, für den Folgendes gilt:</span><span class="sxs-lookup"><span data-stu-id="20699-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="20699-1631">`A` implementiert die-Schnittstelle `System.Runtime.CompilerServices.INotifyCompletion` (im folgenden als `INotifyCompletion` bekannt).</span><span class="sxs-lookup"><span data-stu-id="20699-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="20699-1632">`A` verfügt über eine barrierefreie, lesbare Instanzeigenschaft `IsCompleted` vom Typ `bool`</span><span class="sxs-lookup"><span data-stu-id="20699-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="20699-1633">`A` verfügt über eine barrierefreie Instanzmethode `GetResult` ohne Parameter und ohne Typparameter.</span><span class="sxs-lookup"><span data-stu-id="20699-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="20699-1634">Der Zweck der `GetAwaiter`-Methode ist das Abrufen eines ***akellners*** für die Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="20699-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="20699-1635">Der Typ `A` wird als ***akellnertyp*** für den Erwartungs Ausdruck bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="20699-1636">Der Zweck der Eigenschaft "`IsCompleted`" besteht darin, festzustellen, ob die Aufgabe bereits beendet ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="20699-1637">Wenn dies der Fall ist, muss die Evaluierung nicht angehalten werden.</span><span class="sxs-lookup"><span data-stu-id="20699-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="20699-1638">Der Zweck der `INotifyCompletion.OnCompleted`-Methode besteht darin, eine "Fortsetzung" für die Aufgabe zu registrieren. d. h. ein Delegat (vom Typ `System.Action`), der nach Abschluss der Aufgabe aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="20699-1639">Der Zweck der `GetResult`-Methode besteht darin, das Ergebnis der Aufgabe zu erhalten, sobald Sie fertig ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="20699-1640">Dieses Ergebnis kann erfolgreich abgeschlossen werden, möglicherweise mit einem Ergebniswert, oder es handelt sich um eine Ausnahme, die von der `GetResult`-Methode ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="20699-1641">Klassifizierung von Erwartungs Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="20699-1641">Classification of await expressions</span></span>

<span data-ttu-id="20699-1642">Der Ausdruck `await t` wird auf die gleiche Weise wie der Ausdruck `(t).GetAwaiter().GetResult()` klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="20699-1643">Wenn also der Rückgabetyp von `GetResult` `void` ist, wird *await_expression* als "Nothing" klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="20699-1644">Wenn Sie einen nicht leeren Rückgabetyp `T` aufweist, wird *await_expression* als Wert des Typs `T` klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="20699-1645">Lauf Zeit Auswertung von Erwartungs Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="20699-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="20699-1646">Zur Laufzeit wird der Ausdruck `await t` wie folgt ausgewertet:</span><span class="sxs-lookup"><span data-stu-id="20699-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="20699-1647">Ein akellner-`a` wird durch Auswerten des Ausdrucks `(t).GetAwaiter()` abgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="20699-1648">Eine `bool`-`b` wird durch Auswerten des Ausdrucks `(a).IsCompleted` abgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="20699-1649">Wenn `b` `false` ist, ist die Auswertung davon abhängig, ob die Schnittstelle von `a` implementiert wird `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (im folgenden als `ICriticalNotifyCompletion` bekannt).</span><span class="sxs-lookup"><span data-stu-id="20699-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="20699-1650">Diese Überprüfung erfolgt zur Bindungs Zeit. Dies ist zur Laufzeit, wenn `a` den Kompilier Zeittyp `dynamic` aufweist, und andernfalls zur Kompilierzeit.</span><span class="sxs-lookup"><span data-stu-id="20699-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="20699-1651">Let `r` gibt den Wiederaufnahme Delegaten ([Iteratoren](classes.md#iterators)) an:</span><span class="sxs-lookup"><span data-stu-id="20699-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="20699-1652">Wenn `a` `ICriticalNotifyCompletion` nicht implementiert, wird der Ausdruck `(a as (INotifyCompletion)).OnCompleted(r)` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="20699-1653">Wenn `a` `ICriticalNotifyCompletion` implementiert, wird der Ausdruck `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="20699-1654">Die Auswertung wird dann angehalten, und die Steuerung wird an den aktuellen Aufrufer der Async-Funktion zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="20699-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="20699-1655">Der Ausdruck `(a).GetResult()` wird ausgewertet, entweder unmittelbar nach (wenn `b` `true` ist) oder nach einem späteren Aufruf des Wiederaufnahme Delegaten (wenn `b` `false` war).</span><span class="sxs-lookup"><span data-stu-id="20699-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="20699-1656">Wenn ein Wert zurückgegeben wird, ist dieser Wert das Ergebnis des *await_expression*.</span><span class="sxs-lookup"><span data-stu-id="20699-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="20699-1657">Andernfalls ist das Ergebnis "Nothing".</span><span class="sxs-lookup"><span data-stu-id="20699-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="20699-1658">Die Implementierung der Schnittstellen Methoden eines akellers `INotifyCompletion.OnCompleted` und `ICriticalNotifyCompletion.UnsafeOnCompleted` sollte bewirken, dass der Delegat `r` höchstens einmal aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="20699-1659">Andernfalls ist das Verhalten der einschließenden Async-Funktion nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="20699-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="20699-1660">Arithmetische operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-1660">Arithmetic operators</span></span>

<span data-ttu-id="20699-1661">Die Operatoren `*`, `/`, `%`, `+` und `-` werden als arithmetische Operatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

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

<span data-ttu-id="20699-1662">Wenn ein Operand eines arithmetischen Operators den Kompilier Zeittyp `dynamic` aufweist, wird der Ausdruck dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="20699-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="20699-1663">In diesem Fall ist der Kompilier Zeittyp des Ausdrucks `dynamic`, und die unten beschriebene Auflösung findet zur Laufzeit statt, wobei der Lauf Zeittyp der Operanden verwendet wird, die den Typ der Kompilierzeit `dynamic` aufweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="20699-1664">Multiplikationsoperator</span><span class="sxs-lookup"><span data-stu-id="20699-1664">Multiplication operator</span></span>

<span data-ttu-id="20699-1665">Bei einem Vorgang im Formular `x * y` wird die binäre Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-1666">Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="20699-1667">Die vordefinierten Multiplikations Operatoren sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="20699-1668">Alle Operatoren berechnen das Produkt `x` und `y`.</span><span class="sxs-lookup"><span data-stu-id="20699-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="20699-1669">Integer-Multiplikation:</span><span class="sxs-lookup"><span data-stu-id="20699-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="20699-1670">Wenn sich das Produkt in einem `checked`-Kontext außerhalb des Bereichs des Ergebnis Typs befindet, wird ein `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="20699-1671">In einem `unchecked`-Kontext werden Überläufe nicht gemeldet, und alle signifikanten höherwertigen Bits außerhalb des Bereichs des Ergebnis Typs werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="20699-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="20699-1672">Gleit Komma Multiplikation:</span><span class="sxs-lookup"><span data-stu-id="20699-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="20699-1673">Das Produkt wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="20699-1674">In der folgenden Tabelle werden die Ergebnisse aller möglichen Kombinationen von nicht NULL Endwerten, Nullen, Infinities und Nan-Werten aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="20699-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="20699-1675">In der Tabelle sind `x` und `y` positive Endwerte.</span><span class="sxs-lookup"><span data-stu-id="20699-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="20699-1676">`z` ist das Ergebnis von `x * y`.</span><span class="sxs-lookup"><span data-stu-id="20699-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="20699-1677">Wenn das Ergebnis für den Zieltyp zu groß ist, `z` unendlich.</span><span class="sxs-lookup"><span data-stu-id="20699-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="20699-1678">Wenn das Ergebnis für den Zieltyp zu klein ist, ist `z` 0 (null).</span><span class="sxs-lookup"><span data-stu-id="20699-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="20699-1679">\+ y</span><span class="sxs-lookup"><span data-stu-id="20699-1679">+y</span></span>   | <span data-ttu-id="20699-1680">-y</span><span class="sxs-lookup"><span data-stu-id="20699-1680">-y</span></span>   | <span data-ttu-id="20699-1681">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1681">+0</span></span>  | <span data-ttu-id="20699-1682">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1682">-0</span></span>  | <span data-ttu-id="20699-1683">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1683">+inf</span></span> | <span data-ttu-id="20699-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1684">-inf</span></span> | <span data-ttu-id="20699-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1685">NaN</span></span> | 
   | <span data-ttu-id="20699-1686">+x</span><span class="sxs-lookup"><span data-stu-id="20699-1686">+x</span></span>   | <span data-ttu-id="20699-1687">\+ z</span><span class="sxs-lookup"><span data-stu-id="20699-1687">+z</span></span>   | <span data-ttu-id="20699-1688">-z</span><span class="sxs-lookup"><span data-stu-id="20699-1688">-z</span></span>   | <span data-ttu-id="20699-1689">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1689">+0</span></span>  | <span data-ttu-id="20699-1690">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1690">-0</span></span>  | <span data-ttu-id="20699-1691">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1691">+inf</span></span> | <span data-ttu-id="20699-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1692">-inf</span></span> | <span data-ttu-id="20699-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1693">NaN</span></span> | 
   | <span data-ttu-id="20699-1694">-x</span><span class="sxs-lookup"><span data-stu-id="20699-1694">-x</span></span>   | <span data-ttu-id="20699-1695">-z</span><span class="sxs-lookup"><span data-stu-id="20699-1695">-z</span></span>   | <span data-ttu-id="20699-1696">\+ z</span><span class="sxs-lookup"><span data-stu-id="20699-1696">+z</span></span>   | <span data-ttu-id="20699-1697">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1697">-0</span></span>  | <span data-ttu-id="20699-1698">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1698">+0</span></span>  | <span data-ttu-id="20699-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1699">-inf</span></span> | <span data-ttu-id="20699-1700">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1700">+inf</span></span> | <span data-ttu-id="20699-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1701">NaN</span></span> | 
   | <span data-ttu-id="20699-1702">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1702">+0</span></span>   | <span data-ttu-id="20699-1703">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1703">+0</span></span>   | <span data-ttu-id="20699-1704">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1704">-0</span></span>   | <span data-ttu-id="20699-1705">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1705">+0</span></span>  | <span data-ttu-id="20699-1706">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1706">-0</span></span>  | <span data-ttu-id="20699-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1707">NaN</span></span>  | <span data-ttu-id="20699-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1708">NaN</span></span>  | <span data-ttu-id="20699-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1709">NaN</span></span> | 
   | <span data-ttu-id="20699-1710">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1710">-0</span></span>   | <span data-ttu-id="20699-1711">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1711">-0</span></span>   | <span data-ttu-id="20699-1712">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1712">+0</span></span>   | <span data-ttu-id="20699-1713">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1713">-0</span></span>  | <span data-ttu-id="20699-1714">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1714">+0</span></span>  | <span data-ttu-id="20699-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1715">NaN</span></span>  | <span data-ttu-id="20699-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1716">NaN</span></span>  | <span data-ttu-id="20699-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1717">NaN</span></span> | 
   | <span data-ttu-id="20699-1718">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1718">+inf</span></span> | <span data-ttu-id="20699-1719">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1719">+inf</span></span> | <span data-ttu-id="20699-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1720">-inf</span></span> | <span data-ttu-id="20699-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1721">NaN</span></span> | <span data-ttu-id="20699-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1722">NaN</span></span> | <span data-ttu-id="20699-1723">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1723">+inf</span></span> | <span data-ttu-id="20699-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1724">-inf</span></span> | <span data-ttu-id="20699-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1725">NaN</span></span> | 
   | <span data-ttu-id="20699-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1726">-inf</span></span> | <span data-ttu-id="20699-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1727">-inf</span></span> | <span data-ttu-id="20699-1728">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1728">+inf</span></span> | <span data-ttu-id="20699-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1729">NaN</span></span> | <span data-ttu-id="20699-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1730">NaN</span></span> | <span data-ttu-id="20699-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1731">-inf</span></span> | <span data-ttu-id="20699-1732">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1732">+inf</span></span> | <span data-ttu-id="20699-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1733">NaN</span></span> | 
   | <span data-ttu-id="20699-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1734">NaN</span></span>  | <span data-ttu-id="20699-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1735">NaN</span></span>  | <span data-ttu-id="20699-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1736">NaN</span></span>  | <span data-ttu-id="20699-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1737">NaN</span></span> | <span data-ttu-id="20699-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1738">NaN</span></span> | <span data-ttu-id="20699-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1739">NaN</span></span>  | <span data-ttu-id="20699-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1740">NaN</span></span>  | <span data-ttu-id="20699-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1741">NaN</span></span> | 

*  <span data-ttu-id="20699-1742">Dezimale Multiplikation:</span><span class="sxs-lookup"><span data-stu-id="20699-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="20699-1743">Wenn der resultierende Wert zu groß ist, um im `decimal`-Format dargestellt zu werden, wird ein `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="20699-1744">Wenn der Ergebniswert zu klein ist, um im `decimal`-Format dargestellt zu werden, ist das Ergebnis 0 (null).</span><span class="sxs-lookup"><span data-stu-id="20699-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="20699-1745">Die Dezimalstellen des Ergebnisses, vor der Rundung, sind die Summe der Skalen der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="20699-1746">Die Dezimal Multiplikation entspricht der Verwendung des Multiplikations Operators vom Typ `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="20699-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="20699-1747">Divisionsoperator</span><span class="sxs-lookup"><span data-stu-id="20699-1747">Division operator</span></span>

<span data-ttu-id="20699-1748">Bei einem Vorgang im Formular `x / y` wird die binäre Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-1749">Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="20699-1750">Die vordefinierten Divisions Operatoren sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="20699-1751">Alle Operatoren berechnen den Quotienten von `x` und `y`.</span><span class="sxs-lookup"><span data-stu-id="20699-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="20699-1752">Ganzzahlige Division:</span><span class="sxs-lookup"><span data-stu-id="20699-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="20699-1753">Wenn der Wert des rechten Operanden 0 (null) ist, wird ein `System.DivideByZeroException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="20699-1754">Die Division rundet das Ergebnis auf 0 (null).</span><span class="sxs-lookup"><span data-stu-id="20699-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="20699-1755">Daher ist der absolute Wert des Ergebnisses die größtmögliche Ganzzahl, die kleiner oder gleich dem absoluten Wert des Quotienten der beiden Operanden ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="20699-1756">Das Ergebnis ist 0 (null) oder positiv, wenn die beiden Operanden das gleiche Vorzeichen aufweisen und 0 (null) oder negativ ist, wenn die beiden Operanden gegenteilige Vorzeichen verfügen.</span><span class="sxs-lookup"><span data-stu-id="20699-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="20699-1757">Wenn der linke Operand der kleinste darstellbare `int`-oder `long`-Wert und der rechte Operand `-1` ist, tritt ein Überlauf auf.</span><span class="sxs-lookup"><span data-stu-id="20699-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="20699-1758">In einem `checked`-Kontext bewirkt dies, dass ein `System.ArithmeticException` (oder eine Unterklasse davon) ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="20699-1759">In einem `unchecked`-Kontext wird die Implementierung definiert, ob ein `System.ArithmeticException` (oder eine Unterklasse) ausgelöst wird, oder der Überlauf wird nicht gemeldet, und der resultierende Wert ist der des linken Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="20699-1760">Gleit Komma Division:</span><span class="sxs-lookup"><span data-stu-id="20699-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="20699-1761">Der Quotienten wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="20699-1762">In der folgenden Tabelle werden die Ergebnisse aller möglichen Kombinationen von nicht NULL Endwerten, Nullen, Infinities und Nan-Werten aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="20699-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="20699-1763">In der Tabelle sind `x` und `y` positive Endwerte.</span><span class="sxs-lookup"><span data-stu-id="20699-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="20699-1764">`z` ist das Ergebnis von `x / y`.</span><span class="sxs-lookup"><span data-stu-id="20699-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="20699-1765">Wenn das Ergebnis für den Zieltyp zu groß ist, `z` unendlich.</span><span class="sxs-lookup"><span data-stu-id="20699-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="20699-1766">Wenn das Ergebnis für den Zieltyp zu klein ist, ist `z` 0 (null).</span><span class="sxs-lookup"><span data-stu-id="20699-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="20699-1767">\+ y</span><span class="sxs-lookup"><span data-stu-id="20699-1767">+y</span></span>   | <span data-ttu-id="20699-1768">-y</span><span class="sxs-lookup"><span data-stu-id="20699-1768">-y</span></span>   | <span data-ttu-id="20699-1769">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1769">+0</span></span>   | <span data-ttu-id="20699-1770">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1770">-0</span></span>   | <span data-ttu-id="20699-1771">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1771">+inf</span></span> | <span data-ttu-id="20699-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1772">-inf</span></span> | <span data-ttu-id="20699-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1773">NaN</span></span>  | 
   | <span data-ttu-id="20699-1774">+x</span><span class="sxs-lookup"><span data-stu-id="20699-1774">+x</span></span>   | <span data-ttu-id="20699-1775">\+ z</span><span class="sxs-lookup"><span data-stu-id="20699-1775">+z</span></span>   | <span data-ttu-id="20699-1776">-z</span><span class="sxs-lookup"><span data-stu-id="20699-1776">-z</span></span>   | <span data-ttu-id="20699-1777">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1777">+inf</span></span> | <span data-ttu-id="20699-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1778">-inf</span></span> | <span data-ttu-id="20699-1779">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1779">+0</span></span>   | <span data-ttu-id="20699-1780">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1780">-0</span></span>   | <span data-ttu-id="20699-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1781">NaN</span></span>  | 
   | <span data-ttu-id="20699-1782">-x</span><span class="sxs-lookup"><span data-stu-id="20699-1782">-x</span></span>   | <span data-ttu-id="20699-1783">-z</span><span class="sxs-lookup"><span data-stu-id="20699-1783">-z</span></span>   | <span data-ttu-id="20699-1784">\+ z</span><span class="sxs-lookup"><span data-stu-id="20699-1784">+z</span></span>   | <span data-ttu-id="20699-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1785">-inf</span></span> | <span data-ttu-id="20699-1786">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1786">+inf</span></span> | <span data-ttu-id="20699-1787">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1787">-0</span></span>   | <span data-ttu-id="20699-1788">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1788">+0</span></span>   | <span data-ttu-id="20699-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1789">NaN</span></span>  | 
   | <span data-ttu-id="20699-1790">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1790">+0</span></span>   | <span data-ttu-id="20699-1791">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1791">+0</span></span>   | <span data-ttu-id="20699-1792">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1792">-0</span></span>   | <span data-ttu-id="20699-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1793">NaN</span></span>  | <span data-ttu-id="20699-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1794">NaN</span></span>  | <span data-ttu-id="20699-1795">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1795">+0</span></span>   | <span data-ttu-id="20699-1796">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1796">-0</span></span>   | <span data-ttu-id="20699-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1797">NaN</span></span>  | 
   | <span data-ttu-id="20699-1798">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1798">-0</span></span>   | <span data-ttu-id="20699-1799">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1799">-0</span></span>   | <span data-ttu-id="20699-1800">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1800">+0</span></span>   | <span data-ttu-id="20699-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1801">NaN</span></span>  | <span data-ttu-id="20699-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1802">NaN</span></span>  | <span data-ttu-id="20699-1803">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1803">-0</span></span>   | <span data-ttu-id="20699-1804">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1804">+0</span></span>   | <span data-ttu-id="20699-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1805">NaN</span></span>  | 
   | <span data-ttu-id="20699-1806">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1806">+inf</span></span> | <span data-ttu-id="20699-1807">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1807">+inf</span></span> | <span data-ttu-id="20699-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1808">-inf</span></span> | <span data-ttu-id="20699-1809">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1809">+inf</span></span> | <span data-ttu-id="20699-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1810">-inf</span></span> | <span data-ttu-id="20699-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1811">NaN</span></span>  | <span data-ttu-id="20699-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1812">NaN</span></span>  | <span data-ttu-id="20699-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1813">NaN</span></span>  | 
   | <span data-ttu-id="20699-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1814">-inf</span></span> | <span data-ttu-id="20699-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1815">-inf</span></span> | <span data-ttu-id="20699-1816">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1816">+inf</span></span> | <span data-ttu-id="20699-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1817">-inf</span></span> | <span data-ttu-id="20699-1818">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1818">+inf</span></span> | <span data-ttu-id="20699-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1819">NaN</span></span>  | <span data-ttu-id="20699-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1820">NaN</span></span>  | <span data-ttu-id="20699-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1821">NaN</span></span>  | 
   | <span data-ttu-id="20699-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1822">NaN</span></span>  | <span data-ttu-id="20699-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1823">NaN</span></span>  | <span data-ttu-id="20699-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1824">NaN</span></span>  | <span data-ttu-id="20699-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1825">NaN</span></span>  | <span data-ttu-id="20699-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1826">NaN</span></span>  | <span data-ttu-id="20699-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1827">NaN</span></span>  | <span data-ttu-id="20699-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1828">NaN</span></span>  | <span data-ttu-id="20699-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1829">NaN</span></span>  | 

*  <span data-ttu-id="20699-1830">Dezimale Division:</span><span class="sxs-lookup"><span data-stu-id="20699-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="20699-1831">Wenn der Wert des rechten Operanden 0 (null) ist, wird ein `System.DivideByZeroException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="20699-1832">Wenn der resultierende Wert zu groß ist, um im `decimal`-Format dargestellt zu werden, wird ein `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="20699-1833">Wenn der Ergebniswert zu klein ist, um im `decimal`-Format dargestellt zu werden, ist das Ergebnis 0 (null).</span><span class="sxs-lookup"><span data-stu-id="20699-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="20699-1834">Die Skala des Ergebnisses ist die kleinste Skala, die ein Ergebnis mit dem nächstgelegenen darstellbaren Dezimalwert dem tatsächlichen mathematischen Ergebnis beibehält.</span><span class="sxs-lookup"><span data-stu-id="20699-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="20699-1835">Die Dezimal Division entspricht der Verwendung des Divisions Operators vom Typ `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="20699-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="20699-1836">Restoperator</span><span class="sxs-lookup"><span data-stu-id="20699-1836">Remainder operator</span></span>

<span data-ttu-id="20699-1837">Bei einem Vorgang im Formular `x % y` wird die binäre Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-1838">Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="20699-1839">Die vordefinierten Rest-Operatoren sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="20699-1840">Die Operatoren berechnen alle den Rest der Division zwischen `x` und `y`.</span><span class="sxs-lookup"><span data-stu-id="20699-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="20699-1841">Ganzzahliger Rest:</span><span class="sxs-lookup"><span data-stu-id="20699-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="20699-1842">Das Ergebnis von `x % y` ist der Wert, der von `x - (x / y) * y` erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="20699-1843">Wenn `y` 0 (null) ist, wird eine `System.DivideByZeroException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="20699-1844">Wenn der linke Operand der kleinste `int`-oder `long`-Wert und der rechte Operand `-1` ist, wird ein `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="20699-1845">In keinem Fall löst `x % y` eine Ausnahme aus, wenn `x / y` keine Ausnahme auslöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="20699-1846">Gleit Komma Restwert:</span><span class="sxs-lookup"><span data-stu-id="20699-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="20699-1847">In der folgenden Tabelle werden die Ergebnisse aller möglichen Kombinationen von nicht NULL Endwerten, Nullen, Infinities und Nan-Werten aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="20699-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="20699-1848">In der Tabelle sind `x` und `y` positive Endwerte.</span><span class="sxs-lookup"><span data-stu-id="20699-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="20699-1849">`z` ist das Ergebnis von `x % y` und wird als `x - n * y` berechnet, wobei `n` die größtmögliche Ganzzahl ist, die kleiner oder gleich `x / y` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="20699-1850">Diese Methode zum Berechnen des Restwerts ist analog zu der für ganzzahlige Operanden verwendeten, unterscheidet sich jedoch von der IEEE 754-Definition (in der `n` die ganze Zahl ist, die `x / y` am nächsten ist).</span><span class="sxs-lookup"><span data-stu-id="20699-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="20699-1851">\+ y</span><span class="sxs-lookup"><span data-stu-id="20699-1851">+y</span></span>   | <span data-ttu-id="20699-1852">-y</span><span class="sxs-lookup"><span data-stu-id="20699-1852">-y</span></span>   | <span data-ttu-id="20699-1853">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1853">+0</span></span>   | <span data-ttu-id="20699-1854">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1854">-0</span></span>   | <span data-ttu-id="20699-1855">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1855">+inf</span></span> | <span data-ttu-id="20699-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1856">-inf</span></span> | <span data-ttu-id="20699-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1857">NaN</span></span>  | 
   | <span data-ttu-id="20699-1858">+x</span><span class="sxs-lookup"><span data-stu-id="20699-1858">+x</span></span>   | <span data-ttu-id="20699-1859">\+ z</span><span class="sxs-lookup"><span data-stu-id="20699-1859">+z</span></span>   | <span data-ttu-id="20699-1860">\+ z</span><span class="sxs-lookup"><span data-stu-id="20699-1860">+z</span></span>   | <span data-ttu-id="20699-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1861">NaN</span></span>  | <span data-ttu-id="20699-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1862">NaN</span></span>  | <span data-ttu-id="20699-1863">w</span><span class="sxs-lookup"><span data-stu-id="20699-1863">x</span></span>    | <span data-ttu-id="20699-1864">w</span><span class="sxs-lookup"><span data-stu-id="20699-1864">x</span></span>    | <span data-ttu-id="20699-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1865">NaN</span></span>  | 
   | <span data-ttu-id="20699-1866">-x</span><span class="sxs-lookup"><span data-stu-id="20699-1866">-x</span></span>   | <span data-ttu-id="20699-1867">-z</span><span class="sxs-lookup"><span data-stu-id="20699-1867">-z</span></span>   | <span data-ttu-id="20699-1868">-z</span><span class="sxs-lookup"><span data-stu-id="20699-1868">-z</span></span>   | <span data-ttu-id="20699-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1869">NaN</span></span>  | <span data-ttu-id="20699-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1870">NaN</span></span>  | <span data-ttu-id="20699-1871">-x</span><span class="sxs-lookup"><span data-stu-id="20699-1871">-x</span></span>   | <span data-ttu-id="20699-1872">-x</span><span class="sxs-lookup"><span data-stu-id="20699-1872">-x</span></span>   | <span data-ttu-id="20699-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1873">NaN</span></span>  | 
   | <span data-ttu-id="20699-1874">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1874">+0</span></span>   | <span data-ttu-id="20699-1875">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1875">+0</span></span>   | <span data-ttu-id="20699-1876">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1876">+0</span></span>   | <span data-ttu-id="20699-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1877">NaN</span></span>  | <span data-ttu-id="20699-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1878">NaN</span></span>  | <span data-ttu-id="20699-1879">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1879">+0</span></span>   | <span data-ttu-id="20699-1880">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1880">+0</span></span>   | <span data-ttu-id="20699-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1881">NaN</span></span>  | 
   | <span data-ttu-id="20699-1882">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1882">-0</span></span>   | <span data-ttu-id="20699-1883">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1883">-0</span></span>   | <span data-ttu-id="20699-1884">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1884">-0</span></span>   | <span data-ttu-id="20699-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1885">NaN</span></span>  | <span data-ttu-id="20699-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1886">NaN</span></span>  | <span data-ttu-id="20699-1887">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1887">-0</span></span>   | <span data-ttu-id="20699-1888">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1888">-0</span></span>   | <span data-ttu-id="20699-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1889">NaN</span></span>  | 
   | <span data-ttu-id="20699-1890">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1890">+inf</span></span> | <span data-ttu-id="20699-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1891">NaN</span></span>  | <span data-ttu-id="20699-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1892">NaN</span></span>  | <span data-ttu-id="20699-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1893">NaN</span></span>  | <span data-ttu-id="20699-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1894">NaN</span></span>  | <span data-ttu-id="20699-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1895">NaN</span></span>  | <span data-ttu-id="20699-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1896">NaN</span></span>  | <span data-ttu-id="20699-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1897">NaN</span></span>  | 
   | <span data-ttu-id="20699-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1898">-inf</span></span> | <span data-ttu-id="20699-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1899">NaN</span></span>  | <span data-ttu-id="20699-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1900">NaN</span></span>  | <span data-ttu-id="20699-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1901">NaN</span></span>  | <span data-ttu-id="20699-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1902">NaN</span></span>  | <span data-ttu-id="20699-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1903">NaN</span></span>  | <span data-ttu-id="20699-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1904">NaN</span></span>  | <span data-ttu-id="20699-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1905">NaN</span></span>  | 
   | <span data-ttu-id="20699-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1906">NaN</span></span>  | <span data-ttu-id="20699-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1907">NaN</span></span>  | <span data-ttu-id="20699-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1908">NaN</span></span>  | <span data-ttu-id="20699-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1909">NaN</span></span>  | <span data-ttu-id="20699-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1910">NaN</span></span>  | <span data-ttu-id="20699-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1911">NaN</span></span>  | <span data-ttu-id="20699-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1912">NaN</span></span>  | <span data-ttu-id="20699-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1913">NaN</span></span>  | 

*  <span data-ttu-id="20699-1914">Dezimal Restwert:</span><span class="sxs-lookup"><span data-stu-id="20699-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="20699-1915">Wenn der Wert des rechten Operanden 0 (null) ist, wird ein `System.DivideByZeroException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="20699-1916">Die Dezimalstellen des Ergebnisses, vor der Rundung, sind die größere der Skalen der beiden Operanden, und das Vorzeichen des Ergebnisses, wenn ungleich NULL, entspricht dem Wert von `x`.</span><span class="sxs-lookup"><span data-stu-id="20699-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="20699-1917">Der Dezimal Rest entspricht der Verwendung des Rest-Operators vom Typ "`System.Decimal`".</span><span class="sxs-lookup"><span data-stu-id="20699-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="20699-1918">Additionsoperator</span><span class="sxs-lookup"><span data-stu-id="20699-1918">Addition operator</span></span>

<span data-ttu-id="20699-1919">Bei einem Vorgang im Formular `x + y` wird die binäre Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-1920">Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="20699-1921">Die vordefinierten Additions Operatoren sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="20699-1922">Für numerische und Enumerationstypen berechnen die vordefinierten Additions Operatoren die Summe der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="20699-1923">Wenn ein oder beide Operanden vom Typ Zeichenfolge sind, verketten die vordefinierten Additions Operatoren die Zeichen folgen Darstellung der Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="20699-1924">Ganzzahlige Addition:</span><span class="sxs-lookup"><span data-stu-id="20699-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="20699-1925">Wenn sich in einem `checked`-Kontext die Summe außerhalb des Bereichs des Ergebnis Typs befindet, wird eine `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="20699-1926">In einem `unchecked`-Kontext werden Überläufe nicht gemeldet, und alle signifikanten höherwertigen Bits außerhalb des Bereichs des Ergebnis Typs werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="20699-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="20699-1927">Gleit Komma Addition:</span><span class="sxs-lookup"><span data-stu-id="20699-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="20699-1928">Die Summe wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="20699-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="20699-1929">In der folgenden Tabelle werden die Ergebnisse aller möglichen Kombinationen von nicht NULL Endwerten, Nullen, Infinities und Nan-Werten aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="20699-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="20699-1930">In der Tabelle sind `x` und `y` nicht NULL endliche Werte, und `z` ist das Ergebnis von `x + y`.</span><span class="sxs-lookup"><span data-stu-id="20699-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="20699-1931">Wenn `x` und `y` dieselbe Größe, aber gegen übergesetzte Vorzeichen aufweisen, ist `z` positiv 0 (null).</span><span class="sxs-lookup"><span data-stu-id="20699-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="20699-1932">Wenn `x + y` zu groß ist, um im Zieltyp darzustellen, ist `z` unendlich mit demselben Vorzeichen wie `x + y`.</span><span class="sxs-lookup"><span data-stu-id="20699-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="20699-1933">y</span><span class="sxs-lookup"><span data-stu-id="20699-1933">y</span></span>    | <span data-ttu-id="20699-1934">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1934">+0</span></span>   | <span data-ttu-id="20699-1935">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1935">-0</span></span>   | <span data-ttu-id="20699-1936">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1936">+inf</span></span> | <span data-ttu-id="20699-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1937">-inf</span></span> | <span data-ttu-id="20699-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1938">NaN</span></span>  | 
   | <span data-ttu-id="20699-1939">w</span><span class="sxs-lookup"><span data-stu-id="20699-1939">x</span></span>    | <span data-ttu-id="20699-1940">z</span><span class="sxs-lookup"><span data-stu-id="20699-1940">z</span></span>    | <span data-ttu-id="20699-1941">w</span><span class="sxs-lookup"><span data-stu-id="20699-1941">x</span></span>    | <span data-ttu-id="20699-1942">w</span><span class="sxs-lookup"><span data-stu-id="20699-1942">x</span></span>    | <span data-ttu-id="20699-1943">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1943">+inf</span></span> | <span data-ttu-id="20699-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1944">-inf</span></span> | <span data-ttu-id="20699-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1945">NaN</span></span>  | 
   | <span data-ttu-id="20699-1946">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1946">+0</span></span>   | <span data-ttu-id="20699-1947">y</span><span class="sxs-lookup"><span data-stu-id="20699-1947">y</span></span>    | <span data-ttu-id="20699-1948">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1948">+0</span></span>   | <span data-ttu-id="20699-1949">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1949">+0</span></span>   | <span data-ttu-id="20699-1950">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1950">+inf</span></span> | <span data-ttu-id="20699-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1951">-inf</span></span> | <span data-ttu-id="20699-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1952">NaN</span></span>  | 
   | <span data-ttu-id="20699-1953">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1953">-0</span></span>   | <span data-ttu-id="20699-1954">y</span><span class="sxs-lookup"><span data-stu-id="20699-1954">y</span></span>    | <span data-ttu-id="20699-1955">+0</span><span class="sxs-lookup"><span data-stu-id="20699-1955">+0</span></span>   | <span data-ttu-id="20699-1956">-0</span><span class="sxs-lookup"><span data-stu-id="20699-1956">-0</span></span>   | <span data-ttu-id="20699-1957">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1957">+inf</span></span> | <span data-ttu-id="20699-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1958">-inf</span></span> | <span data-ttu-id="20699-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1959">NaN</span></span>  | 
   | <span data-ttu-id="20699-1960">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1960">+inf</span></span> | <span data-ttu-id="20699-1961">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1961">+inf</span></span> | <span data-ttu-id="20699-1962">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1962">+inf</span></span> | <span data-ttu-id="20699-1963">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1963">+inf</span></span> | <span data-ttu-id="20699-1964">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-1964">+inf</span></span> | <span data-ttu-id="20699-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1965">NaN</span></span>  | <span data-ttu-id="20699-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1966">NaN</span></span>  | 
   | <span data-ttu-id="20699-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1967">-inf</span></span> | <span data-ttu-id="20699-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1968">-inf</span></span> | <span data-ttu-id="20699-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1969">-inf</span></span> | <span data-ttu-id="20699-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1970">-inf</span></span> | <span data-ttu-id="20699-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1971">NaN</span></span>  | <span data-ttu-id="20699-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-1972">-inf</span></span> | <span data-ttu-id="20699-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1973">NaN</span></span>  | 
   | <span data-ttu-id="20699-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1974">NaN</span></span>  | <span data-ttu-id="20699-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1975">NaN</span></span>  | <span data-ttu-id="20699-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1976">NaN</span></span>  | <span data-ttu-id="20699-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1977">NaN</span></span>  | <span data-ttu-id="20699-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1978">NaN</span></span>  | <span data-ttu-id="20699-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1979">NaN</span></span>  | <span data-ttu-id="20699-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-1980">NaN</span></span>  | 

*  <span data-ttu-id="20699-1981">Dezimal Addition:</span><span class="sxs-lookup"><span data-stu-id="20699-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="20699-1982">Wenn der resultierende Wert zu groß ist, um im `decimal`-Format dargestellt zu werden, wird ein `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="20699-1983">Die Dezimalstellen des Ergebnisses, vor der Rundung, sind die größere der Skalen der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="20699-1984">Die Dezimal Addition entspricht der Verwendung des Additions Operators vom Typ `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="20699-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="20699-1985">Enumerationsaddition.</span><span class="sxs-lookup"><span data-stu-id="20699-1985">Enumeration addition.</span></span> <span data-ttu-id="20699-1986">Jeder Enumerationstyp stellt implizit die folgenden vordefinierten Operatoren bereit, wobei `E` den Enumerationstyp und `U` der zugrunde liegende Typ von `E` ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="20699-1987">Zur Laufzeit werden diese Operatoren genau wie `(E)((U)x + (U)y)` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="20699-1988">Verkettung von Zeichen folgen:</span><span class="sxs-lookup"><span data-stu-id="20699-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="20699-1989">Diese über Ladungen des binären `+`-Operators führen eine Zeichen folgen Verkettung durch.</span><span class="sxs-lookup"><span data-stu-id="20699-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="20699-1990">Wenn ein Operand der Zeichen folgen Verkettung `null` ist, wird eine leere Zeichenfolge ersetzt.</span><span class="sxs-lookup"><span data-stu-id="20699-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="20699-1991">Andernfalls wird jedes nicht-Zeichen folgen Argument in seine Zeichen folgen Darstellung konvertiert, indem die vom Typ `object` geerbte virtuelle `ToString`-Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="20699-1992">Wenn `ToString` `null` zurückgibt, wird eine leere Zeichenfolge ersetzt.</span><span class="sxs-lookup"><span data-stu-id="20699-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

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

   Das Ergebnis des Zeichenfolgenverkettungs-Operators ist eine Zeichenfolge, die aus den Zeichen des linken Operanden besteht, gefolgt von den Zeichen des rechten Operanden. Der Operator für die Zeichen folgen Verkettung gibt nie einen `null`-Wert zurück. <span data-ttu-id="20699-1995">Eine `System.OutOfMemoryException` kann ausgelöst werden, wenn nicht genügend Arbeitsspeicher zur Verfügung steht, um die resultierende Zeichenfolge zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="20699-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="20699-1996">Delegatkombination.</span><span class="sxs-lookup"><span data-stu-id="20699-1996">Delegate combination.</span></span> <span data-ttu-id="20699-1997">Jeder Delegattyp stellt implizit den folgenden vordefinierten Operator bereit, wobei `D` der Delegattyp ist:</span><span class="sxs-lookup"><span data-stu-id="20699-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="20699-1998">Der binäre `+`-Operator führt eine Delegatkombination aus, wenn beide Operanden einen Delegattyp haben `D`.</span><span class="sxs-lookup"><span data-stu-id="20699-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="20699-1999">(Wenn die Operanden unterschiedliche Delegattypen aufweisen, tritt ein Bindungs Zeitfehler auf.) Wenn der erste Operand `null` ist, ist das Ergebnis der Operation der Wert des zweiten Operanden (auch wenn dies ebenfalls `null` ist).</span><span class="sxs-lookup"><span data-stu-id="20699-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="20699-2000">Andernfalls, wenn der zweite Operand `null` ist, ist das Ergebnis der Operation der Wert des ersten Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="20699-2001">Andernfalls ist das Ergebnis des Vorgangs eine neue Delegatinstanz, die beim Aufrufen den ersten Operanden aufruft und dann den zweiten Operanden aufruft.</span><span class="sxs-lookup"><span data-stu-id="20699-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="20699-2002">Beispiele für die Kombination von Delegaten finden Sie [unter Subtraktions Operator](expressions.md#subtraction-operator) und [Delegataufruf](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="20699-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="20699-2003">Da `System.Delegate` kein Delegattyp ist, ist `operator` @ no__t-2 nicht dafür definiert.</span><span class="sxs-lookup"><span data-stu-id="20699-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="20699-2004">Subtraktionsoperator</span><span class="sxs-lookup"><span data-stu-id="20699-2004">Subtraction operator</span></span>

<span data-ttu-id="20699-2005">Bei einem Vorgang im Formular `x - y` wird die binäre Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-2006">Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="20699-2007">Die vordefinierten Subtraktions Operatoren sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="20699-2008">Die Operatoren subtrahieren `y` von `x`.</span><span class="sxs-lookup"><span data-stu-id="20699-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="20699-2009">Ganzzahlige Subtraktion:</span><span class="sxs-lookup"><span data-stu-id="20699-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="20699-2010">Wenn sich in einem `checked`-Kontext der Unterschied außerhalb des Bereichs des Ergebnis Typs befindet, wird ein `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="20699-2011">In einem `unchecked`-Kontext werden Überläufe nicht gemeldet, und alle signifikanten höherwertigen Bits außerhalb des Bereichs des Ergebnis Typs werden verworfen.</span><span class="sxs-lookup"><span data-stu-id="20699-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="20699-2012">Gleit Komma Subtraktion:</span><span class="sxs-lookup"><span data-stu-id="20699-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="20699-2013">Der Unterschied wird gemäß den Regeln der IEEE 754-Arithmetik berechnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="20699-2014">In der folgenden Tabelle werden die Ergebnisse aller möglichen Kombinationen von nicht NULL Endwerten, Nullen, Infinities und Nane aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="20699-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="20699-2015">In der Tabelle sind `x` und `y` nicht NULL endliche Werte, und `z` ist das Ergebnis von `x - y`.</span><span class="sxs-lookup"><span data-stu-id="20699-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="20699-2016">Wenn `x` und `y` gleich sind, ist `z` 0 (null).</span><span class="sxs-lookup"><span data-stu-id="20699-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="20699-2017">Wenn `x - y` zu groß ist, um im Zieltyp darzustellen, ist `z` unendlich mit demselben Vorzeichen wie `x - y`.</span><span class="sxs-lookup"><span data-stu-id="20699-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | <span data-ttu-id="20699-2018">y</span><span class="sxs-lookup"><span data-stu-id="20699-2018">y</span></span>    | <span data-ttu-id="20699-2019">+0</span><span class="sxs-lookup"><span data-stu-id="20699-2019">+0</span></span>   | <span data-ttu-id="20699-2020">-0</span><span class="sxs-lookup"><span data-stu-id="20699-2020">-0</span></span>   | <span data-ttu-id="20699-2021">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-2021">+inf</span></span> | <span data-ttu-id="20699-2022">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-2022">-inf</span></span> | <span data-ttu-id="20699-2023">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2023">NaN</span></span> | 
   | <span data-ttu-id="20699-2024">w</span><span class="sxs-lookup"><span data-stu-id="20699-2024">x</span></span>    | <span data-ttu-id="20699-2025">z</span><span class="sxs-lookup"><span data-stu-id="20699-2025">z</span></span>    | <span data-ttu-id="20699-2026">w</span><span class="sxs-lookup"><span data-stu-id="20699-2026">x</span></span>    | <span data-ttu-id="20699-2027">w</span><span class="sxs-lookup"><span data-stu-id="20699-2027">x</span></span>    | <span data-ttu-id="20699-2028">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-2028">-inf</span></span> | <span data-ttu-id="20699-2029">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-2029">+inf</span></span> | <span data-ttu-id="20699-2030">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2030">NaN</span></span> | 
   | <span data-ttu-id="20699-2031">+0</span><span class="sxs-lookup"><span data-stu-id="20699-2031">+0</span></span>   | <span data-ttu-id="20699-2032">-y</span><span class="sxs-lookup"><span data-stu-id="20699-2032">-y</span></span>   | <span data-ttu-id="20699-2033">+0</span><span class="sxs-lookup"><span data-stu-id="20699-2033">+0</span></span>   | <span data-ttu-id="20699-2034">+0</span><span class="sxs-lookup"><span data-stu-id="20699-2034">+0</span></span>   | <span data-ttu-id="20699-2035">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-2035">-inf</span></span> | <span data-ttu-id="20699-2036">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-2036">+inf</span></span> | <span data-ttu-id="20699-2037">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2037">NaN</span></span> | 
   | <span data-ttu-id="20699-2038">-0</span><span class="sxs-lookup"><span data-stu-id="20699-2038">-0</span></span>   | <span data-ttu-id="20699-2039">-y</span><span class="sxs-lookup"><span data-stu-id="20699-2039">-y</span></span>   | <span data-ttu-id="20699-2040">-0</span><span class="sxs-lookup"><span data-stu-id="20699-2040">-0</span></span>   | <span data-ttu-id="20699-2041">+0</span><span class="sxs-lookup"><span data-stu-id="20699-2041">+0</span></span>   | <span data-ttu-id="20699-2042">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-2042">-inf</span></span> | <span data-ttu-id="20699-2043">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-2043">+inf</span></span> | <span data-ttu-id="20699-2044">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2044">NaN</span></span> | 
   | <span data-ttu-id="20699-2045">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-2045">+inf</span></span> | <span data-ttu-id="20699-2046">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-2046">+inf</span></span> | <span data-ttu-id="20699-2047">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-2047">+inf</span></span> | <span data-ttu-id="20699-2048">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-2048">+inf</span></span> | <span data-ttu-id="20699-2049">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2049">NaN</span></span>  | <span data-ttu-id="20699-2050">+inf</span><span class="sxs-lookup"><span data-stu-id="20699-2050">+inf</span></span> | <span data-ttu-id="20699-2051">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2051">NaN</span></span> | 
   | <span data-ttu-id="20699-2052">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-2052">-inf</span></span> | <span data-ttu-id="20699-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-2053">-inf</span></span> | <span data-ttu-id="20699-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-2054">-inf</span></span> | <span data-ttu-id="20699-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-2055">-inf</span></span> | <span data-ttu-id="20699-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="20699-2056">-inf</span></span> | <span data-ttu-id="20699-2057">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2057">NaN</span></span>  | <span data-ttu-id="20699-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2058">NaN</span></span> | 
   | <span data-ttu-id="20699-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2059">NaN</span></span>  | <span data-ttu-id="20699-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2060">NaN</span></span>  | <span data-ttu-id="20699-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2061">NaN</span></span>  | <span data-ttu-id="20699-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2062">NaN</span></span>  | <span data-ttu-id="20699-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2063">NaN</span></span>  | <span data-ttu-id="20699-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2064">NaN</span></span>  | <span data-ttu-id="20699-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="20699-2065">NaN</span></span> | 

*  <span data-ttu-id="20699-2066">Dezimale Subtraktion:</span><span class="sxs-lookup"><span data-stu-id="20699-2066">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="20699-2067">Wenn der resultierende Wert zu groß ist, um im `decimal`-Format dargestellt zu werden, wird ein `System.OverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-2067">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="20699-2068">Die Dezimalstellen des Ergebnisses, vor der Rundung, sind die größere der Skalen der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-2068">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="20699-2069">Die dezimale Subtraktion entspricht der Verwendung des Subtraktions Operators vom Typ `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="20699-2069">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="20699-2070">Enumerationssubtraktion.</span><span class="sxs-lookup"><span data-stu-id="20699-2070">Enumeration subtraction.</span></span> <span data-ttu-id="20699-2071">Jeder Enumerationstyp stellt implizit den folgenden vordefinierten Operator bereit, wobei `E` den Enumerationstyp und `U` der zugrunde liegende Typ von `E` ist:</span><span class="sxs-lookup"><span data-stu-id="20699-2071">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="20699-2072">Dieser Operator wird genau wie `(U)((U)x - (U)y)` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-2072">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="20699-2073">Mit anderen Worten: der-Operator berechnet den Unterschied zwischen den Ordinalwerten von `x` und `y`, und der Ergebnistyp ist der zugrunde liegende Typ der Enumeration.</span><span class="sxs-lookup"><span data-stu-id="20699-2073">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="20699-2074">Dieser Operator wird genau wie `(E)((U)x - y)` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-2074">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="20699-2075">Mit anderen Worten, der Operator subtrahiert einen Wert vom zugrunde liegenden Typ der Enumeration und gibt einen Wert der-Enumeration aus.</span><span class="sxs-lookup"><span data-stu-id="20699-2075">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="20699-2076">Entfernen von Delegaten.</span><span class="sxs-lookup"><span data-stu-id="20699-2076">Delegate removal.</span></span> <span data-ttu-id="20699-2077">Jeder Delegattyp stellt implizit den folgenden vordefinierten Operator bereit, wobei `D` der Delegattyp ist:</span><span class="sxs-lookup"><span data-stu-id="20699-2077">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="20699-2078">Der binäre `-`-Operator führt die Delegatentfernung aus, wenn beide Operanden einen Delegattyp haben `D`.</span><span class="sxs-lookup"><span data-stu-id="20699-2078">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="20699-2079">Wenn die Operanden unterschiedliche Delegattypen aufweisen, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2079">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="20699-2080">Ist der erste Operand `null`, ist das Ergebnis des Vorgangs `null`.</span><span class="sxs-lookup"><span data-stu-id="20699-2080">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="20699-2081">Andernfalls, wenn der zweite Operand `null` ist, ist das Ergebnis der Operation der Wert des ersten Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-2081">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="20699-2082">Andernfalls stellen beide Operanden Aufruf Listen ([Delegatdeklarationen](delegates.md#delegate-declarations)) dar, die mindestens einen Eintrag enthalten, und das Ergebnis ist eine neue Aufruf Liste, die aus der ersten Operanden Liste besteht, in der die Einträge des zweiten Operanden aus der Liste entfernt wurden. die Liste der Operanden ist eine ordnungsgemäße zusammenhängende unterste der ersten.</span><span class="sxs-lookup"><span data-stu-id="20699-2082">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="20699-2083">(Um die Übereinstimmung zu ermitteln, werden die entsprechenden Einträge als für den Delegat-Gleichheits Operator (Delegat-[Gleichheits Operatoren](expressions.md#delegate-equality-operators)) verglichen.) Andernfalls ist das Ergebnis der Wert des linken Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-2083">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="20699-2084">Die Listen der Operanden werden im Prozess nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="20699-2084">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="20699-2085">Wenn die Liste des zweiten Operanden mit mehreren Unterlisten zusammenhängender Einträge in der Liste der ersten Operanden übereinstimmt, wird die am weitesten rechts gerichtete unter Liste von zusammenhängenden Einträgen entfernt.</span><span class="sxs-lookup"><span data-stu-id="20699-2085">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="20699-2086">Sollte durch die Entfernung eine leere Liste entstehen, ist das Ergebnis `null`.</span><span class="sxs-lookup"><span data-stu-id="20699-2086">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="20699-2087">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="20699-2087">For example:</span></span>

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

## <a name="shift-operators"></a><span data-ttu-id="20699-2088">Schiebeoperatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2088">Shift operators</span></span>

<span data-ttu-id="20699-2089">Die Operatoren `<<` und `>>` werden verwendet, um Bitverschiebungs Vorgänge auszuführen.</span><span class="sxs-lookup"><span data-stu-id="20699-2089">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="20699-2090">Wenn ein Operand eines *shift_expression* den Kompilier Zeittyp `dynamic` aufweist, wird der Ausdruck dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="20699-2090">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="20699-2091">In diesem Fall ist der Kompilier Zeittyp des Ausdrucks `dynamic`, und die unten beschriebene Auflösung findet zur Laufzeit statt, wobei der Lauf Zeittyp der Operanden verwendet wird, die den Typ der Kompilierzeit `dynamic` aufweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-2091">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="20699-2092">Bei einem Vorgang in der Form `x << count` oder `x >> count` wird die binäre Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-2092">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-2093">Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-2093">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="20699-2094">Beim Deklarieren eines überladenen Verschiebungs Operators muss der Typ des ersten Operanden immer die Klasse oder Struktur sein, die die Operator Deklaration enthält, und der Typ des zweiten Operanden muss immer `int` sein.</span><span class="sxs-lookup"><span data-stu-id="20699-2094">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="20699-2095">Die vordefinierten Shift-Operatoren sind unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="20699-2095">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="20699-2096">Nach links verschieben:</span><span class="sxs-lookup"><span data-stu-id="20699-2096">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="20699-2097">Der `<<`-Operator verschiebt `x` nach links um eine Anzahl von Bits, die wie unten beschrieben berechnet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2097">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="20699-2098">Die höherwertigen Bits außerhalb des Bereichs des Ergebnis Typs von `x` werden verworfen, die restlichen Bits werden nach links verschoben, und die unteren leeren Bitpositionen werden auf 0 (null) festgelegt.</span><span class="sxs-lookup"><span data-stu-id="20699-2098">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="20699-2099">Nach rechts verschieben:</span><span class="sxs-lookup"><span data-stu-id="20699-2099">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="20699-2100">Der `>>`-Operator verschiebt `x` direkt um eine Anzahl von Bits, die wie unten beschrieben berechnet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2100">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="20699-2101">Wenn `x` vom Typ `int` oder `long` ist, werden die nieder wertigen Bits von `x` verworfen, die restlichen Bits werden nach rechts verschoben, und die hohen Positionen leeren Bitpositionen werden auf 0 (null) festgelegt, wenn `x` nicht negativ ist, und auf einen Wert festgelegt, wenn `x` negativ ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2101">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="20699-2102">Wenn `x` vom Typ `uint` oder `ulong` ist, werden die nieder wertigen Bits von `x` verworfen, die restlichen Bits werden nach rechts verschoben, und die hohen Reihenfolge leeren Bitpositionen werden auf 0 (null) festgelegt.</span><span class="sxs-lookup"><span data-stu-id="20699-2102">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="20699-2103">Für die vordefinierten Operatoren wird die Anzahl der zu Verschiebungs Bits wie folgt berechnet:</span><span class="sxs-lookup"><span data-stu-id="20699-2103">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="20699-2104">Wenn der Typ von `x` `int` oder `uint` ist, wird die UMSCHALT Anzahl durch die nieder wertigen fünf Bits von `count` angegeben.</span><span class="sxs-lookup"><span data-stu-id="20699-2104">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="20699-2105">Mit anderen Worten: die UMSCHALT Anzahl wird aus `count & 0x1F` berechnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2105">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="20699-2106">Wenn der Typ `x` `long` oder `ulong` ist, wird die UMSCHALT Anzahl durch die nieder wertigen sechs Bits von `count` angegeben.</span><span class="sxs-lookup"><span data-stu-id="20699-2106">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="20699-2107">Mit anderen Worten: die UMSCHALT Anzahl wird aus `count & 0x3F` berechnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2107">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="20699-2108">Wenn die resultierende Verschiebungs Anzahl 0 (null) ist, geben die Schiebe Operatoren einfach den Wert `x` zurück.</span><span class="sxs-lookup"><span data-stu-id="20699-2108">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="20699-2109">Verschiebungs Vorgänge verursachen niemals Überläufe und erzeugen dieselben Ergebnisse in `checked`-und `unchecked`-Kontexten.</span><span class="sxs-lookup"><span data-stu-id="20699-2109">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="20699-2110">Wenn der linke Operand des `>>`-Operators einen ganzzahligen Typ mit Vorzeichen hat, führt der Operator eine arithmetische Verschiebung nach rechts aus, bei der der Wert des signifikantesten Bits (das Signier Bit) des Operanden an die Positionen für die hohe Reihenfolge an leeren Bitpositionen weitergegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2110">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="20699-2111">Wenn der linke Operand des `>>`-Operators einen ganzzahligen Typ ohne Vorzeichen aufweist, führt der Operator eine logische Schiebe nach rechts aus, bei der die Positionen für die hohe Reihenfolge von leeren Bitpositionen immer auf NULL festgelegt sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2111">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="20699-2112">Explizite Umwandlungen können verwendet werden, um den umgekehrten Vorgang von auszuführen, der vom Operanden-Typ abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2112">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="20699-2113">Wenn `x` z. b. eine Variable vom Typ "`int`" ist, führt der Vorgang "`unchecked((int)((uint)x >> y))`" eine logische Verschiebung rechts von "`x`" aus.</span><span class="sxs-lookup"><span data-stu-id="20699-2113">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="20699-2114">Relationale und Typtestoperatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2114">Relational and type-testing operators</span></span>

<span data-ttu-id="20699-2115">Die Operatoren `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` und `as` werden als relationale und Typtest Operatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2115">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

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

<span data-ttu-id="20699-2116">Der `is`-Operator wird im [is-Operator](expressions.md#the-is-operator) beschrieben, und der `as`-Operator wird im [as-Operator](expressions.md#the-as-operator)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-2116">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="20699-2117">Der `==`-, `!=`-, `<`-, `>`-, `<=`-und `>=`-Operatoren sind ***Vergleichs Operatoren***.</span><span class="sxs-lookup"><span data-stu-id="20699-2117">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="20699-2118">Wenn ein Operand eines Vergleichs Operators den Kompilier Zeittyp `dynamic` aufweist, wird der Ausdruck dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="20699-2118">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="20699-2119">In diesem Fall ist der Kompilier Zeittyp des Ausdrucks `dynamic`, und die unten beschriebene Auflösung findet zur Laufzeit statt, wobei der Lauf Zeittyp der Operanden verwendet wird, die den Typ der Kompilierzeit `dynamic` aufweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-2119">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="20699-2120">Bei einem Vorgang in der Form `x` *op* `y`, wobei *op* ein Vergleichs Operator ist, wird die Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-2120">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-2121">Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-2121">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="20699-2122">Die vordefinierten Vergleichs Operatoren werden in den folgenden Abschnitten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-2122">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="20699-2123">Alle vordefinierten Vergleichs Operatoren geben ein Ergebnis des Typs `bool` zurück, wie in der folgenden Tabelle beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-2123">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="20699-2124">__Vorgang__</span><span class="sxs-lookup"><span data-stu-id="20699-2124">__Operation__</span></span> | <span data-ttu-id="20699-2125">__Ergebnis__</span><span class="sxs-lookup"><span data-stu-id="20699-2125">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="20699-2126">`true`, wenn `x` gleich `y` ist, andernfalls `false`</span><span class="sxs-lookup"><span data-stu-id="20699-2126">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="20699-2127">`true`, wenn `x` nicht gleich `y` ist, andernfalls `false`</span><span class="sxs-lookup"><span data-stu-id="20699-2127">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="20699-2128">`true`, wenn `x` kleiner als `y` ist, andernfalls `false`</span><span class="sxs-lookup"><span data-stu-id="20699-2128">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="20699-2129">`true`, wenn `x` größer als `y` ist, andernfalls `false`</span><span class="sxs-lookup"><span data-stu-id="20699-2129">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="20699-2130">`true`, wenn `x` kleiner oder gleich `y` ist, andernfalls `false`</span><span class="sxs-lookup"><span data-stu-id="20699-2130">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="20699-2131">`true`, wenn `x` größer oder gleich `y` ist, andernfalls `false`</span><span class="sxs-lookup"><span data-stu-id="20699-2131">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="20699-2132">Integer-Vergleichs Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2132">Integer comparison operators</span></span>

<span data-ttu-id="20699-2133">Die vordefinierten ganzzahligen Vergleichs Operatoren lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-2133">The predefined integer comparison operators are:</span></span>
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

<span data-ttu-id="20699-2134">Jeder dieser Operatoren vergleicht die numerischen Werte der beiden ganzzahligen Operanden und gibt einen `bool`-Wert zurück, der angibt, ob die jeweilige Beziehung `true` oder `false` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2134">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="20699-2135">Vergleichs Operatoren für Gleit Komma Zahlen</span><span class="sxs-lookup"><span data-stu-id="20699-2135">Floating-point comparison operators</span></span>

<span data-ttu-id="20699-2136">Die vordefinierten Gleit Komma Vergleichs Operatoren lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-2136">The predefined floating-point comparison operators are:</span></span>
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

<span data-ttu-id="20699-2137">Die Operanden werden von den Operatoren gemäß den Regeln des IEEE 754-Standards verglichen:</span><span class="sxs-lookup"><span data-stu-id="20699-2137">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="20699-2138">Wenn einer der beiden Operanden NaN ist, ist das Ergebnis für alle Operatoren mit Ausnahme von `!=` `false` (null), für die das Ergebnis `true` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2138">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="20699-2139">Bei zwei Operanden erzeugt `x != y` immer dasselbe Ergebnis wie `!(x == y)`.</span><span class="sxs-lookup"><span data-stu-id="20699-2139">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="20699-2140">Wenn jedoch ein oder beide Operanden NaN sind, führen die Operatoren "`<`", "`>`", "`<=`" und "`>=`" nicht zu den gleichen Ergebnissen wie die logische Negation des umgekehrten Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-2140">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="20699-2141">Wenn z. b. einer `x` und `y` NaN ist, dann ist `x < y` `false`, `!(x >= y)` ist jedoch `true`.</span><span class="sxs-lookup"><span data-stu-id="20699-2141">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="20699-2142">Wenn keiner der Operanden NaN ist, vergleichen die Operatoren die Werte der beiden Gleit Komma Operanden in Bezug auf die Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="20699-2142">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="20699-2143">Dabei sind `min` und `max` die kleinsten und größten positiven Endwerte, die im angegebenen Gleit Komma Format dargestellt werden können.</span><span class="sxs-lookup"><span data-stu-id="20699-2143">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="20699-2144">Wichtige Auswirkungen dieser Reihenfolge:</span><span class="sxs-lookup"><span data-stu-id="20699-2144">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="20699-2145">Negative und positive Nullen werden als gleich betrachtet.</span><span class="sxs-lookup"><span data-stu-id="20699-2145">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="20699-2146">Minus unendlich gilt als kleiner als alle anderen Werte, aber gleichbedeutend mit einem anderen negativen unendlich.</span><span class="sxs-lookup"><span data-stu-id="20699-2146">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="20699-2147">Eine positive Unendlichkeit gilt als größer als alle anderen Werte, aber gleichbedeutend mit einer anderen positiven unendlich.</span><span class="sxs-lookup"><span data-stu-id="20699-2147">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="20699-2148">Dezimale Vergleichs Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2148">Decimal comparison operators</span></span>

<span data-ttu-id="20699-2149">Die vordefinierten dezimalen Vergleichs Operatoren lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-2149">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="20699-2150">Jeder dieser Operatoren vergleicht die numerischen Werte der beiden Decimal-Operanden und gibt einen `bool`-Wert zurück, der angibt, ob die jeweilige Beziehung `true` oder `false` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2150">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="20699-2151">Jeder Dezimal Vergleich entspricht der Verwendung des entsprechenden relationalen or-Gleichheits Operators vom Typ `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="20699-2151">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="20699-2152">Boolesche Gleichheits Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2152">Boolean equality operators</span></span>

<span data-ttu-id="20699-2153">Die vordefinierten booleschen Gleichheits Operatoren lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-2153">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="20699-2154">Das Ergebnis von `==` ist `true`, wenn sowohl `x` als auch `y` `true` sind oder wenn sowohl `x` als auch `y` `false` sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2154">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="20699-2155">Andernfalls ist das Ergebnis `false`.</span><span class="sxs-lookup"><span data-stu-id="20699-2155">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="20699-2156">Das Ergebnis von `!=` ist `false`, wenn sowohl `x` als auch `y` `true` sind oder wenn sowohl `x` als auch `y` `false` sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2156">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="20699-2157">Andernfalls ist das Ergebnis `true`.</span><span class="sxs-lookup"><span data-stu-id="20699-2157">Otherwise, the result is `true`.</span></span> <span data-ttu-id="20699-2158">Wenn die Operanden vom Typ `bool` sind, erzeugt der `!=`-Operator dasselbe Ergebnis wie der `^`-Operator.</span><span class="sxs-lookup"><span data-stu-id="20699-2158">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="20699-2159">Enumerationsvergleichs-Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2159">Enumeration comparison operators</span></span>

<span data-ttu-id="20699-2160">Jeder Enumerationstyp stellt implizit die folgenden vordefinierten Vergleichs Operatoren bereit:</span><span class="sxs-lookup"><span data-stu-id="20699-2160">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="20699-2161">Das Ergebnis der Auswertung von `x op y`, wobei `x` und `y` Ausdrücke eines Enumerationstyps sind `E` mit einem zugrunde liegenden Typ `U`, und `op` ist einer der Vergleichs Operatoren, entspricht dem Auswerten von `((U)x) op ((U)y)`.</span><span class="sxs-lookup"><span data-stu-id="20699-2161">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="20699-2162">Mit anderen Worten, die Vergleichs Operatoren des Enumerationstyps vergleichen einfach die zugrunde liegenden ganzzahligen Werte der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-2162">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="20699-2163">Verweistyp-Gleichheits Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2163">Reference type equality operators</span></span>

<span data-ttu-id="20699-2164">Die vordefinierten Verweistyp-Gleichheits Operatoren sind:</span><span class="sxs-lookup"><span data-stu-id="20699-2164">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="20699-2165">Die Operatoren geben das Ergebnis des Vergleichs der beiden Verweise auf Gleichheit oder nicht Gleichheit zurück.</span><span class="sxs-lookup"><span data-stu-id="20699-2165">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="20699-2166">Da die vordefinierten Verweistyp-Gleichheits Operatoren Operanden vom Typ `object` akzeptieren, gelten Sie für alle Typen, die keine anwendbaren `operator ==`-und `operator !=`-Member deklarieren.</span><span class="sxs-lookup"><span data-stu-id="20699-2166">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="20699-2167">Im Gegensatz dazu Blenden alle anwendbaren benutzerdefinierten Gleichheits Operatoren die vordefinierten Verweistyp-Gleichheits Operatoren aus.</span><span class="sxs-lookup"><span data-stu-id="20699-2167">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="20699-2168">Die vordefinierten Verweistyp-Gleichheits Operatoren erfordern eine der folgenden:</span><span class="sxs-lookup"><span data-stu-id="20699-2168">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="20699-2169">Beide Operanden sind ein Wert eines Typs, der bekanntermaßen ein *reference_type* oder das Literale `null` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2169">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="20699-2170">Darüber hinaus ist eine explizite Verweis Konvertierung ([explizite Verweis Konvertierungen](conversions.md#explicit-reference-conversions)) vom Typ eines der beiden Operanden bis zum Typ des anderen Operanden vorhanden.</span><span class="sxs-lookup"><span data-stu-id="20699-2170">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="20699-2171">Ein Operand ist ein Wert vom Typ "`T`", wobei "`T`" ein *type_parameter* und der andere Operand der Literale `null` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2171">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="20699-2172">Außerdem weist `T` nicht die Werttyp Einschränkung auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2172">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="20699-2173">Wenn eine dieser Bedingungen nicht zutrifft, tritt ein Fehler bei der Bindung auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2173">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="20699-2174">Wichtige Implikationen dieser Regeln sind:</span><span class="sxs-lookup"><span data-stu-id="20699-2174">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="20699-2175">Es handelt sich um einen Bindungs Fehler, bei dem die vordefinierten Verweistyp-Gleichheits Operatoren verwendet werden, um zwei Verweise zu vergleichen, die bekanntermaßen bei der Bindungs Zeit unterschiedlich sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2175">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="20699-2176">Wenn die Bindungs Zeit Typen der Operanden beispielsweise zwei Klassentypen sind `A` und `B`, und wenn weder `A` noch `B` vom anderen abgeleitet ist, wäre es unmöglich, dass die beiden Operanden auf das gleiche Objekt verweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-2176">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="20699-2177">Daher wird der Vorgang als Bindungs Zeit Fehler betrachtet.</span><span class="sxs-lookup"><span data-stu-id="20699-2177">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="20699-2178">Die vordefinierten Verweistyp-Gleichheits Operatoren lassen nicht zu, dass Werttyp Operanden verglichen werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2178">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="20699-2179">Daher ist es nicht möglich, Werte dieses Struktur Typs zu vergleichen, es sei denn, ein Strukturtyp deklariert seine eigenen Gleichheits Operatoren.</span><span class="sxs-lookup"><span data-stu-id="20699-2179">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="20699-2180">Die vordefinierten Verweistyp-Gleichheits Operatoren bewirken nie, dass Boxing-Vorgänge für ihre Operanden ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2180">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="20699-2181">Es wäre bedeutungslos, solche Boxing-Vorgänge auszuführen, da Verweise auf die neu zugeordneten geachtelten Instanzen notwendigerweise von allen anderen verweisen abweichen.</span><span class="sxs-lookup"><span data-stu-id="20699-2181">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="20699-2182">Wenn ein Operand vom typparametertyp `T` mit `null` verglichen wird und der Lauf Zeittyp von `T` ein Werttyp ist, ist das Ergebnis des Vergleichs `false`.</span><span class="sxs-lookup"><span data-stu-id="20699-2182">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="20699-2183">Im folgenden Beispiel wird überprüft, ob ein Argument eines nicht eingeschränkten Typparameter Typs `null` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2183">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="20699-2184">Das `x == null`-Konstrukt ist zulässig, obwohl `T` einen Werttyp darstellen könnte und das Ergebnis einfach als `false` definiert ist, wenn `T` ein Werttyp ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2184">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="20699-2185">Bei einem Vorgang in der Form `x == y` oder `x != y`, sofern zutreffend `operator ==` oder `operator !=` vorhanden ist, wird durch die Auflösung der Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) dieser Operator anstelle der vordefinierten Verweistyp Gleichheit ausgewählt. KOM.</span><span class="sxs-lookup"><span data-stu-id="20699-2185">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="20699-2186">Es ist jedoch immer möglich, den vordefinierten Verweistyp Gleichheits Operator auszuwählen, indem eine oder beide der Operanden explizit in den Typ "`object`" umgewandelt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2186">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="20699-2187">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2187">The example</span></span>
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
<span data-ttu-id="20699-2188">erzeugt die Ausgabe</span><span class="sxs-lookup"><span data-stu-id="20699-2188">produces the output</span></span>
```console
True
False
False
False
```

<span data-ttu-id="20699-2189">Die Variablen "`s`" und "`t`" verweisen auf zwei unterschiedliche `string`-Instanzen, die die gleichen Zeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="20699-2189">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="20699-2190">Der erste Vergleich gibt `True` aus, da der vordefinierte Zeichen folgen Gleichheits Operator ([Zeichen folgen Gleichheits Operatoren](expressions.md#string-equality-operators)) ausgewählt ist, wenn beide Operanden den Typ `string` haben.</span><span class="sxs-lookup"><span data-stu-id="20699-2190">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="20699-2191">Die restlichen Vergleiche alle Ausgabe `False`, da der vordefinierte Verweistyp Gleichheits Operator ausgewählt ist, wenn einer oder beide Operanden vom Typ `object` sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2191">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="20699-2192">Beachten Sie, dass die oben beschriebene Technik für Werttypen nicht sinnvoll ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2192">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="20699-2193">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2193">The example</span></span>
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
<span data-ttu-id="20699-2194">gibt `False` aus, da die Umwandlungen Verweise auf zwei separate Instanzen von `int`-Werten erstellen.</span><span class="sxs-lookup"><span data-stu-id="20699-2194">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="20699-2195">Operatoren für Zeichen folgen</span><span class="sxs-lookup"><span data-stu-id="20699-2195">String equality operators</span></span>

<span data-ttu-id="20699-2196">Die vordefinierten Zeichen folgen-Gleichheits Operatoren sind:</span><span class="sxs-lookup"><span data-stu-id="20699-2196">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="20699-2197">Zwei `string`-Werte werden als gleich betrachtet, wenn eine der folgenden Punkte zutrifft:</span><span class="sxs-lookup"><span data-stu-id="20699-2197">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="20699-2198">Beide Werte sind `null`.</span><span class="sxs-lookup"><span data-stu-id="20699-2198">Both values are `null`.</span></span>
*  <span data-ttu-id="20699-2199">Beide Werte sind Verweise ungleich NULL auf Zeichen folgen Instanzen, die identische Längen und identische Zeichen an jeder Zeichenposition aufweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-2199">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="20699-2200">Die Gleichheits Operatoren für Zeichen folgen vergleichen Zeichen folgen Werte anstelle von Zeichen folgen verweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-2200">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="20699-2201">Wenn zwei separate Zeichen folgen Instanzen genau dieselbe Zeichenfolge enthalten, sind die Werte der Zeichen folgen gleich, aber die Verweise unterscheiden sich.</span><span class="sxs-lookup"><span data-stu-id="20699-2201">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="20699-2202">Wie in [Verweistyp Gleichheits Operatoren](expressions.md#reference-type-equality-operators)beschrieben, können die Verweistyp-Gleichheits Operatoren verwendet werden, um Zeichen folgen Verweise anstelle von Zeichen folgen Werten zu vergleichen.</span><span class="sxs-lookup"><span data-stu-id="20699-2202">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="20699-2203">Delegatenoperatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2203">Delegate equality operators</span></span>

<span data-ttu-id="20699-2204">Jeder Delegattyp stellt implizit die folgenden vordefinierten Vergleichs Operatoren bereit:</span><span class="sxs-lookup"><span data-stu-id="20699-2204">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="20699-2205">Zwei Delegatinstanzen werden wie folgt als gleich betrachtet:</span><span class="sxs-lookup"><span data-stu-id="20699-2205">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="20699-2206">Wenn eine der Delegatinstanzen `null` ist, sind Sie nur dann gleich, wenn beide `null` sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2206">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="20699-2207">Wenn die Delegaten einen anderen Lauf Zeittyp aufweisen, sind Sie nie gleich.</span><span class="sxs-lookup"><span data-stu-id="20699-2207">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="20699-2208">Wenn beide Delegatinstanzen über eine Aufruf Liste ([Delegatdeklarationen](delegates.md#delegate-declarations)) verfügen, sind diese Instanzen nur dann gleich, wenn Ihre Aufruf Listen dieselbe Länge aufweisen und jeder Eintrag in einer Aufruf Liste gleich (wie unten definiert) dem entsprechenden Eintrag in der Reihenfolge in der Aufruf Liste eines anderen.</span><span class="sxs-lookup"><span data-stu-id="20699-2208">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="20699-2209">Die folgenden Regeln bestimmen die Gleichheit von Aufruf Listeneinträgen:</span><span class="sxs-lookup"><span data-stu-id="20699-2209">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="20699-2210">Wenn zwei Aufruf Listeneinträge auf dieselbe statische Methode verweisen, sind die Einträge gleich.</span><span class="sxs-lookup"><span data-stu-id="20699-2210">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="20699-2211">Wenn zwei Aufruf Listeneinträge auf dieselbe nicht statische Methode im gleichen Zielobjekt verweisen (wie durch die Verweis Gleichheits Operatoren definiert), sind die Einträge gleich.</span><span class="sxs-lookup"><span data-stu-id="20699-2211">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="20699-2212">Aufruf Listeneinträge, die aus der Auswertung von semantisch identischen *anonymous_method_expression*s oder *lambda_expression*s mit dem gleichen (möglicherweise leeren) Satz erfasster externer Variablen Instanzen erstellt werden, sind zulässig (aber nicht erforderlich). hoch.</span><span class="sxs-lookup"><span data-stu-id="20699-2212">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="20699-2213">Gleichheits Operatoren und NULL</span><span class="sxs-lookup"><span data-stu-id="20699-2213">Equality operators and null</span></span>

<span data-ttu-id="20699-2214">Mit den Operatoren `==` und `!=` kann ein Operand ein Wert eines Typs sein, der NULL-Werte zulässt, und der andere als `null`-Literale, auch wenn kein vordefinierter oder benutzerdefinierter Operator für den Vorgang vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2214">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="20699-2215">Für einen Vorgang eines der Formulare</span><span class="sxs-lookup"><span data-stu-id="20699-2215">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="20699-2216">Wenn `x` ein Ausdruck eines Typs ist, der NULL-Werte zulässt, wenn die Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) keinen anwendbaren Operator findet, wird das Ergebnis stattdessen aus der `HasValue`-Eigenschaft von `x` berechnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2216">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="20699-2217">Insbesondere werden die ersten beiden Formulare in `!x.HasValue` übersetzt, und die letzten zwei Formen werden in `x.HasValue` übersetzt.</span><span class="sxs-lookup"><span data-stu-id="20699-2217">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="20699-2218">Der is-Operator</span><span class="sxs-lookup"><span data-stu-id="20699-2218">The is operator</span></span>

<span data-ttu-id="20699-2219">Mit dem `is`-Operator wird dynamisch überprüft, ob der Lauf Zeittyp eines Objekts mit einem bestimmten Typ kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2219">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="20699-2220">Das Ergebnis des Vorgangs `E is T`, wobei `E` ein Ausdruck und `T` ein Typ ist, ein boolescher Wert, der angibt, ob `E` erfolgreich durch eine Verweis Konvertierung, eine Boxing-Konvertierung oder eine Unboxing-Konvertierung in den Typ `T` konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="20699-2220">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="20699-2221">Der Vorgang wird wie folgt ausgewertet, nachdem Typargumente für alle Typparameter ersetzt wurden:</span><span class="sxs-lookup"><span data-stu-id="20699-2221">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="20699-2222">Wenn `E` eine anonyme Funktion ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2222">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="20699-2223">Wenn `E` eine Methoden Gruppe oder das `null`-Literale ist, wenn der Typ von `E` ein Referenztyp oder ein Werte zulässt-Typ ist und der Wert von `E` NULL ist, ist das Ergebnis false.</span><span class="sxs-lookup"><span data-stu-id="20699-2223">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="20699-2224">Andernfalls soll `D` den dynamischen Typ `E` wie folgt darstellen:</span><span class="sxs-lookup"><span data-stu-id="20699-2224">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="20699-2225">Wenn der Typ von `E` ein Referenztyp ist, ist `D` der Lauf Zeittyp des instanzverweises von `E`.</span><span class="sxs-lookup"><span data-stu-id="20699-2225">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="20699-2226">Wenn der Typ von `E` ein Typ ist, der NULL-Werte zulässt, ist `D` der zugrunde liegende Typ dieses Typs, der NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="20699-2226">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="20699-2227">Wenn der Typ von `E` ein Werttyp ist, der keine NULL-Werte zulässt, ist `D` der Typ von `E`.</span><span class="sxs-lookup"><span data-stu-id="20699-2227">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="20699-2228">Das Ergebnis des Vorgangs hängt wie folgt von `D` und `T` ab:</span><span class="sxs-lookup"><span data-stu-id="20699-2228">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="20699-2229">Wenn `T` ein Referenztyp ist, ist das Ergebnis true, wenn `D` und `T` denselben Typ haben, wenn `D` ein Referenztyp ist und eine implizite Verweis Konvertierung von `D` in `T` vorhanden ist, oder wenn `D` ein Werttyp und eine Boxing-Konvertierung von `D` ist. , wenn `T` vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2229">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="20699-2230">Wenn `T` ein Typ ist, der NULL-Werte zulässt, ist das Ergebnis true, wenn `D` der zugrunde liegende Typ von `T` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2230">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="20699-2231">Wenn `T` ein Werttyp ist, der keine NULL-Werte zulässt, ist das Ergebnis true, wenn `D` und `T` denselben Typ haben.</span><span class="sxs-lookup"><span data-stu-id="20699-2231">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="20699-2232">Andernfalls ist das Ergebnis false.</span><span class="sxs-lookup"><span data-stu-id="20699-2232">Otherwise, the result is false.</span></span>

<span data-ttu-id="20699-2233">Beachten Sie, dass benutzerdefinierte Konvertierungen vom `is`-Operator nicht berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2233">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="20699-2234">Der as-Operator</span><span class="sxs-lookup"><span data-stu-id="20699-2234">The as operator</span></span>

<span data-ttu-id="20699-2235">Der `as`-Operator wird verwendet, um explizit einen Wert in einen angegebenen Verweistyp oder Werte zulässt-Typ zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="20699-2235">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="20699-2236">Anders als bei einem Umwandlungs Ausdruck ([Cast-Ausdrücke](expressions.md#cast-expressions)) löst der `as`-Operator nie eine Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="20699-2236">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="20699-2237">Wenn die angegebene Konvertierung nicht möglich ist, ist der resultierende Wert `null`.</span><span class="sxs-lookup"><span data-stu-id="20699-2237">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="20699-2238">In einem Vorgang der Form `E as T` muss `E` ein Ausdruck sein, und `T` muss ein Verweistyp sein, ein Typparameter, der als Verweistyp bekannt ist, oder ein Typ, der NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="20699-2238">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="20699-2239">Außerdem muss mindestens einer der folgenden Punkte zutreffen. andernfalls tritt ein Kompilierzeitfehler auf:</span><span class="sxs-lookup"><span data-stu-id="20699-2239">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="20699-2240">Eine Identität ([Identitäts Konvertierung](conversions.md#identity-conversion)), implizite NULL-Werte zulassen ([implizite Konvertierungen auf NULL](conversions.md#implicit-nullable-conversions)zulassen), impliziter Verweis ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions)), Boxing ([Boxing-Konvertierungen](conversions.md#boxing-conversions)), explizite NULL-Werte zulassen ([explizite Nullwerte zulassen Konvertierungen](conversions.md#explicit-nullable-conversions)), explizite Verweise ([explizite Verweis Konvertierungen](conversions.md#explicit-reference-conversions)) oder Unboxing-Konvertierung ([Unboxing](conversions.md#unboxing-conversions)-Konvertierung) von `E` in `T`.</span><span class="sxs-lookup"><span data-stu-id="20699-2240">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="20699-2241">Der Typ `E` oder `T` ist ein offener Typ.</span><span class="sxs-lookup"><span data-stu-id="20699-2241">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="20699-2242">`E` ist das `null`-Literale.</span><span class="sxs-lookup"><span data-stu-id="20699-2242">`E` is the `null` literal.</span></span>

<span data-ttu-id="20699-2243">Wenn der Kompilier Zeittyp `E` nicht `dynamic` ist, ergibt der Vorgang `E as T` dasselbe Ergebnis wie</span><span class="sxs-lookup"><span data-stu-id="20699-2243">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="20699-2244">außer dass `E` nur einmal überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2244">except that `E` is only evaluated once.</span></span> <span data-ttu-id="20699-2245">Der Compiler kann `E as T` optimieren, um höchstens eine dynamische Typüberprüfung durchzuführen, im Gegensatz zu den zwei dynamischen Typüberprüfungen, die von der obigen Erweiterung impliziert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2245">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="20699-2246">Wenn der Kompilier Zeittyp `E` `dynamic` ist, wird der `as`-Operator im Gegensatz zum Cast Operator nicht dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="20699-2246">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="20699-2247">Daher ist die Erweiterung in diesem Fall:</span><span class="sxs-lookup"><span data-stu-id="20699-2247">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="20699-2248">Beachten Sie, dass einige Konvertierungen, wie z. b. benutzerdefinierte Konvertierungen, mit dem `as`-Operator nicht möglich sind und stattdessen mithilfe von Umwandlungs Ausdrücken ausgeführt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="20699-2248">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="20699-2249">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2249">In the example</span></span>
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
<span data-ttu-id="20699-2250">der Typparameter `T` von `G` ist bekannt, dass es sich um einen Verweistyp handelt, da er die-Klassen Einschränkung aufweist.</span><span class="sxs-lookup"><span data-stu-id="20699-2250">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="20699-2251">Der Typparameter `U` von `H` ist jedoch nicht. Daher ist die Verwendung des `as`-Operators in `H` nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="20699-2251">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="20699-2252">Logische Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2252">Logical operators</span></span>

<span data-ttu-id="20699-2253">Die Operatoren "`&`", "`^`" und "`|`" werden als logische Operatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2253">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

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

<span data-ttu-id="20699-2254">Wenn ein Operand eines logischen Operators den Kompilier Zeittyp `dynamic` aufweist, wird der Ausdruck dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="20699-2254">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="20699-2255">In diesem Fall ist der Kompilier Zeittyp des Ausdrucks `dynamic`, und die unten beschriebene Auflösung findet zur Laufzeit statt, wobei der Lauf Zeittyp der Operanden verwendet wird, die den Typ der Kompilierzeit `dynamic` aufweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-2255">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="20699-2256">Bei einem Vorgang in der Form `x op y`, wobei `op` einer der logischen Operatoren ist, wird die Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="20699-2256">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="20699-2257">Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.</span><span class="sxs-lookup"><span data-stu-id="20699-2257">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="20699-2258">Die vordefinierten logischen Operatoren werden in den folgenden Abschnitten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-2258">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="20699-2259">Ganz Zahl logische Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2259">Integer logical operators</span></span>

<span data-ttu-id="20699-2260">Die vordefinierten ganzzahligen logischen Operatoren lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-2260">The predefined integer logical operators are:</span></span>
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

<span data-ttu-id="20699-2261">Der `&`-Operator berechnet das bitweise logische `AND` der beiden Operanden, der `|`-Operator berechnet das bitweise logische `OR` der beiden Operanden, und der `^`-Operator berechnet das bitweise logische exklusive `OR` der beiden Operanden.</span><span class="sxs-lookup"><span data-stu-id="20699-2261">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="20699-2262">Von diesen Vorgängen können keine über Flüsse durchlaufen werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2262">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="20699-2263">Logische Enumerationsoperatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2263">Enumeration logical operators</span></span>

<span data-ttu-id="20699-2264">Jeder Enumerationstyp `E` stellt implizit die folgenden vordefinierten logischen Operatoren bereit:</span><span class="sxs-lookup"><span data-stu-id="20699-2264">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="20699-2265">Das Ergebnis der Auswertung von `x op y`, wobei `x` und `y` Ausdrücke eines Enumerationstyps sind `E` mit einem zugrunde liegenden Typ `U`, und `op` ist einer der logischen Operatoren, entspricht dem Auswerten von `(E)((U)x op (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="20699-2265">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="20699-2266">Mit anderen Worten: die logischen Operatoren des Enumerationstyps führen einfach die logische Operation für den zugrunde liegenden Typ der beiden Operanden aus.</span><span class="sxs-lookup"><span data-stu-id="20699-2266">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="20699-2267">Logische boolesche Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2267">Boolean logical operators</span></span>

<span data-ttu-id="20699-2268">Die vordefinierten booleschen logischen Operatoren lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-2268">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="20699-2269">Das Ergebnis von `x & y` ist `true`, wenn sowohl `x` als auch `y` zu `true` ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2269">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="20699-2270">Andernfalls ist das Ergebnis `false`.</span><span class="sxs-lookup"><span data-stu-id="20699-2270">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="20699-2271">Das Ergebnis von `x | y` ist `true`, wenn entweder `x` oder `y` `true` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2271">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="20699-2272">Andernfalls ist das Ergebnis `false`.</span><span class="sxs-lookup"><span data-stu-id="20699-2272">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="20699-2273">Das Ergebnis von `x ^ y` ist `true`, wenn `x` `true` und `y` `false` oder `x` `false` und `y` `true` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2273">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="20699-2274">Andernfalls ist das Ergebnis `false`.</span><span class="sxs-lookup"><span data-stu-id="20699-2274">Otherwise, the result is `false`.</span></span> <span data-ttu-id="20699-2275">Wenn die Operanden vom Typ `bool` sind, berechnet der `^`-Operator dasselbe Ergebnis wie der `!=`-Operator.</span><span class="sxs-lookup"><span data-stu-id="20699-2275">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="20699-2276">Boolesche logische Operatoren, die NULL-Werte zulassen</span><span class="sxs-lookup"><span data-stu-id="20699-2276">Nullable boolean logical operators</span></span>

<span data-ttu-id="20699-2277">Der booleschen-Typ, der NULL-Werte zulässt `bool?` kann die drei Werte `true`, `false` und `null` darstellen und ist konzeptionell vergleichbar mit dem dreiwertigen Typ, der für boolesche Ausdrücke in SQL verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2277">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="20699-2278">Um sicherzustellen, dass die Ergebnisse, die von den Operanden `&` und `|` für `bool?`-Operanden generiert werden, mit der dreistufigen Logik von SQL konsistent sind, werden die folgenden vordefinierten Operatoren bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="20699-2278">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="20699-2279">In der folgenden Tabelle werden die Ergebnisse aufgelistet, die von diesen Operatoren für alle Kombinationen der Werte `true`, `false` und `null` generiert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2279">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

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

## <a name="conditional-logical-operators"></a><span data-ttu-id="20699-2280">Bedingte logische Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2280">Conditional logical operators</span></span>

<span data-ttu-id="20699-2281">Die Operatoren `&&` und `||` werden als bedingte logische Operatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2281">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="20699-2282">Sie werden auch als "Kurzschluss" logische Operatoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2282">They are also called the "short-circuiting" logical operators.</span></span>

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

<span data-ttu-id="20699-2283">Die Operatoren "`&&`" und "`||`" sind bedingte Versionen der `&`-und `|`-Operatoren:</span><span class="sxs-lookup"><span data-stu-id="20699-2283">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="20699-2284">Der Vorgang `x && y` entspricht dem Vorgang `x & y`, mit dem Unterschied, dass `y` nur ausgewertet wird, wenn `x` nicht `false` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2284">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="20699-2285">Der Vorgang `x || y` entspricht dem Vorgang `x | y`, mit dem Unterschied, dass `y` nur ausgewertet wird, wenn `x` nicht `true` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2285">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="20699-2286">Wenn ein Operand eines bedingten logischen Operators den Kompilier Zeittyp `dynamic` aufweist, wird der Ausdruck dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="20699-2286">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="20699-2287">In diesem Fall ist der Kompilier Zeittyp des Ausdrucks `dynamic`, und die unten beschriebene Auflösung findet zur Laufzeit statt, wobei der Lauf Zeittyp der Operanden verwendet wird, die den Typ der Kompilierzeit `dynamic` aufweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-2287">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="20699-2288">Ein Vorgang in der Form `x && y` oder `x || y` wird durch Anwenden der Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) verarbeitet, als wäre der Vorgang `x & y` oder `x | y` geschrieben worden.</span><span class="sxs-lookup"><span data-stu-id="20699-2288">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="20699-2289">Seitdem</span><span class="sxs-lookup"><span data-stu-id="20699-2289">Then,</span></span>

*  <span data-ttu-id="20699-2290">Wenn die Überladungs Auflösung keinen einzelnen optimalen Operator findet oder wenn die Überladungs Auflösung einen der vordefinierten logischen ganzzahligen Operatoren auswählt, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2290">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="20699-2291">Andernfalls wird der Vorgang verarbeitet, wenn der ausgewählte Operator einer der vordefinierten booleschen logischen Operatoren ([boolesche logische Operatoren](expressions.md#boolean-logical-operators)) oder NULL-Werte zulässt ([boolesche logische](expressions.md#nullable-boolean-logical-operators)Operatoren, die NULL-Werte zulassen), wie in [beschrieben. Boolesche bedingte logische Operatoren](expressions.md#boolean-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="20699-2291">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="20699-2292">Andernfalls ist der ausgewählte Operator ein benutzerdefinierter Operator, und der Vorgang wird wie unter [Benutzerdefinierte bedingte logische Operatoren](expressions.md#user-defined-conditional-logical-operators)beschrieben verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="20699-2292">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="20699-2293">Es ist nicht möglich, die bedingten logischen Operatoren direkt zu überladen.</span><span class="sxs-lookup"><span data-stu-id="20699-2293">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="20699-2294">Da die bedingten logischen Operatoren jedoch in Bezug auf die regulären logischen Operatoren ausgewertet werden, sind über Ladungen der regulären logischen Operatoren mit bestimmten Einschränkungen auch als über Ladungen der bedingten logischen Operatoren zu berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="20699-2294">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="20699-2295">Dies wird weiter unten unter [Benutzerdefinierte bedingte logische Operatoren](expressions.md#user-defined-conditional-logical-operators)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-2295">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="20699-2296">Boolesche bedingte logische Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2296">Boolean conditional logical operators</span></span>

<span data-ttu-id="20699-2297">Wenn die Operanden von `&&` oder `||` vom Typ `bool` sind oder wenn es sich bei den Operanden um Typen handelt, die keinen anwendbaren `operator &` oder `operator |` definieren, aber implizite Konvertierungen in `bool` definieren, wird der Vorgang wie folgt verarbeitet. :</span><span class="sxs-lookup"><span data-stu-id="20699-2297">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="20699-2298">Der Vorgang `x && y` wird als `x ? y : false` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-2298">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="20699-2299">Mit anderen Worten: "`x`" wird zuerst ausgewertet und in den Typ "`bool`" konvertiert.</span><span class="sxs-lookup"><span data-stu-id="20699-2299">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="20699-2300">Wenn `x` `true` ist, wird `y` ausgewertet und in den Typ `bool` konvertiert, und dies wird das Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="20699-2300">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="20699-2301">Andernfalls ist das Ergebnis des Vorgangs `false`.</span><span class="sxs-lookup"><span data-stu-id="20699-2301">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="20699-2302">Der Vorgang `x || y` wird als `x ? true : y` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-2302">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="20699-2303">Mit anderen Worten: "`x`" wird zuerst ausgewertet und in den Typ "`bool`" konvertiert.</span><span class="sxs-lookup"><span data-stu-id="20699-2303">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="20699-2304">Wenn `x` `true` ist, ist das Ergebnis des Vorgangs `true`.</span><span class="sxs-lookup"><span data-stu-id="20699-2304">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="20699-2305">Andernfalls wird `y` ausgewertet und in den Typ `bool` konvertiert, und dies wird das Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="20699-2305">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="20699-2306">Benutzerdefinierte bedingte logische Operatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2306">User-defined conditional logical operators</span></span>

<span data-ttu-id="20699-2307">Wenn die Operanden von `&&` oder `||` von Typen sind, die einen anwendbaren benutzerdefinierten `operator &` oder `operator |` deklarieren, müssen beide der folgenden Punkte zutreffen, wobei `T` der Typ ist, in dem der ausgewählte Operator deklariert ist:</span><span class="sxs-lookup"><span data-stu-id="20699-2307">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="20699-2308">Der Rückgabetyp und der Typ jedes Parameters des ausgewählten Operators müssen `T` sein.</span><span class="sxs-lookup"><span data-stu-id="20699-2308">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="20699-2309">Anders ausgedrückt, muss der Operator die logische `AND` oder die logische `OR` von zwei Operanden des Typs `T` berechnen und ein Ergebnis des Typs `T` zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="20699-2309">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="20699-2310">`T` muss Deklarationen von `operator true` und `operator false` enthalten.</span><span class="sxs-lookup"><span data-stu-id="20699-2310">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="20699-2311">Ein Fehler bei der Bindungs Zeit tritt auf, wenn eine dieser Anforderungen nicht erfüllt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2311">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="20699-2312">Andernfalls wird der `&&`-oder `||`-Vorgang ausgewertet, indem der benutzerdefinierte `operator true` oder `operator false` mit dem ausgewählten benutzerdefinierten Operator kombiniert wird:</span><span class="sxs-lookup"><span data-stu-id="20699-2312">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="20699-2313">Der Vorgang `x && y` wird als `T.false(x) ? x : T.&(x, y)` ausgewertet, wobei `T.false(x)` ein Aufruf der `operator false` ist, die in `T` deklariert ist, und `T.&(x, y)` ein Aufruf des ausgewählten `operator &` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2313">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="20699-2314">Das heißt, `x` wird zuerst ausgewertet, und `operator false` wird für das Ergebnis aufgerufen, um zu bestimmen, ob `x` definitiv false ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2314">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="20699-2315">Wenn `x` definitiv false ist, ist das Ergebnis des Vorgangs der zuvor für `x` berechnete Wert.</span><span class="sxs-lookup"><span data-stu-id="20699-2315">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="20699-2316">Andernfalls wird `y` ausgewertet, und der ausgewählte `operator &` wird für den Wert aufgerufen, der zuvor für `x` berechnet wurde, und der für `y` berechnete Wert, um das Ergebnis des Vorgangs zu liefern.</span><span class="sxs-lookup"><span data-stu-id="20699-2316">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="20699-2317">Der Vorgang `x || y` wird als `T.true(x) ? x : T.|(x, y)` ausgewertet, wobei `T.true(x)` ein Aufruf der `operator true` ist, die in `T` deklariert ist, und `T.|(x,y)` ein Aufruf des ausgewählten `operator|` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2317">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="20699-2318">Das heißt, `x` wird zuerst ausgewertet, und `operator true` wird für das Ergebnis aufgerufen, um zu bestimmen, ob `x` definitiv true ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2318">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="20699-2319">Wenn `x` definitiv true ist, ist das Ergebnis des Vorgangs der zuvor für `x` berechnete Wert.</span><span class="sxs-lookup"><span data-stu-id="20699-2319">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="20699-2320">Andernfalls wird `y` ausgewertet, und der ausgewählte `operator |` wird für den Wert aufgerufen, der zuvor für `x` berechnet wurde, und der für `y` berechnete Wert, um das Ergebnis des Vorgangs zu liefern.</span><span class="sxs-lookup"><span data-stu-id="20699-2320">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="20699-2321">Bei beiden Vorgängen wird der von `x` angegebene Ausdruck nur einmal ausgewertet, und der von `y` angegebene Ausdruck wird entweder nicht genau einmal ausgewertet oder ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-2321">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="20699-2322">Ein Beispiel für einen Typ, der `operator true` und `operator false` implementiert, finden Sie unter [Daten Bank boolescher Typ](structs.md#database-boolean-type).</span><span class="sxs-lookup"><span data-stu-id="20699-2322">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="20699-2323">Der NULL-Sammel Operator</span><span class="sxs-lookup"><span data-stu-id="20699-2323">The null coalescing operator</span></span>

<span data-ttu-id="20699-2324">Der `??`-Operator wird als NULL-Sammel Operator bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2324">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="20699-2325">Ein NULL-Sammel Ausdruck der Form `a ?? b` erfordert, dass `a` ein Typ oder Verweistyp ist, der NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="20699-2325">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="20699-2326">Wenn `a` nicht NULL ist, ist das Ergebnis von `a ?? b` `a`; Andernfalls ist das Ergebnis `b`.</span><span class="sxs-lookup"><span data-stu-id="20699-2326">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="20699-2327">Der Vorgang wertet `b` nur aus, wenn `a` NULL ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2327">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="20699-2328">Der NULL-Sammel Operator ist rechts assoziativ, was bedeutet, dass Vorgänge von rechts nach Links gruppiert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2328">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="20699-2329">Beispielsweise wird ein Ausdruck in der Form `a ?? b ?? c` als `a ?? (b ?? c)` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-2329">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="20699-2330">In der Regel gibt ein Ausdruck der Form `E1 ?? E2 ?? ... ?? En` den ersten der Operanden zurück, der nicht NULL ist, oder NULL, wenn alle Operanden NULL sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2330">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="20699-2331">Der Typ des Ausdrucks `a ?? b` hängt davon ab, welche impliziten Konvertierungen für die Operanden verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2331">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="20699-2332">In der Reihenfolge der Präferenz ist der Typ von `a ?? b` `A0`, `A` oder `B`, wobei `A` der Typ von `a` ist (vorausgesetzt, dass `a` einen-Typ aufweist), `B` der Typ von `b` ist (vorausgesetzt, dass `b` einen-Typ aufweist). , und 0 ist der zugrunde liegende Typ von 1, wenn 2 ein Typ ist, der NULL-Werte zulässt, andernfalls 3.</span><span class="sxs-lookup"><span data-stu-id="20699-2332">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="20699-2333">Insbesondere wird `a ?? b` wie folgt verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="20699-2333">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="20699-2334">Wenn `A` vorhanden und kein Werte zulässt-Typ oder Verweistyp ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2334">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="20699-2335">Wenn `b` ein dynamischer Ausdruck ist, ist der Ergebnistyp `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="20699-2335">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="20699-2336">Zur Laufzeit wird `a` zuerst ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-2336">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="20699-2337">Wenn `a` nicht NULL ist, wird `a` in Dynamic konvertiert, und dies wird zum Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="20699-2337">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="20699-2338">Andernfalls wird `b` ausgewertet und das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="20699-2338">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="20699-2339">Wenn `A` vorhanden ist und ein Typ ist, der NULL-Werte zulässt, und eine implizite Konvertierung von `b` in `A0` erfolgt, ist der Ergebnistyp `A0`.</span><span class="sxs-lookup"><span data-stu-id="20699-2339">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="20699-2340">Zur Laufzeit wird `a` zuerst ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-2340">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="20699-2341">Wenn `a` nicht NULL ist, wird `a` in den Typ `A0` entpackt, und dies wird zum Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="20699-2341">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="20699-2342">Andernfalls wird `b` ausgewertet und in den Typ `A0` konvertiert, und dies wird zum Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="20699-2342">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="20699-2343">Andernfalls ist der Ergebnistyp `A`, wenn `A` vorhanden ist und eine implizite Konvertierung von `b` zu `A` vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2343">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="20699-2344">Zur Laufzeit wird `a` zuerst ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-2344">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="20699-2345">Wenn `a` nicht NULL ist, wird `a` zum Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="20699-2345">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="20699-2346">Andernfalls wird `b` ausgewertet und in den Typ `A` konvertiert, und dies wird zum Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="20699-2346">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="20699-2347">Andernfalls ist der Ergebnistyp `B`, wenn `b` einen Typ aufweist `B` und eine implizite Konvertierung von `a` in `B` erfolgt.</span><span class="sxs-lookup"><span data-stu-id="20699-2347">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="20699-2348">Zur Laufzeit wird `a` zuerst ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-2348">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="20699-2349">Wenn `a` nicht NULL ist, wird `a` in den Typ `A0` entpackt (wenn `A` vorhanden und NULL-Werte zulässt) und in den Typ `B` konvertiert wurde, und dies wird das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="20699-2349">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="20699-2350">Andernfalls wird `b` ausgewertet und zum Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="20699-2350">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="20699-2351">Andernfalls sind `a` und `b` nicht kompatibel, und ein Kompilierzeitfehler tritt auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2351">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="20699-2352">Bedingter Operator</span><span class="sxs-lookup"><span data-stu-id="20699-2352">Conditional operator</span></span>

<span data-ttu-id="20699-2353">Der `?:`-Operator wird als Bedingter Operator bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2353">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="20699-2354">Es wird manchmal auch als ternärer Operator bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2354">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="20699-2355">Ein bedingter Ausdruck der Form `b ? x : y` wertet zuerst die Bedingung `b` aus.</span><span class="sxs-lookup"><span data-stu-id="20699-2355">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="20699-2356">Wenn `b` `true` ist, wird `x` ausgewertet und zum Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="20699-2356">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="20699-2357">Andernfalls wird `y` ausgewertet und zum Ergebnis des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="20699-2357">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="20699-2358">Ein bedingter Ausdruck wertet nicht sowohl `x` als auch `y` aus.</span><span class="sxs-lookup"><span data-stu-id="20699-2358">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="20699-2359">Der bedingte Operator ist rechts assoziativ, was bedeutet, dass Vorgänge von rechts nach Links gruppiert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2359">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="20699-2360">Beispielsweise wird ein Ausdruck in der Form `a ? b : c ? d : e` als `a ? b : (c ? d : e)` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-2360">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="20699-2361">Der erste Operand des `?:`-Operators muss ein Ausdruck sein, der implizit in `bool` konvertiert werden kann, oder ein Ausdruck eines Typs, der `operator true` implementiert.</span><span class="sxs-lookup"><span data-stu-id="20699-2361">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="20699-2362">Wenn keine dieser Anforderungen erfüllt ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2362">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="20699-2363">Mit dem zweiten und dritten Operanden, `x` und `y`, des `?:`-Operators, wird der Typ des bedingten Ausdrucks gesteuert.</span><span class="sxs-lookup"><span data-stu-id="20699-2363">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="20699-2364">Wenn `x` den Typ `X` hat und `y` den Typ `Y` hat, dann</span><span class="sxs-lookup"><span data-stu-id="20699-2364">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="20699-2365">Wenn eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) von `X` in `Y`, aber nicht von `Y` in `X` erfolgt, ist `Y` der Typ des bedingten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="20699-2365">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="20699-2366">Wenn eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) von `Y` in `X`, aber nicht von `X` in `Y` erfolgt, ist `X` der Typ des bedingten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="20699-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="20699-2367">Andernfalls kann kein Ausdruckstyp ermittelt werden, und es tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2367">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="20699-2368">Wenn nur einer `x` und `y` einen-Typ aufweist und sowohl `x` als auch `y` von implizit in diesen Typ konvertiert werden können, ist dies der Typ des bedingten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="20699-2368">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="20699-2369">Andernfalls kann kein Ausdruckstyp ermittelt werden, und es tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2369">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="20699-2370">Die Lauf Zeit Verarbeitung eines bedingten Ausdrucks der Form `b ? x : y` besteht aus den folgenden Schritten:</span><span class="sxs-lookup"><span data-stu-id="20699-2370">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="20699-2371">Zuerst wird `b` ausgewertet, und der `bool`-Wert von `b` wird festgelegt:</span><span class="sxs-lookup"><span data-stu-id="20699-2371">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="20699-2372">Wenn eine implizite Konvertierung vom Typ `b` in `bool` vorhanden ist, wird diese implizite Konvertierung durchgeführt, um einen `bool`-Wert zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="20699-2372">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="20699-2373">Andernfalls wird das vom Typ `b` definierte `operator true` aufgerufen, um einen `bool`-Wert zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="20699-2373">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="20699-2374">Wenn der `bool`-Wert, der mit dem obigen Schritt erstellt wurde, `true` ist, wird `x` ausgewertet und in den Typ des bedingten Ausdrucks konvertiert, und dies wird das Ergebnis des bedingten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="20699-2374">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="20699-2375">Andernfalls wird `y` ausgewertet und in den Typ des bedingten Ausdrucks konvertiert, und dies wird das Ergebnis des bedingten Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="20699-2375">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="20699-2376">Anonyme Funktions Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-2376">Anonymous function expressions</span></span>

<span data-ttu-id="20699-2377">Eine ***anonyme Funktion*** ist ein Ausdruck, der eine "Inline"-Methoden Definition darstellt.</span><span class="sxs-lookup"><span data-stu-id="20699-2377">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="20699-2378">Eine anonyme Funktion verfügt nicht über einen Wert oder einen Typ in und von sich selbst, kann jedoch in einen kompatiblen Delegaten oder Ausdrucks Strukturtyp konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2378">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="20699-2379">Die Auswertung einer anonymen Funktions Konvertierung hängt vom Zieltyp der Konvertierung ab: Wenn es sich um einen Delegattyp handelt, wird die Konvertierung zu einem Delegatwert ausgewertet, der auf die Methode verweist, die die anonyme Funktion definiert.</span><span class="sxs-lookup"><span data-stu-id="20699-2379">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="20699-2380">Wenn es sich um einen Ausdrucks bauentyp handelt, wird die Konvertierung zu einer Ausdrucks Baumstruktur ausgewertet, die die Struktur der Methode als Objektstruktur darstellt.</span><span class="sxs-lookup"><span data-stu-id="20699-2380">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="20699-2381">Aus historischen Gründen gibt es zwei syntaktische Varianten von anonymen Funktionen, nämlich *lambda_expression*s und *anonymous_method_expression*s.</span><span class="sxs-lookup"><span data-stu-id="20699-2381">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="20699-2382">Für fast alle Zwecke sind *lambda_expression*s präziser und ausdrucksstarker als *anonymous_method_expression*s, die für die Abwärtskompatibilität in der Sprache verbleiben.</span><span class="sxs-lookup"><span data-stu-id="20699-2382">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

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

<span data-ttu-id="20699-2383">Der Operator `=>` verfügt über die gleiche Rangfolge wie die Zuweisung (`=`) und ist rechtsassoziativ.</span><span class="sxs-lookup"><span data-stu-id="20699-2383">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="20699-2384">Eine anonyme Funktion mit dem `async`-Modifizierer ist eine Async-Funktion und befolgt die in [Iteratoren](classes.md#iterators)beschriebenen Regeln.</span><span class="sxs-lookup"><span data-stu-id="20699-2384">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="20699-2385">Die Parameter einer anonymen Funktion in Form eines *lambda_expression* können explizit oder implizit eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2385">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="20699-2386">In einer explizit typisierten Parameterliste wird der Typ jedes Parameters explizit angegeben.</span><span class="sxs-lookup"><span data-stu-id="20699-2386">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="20699-2387">In einer implizit typisierten Parameterliste werden die Parametertypen aus dem Kontext abgeleitet, in dem die anonyme Funktion auftritt – insbesondere wenn die anonyme Funktion in einen kompatiblen Delegattyp oder Ausdrucks Strukturtyp konvertiert wird, stellt dieser Typ die Parametertypen ([Anonyme Funktions Konvertierungen](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="20699-2387">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="20699-2388">In einer anonymen Funktion mit einem einzelnen, implizit typisierten Parameter können die Klammern in der Parameterliste weggelassen werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2388">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="20699-2389">Anders ausgedrückt: eine anonyme Funktion der Form</span><span class="sxs-lookup"><span data-stu-id="20699-2389">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="20699-2390">kann abgekürzt werden zu</span><span class="sxs-lookup"><span data-stu-id="20699-2390">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="20699-2391">Die Parameterliste einer anonymen Funktion in Form eines *anonymous_method_expression* ist optional.</span><span class="sxs-lookup"><span data-stu-id="20699-2391">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="20699-2392">Wenn angegeben, müssen die Parameter explizit typisiert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2392">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="20699-2393">Wenn dies nicht der Wert ist, kann die anonyme Funktion in einen Delegaten mit einer beliebigen Parameterliste konvertiert werden, die keine `out`-Parameter</span><span class="sxs-lookup"><span data-stu-id="20699-2393">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="20699-2394">Ein *Block* Körper einer anonymen Funktion ist erreichbar ([Endpunkte und Erreichbarkeit](statements.md#end-points-and-reachability)), es sei denn, die anonyme Funktion tritt innerhalb einer nicht erreichbaren Anweisung auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2394">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="20699-2395">Im folgenden finden Sie einige Beispiele für anonyme Funktionen:</span><span class="sxs-lookup"><span data-stu-id="20699-2395">Some examples of anonymous functions follow below:</span></span>

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

<span data-ttu-id="20699-2396">Das Verhalten von *lambda_expression*s und *anonymous_method_expression*s ist mit Ausnahme der folgenden Punkte identisch:</span><span class="sxs-lookup"><span data-stu-id="20699-2396">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="20699-2397">*anonymous_method_expression*s erlauben, dass die Parameterliste vollständig ausgelassen wird, sodass Sie die Möglichkeit haben, Typen von Wert Parametern zu delegieren.</span><span class="sxs-lookup"><span data-stu-id="20699-2397">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="20699-2398">*lambda_expression*s erlauben, dass Parametertypen ausgelassen und abgeleitet werden, während für *anonymous_method_expression*s Parametertypen explizit angegeben werden müssen.</span><span class="sxs-lookup"><span data-stu-id="20699-2398">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="20699-2399">Der Text eines *lambda_expression* kann ein Ausdruck oder ein Anweisungsblock sein, während der Text eines *anonymous_method_expression* ein Anweisungsblock sein muss.</span><span class="sxs-lookup"><span data-stu-id="20699-2399">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="20699-2400">Nur *lambda_expression*s verfügen über Konvertierungen in kompatible Ausdrucks Baumstruktur Typen ([Ausdrucks Baumstruktur Typen](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="20699-2400">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="20699-2401">Anonyme Funktions Signaturen</span><span class="sxs-lookup"><span data-stu-id="20699-2401">Anonymous function signatures</span></span>

<span data-ttu-id="20699-2402">Mit dem optionalen *anonymous_function_signature* einer anonymen Funktion werden die Namen und optional die Typen der formalen Parameter für die anonyme Funktion definiert.</span><span class="sxs-lookup"><span data-stu-id="20699-2402">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="20699-2403">Der Gültigkeitsbereich der Parameter der anonymen Funktion ist der *anonymous_function_body*.</span><span class="sxs-lookup"><span data-stu-id="20699-2403">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="20699-2404">([Bereiche](basic-concepts.md#scopes)) In Verbindung mit der Parameterliste (falls angegeben) bildet der anonyme Methoden Text einen Deklarations Raum ([Deklarationen](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="20699-2404">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="20699-2405">Daher ist es ein Kompilierzeitfehler, wenn der Name eines Parameters der anonymen Funktion mit dem Namen einer lokalen Variablen, lokalen Konstante oder eines Parameters identisch ist, deren Bereich die *anonymous_method_expression* oder *lambda_expression*enthält.</span><span class="sxs-lookup"><span data-stu-id="20699-2405">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="20699-2406">Wenn eine anonyme Funktion über eine *explicit_anonymous_function_signature*verfügt, ist der Satz kompatibler Delegattypen und Ausdrucks Baum Typen auf diejenigen beschränkt, die dieselben Parametertypen und Modifizierer in der gleichen Reihenfolge aufweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-2406">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="20699-2407">Im Gegensatz zu Methoden Gruppen Konvertierungen ([Methoden Gruppen Konvertierungen](conversions.md#method-group-conversions)) wird die kontra Varianz anonymer Funktionsparameter Typen nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="20699-2407">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="20699-2408">Wenn eine anonyme Funktion keinen *anonymous_function_signature*hat, ist der Satz kompatibler Delegattypen und Ausdrucks Baum Typen auf diejenigen beschränkt, die keine `out`-Parameter haben.</span><span class="sxs-lookup"><span data-stu-id="20699-2408">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="20699-2409">Beachten Sie, dass ein *anonymous_function_signature* keine Attribute oder ein Parameter Array enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="20699-2409">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="20699-2410">Dennoch kann ein *anonymous_function_signature* mit einem Delegattyp kompatibel sein, dessen Parameterliste ein Parameter Array enthält.</span><span class="sxs-lookup"><span data-stu-id="20699-2410">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="20699-2411">Beachten Sie auch, dass bei der Konvertierung in einen Ausdrucks bauentyp, auch wenn diese kompatibel ist, während der Kompilierzeit ([Ausdrucks Baumstruktur Typen](types.md#expression-tree-types)) weiterhin Fehler auftreten können.</span><span class="sxs-lookup"><span data-stu-id="20699-2411">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="20699-2412">Anonyme Funktions Texte</span><span class="sxs-lookup"><span data-stu-id="20699-2412">Anonymous function bodies</span></span>

<span data-ttu-id="20699-2413">Der Text (*Ausdruck* oder *Block*) einer anonymen Funktion unterliegt den folgenden Regeln:</span><span class="sxs-lookup"><span data-stu-id="20699-2413">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="20699-2414">Wenn die anonyme Funktion eine Signatur enthält, sind die in der Signatur angegebenen Parameter im Textkörper verfügbar.</span><span class="sxs-lookup"><span data-stu-id="20699-2414">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="20699-2415">Wenn die anonyme Funktion keine Signatur aufweist, kann Sie in einen Delegattyp oder einen Ausdruckstyp mit Parametern ([Anonyme Funktions Konvertierungen](conversions.md#anonymous-function-conversions)) konvertiert werden, aber auf die Parameter kann im Text nicht zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2415">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="20699-2416">Mit Ausnahme der Parameter "`ref`" oder "`out`", die in der Signatur (sofern vorhanden) der nächstgelegenen einschließenden anonymen Funktion angegeben sind, handelt es sich um einen Kompilierzeitfehler für den Text, um auf einen `ref`-oder `out`-Parameter zuzugreifen</span><span class="sxs-lookup"><span data-stu-id="20699-2416">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="20699-2417">Wenn der Typ von `this` ein Strukturtyp ist, ist dies ein Kompilierzeitfehler für den Text Zugriff auf `this`.</span><span class="sxs-lookup"><span data-stu-id="20699-2417">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="20699-2418">Dies gilt unabhängig davon, ob der Zugriff explizit (wie in `this.x`) oder implizit ist (wie in `x`, wenn `x` ein Instanzmember der Struktur ist).</span><span class="sxs-lookup"><span data-stu-id="20699-2418">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="20699-2419">Diese Regel verhindert einen solchen Zugriff und wirkt sich nicht darauf aus, ob die Member-Suche zu einem Member der Struktur führt.</span><span class="sxs-lookup"><span data-stu-id="20699-2419">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="20699-2420">Der Text hat Zugriff auf die äußeren Variablen ([äußere Variablen](expressions.md#outer-variables)) der anonymen Funktion.</span><span class="sxs-lookup"><span data-stu-id="20699-2420">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="20699-2421">Der Zugriff auf eine äußere Variable verweist auf die Instanz der Variablen, die zum Zeitpunkt der Auswertung von *lambda_expression* oder *anonymous_method_expression* ([Auswertung anonymer Funktions Ausdrücke](expressions.md#evaluation-of-anonymous-function-expressions)) aktiv ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2421">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="20699-2422">Es ist ein Kompilierzeitfehler, wenn der Text eine `goto`-Anweisung, eine `break`-Anweisung oder eine `continue`-Anweisung enthält, deren Ziel außerhalb des Texts oder innerhalb des Texts einer enthaltenen anonymen Funktion liegt.</span><span class="sxs-lookup"><span data-stu-id="20699-2422">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="20699-2423">Eine `return`-Anweisung im Text gibt die Steuerung von einem Aufruf der nächstgelegenen anonymen Funktion zurück, nicht vom einschließenden Funktionsmember.</span><span class="sxs-lookup"><span data-stu-id="20699-2423">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="20699-2424">Ein Ausdruck, der in einer `return`-Anweisung angegeben ist, muss implizit in den Rückgabetyp des Delegattyps oder Ausdrucks Struktur Typs konvertiert werden, in den die nächstgelegene einschließende *lambda_expression* bzw. *anonymous_method_expression* konvertiert wird. [Anonyme Funktions Konvertierungen](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="20699-2424">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="20699-2425">Es ist explizit nicht angegeben, ob es eine Möglichkeit gibt, den Block einer anonymen Funktion auszuführen, außer durch Auswerten und Aufrufen von *lambda_expression* oder *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="20699-2425">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="20699-2426">Insbesondere kann der Compiler eine anonyme Funktion durch das Zusammenführen einer oder mehrerer benannter Methoden oder Typen implementieren.</span><span class="sxs-lookup"><span data-stu-id="20699-2426">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="20699-2427">Die Namen dieser Elemente mit synthetischer kompilarverwendung müssen ein für die Compilerverwendung reserviertes Formular sein.</span><span class="sxs-lookup"><span data-stu-id="20699-2427">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="20699-2428">Überladungs Auflösung und Anonyme Funktionen</span><span class="sxs-lookup"><span data-stu-id="20699-2428">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="20699-2429">Anonyme Funktionen in einer Argumentliste sind an der Typrückschluss-und Überladungs Auflösung beteiligt.</span><span class="sxs-lookup"><span data-stu-id="20699-2429">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="20699-2430">Die genauen Regeln finden Sie unter [Typrückschluss](expressions.md#type-inference) und [Überladungs Auflösung](expressions.md#overload-resolution) .</span><span class="sxs-lookup"><span data-stu-id="20699-2430">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="20699-2431">Das folgende Beispiel veranschaulicht die Auswirkung anonymer Funktionen auf die Überladungs Auflösung.</span><span class="sxs-lookup"><span data-stu-id="20699-2431">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

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

<span data-ttu-id="20699-2432">Die `ItemList<T>`-Klasse verfügt über zwei `Sum`-Methoden.</span><span class="sxs-lookup"><span data-stu-id="20699-2432">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="20699-2433">Jede übernimmt ein `selector`-Argument, das den Wert aus einem Listenelement in Summen extrahiert.</span><span class="sxs-lookup"><span data-stu-id="20699-2433">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="20699-2434">Der extrahierte Wert kann entweder ein `int` oder ein `double` sein, und die resultierende Summe ist entweder ein `int` oder ein `double`.</span><span class="sxs-lookup"><span data-stu-id="20699-2434">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="20699-2435">Die `Sum`-Methoden könnten beispielsweise verwendet werden, um Summen aus einer Liste von Detail Zeilen in einer Reihenfolge zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="20699-2435">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

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

<span data-ttu-id="20699-2436">Beim ersten Aufruf von `orderDetails.Sum` sind beide `Sum`-Methoden anwendbar, da die anonyme Funktion `d => d. UnitCount` mit `Func<Detail,int>` und `Func<Detail,double>` kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2436">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="20699-2437">Die Überladungs Auflösung wählt jedoch die erste `Sum`-Methode aus, da die Konvertierung in `Func<Detail,int>` besser ist als die Konvertierung in `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="20699-2437">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="20699-2438">Beim zweiten Aufruf von `orderDetails.Sum` ist nur die zweite `Sum`-Methode anwendbar, da die anonyme Funktion `d => d.UnitPrice * d.UnitCount` einen Wert vom Typ `double` erzeugt.</span><span class="sxs-lookup"><span data-stu-id="20699-2438">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="20699-2439">Daher wählt die Überladungs Auflösung die zweite `Sum`-Methode für diesen Aufruf aus.</span><span class="sxs-lookup"><span data-stu-id="20699-2439">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="20699-2440">Anonyme Funktionen und dynamische Bindung</span><span class="sxs-lookup"><span data-stu-id="20699-2440">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="20699-2441">Eine anonyme Funktion kann kein Empfänger, Argument oder Operand eines dynamisch gebundenen Vorgangs sein.</span><span class="sxs-lookup"><span data-stu-id="20699-2441">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="20699-2442">Äußere Variablen</span><span class="sxs-lookup"><span data-stu-id="20699-2442">Outer variables</span></span>

<span data-ttu-id="20699-2443">Alle lokalen Variablen, Wert Parameter oder Parameter Arrays, deren Bereich den *lambda_expression* -oder *anonymous_method_expression* -Wert enthält, werden als ***äußere Variable*** der anonymen Funktion bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2443">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="20699-2444">In einem Instanzfunktionsmember einer Klasse wird der `this`-Wert als Wert Parameter angesehen und ist eine äußere Variable einer beliebigen anonymen Funktion, die im Funktionsmember enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2444">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="20699-2445">Erfasste äußere Variablen</span><span class="sxs-lookup"><span data-stu-id="20699-2445">Captured outer variables</span></span>

<span data-ttu-id="20699-2446">Wenn auf eine äußere Variable durch eine anonyme Funktion verwiesen wird, wird die äußere Variable als von der anonymen Funktion ***aufgezeichnet*** .</span><span class="sxs-lookup"><span data-stu-id="20699-2446">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="20699-2447">Normalerweise ist die Lebensdauer einer lokalen Variable auf die Ausführung des Blocks oder der Anweisung beschränkt, mit der Sie verknüpft ist ([lokale Variablen](variables.md#local-variables)).</span><span class="sxs-lookup"><span data-stu-id="20699-2447">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="20699-2448">Die Lebensdauer einer erfassten äußeren Variable wird jedoch mindestens so lange verlängert, bis der Delegat oder die Ausdrucks Struktur, der aus der anonymen Funktion erstellt wurde, für Garbage Collection qualifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2448">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="20699-2449">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2449">In the example</span></span>
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
<span data-ttu-id="20699-2450">die lokale Variable `x` wird von der anonymen Funktion aufgezeichnet, und die Lebensdauer von `x` wird mindestens so lange verlängert, bis der von `F` zurückgegebene Delegat für Garbage Collection infrage kommt (was bis zum Ende des Programms nicht erfolgt).</span><span class="sxs-lookup"><span data-stu-id="20699-2450">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="20699-2451">Da jeder Aufruf der anonymen Funktion auf derselben Instanz von `x` funktioniert, lautet die Ausgabe des Beispiels wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-2451">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```console
1
2
3
```

<span data-ttu-id="20699-2452">Wenn eine lokale Variable oder ein value-Parameter von einer anonymen Funktion aufgezeichnet wird, wird die lokale Variable oder der Parameter nicht mehr als eine festgelegte Variable ([fest-und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)) betrachtet, sondern als eine verschiebbare Variable angesehen.</span><span class="sxs-lookup"><span data-stu-id="20699-2452">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="20699-2453">Daher muss jeder `unsafe`-Code, der die Adresse einer erfassten äußeren Variablen annimmt, zuerst die `fixed`-Anweisung verwenden, um die Variable zu korrigieren.</span><span class="sxs-lookup"><span data-stu-id="20699-2453">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="20699-2454">Beachten Sie, dass im Gegensatz zu einer nicht erfassten Variablen eine aufgezeichnete lokale Variable gleichzeitig für mehrere Ausführungs Threads verfügbar gemacht werden kann.</span><span class="sxs-lookup"><span data-stu-id="20699-2454">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="20699-2455">Instanziierung von lokalen Variablen</span><span class="sxs-lookup"><span data-stu-id="20699-2455">Instantiation of local variables</span></span>

<span data-ttu-id="20699-2456">Eine lokale Variable wird als ***instanziiert*** betrachtet, wenn die Ausführung in den Gültigkeitsbereich der Variablen eintritt.</span><span class="sxs-lookup"><span data-stu-id="20699-2456">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="20699-2457">Wenn z. b. die folgende Methode aufgerufen wird, wird die lokale Variable `x` instanziiert und dreimal initialisiert – einmal für jede Iterationen der Schleife.</span><span class="sxs-lookup"><span data-stu-id="20699-2457">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="20699-2458">Das Verschieben der Deklaration von `x` außerhalb der Schleife führt jedoch zu einer einzelnen Instanziierung von `x`:</span><span class="sxs-lookup"><span data-stu-id="20699-2458">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="20699-2459">Bei nicht Erfassung kann nicht genau beachtet werden, wie oft eine lokale Variable instanziiert wird – da die Lebensdauer der Instanziierungen disjunkt ist, kann jede Instanziierung einfach denselben Speicherort verwenden.</span><span class="sxs-lookup"><span data-stu-id="20699-2459">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="20699-2460">Wenn eine anonyme Funktion jedoch eine lokale Variable erfasst, werden die Auswirkungen der Instanziierung offensichtlich.</span><span class="sxs-lookup"><span data-stu-id="20699-2460">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="20699-2461">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2461">The example</span></span>
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
<span data-ttu-id="20699-2462">erzeugt die Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="20699-2462">produces the output:</span></span>
```console
1
3
5
```

<span data-ttu-id="20699-2463">Wenn jedoch die Deklaration von `x` außerhalb der Schleife verschoben wird:</span><span class="sxs-lookup"><span data-stu-id="20699-2463">However, when the declaration of `x` is moved outside the loop:</span></span>
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
<span data-ttu-id="20699-2464">Die Ausgabe lautet:</span><span class="sxs-lookup"><span data-stu-id="20699-2464">the output is:</span></span>
```console
5
5
5
```

<span data-ttu-id="20699-2465">Wenn eine for-Schleife eine Iterations Variable deklariert, wird die Variable selbst als außerhalb der Schleife deklariert.</span><span class="sxs-lookup"><span data-stu-id="20699-2465">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="20699-2466">Wenn das Beispiel so geändert wird, dass die Iterations Variable selbst aufgezeichnet wird:</span><span class="sxs-lookup"><span data-stu-id="20699-2466">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="20699-2467">Es wird nur eine Instanz der Iterations Variablen aufgezeichnet, die die Ausgabe erzeugt:</span><span class="sxs-lookup"><span data-stu-id="20699-2467">only one instance of the iteration variable is captured, which produces the output:</span></span>
```console
3
3
3
```

<span data-ttu-id="20699-2468">Es ist möglich, dass anonyme Funktions Delegaten einige erfasste Variablen freigeben, aber über separate Instanzen anderer Instanzen verfügen.</span><span class="sxs-lookup"><span data-stu-id="20699-2468">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="20699-2469">Wenn beispielsweise `F` in geändert wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2469">For example, if `F` is changed to</span></span>
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
<span data-ttu-id="20699-2470">die drei Delegaten erfassen dieselbe Instanz von `x`, aber separate Instanzen von `y`, und die Ausgabe sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="20699-2470">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```console
1 1
2 1
3 1
```

<span data-ttu-id="20699-2471">Separate anonyme Funktionen können dieselbe Instanz einer äußeren Variablen erfassen.</span><span class="sxs-lookup"><span data-stu-id="20699-2471">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="20699-2472">Im folgenden Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2472">In the example:</span></span>
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
<span data-ttu-id="20699-2473">die beiden anonymen Funktionen erfassen dieselbe Instanz der lokalen Variablen `x` und können daher über diese Variable kommunizieren.</span><span class="sxs-lookup"><span data-stu-id="20699-2473">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="20699-2474">Die Ausgabe des Beispiels lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-2474">The output of the example is:</span></span>
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="20699-2475">Auswertung anonymer Funktions Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-2475">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="20699-2476">Eine anonyme Funktion `F` muss immer in einen Delegattyp konvertiert werden `D` oder ein Ausdrucks bauentyp `E`, entweder direkt oder durch die Ausführung eines delegaterstellungs-Ausdrucks `new D(F)`.</span><span class="sxs-lookup"><span data-stu-id="20699-2476">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="20699-2477">Diese Konvertierung bestimmt das Ergebnis der anonymen Funktion, wie in [Anonyme Funktions Konvertierungen](conversions.md#anonymous-function-conversions)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-2477">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="20699-2478">Abfrageausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-2478">Query expressions</span></span>

<span data-ttu-id="20699-2479">***Abfrage Ausdrücke*** bieten eine sprach integrierte Syntax für Abfragen, die relationalen und hierarchischen Abfrage Sprachen wie SQL und XQuery ähneln.</span><span class="sxs-lookup"><span data-stu-id="20699-2479">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

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

<span data-ttu-id="20699-2480">Ein Abfrage Ausdruck beginnt mit einer `from`-Klausel und endet mit einer `select`-oder `group`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="20699-2480">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="20699-2481">Auf die anfängliche `from`-Klausel können NULL oder mehr `from`-, `let`-, `where`-, `join`-oder `orderby`-Klauseln folgen.</span><span class="sxs-lookup"><span data-stu-id="20699-2481">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="20699-2482">Jede `from`-Klausel ist ein Generator, der eine ***Bereichs Variable*** einführt, in der die Elemente einer ***Sequenz***liegen.</span><span class="sxs-lookup"><span data-stu-id="20699-2482">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="20699-2483">Jede `let`-Klausel führt eine Bereichs Variable ein, die einen Wert darstellt, der mithilfe vorheriger Bereichs Variablen berechnet wurde.</span><span class="sxs-lookup"><span data-stu-id="20699-2483">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="20699-2484">Jede `where`-Klausel ist ein Filter, der Elemente aus dem Ergebnis ausschließt.</span><span class="sxs-lookup"><span data-stu-id="20699-2484">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="20699-2485">Jede `join`-Klausel vergleicht die angegebenen Schlüssel der Quell Sequenz mit Schlüsseln einer anderen Sequenz und gibt passende Paare aus.</span><span class="sxs-lookup"><span data-stu-id="20699-2485">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="20699-2486">Jede `orderby`-Klausel ordnet Elemente gemäß den angegebenen Kriterien neu an. Die abschließende `select`-oder `group`-Klausel gibt die Form des Ergebnisses in Bezug auf die Bereichs Variablen an.</span><span class="sxs-lookup"><span data-stu-id="20699-2486">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="20699-2487">Zum Schluss kann eine `into`-Klausel verwendet werden, um Abfragen zu "Splice" zu verwenden, indem die Ergebnisse einer Abfrage als Generator in einer nachfolgenden Abfrage behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2487">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="20699-2488">Mehrdeutigkeiten in Abfrage Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="20699-2488">Ambiguities in query expressions</span></span>

<span data-ttu-id="20699-2489">Abfrage Ausdrücke enthalten eine Reihe von "Kontext Schlüsselwörtern", d. h. Bezeichner, die in einem bestimmten Kontext eine besondere Bedeutung haben.</span><span class="sxs-lookup"><span data-stu-id="20699-2489">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="20699-2490">Dabei handelt es sich insbesondere um `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, 0, 1 und 2.</span><span class="sxs-lookup"><span data-stu-id="20699-2490">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="20699-2491">Um Mehrdeutigkeiten in Abfrage Ausdrücken zu vermeiden, die durch die gemischte Verwendung dieser Bezeichner als Schlüsselwörter oder einfache Namen verursacht werden, werden diese Bezeichner als Schlüsselwörter betrachtet, wenn Sie an einer beliebigen Stelle innerhalb eines Abfrage Ausdrucks auftreten.</span><span class="sxs-lookup"><span data-stu-id="20699-2491">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="20699-2492">Zu diesem Zweck ist ein Abfrage Ausdruck ein beliebiger Ausdruck, der mit "`from identifier`" beginnt, gefolgt von einem beliebigen Token mit Ausnahme von "`;`", "`=`" oder "`,`".</span><span class="sxs-lookup"><span data-stu-id="20699-2492">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="20699-2493">Damit diese Wörter als Bezeichner in einem Abfrage Ausdruck verwendet werden können, kann "`@`[" (](lexical-structure.md#identifiers)Bezeichner) als Präfix verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2493">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="20699-2494">Abfrage Ausdrucks Übersetzung</span><span class="sxs-lookup"><span data-stu-id="20699-2494">Query expression translation</span></span>

<span data-ttu-id="20699-2495">Die C# Sprache gibt nicht die Ausführungs Semantik von Abfrage Ausdrücken an.</span><span class="sxs-lookup"><span data-stu-id="20699-2495">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="20699-2496">Stattdessen werden Abfrage Ausdrücke in Aufrufe von Methoden übersetzt, die dem *Abfrage Ausdrucksmuster* ([dem Abfrage Ausdrucksmuster](expressions.md#the-query-expression-pattern)) entsprechen.</span><span class="sxs-lookup"><span data-stu-id="20699-2496">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="20699-2497">Abfrage Ausdrücke werden insbesondere in Aufrufe von Methoden mit dem Namen "`Where`", "`Select`", "`SelectMany`", "`Join`", "`GroupJoin`", "`OrderBy`", "`OrderByDescending`", "`ThenBy`", "`ThenByDescending`" und "0" übersetzt. Diese Methoden verfügen über bestimmte Signaturen und Ergebnistypen, wie im [Abfrage Ausdrucksmuster](expressions.md#the-query-expression-pattern)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-2497">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="20699-2498">Diese Methoden können Instanzmethoden des Objekts sein, das abgefragt wird, oder Erweiterungs Methoden, die sich außerhalb des Objekts befinden, und Sie implementieren die tatsächliche Ausführung der Abfrage.</span><span class="sxs-lookup"><span data-stu-id="20699-2498">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="20699-2499">Die Übersetzung von Abfrage Ausdrücken in Methodenaufrufe ist eine syntaktische Zuordnung, die vor der Durchführung einer Typbindung oder Überladungs Auflösung auftritt.</span><span class="sxs-lookup"><span data-stu-id="20699-2499">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="20699-2500">Es ist garantiert, dass die Übersetzung syntaktisch korrekt ist, aber es wird nicht garantiert, dass C# semantisch korrekter Code erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2500">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="20699-2501">Nach der Übersetzung von Abfrage Ausdrücken werden die resultierenden Methodenaufrufe als reguläre Methodenaufrufe verarbeitet. Dies kann wiederum zu Fehlern führen, z. b. wenn die Methoden nicht vorhanden sind, wenn Argumente falsche Typen aufweisen oder wenn die Methoden generisch sind. der Typrückschluss schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="20699-2501">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="20699-2502">Ein Abfrage Ausdruck wird durch wiederholtes Anwenden der folgenden Übersetzungen verarbeitet, bis keine weiteren Reduzierungen möglich sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2502">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="20699-2503">Die Übersetzungen werden in der Reihenfolge der Anwendung aufgelistet: in jedem Abschnitt wird davon ausgegangen, dass die Übersetzungen in den vorangehenden Abschnitten umfassend ausgeführt wurden, und sobald Sie aufgebraucht sind, wird ein Abschnitt bei der Verarbeitung desselben Abfrage Ausdrucks nicht mehr untersucht.</span><span class="sxs-lookup"><span data-stu-id="20699-2503">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="20699-2504">Die Zuweisung zu Bereichs Variablen ist in Abfrage Ausdrücken nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="20699-2504">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="20699-2505">Eine C# -Implementierung ist jedoch berechtigt, diese Einschränkung nicht immer zu erzwingen, da dies möglicherweise manchmal nicht mit dem hier dargestellten syntaktische Translation-Schema möglich ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2505">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="20699-2506">Bestimmte Übersetzungen fügen Bereichs Variablen mit transparenten Bezeichner ein, die durch `*` bezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2506">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="20699-2507">Die besonderen Eigenschaften von transparenten bezeichmern werden in [transparenten bezeichmern](expressions.md#transparent-identifiers)weiter erläutert.</span><span class="sxs-lookup"><span data-stu-id="20699-2507">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="20699-2508">SELECT-und GroupBy-Klauseln mit Fortsetzungen</span><span class="sxs-lookup"><span data-stu-id="20699-2508">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="20699-2509">Ein Abfrage Ausdruck mit einer Fortsetzung</span><span class="sxs-lookup"><span data-stu-id="20699-2509">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="20699-2510">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2510">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="20699-2511">Bei den Übersetzungen in den folgenden Abschnitten wird davon ausgegangen, dass-Abfragen keine `into`-Fortsetzungen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-2511">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="20699-2512">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2512">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="20699-2513">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2513">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="20699-2514">die letzte Übersetzung, von der</span><span class="sxs-lookup"><span data-stu-id="20699-2514">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="20699-2515">Explizite Bereichs Variablen Typen</span><span class="sxs-lookup"><span data-stu-id="20699-2515">Explicit range variable types</span></span>

<span data-ttu-id="20699-2516">Eine `from`-Klausel, die explizit einen Bereichs Variablentyp angibt.</span><span class="sxs-lookup"><span data-stu-id="20699-2516">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="20699-2517">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2517">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="20699-2518">Eine `join`-Klausel, die explizit einen Bereichs Variablentyp angibt.</span><span class="sxs-lookup"><span data-stu-id="20699-2518">A `join` clause that explicitly specifies a range variable type</span></span>
```csharp
join T x in e on k1 equals k2
```
<span data-ttu-id="20699-2519">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2519">is translated into</span></span>
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="20699-2520">Bei den Übersetzungen in den folgenden Abschnitten wird davon ausgegangen, dass Abfragen keine expliziten Bereichs Variablen Typen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-2520">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="20699-2521">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2521">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="20699-2522">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2522">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="20699-2523">die letzte Übersetzung, von der</span><span class="sxs-lookup"><span data-stu-id="20699-2523">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="20699-2524">Explizite Bereichs Variablen Typen sind nützlich für das Abfragen von Auflistungen, die die nicht generische Schnittstelle "`IEnumerable`" implementieren, aber nicht die generische `IEnumerable<T>`-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="20699-2524">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="20699-2525">Im obigen Beispiel wäre dies der Fall, wenn `customers` vom Typ `ArrayList` wäre.</span><span class="sxs-lookup"><span data-stu-id="20699-2525">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="20699-2526">Degenerierte Abfrage Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-2526">Degenerate query expressions</span></span>

<span data-ttu-id="20699-2527">Ein Abfrage Ausdruck der Form</span><span class="sxs-lookup"><span data-stu-id="20699-2527">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="20699-2528">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2528">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="20699-2529">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2529">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="20699-2530">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2530">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="20699-2531">Ein degenerierter Abfrage Ausdruck ist ein Ausdruck, der die Elemente der Quelle trivial auswählt.</span><span class="sxs-lookup"><span data-stu-id="20699-2531">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="20699-2532">In einer späteren Phase der Übersetzung werden degenerierte Abfragen entfernt, die durch andere Übersetzungsschritte eingeführt wurden, indem Sie durch ihre Quelle ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2532">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="20699-2533">Es ist jedoch wichtig zu gewährleisten, dass das Ergebnis eines Abfrage Ausdrucks nie das Quell Objekt selbst ist, da dadurch der Typ und die Identität der Quelle für den Client der Abfrage offengelegt werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2533">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="20699-2534">Daher schützt dieser Schritt degenerierte Abfragen, die direkt im Quellcode geschrieben wurden, indem `Select` für die Quelle explizit aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2534">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="20699-2535">Dann werden die Implementierer von `Select` und anderen Abfrage Operatoren übernommen, um sicherzustellen, dass diese Methoden nie das Quell Objekt selbst zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="20699-2535">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="20699-2536">From-, Let-, WHERE-, Join-und OrderBy-Klauseln</span><span class="sxs-lookup"><span data-stu-id="20699-2536">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="20699-2537">Ein Abfrage Ausdruck mit einer zweiten `from`-Klausel, gefolgt von einer `select`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="20699-2537">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="20699-2538">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2538">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="20699-2539">Ein Abfrage Ausdruck mit einer zweiten `from`-Klausel, gefolgt von einer anderen Klausel als einer `select`-Klausel:</span><span class="sxs-lookup"><span data-stu-id="20699-2539">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="20699-2540">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2540">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="20699-2541">Ein Abfrage Ausdruck mit einer `let`-Klausel</span><span class="sxs-lookup"><span data-stu-id="20699-2541">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="20699-2542">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2542">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="20699-2543">Ein Abfrage Ausdruck mit einer `where`-Klausel</span><span class="sxs-lookup"><span data-stu-id="20699-2543">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="20699-2544">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2544">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="20699-2545">Ein Abfrage Ausdruck mit einer `join`-Klausel ohne `into`, gefolgt von einer `select`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="20699-2545">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="20699-2546">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2546">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="20699-2547">Ein Abfrage Ausdruck mit einer `join`-Klausel ohne einen `into`, gefolgt von einer `select`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="20699-2547">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="20699-2548">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2548">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="20699-2549">Ein Abfrage Ausdruck mit einer `join`-Klausel mit einem `into`, gefolgt von einer `select`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="20699-2549">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="20699-2550">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2550">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="20699-2551">Ein Abfrage Ausdruck mit einer `join`-Klausel mit einem `into`, gefolgt von einer `select`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="20699-2551">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="20699-2552">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2552">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="20699-2553">Ein Abfrage Ausdruck mit einer `orderby`-Klausel</span><span class="sxs-lookup"><span data-stu-id="20699-2553">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="20699-2554">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2554">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="20699-2555">Wenn eine Sortier Klausel einen `descending`-Richtungsindikator angibt, wird stattdessen ein Aufruf von `OrderByDescending` oder `ThenByDescending` erstellt.</span><span class="sxs-lookup"><span data-stu-id="20699-2555">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="20699-2556">Bei den folgenden Übersetzungen wird davon ausgegangen, dass es keine `let`-, `where`-, `join`-oder `orderby`-Klauseln und nicht mehr als eine anfängliche `from`-Klausel in jedem Abfrage Ausdruck gibt.</span><span class="sxs-lookup"><span data-stu-id="20699-2556">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="20699-2557">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2557">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="20699-2558">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2558">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="20699-2559">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2559">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="20699-2560">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2560">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="20699-2561">die letzte Übersetzung, von der</span><span class="sxs-lookup"><span data-stu-id="20699-2561">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="20699-2562">Dabei ist `x` ein vom Compiler generierter Bezeichner, der andernfalls unsichtbar ist und nicht zugänglich ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2562">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="20699-2563">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2563">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="20699-2564">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2564">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="20699-2565">die letzte Übersetzung, von der</span><span class="sxs-lookup"><span data-stu-id="20699-2565">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="20699-2566">Dabei ist `x` ein vom Compiler generierter Bezeichner, der andernfalls unsichtbar ist und nicht zugänglich ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2566">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="20699-2567">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2567">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="20699-2568">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2568">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="20699-2569">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2569">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="20699-2570">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2570">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="20699-2571">die letzte Übersetzung, von der</span><span class="sxs-lookup"><span data-stu-id="20699-2571">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="20699-2572">Dabei sind `x` und `y` vom Compiler generierte Bezeichner, die andernfalls unsichtbar und nicht zugänglich sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2572">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="20699-2573">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2573">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="20699-2574">hat die endgültige Übersetzung</span><span class="sxs-lookup"><span data-stu-id="20699-2574">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="20699-2575">Select-Klauseln</span><span class="sxs-lookup"><span data-stu-id="20699-2575">Select clauses</span></span>

<span data-ttu-id="20699-2576">Ein Abfrage Ausdruck der Form</span><span class="sxs-lookup"><span data-stu-id="20699-2576">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="20699-2577">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2577">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="20699-2578">mit der Ausnahme, dass v der Bezeichner x ist, ist die Übersetzung einfach</span><span class="sxs-lookup"><span data-stu-id="20699-2578">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="20699-2579">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="20699-2579">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="20699-2580">wird einfach in übersetzt</span><span class="sxs-lookup"><span data-stu-id="20699-2580">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="20699-2581">GroupBy-Klauseln</span><span class="sxs-lookup"><span data-stu-id="20699-2581">Groupby clauses</span></span>

<span data-ttu-id="20699-2582">Ein Abfrage Ausdruck der Form</span><span class="sxs-lookup"><span data-stu-id="20699-2582">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="20699-2583">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2583">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="20699-2584">außer wenn v der Bezeichner x ist, ist die Übersetzung</span><span class="sxs-lookup"><span data-stu-id="20699-2584">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="20699-2585">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2585">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="20699-2586">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2586">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="20699-2587">Transparente Bezeichner</span><span class="sxs-lookup"><span data-stu-id="20699-2587">Transparent identifiers</span></span>

<span data-ttu-id="20699-2588">Bestimmte Übersetzungen fügen Bereichs Variablen mit ***transparenten Bezeichner*** ein, die von `*` angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2588">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="20699-2589">Transparente Bezeichner sind keine ordnungsgemäße Sprachfunktion. Sie sind nur als Zwischenschritt im Übersetzungsprozess des Abfrage Ausdrucks vorhanden.</span><span class="sxs-lookup"><span data-stu-id="20699-2589">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="20699-2590">Wenn eine Abfrage Übersetzung einen transparenten Bezeichner einfügt, verbreiten weitere Übersetzungsschritte den transparenten Bezeichner an anonyme Funktionen und anonyme Objektinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="20699-2590">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="20699-2591">In diesen Kontexten weisen transparente Bezeichner folgendes Verhalten auf:</span><span class="sxs-lookup"><span data-stu-id="20699-2591">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="20699-2592">Wenn ein transparenter Bezeichner als Parameter in einer anonymen Funktion auftritt, werden die Member des zugeordneten anonymen Typs automatisch im Gültigkeitsbereich des Texts der anonymen Funktion angezeigt.</span><span class="sxs-lookup"><span data-stu-id="20699-2592">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="20699-2593">Wenn sich ein Element mit einem transparenten Bezeichner im Gültigkeitsbereich befindet, sind die Member dieses Members ebenfalls im Gültigkeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="20699-2593">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="20699-2594">Wenn ein transparenter Bezeichner als Member-Deklarator in einem anonymen Objektinitialisierer auftritt, führt er einen Member mit einem transparenten Bezeichner ein.</span><span class="sxs-lookup"><span data-stu-id="20699-2594">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="20699-2595">In den oben beschriebenen Übersetzungs Schritten werden transparente Bezeichner immer mit anonymen Typen eingeführt, mit dem Ziel, mehrere Bereichs Variablen als Member eines einzelnen Objekts zu erfassen.</span><span class="sxs-lookup"><span data-stu-id="20699-2595">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="20699-2596">Eine Implementierung von C# darf einen anderen Mechanismus als anonyme Typen verwenden, um mehrere Bereichs Variablen zu gruppieren.</span><span class="sxs-lookup"><span data-stu-id="20699-2596">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="20699-2597">In den folgenden Übersetzungs Beispielen wird davon ausgegangen, dass anonyme Typen verwendet werden, und es wird gezeigt, wie transparente Bezeichner übersetzt werden können.</span><span class="sxs-lookup"><span data-stu-id="20699-2597">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="20699-2598">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2598">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="20699-2599">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2599">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="20699-2600">weiter übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2600">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="20699-2601">, die beim Löschen transparenter Bezeichner entspricht.</span><span class="sxs-lookup"><span data-stu-id="20699-2601">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="20699-2602">Dabei ist `x` ein vom Compiler generierter Bezeichner, der andernfalls unsichtbar ist und nicht zugänglich ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2602">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="20699-2603">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2603">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="20699-2604">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="20699-2604">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="20699-2605">Dies wird weiter reduziert auf</span><span class="sxs-lookup"><span data-stu-id="20699-2605">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="20699-2606">die letzte Übersetzung, von der</span><span class="sxs-lookup"><span data-stu-id="20699-2606">the final translation of which is</span></span>
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
<span data-ttu-id="20699-2607">Dabei sind `x`, `y` und `z` vom Compiler generierte Bezeichner, die andernfalls unsichtbar und nicht zugänglich sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2607">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="20699-2608">Das Abfrage Ausdrucksmuster</span><span class="sxs-lookup"><span data-stu-id="20699-2608">The query expression pattern</span></span>

<span data-ttu-id="20699-2609">Das ***Abfrage Ausdrucksmuster*** stellt ein Muster von Methoden dar, die von Typen implementiert werden können, um Abfrage Ausdrücke zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="20699-2609">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="20699-2610">Da Abfrage Ausdrücke über eine syntaktische Zuordnung in Methodenaufrufe übersetzt werden, haben Typen bei der Implementierung des Abfrage Ausdrucks Musters eine beträchtliche Flexibilität.</span><span class="sxs-lookup"><span data-stu-id="20699-2610">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="20699-2611">Beispielsweise können die Methoden des Musters als Instanzmethoden oder als Erweiterungs Methoden implementiert werden, da beide die gleiche Aufruf Syntax aufweisen und die Methoden Delegaten oder Ausdrucks Baumstrukturen anfordern können, da anonyme Funktionen in beides konvertierbar sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2611">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="20699-2612">Die empfohlene Form eines generischen Typs `C<T>`, der das Abfrage Ausdrucksmuster unterstützt, ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="20699-2612">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="20699-2613">Ein generischer Typ wird verwendet, um die richtigen Beziehungen zwischen Parameter-und Ergebnistypen zu veranschaulichen, aber es ist auch möglich, das Muster für nicht generische Typen zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="20699-2613">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

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

<span data-ttu-id="20699-2614">Die oben genannten Methoden verwenden die generischen Delegattypen `Func<T1,R>` und `Func<T1,T2,R>`, aber Sie könnten auch andere Delegattypen von Delegaten oder Ausdrücken mit denselben Beziehungen in Parameter-und Ergebnistypen verwenden.</span><span class="sxs-lookup"><span data-stu-id="20699-2614">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="20699-2615">Beachten Sie die empfohlene Beziehung zwischen `C<T>` und `O<T>`, mit der sichergestellt wird, dass die `ThenBy`-und `ThenByDescending`-Methode nur für das Ergebnis einer `OrderBy` oder `OrderByDescending` verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2615">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="20699-2616">Beachten Sie auch die empfohlene Form des Ergebnisses `GroupBy`-eine Sequenz von Sequenzen, wobei jede innere Sequenz über eine zusätzliche `Key`-Eigenschaft verfügt.</span><span class="sxs-lookup"><span data-stu-id="20699-2616">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="20699-2617">Der `System.Linq`-Namespace stellt eine Implementierung des Abfrage Operator Musters für jeden Typ bereit, der die `System.Collections.Generic.IEnumerable<T>`-Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="20699-2617">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="20699-2618">Zuweisungsoperatoren</span><span class="sxs-lookup"><span data-stu-id="20699-2618">Assignment operators</span></span>

<span data-ttu-id="20699-2619">Die Zuweisungs Operatoren weisen einer Variablen, einer Eigenschaft, einem Ereignis oder einem Indexer-Element einen neuen Wert zu.</span><span class="sxs-lookup"><span data-stu-id="20699-2619">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

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

<span data-ttu-id="20699-2620">Der linke Operand einer Zuweisung muss ein Ausdruck sein, der als Variable, Eigenschaften Zugriff, Indexerzugriff oder Ereignis Zugriff klassifiziert ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2620">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="20699-2621">Der `=`-Operator wird als ***einfacher Zuweisungs Operator***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2621">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="20699-2622">Er weist den Wert des rechten Operanden der Variablen, der Eigenschaft oder dem Indexer-Element zu, die durch den linken Operanden angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2622">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="20699-2623">Der linke Operand des einfachen Zuweisungs Operators ist möglicherweise kein Ereignis Zugriff (außer wie in [Feld ähnlichen Ereignissen](classes.md#field-like-events)beschrieben).</span><span class="sxs-lookup"><span data-stu-id="20699-2623">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="20699-2624">Der einfache Zuweisungs Operator wird unter [einfache Zuweisung](expressions.md#simple-assignment)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-2624">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="20699-2625">Die anderen Zuweisungs Operatoren als der `=`-Operator werden als ***Verbund Zuweisungs Operatoren***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2625">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="20699-2626">Diese Operatoren führen den angegebenen Vorgang für die beiden Operanden aus und weisen dann den resultierenden Wert der Variablen, der Eigenschaft oder dem Indexer-Element zu, das durch den linken Operanden angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2626">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="20699-2627">Die Verbund Zuweisungs Operatoren werden unter [Verbund Zuweisung](expressions.md#compound-assignment)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-2627">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="20699-2628">Die Operatoren `+=` und `-=` mit einem Ereignis Zugriffs Ausdruck als Linker Operand werden als *Ereignis Zuweisungs Operatoren*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2628">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="20699-2629">Kein anderer Zuweisungs Operator ist mit einem Ereignis Zugriff als Linker Operand gültig.</span><span class="sxs-lookup"><span data-stu-id="20699-2629">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="20699-2630">Die Ereignis Zuweisungs Operatoren werden unter [Ereignis Zuweisung](expressions.md#event-assignment)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-2630">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="20699-2631">Die Zuweisungs Operatoren sind rechts assoziativ, was bedeutet, dass Vorgänge von rechts nach Links gruppiert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2631">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="20699-2632">Beispielsweise wird ein Ausdruck in der Form `a = b = c` als `a = (b = c)` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-2632">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="20699-2633">Einfache Zuweisung</span><span class="sxs-lookup"><span data-stu-id="20699-2633">Simple assignment</span></span>

<span data-ttu-id="20699-2634">Der `=`-Operator wird als einfacher Zuweisungs Operator bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="20699-2634">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="20699-2635">Wenn der linke Operand einer einfachen Zuweisung das Format `E.P` oder `E[Ei]` aufweist, wobei `E` den Kompilier Zeittyp `dynamic` aufweist, wird die Zuweisung dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="20699-2635">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="20699-2636">In diesem Fall ist der Kompilier Zeittyp des Zuweisungs Ausdrucks `dynamic`, und die unten beschriebene Auflösung erfolgt zur Laufzeit basierend auf dem Lauf Zeittyp `E`.</span><span class="sxs-lookup"><span data-stu-id="20699-2636">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="20699-2637">In einer einfachen Zuweisung muss der rechte Operand ein Ausdruck sein, der implizit in den Typ des linken Operanden konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="20699-2637">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="20699-2638">Der-Vorgang weist den Wert des rechten Operanden der Variablen, der Eigenschaft oder dem Indexer-Element zu, die durch den linken Operanden angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2638">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="20699-2639">Das Ergebnis eines einfachen Zuweisungs Ausdrucks ist der Wert, der dem linken Operanden zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2639">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="20699-2640">Das Ergebnis hat denselben Typ wie der linke Operand und wird immer als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-2640">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="20699-2641">Wenn der linke Operand eine Eigenschaft oder ein Indexer-Zugriff ist, muss die Eigenschaft oder der Indexer über einen `set`-Accessor verfügen.</span><span class="sxs-lookup"><span data-stu-id="20699-2641">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="20699-2642">Wenn dies nicht der Fall ist, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2642">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="20699-2643">Die Lauf Zeit Verarbeitung einer einfachen Zuweisung der Form `x = y` besteht aus den folgenden Schritten:</span><span class="sxs-lookup"><span data-stu-id="20699-2643">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="20699-2644">Wenn `x` als Variable klassifiziert ist:</span><span class="sxs-lookup"><span data-stu-id="20699-2644">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="20699-2645">`x` wird ausgewertet, um die Variable zu entwickeln.</span><span class="sxs-lookup"><span data-stu-id="20699-2645">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="20699-2646">`y` wird ausgewertet und, falls erforderlich, durch eine implizite Konvertierung in den Typ `x` konvertiert ([implizite Konvertierungen](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="20699-2646">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="20699-2647">Wenn die von `x` angegebene Variable ein Array Element eines *reference_type*ist, wird eine Lauf Zeit Überprüfung durchgeführt, um sicherzustellen, dass der für `y` berechnete Wert mit der Array Instanz kompatibel ist, von der `x` ein Element ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2647">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="20699-2648">Die Überprüfung ist erfolgreich, wenn `y` `null` ist, oder wenn eine implizite Verweis Konvertierung ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions)) vom tatsächlichen Typ der Instanz vorhanden ist, auf die von `y` auf den eigentlichen Elementtyp der Array Instanz verwiesen wird, die `x` enthält.</span><span class="sxs-lookup"><span data-stu-id="20699-2648">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="20699-2649">Andernfalls wird eine `System.ArrayTypeMismatchException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="20699-2649">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="20699-2650">Der Wert, der sich aus der Auswertung und Konvertierung von `y` ergibt, wird an dem Speicherort gespeichert, der durch die Auswertung von `x` angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2650">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="20699-2651">Wenn `x` als Eigenschaft oder Indexer-Zugriff klassifiziert ist:</span><span class="sxs-lookup"><span data-stu-id="20699-2651">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="20699-2652">Der Instanzausdruck (wenn `x` nicht `static` ist) und die Argumentliste (wenn `x` ein Indexer-Zugriff ist), die `x` zugeordnet ist, werden ausgewertet, und die Ergebnisse werden im nachfolgenden `set`-Accessor-Aufruf verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-2652">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="20699-2653">`y` wird ausgewertet und, falls erforderlich, durch eine implizite Konvertierung in den Typ `x` konvertiert ([implizite Konvertierungen](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="20699-2653">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="20699-2654">Der `set`-Accessor von `x` wird mit dem für `y` berechneten Wert als `value`-Argument aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-2654">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="20699-2655">Die Array-Co-Varianz Regeln ([Array-Kovarianz](arrays.md#array-covariance)) erlauben, dass ein Wert eines Arraytyps `A[]` ein Verweis auf eine Instanz eines Arraytyps `B[]` ist, vorausgesetzt, dass eine implizite Verweis Konvertierung von `B` in `A` vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2655">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="20699-2656">Aufgrund dieser Regeln erfordert die Zuweisung zu einem Array Element eines *reference_type* eine Lauf Zeit Überprüfung, um sicherzustellen, dass der zugewiesene Wert mit der Array Instanz kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2656">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="20699-2657">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2657">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="20699-2658">die letzte Zuweisung bewirkt, dass eine `System.ArrayTypeMismatchException` ausgelöst wird, weil eine Instanz von `ArrayList` nicht in einem Element eines `string[]` gespeichert werden kann.</span><span class="sxs-lookup"><span data-stu-id="20699-2658">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="20699-2659">Wenn eine Eigenschaft oder ein Indexer, die in einem *struct_type* deklariert ist, das Ziel einer Zuweisung ist, muss der Instanzausdruck, der der Eigenschaft oder dem Indexer-Zugriff zugeordnet ist, als Variable klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2659">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="20699-2660">Wenn der Instanzausdruck als Wert klassifiziert wird, tritt ein Fehler bei der Bindung auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2660">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="20699-2661">Aufgrund des [Mitglieds Zugriffs](expressions.md#member-access)gilt die gleiche Regel auch für Felder.</span><span class="sxs-lookup"><span data-stu-id="20699-2661">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="20699-2662">Bei den Deklarationen:</span><span class="sxs-lookup"><span data-stu-id="20699-2662">Given the declarations:</span></span>
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
<span data-ttu-id="20699-2663">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2663">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="20699-2664">die Zuweisungen zu `p.X`, `p.Y`, `r.A` und `r.B` sind zulässig, da `p` und `r` Variablen sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2664">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="20699-2665">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2665">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="20699-2666">die Zuweisungen sind ungültig, da `r.A` und `r.B` keine Variablen sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2666">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="20699-2667">Verbundzuweisung</span><span class="sxs-lookup"><span data-stu-id="20699-2667">Compound assignment</span></span>

<span data-ttu-id="20699-2668">Wenn der linke Operand einer Verbund Zuweisung das Format `E.P` oder `E[Ei]` aufweist, wobei `E` den Kompilier Zeittyp `dynamic` aufweist, wird die Zuweisung dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="20699-2668">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="20699-2669">In diesem Fall ist der Kompilier Zeittyp des Zuweisungs Ausdrucks `dynamic`, und die unten beschriebene Auflösung erfolgt zur Laufzeit basierend auf dem Lauf Zeittyp `E`.</span><span class="sxs-lookup"><span data-stu-id="20699-2669">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="20699-2670">Ein Vorgang in der Form `x op= y` wird durch Anwenden der Überladungs Auflösung für binäre Operatoren ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) verarbeitet, als wäre der Vorgang `x op y` geschrieben worden.</span><span class="sxs-lookup"><span data-stu-id="20699-2670">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="20699-2671">Seitdem</span><span class="sxs-lookup"><span data-stu-id="20699-2671">Then,</span></span>

*  <span data-ttu-id="20699-2672">Wenn der Rückgabetyp des ausgewählten Operators implizit in den Typ von `x` konvertiert werden kann, wird der Vorgang als `x = x op y` ausgewertet, mit dem Unterschied, dass `x` nur einmal ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2672">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="20699-2673">Andernfalls, wenn der ausgewählte Operator ein vordefinierter Operator ist,, wenn der Rückgabetyp des ausgewählten Operators explizit in den Typ von `x` konvertiert werden kann, und wenn `y` implizit in den Typ von `x` konvertiert werden kann oder wenn der Operator ein Verschiebungs Operator ist. , wird der Vorgang als `x = (T)(x op y)` ausgewertet, wobei `T` der Typ von `x` ist, mit dem Unterschied, dass `x` nur einmal ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2673">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="20699-2674">Andernfalls ist die Verbund Zuweisung ungültig, und es tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2674">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="20699-2675">Der Begriff "nur einmal ausgewertet" bedeutet, dass bei der Auswertung von "`x op y`" die Ergebnisse der Bestandteile von `x` temporär gespeichert und dann wieder verwendet werden, wenn die Zuweisung zu `x` durchgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2675">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="20699-2676">Beispielsweise wird im Zuweisungs `A()[B()] += C()`, wobei `A` eine Methode ist, die `int[]` zurückgibt, und `B` und `C` sind Methoden, die `int` zurückgeben, die Methoden nur einmal aufgerufen, in der Reihenfolge `A`, `B` `C`.</span><span class="sxs-lookup"><span data-stu-id="20699-2676">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="20699-2677">Wenn der linke Operand einer Verbund Zuweisung ein Eigenschaften Zugriff oder Indexer-Zugriff ist, muss die Eigenschaft oder der Indexer sowohl einen `get`-Accessor als auch einen `set`-Accessor haben.</span><span class="sxs-lookup"><span data-stu-id="20699-2677">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="20699-2678">Wenn dies nicht der Fall ist, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2678">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="20699-2679">Mit der zweiten obigen Regel können `x op= y` in bestimmten Kontexten als `x = (T)(x op y)` ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="20699-2679">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="20699-2680">Die Regel besteht darin, dass die vordefinierten Operatoren als Verbund Operatoren verwendet werden können, wenn der linke Operand vom Typ `sbyte`, `byte`, `short`, `ushort` oder `char` ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2680">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="20699-2681">Auch wenn beide Argumente von einem dieser Typen sind, führen die vordefinierten Operatoren ein Ergebnis vom Typ `int` aus, wie in [Binäre numerische](expressions.md#binary-numeric-promotions)Erweiterungen beschrieben.</span><span class="sxs-lookup"><span data-stu-id="20699-2681">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="20699-2682">Daher wäre es ohne Umwandlung nicht möglich, das Ergebnis dem linken Operanden zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="20699-2682">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="20699-2683">Die intuitive Auswirkung der Regel auf vordefinierte Operatoren besteht darin, dass `x op= y` zulässig ist, wenn sowohl `x op y` als auch `x = y` zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2683">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="20699-2684">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2684">In the example</span></span>
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
<span data-ttu-id="20699-2685">der intuitive Grund für jeden Fehler ist, dass eine entsprechende einfache Zuweisung ebenfalls ein Fehler wäre.</span><span class="sxs-lookup"><span data-stu-id="20699-2685">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="20699-2686">Dies bedeutet auch, dass aufgesetzte Zuweisungs Vorgänge aufgesetzte Vorgänge unterstützen.</span><span class="sxs-lookup"><span data-stu-id="20699-2686">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="20699-2687">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="20699-2687">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="20699-2688">der angehobene Operator `+(int?,int?)` wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="20699-2688">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="20699-2689">Ereignis Zuweisung</span><span class="sxs-lookup"><span data-stu-id="20699-2689">Event assignment</span></span>

<span data-ttu-id="20699-2690">Wenn der linke Operand eines `+=`-oder `-=`-Operators als Ereignis Zugriff klassifiziert ist, wird der Ausdruck wie folgt ausgewertet:</span><span class="sxs-lookup"><span data-stu-id="20699-2690">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="20699-2691">Der Instanzausdruck des Ereignis Zugriffs wird ausgewertet, falls vorhanden.</span><span class="sxs-lookup"><span data-stu-id="20699-2691">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="20699-2692">Der rechte Operand des `+=`-oder `-=`-Operators wird ausgewertet und, falls erforderlich, durch eine implizite Konvertierung in den Typ des linken Operanden konvertiert ([implizite Konvertierungen](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="20699-2692">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="20699-2693">Ein Ereignis Accessor des Ereignisses wird mit einer Argumentliste aufgerufen, die den rechten Operanden nach der Auswertung und ggf. Konvertierung umfasst.</span><span class="sxs-lookup"><span data-stu-id="20699-2693">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="20699-2694">Wenn der Operator `+=` ist, wird der `add`-Accessor aufgerufen. Wenn der Operator `-=` ist, wird der `remove`-Accessor aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="20699-2694">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="20699-2695">Ein Ereignis Zuweisungs Ausdruck ergibt keinen Wert.</span><span class="sxs-lookup"><span data-stu-id="20699-2695">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="20699-2696">Daher ist ein Ereignis Zuweisungs Ausdruck nur im Kontext einer *statement_expression* ([Ausdrucks Anweisungen](statements.md#expression-statements)) gültig.</span><span class="sxs-lookup"><span data-stu-id="20699-2696">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="20699-2697">expression</span><span class="sxs-lookup"><span data-stu-id="20699-2697">Expression</span></span>

<span data-ttu-id="20699-2698">Ein *Ausdruck* ist entweder ein *non_assignment_expression* oder eine *Zuweisung*.</span><span class="sxs-lookup"><span data-stu-id="20699-2698">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

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

## <a name="constant-expressions"></a><span data-ttu-id="20699-2699">Konstante Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-2699">Constant expressions</span></span>

<span data-ttu-id="20699-2700">Ein *constant_expression* ist ein Ausdruck, der zur Kompilierzeit vollständig ausgewertet werden kann.</span><span class="sxs-lookup"><span data-stu-id="20699-2700">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="20699-2701">Ein konstanter Ausdruck muss das `null`-Literale oder ein Wert mit einem der folgenden Typen sein: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3, 4 , 5 oder ein beliebiger Enumerationstyp.</span><span class="sxs-lookup"><span data-stu-id="20699-2701">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="20699-2702">Nur die folgenden Konstrukte sind in konstanten Ausdrücken zulässig:</span><span class="sxs-lookup"><span data-stu-id="20699-2702">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="20699-2703">Literale (einschließlich des `null`-Literals).</span><span class="sxs-lookup"><span data-stu-id="20699-2703">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="20699-2704">Verweise auf `const`-Member der Klassen-und Strukturtypen.</span><span class="sxs-lookup"><span data-stu-id="20699-2704">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="20699-2705">Verweise auf Member von Enumerationstypen.</span><span class="sxs-lookup"><span data-stu-id="20699-2705">References to members of enumeration types.</span></span>
*  <span data-ttu-id="20699-2706">Verweise auf @no__t 0-Parameter oder lokale Variablen</span><span class="sxs-lookup"><span data-stu-id="20699-2706">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="20699-2707">Unter Ausdrücke in Klammern, die selbst Konstante Ausdrücke sind.</span><span class="sxs-lookup"><span data-stu-id="20699-2707">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="20699-2708">Umwandlungs Ausdrücke, wenn der Zieltyp einem der oben aufgeführten Typen entspricht.</span><span class="sxs-lookup"><span data-stu-id="20699-2708">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="20699-2709">`checked`-und `unchecked`-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-2709">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="20699-2710">Standardwert Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-2710">Default value expressions</span></span>
*  <span data-ttu-id="20699-2711">Nameof-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-2711">Nameof expressions</span></span>
*  <span data-ttu-id="20699-2712">Die vordefinierten `+`, `-`, `!` und `~` unäre Operatoren.</span><span class="sxs-lookup"><span data-stu-id="20699-2712">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="20699-2713">Der vordefinierte `+`, `-`, `*`-, `/`-, `%`-, `<<`-, `>>`-, `&`-, `|`-, `^`-, 0-, 1-, 4-, @no__t-, 6-und 7-Operatoren der oben aufgeführte Typ.</span><span class="sxs-lookup"><span data-stu-id="20699-2713">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="20699-2714">Der bedingte Operator "`?:`".</span><span class="sxs-lookup"><span data-stu-id="20699-2714">The `?:` conditional operator.</span></span>

<span data-ttu-id="20699-2715">Die folgenden Konvertierungen sind in konstanten Ausdrücken zulässig:</span><span class="sxs-lookup"><span data-stu-id="20699-2715">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="20699-2716">Identitäts Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="20699-2716">Identity conversions</span></span>
*  <span data-ttu-id="20699-2717">Numerische Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="20699-2717">Numeric conversions</span></span>
*  <span data-ttu-id="20699-2718">Enumerationskonvertierungen</span><span class="sxs-lookup"><span data-stu-id="20699-2718">Enumeration conversions</span></span>
*  <span data-ttu-id="20699-2719">Konstante Ausdrucks Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="20699-2719">Constant expression conversions</span></span>
*  <span data-ttu-id="20699-2720">Implizite und explizite Verweis Konvertierungen, vorausgesetzt, dass die Quelle der Konvertierungen ein konstanter Ausdruck ist, der zu einem NULL-Wert ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="20699-2720">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="20699-2721">Andere Konvertierungen, einschließlich Boxing, Unboxing und impliziter Verweis Konvertierungen von nicht-NULL-Werten, sind in konstanten Ausdrücken nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="20699-2721">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="20699-2722">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="20699-2722">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="20699-2723">die Initialisierung von i ist ein Fehler, da eine Boxing-Konvertierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2723">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="20699-2724">Die Initialisierung von Str ist ein Fehler, da eine implizite Verweis Konvertierung von einem nicht-NULL-Wert erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="20699-2724">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="20699-2725">Wenn ein Ausdruck die oben aufgeführten Anforderungen erfüllt, wird der Ausdruck zur Kompilierzeit ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="20699-2725">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="20699-2726">Dies gilt auch, wenn der Ausdruck ein unter Ausdruck eines größeren Ausdrucks ist, der nicht konstante Konstrukte enthält.</span><span class="sxs-lookup"><span data-stu-id="20699-2726">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="20699-2727">Die Kompilierzeit Auswertung konstanter Ausdrücke verwendet dieselben Regeln wie die Lauf Zeit Auswertung von nicht konstanten Ausdrücken, mit dem Unterschied, dass die Kompilierzeit Auswertung dazu führt, dass ein Kompilierzeitfehler auftritt, wenn die Lauf Zeit Auswertung eine Ausnahme ausgelöst hätte.</span><span class="sxs-lookup"><span data-stu-id="20699-2727">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="20699-2728">Wenn ein konstanter Ausdruck nicht explizit in einem `unchecked`-Kontext platziert wird, verursachen über Flüsse, die in arithmetischen Operationen für ganzzahlige Typen und Konvertierungen während der Kompilierzeit Auswertung des Ausdrucks auftreten, immer Kompilierzeitfehler ([konstant Ausdrücke](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="20699-2728">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="20699-2729">In den unten aufgeführten Kontexten treten Konstante Ausdrücke auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2729">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="20699-2730">In diesen Kontexten tritt ein Kompilierzeitfehler auf, wenn ein Ausdruck zur Kompilierzeit nicht vollständig ausgewertet werden kann.</span><span class="sxs-lookup"><span data-stu-id="20699-2730">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="20699-2731">Konstante Deklarationen ([Konstanten](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="20699-2731">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="20699-2732">Deklarationen von Enumerationsmembern ([enum](enums.md#enum-members)-Member).</span><span class="sxs-lookup"><span data-stu-id="20699-2732">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="20699-2733">Standardargumente von formalen Parameterlisten ([Methoden Parameter](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="20699-2733">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="20699-2734">`case`-Bezeichnungen einer `switch`-Anweisung ([Switch-Anweisung](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="20699-2734">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="20699-2735">`goto case`-Anweisungen ([die GoTo-Anweisung](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="20699-2735">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="20699-2736">Dimensions Längen in einem Array Erstellungs Ausdruck ([Array Erstellungs Ausdrücke](expressions.md#array-creation-expressions)), der einen Initialisierer einschließt.</span><span class="sxs-lookup"><span data-stu-id="20699-2736">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="20699-2737">Attribute ([Attribute](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="20699-2737">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="20699-2738">Eine implizite Konstante Ausdrucks Konvertierung ([implizite Konstante Ausdrucks Konvertierungen](conversions.md#implicit-constant-expression-conversions)) ermöglicht das Konvertieren eines konstanten Ausdrucks vom Typ `int` in `sbyte`, `byte`, `short`, `ushort`, `uint` oder `ulong`, sofern der Wert des der Konstante Ausdruck liegt innerhalb des Bereichs des Zieltyps.</span><span class="sxs-lookup"><span data-stu-id="20699-2738">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="20699-2739">Boolesche Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="20699-2739">Boolean expressions</span></span>

<span data-ttu-id="20699-2740">Ein *Boolean_expression* ist ein Ausdruck, der ein Ergebnis vom Typ `bool` ergibt. entweder direkt oder über die Anwendung von `operator true` in bestimmten Kontexten, wie im folgenden angegeben.</span><span class="sxs-lookup"><span data-stu-id="20699-2740">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="20699-2741">Der steuernde bedingte Ausdruck eines *if_statement* ([die if-Anweisung](statements.md#the-if-statement)), *while_statement* ([die while](statements.md#the-while-statement)-Anweisung), *do_statement* ([die Do-Anweisung](statements.md#the-do-statement)) oder *for_Statement* ([für Statement](statements.md#the-for-statement)) ist eine *Boolean_expression*.</span><span class="sxs-lookup"><span data-stu-id="20699-2741">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="20699-2742">Der steuernde bedingte Ausdruck des `?:`-Operators ([Bedingter Operator](expressions.md#conditional-operator)) folgt denselben Regeln wie ein *Boolean_expression*, aber aus Gründen der Operator Rangfolge wird Sie als *conditional_or_expression*klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="20699-2742">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="20699-2743">Ein *Boolean_expression* -`E` ist erforderlich, um in der Lage zu sein, einen Wert vom Typ "`bool`" zu erhalten, wie folgt:</span><span class="sxs-lookup"><span data-stu-id="20699-2743">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="20699-2744">Wenn `E` implizit in `bool` konvertiert werden kann, wird zur Laufzeit die implizite Konvertierung angewendet.</span><span class="sxs-lookup"><span data-stu-id="20699-2744">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="20699-2745">Andernfalls wird die Überladungs Auflösung des unären Operators ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) verwendet, um eine eindeutige beste Implementierung von Operator `true` auf `E` zu finden, und diese Implementierung wird zur Laufzeit angewendet.</span><span class="sxs-lookup"><span data-stu-id="20699-2745">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="20699-2746">Wenn kein solcher Operator gefunden wird, tritt ein Bindungs Zeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="20699-2746">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="20699-2747">Der `DBBool`-Strukturtyp in einem [booleschen Datenbanktyp](structs.md#database-boolean-type) bietet ein Beispiel für einen Typ, der `operator true` und `operator false` implementiert.</span><span class="sxs-lookup"><span data-stu-id="20699-2747">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
