---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79484369"
---
# <a name="readonly-instance-members"></a><span data-ttu-id="a7f5a-101">Schreibgeschützte Instanzmember</span><span class="sxs-lookup"><span data-stu-id="a7f5a-101">Readonly Instance Members</span></span>

<span data-ttu-id="a7f5a-102">Propagierte Problem: <https://github.com/dotnet/csharplang/issues/1710></span><span class="sxs-lookup"><span data-stu-id="a7f5a-102">Championed Issue: <https://github.com/dotnet/csharplang/issues/1710></span></span>

## <a name="summary"></a><span data-ttu-id="a7f5a-103">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="a7f5a-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a7f5a-104">Geben Sie an, wie die einzelnen Instanzmember in einer Struktur nicht geändert werden sollen, und zwar auf dieselbe Weise, in der `readonly struct` die den Status "ändern" der Instanzelemente angibt.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-104">Provide a way to specify individual instance members on a struct do not modify state, in the same way that `readonly struct` specifies no instance members modify state.</span></span>

<span data-ttu-id="a7f5a-105">Beachten Sie, dass `readonly instance member`! = `pure instance member`.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-105">It is worth noting that `readonly instance member` != `pure instance member`.</span></span> <span data-ttu-id="a7f5a-106">Ein `pure` Instanzmember gewährleistet, dass kein Zustand geändert wird.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-106">A `pure` instance member guarantees no state will be modified.</span></span> <span data-ttu-id="a7f5a-107">Ein `readonly` Instanzmember garantiert nur, dass der Instanzstatus nicht geändert wird.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-107">A `readonly` instance member only guarantees that instance state will not be modified.</span></span>

<span data-ttu-id="a7f5a-108">Alle Instanzmember in einer `readonly struct` können als implizit `readonly instance members`betrachtet werden.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-108">All instance members on a `readonly struct` could be considered implicitly `readonly instance members`.</span></span> <span data-ttu-id="a7f5a-109">Explizite `readonly instance members`, die für nicht schreibgeschützte Strukturen deklariert sind, Verhalten sich auf die gleiche Weise.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-109">Explicit `readonly instance members` declared on non-readonly structs would behave in the same manner.</span></span> <span data-ttu-id="a7f5a-110">Beispielsweise würden Sie weiterhin verborgene Kopien erstellen, wenn Sie einen Instanzmember (auf der aktuellen Instanz oder in einem Feld der Instanz) aufgerufen haben, der selbst nicht schreibgeschützt war.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-110">For example, they would still create hidden copies if you called an instance member (on the current instance or on a field of the instance) which was itself not-readonly.</span></span>

## <a name="motivation"></a><span data-ttu-id="a7f5a-111">Motivation</span><span class="sxs-lookup"><span data-stu-id="a7f5a-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a7f5a-112">Heute haben Benutzer die Möglichkeit, `readonly struct` Typen zu erstellen, die der Compiler erzwingt, dass alle Felder schreibgeschützt sind (und durch Erweiterung, dass keine Instanzmember den Zustand ändern).</span><span class="sxs-lookup"><span data-stu-id="a7f5a-112">Today, users have the ability to create `readonly struct` types which the compiler enforces that all fields are readonly (and by extension, that no instance members modify the state).</span></span> <span data-ttu-id="a7f5a-113">Es gibt jedoch einige Szenarien, in denen Sie über eine vorhandene API verfügen, die barrierefreie Felder verfügbar macht oder eine Mischung aus veränderlichen und nicht veränderlichen Membern aufweist.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-113">However, there are some scenarios where you have an existing API that exposes accessible fields or that has a mix of mutating and non-mutating members.</span></span> <span data-ttu-id="a7f5a-114">Unter diesen Umständen können Sie den Typ nicht als `readonly` markieren (Breaking Change).</span><span class="sxs-lookup"><span data-stu-id="a7f5a-114">Under these circumstances, you cannot mark the type as `readonly` (it would be a breaking change).</span></span>

<span data-ttu-id="a7f5a-115">Dies hat normalerweise keine großen Auswirkungen, außer im Fall von `in`-Parametern.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-115">This normally doesn't have much impact, except in the case of `in` parameters.</span></span> <span data-ttu-id="a7f5a-116">Mit `in`-Parametern für nicht schreibgeschützte Strukturen erstellt der Compiler eine Kopie des-Parameters für jeden Aufruf des Instanzmembers, da er nicht gewährleisten kann, dass der interne Zustand durch den Aufruf nicht geändert wird.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-116">With `in` parameters for non-readonly structs, the compiler will make a copy of the parameter for each instance member invocation, since it cannot guarantee that the invocation does not modify internal state.</span></span> <span data-ttu-id="a7f5a-117">Dies kann zu einer Vielzahl von Kopien und einer schlechteren Gesamtleistung führen, als wenn Sie die Struktur einfach direkt als Wert übermittelt hätten.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-117">This can lead to a multitude of copies and worse overall performance than if you had just passed the struct directly by value.</span></span> <span data-ttu-id="a7f5a-118">Ein Beispiel finden Sie in diesem Code auf [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==) .</span><span class="sxs-lookup"><span data-stu-id="a7f5a-118">For an example, see this code on [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)</span></span>

<span data-ttu-id="a7f5a-119">Einige andere Szenarien, in denen ausgeblendete Kopien auftreten können, sind `static readonly fields` und `literals`.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-119">Some other scenarios where hidden copies can occur include `static readonly fields` and `literals`.</span></span> <span data-ttu-id="a7f5a-120">Wenn Sie zu einem späteren Zeitpunkt unterstützt werden, werden `blittable constants` am gleichen Boot. Das heißt, dass alle zurzeit eine vollständige Kopie erfordern (bei einem Instanzmember-Aufruf), wenn die Struktur nicht als `readonly`gekennzeichnet ist.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-120">If they are supported in the future, `blittable constants` would end up in the same boat; that is they all currently necessitate a full copy (on instance member invocation) if the struct is not marked `readonly`.</span></span>

## <a name="design"></a><span data-ttu-id="a7f5a-121">Entwurf</span><span class="sxs-lookup"><span data-stu-id="a7f5a-121">Design</span></span>
[design]: #design

<span data-ttu-id="a7f5a-122">Ermöglicht es Benutzern, anzugeben, dass ein Instanzmember selbst `readonly` ist, und ändert nicht den Status der Instanz (natürlich mit der gesamten entsprechenden Überprüfung des Compilers).</span><span class="sxs-lookup"><span data-stu-id="a7f5a-122">Allow a user to specify that an instance member is, itself, `readonly` and does not modify the state of the instance (with all the appropriate verification done by the compiler, of course).</span></span> <span data-ttu-id="a7f5a-123">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a7f5a-123">For example:</span></span>

```csharp
public struct Vector2
{
    public float x;
    public float y;

    public readonly float GetLengthReadonly()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public float GetLength()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public readonly float GetLengthIllegal()
    {
        var tmp = MathF.Sqrt(LengthSquared);

        x = tmp;    // Compiler error, cannot write x
        y = tmp;    // Compiler error, cannot write y

        return tmp;
    }

    public float LengthSquared
    {
        readonly get
        {
            return (x * x) +
                   (y * y);
        }
    }
}

public static class MyClass
{
    public static float ExistingBehavior(in Vector2 vector)
    {
        // This code causes a hidden copy, the compiler effectively emits:
        //    var tmpVector = vector;
        //    return tmpVector.GetLength();
        //
        // This is done because the compiler doesn't know that `GetLength()`
        // won't mutate `vector`.

        return vector.GetLength();
    }

    public static float ReadonlyBehavior(in Vector2 vector)
    {
        // This code is emitted exactly as listed. There are no hidden
        // copies as the `readonly` modifier indicates that the method
        // won't mutate `vector`.

        return vector.GetLengthReadonly();
    }
}
```

<span data-ttu-id="a7f5a-124">"Schreibgeschützt" kann auf Eigenschaftenaccessoren angewendet werden, um anzugeben, dass `this` im Accessor nicht mutiert werden.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-124">Readonly can be applied to property accessors to indicate that `this` will not be mutated in the accessor.</span></span> <span data-ttu-id="a7f5a-125">Die folgenden Beispiele verfügen über schreibgeschützte Setter, da diese Accessoren den Zustand des Element Felds ändern, aber nicht den Wert dieses Member-Felds ändern.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-125">The following examples have readonly setters because those accessors modify the state of member field, but do not modify the value of that member field.</span></span>

```csharp
public int Prop1
{
    readonly get
    {
        return this._store["Prop1"];
    }
    readonly set
    {
        this._store["Prop1"] = value;
    }
}
```

<span data-ttu-id="a7f5a-126">Wenn `readonly` auf die Eigenschaften Syntax angewendet wird, bedeutet dies, dass alle Accessoren `readonly`werden.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-126">When `readonly` is applied to the property syntax, it means that all accessors are `readonly`.</span></span>

```csharp
public readonly int Prop2
{
    get
    {
        return this._store["Prop2"];
    }
    set
    {
        this._store["Prop2"] = value;
    }
}
```

<span data-ttu-id="a7f5a-127">"Schreibgeschützt" kann nur auf Accessoren angewendet werden, die den enthaltenden Typ nicht mutieren.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-127">Readonly can only be applied to accessors which do not mutate the containing type.</span></span>

```csharp
public int Prop3
{
    readonly get
    {
        return this._prop3;
    }
    set
    {
        this._prop3 = value;
    }
}
```

<span data-ttu-id="a7f5a-128">"Schreibgeschützt" kann auf einige automatisch implementierte Eigenschaften angewendet werden, hat jedoch keinen sinnvollen Effekt.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-128">Readonly can be applied to some auto-implemented properties, but it won't have a meaningful effect.</span></span> <span data-ttu-id="a7f5a-129">Der Compiler behandelt alle automatisch implementierten Getter als schreibgeschützt, unabhängig davon, ob das `readonly`-Schlüsselwort vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-129">The compiler will treat all auto-implemented getters as readonly whether or not the `readonly` keyword is present.</span></span>

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

<span data-ttu-id="a7f5a-130">"Schreibgeschützt" kann auf manuell implementierte Ereignisse angewendet werden, aber nicht auf Feld ähnliche Ereignisse.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-130">Readonly can be applied to manually-implemented events, but not field-like events.</span></span> <span data-ttu-id="a7f5a-131">"Schreibgeschützt" kann nicht auf einzelne Ereignisaccessoren (hinzufügen/entfernen) angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-131">Readonly cannot be applied to individual event accessors (add/remove).</span></span>

```csharp
// Allowed
public readonly event Action<EventArgs> Event1
{
    add { }
    remove { }
}

// Not allowed
public readonly event Action<EventArgs> Event2;
public event Action<EventArgs> Event3
{
    readonly add { }
    readonly remove { }
}
public static readonly event Event4
{
    add { }
    remove { }
}
```

<span data-ttu-id="a7f5a-132">Einige weitere Syntax Beispiele:</span><span class="sxs-lookup"><span data-stu-id="a7f5a-132">Some other syntax examples:</span></span>

* <span data-ttu-id="a7f5a-133">Ausdruckskörpermember: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span><span class="sxs-lookup"><span data-stu-id="a7f5a-133">Expression bodied members: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span></span>
* <span data-ttu-id="a7f5a-134">Generische Einschränkungen: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span><span class="sxs-lookup"><span data-stu-id="a7f5a-134">Generic constraints: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span></span>

<span data-ttu-id="a7f5a-135">Der Compiler gibt den Instanzmember wie üblich aus und gibt darüber hinaus ein vom Compiler erkanntes Attribut aus, das angibt, dass der Instanzmember den Zustand nicht ändert.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-135">The compiler would emit the instance member, as usual, and would additionally emit a compiler recognized attribute indicating that the instance member does not modify state.</span></span> <span data-ttu-id="a7f5a-136">Dies bewirkt, dass der ausgeblendete `this`-Parameter anstelle `ref T``in T` wird.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-136">This effectively causes the hidden `this` parameter to become `in T` instead of `ref T`.</span></span>

<span data-ttu-id="a7f5a-137">Dadurch kann der Benutzer die genannte Instanzmethode sicher aufzurufen, ohne dass der Compiler eine Kopie erstellen muss.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-137">This would allow the user to safely call said instance method without the compiler needing to make a copy.</span></span>

<span data-ttu-id="a7f5a-138">Die Einschränkungen umfassen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="a7f5a-138">The restrictions would include:</span></span>

* <span data-ttu-id="a7f5a-139">Der `readonly` Modifizierer kann nicht auf statische Methoden, Konstruktoren oder Dekonstruktoren angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-139">The `readonly` modifier cannot be applied to static methods, constructors or destructors.</span></span>
* <span data-ttu-id="a7f5a-140">Der `readonly` Modifizierer kann nicht auf Delegaten angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-140">The `readonly` modifier cannot be applied to delegates.</span></span>
* <span data-ttu-id="a7f5a-141">Der `readonly` Modifizierer kann nicht auf Member der Klasse oder Schnittstelle angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-141">The `readonly` modifier cannot be applied to members of class or interface.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="a7f5a-142">Nachteile</span><span class="sxs-lookup"><span data-stu-id="a7f5a-142">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a7f5a-143">Die gleichen Nachteile wie bei den `readonly struct` Methoden sind heute vorhanden.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-143">Same drawbacks as exist with `readonly struct` methods today.</span></span> <span data-ttu-id="a7f5a-144">Bestimmter Code verursacht möglicherweise weiterhin verborgene Kopien.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-144">Certain code may still cause hidden copies.</span></span>

## <a name="notes"></a><span data-ttu-id="a7f5a-145">Hinweise</span><span class="sxs-lookup"><span data-stu-id="a7f5a-145">Notes</span></span>
[notes]: #notes

<span data-ttu-id="a7f5a-146">Die Verwendung eines Attributs oder eines anderen Schlüssel Worts ist möglicherweise ebenfalls möglich.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-146">Using an attribute or another keyword may also be possible.</span></span>

<span data-ttu-id="a7f5a-147">Dieser Vorschlag steht in gewisser Weise im Zusammenhang mit (aber eine Teilmenge) `functional purity` und/oder `constant expressions`, von denen beide bereits über einige Vorschläge verfügen.</span><span class="sxs-lookup"><span data-stu-id="a7f5a-147">This proposal is somewhat related to (but is more a subset of) `functional purity` and/or `constant expressions`, both of which have had some existing proposals.</span></span>
