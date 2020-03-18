---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2020
ms.locfileid: "79484105"
---
# <a name="nullable-reference-types-specification"></a><span data-ttu-id="dacd9-101">Verweis Typen Spezifikation für Nullwerte zulassen</span><span class="sxs-lookup"><span data-stu-id="dacd9-101">Nullable Reference Types Specification</span></span>

<span data-ttu-id="dacd9-102">Dies ist eine laufende Arbeit. mehrere Teile fehlen oder sind unvollständig.</span><span class="sxs-lookup"><span data-stu-id="dacd9-102">\*\*\* This is a work in progress - several parts are missing or incomplete.</span></span> ***

## <a name="syntax"></a><span data-ttu-id="dacd9-103">Syntax</span><span class="sxs-lookup"><span data-stu-id="dacd9-103">Syntax</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="dacd9-104">Nullwerte zulassende Verweistypen</span><span class="sxs-lookup"><span data-stu-id="dacd9-104">Nullable reference types</span></span>

<span data-ttu-id="dacd9-105">Verweis Typen, die NULL-Werte zulassen, haben dieselbe Syntax `T?` wie die Kurzform von Werttypen, die NULL-Werte zulassen, aber nicht über eine entsprechende lange Form verfügen.</span><span class="sxs-lookup"><span data-stu-id="dacd9-105">Nullable reference types have the same syntax `T?` as the short form of nullable value types, but do not have a corresponding long form.</span></span>

<span data-ttu-id="dacd9-106">Zum Zweck der Spezifikation wird die aktuelle `nullable_type` Produktion in `nullable_value_type`umbenannt, und es wird eine `nullable_reference_type` Produktion hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="dacd9-106">For the purposes of the specification, the current `nullable_type` production is renamed to `nullable_value_type`, and a `nullable_reference_type` production is added:</span></span>

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

<span data-ttu-id="dacd9-107">Der `non_nullable_reference_type` in einem `nullable_reference_type` muss ein Verweistyp sein, der keine NULL-Werte zulässt (Klasse, Schnittstelle, Delegat oder Array), oder ein Typparameter, der auf einen Verweistyp beschränkt ist, der keine NULL-Werte zulässt (durch die `class`-Einschränkung oder eine andere Klasse als `object`).</span><span class="sxs-lookup"><span data-stu-id="dacd9-107">The `non_nullable_reference_type` in a `nullable_reference_type` must be a non-nullable reference type (class, interface, delegate or array), or a type parameter that is constrained to be a non-nullable reference type (through the `class` constraint, or a class other than `object`).</span></span>

<span data-ttu-id="dacd9-108">Verweis Typen, die NULL-Werte zulassen, können nicht in folgenden Positionen auftreten:</span><span class="sxs-lookup"><span data-stu-id="dacd9-108">Nullable reference types cannot occur in the following positions:</span></span>

- <span data-ttu-id="dacd9-109">als Basisklasse oder Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="dacd9-109">as a base class or interface</span></span>
- <span data-ttu-id="dacd9-110">als Empfänger eines `member_access`</span><span class="sxs-lookup"><span data-stu-id="dacd9-110">as the receiver of a `member_access`</span></span>
- <span data-ttu-id="dacd9-111">als `type` in einer `object_creation_expression`</span><span class="sxs-lookup"><span data-stu-id="dacd9-111">as the `type` in an `object_creation_expression`</span></span>
- <span data-ttu-id="dacd9-112">als `delegate_type` in einer `delegate_creation_expression`</span><span class="sxs-lookup"><span data-stu-id="dacd9-112">as the `delegate_type` in a `delegate_creation_expression`</span></span>
- <span data-ttu-id="dacd9-113">als `type` in einer `is_expression`, eine `catch_clause` oder eine `type_pattern`</span><span class="sxs-lookup"><span data-stu-id="dacd9-113">as the `type` in an `is_expression`, a `catch_clause` or a `type_pattern`</span></span>
- <span data-ttu-id="dacd9-114">als `interface` in einem voll qualifizierten Schnittstellenmember-Namen</span><span class="sxs-lookup"><span data-stu-id="dacd9-114">as the `interface` in a fully qualified interface member name</span></span>

<span data-ttu-id="dacd9-115">Eine Warnung wird bei einer `nullable_reference_type` angegeben, bei der der Anmerkung-Kontext der Nullwerte deaktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="dacd9-115">A warning is given on a `nullable_reference_type` where the nullable annotation context is disabled.</span></span>

### <a name="nullable-class-constraint"></a><span data-ttu-id="dacd9-116">NULL-Werte zulassen-Klassen Einschränkung</span><span class="sxs-lookup"><span data-stu-id="dacd9-116">Nullable class constraint</span></span>

<span data-ttu-id="dacd9-117">Die `class`-Einschränkung verfügt über ein `class?`, das NULL-Werte zulässt:</span><span class="sxs-lookup"><span data-stu-id="dacd9-117">The `class` constraint has a nullable counterpart `class?`:</span></span>

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a><span data-ttu-id="dacd9-118">Der NULL-Forgiving-Operator</span><span class="sxs-lookup"><span data-stu-id="dacd9-118">The null-forgiving operator</span></span>

<span data-ttu-id="dacd9-119">Der postfix `!`-Operator wird als NULL-forder Operator bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="dacd9-119">The post-fix `!` operator is called the null-forgiving operator.</span></span>

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

<span data-ttu-id="dacd9-120">Der `primary_expression` muss ein Verweistyp sein.</span><span class="sxs-lookup"><span data-stu-id="dacd9-120">The `primary_expression` must be of a reference type.</span></span>  

<span data-ttu-id="dacd9-121">Der postfix-`!` Operator hat keinen Lauf Zeiteffekt und wertet das Ergebnis des zugrunde liegenden Ausdrucks aus.</span><span class="sxs-lookup"><span data-stu-id="dacd9-121">The postfix `!` operator has no runtime effect - it evaluates to the result of the underlying expression.</span></span> <span data-ttu-id="dacd9-122">Die einzige Rolle besteht darin, den NULL-Status des Ausdrucks zu ändern und die bei der Verwendung angegebenen Warnungen einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="dacd9-122">Its only role is to change the null state of the expression, and to limit warnings given on its use.</span></span>

### <a name="nullable-implicitly-typed-local-variables"></a><span data-ttu-id="dacd9-123">implizit typisierte lokale Variablen, die NULL zulassen</span><span class="sxs-lookup"><span data-stu-id="dacd9-123">nullable implicitly typed local variables</span></span>

<span data-ttu-id="dacd9-124">`var` leitet einen Typ mit Anmerkungen für Verweis Typen ab.</span><span class="sxs-lookup"><span data-stu-id="dacd9-124">`var` infers an annotated type for reference types.</span></span>
<span data-ttu-id="dacd9-125">Beispielsweise wird in `var s = "";` die `var` als `string?`abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="dacd9-125">For instance, in `var s = "";` the `var` is inferred as `string?`.</span></span>

### <a name="nullable-compiler-directives"></a><span data-ttu-id="dacd9-126">Nullable-Compilerdirektiven</span><span class="sxs-lookup"><span data-stu-id="dacd9-126">Nullable compiler directives</span></span>

<span data-ttu-id="dacd9-127">`#nullable` Direktiven steuern den Kontext, der auf NULL festgelegt werden kann, und Warnungen.</span><span class="sxs-lookup"><span data-stu-id="dacd9-127">`#nullable` directives control the nullable annotation and warning contexts.</span></span>

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

<span data-ttu-id="dacd9-128">`#pragma warning` Direktiven werden erweitert, um das Ändern des Warnungs Kontexts, der NULL-Werte zulässt, und das Zulassen von einzelnen Warnungen auch dann zuzulassen, wenn Sie standardmäßig deaktiviert sind:</span><span class="sxs-lookup"><span data-stu-id="dacd9-128">`#pragma warning` directives are expanded to allow changing the nullable warning context, and to allow individual warnings to be enabled on even when they're disabled by default:</span></span>

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

<span data-ttu-id="dacd9-129">Beachten Sie, dass die neue Form von `pragma_warning_body` `nullable_action`verwendet, nicht `warning_action`.</span><span class="sxs-lookup"><span data-stu-id="dacd9-129">Note that the new form of `pragma_warning_body` uses `nullable_action`, not `warning_action`.</span></span>

## <a name="nullable-contexts"></a><span data-ttu-id="dacd9-130">Nullable-Kontexte</span><span class="sxs-lookup"><span data-stu-id="dacd9-130">Nullable contexts</span></span>

<span data-ttu-id="dacd9-131">Jede Zeile des Quellcodes enthält einen *Anmerkung-Kontext mit NULL-Werten* und einen *Warnungs Kontext*, der NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="dacd9-131">Every line of source code has a *nullable annotation context* and a *nullable warning context*.</span></span> <span data-ttu-id="dacd9-132">Diese Steuern, ob auf NULL festleg Bare Anmerkungen wirksam sind und ob NULL-Zulässigkeit-Warnungen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="dacd9-132">These control whether nullable annotations have effect, and whether nullability warnings are given.</span></span> <span data-ttu-id="dacd9-133">Der Anmerkung-Kontext einer bestimmten Zeile ist entweder *deaktiviert* oder *aktiviert*.</span><span class="sxs-lookup"><span data-stu-id="dacd9-133">The annotation context of a given line is either *disabled* or *enabled*.</span></span> <span data-ttu-id="dacd9-134">Der Warnungs Kontext einer bestimmten Zeile ist entweder *deaktiviert* oder *aktiviert*.</span><span class="sxs-lookup"><span data-stu-id="dacd9-134">The warning context of a given line is either *disabled* or *enabled*.</span></span>

<span data-ttu-id="dacd9-135">Beide Kontexte können auf Projektebene (außerhalb des C# Quellcodes) oder an einer beliebigen Stelle innerhalb einer Quelldatei über `#nullable` Präprozessordirektiven angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="dacd9-135">Both contexts can be specified at the project level (outside of C# source code), or anywhere within a source file via `#nullable` pre-processor directives.</span></span> <span data-ttu-id="dacd9-136">Wenn keine Einstellungen auf Projektebene bereitgestellt werden, ist der Standardwert, wenn beide Kontexte *deaktiviert*werden.</span><span class="sxs-lookup"><span data-stu-id="dacd9-136">If no project level settings are provided the default is for both contexts to be *disabled*.</span></span>

<span data-ttu-id="dacd9-137">Die `#nullable`-Direktive steuert die-Anmerkung und die Warnungs Kontexte innerhalb des Quell Texts und hat Vorrang vor den Einstellungen auf Projektebene.</span><span class="sxs-lookup"><span data-stu-id="dacd9-137">The `#nullable` directive controls the annotation and warning contexts within the source text, and take precedence over the project-level settings.</span></span>

<span data-ttu-id="dacd9-138">Eine-Direktive legt die für nachfolgende Codezeilen fest zufügende Kontexts fest, bis Sie von einer anderen Direktive überschrieben werden, oder bis zum Ende der Quelldatei.</span><span class="sxs-lookup"><span data-stu-id="dacd9-138">A directive sets the context(s) it controls for subsequent lines of code, until another directive overrides it, or until the end of the source file.</span></span>

<span data-ttu-id="dacd9-139">Die-Direktiven haben folgende Auswirkung:</span><span class="sxs-lookup"><span data-stu-id="dacd9-139">The effect of the directives is as follows:</span></span>

- <span data-ttu-id="dacd9-140">`#nullable disable`: legt die Anmerkung und den Warnungs Kontexte der Nullwerte auf " *deaktiviert* " fest</span><span class="sxs-lookup"><span data-stu-id="dacd9-140">`#nullable disable`: Sets the nullable annotation and warning contexts to *disabled*</span></span>
- <span data-ttu-id="dacd9-141">`#nullable enable`: legt fest, dass die Anmerkung und der Warnungs Kontexte der Nullwerte *aktiviert* werden</span><span class="sxs-lookup"><span data-stu-id="dacd9-141">`#nullable enable`: Sets the nullable annotation and warning contexts to *enabled*</span></span>
- <span data-ttu-id="dacd9-142">`#nullable restore`: stellt die Anmerkung und den Warnungs Kontexte der NULL-Werte auf Projekteinstellungen wieder her</span><span class="sxs-lookup"><span data-stu-id="dacd9-142">`#nullable restore`: Restores the nullable annotation and warning contexts to project settings</span></span>
- <span data-ttu-id="dacd9-143">`#nullable disable annotations`: legt den Kontext der Anmerkung auf " *deaktiviert* " fest.</span><span class="sxs-lookup"><span data-stu-id="dacd9-143">`#nullable disable annotations`: Sets the nullable annotation context to *disabled*</span></span>
- <span data-ttu-id="dacd9-144">`#nullable enable annotations`: legt den Kontext der Anmerkung, der NULL zulässt, auf *aktiviert* fest</span><span class="sxs-lookup"><span data-stu-id="dacd9-144">`#nullable enable annotations`: Sets the nullable annotation context to *enabled*</span></span>
- <span data-ttu-id="dacd9-145">`#nullable restore annotations`: stellt den Kontext der Anmerkung mit NULL-Werten in den Projekteinstellungen wieder her</span><span class="sxs-lookup"><span data-stu-id="dacd9-145">`#nullable restore annotations`: Restores the nullable annotation context to project settings</span></span>
- <span data-ttu-id="dacd9-146">`#nullable disable warnings`: legt den Warnungs Kontext, der NULL zulässt, auf *deaktiviert* fest</span><span class="sxs-lookup"><span data-stu-id="dacd9-146">`#nullable disable warnings`: Sets the nullable warning context to *disabled*</span></span>
- <span data-ttu-id="dacd9-147">`#nullable enable warnings`: legt den Warnungs Kontext, der NULL zulässt, auf *aktiviert* fest</span><span class="sxs-lookup"><span data-stu-id="dacd9-147">`#nullable enable warnings`: Sets the nullable warning context to *enabled*</span></span>
- <span data-ttu-id="dacd9-148">`#nullable restore warnings`: Wiederherstellen des Warnungs Kontexts, der NULL-Werte zulässt,</span><span class="sxs-lookup"><span data-stu-id="dacd9-148">`#nullable restore warnings`: Restores the nullable warning context to project settings</span></span>

## <a name="nullability-of-types"></a><span data-ttu-id="dacd9-149">NULL-Zulässigkeit von Typen</span><span class="sxs-lookup"><span data-stu-id="dacd9-149">Nullability of types</span></span>

<span data-ttu-id="dacd9-150">Ein bestimmter Typ kann eine von vier NULL-Fähigkeiten aufweisen *:* *nicht*zulässig, NULL *-Werte zulassen* und *unbekannt*.</span><span class="sxs-lookup"><span data-stu-id="dacd9-150">A given type can have one of four nullabilities: *Oblivious*, *nonnullable*, *nullable* and *unknown*.</span></span> 

<span data-ttu-id="dacd9-151">*Nicht auf NULL* festleg Bare und *unbekannte* Typen können Warnungen auslösen, wenn Ihnen ein potenzieller `null` Wert zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="dacd9-151">*Nonnullable* and *unknown* types may cause warnings if a potential `null` value is assigned to them.</span></span> <span data-ttu-id="dacd9-152">Typen, die*null-* Werte zulassen, sind jedoch "NULL *zustellbar* *" und können* `null` Werte ohne Warnungen zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="dacd9-152">*Oblivious* and *nullable* types, however, are "*null-assignable*" and can have `null` values assigned to them without warnings.</span></span> 

<span data-ttu-id="dacd9-153">*Oblivious* *Nicht auf NULL* festleg Bare Typen können ohne Warnungen dereferenziert oder zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="dacd9-153">*Oblivious* and *nonnullable* types can be dereferenced or assigned without warnings.</span></span> <span data-ttu-id="dacd9-154">Werte von *Werte zulässt* -Typen und *unbekannten* Typen sind jedoch "*null-* Ergebnisse" und können Warnungen verursachen, wenn Sie ohne entsprechende NULL-Überprüfung dereferenziert oder zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="dacd9-154">Values of *nullable* and *unknown* types, however, are "*null-yielding*" and may cause warnings when dereferenced or assigned without proper null checking.</span></span> 

<span data-ttu-id="dacd9-155">Der *standardmäßige NULL-Status* eines Typs, der NULL ergibt, ist "vielleicht NULL".</span><span class="sxs-lookup"><span data-stu-id="dacd9-155">The *default null state* of a null-yielding type is "maybe null".</span></span> <span data-ttu-id="dacd9-156">Der standardmäßige NULL-Status eines Typs, der keine NULL-Werte ergibt, ist "not NULL".</span><span class="sxs-lookup"><span data-stu-id="dacd9-156">The default null state of a non-null-yielding type is "not null".</span></span>

<span data-ttu-id="dacd9-157">Die Art des Typs und der Kontext, der NULL-Werte zulässt, in dem er auftritt, um seine NULL-Zulässigkeit zu ermitteln:</span><span class="sxs-lookup"><span data-stu-id="dacd9-157">The kind of type and the nullable annotation context it occurs in determine its nullability:</span></span>

- <span data-ttu-id="dacd9-158">Ein Werttyp, der keine NULL-Werte zulässt, *`S` immer NULL-Werte zulässt*</span><span class="sxs-lookup"><span data-stu-id="dacd9-158">A nonnullable value type `S` is always *nonnullable*</span></span>
- <span data-ttu-id="dacd9-159">Ein Werttyp, der auf NULL festgelegt werden kann `S?` immer *null*</span><span class="sxs-lookup"><span data-stu-id="dacd9-159">A nullable value type `S?` is always *nullable*</span></span>
- <span data-ttu-id="dacd9-160">Ein referenzter Verweistyp `C` in einem *deaktivierten* Anmerkung-Kontext ist nicht *sichtbar* .</span><span class="sxs-lookup"><span data-stu-id="dacd9-160">An unannotated reference type `C` in a *disabled* annotation context is *oblivious*</span></span>
- <span data-ttu-id="dacd9-161">Ein Referenztyp, der in einem *aktivierten* Anmerkung-Kontext `C` ist, lässt keine *NULL-Werte* zu.</span><span class="sxs-lookup"><span data-stu-id="dacd9-161">An unannotated reference type `C` in an *enabled* annotation context is *nonnullable*</span></span>
- <span data-ttu-id="dacd9-162">Ein Verweistyp, der NULL-Werte zulässt `C?` *NULL-Werte* zulässt (es kann jedoch eine Warnung in einem *deaktivierten* Anmerkung-Kontext ausgegeben werden)</span><span class="sxs-lookup"><span data-stu-id="dacd9-162">A nullable reference type `C?` is *nullable* (but a warning may be yielded in a *disabled* annotation context)</span></span>

<span data-ttu-id="dacd9-163">Typparameter berücksichtigen zusätzlich die Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="dacd9-163">Type parameters additionally take their constraints into account:</span></span>

- <span data-ttu-id="dacd9-164">Ein Typparameter `T`, bei dem alle Einschränkungen (sofern vorhanden) entweder NULL-Werte (*Werte zulässt* und *Unknown*) oder die `class?` Einschränkung *unbekannt* sind.</span><span class="sxs-lookup"><span data-stu-id="dacd9-164">A type parameter `T` where all constraints (if any) are either null-yielding types (*nullable* and *unknown*) or the `class?` constraint is *unknown*</span></span>
- <span data-ttu-id="dacd9-165">Ein Typparameter `T`, bei dem mindestens eine Einschränkung entweder nicht zulässig ist oder keine *NULL-Werte* zulässt oder eine der `struct`-oder `class` Einschränkungen ist. *oblivious*</span><span class="sxs-lookup"><span data-stu-id="dacd9-165">A type parameter `T` where at least one constraint is either *oblivious* or *nonnullable* or one of the `struct` or `class` constraints is</span></span>
    - <span data-ttu-id="dacd9-166">nicht *in einem* *deaktivierten* Anmerkung-Kontext.</span><span class="sxs-lookup"><span data-stu-id="dacd9-166">*oblivious* in a *disabled* annotation context</span></span>
    - <span data-ttu-id="dacd9-167">*nicht auf NULL festlegbar* in einem *aktivierten* Anmerkung-Kontext</span><span class="sxs-lookup"><span data-stu-id="dacd9-167">*nonnullable* in an *enabled* annotation context</span></span>
- <span data-ttu-id="dacd9-168">Ein Typparameter, der NULL-Werte zulässt `T?` ist, wenn mindestens eine der Einschränkungen von `T`*nicht* *zulässig oder eine* der `struct`-oder `class` Einschränkungen ist.</span><span class="sxs-lookup"><span data-stu-id="dacd9-168">A nullable type parameter `T?` where at least one of `T`'s constraints is *oblivious* or *nonnullable* or one of the `struct` or `class` constraints, is</span></span>
    - <span data-ttu-id="dacd9-169">*NULL-Werte* in einem *deaktivierten* Anmerkung-Kontext (es wird jedoch eine Warnung ausgegeben)</span><span class="sxs-lookup"><span data-stu-id="dacd9-169">*nullable* in a *disabled* annotation context (but a warning is yielded)</span></span>
    - <span data-ttu-id="dacd9-170">*NULL-Werte* in *aktiviertem* Anmerkung-Kontext</span><span class="sxs-lookup"><span data-stu-id="dacd9-170">*nullable* in an *enabled* annotation context</span></span>

<span data-ttu-id="dacd9-171">Bei einem Typparameter `T`ist `T?` nur zulässig, wenn `T` bekanntermaßen ein Werttyp oder bekannt ist, dass es sich um einen Verweistyp handelt.</span><span class="sxs-lookup"><span data-stu-id="dacd9-171">For a type parameter `T`, `T?` is only allowed if `T` is known to be a value type or known to be a reference type.</span></span>

### <a name="oblivious-vs-nonnullable"></a><span data-ttu-id="dacd9-172">Schädliche und nicht auf NULL festleg Bare Werte</span><span class="sxs-lookup"><span data-stu-id="dacd9-172">Oblivious vs nonnullable</span></span>

<span data-ttu-id="dacd9-173">Ein `type` wird in einem gegebenen Anmerkung-Kontext als auftreten betrachtet, wenn das letzte Token des Typs innerhalb dieses Kontexts liegt.</span><span class="sxs-lookup"><span data-stu-id="dacd9-173">A `type` is deemed to occur in a given annotation context when the last token of the type is within that context.</span></span>

<span data-ttu-id="dacd9-174">Ob ein angegebener Verweistyp, der im Quellcode `C` wird, als nicht zulässig oder nicht NULL-Werte interpretiert wird, hängt vom Anmerkung-Kontext dieses Quellcodes ab.</span><span class="sxs-lookup"><span data-stu-id="dacd9-174">Whether a given reference type `C` in source code is interpreted as oblivious or nonnullable depends on the annotation context of that source code.</span></span> <span data-ttu-id="dacd9-175">Nachdem er festgelegt wurde, wird er als Teil dieses Typs angesehen und mit ihm "bewegt", z. b. während der Ersetzung von generischen Typargumenten.</span><span class="sxs-lookup"><span data-stu-id="dacd9-175">But once established, it is considered part of that type, and "travels with it" e.g. during substitution of generic type arguments.</span></span> <span data-ttu-id="dacd9-176">Es ist so, als ob eine Anmerkung wie `?` für den Typ vorhanden ist, aber unsichtbar ist.</span><span class="sxs-lookup"><span data-stu-id="dacd9-176">It is as if there is an annotation like `?` on the type, but invisible.</span></span>

## <a name="constraints"></a><span data-ttu-id="dacd9-177">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="dacd9-177">Constraints</span></span>

<span data-ttu-id="dacd9-178">Verweis Typen, die NULL-Werte zulassen, können als generische Einschränkungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="dacd9-178">Nullable reference types can be used as generic constraints.</span></span> <span data-ttu-id="dacd9-179">Darüber hinaus ist `object` jetzt als explizite Einschränkung gültig.</span><span class="sxs-lookup"><span data-stu-id="dacd9-179">Furthermore `object` is now valid as an explicit constraint.</span></span> <span data-ttu-id="dacd9-180">Das Fehlen einer Einschränkung entspricht nun einer `object?` Einschränkung (statt `object`), aber (im Gegensatz `object` vor) `object?` nicht als explizite Einschränkung zulässig.</span><span class="sxs-lookup"><span data-stu-id="dacd9-180">Absence of a constraint is now equivalent to an `object?` constraint (instead of `object`), but (unlike `object` before) `object?` is not prohibited as an explicit constraint.</span></span>

<span data-ttu-id="dacd9-181">`class?` ist eine neue Einschränkung, die "möglicherweise Werte zulässt Reference Type" angibt, wohingegen `class` "Verweis Typen, die keine NULL-Werte zulassen" bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="dacd9-181">`class?` is a new constraint denoting "possibly nullable reference type", whereas `class` denotes "nonnullable reference type".</span></span>

<span data-ttu-id="dacd9-182">Die NULL-Zulässigkeit eines Typarguments oder einer Einschränkung wirkt sich nicht darauf aus, ob der Typ die Einschränkung erfüllt, es sei denn, dies ist bereits der Fall, da die Werttypen, die NULL-Werte zulassen, die `struct` Einschränkung nicht erfüllen).</span><span class="sxs-lookup"><span data-stu-id="dacd9-182">The nullability of a type argument or of a constraint does not impact whether the type satisfies the constraint, except where that is already the case today (nullable value types do not satisfy the `struct` constraint).</span></span> <span data-ttu-id="dacd9-183">Wenn das Typargument jedoch die Anforderungen für die NULL-Zulässigkeit der Einschränkung nicht erfüllt, kann eine Warnung ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="dacd9-183">However, if the type argument does not satisfy the nullability requirements of the constraint, a warning may be given.</span></span>

## <a name="null-state-and-null-tracking"></a><span data-ttu-id="dacd9-184">NULL-Status und NULL-Nachverfolgung</span><span class="sxs-lookup"><span data-stu-id="dacd9-184">Null state and null tracking</span></span>

<span data-ttu-id="dacd9-185">Jeder Ausdruck an einem angegebenen Quell Speicherort weist einen *NULL-Status*auf, der angibt, ob der Wert potenziell NULL ergibt.</span><span class="sxs-lookup"><span data-stu-id="dacd9-185">Every expression in a given source location has a *null state*, which indicated whether it is believed to potentially evaluate to null.</span></span> <span data-ttu-id="dacd9-186">Der NULL-Status ist entweder "not NULL" oder "vielleicht NULL".</span><span class="sxs-lookup"><span data-stu-id="dacd9-186">The null state is either "not null" or "maybe null".</span></span> <span data-ttu-id="dacd9-187">Der NULL-Status wird verwendet, um zu bestimmen, ob eine Warnung über Null-unsichere Konvertierungen und Dereferenzierungen ausgegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="dacd9-187">The null state is used to determine whether a warning should be given about null-unsafe conversions and dereferences.</span></span>

### <a name="null-tracking-for-variables"></a><span data-ttu-id="dacd9-188">NULL-Nachverfolgung für Variablen</span><span class="sxs-lookup"><span data-stu-id="dacd9-188">Null tracking for variables</span></span>

<span data-ttu-id="dacd9-189">Für bestimmte Ausdrücke, die Variablen oder Eigenschaften kennzeichnen, wird der NULL-Status zwischen den vorkommen nachverfolgt, basierend auf den zugewiesenen Zuweisungen, den darauf ausgeführten Tests und der Ablauf Steuerung zwischen den vorkommen.</span><span class="sxs-lookup"><span data-stu-id="dacd9-189">For certain expressions denoting variables or properties, the null state is tracked between occurrences, based on assignments to them, tests performed on them and the control flow between them.</span></span> <span data-ttu-id="dacd9-190">Dies ähnelt der Art und Weise, wie eine definitive Zuweisung für Variablen nachverfolgt wird.</span><span class="sxs-lookup"><span data-stu-id="dacd9-190">This is similar to how definite assignment is tracked for variables.</span></span> <span data-ttu-id="dacd9-191">Die nach verfolgten Ausdrücke sind in der folgenden Form:</span><span class="sxs-lookup"><span data-stu-id="dacd9-191">The tracked expressions are the ones of the following form:</span></span>

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

<span data-ttu-id="dacd9-192">, Wobei die Bezeichner Felder oder Eigenschaften kennzeichnen.</span><span class="sxs-lookup"><span data-stu-id="dacd9-192">Where the identifiers denote fields or properties.</span></span>

<span data-ttu-id="dacd9-193">***Beschreiben von NULL-Status Übergängen, die einer bestimmten Zuweisung ähneln***</span><span class="sxs-lookup"><span data-stu-id="dacd9-193">***Describe null state transitions similar to definite assignment***</span></span>

### <a name="null-state-for-expressions"></a><span data-ttu-id="dacd9-194">NULL-Status für Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="dacd9-194">Null state for expressions</span></span>

<span data-ttu-id="dacd9-195">Der NULL-Status eines Ausdrucks wird von seinem Formular und Typ und vom Null-Zustand der an ihm beteiligten Variablen abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="dacd9-195">The null state of an expression is derived from its form and type, and from the null state of variables involved in it.</span></span>

### <a name="literals"></a><span data-ttu-id="dacd9-196">Literale</span><span class="sxs-lookup"><span data-stu-id="dacd9-196">Literals</span></span>

<span data-ttu-id="dacd9-197">Der NULL-Status eines `null` Literals ist "vielleicht NULL".</span><span class="sxs-lookup"><span data-stu-id="dacd9-197">The null state of a `null` literal is "maybe null".</span></span> <span data-ttu-id="dacd9-198">Der NULL-Status eines `default` Literals, das in einen Typ konvertiert wird, dem bekannt ist, dass es sich um einen Werttyp handelt, der nicht NULL sein kann, ist "vielleicht NULL".</span><span class="sxs-lookup"><span data-stu-id="dacd9-198">The null state of a `default` literal that is being converted to a type that is known not to be a nonnullable value type is "maybe null".</span></span> <span data-ttu-id="dacd9-199">Der NULL-Status eines beliebigen anderen Literals ist "not NULL".</span><span class="sxs-lookup"><span data-stu-id="dacd9-199">The null state of any other literal is "not null".</span></span>

### <a name="simple-names"></a><span data-ttu-id="dacd9-200">Einfache Namen</span><span class="sxs-lookup"><span data-stu-id="dacd9-200">Simple names</span></span>

<span data-ttu-id="dacd9-201">Wenn ein `simple_name` nicht als Wert klassifiziert wird, ist sein NULL-Status "not NULL".</span><span class="sxs-lookup"><span data-stu-id="dacd9-201">If a `simple_name` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="dacd9-202">Andernfalls handelt es sich um einen nach verfolgten Ausdruck, und sein NULL-Status ist der überwachte NULL-Status an diesem Quell Speicherort.</span><span class="sxs-lookup"><span data-stu-id="dacd9-202">Otherwise it is a tracked expression, and its null state is its tracked null state at this source location.</span></span>

### <a name="member-access"></a><span data-ttu-id="dacd9-203">Memberzugriff</span><span class="sxs-lookup"><span data-stu-id="dacd9-203">Member access</span></span>

<span data-ttu-id="dacd9-204">Wenn ein `member_access` nicht als Wert klassifiziert wird, ist sein NULL-Status "not NULL".</span><span class="sxs-lookup"><span data-stu-id="dacd9-204">If a `member_access` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="dacd9-205">Andernfalls ist der NULL-Status bei einem überwachten Ausdruck der überwachte NULL-Status an diesem Quell Speicherort.</span><span class="sxs-lookup"><span data-stu-id="dacd9-205">Otherwise, if it is a tracked expression, its null state is its tracked null state at this source location.</span></span> <span data-ttu-id="dacd9-206">Andernfalls ist der NULL-Status der standardmäßige NULL-Status seines Typs.</span><span class="sxs-lookup"><span data-stu-id="dacd9-206">Otherwise its null state is the default null state of its type.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="dacd9-207">Aufrufausdrücke</span><span class="sxs-lookup"><span data-stu-id="dacd9-207">Invocation expressions</span></span>

<span data-ttu-id="dacd9-208">Wenn eine `invocation_expression` einen Member aufruft, der mit einem oder mehreren Attributen für ein spezielles NULL-Verhalten deklariert ist, wird der NULL-Status durch diese Attribute bestimmt.</span><span class="sxs-lookup"><span data-stu-id="dacd9-208">If an `invocation_expression` invokes a member that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="dacd9-209">Andernfalls ist der NULL-Status des Ausdrucks der standardmäßige NULL-Status seines Typs.</span><span class="sxs-lookup"><span data-stu-id="dacd9-209">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="element-access"></a><span data-ttu-id="dacd9-210">Element Zugriff</span><span class="sxs-lookup"><span data-stu-id="dacd9-210">Element access</span></span>

<span data-ttu-id="dacd9-211">Wenn ein `element_access` einen Indexer aufruft, der mit einem oder mehreren Attributen für ein spezielles NULL-Verhalten deklariert ist, wird der NULL-Status durch diese Attribute bestimmt.</span><span class="sxs-lookup"><span data-stu-id="dacd9-211">If an `element_access` invokes an indexer that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="dacd9-212">Andernfalls ist der NULL-Status des Ausdrucks der standardmäßige NULL-Status seines Typs.</span><span class="sxs-lookup"><span data-stu-id="dacd9-212">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="base-access"></a><span data-ttu-id="dacd9-213">Basis Zugriff</span><span class="sxs-lookup"><span data-stu-id="dacd9-213">Base access</span></span>

<span data-ttu-id="dacd9-214">Wenn `B` den Basistyp des einschließenden Typs angibt, hat `base.I` denselben NULL-Status wie `((B)this).I`, und `base[E]` hat denselben NULL-Status wie `((B)this)[E]`.</span><span class="sxs-lookup"><span data-stu-id="dacd9-214">If `B` denotes the base type of the enclosing type, `base.I` has the same null state as `((B)this).I` and `base[E]` has the same null state as `((B)this)[E]`.</span></span>

### <a name="default-expressions"></a><span data-ttu-id="dacd9-215">Standardausdrücke</span><span class="sxs-lookup"><span data-stu-id="dacd9-215">Default expressions</span></span>

<span data-ttu-id="dacd9-216">`default(T)` weist den NULL-Status "nicht-NULL" auf, wenn bekannt ist, dass `T` ein Werttyp ist, der keine NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="dacd9-216">`default(T)` has the null state "non-null" if `T` is known to be a nonnullable value type.</span></span> <span data-ttu-id="dacd9-217">Andernfalls hat Sie den NULL-Status "vielleicht NULL".</span><span class="sxs-lookup"><span data-stu-id="dacd9-217">Otherwise it has the null state "maybe null".</span></span>

### <a name="null-conditional-expressions"></a><span data-ttu-id="dacd9-218">NULL bedingte Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="dacd9-218">Null-conditional expressions</span></span>

<span data-ttu-id="dacd9-219">Ein `null_conditional_expression` hat den NULL-Status "vielleicht NULL".</span><span class="sxs-lookup"><span data-stu-id="dacd9-219">A `null_conditional_expression` has the null state "maybe null".</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="dacd9-220">Umwandlungs Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="dacd9-220">Cast expressions</span></span>

<span data-ttu-id="dacd9-221">Wenn ein Cast Ausdruck `(T)E` eine benutzerdefinierte Konvertierung aufruft, ist der NULL-Status des Ausdrucks der standardmäßige NULL-Status für seinen Typ.</span><span class="sxs-lookup"><span data-stu-id="dacd9-221">If a cast expression `(T)E` invokes a user-defined conversion, then the null state of the expression is the default null state for its type.</span></span> <span data-ttu-id="dacd9-222">Andernfalls ist der NULL-Status "vielleicht NULL", wenn `T` NULL ergibt (*Werte zulässt* oder *Unknown*).</span><span class="sxs-lookup"><span data-stu-id="dacd9-222">Otherwise, if `T` is null-yielding (*nullable* or *unknown*) then the null state is "maybe null".</span></span> <span data-ttu-id="dacd9-223">Andernfalls ist der NULL-Status mit dem NULL-Status `E`identisch.</span><span class="sxs-lookup"><span data-stu-id="dacd9-223">Otherwise the null state is the same as the null state of `E`.</span></span>

### <a name="await-expressions"></a><span data-ttu-id="dacd9-224">Erwartungs Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="dacd9-224">Await expressions</span></span>

<span data-ttu-id="dacd9-225">Der NULL-Status `await E` ist der standardmäßige NULL-Status seines Typs.</span><span class="sxs-lookup"><span data-stu-id="dacd9-225">The null state of `await E` is the default null state of its type.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="dacd9-226">Der `as`-Operator</span><span class="sxs-lookup"><span data-stu-id="dacd9-226">The `as` operator</span></span>

<span data-ttu-id="dacd9-227">Ein `as` Ausdruck hat den NULL-Status "vielleicht NULL".</span><span class="sxs-lookup"><span data-stu-id="dacd9-227">An `as` expression has the null state "maybe null".</span></span>

### <a name="the-null-coalescing-operator"></a><span data-ttu-id="dacd9-228">Der NULL-Sammel Operator</span><span class="sxs-lookup"><span data-stu-id="dacd9-228">The null-coalescing operator</span></span>

<span data-ttu-id="dacd9-229">`E1 ?? E2` hat denselben NULL-Status wie `E2`</span><span class="sxs-lookup"><span data-stu-id="dacd9-229">`E1 ?? E2` has the same null state as `E2`</span></span>

### <a name="the-conditional-operator"></a><span data-ttu-id="dacd9-230">Der bedingte Operator</span><span class="sxs-lookup"><span data-stu-id="dacd9-230">The conditional operator</span></span>

<span data-ttu-id="dacd9-231">Der NULL-Status `E1 ? E2 : E3` ist "not NULL", wenn der NULL-Status von `E2` und `E3` "not NULL" ist.</span><span class="sxs-lookup"><span data-stu-id="dacd9-231">The null state of `E1 ? E2 : E3` is "not null" if the null state of both `E2` and `E3` are "not null".</span></span> <span data-ttu-id="dacd9-232">Andernfalls ist es "vielleicht NULL".</span><span class="sxs-lookup"><span data-stu-id="dacd9-232">Otherwise it is "maybe null".</span></span>

### <a name="query-expressions"></a><span data-ttu-id="dacd9-233">Abfrageausdrücke</span><span class="sxs-lookup"><span data-stu-id="dacd9-233">Query expressions</span></span>

<span data-ttu-id="dacd9-234">Der NULL-Status eines Abfrage Ausdrucks ist der standardmäßige NULL-Status seines Typs.</span><span class="sxs-lookup"><span data-stu-id="dacd9-234">The null state of a query expression is the default null state of its type.</span></span>

### <a name="assignment-operators"></a><span data-ttu-id="dacd9-235">Zuweisungsoperatoren</span><span class="sxs-lookup"><span data-stu-id="dacd9-235">Assignment operators</span></span>

<span data-ttu-id="dacd9-236">`E1 = E2` und `E1 op= E2` haben den gleichen NULL-Status wie `E2`, nachdem alle impliziten Konvertierungen angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="dacd9-236">`E1 = E2` and `E1 op= E2` have the same null state as `E2` after any implicit conversions have been applied.</span></span>

### <a name="unary-and-binary-operators"></a><span data-ttu-id="dacd9-237">Unäre Operatoren und binäre Operatoren</span><span class="sxs-lookup"><span data-stu-id="dacd9-237">Unary and binary operators</span></span>

<span data-ttu-id="dacd9-238">Wenn ein unärer oder binärer Operator einen benutzerdefinierten Operator aufruft, der mit einem oder mehreren Attributen für ein spezielles NULL-Verhalten deklariert ist, wird der NULL-Status durch diese Attribute bestimmt.</span><span class="sxs-lookup"><span data-stu-id="dacd9-238">If a unary or binary operator invokes an user-defined operator that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="dacd9-239">Andernfalls ist der NULL-Status des Ausdrucks der standardmäßige NULL-Status seines Typs.</span><span class="sxs-lookup"><span data-stu-id="dacd9-239">Otherwise the null state of the expression is the default null state of its type.</span></span>

<span data-ttu-id="dacd9-240">***Was ist für binäre `+` über Zeichen folgen und Delegaten etwas Besonderes zu tun?***</span><span class="sxs-lookup"><span data-stu-id="dacd9-240">***Something special to do for binary `+` over strings and delegates?***</span></span>

### <a name="expressions-that-propagate-null-state"></a><span data-ttu-id="dacd9-241">Ausdrücke, die einen NULL-Status</span><span class="sxs-lookup"><span data-stu-id="dacd9-241">Expressions that propagate null state</span></span>

<span data-ttu-id="dacd9-242">`(E)`, `checked(E)` und `unchecked(E)` alle denselben NULL-Status aufweisen wie `E`.</span><span class="sxs-lookup"><span data-stu-id="dacd9-242">`(E)`, `checked(E)` and `unchecked(E)` all have the same null state as `E`.</span></span>

### <a name="expressions-that-are-never-null"></a><span data-ttu-id="dacd9-243">Ausdrücke, die nie NULL sind</span><span class="sxs-lookup"><span data-stu-id="dacd9-243">Expressions that are never null</span></span>

<span data-ttu-id="dacd9-244">Der NULL-Status der folgenden Ausdrucks Formulare ist immer "not NULL":</span><span class="sxs-lookup"><span data-stu-id="dacd9-244">The null state of the following expression forms is always "not null":</span></span>

- <span data-ttu-id="dacd9-245">`this` -Zugriff</span><span class="sxs-lookup"><span data-stu-id="dacd9-245">`this` access</span></span>
- <span data-ttu-id="dacd9-246">interpoliert Zeichen folgen</span><span class="sxs-lookup"><span data-stu-id="dacd9-246">interpolated strings</span></span>
- <span data-ttu-id="dacd9-247">`new` Ausdrücke (Objekt-, Delegat-, anonyme Objekt-und Array Erstellungs Ausdrücke)</span><span class="sxs-lookup"><span data-stu-id="dacd9-247">`new` expressions (object, delegate, anonymous object and array creation expressions)</span></span>
- <span data-ttu-id="dacd9-248">`typeof`-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="dacd9-248">`typeof` expressions</span></span>
- <span data-ttu-id="dacd9-249">`nameof`-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="dacd9-249">`nameof` expressions</span></span>
- <span data-ttu-id="dacd9-250">Anonyme Funktionen (anonyme Methoden und Lambda-Ausdrücke)</span><span class="sxs-lookup"><span data-stu-id="dacd9-250">anonymous functions (anonymous methods and lambda expressions)</span></span>
- <span data-ttu-id="dacd9-251">NULL-Ausdrucks Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="dacd9-251">null-forgiving expressions</span></span>
- <span data-ttu-id="dacd9-252">`is`-Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="dacd9-252">`is` expressions</span></span>

## <a name="type-inference"></a><span data-ttu-id="dacd9-253">Typrückschluss</span><span class="sxs-lookup"><span data-stu-id="dacd9-253">Type inference</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="dacd9-254">Typrückschluss für `var`</span><span class="sxs-lookup"><span data-stu-id="dacd9-254">Type inference for `var`</span></span>

<span data-ttu-id="dacd9-255">Der Typ, der für lokale Variablen abgeleitet wird, die mit `var` deklariert werden, wird durch den NULL-Status des Initialisierungs Ausdrucks informiert.</span><span class="sxs-lookup"><span data-stu-id="dacd9-255">The type inferred for local variables declared with `var` is informed by the null state of the initializing expression.</span></span>

```csharp
var x = E;
```

<span data-ttu-id="dacd9-256">Wenn der Typ des `E` ein Verweistyp ist, der NULL-Werte zulässt `C?` und der NULL-Status von `E` "not NULL" ist, wird der für `x` herleitet Typ `C`.</span><span class="sxs-lookup"><span data-stu-id="dacd9-256">If the type of `E` is a nullable reference type `C?` and the null state of `E` is "not null" then the type inferred for `x` is `C`.</span></span> <span data-ttu-id="dacd9-257">Andernfalls ist der abherleitet Typ der Typ der `E`.</span><span class="sxs-lookup"><span data-stu-id="dacd9-257">Otherwise, the inferred type is the type of `E`.</span></span>

<span data-ttu-id="dacd9-258">Die NULL-Zulässigkeit des Typs, der für `x` abgeleitet wurde, wird wie oben beschrieben basierend auf dem Anmerkung-Kontext der `var`bestimmt, so als ob der Typ explizit an dieser Position angegeben worden wäre.</span><span class="sxs-lookup"><span data-stu-id="dacd9-258">The nullability of the type inferred for `x` is determined as described above, based on the annotation context of the `var`, just as if the type had been given explicitly in that position.</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="dacd9-259">Typrückschluss für `var?`</span><span class="sxs-lookup"><span data-stu-id="dacd9-259">Type inference for `var?`</span></span>

<span data-ttu-id="dacd9-260">Der Typ, der für lokale Variablen abgeleitet wurde, die mit `var?` deklariert werden, ist unabhängig vom NULL-Status des Initialisierungs Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="dacd9-260">The type inferred for local variables declared with `var?` is independent of the null state of the initializing expression.</span></span>

```csharp
var? x = E;
```

<span data-ttu-id="dacd9-261">Wenn der Typ `T` `E` ein auf NULL festleg barer Werttyp oder ein Verweistyp ist, der NULL-Werte zulässt, wird der für `x` herleitet Typ `T`.</span><span class="sxs-lookup"><span data-stu-id="dacd9-261">If the type `T` of `E` is a nullable value type or a nullable reference type then the type inferred for `x` is `T`.</span></span> <span data-ttu-id="dacd9-262">Andernfalls, wenn `T` ein Werttyp ist, der keine NULL-Werte zulässt `S` wird der abherstell Ende Typ `S?`.</span><span class="sxs-lookup"><span data-stu-id="dacd9-262">Otherwise, if `T` is a nonnullable value type `S` the type inferred is `S?`.</span></span> <span data-ttu-id="dacd9-263">Andernfalls, wenn `T` ein Referenztyp ist, der keine NULL-Werte zulässt `C` wird der abherzuende Typ `C?`.</span><span class="sxs-lookup"><span data-stu-id="dacd9-263">Otherwise, if `T` is a nonnullable reference type `C` the type inferred is `C?`.</span></span> <span data-ttu-id="dacd9-264">Andernfalls ist die Deklaration unzulässig.</span><span class="sxs-lookup"><span data-stu-id="dacd9-264">Otherwise, the declaration is illegal.</span></span>

<span data-ttu-id="dacd9-265">Die NULL-Zulässigkeit des Typs, der für `x` abgeleitet ist, ist immer *NULL-Werte*zulässig.</span><span class="sxs-lookup"><span data-stu-id="dacd9-265">The nullability of the type inferred for `x` is always *nullable*.</span></span>

### <a name="generic-type-inference"></a><span data-ttu-id="dacd9-266">Generischer Typrückschluss</span><span class="sxs-lookup"><span data-stu-id="dacd9-266">Generic type inference</span></span>

<span data-ttu-id="dacd9-267">Der generische Typrückschluss wurde verbessert, um zu entscheiden, ob abzurufende Verweis Typen NULL-Werte zulassen sollen oder nicht.</span><span class="sxs-lookup"><span data-stu-id="dacd9-267">Generic type inference is enhanced to help decide whether inferred reference types should be nullable or not.</span></span> <span data-ttu-id="dacd9-268">Dabei handelt es sich um einen optimalen Aufwand, der nicht in und selbst Warnungen liefert, aber möglicherweise zu Warnungen führt, wenn die abzurufenden Typen der ausgewählten Überladung auf die Argumente angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="dacd9-268">This is a best effort, and does not in and of itself yield warnings, but may lead to nullable warnings when the inferred types of the selected overload are applied to the arguments.</span></span>

<span data-ttu-id="dacd9-269">Der Typrückschluss beruht nicht auf dem Anmerkung-Kontext eingehender Typen.</span><span class="sxs-lookup"><span data-stu-id="dacd9-269">The type inference does not rely on the annotation context of incoming types.</span></span> <span data-ttu-id="dacd9-270">Stattdessen wird ein `type` abgeleitet, das seinen eigenen Anmerkung-Kontext von dem Speicherort abruft, in dem er explizit ausgedrückt wurde.</span><span class="sxs-lookup"><span data-stu-id="dacd9-270">Instead a `type` is inferred which acquires its own annotation context from where it "would have been" if it had been expressed explicitly.</span></span> <span data-ttu-id="dacd9-271">Dies unterstreicht die Rolle des Typrückschlusses als praktische Hilfe für das, was Sie selbst geschrieben hätten.</span><span class="sxs-lookup"><span data-stu-id="dacd9-271">This underscores the role of type inference as a convenience for what you could have written yourself.</span></span>

<span data-ttu-id="dacd9-272">Genauer ist, dass der Anmerkung-Kontext für ein abhergegriffes Typargument der Kontext des Tokens ist, dem die `<...>`-Typparameter Liste gefolgt wäre, hätte eine vorhanden sein. d. h. der Name der generischen Methode, die aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="dacd9-272">More precisely, the annotation context for an inferred type argument is the context of the token that would have been followed by the `<...>` type parameter list, had there been one; i.e. the name of the generic method being called.</span></span> <span data-ttu-id="dacd9-273">Für Abfrage Ausdrücke, die in solche Aufrufe übersetzt werden, wird der Kontext aus dem anfänglichen kontextbezogenen Schlüsselwort der Abfrage Klausel entnommen, von der der Aufruf generiert wird.</span><span class="sxs-lookup"><span data-stu-id="dacd9-273">For query expressions that translate to such calls, the context is taken from the initial contextual keyword of the query clause from which the call is generated.</span></span>

### <a name="the-first-phase"></a><span data-ttu-id="dacd9-274">Die erste Phase</span><span class="sxs-lookup"><span data-stu-id="dacd9-274">The first phase</span></span>

<span data-ttu-id="dacd9-275">Verweis Typen, die NULL-Werte zulassen, werden wie unten beschrieben in die Begrenzungen der anfänglichen Ausdrücke übertragen.</span><span class="sxs-lookup"><span data-stu-id="dacd9-275">Nullable reference types flow into the bounds from the initial expressions, as described below.</span></span> <span data-ttu-id="dacd9-276">Außerdem werden zwei neue Arten von Begrenzungen eingeführt, nämlich `null` und `default`.</span><span class="sxs-lookup"><span data-stu-id="dacd9-276">In addition, two new kinds of bounds, namely `null` and `default` are introduced.</span></span> <span data-ttu-id="dacd9-277">Ihr Zweck besteht darin, Vorkommen von `null` oder `default` in den Eingabe Ausdrücken zu durchlaufen, was dazu führen kann, dass ein abgerichteter Typ NULL-Werte zulässt, auch wenn dies andernfalls nicht der Fall wäre.</span><span class="sxs-lookup"><span data-stu-id="dacd9-277">Their purpose is to carry through occurrences of `null` or `default` in the input expressions, which may cause an inferred type to be nullable, even when it otherwise wouldn't.</span></span> <span data-ttu-id="dacd9-278">Dies funktioniert sogar für *Werttypen* , die NULL-Werte zulassen, die verbessert werden, um "nullness" im Rückschluss Prozess zu erfassen.</span><span class="sxs-lookup"><span data-stu-id="dacd9-278">This works even for nullable *value* types, which are enhanced to pick up "nullness" in the inference process.</span></span>

<span data-ttu-id="dacd9-279">Die Festlegung, welche Begrenzungen in der ersten Phase hinzugefügt werden sollen, wird wie folgt erweitert:</span><span class="sxs-lookup"><span data-stu-id="dacd9-279">The determination of what bounds to add in the first phase are enhanced as follows:</span></span>

<span data-ttu-id="dacd9-280">Wenn ein Argument `Ei` einen Verweistyp aufweist, hängt der Typ `U`, der für den Rückschluss verwendet wird, vom NULL-Status `Ei` und vom deklarierten Typ ab:</span><span class="sxs-lookup"><span data-stu-id="dacd9-280">If an argument `Ei` has a reference type, the type `U` used for inference depends on the null state of `Ei` as well as its declared type:</span></span>
- <span data-ttu-id="dacd9-281">Wenn der deklarierte Typ ein Verweistyp ist, der keine NULL-Werte zulässt `U0` oder ein Verweistyp, der NULL-Werte zulässt `U0?`</span><span class="sxs-lookup"><span data-stu-id="dacd9-281">If the declared type is a nonnullable reference type `U0` or a nullable reference type `U0?` then</span></span>
    - <span data-ttu-id="dacd9-282">Wenn der NULL-Status `Ei` "not NULL" ist, wird `U` `U0`</span><span class="sxs-lookup"><span data-stu-id="dacd9-282">if the null state of `Ei` is "not null" then `U` is `U0`</span></span>
    - <span data-ttu-id="dacd9-283">Wenn der NULL-Status `Ei` "vielleicht NULL" ist, wird `U` `U0?`</span><span class="sxs-lookup"><span data-stu-id="dacd9-283">if the null state of `Ei` is "maybe null" then `U` is `U0?`</span></span>
- <span data-ttu-id="dacd9-284">Andernfalls, wenn `Ei` über einen deklarierten Typ verfügt, ist `U` dieser Typ.</span><span class="sxs-lookup"><span data-stu-id="dacd9-284">Otherwise if `Ei` has a declared type, `U` is that type</span></span>
- <span data-ttu-id="dacd9-285">Andernfalls, wenn `Ei` `null`, ist `U` die spezielle Grenze `null`</span><span class="sxs-lookup"><span data-stu-id="dacd9-285">Otherwise if `Ei` is `null` then `U` is the special bound `null`</span></span>
- <span data-ttu-id="dacd9-286">Andernfalls, wenn `Ei` `default`, ist `U` die spezielle Grenze `default`</span><span class="sxs-lookup"><span data-stu-id="dacd9-286">Otherwise if `Ei` is `default` then `U` is the special bound `default`</span></span>
- <span data-ttu-id="dacd9-287">Andernfalls wird kein Rückschluss erstellt.</span><span class="sxs-lookup"><span data-stu-id="dacd9-287">Otherwise no inference is made.</span></span>

### <a name="exact-upper-bound-and-lower-bound-inferences"></a><span data-ttu-id="dacd9-288">Exakte, obere und untere gebundene Rückschlüsse</span><span class="sxs-lookup"><span data-stu-id="dacd9-288">Exact, upper-bound and lower-bound inferences</span></span>

<span data-ttu-id="dacd9-289">Bei Rückschlüsse *vom* Typ `U` *zum* Typ `V`wird in den folgenden Klauseln `V0` anstelle von `V` verwendet, wenn `V` ein Referenztyp ist, der NULL-Werte zulässt `V0?`.</span><span class="sxs-lookup"><span data-stu-id="dacd9-289">In inferences *from* the type `U` *to* the type `V`, if `V` is a nullable reference type `V0?`, then `V0` is used instead of `V` in the following clauses.</span></span>
- <span data-ttu-id="dacd9-290">Wenn `V` eine der unfixed-Typvariablen ist, wird `U` als exakte, obere oder untere Grenze wie zuvor hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="dacd9-290">If `V` is one of the unfixed type variables, `U` is added as an exact, upper or lower bound as before</span></span>
- <span data-ttu-id="dacd9-291">Andernfalls, wenn `U` `null` oder `default`ist, wird kein Rückschluss erstellt.</span><span class="sxs-lookup"><span data-stu-id="dacd9-291">Otherwise, if `U` is `null` or `default`, no inference is made</span></span>
- <span data-ttu-id="dacd9-292">Andernfalls, wenn `U` ein Verweistyp, der NULL-Werte zulässt `U0?`ist, wird `U0` anstelle der `U` in den nachfolgenden Klauseln verwendet.</span><span class="sxs-lookup"><span data-stu-id="dacd9-292">Otherwise, if `U` is a nullable reference type `U0?`, then `U0` is used instead of `U` in the subsequent clauses.</span></span>

<span data-ttu-id="dacd9-293">Der Grund dafür ist, dass die NULL-Zulässigkeit, die sich direkt auf eine der Variablen ohne fester Größe bezieht, in ihren Grenzen bewahrt wird.</span><span class="sxs-lookup"><span data-stu-id="dacd9-293">The essence is that nullability that pertains directly to one of the unfixed type variables is preserved into its bounds.</span></span> <span data-ttu-id="dacd9-294">Für die Rückschlüsse, die weiter unten in den Quell-und Zieltyp rekurdieren, wird die NULL-Zulässigkeit ignoriert.</span><span class="sxs-lookup"><span data-stu-id="dacd9-294">For the inferences that recurse further into the source and target types, on the other hand, nullability is ignored.</span></span> <span data-ttu-id="dacd9-295">Dies ist möglicherweise nicht möglich, aber wenn dies nicht der Fall ist, wird später eine Warnung ausgegeben, wenn die Überladung ausgewählt und angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="dacd9-295">It may or may not match, but if it doesn't, a warning will be issued later if the overload is chosen and applied.</span></span>

### <a name="fixing"></a><span data-ttu-id="dacd9-296">Festnahme</span><span class="sxs-lookup"><span data-stu-id="dacd9-296">Fixing</span></span>

<span data-ttu-id="dacd9-297">Die Spezifikation führt derzeit keine gute Aufgabe aus, zu beschreiben, was geschieht, wenn mehrere Grenzen in einander umsetzbar sind, aber unterschiedlich sind.</span><span class="sxs-lookup"><span data-stu-id="dacd9-297">The spec currently does not do a good job of describing what happens when multiple bounds are identity convertible to each other, but are different.</span></span> <span data-ttu-id="dacd9-298">Dies kann zwischen `object` und `dynamic`auftreten, zwischen Tupeltypen, die sich nur in Elementnamen unterscheiden, zwischen Typen, die hiervon erstellt werden, und jetzt auch zwischen `C` und `C?` für Verweis Typen.</span><span class="sxs-lookup"><span data-stu-id="dacd9-298">This may happen between `object` and `dynamic`, between tuple types that differ only in element names, between types constructed thereof and now also between `C` and `C?` for reference types.</span></span>

<span data-ttu-id="dacd9-299">Außerdem müssen wir "nullness" aus den Eingabe Ausdrücken an den Ergebnistyp weitergeben.</span><span class="sxs-lookup"><span data-stu-id="dacd9-299">In addition we need to propagate "nullness" from the input expressions to the result type.</span></span> 

<span data-ttu-id="dacd9-300">Um diese Schritte durchzuführen, fügen wir weitere Phasen zur Korrektur hinzu. Dies ist jetzt der:</span><span class="sxs-lookup"><span data-stu-id="dacd9-300">To handle these we add more phases to fixing, which is now:</span></span>

1. <span data-ttu-id="dacd9-301">Erfassen aller Typen in allen Begrenzungen als Kandidaten, Entfernen von `?` aus allen, die auf NULL festleg Bare Verweis Typen sind</span><span class="sxs-lookup"><span data-stu-id="dacd9-301">Gather all the types in all the bounds as candidates, removing `?` from all that are nullable reference types</span></span>
2. <span data-ttu-id="dacd9-302">Entfernen Sie Kandidaten auf der Grundlage von exakten, unteren und oberen Begrenzungen (behalten Sie `null` und `default` Grenzen bei)</span><span class="sxs-lookup"><span data-stu-id="dacd9-302">Eliminate candidates based on requirements of exact, lower and upper bounds (keeping `null` and `default` bounds)</span></span>
3. <span data-ttu-id="dacd9-303">Entfernen Sie Kandidaten, die keine implizite Konvertierung in alle anderen Kandidaten haben.</span><span class="sxs-lookup"><span data-stu-id="dacd9-303">Eliminate candidates that do not have an implicit conversion to all the other candidates</span></span>
4. <span data-ttu-id="dacd9-304">Wenn die verbleibenden Kandidaten nicht alle über Identitäts Konvertierungen miteinander verfügen, schlägt der Typrückschluss fehl.</span><span class="sxs-lookup"><span data-stu-id="dacd9-304">If the remaining candidates do not all have identity conversions to one another, then type inference fails</span></span>
5. <span data-ttu-id="dacd9-305">*Führen* Sie die übrigen Kandidaten wie unten beschrieben zusammen.</span><span class="sxs-lookup"><span data-stu-id="dacd9-305">*Merge* the remaining candidates as described below</span></span>
6. <span data-ttu-id="dacd9-306">Wenn der resultierende Kandidat ein Referenztyp oder ein nicht auf NULL festleg barer Werttyp ist und *alle* exakten Begrenzungen oder *eine* der unteren Begrenzungen auf NULL festleg Bare Werttypen, auf NULL festleg Bare Verweis Typen, `null` oder `default`, werden dem resultierenden Kandidaten `?` hinzugefügt, sodass es sich um einen Werttyp oder Verweistyp handelt, der NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="dacd9-306">If the resulting candidate is a reference type or a nonnullable value type and *all* of the exact bounds or *any* of the lower bounds are nullable value types, nullable reference types, `null` or `default`, then `?` is added to the resulting candidate, making it a nullable value type or reference type.</span></span>

<span data-ttu-id="dacd9-307">Die zusammen *Führung* wird zwischen zwei Kandidaten Typen beschrieben.</span><span class="sxs-lookup"><span data-stu-id="dacd9-307">*Merging* is described between two candidate types.</span></span> <span data-ttu-id="dacd9-308">Es ist transitiv und kommutativ, sodass die Kandidaten in beliebiger Reihenfolge mit dem gleichen ultimativen Ergebnis zusammengeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="dacd9-308">It is transitive and commutative, so the candidates can be merged in any order with the same ultimate result.</span></span> <span data-ttu-id="dacd9-309">Er ist nicht definiert, wenn die beiden Kandidaten Typen nicht in einander konvertierbar sind.</span><span class="sxs-lookup"><span data-stu-id="dacd9-309">It is undefined if the two candidate types are not identity convertible to each other.</span></span>

<span data-ttu-id="dacd9-310">Die *Merge* -Funktion nimmt zwei Kandidaten Typen und eine Richtung ( *+* oder *-* ) an:</span><span class="sxs-lookup"><span data-stu-id="dacd9-310">The *Merge* function takes two candidate types and a direction (*+* or *-*):</span></span>

- <span data-ttu-id="dacd9-311">*Merge*(`T`, `T`, *d*) = t</span><span class="sxs-lookup"><span data-stu-id="dacd9-311">*Merge*(`T`, `T`, *d*) = T</span></span>
- <span data-ttu-id="dacd9-312">*Merge*(`S`, `T?` *+* ) = *Merge*(`S?`, `T`, *+* ) = *Merge*(`S`, `T`, *+* )`?`</span><span class="sxs-lookup"><span data-stu-id="dacd9-312">*Merge*(`S`, `T?`, *+*) = *Merge*(`S?`, `T`, *+*) = *Merge*(`S`, `T`, *+*)`?`</span></span>
- <span data-ttu-id="dacd9-313">*Merge*(`S`, `T?`, *-* ) = *Merge*(`S?`, `T`, *-* ) = *Merge*(`S`, `T`, *-* )</span><span class="sxs-lookup"><span data-stu-id="dacd9-313">*Merge*(`S`, `T?`, *-*) = *Merge*(`S?`, `T`, *-*) = *Merge*(`S`, `T`, *-*)</span></span>
- <span data-ttu-id="dacd9-314">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>` *+* ) = `C<`*Merge*(`S1`, `T1`, *D1*)`,...,`*Merge*(`Sn`, `Tn`, *DN*)`>`, *wobei*</span><span class="sxs-lookup"><span data-stu-id="dacd9-314">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="dacd9-315">`di` =  *+* , wenn der `i`"th Type-Parameter von `C<...>` kovariant ist.</span><span class="sxs-lookup"><span data-stu-id="dacd9-315">`di` = *+* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="dacd9-316">`di` =  *-* , wenn der `i`"th Type-Parameter von `C<...>` Kontra oder invariante ist.</span><span class="sxs-lookup"><span data-stu-id="dacd9-316">`di` = *-* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="dacd9-317">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>` *-* ) = `C<`*Merge*(`S1`, `T1`, *D1*)`,...,`*Merge*(`Sn`, `Tn`, *DN*)`>`, *wobei*</span><span class="sxs-lookup"><span data-stu-id="dacd9-317">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="dacd9-318">`di` =  *-* , wenn der `i`"th Type-Parameter von `C<...>` kovariant ist.</span><span class="sxs-lookup"><span data-stu-id="dacd9-318">`di` = *-* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="dacd9-319">`di` =  *+* , wenn der `i`"th Type-Parameter von `C<...>` Kontra oder invariante ist.</span><span class="sxs-lookup"><span data-stu-id="dacd9-319">`di` = *+* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="dacd9-320">*Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*Merge*(`S1`, `T1`, *d*)`n1,...,`*Merge*(`Sn`, `Tn`, *d*) `nn)`, *wobei*</span><span class="sxs-lookup"><span data-stu-id="dacd9-320">*Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*Merge*(`S1`, `T1`, *d*)`n1,...,`*Merge*(`Sn`, `Tn`, *d*) `nn)`, *where*</span></span>
    - <span data-ttu-id="dacd9-321">`ni` fehlt, wenn sich `si` und `ti` unterscheiden oder wenn beide nicht vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="dacd9-321">`ni` is absent if `si` and `ti` differ, or if both are absent</span></span>
    - <span data-ttu-id="dacd9-322">`ni` ist `si`, wenn `si` und `ti` identisch sind.</span><span class="sxs-lookup"><span data-stu-id="dacd9-322">`ni` is `si` if `si` and `ti` are the same</span></span>
- <span data-ttu-id="dacd9-323">*Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`</span><span class="sxs-lookup"><span data-stu-id="dacd9-323">*Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`</span></span>

## <a name="warnings"></a><span data-ttu-id="dacd9-324">Warnungen</span><span class="sxs-lookup"><span data-stu-id="dacd9-324">Warnings</span></span>

### <a name="potential-null-assignment"></a><span data-ttu-id="dacd9-325">Mögliche NULL-Zuweisung</span><span class="sxs-lookup"><span data-stu-id="dacd9-325">Potential null assignment</span></span>

### <a name="potential-null-dereference"></a><span data-ttu-id="dacd9-326">Mögliche Null-Dereferenzierung</span><span class="sxs-lookup"><span data-stu-id="dacd9-326">Potential null dereference</span></span>

### <a name="constraint-nullability-mismatch"></a><span data-ttu-id="dacd9-327">Einschränkung der NULL-Zulässigkeit der Einschränkung</span><span class="sxs-lookup"><span data-stu-id="dacd9-327">Constraint nullability mismatch</span></span>

### <a name="nullable-types-in-disabled-annotation-context"></a><span data-ttu-id="dacd9-328">Typen, die NULL-Werte zulassen</span><span class="sxs-lookup"><span data-stu-id="dacd9-328">Nullable types in disabled annotation context</span></span>

## <a name="attributes-for-special-null-behavior"></a><span data-ttu-id="dacd9-329">Attribute für das besondere NULL-Verhalten</span><span class="sxs-lookup"><span data-stu-id="dacd9-329">Attributes for special null behavior</span></span>


