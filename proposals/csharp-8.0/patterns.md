---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483979"
---
# <a name="recursive-pattern-matching"></a><span data-ttu-id="2a465-101">Rekursiver Musterabgleich</span><span class="sxs-lookup"><span data-stu-id="2a465-101">Recursive Pattern Matching</span></span>

## <a name="summary"></a><span data-ttu-id="2a465-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="2a465-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="2a465-103">Muster Vergleichs Erweiterungen für C# ermöglichen viele der Vorteile von algebraischen Datentypen und Musterabgleich von funktionalen Sprachen, aber auf eine Weise, die nahtlos in das Verhalten der zugrunde liegenden Sprache integriert ist.</span><span class="sxs-lookup"><span data-stu-id="2a465-103">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="2a465-104">Elemente dieses Ansatzes sind von verwandten Features in den Programmiersprachen [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Erweiterbarer Musterabgleich über eine vereinfachte Sprache") und [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Abgleichen von Objekten mit Mustern, Seite 273")inspiriert.</span><span class="sxs-lookup"><span data-stu-id="2a465-104">Elements of this approach are inspired by related features in the programming languages [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Matching Objects With Patterns, page 273").</span></span>

## <a name="detailed-design"></a><span data-ttu-id="2a465-105">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="2a465-105">Detailed design</span></span>
[design]: #detailed-design

### <a name="is-expression"></a><span data-ttu-id="2a465-106">Is-Ausdruck</span><span class="sxs-lookup"><span data-stu-id="2a465-106">Is Expression</span></span>

<span data-ttu-id="2a465-107">Der `is`-Operator wird erweitert, um einen Ausdruck mit einem *Muster*zu testen.</span><span class="sxs-lookup"><span data-stu-id="2a465-107">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="2a465-108">Diese Form von *relational_expression* wird zusätzlich zu den in der C# Spezifikation vorhandenen Formularen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2a465-108">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="2a465-109">Es handelt sich um einen Kompilierzeitfehler, wenn der *relational_expression* auf der linken Seite des `is`-Tokens keinen Wert oder keinen Typ festgelegt hat.</span><span class="sxs-lookup"><span data-stu-id="2a465-109">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="2a465-110">Jeder *Bezeichner* des Musters führt eine neue lokale Variable ein, die *definitiv zugewiesen* wird, nachdem der `is`-Operator `true` wurde (d.h. *definitiv zugewiesen, wenn true*).</span><span class="sxs-lookup"><span data-stu-id="2a465-110">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="2a465-111">Hinweis: Es gibt technisch gesehen eine Mehrdeutigkeit zwischen dem *Typ* in einer `is-expression` und *constant_pattern*, von denen jede eine gültige Analyse eines qualifizierten Bezeichners sein könnte.</span><span class="sxs-lookup"><span data-stu-id="2a465-111">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="2a465-112">Wir versuchen, Sie als Typ für die Kompatibilität mit früheren Versionen der Sprache zu binden. nur wenn dies nicht möglich ist, lösen wir es wie einen Ausdruck in anderen Kontexten auf, um den ersten gefundenen Vorgang (bei dem es sich um eine Konstante oder einen Typ handeln muss) durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="2a465-112">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do an expression in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="2a465-113">Diese Mehrdeutigkeit ist nur auf der rechten Seite eines `is` Ausdrucks vorhanden.</span><span class="sxs-lookup"><span data-stu-id="2a465-113">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="patterns"></a><span data-ttu-id="2a465-114">Muster</span><span class="sxs-lookup"><span data-stu-id="2a465-114">Patterns</span></span>

<span data-ttu-id="2a465-115">Muster werden im *is_pattern* Operator, in einem *switch_statement*und in einem *switch_expression* verwendet, um die Form der Daten auszudrücken, mit denen eingehende Daten (die als Eingabe Wert bezeichnet werden) verglichen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="2a465-115">Patterns are used in the *is_pattern* operator, in a *switch_statement*, and in a *switch_expression* to express the shape of data against which incoming data  (which we call the input value) is to be compared.</span></span> <span data-ttu-id="2a465-116">Muster können rekursiv sein, damit Teile der Daten mit unter Mustern verglichen werden können.</span><span class="sxs-lookup"><span data-stu-id="2a465-116">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a><span data-ttu-id="2a465-117">Deklarations Muster</span><span class="sxs-lookup"><span data-stu-id="2a465-117">Declaration Pattern</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="2a465-118">Der *declaration_pattern* testet, ob ein Ausdruck einen bestimmten Typ hat, und wandelt ihn in diesen Typ um, wenn der Test erfolgreich ist.</span><span class="sxs-lookup"><span data-stu-id="2a465-118">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="2a465-119">Dadurch kann eine lokale Variable des angegebenen Typs, benannt durch den angegebenen Bezeichner, eingeführt werden, wenn die Bezeichnung eine *single_variable_designation*ist.</span><span class="sxs-lookup"><span data-stu-id="2a465-119">This may introduce a local variable of the given type named by the given identifier, if the designation is a *single_variable_designation*.</span></span> <span data-ttu-id="2a465-120">Diese lokale Variable ist *definitiv zugewiesen* , wenn das Ergebnis der Muster Vergleichsoperation `true`wird.</span><span class="sxs-lookup"><span data-stu-id="2a465-120">That local variable is *definitely assigned* when the result of the pattern-matching operation is `true`.</span></span>

<span data-ttu-id="2a465-121">Die Lauf Zeit Semantik dieses Ausdrucks besteht darin, dass er den Lauf Zeittyp des linken *relational_expression* Operanden mit dem *Typ* im Muster testet.</span><span class="sxs-lookup"><span data-stu-id="2a465-121">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span>  <span data-ttu-id="2a465-122">Wenn es sich um einen Lauf Zeittyp (oder einen Untertyp) handelt und nicht `null`, ist das Ergebnis der `is operator` `true`.</span><span class="sxs-lookup"><span data-stu-id="2a465-122">If it is of that runtime type (or some subtype) and not `null`, the result of the `is operator` is `true`.</span></span>

<span data-ttu-id="2a465-123">Bestimmte Kombinationen von statischem Typ der linken Seite und des angegebenen Typs werden als nicht kompatibel betrachtet und führen zu einem Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="2a465-123">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="2a465-124">Ein Wert vom Typ "static Type `E`" ist mit einem Typ " *Muster kompatibel* " `T`, wenn eine Identitäts Konvertierung, eine implizite Verweis Konvertierung, eine Boxing-Konvertierung, eine explizite Verweis Konvertierung oder eine Unboxing-Konvertierung von `E` in `T`vorhanden ist, oder wenn einer dieser Typen ein offener Typ ist.</span><span class="sxs-lookup"><span data-stu-id="2a465-124">A value of static type `E` is said to be *pattern-compatible* with a type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, or if one of those types is an open type.</span></span> <span data-ttu-id="2a465-125">Es handelt sich um einen Kompilierzeitfehler, wenn eine Eingabe vom Typ `E` nicht mit dem *Typ* in einem Typmuster *kompatibel* ist, mit dem Sie übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="2a465-125">It is a compile-time error if an input of type `E` is not *pattern-compatible* with the *type* in a type pattern that it is matched with.</span></span>

<span data-ttu-id="2a465-126">Das Typmuster ist nützlich zum Ausführen von Lauf Zeittyp Tests von Verweis Typen und ersetzt die Ausdrucksweise.</span><span class="sxs-lookup"><span data-stu-id="2a465-126">The type pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

<span data-ttu-id="2a465-127">Mit etwas präziseren</span><span class="sxs-lookup"><span data-stu-id="2a465-127">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v
```

<span data-ttu-id="2a465-128">Es ist ein Fehler, wenn der *Typ* ein Werte zulässt-Werttyp ist.</span><span class="sxs-lookup"><span data-stu-id="2a465-128">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="2a465-129">Das Typmuster kann verwendet werden, um Werte von Typen zu testen, die NULL-Werte zulassen: ein Wert vom Typ `Nullable<T>` (oder ein gebocktes `T`) entspricht einem Typmuster `T2 id` wenn der Wert nicht NULL ist und der Typ der `T2` `T`oder ein Basistyp oder eine Schnittstelle `T`ist.</span><span class="sxs-lookup"><span data-stu-id="2a465-129">The type pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="2a465-130">Beispielsweise im Code Fragment</span><span class="sxs-lookup"><span data-stu-id="2a465-130">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v
```

<span data-ttu-id="2a465-131">Die Bedingung der `if`-Anweisung wird zur Laufzeit `true`, und die Variable `v` enthält den Wert `3` vom Typ `int` innerhalb des Blocks.</span><span class="sxs-lookup"><span data-stu-id="2a465-131">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span> <span data-ttu-id="2a465-132">Nach dem Block ist die Variable `v` im Gültigkeitsbereich, aber nicht definitiv zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="2a465-132">After the block the variable `v` is in scope but not definitely assigned.</span></span>

#### <a name="constant-pattern"></a><span data-ttu-id="2a465-133">Konstantes Muster</span><span class="sxs-lookup"><span data-stu-id="2a465-133">Constant Pattern</span></span>

```antlr
constant_pattern
    : constant_expression
    ;
```

<span data-ttu-id="2a465-134">Ein konstantes Muster testet den Wert eines Ausdrucks mit einem konstanten Wert.</span><span class="sxs-lookup"><span data-stu-id="2a465-134">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="2a465-135">Die Konstante kann ein beliebiger konstanter Ausdruck sein, z. b. ein Literalwert, der Name einer deklarierten `const` Variablen oder eine Enumerationskonstante.</span><span class="sxs-lookup"><span data-stu-id="2a465-135">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant.</span></span> <span data-ttu-id="2a465-136">Wenn der Eingabe Wert kein offener Typ ist, wird der Konstante Ausdruck implizit in den Typ des übereinstimmenden Ausdrucks konvertiert. Wenn der Typ des Eingabe Werts nicht mit dem Typ des konstanten Ausdrucks *Muster kompatibel* ist, ist der Muster Vergleichs Vorgang ein Fehler.</span><span class="sxs-lookup"><span data-stu-id="2a465-136">When the input value is not an open type, the constant expression is implicitly converted to the type of the matched expression; if the type of the input value is not *pattern-compatible* with the type of the constant expression, the pattern-matching operation is an error.</span></span>

<span data-ttu-id="2a465-137">Das Muster *c* wird als Übereinstimmung mit dem konvertierten Eingabe Wert *e* betrachtet, wenn `object.Equals(c, e)` `true`zurückgeben würde.</span><span class="sxs-lookup"><span data-stu-id="2a465-137">The pattern *c* is considered matching the converted input value *e* if `object.Equals(c, e)` would return `true`.</span></span>

<span data-ttu-id="2a465-138">Es ist zu erwarten, dass `e is null` als gängigste Methode zum Testen auf `null` in neu geschriebenem Code angezeigt wird, da keine benutzerdefinierte `operator==`aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="2a465-138">We expect to see `e is null` as the most common way to test for `null` in newly written code, as it cannot invoke a user-defined `operator==`.</span></span>

#### <a name="var-pattern"></a><span data-ttu-id="2a465-139">Var-Muster</span><span class="sxs-lookup"><span data-stu-id="2a465-139">Var Pattern</span></span>

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

<span data-ttu-id="2a465-140">Wenn die *Bezeichnung* eine *simple_designation*ist, entspricht der Ausdruck *e* dem Muster.</span><span class="sxs-lookup"><span data-stu-id="2a465-140">If the *designation* is a *simple_designation*, an expression *e* matches the pattern.</span></span> <span data-ttu-id="2a465-141">Anders ausgedrückt: eine Entsprechung zu einem *var-Muster* ist immer mit einem *simple_designation*erfolgreich.</span><span class="sxs-lookup"><span data-stu-id="2a465-141">In other words, a match to a *var pattern* always succeeds with a *simple_designation*.</span></span> <span data-ttu-id="2a465-142">Wenn die *simple_designation* eine *single_variable_designation*ist, liegt der Wert von *e* an einer neu eingeführten lokalen Variablen.</span><span class="sxs-lookup"><span data-stu-id="2a465-142">If the *simple_designation* is a *single_variable_designation*, the value of *e* is bounds to a newly introduced local variable.</span></span> <span data-ttu-id="2a465-143">Der Typ der lokalen Variablen ist der statische Typ von *e*.</span><span class="sxs-lookup"><span data-stu-id="2a465-143">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="2a465-144">Wenn es sich bei der *Bezeichnung* um einen *tuple_designation*handelt, entspricht das Muster einem *positional_pattern* der Form `(var` *Bezeichnung*,... `)`, bei der die *Bezeichnungen*im *tuple_designation*gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="2a465-144">If the *designation* is a *tuple_designation*, then the pattern is equivalent to a *positional_pattern* of the form `(var` *designation*, ... `)` where the *designation*s are those found within the *tuple_designation*.</span></span>  <span data-ttu-id="2a465-145">Beispielsweise entspricht das Muster `var (x, (y, z))` `(var x, (var y, var z))`.</span><span class="sxs-lookup"><span data-stu-id="2a465-145">For example, the pattern `var (x, (y, z))` is equivalent to `(var x, (var y, var z))`.</span></span>

<span data-ttu-id="2a465-146">Wenn der Name `var` an einen Typ bindet, ist ein Fehler aufgetreten.</span><span class="sxs-lookup"><span data-stu-id="2a465-146">It is an error if the name `var` binds to a type.</span></span>

#### <a name="discard-pattern"></a><span data-ttu-id="2a465-147">Muster verwerfen</span><span class="sxs-lookup"><span data-stu-id="2a465-147">Discard Pattern</span></span>

```antlr
discard_pattern
    : '_'
    ;
```

<span data-ttu-id="2a465-148">Ein Ausdruck *e* stimmt mit dem Muster `_` immer überein.</span><span class="sxs-lookup"><span data-stu-id="2a465-148">An expression *e* matches the pattern `_` always.</span></span> <span data-ttu-id="2a465-149">Dies bedeutet, dass jeder Ausdruck mit dem Verwerfungs Muster übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="2a465-149">In other words, every expression matches the discard pattern.</span></span>

<span data-ttu-id="2a465-150">Ein Verwerfungs Muster kann nicht als Muster eines *is_pattern_expression*verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2a465-150">A discard pattern may not be used as the pattern of an *is_pattern_expression*.</span></span>

#### <a name="positional-pattern"></a><span data-ttu-id="2a465-151">Positions Muster</span><span class="sxs-lookup"><span data-stu-id="2a465-151">Positional Pattern</span></span>

<span data-ttu-id="2a465-152">Ein Positions Muster prüft, ob der Eingabe Wert nicht `null`ist, ruft eine entsprechende `Deconstruct` Methode auf und führt einen weiteren Musterabgleich für die resultierenden Werte aus.</span><span class="sxs-lookup"><span data-stu-id="2a465-152">A positional pattern checks that the input value is not `null`, invokes an appropriate `Deconstruct` method, and performs further pattern matching on the resulting values.</span></span>  <span data-ttu-id="2a465-153">Sie unterstützt auch eine tupelähnliche Muster Syntax (ohne den Typ), wenn der Typ des Eingabe Werts mit dem Typ übereinstimmt, der `Deconstruct`enthält, oder wenn der Typ des Eingabe Werts ein tupeltyp ist, oder wenn der Typ des Eingabe Werts `object` oder `ITuple` ist und der Lauf Zeittyp des Ausdrucks `ITuple`implementiert.</span><span class="sxs-lookup"><span data-stu-id="2a465-153">It also supports a tuple-like pattern syntax (without the type being provided) when the type of the input value is the same as the type containing `Deconstruct`, or if the type of the input value is a tuple type, or if the type of the input value is `object` or `ITuple` and the runtime type of the expression implements `ITuple`.</span></span>

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

<span data-ttu-id="2a465-154">Wenn der *Typ* weggelassen wird, wird er als statischer Typ des Eingabe Werts übernommen.</span><span class="sxs-lookup"><span data-stu-id="2a465-154">If the *type* is omitted, we take it to be the static type of the input value.</span></span>

<span data-ttu-id="2a465-155">Wenn eine Übereinstimmung eines Eingabe Werts mit dem *Mustertyp* `(` *subpattern_list* `)`vorliegt, wird eine Methode ausgewählt, indem der *Typ* nach zugänglichen Deklarationen von `Deconstruct` durchsucht und eine Methode ausgewählt wird, die dieselben Regeln wie für die Dekonstruktions Deklaration verwendet.</span><span class="sxs-lookup"><span data-stu-id="2a465-155">Given a match of an input value to the pattern *type* `(` *subpattern_list* `)`, a method is selected by searching in *type* for accessible declarations of `Deconstruct` and selecting one among them using the same rules as for the deconstruction declaration.</span></span>

<span data-ttu-id="2a465-156">Es ist ein Fehler, wenn *ein positional_pattern* den Typ auslässt, über ein *einzelnes unter Muster* ohne einen *Bezeichner*verfügt, keine *property_subpattern* hat und keine *simple_designation*hat.</span><span class="sxs-lookup"><span data-stu-id="2a465-156">It is an error if a *positional_pattern* omits the type, has a single *subpattern* without an *identifier*, has no *property_subpattern* and has no *simple_designation*.</span></span> <span data-ttu-id="2a465-157">Dies unterscheidet sich zwischen einer *constant_pattern* , die in Klammern gesetzt ist, und einem *positional_pattern*.</span><span class="sxs-lookup"><span data-stu-id="2a465-157">This disambiguates between a *constant_pattern* that is parenthesized and a *positional_pattern*.</span></span>

<span data-ttu-id="2a465-158">Um die Werte zu extrahieren, die mit den Mustern in der Liste abgeglichen werden sollen,</span><span class="sxs-lookup"><span data-stu-id="2a465-158">In order to extract the values to match against the patterns in the list,</span></span>
- <span data-ttu-id="2a465-159">Wenn *Type* ausgelassen wurde und der Typ des Eingabe Werts ein tupeltyp ist, muss die Anzahl der Teil Muster der Kardinalität des Tupels entsprechen.</span><span class="sxs-lookup"><span data-stu-id="2a465-159">If *type* was omitted and the input value's type is a tuple type, then the number of subpatterns is required to be the same as the cardinality of the tuple.</span></span> <span data-ttu-id="2a465-160">Jedes Tupelelement wird mit dem entsprechenden *Teil Muster*verglichen, und die Übereinstimmung ist erfolgreich, wenn alle diese erfolgreich sind.</span><span class="sxs-lookup"><span data-stu-id="2a465-160">Each tuple element is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="2a465-161">Wenn ein *Teil Muster* über einen *Bezeichner*verfügt, muss dieser ein Tupelelement an der entsprechenden Position im tupeltyp benennen.</span><span class="sxs-lookup"><span data-stu-id="2a465-161">If any *subpattern* has an *identifier*, then that must name a tuple element at the corresponding position in the tuple type.</span></span>
- <span data-ttu-id="2a465-162">Wenn eine geeignete `Deconstruct` als Member des *Typs*vorhanden ist, handelt es sich um einen Kompilierzeitfehler, wenn der Typ des Eingabe Werts nicht mit dem *Typ*" *Muster kompatibel* " ist.</span><span class="sxs-lookup"><span data-stu-id="2a465-162">Otherwise, if a suitable `Deconstruct` exists as a member of *type*, it is a compile-time error if the type of the input value is not *pattern-compatible* with *type*.</span></span> <span data-ttu-id="2a465-163">Zur Laufzeit wird der Eingabe Wert anhand des *Typs*getestet.</span><span class="sxs-lookup"><span data-stu-id="2a465-163">At runtime the input value is tested against *type*.</span></span> <span data-ttu-id="2a465-164">Wenn dies fehlschlägt, schlägt die Zuordnung des Positions Musters fehl.</span><span class="sxs-lookup"><span data-stu-id="2a465-164">If this fails then the positional pattern match fails.</span></span> <span data-ttu-id="2a465-165">Wenn dies erfolgreich ist, wird der Eingabe Wert in diesen Typ konvertiert, und `Deconstruct` wird mit neuen vom Compiler generierten Variablen aufgerufen, um die `out` Parameter zu empfangen.</span><span class="sxs-lookup"><span data-stu-id="2a465-165">If it succeeds,  the input value is converted to this type and `Deconstruct` is invoked with fresh compiler-generated variables to receive the `out` parameters.</span></span> <span data-ttu-id="2a465-166">Jeder empfangene Wert wird mit dem entsprechenden *Teil Muster*verglichen, und die Übereinstimmung ist erfolgreich, wenn alle diese erfolgreich sind.</span><span class="sxs-lookup"><span data-stu-id="2a465-166">Each value that was received is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="2a465-167">Wenn ein *Teil Muster* über einen *Bezeichner*verfügt, muss dieser einen Parameter an der entsprechenden Position `Deconstruct`benennen.</span><span class="sxs-lookup"><span data-stu-id="2a465-167">If any *subpattern* has an *identifier*, then that must name a parameter at the corresponding position of `Deconstruct`.</span></span>
- <span data-ttu-id="2a465-168">Andernfalls, wenn der *Typ* weggelassen wurde und der Eingabe Wert vom Typ `object` oder `ITuple` oder ein Typ ist, der durch eine implizite Verweis Konvertierung in `ITuple` konvertiert werden kann, und in den Teil Mustern kein *Bezeichner* angezeigt wird, stimmen wir mithilfe `ITuple`ab.</span><span class="sxs-lookup"><span data-stu-id="2a465-168">Otherwise if *type* was omitted, and the input value is of type `object` or `ITuple` or some type that can be converted to `ITuple` by an implicit reference conversion, and no *identifier* appears among the subpatterns, then we match using `ITuple`.</span></span>
- <span data-ttu-id="2a465-169">Andernfalls ist das Muster ein Kompilierzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="2a465-169">Otherwise the pattern is a compile-time error.</span></span>

<span data-ttu-id="2a465-170">Die Reihenfolge, in der Teil Muster zur Laufzeit abgeglichen werden, ist nicht angegeben, und eine fehlgeschlagene Übereinstimmung versucht möglicherweise nicht, alle Teil Muster abzugleichen.</span><span class="sxs-lookup"><span data-stu-id="2a465-170">The order in which subpatterns are matched at runtime is unspecified, and a failed match may not attempt to match all subpatterns.</span></span>

##### <a name="example"></a><span data-ttu-id="2a465-171">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2a465-171">Example</span></span>

<span data-ttu-id="2a465-172">In diesem Beispiel werden viele Funktionen verwendet, die in dieser Spezifikation beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="2a465-172">This example uses many of the features described in this specification</span></span>

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a><span data-ttu-id="2a465-173">Eigenschafts Muster</span><span class="sxs-lookup"><span data-stu-id="2a465-173">Property Pattern</span></span>

<span data-ttu-id="2a465-174">Ein Eigenschafts Muster prüft, ob der Eingabe Wert nicht `null` ist, und vergleicht rekursiv Werte, die durch die Verwendung von zugänglichen Eigenschaften oder Feldern extrahiert werden.</span><span class="sxs-lookup"><span data-stu-id="2a465-174">A property pattern checks that the input value is not `null` and recursively matches values extracted by the use of accessible properties or fields.</span></span>

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

<span data-ttu-id="2a465-175">Es tritt ein Fehler auf, wenn ein _Teil Muster_ einer _property_pattern_ keinen _Bezeichner_ enthält (es muss sich um das zweite Formular handeln, das einen _Bezeichner_aufweist).</span><span class="sxs-lookup"><span data-stu-id="2a465-175">It is an error if any _subpattern_ of a _property_pattern_ does not contain an _identifier_ (it must be of the second form, which has an _identifier_).</span></span>  <span data-ttu-id="2a465-176">Ein nach gestelltes Komma nach dem letzten Teil Muster ist optional.</span><span class="sxs-lookup"><span data-stu-id="2a465-176">A trailing comma after the last subpattern is optional.</span></span>

<span data-ttu-id="2a465-177">Beachten Sie, dass ein Muster mit NULL-Überprüfung von einem trivialen Eigenschafts Muster abfällt.</span><span class="sxs-lookup"><span data-stu-id="2a465-177">Note that a null-checking pattern falls out of a trivial property pattern.</span></span> <span data-ttu-id="2a465-178">Um zu überprüfen, ob die Zeichenfolge `s` nicht NULL ist, können Sie jedes der folgenden Formulare schreiben.</span><span class="sxs-lookup"><span data-stu-id="2a465-178">To check if the string `s` is non-null, you can write any of the following forms</span></span>

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

<span data-ttu-id="2a465-179">Wenn eine Entsprechung eines Ausdrucks *e* zum *Mustertyp* `{` *property_pattern_list* `}`vorliegt, handelt es sich um einen Kompilierzeitfehler, wenn der Ausdruck *e* mit dem Typ *t* , der vom *Typ*festgelegt ist, nicht mit dem *Muster kompatibel* ist.</span><span class="sxs-lookup"><span data-stu-id="2a465-179">Given a match of an expression *e* to the pattern *type* `{` *property_pattern_list* `}`, it is a compile-time error if the expression *e* is not *pattern-compatible* with the type *T* designated by *type*.</span></span> <span data-ttu-id="2a465-180">Wenn der Typ nicht vorhanden ist, wird er als statischer Typ von *e*übernommen.</span><span class="sxs-lookup"><span data-stu-id="2a465-180">If the type is absent, we take it to be the static type of *e*.</span></span> <span data-ttu-id="2a465-181">Wenn der *Bezeichner* vorhanden ist, wird eine Muster Variable vom Typ *Type deklariert.*</span><span class="sxs-lookup"><span data-stu-id="2a465-181">If the *identifier* is present, it declares a pattern variable of type *type*.</span></span> <span data-ttu-id="2a465-182">Jeder Bezeichner, der auf der linken Seite des *property_pattern_list* angezeigt wird, muss eine barrierefreie lesbare Eigenschaft oder ein Feld von *T*angeben. Wenn die *simple_designation* des *property_pattern* vorhanden ist, wird eine Muster Variable vom Typ *T*definiert.</span><span class="sxs-lookup"><span data-stu-id="2a465-182">Each of the identifiers appearing on the left-hand-side of its *property_pattern_list* must designate an accessible readable property or field of *T*. If the *simple_designation* of the *property_pattern* is present, it defines a pattern variable of type *T*.</span></span>

<span data-ttu-id="2a465-183">Zur Laufzeit wird der Ausdruck für *T*getestet. Wenn dies fehlschlägt, schlägt das Eigenschafts Musterabgleich fehl, und das Ergebnis wird `false`.</span><span class="sxs-lookup"><span data-stu-id="2a465-183">At runtime, the expression is tested against *T*. If this fails then the property pattern match fails and the result is `false`.</span></span> <span data-ttu-id="2a465-184">Wenn der Vorgang erfolgreich ist, werden die einzelnen *property_subpattern* Felder oder-Eigenschaften gelesen und deren Wert mit dem entsprechenden Muster übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2a465-184">If it succeeds, then each *property_subpattern* field or property is read and its value matched against its corresponding pattern.</span></span> <span data-ttu-id="2a465-185">Das Ergebnis der ganzen Übereinstimmung ist nur `false`, wenn das Ergebnis eines dieser `false`ist.</span><span class="sxs-lookup"><span data-stu-id="2a465-185">The result of the whole match is `false` only if the result of any of these is `false`.</span></span> <span data-ttu-id="2a465-186">Die Reihenfolge, in der Teil Muster abgeglichen werden, ist nicht angegeben, und eine fehlgeschlagene Übereinstimmung entspricht möglicherweise nicht allen Teil Mustern zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="2a465-186">The order in which subpatterns are matched is not specified, and a failed match may not match all subpatterns at runtime.</span></span> <span data-ttu-id="2a465-187">Wenn die Übereinstimmung erfolgreich ist und die *simple_designation* der *property_pattern* ein *single_variable_designation*ist, wird eine Variable vom Typ *T* definiert, der der übereinstimmende Wert zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="2a465-187">If the match succeeds and the *simple_designation* of the *property_pattern* is a *single_variable_designation*, it defines a variable of type *T* that is assigned the matched value.</span></span>

> <span data-ttu-id="2a465-188">Hinweis: das Eigenschafts Muster kann verwendet werden, um eine Muster Übereinstimmung mit anonymen Typen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="2a465-188">Note: The property pattern can be used to pattern-match with anonymous types.</span></span>

##### <a name="example"></a><span data-ttu-id="2a465-189">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2a465-189">Example</span></span>

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a><span data-ttu-id="2a465-190">Switch-Ausdruck</span><span class="sxs-lookup"><span data-stu-id="2a465-190">Switch Expression</span></span>

<span data-ttu-id="2a465-191">Es wird ein *switch_expression* hinzugefügt, um `switch`ähnliche Semantik für einen Ausdrucks Kontext zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="2a465-191">A *switch_expression* is added to support `switch`-like semantics for an expression context.</span></span>

<span data-ttu-id="2a465-192">Die C# Sprachsyntax wird durch die folgenden syntaktischen Produktionen erweitert:</span><span class="sxs-lookup"><span data-stu-id="2a465-192">The C# language syntax is augmented with the following syntactic productions:</span></span>

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

<span data-ttu-id="2a465-193">Der *switch_expression* ist als *expression_statement*nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="2a465-193">The *switch_expression* is not permitted as an *expression_statement*.</span></span>

> <span data-ttu-id="2a465-194">Wir möchten dies in einer zukünftigen Revision lockern.</span><span class="sxs-lookup"><span data-stu-id="2a465-194">We are looking at relaxing this in a future revision.</span></span>

<span data-ttu-id="2a465-195">Der Typ der *switch_expression* ist der [*am häufigsten*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) vorkommende Typ der Ausdrücke, die auf der rechten Seite des `=>` Tokens der *switch_expression_arm*s angezeigt werden, wenn ein solcher Typ vorhanden ist und der Ausdruck in jedem Arm des switch-Ausdrucks implizit in diesen Typ konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="2a465-195">The type of the *switch_expression* is the [*best common type*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) of the expressions appearing to the right of the `=>` tokens of the *switch_expression_arm*s if such a type exists and the expression in every arm of the switch expression can be implicitly converted to that type.</span></span>  <span data-ttu-id="2a465-196">Außerdem fügen wir eine neue *switchausdrucks Konvertierung*hinzu, bei der es sich um eine vordefinierte implizite Konvertierung von einem Switch-Ausdruck in alle Typen handelt `T` für die eine implizite Konvertierung von jedem Arm-Ausdruck in `T`vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="2a465-196">In addition, we add a new *switch expression conversion*, which is a predefined implicit conversion from a switch expression to every type `T` for which there exists an implicit conversion from each arm's expression to `T`.</span></span>

<span data-ttu-id="2a465-197">Wenn sich das Muster einiger *switch_expression_arm*nicht auf das Ergebnis auswirken kann, tritt ein Fehler auf, da einige vorherige Muster und Wächter immer eine Entsprechung haben.</span><span class="sxs-lookup"><span data-stu-id="2a465-197">It is an error if some *switch_expression_arm*'s pattern cannot affect the result because some previous pattern and guard will always match.</span></span>

<span data-ttu-id="2a465-198">Ein Switch-Ausdruck wird als voll *ständig* bezeichnet, wenn ein Arm des switch-Ausdrucks jeden Wert seiner Eingabe behandelt.</span><span class="sxs-lookup"><span data-stu-id="2a465-198">A switch expression is said to be *exhaustive* if some arm of the switch expression handles every value of its input.</span></span>  <span data-ttu-id="2a465-199">Der Compiler erzeugt eine Warnung, wenn ein Switch-Ausdruck nicht voll *ständig*ist.</span><span class="sxs-lookup"><span data-stu-id="2a465-199">The compiler shall produce a warning if a switch expression is not *exhaustive*.</span></span>

<span data-ttu-id="2a465-200">Zur Laufzeit ist das Ergebnis des *switch_expression* der Wert des *Ausdrucks* des ersten *switch_expression_arm* , für den der Ausdruck auf der linken Seite des *switch_expression* dem Muster der *switch_expression_arm*entspricht und *für den case_guard der* *switch_expression_arm*, falls vorhanden, zu `true`ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="2a465-200">At runtime, the result of the *switch_expression* is the value of the *expression* of the first *switch_expression_arm* for which the expression on the left-hand-side of the *switch_expression* matches the *switch_expression_arm*'s pattern, and for which the *case_guard* of the *switch_expression_arm*, if present, evaluates to `true`.</span></span> <span data-ttu-id="2a465-201">Wenn keine solche *switch_expression_arm*vorhanden ist, löst die *switch_expression* eine Instanz der Ausnahme `System.Runtime.CompilerServices.SwitchExpressionException`aus.</span><span class="sxs-lookup"><span data-stu-id="2a465-201">If there is no such *switch_expression_arm*, the *switch_expression* throws an instance of the exception `System.Runtime.CompilerServices.SwitchExpressionException`.</span></span>

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a><span data-ttu-id="2a465-202">Optionale Unterstriche beim Umschalten eines tupelliterals.</span><span class="sxs-lookup"><span data-stu-id="2a465-202">Optional parens when switching on a tuple literal</span></span>

<span data-ttu-id="2a465-203">Um mit dem *switch_statement*ein tupelliteralelement zu wechseln, müssen Sie die scheinbar redundante Parameter schreiben.</span><span class="sxs-lookup"><span data-stu-id="2a465-203">In order to switch on a tuple literal using the *switch_statement*, you have to write what appear to be redundant parens</span></span>

```csharp
switch ((a, b))
{
```

<span data-ttu-id="2a465-204">Zum zulassen</span><span class="sxs-lookup"><span data-stu-id="2a465-204">To permit</span></span>

```csharp
switch (a, b)
{
```

<span data-ttu-id="2a465-205">die Klammern der Switch-Anweisung sind optional, wenn der Ausdruck, für den gewechselt wird, ein tupelliteral ist.</span><span class="sxs-lookup"><span data-stu-id="2a465-205">the parentheses of the switch statement are optional when the expression being switched on is a tuple literal.</span></span>

### <a name="order-of-evaluation-in-pattern-matching"></a><span data-ttu-id="2a465-206">Reihenfolge der Auswertung in Pattern-Matching</span><span class="sxs-lookup"><span data-stu-id="2a465-206">Order of evaluation in pattern-matching</span></span>

<span data-ttu-id="2a465-207">Die Flexibilität des Compilers beim Neuordnen der während des Musterabgleich ausgeführten Vorgänge kann die Flexibilität ermöglichen, die zur Verbesserung der Effizienz der Muster Übereinstimmung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="2a465-207">Giving the compiler flexibility in reordering the operations executed during pattern-matching can permit flexibility that can be used to improve the efficiency of pattern-matching.</span></span> <span data-ttu-id="2a465-208">Die (nicht erzwungene) Anforderung wäre, dass Eigenschaften, auf die in einem Muster zugegriffen wird, und die dekonstruktionsmethoden "Pure" (Nebeneffekt frei, idempotent usw.) sein müssen.</span><span class="sxs-lookup"><span data-stu-id="2a465-208">The (unenforced) requirement would be that properties accessed in a pattern, and the Deconstruct methods, are required to be "pure" (side-effect free, idempotent, etc).</span></span> <span data-ttu-id="2a465-209">Dies bedeutet nicht, dass wir die Reinheit als sprach Konzept hinzufügen würden, nur dass wir die compilerflexibilität beim Neuanordnen von Vorgängen ermöglichen würden.</span><span class="sxs-lookup"><span data-stu-id="2a465-209">That doesn't mean that we would add purity as a language concept, only that we would allow the compiler flexibility in reordering operations.</span></span>

<span data-ttu-id="2a465-210">**Lösung 2018-04-04 LDM**: bestätigt: der Compiler ist berechtigt, Aufrufe an `Deconstruct`, Eigenschaften Zugriffe und Aufrufe von Methoden in `ITuple`neu anzuordnen, und es kann davon ausgegangen werden, dass die zurückgegebenen Werte bei mehreren aufrufen identisch sind.</span><span class="sxs-lookup"><span data-stu-id="2a465-210">**Resolution 2018-04-04 LDM**: confirmed: the compiler is permitted to reorder calls to `Deconstruct`, property accesses, and invocations of methods in `ITuple`, and may assume that returned values are the same from multiple calls.</span></span> <span data-ttu-id="2a465-211">Der Compiler sollte keine Funktionen aufrufen, die das Ergebnis nicht beeinflussen können, und wir werden sehr vorsichtig sein, bevor wir Änderungen an der vom Compiler generierten Reihenfolge der Auswertung in Zukunft vornehmen.</span><span class="sxs-lookup"><span data-stu-id="2a465-211">The compiler should not invoke functions that cannot affect the result, and we will be very careful before making any changes to the compiler-generated order of evaluation in the future.</span></span>

### <a name="some-possible-optimizations"></a><span data-ttu-id="2a465-212">Einige mögliche Optimierungen</span><span class="sxs-lookup"><span data-stu-id="2a465-212">Some Possible Optimizations</span></span>

<span data-ttu-id="2a465-213">Die Kompilierung des Musterabgleich kann allgemeine Teile von Mustern nutzen.</span><span class="sxs-lookup"><span data-stu-id="2a465-213">The compilation of pattern matching can take advantage of common parts of patterns.</span></span> <span data-ttu-id="2a465-214">Wenn z. b. der Typtest der obersten Ebene von zwei aufeinander folgenden Mustern in einer *switch_statement* denselben Typ hat, kann der generierte Code den Typtest für das zweite Muster überspringen.</span><span class="sxs-lookup"><span data-stu-id="2a465-214">For example, if the top-level type test of two successive patterns in a *switch_statement* is the same type, the generated code can skip the type test for the second pattern.</span></span>

<span data-ttu-id="2a465-215">Wenn einige der Muster ganze Zahlen oder Zeichen folgen sind, kann der Compiler dieselbe Art von Code generieren, der für eine Switch-Anweisung in früheren Versionen der Sprache generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="2a465-215">When some of the patterns are integers or strings, the compiler can generate the same kind of code it generates for a switch-statement in earlier versions of the language.</span></span>

<span data-ttu-id="2a465-216">Weitere Informationen zu diesen Optimierungs Arten finden Sie unter [[Scott und Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "Wann ist die Heuristik für die Kompilierung wichtig?").</span><span class="sxs-lookup"><span data-stu-id="2a465-216">For more on these kinds of optimizations, see [[Scott and Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "When Do Match-Compilation Heuristics Matter?").</span></span>
