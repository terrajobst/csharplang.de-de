---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "79483877"
---
# <a name="records"></a><span data-ttu-id="5e724-101">Datensätze</span><span class="sxs-lookup"><span data-stu-id="5e724-101">records</span></span>

* <span data-ttu-id="5e724-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="5e724-102">[x] Proposed</span></span>
* <span data-ttu-id="5e724-103">[] Prototyp: [Fertig](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME) stellen</span><span class="sxs-lookup"><span data-stu-id="5e724-103">[ ] Prototype: [Complete](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="5e724-104">[] Implementierung: [in](https://github.com/dotnet/roslyn/BRANCH_NAME) Bearbeitung</span><span class="sxs-lookup"><span data-stu-id="5e724-104">[ ] Implementation: [In Progress](https://github.com/dotnet/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="5e724-105">[] Spezifikation: umschlossene Spezifikation</span><span class="sxs-lookup"><span data-stu-id="5e724-105">[ ] Specification: Draft specification enclosed</span></span>

## <a name="summary"></a><span data-ttu-id="5e724-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="5e724-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="5e724-107">Datensätze sind ein neues, vereinfachtes deklarationsformular für C# Klassen-und Strukturtypen, die die Vorteile einer Reihe einfacherer Features kombinieren.</span><span class="sxs-lookup"><span data-stu-id="5e724-107">Records are a new, simplified declaration form for C# class and struct types that combine the benefits of a number of simpler features.</span></span> <span data-ttu-id="5e724-108">Wir beschreiben die neuen Features (Aufrufer-Empfänger Parameter und *with-Expression*s), geben die Syntax und die Semantik für Daten Satz Deklarationen an und geben dann einige Beispiele an.</span><span class="sxs-lookup"><span data-stu-id="5e724-108">We describe the new features (caller-receiver parameters and *with-expression*s), give the syntax and semantics for record declarations, and then provide some examples.</span></span>


## <a name="motivation"></a><span data-ttu-id="5e724-109">Motivation</span><span class="sxs-lookup"><span data-stu-id="5e724-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="5e724-110">Eine große Anzahl von Typdeklarationen C# in ist wenig mehr als Aggregat Auflistungen typisierter Daten.</span><span class="sxs-lookup"><span data-stu-id="5e724-110">A significant number of type declarations in C# are little more than aggregate collections of typed data.</span></span> <span data-ttu-id="5e724-111">Leider erfordert das Deklarieren solcher Typen einen großen Teil des Code Bausteine.</span><span class="sxs-lookup"><span data-stu-id="5e724-111">Unfortunately, declaring such types requires a great deal of boilerplate code.</span></span> <span data-ttu-id="5e724-112">*Datensätze* bieten einen Mechanismus zum Deklarieren eines Datentyps, indem die Elemente des Aggregats zusammen mit zusätzlichem Code oder Abweichungen von der üblichen Bausteine, sofern vorhanden, beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="5e724-112">*Records* provide a mechanism for declaring a datatype by describing the members of the aggregate along with additional code or deviations from the usual boilerplate, if any.</span></span>

<span data-ttu-id="5e724-113">Siehe [Beispiele](#examples)weiter unten.</span><span class="sxs-lookup"><span data-stu-id="5e724-113">See [Examples](#examples), below.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="5e724-114">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="5e724-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a><span data-ttu-id="5e724-115">Parameter des Aufrufers-Empfängers</span><span class="sxs-lookup"><span data-stu-id="5e724-115">caller-receiver parameters</span></span>

<span data-ttu-id="5e724-116">Derzeit muss das *default-Argument* eines Methoden Parameters lauten.</span><span class="sxs-lookup"><span data-stu-id="5e724-116">Currently a method parameter's *default-argument* must be</span></span> 
- <span data-ttu-id="5e724-117">ein *konstanter Ausdruck*. noch</span><span class="sxs-lookup"><span data-stu-id="5e724-117">a *constant-expression*; or</span></span>
- <span data-ttu-id="5e724-118">ein Ausdruck der Form `new S()`, wobei `S` ein Werttyp ist. noch</span><span class="sxs-lookup"><span data-stu-id="5e724-118">an expression of the form `new S()` where `S` is a value type; or</span></span>
- <span data-ttu-id="5e724-119">ein Ausdruck der Form `default(S)`, wobei `S` ein Werttyp ist.</span><span class="sxs-lookup"><span data-stu-id="5e724-119">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="5e724-120">Dies wird erweitert, um Folgendes hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="5e724-120">We extend this to add the following</span></span>
- <span data-ttu-id="5e724-121">ein Ausdruck der Form `this.Identifier`</span><span class="sxs-lookup"><span data-stu-id="5e724-121">an expression of the form `this.Identifier`</span></span>

<span data-ttu-id="5e724-122">Dieses neue Formular wird als ' *Caller-Receiver-Standardargument*' bezeichnet und ist nur zulässig, wenn alle der folgenden Bedingungen erfüllt sind.</span><span class="sxs-lookup"><span data-stu-id="5e724-122">This new form is called a *caller-receiver default-argument*, and is allowed only if all of the following are satisfied</span></span>
- <span data-ttu-id="5e724-123">Die Methode, in der Sie angezeigt wird, ist eine Instanzmethode. immer</span><span class="sxs-lookup"><span data-stu-id="5e724-123">The method in which it appears is an instance method; and</span></span>
- <span data-ttu-id="5e724-124">Der Ausdruck `this.Identifier` an einen Instanzmember des einschließenden Typs gebunden, bei dem es sich entweder um ein Feld oder eine Eigenschaft handeln muss. immer</span><span class="sxs-lookup"><span data-stu-id="5e724-124">The expression `this.Identifier` binds to an instance member of the enclosing type, which must be either a field or a property; and</span></span>
- <span data-ttu-id="5e724-125">Der Member, an den er gebunden wird (und der `get` Accessor, wenn er eine Eigenschaft ist), ist zumindest so verfügbar wie die-Methode. immer</span><span class="sxs-lookup"><span data-stu-id="5e724-125">The member to which it binds (and the `get` accessor, if it is a property) is at least as accessible as the method; and</span></span>
- <span data-ttu-id="5e724-126">Der `this.Identifier` Typ kann implizit durch eine Identität oder eine Konvertierung in den Typ des Parameters konvertiert werden (Dies ist eine vorhandene Einschränkung für *default-Argument*).</span><span class="sxs-lookup"><span data-stu-id="5e724-126">The type of `this.Identifier` is implicitly convertible by an identity or nullable conversion to the type of the parameter (this is an existing constraint on *default-argument*).</span></span>

<span data-ttu-id="5e724-127">Wenn ein Argument bei einem Aufruf eines Funktionsmembers für einen entsprechenden optionalen Parameter mit einem *Caller-Receiver-default-Argument*ausgelassen wird, wird der Wert des Empfänger Members implizit übergeben.</span><span class="sxs-lookup"><span data-stu-id="5e724-127">When an argument is omitted from an invocation of a function member for a corresponding optional parameter with a *caller-receiver default-argument*, the value of the receiver's member is implicitly passed.</span></span> 

> <span data-ttu-id="5e724-128">**Entwurfs Hinweise**: der Hauptgrund für den Parameter "Caller-Receiver" ist die Unterstützung von *with-Expression*.</span><span class="sxs-lookup"><span data-stu-id="5e724-128">**Design Notes**: the main reason for the caller-receiver parameter is to support the *with-expression*.</span></span> <span data-ttu-id="5e724-129">Die Idee ist, dass Sie eine Methode wie diese deklarieren können.</span><span class="sxs-lookup"><span data-stu-id="5e724-129">The idea is that you can declare a method like this</span></span>
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> <span data-ttu-id="5e724-130">und dann wie folgt verwenden</span><span class="sxs-lookup"><span data-stu-id="5e724-130">and then use it like this</span></span>
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> <span data-ttu-id="5e724-131">Zum Erstellen einer neuen `Point` wie eine vorhandene `Point`, aber mit dem Wert `X` geändert.</span><span class="sxs-lookup"><span data-stu-id="5e724-131">To create a new `Point` just like an existing `Point` but with the value of `X` changed.</span></span>
> 
> <span data-ttu-id="5e724-132">Es ist eine offene Frage, ob die syntaktische Form des *with-Ausdrucks* hinzugefügt werden soll, sobald wir die Unterstützung für Aufrufer-Empfänger Parameter haben. Daher ist es möglich, dies anstelle von *with-Expression* *anstelle von* *zu* verwenden.</span><span class="sxs-lookup"><span data-stu-id="5e724-132">It is an open question whether or not the syntactic form of the *with-expression* is worth adding once we have support for caller-receiver parameters, so it is possible we would do this *instead of* rather than *in addition to* the *with-expression*.</span></span>

- <span data-ttu-id="5e724-133">[] **Problem öffnen**: wie hoch ist die Reihenfolge, in der das *Standardargument des Aufrufers-Empfängers* in Bezug auf andere Argumente ausgewertet wird?</span><span class="sxs-lookup"><span data-stu-id="5e724-133">[ ] **Open issue**: What is the order in which a *caller-receiver default-argument* is evaluated with respect to other arguments?</span></span> <span data-ttu-id="5e724-134">Sollten wir sagen, dass es nicht angegeben ist?</span><span class="sxs-lookup"><span data-stu-id="5e724-134">Should we say that it is unspecified?</span></span>

### <a name="with-expressions"></a><span data-ttu-id="5e724-135">with-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="5e724-135">with-expressions</span></span>

<span data-ttu-id="5e724-136">Ein neues Ausdrucks Formular wird vorgeschlagen:</span><span class="sxs-lookup"><span data-stu-id="5e724-136">A new expression form is proposed:</span></span>

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

<span data-ttu-id="5e724-137">Der Token-`with` ist ein neues Kontext abhängiger Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="5e724-137">The token `with` is a new context-sensitive keyword.</span></span>

<span data-ttu-id="5e724-138">Jeder *Bezeichner* auf der linken Seite eines *with_initializer* muss an ein barrierefreies Instanzfeld oder eine Eigenschaft des Typs der *primary_expression* des *with_expression*gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="5e724-138">Each *identifier* on the left of a *with_initializer* must bind to an accessible instance field or property of the type of the *primary_expression* of the *with_expression*.</span></span> <span data-ttu-id="5e724-139">Es gibt möglicherweise keinen doppelten Namen zwischen diesen bezeichlern einer angegebenen *with_expression*.</span><span class="sxs-lookup"><span data-stu-id="5e724-139">There may be no duplicated name among these identifiers of a given *with_expression*.</span></span>

<span data-ttu-id="5e724-140">Eine *with_expression* des Formulars.</span><span class="sxs-lookup"><span data-stu-id="5e724-140">A *with_expression* of the form</span></span>

> <span data-ttu-id="5e724-141">*E1* `with` `{` *Bezeichner* = *E2*... `}`</span><span class="sxs-lookup"><span data-stu-id="5e724-141">*e1* `with` `{` *identifier* = *e2*, ... `}`</span></span>

<span data-ttu-id="5e724-142">wird als Aufruf des Formulars behandelt.</span><span class="sxs-lookup"><span data-stu-id="5e724-142">is treated as an invocation of the form</span></span>

> <span data-ttu-id="5e724-143">*E1*`.With(`*Bezeichner2*`:` *E2*,...`)`</span><span class="sxs-lookup"><span data-stu-id="5e724-143">*e1*`.With(`*identifier2*`:` *e2*, ...`)`</span></span>

<span data-ttu-id="5e724-144">Dabei wird für jede Methode mit dem Namen `With`, die ein Barrierefreiheits-Instanzmember von *E1*ist, *Bezeichner2* als Name des ersten Parameters in dieser Methode ausgewählt, der über einen Caller-Receiver-Parameter verfügt *, bei dem es sich um denselben*Member wie das Instanzfeld oder die Eigenschaft handelt</span><span class="sxs-lookup"><span data-stu-id="5e724-144">Where, for each method named `With` that is an accessible instance member of *e1*, we select *identifier2* as the name of the first parameter in that method that has a caller-receiver parameter that is the same member as the instance field or property bound to *identifier*.</span></span> <span data-ttu-id="5e724-145">Wenn kein solcher Parameter gefunden werden kann, wird die Methode nicht berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="5e724-145">If no such parameter can be identified that method is eliminated from consideration.</span></span> <span data-ttu-id="5e724-146">Die aufzurufende Methode wird bei der Überladungs Auflösung aus den verbleibenden Kandidaten ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="5e724-146">The method to be invoked is selected from among the remaining candidates by overload resolution.</span></span>

> <span data-ttu-id="5e724-147">**Entwurfs Hinweise**: bei den Parametern für den aufrufenden Empfänger sind viele der Vorteile von *with-Expression* ohne dieses spezielle Syntax Formular verfügbar.</span><span class="sxs-lookup"><span data-stu-id="5e724-147">**Design Notes**: Given caller-receiver parameters, many of the benefits of the *with-expression* are available without this special syntax form.</span></span> <span data-ttu-id="5e724-148">Daher wird in Erwägung gezogen, ob es erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="5e724-148">We are therefore considering whether or not it is needed.</span></span> <span data-ttu-id="5e724-149">Der Hauptvorteil besteht darin, dass ein Programm in Bezug auf die Namen von Feldern und Eigenschaften und nicht in Bezug auf die Namen von Parametern programmiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="5e724-149">Its main benefit is allowing one to program in terms of the names of fields and properties, rather than in terms of the names of parameters.</span></span> <span data-ttu-id="5e724-150">Auf diese Weise verbessern wir sowohl die Lesbarkeit als auch die Qualität der Tools (z. b. Gehe zu Definition für den Bezeichner einer *with_expression* die anstelle eines Methoden Parameters zur-Eigenschaft navigieren würde).</span><span class="sxs-lookup"><span data-stu-id="5e724-150">In this way we improve both readability and the quality of tooling (e.g. go-to-definition on the identifier of a *with_expression* would navigate to the property rather than to a method parameter).</span></span>

- <span data-ttu-id="5e724-151">[] **Problem öffnen**: Diese Beschreibung muss geändert werden, um Erweiterungs Methoden zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="5e724-151">[ ] **Open issue**: This description should be modified to support extension methods.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="5e724-152">Mustervergleich</span><span class="sxs-lookup"><span data-stu-id="5e724-152">pattern-matching</span></span>

<span data-ttu-id="5e724-153">Eine Angabe `Deconstruct` und der zugehörigen Beziehung zum Muster Vergleich finden Sie in der [Muster Vergleichs Spezifikation](csharp-8.0/patterns.md#positional-pattern) .</span><span class="sxs-lookup"><span data-stu-id="5e724-153">See the [Pattern Matching Specification](csharp-8.0/patterns.md#positional-pattern) for a specification of `Deconstruct` and its relationship to pattern-matching.</span></span>

> <span data-ttu-id="5e724-154">**Entwurfs Hinweise**: durch die vom Compiler generierte `Deconstruct` wie hier angegeben, und die Spezifikation für den Musterabgleich, eine Daten Satz Deklaration</span><span class="sxs-lookup"><span data-stu-id="5e724-154">**Design Notes**: By virtue of the compiler-generated `Deconstruct` as specified herein, and the specification for pattern-matching, a record declaration</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="5e724-155">unterstützt die Zuordnung von Positions Mustern wie folgt:</span><span class="sxs-lookup"><span data-stu-id="5e724-155">will support positional pattern-matching as follows</span></span>
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a><span data-ttu-id="5e724-156">Daten Satz Typen Deklarationen</span><span class="sxs-lookup"><span data-stu-id="5e724-156">record type declarations</span></span>

<span data-ttu-id="5e724-157">Die Syntax für eine `class`-oder `struct` Deklaration wird erweitert, um Wert Parameter zu unterstützen. die Parameter werden zu Eigenschaften des Typs:</span><span class="sxs-lookup"><span data-stu-id="5e724-157">The syntax for a `class` or `struct` declaration is extended to support value parameters; the parameters become properties of the type:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> <span data-ttu-id="5e724-158">**Entwurfs Hinweise**: Da Daten Satz Typen häufig nützlich sind, ohne dass explizit in einem Klassen Körper deklarierte Elemente erforderlich sind, ändern wir die Syntax der Deklaration, damit ein Text einfach ein Semikolon sein kann.</span><span class="sxs-lookup"><span data-stu-id="5e724-158">**Design Notes**: Because record types are often useful without the need for any members explicitly declared in a class-body, we modify the syntax of the declaration to allow a body to be simply a semicolon.</span></span>

<span data-ttu-id="5e724-159">Eine Klasse (Struktur), die mit den *Datensatz-Parametern* deklariert wurde, wird als *Daten Satz Klasse* (Daten Satz*Struktur*) bezeichnet, von denen jeder ein *Daten Satz Typ*ist.</span><span class="sxs-lookup"><span data-stu-id="5e724-159">A class (struct) declared with the *record-parameters* is called a *record class* (*record struct*), either of which is a *record type*.</span></span>

- <span data-ttu-id="5e724-160">[] **Problem öffnen**: Wir müssen *primary_constructor_body* in die Grammatik einschließen, damit es in einer Daten Satz-Typdeklaration vorkommen kann.</span><span class="sxs-lookup"><span data-stu-id="5e724-160">[ ] **Open issue**: We need to include *primary_constructor_body* in the grammar so that it can appear inside a record type declaration.</span></span>
- <span data-ttu-id="5e724-161">[] **Problem öffnen**: welche namens Konfliktregeln gelten für die Parameternamen?</span><span class="sxs-lookup"><span data-stu-id="5e724-161">[ ] **Open issue**: What are the name conflict rules for the parameter names?</span></span> <span data-ttu-id="5e724-162">Vermutlich ist ein Konflikt mit einem Typparameter oder einem anderen *Datensatz-Parameter*nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="5e724-162">Presumably one is not allowed to conflict with a type parameter or another *record-parameter*.</span></span>
- <span data-ttu-id="5e724-163">[] **Problem öffnen**: der Bereich der Datensatz-Parameter muss angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="5e724-163">[ ] **Open issue**: We need to specify the scope of the record-parameters.</span></span> <span data-ttu-id="5e724-164">Wo können Sie verwendet werden?</span><span class="sxs-lookup"><span data-stu-id="5e724-164">Where can they be used?</span></span> <span data-ttu-id="5e724-165">Vermutlich innerhalb von instanzfeldinitialisierern und *primary_constructor_body* mindestens.</span><span class="sxs-lookup"><span data-stu-id="5e724-165">Presumably within instance field initializers and *primary_constructor_body* at least.</span></span>
- <span data-ttu-id="5e724-166">[] **Problem öffnen**: kann eine Daten Satz-Typdeklaration partiell sein?</span><span class="sxs-lookup"><span data-stu-id="5e724-166">[ ] **Open issue**: Can a record type declaration be partial?</span></span> <span data-ttu-id="5e724-167">Wenn dies der Fall ist, müssen die Parameter für jeden Teil wiederholt werden?</span><span class="sxs-lookup"><span data-stu-id="5e724-167">If so, must the parameters be repeated on each part?</span></span>

#### <a name="members-of-a-record-type"></a><span data-ttu-id="5e724-168">Mitglieder eines Daten Satz Typs</span><span class="sxs-lookup"><span data-stu-id="5e724-168">Members of a record type</span></span>

<span data-ttu-id="5e724-169">Zusätzlich zu den Membern, die im *Class-Body*deklariert sind, verfügt ein Daten Satz Typen über die folgenden zusätzlichen Member:</span><span class="sxs-lookup"><span data-stu-id="5e724-169">In addition to the members declared in the *class-body*, a record type has the following additional members:</span></span>

##### <a name="primary-constructor"></a><span data-ttu-id="5e724-170">Primärer Konstruktor</span><span class="sxs-lookup"><span data-stu-id="5e724-170">Primary Constructor</span></span>

<span data-ttu-id="5e724-171">Ein Daten Recordtyp verfügt über einen `public` Konstruktor, dessen Signatur den Wert Parametern der Typdeklaration entspricht.</span><span class="sxs-lookup"><span data-stu-id="5e724-171">A record type has a `public` constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="5e724-172">Dies wird als *primärer Konstruktor* für den Typ bezeichnet und bewirkt, dass der implizit deklarierte *Standardkonstruktor* unterdrückt wird.</span><span class="sxs-lookup"><span data-stu-id="5e724-172">This is called the *primary constructor* for the type, and causes the implicitly declared *default constructor* to be suppressed.</span></span>

<span data-ttu-id="5e724-173">Zur Laufzeit der primäre Konstruktor</span><span class="sxs-lookup"><span data-stu-id="5e724-173">At runtime the primary constructor</span></span>

* <span data-ttu-id="5e724-174">Initialisiert vom Compiler generierte Unterstützungs Felder für die Eigenschaften, die den Wert Parametern entsprechen (wenn diese Eigenschaften vom Compiler bereitgestellt werden). [Siehe 1.1.2](#1.1.2)); Seitdem</span><span class="sxs-lookup"><span data-stu-id="5e724-174">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; [see 1.1.2](#1.1.2)); then</span></span>
* <span data-ttu-id="5e724-175">führt die instanzfeldinitialisierer aus, die in der *Klasse Body*angezeigt werden. Und dann</span><span class="sxs-lookup"><span data-stu-id="5e724-175">executes the instance field initializers appearing in the *class-body*; and then</span></span>
* <span data-ttu-id="5e724-176">Ruft einen Basisklassenkonstruktor auf:</span><span class="sxs-lookup"><span data-stu-id="5e724-176">invokes a base class constructor:</span></span>
    * <span data-ttu-id="5e724-177">Wenn im *record_base_arguments*Argumente vorhanden sind, wird ein Basiskonstruktor aufgerufen, der durch die Überladungs Auflösung mit diesen Argumenten ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="5e724-177">If there are arguments in the *record_base_arguments*, a base constructor selected by overload resolution with these arguments is invoked;</span></span>
    * <span data-ttu-id="5e724-178">Andernfalls wird ein Basiskonstruktor ohne Argumente aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="5e724-178">Otherwise a base constructor is invoked with no arguments.</span></span>
* <span data-ttu-id="5e724-179">führt den Text der einzelnen *primary_constructor_body*(sofern vorhanden) in der Quell Reihenfolge aus.</span><span class="sxs-lookup"><span data-stu-id="5e724-179">executes the body of each *primary_constructor_body*, if any, in source order.</span></span>

- <span data-ttu-id="5e724-180">[] **Problem öffnen**: Wir müssen diese Reihenfolge angeben, insbesondere in Kompilierungs Einheiten für partiale.</span><span class="sxs-lookup"><span data-stu-id="5e724-180">[ ] **Open issue**: We need to specify that order, particularly across compilation units for partials.</span></span>
- <span data-ttu-id="5e724-181">[] **Problem öffnen**: Wir müssen angeben, dass jeder explizit deklarierte Konstruktor mit dem primären Konstruktor verkettet werden muss.</span><span class="sxs-lookup"><span data-stu-id="5e724-181">[ ] **Open Issue**: We need to specify that every explicitly declared constructor must chain to the primary constructor.</span></span>
- <span data-ttu-id="5e724-182">[] **Problem öffnen**: sollte es zulässig sein, den Zugriffsmodifizierer für den primären Konstruktor zu ändern?</span><span class="sxs-lookup"><span data-stu-id="5e724-182">[ ] **Open issue**: Should it be allowed to change the access modifier on the primary constructor?</span></span>
- <span data-ttu-id="5e724-183">[] **Problem öffnen**: in einer Daten Satzstruktur ist es ein Fehler, dass keine Daten Satz Parameter vorhanden sind?</span><span class="sxs-lookup"><span data-stu-id="5e724-183">[ ] **Open issue**: In a record struct, it is an error for there to be no record parameters?</span></span>

##### <a name="primary-constructor-body"></a><span data-ttu-id="5e724-184">Primärer Konstruktortext</span><span class="sxs-lookup"><span data-stu-id="5e724-184">Primary constructor body</span></span>

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

<span data-ttu-id="5e724-185">Eine *primary_constructor_body* darf nur innerhalb einer Daten Satz Typen Deklaration verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5e724-185">A *primary_constructor_body* may only be used within a record type declaration.</span></span> <span data-ttu-id="5e724-186">Der *Bezeichner* eines *primary_constructor_body* muss den Daten Satz Typen benennen, in dem er deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="5e724-186">The *identifier* of a *primary_constructor_body* shall name the record type in which it is declared.</span></span>

<span data-ttu-id="5e724-187">Der *primary_constructor_body* deklariert keinen Member eigenständig, stellt jedoch eine Möglichkeit für den Programmierer dar, Attribute für bereitzustellen und den Zugriff auf den primären Konstruktor eines Daten Satz Typs anzugeben.</span><span class="sxs-lookup"><span data-stu-id="5e724-187">The *primary_constructor_body* does not declare a member on its own, but is a way for the programmer to provide attributes for, and specify the access of, a record type's primary constructor.</span></span> <span data-ttu-id="5e724-188">Außerdem kann der Programmierer zusätzlichen Code bereitstellen, der ausgeführt wird, wenn eine Instanz des Daten Satz Typs erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="5e724-188">It also enables the programmer to provide additional code that will be executed when an instance of the record type is constructed.</span></span>

- <span data-ttu-id="5e724-189">[] **Problem öffnen**: Beachten Sie, dass ein strukturstandardkonstruktor Dies umgeht.</span><span class="sxs-lookup"><span data-stu-id="5e724-189">[ ] **Open issue**: We should note that a struct default constructor bypasses this.</span></span>
- <span data-ttu-id="5e724-190">[] **Problem öffnen**: Wir sollten die Ausführungsreihenfolge der Initialisierung angeben.</span><span class="sxs-lookup"><span data-stu-id="5e724-190">[ ] **Open issue**: We should specify the execution order of initialization.</span></span>
- <span data-ttu-id="5e724-191">[] **Problem öffnen**: sollte etwas wie ein *primary_constructor_body* (vermutlich ohne Attribute und Modifizierer) in einer nicht-Datensatz-Typdeklaration zulässig sein, und es sollte wie der Code eines instanzfeldinitialisierers behandelt werden?</span><span class="sxs-lookup"><span data-stu-id="5e724-191">[ ] **Open issue**: Should we allow something like a *primary_constructor_body* (presumably without attributes and modifiers) in a non-record type declaration, and treat it like we would the code of an instance field initializer?</span></span>

##### <a name="properties"></a><span data-ttu-id="5e724-192">Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="5e724-192">Properties</span></span>

<span data-ttu-id="5e724-193">Für jeden Datensatz-Parameter einer Daten Satz-Typdeklaration gibt es einen entsprechenden `public` Eigenschaftenmember, dessen Name und Typ aus der Deklaration des Wert Parameters entnommen werden.</span><span class="sxs-lookup"><span data-stu-id="5e724-193">For each record parameter of a record type declaration there is a corresponding `public` property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="5e724-194">Der Name ist der *Bezeichner* der *record_property_name*, sofern vorhanden, oder der *Bezeichner* des *record_parameter* andernfalls.</span><span class="sxs-lookup"><span data-stu-id="5e724-194">Its name is the *identifier* of the *record_property_name*, if present, or the *identifier* of the *record_parameter* otherwise.</span></span> <span data-ttu-id="5e724-195">Wenn keine konkrete (d. h. nicht abstrakte) öffentliche Eigenschaft mit einem `get`-Accessor und mit diesem Namen und Typ explizit deklariert oder geerbt wird, wird der Compiler wie folgt erstellt:</span><span class="sxs-lookup"><span data-stu-id="5e724-195">If no concrete (i.e. non-abstract) public property with a `get` accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

* <span data-ttu-id="5e724-196">Für eine *Daten Satzstruktur* oder eine `sealed` *Record-Klasse*:</span><span class="sxs-lookup"><span data-stu-id="5e724-196">For a *record struct* or a `sealed` *record class*:</span></span>
 * <span data-ttu-id="5e724-197">Ein `private` `readonly` Feld wird als dahinter liegendes Feld für eine `readonly` Eigenschaft erstellt.</span><span class="sxs-lookup"><span data-stu-id="5e724-197">A `private` `readonly` field is produced as a backing field for a `readonly` property.</span></span> <span data-ttu-id="5e724-198">Der Wert wird während der Erstellung mit dem Wert des entsprechenden primären Konstruktorparameters initialisiert.</span><span class="sxs-lookup"><span data-stu-id="5e724-198">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span>
 * <span data-ttu-id="5e724-199">Der `get` Accessor der Eigenschaft wird implementiert, um den Wert des dahinter liegenden Felds zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="5e724-199">The property's `get` accessor is implemented to return the value of the backing field.</span></span>
 * <span data-ttu-id="5e724-200">Jede "übereinstimmende" vererbte virtuelle Eigenschaft `get` Accessor wird überschrieben.</span><span class="sxs-lookup"><span data-stu-id="5e724-200">Each "matching" inherited virtual property's `get` accessor is overridden.</span></span>

> <span data-ttu-id="5e724-201">**Entwurfs Hinweise**: anders ausgedrückt: Wenn Sie eine Basisklasse erweitern oder eine Schnittstelle implementieren, die eine öffentliche abstrakte Eigenschaft mit demselben Namen und Typ wie ein Datensatz-Parameter deklariert, wird diese Eigenschaft überschrieben oder implementiert.</span><span class="sxs-lookup"><span data-stu-id="5e724-201">**Design notes**: In other words, if you extend a base class or implement an interface that declares a public abstract property with the same name and type as a record parameter, that property is overridden or implemented.</span></span>

- <span data-ttu-id="5e724-202">[] **Problem öffnen**: sollte es möglich sein, den Zugriffsmodifizierer für eine Eigenschaft zu ändern, wenn er explizit deklariert wird?</span><span class="sxs-lookup"><span data-stu-id="5e724-202">[ ] **Open issue**: Should it be possible to change the access modifier on a property when it is explicitly declared?</span></span>
- <span data-ttu-id="5e724-203">[] **Problem öffnen**: sollte es möglich sein, ein Feld für eine Eigenschaft zu ersetzen?</span><span class="sxs-lookup"><span data-stu-id="5e724-203">[ ] **Open issue**: Should it be possible to substitute a field for a property?</span></span>

##### <a name="object-methods"></a><span data-ttu-id="5e724-204">Objektmethoden</span><span class="sxs-lookup"><span data-stu-id="5e724-204">Object Methods</span></span>

<span data-ttu-id="5e724-205">Bei einer *Daten Satzstruktur* oder einer `sealed`- *Daten Satz Klasse*werden Implementierungen der Methoden `object.GetHashCode()` und `object.Equals(object)` vom Compiler erstellt, es sei denn, Sie werden vom Benutzer bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="5e724-205">For a *record struct* or a `sealed` *record class*, implementations of the methods `object.GetHashCode()` and `object.Equals(object)` are produced by the compiler unless provided by the user.</span></span>

- <span data-ttu-id="5e724-206">[] **Problem öffnen**: Wir sollten die Implementierung genau angeben.</span><span class="sxs-lookup"><span data-stu-id="5e724-206">[ ] **Open issue**: We should precisely specify their implementation.</span></span>
- <span data-ttu-id="5e724-207">[] **Problem öffnen**: Wir sollten auch die-Schnittstelle `IEquatable<T>` für den Daten Satz Typ hinzufügen und angeben, dass Implementierungen bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="5e724-207">[ ] **Open issue**: We should also add the interface `IEquatable<T>` for the record type and specify that implementations are provided.</span></span>
- <span data-ttu-id="5e724-208">[] **Problem öffnen**: Wir sollten auch angeben, dass alle `IEquatable<T>.Equals`implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="5e724-208">[ ] **Open issue**: We should also specify that we implement every `IEquatable<T>.Equals`.</span></span>
- <span data-ttu-id="5e724-209">[] **Problem öffnen**: Wir sollten genau angeben, wie das Problem von gleich bei der Daten Satz Vererbung gelöst wird: insbesondere, wie Gleichheits Methoden generiert werden, sodass Sie symmetrisch, transitiv, reflexiv usw. sind.</span><span class="sxs-lookup"><span data-stu-id="5e724-209">[ ] **Open issue**: We should specify precisely how we solve the problem of Equals in the face of record inheritance: specifically how we generate equality methods such that they are symmetric, transitive, reflexive, etc.</span></span>
- <span data-ttu-id="5e724-210">[] **Problem öffnen**: Es wurde vorgeschlagen, dass wir `operator ==` und `operator !=` für Daten Satz Typen implementieren.</span><span class="sxs-lookup"><span data-stu-id="5e724-210">[ ] **Open issue**: It has been proposed that we implement `operator ==` and `operator !=` for record types.</span></span>
- <span data-ttu-id="5e724-211">[] **Problem öffnen**: sollte eine Implementierung von `object.ToString`automatisch generiert werden?</span><span class="sxs-lookup"><span data-stu-id="5e724-211">[ ] **Open issue**: Should we auto-generate an implementation of `object.ToString`?</span></span>

##### `Deconstruct`

<span data-ttu-id="5e724-212">Ein Daten Recordtyp verfügt über eine vom Compiler generierte `public` Methode `void Deconstruct`, es sei denn, eine mit einer Signatur wird vom Benutzer bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="5e724-212">A record type has a compiler-generated `public` method `void Deconstruct` unless one with any signature is provided by the user.</span></span> <span data-ttu-id="5e724-213">Jeder Parameter ist ein `out` Parameter desselben Namens und Typs wie der entsprechende Parameter des Daten Satz Typs.</span><span class="sxs-lookup"><span data-stu-id="5e724-213">Each parameter is an `out` parameter of the same name and type as the corresponding parameter of the record type.</span></span> <span data-ttu-id="5e724-214">Die vom Compiler bereitgestellte Implementierung dieser Methode muss jedem `out`-Parameter den Wert der entsprechenden-Eigenschaft zuweisen.</span><span class="sxs-lookup"><span data-stu-id="5e724-214">The compiler-provided implementation of this method shall assign each `out` parameter with the value of the corresponding property.</span></span>

<span data-ttu-id="5e724-215">Die [Muster Vergleichs Spezifikation](csharp-8.0/patterns.md#positional-pattern) für die Semantik von `Deconstruct`finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="5e724-215">See [the pattern-matching specification](csharp-8.0/patterns.md#positional-pattern) for the semantics of `Deconstruct`.</span></span>

##### <a name="with-method"></a><span data-ttu-id="5e724-216">`With` -Methode</span><span class="sxs-lookup"><span data-stu-id="5e724-216">`With` method</span></span>

<span data-ttu-id="5e724-217">Es sei denn, es gibt einen vom Benutzer deklarierten Member mit dem Namen `With` deklariert wurde. ein Daten Recordtyp verfügt über eine vom Compiler bereitgestellte Methode mit dem Namen `With`, deren Rückgabetyp der Datensatz selbst ist und einen Wert Parameter enthält, der den einzelnen *Datensatz-Para* Metern in derselben Reihenfolge entspricht, in der diese Parameter in der Deklaration</span><span class="sxs-lookup"><span data-stu-id="5e724-217">Unless there is a user-declared member named `With` declared, a record type has a compiler-provided method named `With` whose return type is the record type itself, and containing one value parameter corresponding to each *record-parameter* in the same order that these parameters appear in the record type declaration.</span></span> <span data-ttu-id="5e724-218">Jeder Parameter muss über ein *Caller-Receiver-default-Argument* der entsprechenden Eigenschaft verfügen.</span><span class="sxs-lookup"><span data-stu-id="5e724-218">Each parameter shall have a *caller-receiver default-argument* of the corresponding property.</span></span>

<span data-ttu-id="5e724-219">In einer `abstract` Record-Klasse ist die vom Compiler angegebene `With` Methode abstrakt.</span><span class="sxs-lookup"><span data-stu-id="5e724-219">In an `abstract` record class, the compiler-provided `With` method is abstract.</span></span> <span data-ttu-id="5e724-220">In einer Daten Satzstruktur oder einer versiegelten Daten Satz Klasse wird die vom Compiler bereitgestellte `With` Methode `sealed`.</span><span class="sxs-lookup"><span data-stu-id="5e724-220">In a record struct, or a sealed record class, the compiler-provided `With` method is `sealed`.</span></span> <span data-ttu-id="5e724-221">Andernfalls ist die vom Compiler bereitgestellte `With` Methode "virtuell", und ihre Implementierung gibt eine neue-Instanz zurück, die durch den Aufruf des primären Konstruktors mit den Parametern als Argumente erzeugt wird, um eine neue Instanz aus den Parametern zu erstellen und diese neue Instanz zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="5e724-221">Otherwise the compiler-provided `With` method is \`virtual and its implementation shall return a new instance produced by invoking the primary constructor with the parameters as arguments to create a new instance from the parameters, and return that new instance.</span></span>

- <span data-ttu-id="5e724-222">[] **Problem öffnen**: Wir sollten auch unter den Bedingungen angeben, die von uns überschrieben werden, oder die geerbten virtuellen `With` Methoden oder `With` Methoden von implementierten Schnittstellen implementieren</span><span class="sxs-lookup"><span data-stu-id="5e724-222">[ ] **Open issue**: We should also specify under what conditions we override or implement inherited virtual `With` methods or `With` methods from implemented interfaces.</span></span>
- <span data-ttu-id="5e724-223">[] **Problem öffnen**: Wir sollten sagen, was geschieht, wenn wir eine nicht virtuelle `With` Methode erben.</span><span class="sxs-lookup"><span data-stu-id="5e724-223">[ ] **Open issue**: We should say what happens when we inherit a non-virtual `With` method.</span></span>

> <span data-ttu-id="5e724-224">**Entwurfs Hinweise**: Da Daten Satz Typen standardmäßig unveränderlich sind, bietet die `With`-Methode eine Möglichkeit, eine neue-Instanz zu erstellen, die mit einer vorhandenen Instanz identisch ist, jedoch mit den ausgewählten Eigenschaften mit neuen Werten.</span><span class="sxs-lookup"><span data-stu-id="5e724-224">**Design notes**: Because record types are by default immutable, the `With` method provides a way of creating a new instance that is the same as an existing instance but with selected properties given new values.</span></span> <span data-ttu-id="5e724-225">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="5e724-225">For example, given</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="5e724-226">ein vom Compiler bereitgestellter Member</span><span class="sxs-lookup"><span data-stu-id="5e724-226">there is a compiler-provided member</span></span>
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> <span data-ttu-id="5e724-227">Ermöglicht eine Variable des Daten Satz Typs.</span><span class="sxs-lookup"><span data-stu-id="5e724-227">Which enables an variable of the record type</span></span>
> ```cs
> var p = new Point(3, 4);
> ```
> <span data-ttu-id="5e724-228">zum Ersetzen durch eine Instanz, die eine oder mehrere Eigenschaften unterscheidet</span><span class="sxs-lookup"><span data-stu-id="5e724-228">to be replaced with an instance that has one or more properties different</span></span>
> ```cs
>     p = p.With(X: 5);
> ```
> <span data-ttu-id="5e724-229">Dies kann auch mithilfe des *with_expression*ausgedrückt werden:</span><span class="sxs-lookup"><span data-stu-id="5e724-229">This can also be expressed using the *with_expression*:</span></span>
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a><span data-ttu-id="5e724-230">Beispiele</span><span class="sxs-lookup"><span data-stu-id="5e724-230">Examples</span></span>

#### <a name="compatibility-of-record-types"></a><span data-ttu-id="5e724-231">Kompatibilität von Daten Satz Typen</span><span class="sxs-lookup"><span data-stu-id="5e724-231">Compatibility of record types</span></span>

<span data-ttu-id="5e724-232">Da der Programmierer der Deklaration eines Daten Satz Typs Mitglieder hinzufügen kann, ist es oft möglich, den Satz von Daten Satz Elementen zu ändern, ohne dass sich dies auf vorhandene Clients auswirkt.</span><span class="sxs-lookup"><span data-stu-id="5e724-232">Because the programmer can add members to a record type declaration, it is often possible to change the set of record elements without affecting existing clients.</span></span> <span data-ttu-id="5e724-233">Beispielsweise bei einer ersten Version eines Daten Satz Typs.</span><span class="sxs-lookup"><span data-stu-id="5e724-233">For example, given an initial version of a record type</span></span>

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

<span data-ttu-id="5e724-234">Ein neues Element des Daten Satz Typs kann in der nächsten Revision des Typs kompatibel hinzugefügt werden, ohne die Binär-oder Quell Kompatibilität zu beeinträchtigen:</span><span class="sxs-lookup"><span data-stu-id="5e724-234">A new element of the record type can be compatibly added in the next revision of the type without affecting binary or source compatibility:</span></span>

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a><span data-ttu-id="5e724-235">Beispiel für Daten Satzstruktur</span><span class="sxs-lookup"><span data-stu-id="5e724-235">record struct example</span></span>

<span data-ttu-id="5e724-236">Diese Daten Satzstruktur</span><span class="sxs-lookup"><span data-stu-id="5e724-236">This record struct</span></span>

```cs
public struct Pair(object First, object Second);
```

<span data-ttu-id="5e724-237">wird in diesen Code übersetzt</span><span class="sxs-lookup"><span data-stu-id="5e724-237">is translated to this code</span></span>

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- <span data-ttu-id="5e724-238">[] **Problem öffnen**: sollte die Implementierung von Gleichheits (paar anderen) ein öffentliches Member des Paars sein?</span><span class="sxs-lookup"><span data-stu-id="5e724-238">[ ] **Open issue**: should the implementation of Equals(Pair other) be a public member of Pair?</span></span>
- <span data-ttu-id="5e724-239">[] **Problem öffnen**: Diese Implementierung von `Equals` ist bei Vererbung nicht symmetrisch.</span><span class="sxs-lookup"><span data-stu-id="5e724-239">[ ] **Open issue**: This implementation of `Equals` is not symmetric in the face of inheritance.</span></span>
- <span data-ttu-id="5e724-240">[] **Problem öffnen**: sollte ein Datensatz `operator ==` und `operator !=`deklarieren?</span><span class="sxs-lookup"><span data-stu-id="5e724-240">[ ] **Open issue**: Should a record declare `operator ==` and `operator !=`?</span></span>

> <span data-ttu-id="5e724-241">**Entwurfs Hinweise**: da ein Daten Recordtyp von einem anderen Typ erben kann und diese Implementierung von `Equals` in diesem Fall nicht symmetrisch wäre, ist er nicht richtig.</span><span class="sxs-lookup"><span data-stu-id="5e724-241">**Design notes**: Because one record type can inherit from another, and this implementation of `Equals` would not be symmetric in that case, it is not correct.</span></span> <span data-ttu-id="5e724-242">Es wird vorgeschlagen, Gleichheit auf diese Weise zu implementieren:</span><span class="sxs-lookup"><span data-stu-id="5e724-242">We propose to implement equality this way:</span></span>
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> <span data-ttu-id="5e724-243">Abgeleitete Datensätze würden `override EqualityContract`.</span><span class="sxs-lookup"><span data-stu-id="5e724-243">Derived records would `override EqualityContract`.</span></span> <span data-ttu-id="5e724-244">Die weniger attraktive Alternative besteht darin, die Vererbung einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="5e724-244">The less attractive alternative is to restrict inheritance.</span></span>

#### <a name="sealed-record-example"></a><span data-ttu-id="5e724-245">Beispiel für versiegelten Datensatz</span><span class="sxs-lookup"><span data-stu-id="5e724-245">sealed record example</span></span>

<span data-ttu-id="5e724-246">Diese versiegelte Daten Satz Klasse</span><span class="sxs-lookup"><span data-stu-id="5e724-246">This sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa);
```

<span data-ttu-id="5e724-247">wird in diesen Code übersetzt</span><span class="sxs-lookup"><span data-stu-id="5e724-247">is translated into this code</span></span>

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a><span data-ttu-id="5e724-248">Beispiel für eine abstrakte Daten Satz Klasse</span><span class="sxs-lookup"><span data-stu-id="5e724-248">abstract record class example</span></span>

<span data-ttu-id="5e724-249">Diese abstrakte Daten Satz Klasse</span><span class="sxs-lookup"><span data-stu-id="5e724-249">This abstract record class</span></span>

```cs
public abstract class Person(string Name);
```

<span data-ttu-id="5e724-250">wird in diesen Code übersetzt</span><span class="sxs-lookup"><span data-stu-id="5e724-250">is translated into this code</span></span>

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a><span data-ttu-id="5e724-251">Kombinieren von abstrakten und versiegelten Datensätzen</span><span class="sxs-lookup"><span data-stu-id="5e724-251">combining abstract and sealed records</span></span>

<span data-ttu-id="5e724-252">Bei Angabe der abstrakten Daten Satz Klasse `Person` oberhalb dieser versiegelten Daten Satz Klasse</span><span class="sxs-lookup"><span data-stu-id="5e724-252">Given the abstract record class `Person` above, this sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

<span data-ttu-id="5e724-253">wird in diesen Code übersetzt</span><span class="sxs-lookup"><span data-stu-id="5e724-253">is translated into this code</span></span>

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a><span data-ttu-id="5e724-254">Nachteile</span><span class="sxs-lookup"><span data-stu-id="5e724-254">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="5e724-255">Wie bei allen Sprach Features müssen wir Fragen, ob die zusätzliche Komplexität der Sprache in der zusätzlichen Klarheit für den Text der C# Programme, die von der Funktion profitieren würden, zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="5e724-255">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="5e724-256">Alternativen</span><span class="sxs-lookup"><span data-stu-id="5e724-256">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="5e724-257">Wir haben in C# 6 als *primäre Konstruktoren* hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5e724-257">We considered adding *primary constructors* in C# 6.</span></span> <span data-ttu-id="5e724-258">Obwohl Sie die gleiche syntaktische Oberfläche wie dieser Vorschlag belegen, haben wir festgestellt, dass Sie die Vorteile der Datensätze nicht nur kurz hatten.</span><span class="sxs-lookup"><span data-stu-id="5e724-258">Although they occupy the same syntactic surface as this proposal, we found that they fell short of the advantages offered by records.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="5e724-259">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="5e724-259">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="5e724-260">Offene Fragen werden im Hauptteil des Angebots angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5e724-260">Open questions appear in the body of the proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="5e724-261">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="5e724-261">Design meetings</span></span>

<span data-ttu-id="5e724-262">TBD</span><span class="sxs-lookup"><span data-stu-id="5e724-262">TBD</span></span>
