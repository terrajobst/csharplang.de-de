---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484069"
---
# <a name="pattern-matching-for-c-7"></a><span data-ttu-id="b7992-101">Musterabgleich C# für 7</span><span class="sxs-lookup"><span data-stu-id="b7992-101">Pattern Matching for C# 7</span></span>

<span data-ttu-id="b7992-102">Muster Vergleichs Erweiterungen für C# ermöglichen viele der Vorteile von algebraischen Datentypen und Musterabgleich von funktionalen Sprachen, aber auf eine Weise, die nahtlos in das Verhalten der zugrunde liegenden Sprache integriert ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-102">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="b7992-103">Die grundlegenden Features sind: [Daten Satz Typen](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), bei denen es sich um Typen handelt, deren semantische Bedeutung durch die Form der Daten beschrieben wird. und Musterabgleich, bei dem es sich um ein neues Ausdrucks Formular handelt, das eine extrem präzise Zerlegung dieser Datentypen ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="b7992-103">The basic features are: [record types](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), which are types whose semantic meaning is described by the shape of the data; and pattern matching, which is a new expression form that enables extremely concise multilevel decomposition of these data types.</span></span> <span data-ttu-id="b7992-104">Elemente dieses Ansatzes sind von verwandten Features in den Programmiersprachen [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Erweiterbarer Musterabgleich über eine vereinfachte Sprache") und [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Abgleichen von Objekten mit Mustern")inspiriert.</span><span class="sxs-lookup"><span data-stu-id="b7992-104">Elements of this approach are inspired by related features in the programming languages [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Matching Objects With Patterns").</span></span>

## <a name="is-expression"></a><span data-ttu-id="b7992-105">Is-Ausdruck</span><span class="sxs-lookup"><span data-stu-id="b7992-105">Is expression</span></span>

<span data-ttu-id="b7992-106">Der `is`-Operator wird erweitert, um einen Ausdruck mit einem *Muster*zu testen.</span><span class="sxs-lookup"><span data-stu-id="b7992-106">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="b7992-107">Diese Form von *relational_expression* wird zusätzlich zu den in der C# Spezifikation vorhandenen Formularen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b7992-107">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="b7992-108">Es handelt sich um einen Kompilierzeitfehler, wenn der *relational_expression* auf der linken Seite des `is`-Tokens keinen Wert oder keinen Typ festgelegt hat.</span><span class="sxs-lookup"><span data-stu-id="b7992-108">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="b7992-109">Jeder *Bezeichner* des Musters führt eine neue lokale Variable ein, die *definitiv zugewiesen* wird, nachdem der `is`-Operator `true` wurde (d.h. *definitiv zugewiesen, wenn true*).</span><span class="sxs-lookup"><span data-stu-id="b7992-109">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="b7992-110">Hinweis: Es gibt technisch gesehen eine Mehrdeutigkeit zwischen dem *Typ* in einer `is-expression` und *constant_pattern*, von denen jede eine gültige Analyse eines qualifizierten Bezeichners sein könnte.</span><span class="sxs-lookup"><span data-stu-id="b7992-110">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="b7992-111">Wir versuchen, Sie als Typ für die Kompatibilität mit früheren Versionen der Sprache zu binden. nur wenn dies nicht möglich ist, lösen wir es wie in anderen Kontexten, bis zum ersten gefundenen (das entweder eine Konstante oder ein Typ sein muss).</span><span class="sxs-lookup"><span data-stu-id="b7992-111">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="b7992-112">Diese Mehrdeutigkeit ist nur auf der rechten Seite eines `is` Ausdrucks vorhanden.</span><span class="sxs-lookup"><span data-stu-id="b7992-112">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

## <a name="patterns"></a><span data-ttu-id="b7992-113">Muster</span><span class="sxs-lookup"><span data-stu-id="b7992-113">Patterns</span></span>

<span data-ttu-id="b7992-114">Muster werden im `is` Operator und in einem *switch_statement* verwendet, um die Form der Daten auszudrücken, mit denen eingehende Daten verglichen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="b7992-114">Patterns are used in the `is` operator and in a *switch_statement* to express the shape of data against which incoming data is to be compared.</span></span> <span data-ttu-id="b7992-115">Muster können rekursiv sein, damit Teile der Daten mit unter Mustern verglichen werden können.</span><span class="sxs-lookup"><span data-stu-id="b7992-115">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> <span data-ttu-id="b7992-116">Hinweis: Es gibt technisch gesehen eine Mehrdeutigkeit zwischen dem *Typ* in einer `is-expression` und *constant_pattern*, von denen jede eine gültige Analyse eines qualifizierten Bezeichners sein könnte.</span><span class="sxs-lookup"><span data-stu-id="b7992-116">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="b7992-117">Wir versuchen, Sie als Typ für die Kompatibilität mit früheren Versionen der Sprache zu binden. nur wenn dies nicht möglich ist, lösen wir es wie in anderen Kontexten, bis zum ersten gefundenen (das entweder eine Konstante oder ein Typ sein muss).</span><span class="sxs-lookup"><span data-stu-id="b7992-117">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="b7992-118">Diese Mehrdeutigkeit ist nur auf der rechten Seite eines `is` Ausdrucks vorhanden.</span><span class="sxs-lookup"><span data-stu-id="b7992-118">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="declaration-pattern"></a><span data-ttu-id="b7992-119">Deklarations Muster</span><span class="sxs-lookup"><span data-stu-id="b7992-119">Declaration pattern</span></span>

<span data-ttu-id="b7992-120">Der *declaration_pattern* testet, ob ein Ausdruck einen bestimmten Typ hat, und wandelt ihn in diesen Typ um, wenn der Test erfolgreich ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-120">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="b7992-121">Wenn die *simple_designation* ein Bezeichner ist, wird eine lokale Variable des angegebenen Typs eingeführt, der durch den angegebenen Bezeichner benannt wird.</span><span class="sxs-lookup"><span data-stu-id="b7992-121">If the *simple_designation* is an identifier, it introduces a local variable of the given type named by the given identifier.</span></span> <span data-ttu-id="b7992-122">Diese lokale Variable ist *definitiv zugewiesen* , wenn das Ergebnis der Muster Vergleichsoperation "true" ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-122">That local variable is *definitely assigned* when the result of the pattern-matching operation is true.</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="b7992-123">Die Lauf Zeit Semantik dieses Ausdrucks besteht darin, dass er den Lauf Zeittyp des linken *relational_expression* Operanden mit dem *Typ* im Muster testet.</span><span class="sxs-lookup"><span data-stu-id="b7992-123">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span> <span data-ttu-id="b7992-124">Wenn es sich um einen Lauf Zeittyp (oder einen Untertyp) handelt, wird das Ergebnis des `is operator` `true`.</span><span class="sxs-lookup"><span data-stu-id="b7992-124">If it is of that runtime type (or some subtype), the result of the `is operator` is `true`.</span></span> <span data-ttu-id="b7992-125">Er deklariert eine neue lokale Variable namens durch den *Bezeichner* , dem der Wert des linken Operanden zugewiesen wird, wenn das Ergebnis `true`ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-125">It declares a new local variable named by the *identifier* that is assigned the value of the left-hand operand when the result is `true`.</span></span>

<span data-ttu-id="b7992-126">Bestimmte Kombinationen von statischem Typ der linken Seite und des angegebenen Typs werden als nicht kompatibel betrachtet und führen zu einem Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="b7992-126">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="b7992-127">Ein Wert vom Typ "statischer Typ `E`" ist mit dem Typ " *Muster kompatibel* " `T`, wenn eine Identitäts Konvertierung, eine implizite Verweis Konvertierung, eine Boxing-Konvertierung, eine explizite Verweis Konvertierung oder eine Unboxing-Konvertierung von `E` in `T`vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-127">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`.</span></span> <span data-ttu-id="b7992-128">Es handelt sich um einen Kompilierzeitfehler, wenn ein Ausdruck vom Typ `E` nicht mit dem Typ in einem Typmuster kompatibel ist, mit dem er übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="b7992-128">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

> <span data-ttu-id="b7992-129">Hinweis: [in C# 7,1 erweitern wir dies](../csharp-7.1/generics-pattern-match.md) , um einen Muster Vergleichs Vorgang zuzulassen, wenn entweder der Eingabetyp oder der Typ `T` ein offener Typ ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-129">Note: [In C# 7.1 we extend this](../csharp-7.1/generics-pattern-match.md) to permit a pattern-matching operation if either the input type or the type `T` is an open type.</span></span> <span data-ttu-id="b7992-130">Dieser Absatz wird durch Folgendes ersetzt:</span><span class="sxs-lookup"><span data-stu-id="b7992-130">This paragraph is replaced by the following:</span></span>
> 
> <span data-ttu-id="b7992-131">Bestimmte Kombinationen von statischem Typ der linken Seite und des angegebenen Typs werden als nicht kompatibel betrachtet und führen zu einem Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="b7992-131">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="b7992-132">Ein Wert vom Typ "statischer Typ `E`" ist mit dem Typ " *Muster kompatibel* " `T`, wenn eine Identitäts Konvertierung, eine implizite Verweis Konvertierung, eine Boxing-Konvertierung, eine explizite Verweis Konvertierung oder eine Unboxing-Konvertierung von `E` in `T`vorhanden **ist, oder wenn entweder `E` oder `T` ein offener Typ ist**.</span><span class="sxs-lookup"><span data-stu-id="b7992-132">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, **or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="b7992-133">Es handelt sich um einen Kompilierzeitfehler, wenn ein Ausdruck vom Typ `E` nicht mit dem Typ in einem Typmuster kompatibel ist, mit dem er übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="b7992-133">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

<span data-ttu-id="b7992-134">Das Deklarations Muster ist nützlich zum Ausführen von Lauf Zeittyp Tests von Verweis Typen und ersetzt die Ausdrucksweise.</span><span class="sxs-lookup"><span data-stu-id="b7992-134">The declaration pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

<span data-ttu-id="b7992-135">Mit etwas präziseren</span><span class="sxs-lookup"><span data-stu-id="b7992-135">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v }
```

<span data-ttu-id="b7992-136">Es ist ein Fehler, wenn der *Typ* ein Werte zulässt-Werttyp ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-136">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="b7992-137">Das Deklarations Muster kann verwendet werden, um Werte von Typen zu testen, die NULL-Werte zulassen: ein Wert vom Typ `Nullable<T>` (oder ein gebocktes `T`) entspricht einem Typmuster `T2 id` wenn der Wert nicht NULL ist und der Typ der `T2` `T`oder ein Basistyp oder eine Schnittstelle `T`ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-137">The declaration pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="b7992-138">Beispielsweise im Code Fragment</span><span class="sxs-lookup"><span data-stu-id="b7992-138">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

<span data-ttu-id="b7992-139">Die Bedingung der `if`-Anweisung wird zur Laufzeit `true`, und die Variable `v` enthält den Wert `3` vom Typ `int` innerhalb des Blocks.</span><span class="sxs-lookup"><span data-stu-id="b7992-139">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span>

### <a name="constant-pattern"></a><span data-ttu-id="b7992-140">Konstantenmuster</span><span class="sxs-lookup"><span data-stu-id="b7992-140">Constant pattern</span></span>

```antlr
constant_pattern
    : shift_expression
    ;
```

<span data-ttu-id="b7992-141">Ein konstantes Muster testet den Wert eines Ausdrucks mit einem konstanten Wert.</span><span class="sxs-lookup"><span data-stu-id="b7992-141">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="b7992-142">Die Konstante kann ein beliebiger konstanter Ausdruck sein, z. b. ein Literalwert, der Name einer deklarierten `const` Variablen oder eine Enumerationskonstante oder ein `typeof` Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="b7992-142">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant, or a `typeof` expression.</span></span>

<span data-ttu-id="b7992-143">Wenn sowohl *e* als auch *c* ganzzahlige Typen sind, wird das Muster als Übereinstimmung betrachtet, wenn das Ergebnis des Ausdrucks `e == c` `true`ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-143">If both *e* and *c* are of integral types, the pattern is considered matched if the result of the expression `e == c` is `true`.</span></span>

<span data-ttu-id="b7992-144">Andernfalls wird das Muster als übereinstimmend betrachtet, wenn `object.Equals(e, c)` `true`zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="b7992-144">Otherwise the pattern is considered matching if `object.Equals(e, c)` returns `true`.</span></span> <span data-ttu-id="b7992-145">In diesem Fall handelt es sich um einen Kompilierzeitfehler, wenn der statische Typ von *e* nicht mit dem Typ der Konstante *kompatibel* ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-145">In this case it is a compile-time error if the static type of *e* is not *pattern compatible* with the type of the constant.</span></span>

### <a name="var-pattern"></a><span data-ttu-id="b7992-146">Var-Muster</span><span class="sxs-lookup"><span data-stu-id="b7992-146">Var pattern</span></span>

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

<span data-ttu-id="b7992-147">Ein Ausdruck *e* stimmt mit einem *var_pattern* immer überein.</span><span class="sxs-lookup"><span data-stu-id="b7992-147">An expression *e* matches a *var_pattern* always.</span></span> <span data-ttu-id="b7992-148">Anders ausgedrückt, eine Entsprechung für ein *var-Muster* ist immer erfolgreich.</span><span class="sxs-lookup"><span data-stu-id="b7992-148">In other words, a match to a *var pattern* always succeeds.</span></span> <span data-ttu-id="b7992-149">Wenn die *simple_designation* ein Bezeichner ist, wird zur Laufzeit der Wert von *e* an eine neu eingeführte lokale Variable gebunden.</span><span class="sxs-lookup"><span data-stu-id="b7992-149">If the *simple_designation* is an identifier, then at runtime the value of *e* is bound to a newly introduced local variable.</span></span> <span data-ttu-id="b7992-150">Der Typ der lokalen Variablen ist der statische Typ von *e*.</span><span class="sxs-lookup"><span data-stu-id="b7992-150">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="b7992-151">Wenn der Name `var` an einen Typ bindet, ist ein Fehler aufgetreten.</span><span class="sxs-lookup"><span data-stu-id="b7992-151">It is an error if the name `var` binds to a type.</span></span>

## <a name="switch-statement"></a><span data-ttu-id="b7992-152">switch-Anweisung</span><span class="sxs-lookup"><span data-stu-id="b7992-152">Switch statement</span></span>

<span data-ttu-id="b7992-153">Die `switch`-Anweisung wird erweitert, um für die Ausführung auszuwählen. der erste Block verfügt über ein zugeordnetes Muster, das dem *Switch-Ausdruck*entspricht</span><span class="sxs-lookup"><span data-stu-id="b7992-153">The `switch` statement is extended to select for execution the first block having an associated pattern that matches the *switch expression*.</span></span>

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

<span data-ttu-id="b7992-154">Die Reihenfolge, in der Muster abgeglichen werden, ist nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="b7992-154">The order in which patterns are matched is not defined.</span></span> <span data-ttu-id="b7992-155">Ein Compiler ist berechtigt, Muster in falscher Reihenfolge abzugleichen und die Ergebnisse von bereits übereinstimmenden Mustern wiederzuverwenden, um das Ergebnis der Übereinstimmung von anderen Mustern zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="b7992-155">A compiler is permitted to match patterns out of order, and to reuse the results of already matched patterns to compute the result of matching of other patterns.</span></span>

<span data-ttu-id="b7992-156">Wenn ein *Case-Guard* vorhanden ist, ist der Ausdruck vom Typ `bool`.</span><span class="sxs-lookup"><span data-stu-id="b7992-156">If a *case-guard* is present, its expression is of type `bool`.</span></span> <span data-ttu-id="b7992-157">Es wird als zusätzliche Bedingung ausgewertet, die erfüllt werden muss, damit die Groß-/Kleinschreibung als erfüllt betrachtet wird.</span><span class="sxs-lookup"><span data-stu-id="b7992-157">It is evaluated as an additional condition that must be satisfied for the case to be considered satisfied.</span></span>

<span data-ttu-id="b7992-158">Es tritt ein Fehler auf, wenn ein *switch_label* zur Laufzeit keine Auswirkung haben kann, da sein Muster in vorherigen Fällen subsudiert wird.</span><span class="sxs-lookup"><span data-stu-id="b7992-158">It is an error if a *switch_label* can have no effect at runtime because its pattern is subsumed by previous cases.</span></span> <span data-ttu-id="b7992-159">[TODO: Wir sollten präziser zu den Techniken sein, die der Compiler zum Erreichen dieses Urteils benötigt.]</span><span class="sxs-lookup"><span data-stu-id="b7992-159">[TODO: We should be more precise about the techniques the compiler is required to use to reach this judgment.]</span></span>

<span data-ttu-id="b7992-160">Eine in einem *switch_label* deklarierte Muster Variable wird in Ihrem Fall Block definitiv zugewiesen, wenn dieser Fall Block genau einen *switch_label*enthält.</span><span class="sxs-lookup"><span data-stu-id="b7992-160">A pattern variable declared in a *switch_label* is definitely assigned in its case block if and only if that case block contains precisely one *switch_label*.</span></span>

<span data-ttu-id="b7992-161">[TODO: Wir sollten angeben, wann ein *switch-Block* erreichbar ist.]</span><span class="sxs-lookup"><span data-stu-id="b7992-161">[TODO: We should specify when a *switch block* is reachable.]</span></span>

### <a name="scope-of-pattern-variables"></a><span data-ttu-id="b7992-162">Bereich von Muster Variablen</span><span class="sxs-lookup"><span data-stu-id="b7992-162">Scope of pattern variables</span></span>

<span data-ttu-id="b7992-163">Der Gültigkeitsbereich einer Variablen, die in einem Muster deklariert ist, lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="b7992-163">The scope of a variable declared in a pattern is as follows:</span></span>

- <span data-ttu-id="b7992-164">Wenn das Muster eine Case-Bezeichnung ist, ist der Gültigkeitsbereich der Variablen der *Case-Block*.</span><span class="sxs-lookup"><span data-stu-id="b7992-164">If the pattern is a case label, then the scope of the variable is the *case block*.</span></span>

<span data-ttu-id="b7992-165">Andernfalls wird die Variable in einem *is_pattern* Ausdruck deklariert, und ihr Bereich basiert auf dem-Konstrukt, das den Ausdruck, der den *is_pattern* Ausdruck enthält, unmittelbar wie folgt umschließt:</span><span class="sxs-lookup"><span data-stu-id="b7992-165">Otherwise the variable is declared in an *is_pattern* expression, and its scope is based on the construct immediately enclosing the expression containing the *is_pattern* expression as follows:</span></span>

- <span data-ttu-id="b7992-166">Wenn sich der Ausdruck in einem Ausdrucks Körper-Lambda befindet, ist sein Bereich der Text des Lambda-Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="b7992-166">If the expression is in an expression-bodied lambda, its scope is the body of the lambda.</span></span>
- <span data-ttu-id="b7992-167">Wenn sich der Ausdruck in einer Ausdrucks Körper-Methode oder-Eigenschaft befindet, ist sein Bereich der Text der Methode oder Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="b7992-167">If the expression is in an expression-bodied method or property, its scope is the body of the method or property.</span></span>
- <span data-ttu-id="b7992-168">Wenn sich der Ausdruck in einer `when`-Klausel einer `catch`-Klausel befindet, ist der Gültigkeitsbereich die `catch`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="b7992-168">If the expression is in a `when` clause of a `catch` clause, its scope is that `catch` clause.</span></span>
- <span data-ttu-id="b7992-169">Wenn sich der Ausdruck in einem *iteration_statement*befindet, ist sein Bereich nur diese Anweisung.</span><span class="sxs-lookup"><span data-stu-id="b7992-169">If the expression is in an *iteration_statement*, its scope is just that statement.</span></span>
- <span data-ttu-id="b7992-170">Andernfalls ist der Gültigkeitsbereich der Bereich, der die Anweisung enthält, wenn sich der Ausdruck in einem anderen Anweisungs Formular befindet.</span><span class="sxs-lookup"><span data-stu-id="b7992-170">Otherwise if the expression is in some other statement form, its scope is the scope containing the statement.</span></span>

<span data-ttu-id="b7992-171">Zum Ermitteln des Bereichs wird ein *embedded_statement* als in seinem eigenen Bereich betrachtet.</span><span class="sxs-lookup"><span data-stu-id="b7992-171">For the purpose of determining the scope, an *embedded_statement* is considered to be in its own scope.</span></span> <span data-ttu-id="b7992-172">Beispielsweise ist die Grammatik für eine *if_statement*</span><span class="sxs-lookup"><span data-stu-id="b7992-172">For example, the grammar for an *if_statement* is</span></span>

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="b7992-173">Wenn die gesteuerte Anweisung einer *if_statement* eine Muster Variable deklariert, ist der Gültigkeitsbereich auf diese *embedded_statement*beschränkt:</span><span class="sxs-lookup"><span data-stu-id="b7992-173">So if the controlled statement of an *if_statement* declares a pattern variable, its scope is restricted to that *embedded_statement*:</span></span>

```csharp
if (x) M(y is var z);
```

<span data-ttu-id="b7992-174">In diesem Fall ist der Bereich `z` die eingebettete Anweisung `M(y is var z);`.</span><span class="sxs-lookup"><span data-stu-id="b7992-174">In this case the scope of `z` is the embedded statement `M(y is var z);`.</span></span>

<span data-ttu-id="b7992-175">Andere Fälle sind Fehler aus anderen Gründen (z. b. im Standardwert eines Parameters oder in einem Attribut, bei denen es sich um einen Fehler handelt, da für diese Kontexte ein konstanter Ausdruck erforderlich ist).</span><span class="sxs-lookup"><span data-stu-id="b7992-175">Other cases are errors for other reasons (e.g. in a parameter's default value or an attribute, both of which are an error because those contexts require a constant expression).</span></span>

> <span data-ttu-id="b7992-176">[In C# 7,3 haben wir die folgenden Kontexte hinzugefügt](../csharp-7.3/expression-variables-in-initializers.md) , in denen eine Muster Variable deklariert werden kann:</span><span class="sxs-lookup"><span data-stu-id="b7992-176">[In C# 7.3 we added the following contexts](../csharp-7.3/expression-variables-in-initializers.md) in which a pattern variable may be declared:</span></span>
> - <span data-ttu-id="b7992-177">Wenn sich der Ausdruck in einem *Konstruktorinitialisierer*befindet, ist sein Bereich der *Konstruktorinitialisierer* und der Text des Konstruktors.</span><span class="sxs-lookup"><span data-stu-id="b7992-177">If the expression is in a *constructor initializer*, its scope is the *constructor initializer* and the constructor's body.</span></span>
> - <span data-ttu-id="b7992-178">Wenn sich der Ausdruck in einem Feldinitialisierer befindet, ist sein Bereich der *equals_value_clause* , in dem er angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="b7992-178">If the expression is in a field initializer, its scope is the *equals_value_clause* in which it appears.</span></span>
> - <span data-ttu-id="b7992-179">Wenn der Ausdruck in einer Abfrage Klausel enthalten ist, die angegeben wird, dass Sie in den Text eines Lambda-Ausdrucks übersetzt werden soll, ist der Gültigkeitsbereich nur dieser Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="b7992-179">If the expression is in a query clause that is specified to be translated into the body of a lambda, its scope is just that expression.</span></span>

## <a name="changes-to-syntactic-disambiguation"></a><span data-ttu-id="b7992-180">Änderungen an syntaktischer Eindeutigkeit</span><span class="sxs-lookup"><span data-stu-id="b7992-180">Changes to syntactic disambiguation</span></span>

<span data-ttu-id="b7992-181">Es gibt Situationen, in denen die C# Grammatik mehrdeutig ist, und die Sprachspezifikation gibt an, wie diese Mehrdeutigkeiten gelöst werden können:</span><span class="sxs-lookup"><span data-stu-id="b7992-181">There are situations involving generics where the C# grammar is ambiguous, and the language spec says how to resolve those ambiguities:</span></span>

> #### <a name="7652-grammar-ambiguities"></a><span data-ttu-id="b7992-182">7.6.5.2 Grammatik Mehrdeutigkeiten</span><span class="sxs-lookup"><span data-stu-id="b7992-182">7.6.5.2 Grammar ambiguities</span></span>
> <span data-ttu-id="b7992-183">Die Produktion für *einfachen Namen* ("7.6.3") und " *Member-Access* " (§ 7.6.5) kann zu Mehrdeutigkeiten in der Grammatik für Ausdrücke führen.</span><span class="sxs-lookup"><span data-stu-id="b7992-183">The productions for *simple-name* (§7.6.3) and *member-access* (§7.6.5) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="b7992-184">Beispielsweise ist die-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="b7992-184">For example, the statement:</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="b7992-185">kann als `F` mit zwei Argumenten interpretiert werden, `G < A` und `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="b7992-185">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="b7992-186">Alternativ könnte Sie auch als `F` mit einem Argument interpretiert werden, bei dem es sich um einen aufzurufenden generischen Methoden `G` mit zwei Typargumenten und einem regulären Argument handelt.</span><span class="sxs-lookup"><span data-stu-id="b7992-186">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

> <span data-ttu-id="b7992-187">Wenn eine Sequenz von Token (im Kontext) als *Simple-Name* (7.6.3), Element *-Access* (§ 7.6.5) oder *Pointer-Member-Access* (§ 18.5.2) analysiert werden kann, die mit einer *Type-Argument-List* (§ 4.4.1) endet, wird das Token, das unmittelbar auf das schließende `>` Token folgt, untersucht.</span><span class="sxs-lookup"><span data-stu-id="b7992-187">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="b7992-188">Wenn eine von</span><span class="sxs-lookup"><span data-stu-id="b7992-188">If it is one of</span></span>
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> <span data-ttu-id="b7992-189">Anschließend wird die *Type-Argument-List* als Teil des *Simple-namens*, des *Mitglieds Zugriffs* oder des *Zeigers für den Zeiger Zugriff* aufbewahrt, und jede andere mögliche Analyse der Sequenz von Token wird verworfen.</span><span class="sxs-lookup"><span data-stu-id="b7992-189">then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="b7992-190">Andernfalls wird die *Type-Argument-List* nicht als Teil des *Simple-namens*, des *Member-Access* -oder > *Pointer-Member-Access*betrachtet, auch wenn es keine andere Möglichkeit gibt, die Sequenz von Token zu analysieren.</span><span class="sxs-lookup"><span data-stu-id="b7992-190">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or > *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="b7992-191">Beachten Sie, dass diese Regeln beim Parsen einer *Type-Argument-List* in einem *Namespace-oder-Type-Name* (§ 3,8) nicht angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="b7992-191">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span> <span data-ttu-id="b7992-192">Die Anweisung</span><span class="sxs-lookup"><span data-stu-id="b7992-192">The statement</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="b7992-193">wird gemäß dieser Regel als ein `F` mit einem Argument interpretiert, bei dem es sich um einen aufzurufenden generischen Methoden `G` mit zwei Typargumenten und einem regulären Argument handelt.</span><span class="sxs-lookup"><span data-stu-id="b7992-193">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="b7992-194">Die Anweisungen</span><span class="sxs-lookup"><span data-stu-id="b7992-194">The statements</span></span>
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> <span data-ttu-id="b7992-195">wird als `F` mit zwei Argumenten interpretiert.</span><span class="sxs-lookup"><span data-stu-id="b7992-195">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="b7992-196">Die Anweisung</span><span class="sxs-lookup"><span data-stu-id="b7992-196">The statement</span></span>
> ```csharp
> x = F < A > +y;
> ```
> <span data-ttu-id="b7992-197">wird als ein kleiner-als-Operator, größer als-Operator und Unärer Plus-Operator interpretiert, als wäre die Anweisung `x = (F < A) > (+y)`geschrieben worden, anstatt als *einfacher Name* mit einer *Type-Argument-List* gefolgt von einem binären Plus-Operator.</span><span class="sxs-lookup"><span data-stu-id="b7992-197">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple-name* with a *type-argument-list* followed by a binary plus operator.</span></span> <span data-ttu-id="b7992-198">In der-Anweisung</span><span class="sxs-lookup"><span data-stu-id="b7992-198">In the statement</span></span>
> ```csharp
> x = y is C<T> + z;
> ```
> <span data-ttu-id="b7992-199">die Token `C<T>` werden als *Namespace-oder-Typname* mit einer *Type-Argument-List*interpretiert.</span><span class="sxs-lookup"><span data-stu-id="b7992-199">the tokens `C<T>` are interpreted as a *namespace-or-type-name* with a *type-argument-list*.</span></span>

<span data-ttu-id="b7992-200">Es gibt eine Reihe von Änderungen, die in C# 7 eingeführt werden, die diese mehrdeutigkeits Regeln nicht mehr ausreichen, um die Komplexität der Sprache zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="b7992-200">There are a number of changes being introduced in C# 7 that make these disambiguation rules no longer sufficient to handle the complexity of the language.</span></span>

### <a name="out-variable-declarations"></a><span data-ttu-id="b7992-201">out-Variablendeklaration</span><span class="sxs-lookup"><span data-stu-id="b7992-201">Out variable declarations</span></span>

<span data-ttu-id="b7992-202">Es ist nun möglich, eine Variable in einem out-Argument zu deklarieren:</span><span class="sxs-lookup"><span data-stu-id="b7992-202">It is now possible to declare a variable in an out argument:</span></span>

```csharp
M(out Type name);
```

<span data-ttu-id="b7992-203">Der Typ kann jedoch generisch sein:</span><span class="sxs-lookup"><span data-stu-id="b7992-203">However, the type may be generic:</span></span> 

```csharp
M(out A<B> name);
```

<span data-ttu-id="b7992-204">Da die Sprachgrammatik für das-Argument *Expression*verwendet, unterliegt dieser Kontext der disambiguations-Regel.</span><span class="sxs-lookup"><span data-stu-id="b7992-204">Since the language grammar for the argument uses *expression*, this context is subject to the disambiguation rule.</span></span> <span data-ttu-id="b7992-205">In diesem Fall folgt dem schließenden `>` ein *Bezeichner*, bei dem es sich nicht um eines der Token handelt, das es ermöglicht, als *Type-Argument-List*behandelt zu werden.</span><span class="sxs-lookup"><span data-stu-id="b7992-205">In this case the closing `>` is followed by an *identifier*, which is not one of the tokens that permits it to be treated as a *type-argument-list*.</span></span> <span data-ttu-id="b7992-206">Ich schlage daher vor, **einen *Bezeichner* zu dem Satz von Token hinzuzufügen, der die Eindeutigkeit einer *Type-Argument-List*auslöst.**</span><span class="sxs-lookup"><span data-stu-id="b7992-206">I therefore propose to **add *identifier* to the set of tokens that triggers the disambiguation to a *type-argument-list*.**</span></span>

### <a name="tuples-and-deconstruction-declarations"></a><span data-ttu-id="b7992-207">Tupel-und Debug-Deklarationen</span><span class="sxs-lookup"><span data-stu-id="b7992-207">Tuples and deconstruction declarations</span></span>

<span data-ttu-id="b7992-208">Ein tupelliteral wird genau das gleiche Problem haben.</span><span class="sxs-lookup"><span data-stu-id="b7992-208">A tuple literal runs into exactly the same issue.</span></span> <span data-ttu-id="b7992-209">Beachten Sie den Tupelausdruck.</span><span class="sxs-lookup"><span data-stu-id="b7992-209">Consider the tuple expression</span></span>

```csharp
(A < B, C > D, E < F, G > H)
```

<span data-ttu-id="b7992-210">Unter den alten C# 6 Regeln zum Analysieren einer Argumentliste würde dies als Tupel mit vier Elementen analysiert werden, beginnend mit `A < B` als ersten.</span><span class="sxs-lookup"><span data-stu-id="b7992-210">Under the old C# 6 rules for parsing an argument list, this would parse as a tuple with four elements, starting with `A < B` as the first.</span></span> <span data-ttu-id="b7992-211">Wenn dies jedoch auf der linken Seite einer Debug-Datei angezeigt wird, soll die Unterscheidung wie oben beschrieben durch das *Bezeichnertoken* ausgelöst werden:</span><span class="sxs-lookup"><span data-stu-id="b7992-211">However, when this appears on the left of a deconstruction, we want the disambiguation triggered by the *identifier* token as described above:</span></span>

```csharp
(A<B,C> D, E<F,G> H) = e;
```

<span data-ttu-id="b7992-212">Dies ist eine Dekonstruktions Deklaration, die zwei Variablen deklariert, wobei der erste vom Typ `A<B,C>` und der benannte `D`ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-212">This is a deconstruction declaration which declares two variables, the first of which is of type `A<B,C>` and named `D`.</span></span> <span data-ttu-id="b7992-213">Das heißt, das tupelliterale enthält zwei Ausdrücke, von denen jeder ein Deklarations Ausdruck ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-213">In other words, the tuple literal contains two expressions, each of which is a declaration expression.</span></span>

<span data-ttu-id="b7992-214">Aus Gründen der Einfachheit der Spezifikation und des Compilers schlage ich vor, dass dieses tupelliterals ein Tupel mit zwei Elementen analysiert wird, wo es angezeigt wird (unabhängig davon, ob es auf der linken Seite einer Zuweisung angezeigt wird).</span><span class="sxs-lookup"><span data-stu-id="b7992-214">For simplicity of the specification and compiler, I propose that this tuple literal be parsed as a two-element tuple wherever it appears (whether or not it appears on the left-hand-side of an assignment).</span></span> <span data-ttu-id="b7992-215">Dies wäre ein natürliches Ergebnis der Eindeutigkeit, die im vorherigen Abschnitt beschrieben wurde.</span><span class="sxs-lookup"><span data-stu-id="b7992-215">That would be a natural result of the disambiguation described in the previous section.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="b7992-216">Muster Vergleich</span><span class="sxs-lookup"><span data-stu-id="b7992-216">Pattern-matching</span></span>

<span data-ttu-id="b7992-217">Der Muster Vergleich führt einen neuen Kontext ein, bei dem die Ausdrucks typmehrdeutigkeit auftritt.</span><span class="sxs-lookup"><span data-stu-id="b7992-217">Pattern matching introduces a new context where the expression-type ambiguity arises.</span></span> <span data-ttu-id="b7992-218">Zuvor war die Rechte Seite eines `is` Operators ein Typ.</span><span class="sxs-lookup"><span data-stu-id="b7992-218">Previously the right-hand-side of an `is` operator was a type.</span></span> <span data-ttu-id="b7992-219">Nun kann es sich um einen Typ oder einen Ausdruck handeln, und wenn es sich um einen Typ handelt, kann ein Bezeichner folgen.</span><span class="sxs-lookup"><span data-stu-id="b7992-219">Now it can be a type or expression, and if it is a type it may be followed by an identifier.</span></span> <span data-ttu-id="b7992-220">Dies kann technisch gesehen die Bedeutung von vorhandenem Code ändern:</span><span class="sxs-lookup"><span data-stu-id="b7992-220">This can, technically, change the meaning of existing code:</span></span>

```csharp
var x = e is T < A > B;
```

<span data-ttu-id="b7992-221">Diese kann unter c# 6-Regeln analysiert werden als</span><span class="sxs-lookup"><span data-stu-id="b7992-221">This could be parsed under C#6 rules as</span></span>

```csharp
var x = ((e is T) < A) > B;
```

<span data-ttu-id="b7992-222">unter "c# 7"-Regeln (mit der oben vorgeschlagenen Mehrdeutigkeit) werden jedoch als</span><span class="sxs-lookup"><span data-stu-id="b7992-222">but under under C#7 rules (with the disambiguation proposed above) would be parsed as</span></span>

```csharp
var x = e is T<A> B;
```

<span data-ttu-id="b7992-223">deklariert eine Variable `B` vom Typ `T<A>`.</span><span class="sxs-lookup"><span data-stu-id="b7992-223">which declares a variable `B` of type `T<A>`.</span></span> <span data-ttu-id="b7992-224">Glücklicherweise haben die systemeigenen und Roslyn-Compiler einen Fehler, wenn Sie einen Syntax Fehler im c# 6-Code verursachen.</span><span class="sxs-lookup"><span data-stu-id="b7992-224">Fortunately, the native and Roslyn compilers have a bug whereby they give a syntax error on the C#6 code.</span></span> <span data-ttu-id="b7992-225">Daher ist diese spezielle Breaking Change kein Problem.</span><span class="sxs-lookup"><span data-stu-id="b7992-225">Therefore this particular breaking change is not a concern.</span></span>

<span data-ttu-id="b7992-226">Bei Pattern-Matching werden zusätzliche Token eingeführt, die die mehrdeutigkeitsauflösung bei der Auswahl eines Typs steuern.</span><span class="sxs-lookup"><span data-stu-id="b7992-226">Pattern-matching introduces additional tokens that should drive the ambiguity resolution toward selecting a type.</span></span> <span data-ttu-id="b7992-227">Die folgenden Beispiele für vorhandenen gültigen c# 6-Code werden ohne zusätzliche mehrdeutigkeits Regeln getrennt:</span><span class="sxs-lookup"><span data-stu-id="b7992-227">The following examples of existing valid C#6 code would be broken without additional disambiguation rules:</span></span>

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a><span data-ttu-id="b7992-228">Vorgeschlagene Änderung der Eindeutigkeits Regel</span><span class="sxs-lookup"><span data-stu-id="b7992-228">Proposed change to the disambiguation rule</span></span>

<span data-ttu-id="b7992-229">Ich schlage vor, die Spezifikation zu überarbeiten, um die Liste der mehrdeutigkeits Token zu ändern.</span><span class="sxs-lookup"><span data-stu-id="b7992-229">I propose to revise the specification to change the list of disambiguating tokens from</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

<span data-ttu-id="b7992-230">An</span><span class="sxs-lookup"><span data-stu-id="b7992-230">to</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

<span data-ttu-id="b7992-231">In bestimmten Kontexten behandeln wir den *Bezeichner* als Eindeutigkeits Token.</span><span class="sxs-lookup"><span data-stu-id="b7992-231">And, in certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="b7992-232">In diesen Kontexten wird der Sequenz von Token, die unterschieden werden, unmittelbar eines der Schlüsselwörter `is`, `case`oder `out`vorangestellt oder beim Parsen des ersten Elements eines tupelliterals vorangestellt (in diesem Fall werden den Token `(` oder `:` vorangestellt, und der Bezeichner folgt einem `,`) oder einem nachfolgenden Element eines tupelliterals.</span><span class="sxs-lookup"><span data-stu-id="b7992-232">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case`, or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>

### <a name="modified-disambiguation-rule"></a><span data-ttu-id="b7992-233">Geänderte disambiguations-Regel</span><span class="sxs-lookup"><span data-stu-id="b7992-233">Modified disambiguation rule</span></span>

<span data-ttu-id="b7992-234">Die überarbeitete Regel für die Mehrdeutigkeit würde etwa wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="b7992-234">The revised disambiguation rule would be something like this</span></span>

> <span data-ttu-id="b7992-235">Wenn eine Sequenz von Token (im Kontext) als *Simple-Name* (7.6.3), Element *-Access* (§ 7.6.5) oder *Pointer-Member-Access* (§ 18.5.2) analysiert werden kann, die mit einer *Type-Argument-List* (§ 4.4.1) endet, wird das Token, das unmittelbar auf das schließende `>` Token folgt, untersucht, um festzustellen, ob es</span><span class="sxs-lookup"><span data-stu-id="b7992-235">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined, to see if it is</span></span>
> - <span data-ttu-id="b7992-236">Eines der `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; noch</span><span class="sxs-lookup"><span data-stu-id="b7992-236">One of `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; or</span></span>
> - <span data-ttu-id="b7992-237">Einer der relationalen Operatoren `<  >  <=  >=  is as`; noch</span><span class="sxs-lookup"><span data-stu-id="b7992-237">One of the relational operators `<  >  <=  >=  is as`; or</span></span>
> - <span data-ttu-id="b7992-238">Ein Kontext abhängiges Abfrage Schlüsselwort in einem Abfrage Ausdruck. noch</span><span class="sxs-lookup"><span data-stu-id="b7992-238">A contextual query keyword appearing inside a query expression; or</span></span>
> - <span data-ttu-id="b7992-239">In bestimmten Kontexten behandeln wir den *Bezeichner* als Eindeutigkeits Token.</span><span class="sxs-lookup"><span data-stu-id="b7992-239">In certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="b7992-240">In diesen Kontexten wird der Sequenz von Token, die unterschieden werden, unmittelbar eines der Schlüsselwörter `is`, `case` oder `out`, oder beim Parsen des ersten Elements eines tupelliterals vorangestellt. (in diesem Fall werden den Token `(` oder `:` vorangestellt, und der Bezeichner folgt einem `,`) oder einem nachfolgenden Element eines tupelliterals.</span><span class="sxs-lookup"><span data-stu-id="b7992-240">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case` or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>
> 
> <span data-ttu-id="b7992-241">Wenn das folgende Token in dieser Liste enthalten ist, oder ein Bezeichner in einem solchen Kontext, wird die *Type-Argument-List* als Teil des *Simple-namens*, des Element *Zugriffs* oder des *Zeigers des Zeiger* Members beibehalten, und jede andere mögliche Analyse der Sequenz von Token wird verworfen.</span><span class="sxs-lookup"><span data-stu-id="b7992-241">If the following token is among this list, or an identifier in such a context, then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or  *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span>  <span data-ttu-id="b7992-242">Andernfalls wird die *Type-Argument-List* nicht als Bestandteil des *Simple-namens*, des Element *Zugriffs* oder des *Zeigers für Zeiger*Elemente angesehen, auch wenn es keine andere Möglichkeit gibt, die Sequenz von Token zu analysieren.</span><span class="sxs-lookup"><span data-stu-id="b7992-242">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="b7992-243">Beachten Sie, dass diese Regeln beim Parsen einer *Type-Argument-List* in einem *Namespace-oder-Type-Name* (§ 3,8) nicht angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="b7992-243">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span>

### <a name="breaking-changes-due-to-this-proposal"></a><span data-ttu-id="b7992-244">Wichtige Änderungen aufgrund dieses Angebots</span><span class="sxs-lookup"><span data-stu-id="b7992-244">Breaking changes due to this proposal</span></span>

<span data-ttu-id="b7992-245">Aufgrund dieser vorgeschlagenen mehrdeutigkeits Regel sind keine wichtigen Änderungen bekannt.</span><span class="sxs-lookup"><span data-stu-id="b7992-245">No breaking changes are known due to this proposed disambiguation rule.</span></span>

### <a name="interesting-examples"></a><span data-ttu-id="b7992-246">Interessante Beispiele</span><span class="sxs-lookup"><span data-stu-id="b7992-246">Interesting examples</span></span>

<span data-ttu-id="b7992-247">Im folgenden finden Sie einige interessante Ergebnisse dieser mehrdeutigkeits Regeln:</span><span class="sxs-lookup"><span data-stu-id="b7992-247">Here are some interesting results of these disambiguation rules:</span></span>

<span data-ttu-id="b7992-248">Der Ausdruck `(A < B, C > D)` ist ein Tupel mit zwei Elementen, jeweils ein Vergleich.</span><span class="sxs-lookup"><span data-stu-id="b7992-248">The expression `(A < B, C > D)` is a tuple with two elements, each a comparison.</span></span>

<span data-ttu-id="b7992-249">Der Ausdruck `(A<B,C> D, E)` ist ein Tupel mit zwei Elementen, wobei der erste ein Deklarations Ausdruck ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-249">The expression `(A<B,C> D, E)` is a tuple with two elements, the first of which is a declaration expression.</span></span>

<span data-ttu-id="b7992-250">Der Aufruf `M(A < B, C > D, E)` hat drei Argumente.</span><span class="sxs-lookup"><span data-stu-id="b7992-250">The invocation `M(A < B, C > D, E)` has three arguments.</span></span>

<span data-ttu-id="b7992-251">Der Aufruf `M(out A<B,C> D, E)` hat zwei Argumente, wobei der erste eine `out` Deklaration ist.</span><span class="sxs-lookup"><span data-stu-id="b7992-251">The invocation `M(out A<B,C> D, E)` has two arguments, the first of which is an `out` declaration.</span></span>

<span data-ttu-id="b7992-252">Der Ausdrucks `e is A<B> C` verwendet einen Deklarations Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="b7992-252">The expression `e is A<B> C` uses a declaration expression.</span></span>

<span data-ttu-id="b7992-253">Die Case-Bezeichnung `case A<B> C:` verwendet einen Deklarations Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="b7992-253">The case label `case A<B> C:` uses a declaration expression.</span></span>

## <a name="some-examples-of-pattern-matching"></a><span data-ttu-id="b7992-254">Einige Beispiele für den Musterabgleich</span><span class="sxs-lookup"><span data-stu-id="b7992-254">Some examples of pattern matching</span></span>

### <a name="is-as"></a><span data-ttu-id="b7992-255">Ist-As</span><span class="sxs-lookup"><span data-stu-id="b7992-255">Is-As</span></span>

<span data-ttu-id="b7992-256">Wir können die Ausdrucksweise ersetzen.</span><span class="sxs-lookup"><span data-stu-id="b7992-256">We can replace the idiom</span></span>

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

<span data-ttu-id="b7992-257">Mit etwas präziseren und direkteren</span><span class="sxs-lookup"><span data-stu-id="b7992-257">With the slightly more concise and direct</span></span>

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a><span data-ttu-id="b7992-258">Testen von NULL-Werten</span><span class="sxs-lookup"><span data-stu-id="b7992-258">Testing nullable</span></span>

<span data-ttu-id="b7992-259">Wir können die Ausdrucksweise ersetzen.</span><span class="sxs-lookup"><span data-stu-id="b7992-259">We can replace the idiom</span></span>

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

<span data-ttu-id="b7992-260">Mit etwas präziseren und direkteren</span><span class="sxs-lookup"><span data-stu-id="b7992-260">With the slightly more concise and direct</span></span>

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a><span data-ttu-id="b7992-261">Arithmetische Vereinfachung</span><span class="sxs-lookup"><span data-stu-id="b7992-261">Arithmetic simplification</span></span>

<span data-ttu-id="b7992-262">Angenommen, wir definieren eine Reihe von rekursiven Typen zur Darstellung von Ausdrücken (gemäß einem separaten Vorschlag):</span><span class="sxs-lookup"><span data-stu-id="b7992-262">Suppose we define a set of recursive types to represent expressions (per a separate proposal):</span></span>

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

<span data-ttu-id="b7992-263">Nun können wir eine Funktion definieren, um die (nicht reduzierte) Ableitung eines Ausdrucks zu berechnen:</span><span class="sxs-lookup"><span data-stu-id="b7992-263">Now we can define a function to compute the (unreduced) derivative of an expression:</span></span>

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

<span data-ttu-id="b7992-264">Ein Ausdrucks vereinfachende zeigt Positions Muster:</span><span class="sxs-lookup"><span data-stu-id="b7992-264">An expression simplifier demonstrates positional patterns:</span></span>

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
