---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483541"
---
# <a name="simplified-null-argument-checking"></a><span data-ttu-id="8e976-101">Vereinfachte NULL-Argument Überprüfung</span><span class="sxs-lookup"><span data-stu-id="8e976-101">Simplified Null Argument Checking</span></span>

## <a name="summary"></a><span data-ttu-id="8e976-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="8e976-102">Summary</span></span>
<span data-ttu-id="8e976-103">Dieser Vorschlag stellt eine vereinfachte Syntax für das Validieren von Methoden Argumenten dar, die nicht `null` sind und `ArgumentNullException` entsprechend auslöst.</span><span class="sxs-lookup"><span data-stu-id="8e976-103">This proposal provides a simplified syntax for validating method arguments are not `null` and throwing `ArgumentNullException` appropriately.</span></span>

## <a name="motivation"></a><span data-ttu-id="8e976-104">Motivation</span><span class="sxs-lookup"><span data-stu-id="8e976-104">Motivation</span></span>
<span data-ttu-id="8e976-105">Die Arbeit beim Entwerfen von Verweis Typen, die NULL-Werte zulassen, hat dazu geführt, dass wir den Code untersuchen, der für die `null`</span><span class="sxs-lookup"><span data-stu-id="8e976-105">The work on designing nullable reference types has caused us to examine the code necessary for `null` argument validation.</span></span> <span data-ttu-id="8e976-106">Da sich NRT nicht auf die Codeausführung auswirkt, müssen Entwickler weiterhin `if (arg is null) throw`-Kessel Bausteine hinzufügen, auch in Projekten, die vollständig `null` bereinigt sind.</span><span class="sxs-lookup"><span data-stu-id="8e976-106">Given that NRT doesn't affect code execution developers still must add `if (arg is null) throw` boiler plate code even in projects which are fully `null` clean.</span></span> <span data-ttu-id="8e976-107">Dadurch haben wir die Absicht, eine minimale Syntax für die Argument `null` Validierung in der Sprache zu untersuchen.</span><span class="sxs-lookup"><span data-stu-id="8e976-107">This gave us the desire to explore a minimal syntax for argument `null` validation in the language.</span></span> 

<span data-ttu-id="8e976-108">Obwohl diese `null` Parameter-Validierungs Syntax häufig mit NRT gekoppelt werden soll, ist der Vorschlag vollständig unabhängig davon.</span><span class="sxs-lookup"><span data-stu-id="8e976-108">While this `null` parameter validation syntax is expected to pair frequently with NRT the proposal is fully independent of it.</span></span> <span data-ttu-id="8e976-109">Die Syntax kann unabhängig von `#nullable` Direktiven verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8e976-109">The syntax can be used independent of `#nullable` directives.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="8e976-110">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="8e976-110">Detailed Design</span></span> 

### <a name="null-validation-parameter-syntax"></a><span data-ttu-id="8e976-111">Syntax der NULL-Validierungs Parameter</span><span class="sxs-lookup"><span data-stu-id="8e976-111">Null validation parameter syntax</span></span>
<span data-ttu-id="8e976-112">Der Bang-Operator (`!`) kann nach einem Parameternamen in einer Parameterliste positioniert werden. Dies bewirkt, C# dass der Compiler Standard `null` Überprüfungs Code für diesen Parameter ausgibt.</span><span class="sxs-lookup"><span data-stu-id="8e976-112">The bang operator, `!`, can be positioned after a parameter name in a parameter list and this will cause the C# compiler to emit standard `null` checking code for that parameter.</span></span> <span data-ttu-id="8e976-113">Dies wird als `null` Validierungs Parameter Syntax bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="8e976-113">This is referred to as `null` validation parameter syntax.</span></span> <span data-ttu-id="8e976-114">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e976-114">For example:</span></span>

``` csharp
void M(string name!) {
    ...
}
```

<span data-ttu-id="8e976-115">Wird übersetzt in:</span><span class="sxs-lookup"><span data-stu-id="8e976-115">Will be translated into:</span></span>

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

<span data-ttu-id="8e976-116">Die generierte `null` Prüfung erfolgt vor einem von Entwicklern erstellten Code in der-Methode.</span><span class="sxs-lookup"><span data-stu-id="8e976-116">The generated `null` check will occur before any developer authored code in the method.</span></span> <span data-ttu-id="8e976-117">Wenn mehrere Parameter den `!` Operator enthalten, werden die Überprüfungen in derselben Reihenfolge ausgeführt, in der die Parameter deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="8e976-117">When multiple parameters contain the `!` operator then the checks will occur in the same order as the parameters are declared.</span></span>

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

<span data-ttu-id="8e976-118">Die Überprüfung bezieht sich speziell auf Verweis Gleichheit auf `null`, ruft keine `==` oder benutzerdefinierten Operatoren auf.</span><span class="sxs-lookup"><span data-stu-id="8e976-118">The check will be specifically for reference equality to `null`, it does not invoke `==` or any user defined operators.</span></span> <span data-ttu-id="8e976-119">Dies bedeutet auch, dass der `!` Operator nur zu Parametern hinzugefügt werden kann, deren Typ auf Gleichheit mit `null`getestet werden kann.</span><span class="sxs-lookup"><span data-stu-id="8e976-119">This also means the `!` operator can only be added to parameters whose type can be tested for equality against `null`.</span></span> <span data-ttu-id="8e976-120">Dies bedeutet, dass Sie nicht für einen Parameter verwendet werden kann, dessen Typ bekanntermaßen ein Werttyp ist.</span><span class="sxs-lookup"><span data-stu-id="8e976-120">This means it can't be used on a parameter whose type is known to be a value type.</span></span>

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

<span data-ttu-id="8e976-121">Im Fall eines Konstruktors erfolgt die `null` Validierung vor jedem anderen Code im Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="8e976-121">In the case of a constructor the `null` validation will occur before any other code in the constructor.</span></span> <span data-ttu-id="8e976-122">Dies umfasst Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8e976-122">That includes:</span></span> 

- <span data-ttu-id="8e976-123">Verkettung von anderen Konstruktoren mit `this` oder `base`</span><span class="sxs-lookup"><span data-stu-id="8e976-123">Chaining to other constructors with `this` or `base`</span></span> 
- <span data-ttu-id="8e976-124">Feldinitialisierer, die implizit im Konstruktor auftreten</span><span class="sxs-lookup"><span data-stu-id="8e976-124">Field initializers which implicitly occur in the constructor</span></span>

<span data-ttu-id="8e976-125">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8e976-125">For example:</span></span>

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

<span data-ttu-id="8e976-126">Wird ungefähr in Folgendes übersetzt:</span><span class="sxs-lookup"><span data-stu-id="8e976-126">Will be roughly translated into the following:</span></span>

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

<span data-ttu-id="8e976-127">Hinweis: Dies ist kein gültiger C# Code, sondern lediglich eine Näherung der Implementierung der Implementierung.</span><span class="sxs-lookup"><span data-stu-id="8e976-127">Note: this is not legal C# code but instead just an approximation of what the implementation does.</span></span> 

<span data-ttu-id="8e976-128">Die Syntax der `null` Validierungs Parameter ist auch in Lambda-Parameterlisten gültig.</span><span class="sxs-lookup"><span data-stu-id="8e976-128">The `null` validation parameter syntax will also be valid on lambda parameter lists.</span></span> <span data-ttu-id="8e976-129">Dies ist auch in der einzelnen Parameter Syntax gültig, bei der keine Parameter fehlen.</span><span class="sxs-lookup"><span data-stu-id="8e976-129">This is valid even in the single parameter syntax that lacks parens.</span></span>

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

<span data-ttu-id="8e976-130">Die Syntax ist auch für Parameter für Iteratormethoden gültig.</span><span class="sxs-lookup"><span data-stu-id="8e976-130">The syntax is also valid on parameters to iterator methods.</span></span> <span data-ttu-id="8e976-131">Im Gegensatz zu anderem Code im Iterator wird `null` Validierung ausgeführt, wenn die Iteratormethode aufgerufen wird, nicht, wenn der zugrunde liegende Enumerator durchlaufen wird.</span><span class="sxs-lookup"><span data-stu-id="8e976-131">Unlike other code in the iterator the `null` validation will occur when the iterator method is invoked, not when the underlying enumerator is walked.</span></span> <span data-ttu-id="8e976-132">Dies gilt für herkömmliche oder `async` Iteratoren.</span><span class="sxs-lookup"><span data-stu-id="8e976-132">This is true for traditional or `async` iterators.</span></span>

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

<span data-ttu-id="8e976-133">Der `!`-Operator kann nur für Parameterlisten verwendet werden, die über einen zugeordneten Methoden Text verfügen.</span><span class="sxs-lookup"><span data-stu-id="8e976-133">The `!` operator can only be used for parameter lists which have an associated method body.</span></span> <span data-ttu-id="8e976-134">Dies bedeutet, dass Sie nicht in einer `abstract` Methode, `interface``delegate` oder `partial` Methoden Definition verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="8e976-134">This means it cannot be used in an `abstract` method, `interface`, `delegate` or `partial` method definition.</span></span>

### <a name="extending-is-null"></a><span data-ttu-id="8e976-135">Erweiterung ist NULL</span><span class="sxs-lookup"><span data-stu-id="8e976-135">Extending is null</span></span>
<span data-ttu-id="8e976-136">Die Typen, für die der Ausdruck `is null` gültig ist, werden so erweitert, dass Sie nicht eingeschränkte Typparameter einschließen.</span><span class="sxs-lookup"><span data-stu-id="8e976-136">The types for which the expression `is null` is valid will be extended to include unconstrained type parameters.</span></span> <span data-ttu-id="8e976-137">Dadurch wird es ermöglicht, die `null` für alle Typen zu überprüfen, die eine `null` Prüfung gültig ist.</span><span class="sxs-lookup"><span data-stu-id="8e976-137">This will allow it to fill the intent of checking for `null` on all types which a `null` check is valid.</span></span> <span data-ttu-id="8e976-138">Dabei handelt es sich insbesondere um Typen, bei denen es sich nicht definitiv um Werttypen handelt.</span><span class="sxs-lookup"><span data-stu-id="8e976-138">Specifically that is types which are not definitely known to be value types.</span></span> <span data-ttu-id="8e976-139">Beispielsweise können Typparameter, die auf `struct` beschränkt sind, nicht mit dieser Syntax verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8e976-139">For example Type parameters which are constrained to `struct` cannot be used with this syntax.</span></span>

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

<span data-ttu-id="8e976-140">Das Verhalten von `is null` für einen Typparameter ist derselbe wie `== null` heute.</span><span class="sxs-lookup"><span data-stu-id="8e976-140">The behavior of `is null` on a type parameter will be the same as `== null` today.</span></span> <span data-ttu-id="8e976-141">In Fällen, in denen der Typparameter als Werttyp instanziiert wird, wird der Code als `false`ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="8e976-141">In the cases where the type parameter is instantiated as a value type the code will be evaluated as `false`.</span></span> <span data-ttu-id="8e976-142">In Fällen, in denen es sich um einen Verweistyp handelt, führt der Code eine ordnungsgemäße `is null` Überprüfung durch.</span><span class="sxs-lookup"><span data-stu-id="8e976-142">For cases where it is a reference type the code will do a proper `is null` check.</span></span>

### <a name="intersection-with-nullable-reference-types"></a><span data-ttu-id="8e976-143">Schnittmenge mit Verweis Typen mit Nullwert</span><span class="sxs-lookup"><span data-stu-id="8e976-143">Intersection with Nullable Reference Types</span></span>
<span data-ttu-id="8e976-144">Jeder Parameter, der einen `!`-Operator hat, der auf seinen Namen angewendet wird, beginnt mit dem Zustand, der NULL-Werte zulässt, nicht `null`.</span><span class="sxs-lookup"><span data-stu-id="8e976-144">Any parameter which has a `!` operator applied to it's name will start with the nullable state being not `null`.</span></span> <span data-ttu-id="8e976-145">Dies gilt auch, wenn der Typ des Parameters selbst potenziell `null`ist.</span><span class="sxs-lookup"><span data-stu-id="8e976-145">This is true even if the type of the parameter itself is potentially `null`.</span></span> <span data-ttu-id="8e976-146">Dies kann bei einem explizit auf NULL festleg baren Typ vorkommen, z. b. `string?`, oder mit einem uneingeschränkten Typparameter.</span><span class="sxs-lookup"><span data-stu-id="8e976-146">That can occur with an explicitly nullable type, such as say `string?`, or with an unconstrained type parameter.</span></span> 

<span data-ttu-id="8e976-147">Wenn eine `!` Syntax für Parameter mit einem explizit auf NULL festleg baren Typ für den Parameter kombiniert wird, wird vom Compiler eine Warnung ausgegeben:</span><span class="sxs-lookup"><span data-stu-id="8e976-147">When a `!` syntax on parameters is combined with an explicitly nullable type on the parameter then a warning will be issued by the compiler:</span></span>

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a><span data-ttu-id="8e976-148">Offene Probleme</span><span class="sxs-lookup"><span data-stu-id="8e976-148">Open Issues</span></span>
<span data-ttu-id="8e976-149">Keine</span><span class="sxs-lookup"><span data-stu-id="8e976-149">None</span></span>

## <a name="considerations"></a><span data-ttu-id="8e976-150">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="8e976-150">Considerations</span></span>

### <a name="constructors"></a><span data-ttu-id="8e976-151">Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="8e976-151">Constructors</span></span>
<span data-ttu-id="8e976-152">Die Codegenerierung für Konstruktoren bedeutet, dass es ein kleines, aber beobachtbares Behavior Change gibt, wenn Sie die Standard `null` Validierung heute und die Syntax für `null` Validierungs Parameter (`!`) verschieben.</span><span class="sxs-lookup"><span data-stu-id="8e976-152">The code generation for constructors means there is a small, but observable, behavior change when moving from standard `null` validation today and the `null` validation parameter syntax (`!`).</span></span> <span data-ttu-id="8e976-153">Die `null` Prüfung in der Standard Validierung erfolgt nach den feldinitialisierern und allen `base`-oder `this` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="8e976-153">The `null` check in standard validation occurs after both field initializers and any `base` or `this` calls.</span></span> <span data-ttu-id="8e976-154">Dies bedeutet, dass ein Entwickler nicht notwendigerweise 100% der `null` Validierung zur neuen Syntax migrieren kann.</span><span class="sxs-lookup"><span data-stu-id="8e976-154">This means a developer can't necessarily migrate 100% of their `null` validation to the new syntax.</span></span> <span data-ttu-id="8e976-155">Für Konstruktoren ist mindestens eine Prüfung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="8e976-155">Constructors at least require some inspection.</span></span>

<span data-ttu-id="8e976-156">Nach der Erörterung wurde beschlossen, dass es sehr unwahrscheinlich ist, dass dies zu erheblichen Akzeptanz Problemen führt.</span><span class="sxs-lookup"><span data-stu-id="8e976-156">After discussion though it was decided that this is very unlikely to cause any significant adoption issues.</span></span> <span data-ttu-id="8e976-157">Es ist logischer, dass die `null` Prüfung ausgeführt wird, bevor eine Logik im Konstruktor funktioniert.</span><span class="sxs-lookup"><span data-stu-id="8e976-157">It's more logical that the `null` check run before any logic in the constructor does.</span></span> <span data-ttu-id="8e976-158">Kann erneut auftreten, wenn signifikante kompatibleme Probleme erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="8e976-158">Can revisit if significant compat issues are discovered.</span></span>

### <a name="warning-when-mixing--and-"></a><span data-ttu-id="8e976-159">Warnung beim Mischen?</span><span class="sxs-lookup"><span data-stu-id="8e976-159">Warning when mixing ?</span></span> <span data-ttu-id="8e976-160">immer!</span><span class="sxs-lookup"><span data-stu-id="8e976-160">and !</span></span>
<span data-ttu-id="8e976-161">Es wurde ausführlich diskutiert, ob eine Warnung ausgegeben werden soll, wenn die `!`-Syntax auf einen Parameter angewendet wird, der explizit in einen Werte zulässt-Typ eingegeben wird.</span><span class="sxs-lookup"><span data-stu-id="8e976-161">There was a lengthy discussion on whether or not a warning should be issued when the `!` syntax is applied to a parameter which is explicitly typed to a nullable type.</span></span> <span data-ttu-id="8e976-162">Auf der Oberfläche scheint es eine unsinnige Deklaration durch den Entwickler zu sein, aber es gibt Fälle, in denen Typhierarchien Entwicklern eine solche Situation erzwingen könnten.</span><span class="sxs-lookup"><span data-stu-id="8e976-162">On the surface it seems like a nonsensical declaration by the developer but there are cases where type hierarchies could force developers into such a situation.</span></span> 

<span data-ttu-id="8e976-163">Beachten Sie die folgende Klassenhierarchie für eine Reihe von Assemblys (vorausgesetzt, alle werden mit aktivierter `null` Prüfung kompiliert):</span><span class="sxs-lookup"><span data-stu-id="8e976-163">Consider the following class hierarchy across a series of assemblies (assuming all are compiled with `null` checking enabled):</span></span>

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

<span data-ttu-id="8e976-164">Hier hat der Autor von `C3` beschlossen, dem Parameter `o``null` Validierung hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="8e976-164">Here the author of `C3` decided to add `null` validation to the parameter `o`.</span></span> <span data-ttu-id="8e976-165">Dies ist vollständig mit der Verwendungsweise des Features in Einklang.</span><span class="sxs-lookup"><span data-stu-id="8e976-165">This is completely in line with how the feature is intended to be used.</span></span>

<span data-ttu-id="8e976-166">Stellen Sie sich jetzt vor, zu einem späteren Zeitpunkt beschließt der Autor von Assembly2, die folgende außer Kraft Setzung hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="8e976-166">Now imagine at a later date the author of Assembly2 decides to add the following override:</span></span>

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

<span data-ttu-id="8e976-167">Dies ist für Verweis Typen zulässig, die auf NULL festgelegt werden können, da es zulässig ist, den Vertrag für Eingabe Positionen flexibler zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="8e976-167">This is allowed by nullable reference types as it's legal to make the contract more flexible for input positions.</span></span> <span data-ttu-id="8e976-168">Die NRT-Funktion im allgemeinen ermöglicht angemessene Co/kontra Varianz in der NULL-Zulässigkeit von Parametern/Rückgabe.</span><span class="sxs-lookup"><span data-stu-id="8e976-168">The NRT feature in general allows for reasonable co/contravariance on parameter / return nullability.</span></span> <span data-ttu-id="8e976-169">Die Sprache führt jedoch die Co/kontra Varianz Überprüfung basierend auf der spezifischsten außer Kraft Setzung und nicht mit der ursprünglichen Deklaration durch.</span><span class="sxs-lookup"><span data-stu-id="8e976-169">However the language does the co/contravariance checking based on the most specific override, not the original declaration.</span></span> <span data-ttu-id="8e976-170">Dies bedeutet, dass der Autor von "Assembly3" eine Warnung über den Typ der `o`, die nicht übereinstimmt, erhält und die Signatur wie folgt ändern muss, um dies auszuschließen:</span><span class="sxs-lookup"><span data-stu-id="8e976-170">This means the author of Assembly3 will get a warning about the type of `o` not matching and will need to change the signature to the following to eliminate it:</span></span> 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

<span data-ttu-id="8e976-171">An diesem Punkt hat der Autor von Assembly3 einige Möglichkeiten:</span><span class="sxs-lookup"><span data-stu-id="8e976-171">At this point the author of Assembly3 has a few choices:</span></span>

- <span data-ttu-id="8e976-172">Sie können die Warnung über `object?` und `object` Konflikt annehmen/unterdrücken.</span><span class="sxs-lookup"><span data-stu-id="8e976-172">They can accept / suppress the warning about `object?` and `object` mismatch.</span></span>
- <span data-ttu-id="8e976-173">Sie können die Warnung über `object?` und `!` Konflikt annehmen/unterdrücken.</span><span class="sxs-lookup"><span data-stu-id="8e976-173">They can accept / suppress the warning about `object?` and `!` mismatch.</span></span>
- <span data-ttu-id="8e976-174">Sie können einfach die `null` Überprüfungs Prüfung entfernen (Lösch `!` und explizite Überprüfung ausführen).</span><span class="sxs-lookup"><span data-stu-id="8e976-174">They can just remove the `null` validation check (delete `!` and do explicit checking)</span></span>

<span data-ttu-id="8e976-175">Dabei handelt es sich um ein echtes Szenario, aber vorerst besteht die Idee darin, mit der Warnung fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="8e976-175">This is a real scenario but for now the idea is to move forward with the warning.</span></span> <span data-ttu-id="8e976-176">Wenn sich herausstellt, dass die Warnung häufiger als erwartet auftritt, können wir Sie später entfernen (das Gegenteil ist nicht der Fall).</span><span class="sxs-lookup"><span data-stu-id="8e976-176">If it turns out the warning happens more frequently than we anticipate then we can remove it later (the reverse is not true).</span></span>

### <a name="implicit-property-setter-arguments"></a><span data-ttu-id="8e976-177">Implizite Eigenschaften Setter-Argumente</span><span class="sxs-lookup"><span data-stu-id="8e976-177">Implicit property setter arguments</span></span>
<span data-ttu-id="8e976-178">Das `value`-Argument eines-Parameters ist implizit und wird nicht in einer Parameterliste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="8e976-178">The `value` argument of a parameter is implicit and does not appear in any parameter list.</span></span> <span data-ttu-id="8e976-179">Dies bedeutet, dass es sich nicht um ein Ziel dieses Features handeln kann.</span><span class="sxs-lookup"><span data-stu-id="8e976-179">That means it cannot be a target of this feature.</span></span> <span data-ttu-id="8e976-180">Die Eigenschaften Setter-Syntax kann so erweitert werden, dass Sie eine Parameterliste enthält, um die Anwendung des `!` Operators zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="8e976-180">The property setter syntax could be extended to include a parameter list to allow the `!` operator to be applied.</span></span> <span data-ttu-id="8e976-181">Das ist jedoch gegen die Idee dieses Features, `null` Validierung zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="8e976-181">But that cuts against the idea of this feature making `null` validation simpler.</span></span> <span data-ttu-id="8e976-182">Daher funktioniert das implizite `value` Argument nur mit diesem Feature.</span><span class="sxs-lookup"><span data-stu-id="8e976-182">As such the implicit `value` argument just won't work with this feature.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="8e976-183">Überlegungen für die Zukunft</span><span class="sxs-lookup"><span data-stu-id="8e976-183">Future Considerations</span></span>
<span data-ttu-id="8e976-184">Keine</span><span class="sxs-lookup"><span data-stu-id="8e976-184">None</span></span>
