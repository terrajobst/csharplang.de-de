---
ms.openlocfilehash: 50f2bd2d0a84064cfe35fe65b9e5c59c052d19ac
ms.sourcegitcommit: 1dbb8e82bed5012a58a3a035bf2c3737ed570d07
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80122948"
---
# <a name="ranges"></a><span data-ttu-id="47bf9-101">Ranges</span><span class="sxs-lookup"><span data-stu-id="47bf9-101">Ranges</span></span>

## <a name="summary"></a><span data-ttu-id="47bf9-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="47bf9-102">Summary</span></span>

<span data-ttu-id="47bf9-103">Diese Funktion dient zum Übernehmen von zwei neuen Operatoren, die das Erstellen von `System.Index`-und `System.Range` Objekten sowie deren Verwendung zum Indizieren/Slice von Auflistungen zur Laufzeit ermöglichen</span><span class="sxs-lookup"><span data-stu-id="47bf9-103">This feature is about delivering two new operators that allow constructing `System.Index` and `System.Range` objects, and using them to index/slice collections at runtime.</span></span>

## <a name="overview"></a><span data-ttu-id="47bf9-104">Übersicht</span><span class="sxs-lookup"><span data-stu-id="47bf9-104">Overview</span></span>

### <a name="well-known-types-and-members"></a><span data-ttu-id="47bf9-105">Bekannte Typen und Member</span><span class="sxs-lookup"><span data-stu-id="47bf9-105">Well-known types and members</span></span>

<span data-ttu-id="47bf9-106">Um die neuen syntaktischen Formen für `System.Index` und `System.Range`zu verwenden, sind möglicherweise neue bekannte Typen und Member erforderlich, je nachdem, welche syntaktischen Formulare verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="47bf9-106">To use the new syntactic forms for `System.Index` and `System.Range`, new well-known types and members may be necessary, depending on which syntactic forms are used.</span></span>

<span data-ttu-id="47bf9-107">Um den "hat"-Operator (`^`) zu verwenden, ist Folgendes erforderlich:</span><span class="sxs-lookup"><span data-stu-id="47bf9-107">To use the "hat" operator (`^`), the following is required</span></span>

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

<span data-ttu-id="47bf9-108">Um den `System.Index`-Typ als Argument in einem Array Element-Zugriff zu verwenden, ist der folgende Member erforderlich:</span><span class="sxs-lookup"><span data-stu-id="47bf9-108">To use the `System.Index` type as an argument in an array element access, the following member is required:</span></span>

```csharp
int System.Index.GetOffset(int length);
```

<span data-ttu-id="47bf9-109">Die `..` Syntax für `System.Range` erfordert den `System.Range` Typ sowie einen oder mehrere der folgenden Member:</span><span class="sxs-lookup"><span data-stu-id="47bf9-109">The `..` syntax for `System.Range` will require the `System.Range` type, as well as one or more of the following members:</span></span>

```csharp
namespace System
{
    public readonly struct Range
    {
        public Range(System.Index start, System.Index end);
        public static Range StartAt(System.Index start);
        public static Range EndAt(System.Index end);
        public static Range All { get; }
    }
}
```

<span data-ttu-id="47bf9-110">Die `..`-Syntax ermöglicht, dass sowohl als auch keines der Argumente vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="47bf9-110">The `..` syntax allows for either, both, or none of its arguments to be absent.</span></span> <span data-ttu-id="47bf9-111">Unabhängig von der Anzahl der Argumente ist der `Range`-Konstruktor für die Verwendung der `Range`-Syntax immer ausreichend.</span><span class="sxs-lookup"><span data-stu-id="47bf9-111">Regardless of the number of arguments, the `Range` constructor is always sufficient for using the `Range` syntax.</span></span> <span data-ttu-id="47bf9-112">Wenn jedoch eines der anderen Member vorhanden ist, und mindestens ein `..` Argument fehlt, kann der entsprechende Member ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="47bf9-112">However, if any of the other members are present and one or more of the `..` arguments are missing, the appropriate member may be substituted.</span></span>

<span data-ttu-id="47bf9-113">Zum Schluss muss für einen Wert des Typs `System.Range` in einem Array Element-Zugriffs Ausdruck verwendet werden, der folgende Member muss vorhanden sein:</span><span class="sxs-lookup"><span data-stu-id="47bf9-113">Finally, for a value of type `System.Range` to be used in an array element access expression, the following member must be present:</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a><span data-ttu-id="47bf9-114">System. Index</span><span class="sxs-lookup"><span data-stu-id="47bf9-114">System.Index</span></span>

<span data-ttu-id="47bf9-115">C#hat keine Möglichkeit, eine Auflistung vom Ende zu indizieren, aber die meisten Indexer verwenden die "from Start"-Idee oder einen "Length-i"-Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="47bf9-115">C# has no way of indexing a collection from the end, but rather most indexers use the "from start" notion, or do a "length - i" expression.</span></span> <span data-ttu-id="47bf9-116">Wir stellen einen neuen Index Ausdruck vor, der "vom Ende" bedeutet.</span><span class="sxs-lookup"><span data-stu-id="47bf9-116">We introduce a new Index expression that means "from the end".</span></span> <span data-ttu-id="47bf9-117">Mit diesem Feature wird ein neuer unärer Prefix-Operator (hat) eingeführt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-117">The feature will introduce a new unary prefix "hat" operator.</span></span> <span data-ttu-id="47bf9-118">Der einzige Operand muss in `System.Int32`konvertierbar sein.</span><span class="sxs-lookup"><span data-stu-id="47bf9-118">Its single operand must be convertible to `System.Int32`.</span></span> <span data-ttu-id="47bf9-119">Sie wird in den entsprechenden `System.Index` factorymethodenaufzurufen gesenkt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-119">It will be lowered into the appropriate `System.Index` factory method call.</span></span>

<span data-ttu-id="47bf9-120">Wir erweitern die Grammatik für *unary_expression* mit der folgenden zusätzlichen Syntax Form:</span><span class="sxs-lookup"><span data-stu-id="47bf9-120">We augment the grammar for *unary_expression* with the following additional syntax form:</span></span>

```antlr
unary_expression
    : '^' unary_expression
    ;
```

<span data-ttu-id="47bf9-121">Dies wird als *Index des endoperators* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="47bf9-121">We call this the *index from end* operator.</span></span> <span data-ttu-id="47bf9-122">Der vordefinierte *Index von endoperatoren* lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="47bf9-122">The predefined *index from end* operators are as follows:</span></span>

```csharp
    System.Index operator ^(int fromEnd);
```

<span data-ttu-id="47bf9-123">Das Verhalten dieses Operators ist nur für Eingabewerte, die größer oder gleich 0 (null) sind, definiert.</span><span class="sxs-lookup"><span data-stu-id="47bf9-123">The behavior of this operator is only defined for input values greater than or equal to zero.</span></span>

<span data-ttu-id="47bf9-124">Beispiele:</span><span class="sxs-lookup"><span data-stu-id="47bf9-124">Examples:</span></span>

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a><span data-ttu-id="47bf9-125">System. Range</span><span class="sxs-lookup"><span data-stu-id="47bf9-125">System.Range</span></span>

<span data-ttu-id="47bf9-126">C#hat keine syntaktische Methode, um auf Bereiche oder Slices von Sammlungen zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="47bf9-126">C# has no syntactic way to access "ranges" or "slices" of collections.</span></span> <span data-ttu-id="47bf9-127">Normalerweise sind Benutzer gezwungen, komplexe Strukturen zu implementieren, um Slices von Arbeitsspeicher zu filtern oder zu verarbeiten, oder auf LINQ-Methoden wie `list.Skip(5).Take(2)`zurückgreifen.</span><span class="sxs-lookup"><span data-stu-id="47bf9-127">Usually users are forced to implement complex structures to filter/operate on slices of memory, or resort to LINQ methods like `list.Skip(5).Take(2)`.</span></span> <span data-ttu-id="47bf9-128">Wenn `System.Span<T>` und andere ähnliche Typen hinzugefügt werden, ist es wichtiger, dass diese Art von Vorgang auf einer tieferen Ebene in der Sprache/Laufzeit unterstützt wird und die Schnittstelle vereinheitlicht ist.</span><span class="sxs-lookup"><span data-stu-id="47bf9-128">With the addition of `System.Span<T>` and other similar types, it becomes more important to have this kind of operation supported on a deeper level in the language/runtime, and have the interface unified.</span></span>

<span data-ttu-id="47bf9-129">In der Sprache wird ein neuer Bereichs Operator `x..y`eingeführt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-129">The language will introduce a new range operator `x..y`.</span></span> <span data-ttu-id="47bf9-130">Dabei handelt es sich um einen binären Infix-Operator, der zwei Ausdrücke akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="47bf9-130">It is a binary infix operator that accepts two expressions.</span></span> <span data-ttu-id="47bf9-131">Beide Operanden können ausgelassen werden (Beispiele unten) und müssen in `System.Index`konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="47bf9-131">Either operand can be omitted (examples below), and they have to be convertible to `System.Index`.</span></span> <span data-ttu-id="47bf9-132">Der Vorgang wird auf den entsprechenden `System.Range` Factory-Methoden aufrufsvorgang herabgesetzt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-132">It will be lowered to the appropriate `System.Range` factory method call.</span></span>

<span data-ttu-id="47bf9-133">Wir ersetzen die C# Grammatikregeln für *multiplicative_expression* durch Folgendes (um eine neue Rang folgen Ebene einzuführen):</span><span class="sxs-lookup"><span data-stu-id="47bf9-133">We replace the C# grammar rules for *multiplicative_expression* with the following (in order to introduce a new precedence level):</span></span>

```antlr
range_expression
    : unary_expression
    | range_expression? '..' range_expression?
    ;

multiplicative_expression
    : range_expression
    | multiplicative_expression '*' range_expression
    | multiplicative_expression '/' range_expression
    | multiplicative_expression '%' range_expression
    ;
```

<span data-ttu-id="47bf9-134">Alle Formen des *Bereichs Operators* haben die gleiche Rangfolge.</span><span class="sxs-lookup"><span data-stu-id="47bf9-134">All forms of the *range operator* have the same precedence.</span></span> <span data-ttu-id="47bf9-135">Diese neue Rang Folge Gruppe ist niedriger als die *unären Operatoren* und höher als die *arithmetischen Operatoren mulitiplizierung*.</span><span class="sxs-lookup"><span data-stu-id="47bf9-135">This new precedence group is lower than the *unary operators* and higher than the *mulitiplicative arithmetic operators*.</span></span>

<span data-ttu-id="47bf9-136">Wir nennen den `..` Operator den *Bereichs Operator*.</span><span class="sxs-lookup"><span data-stu-id="47bf9-136">We call the `..` operator the *range operator*.</span></span> <span data-ttu-id="47bf9-137">Der integrierte Bereichs Operator kann ungefähr verstanden werden, um dem Aufruf eines integrierten Operators in dieser Form zu entsprechen:</span><span class="sxs-lookup"><span data-stu-id="47bf9-137">The built-in range operator can roughly be understood to correspond to the invocation of a built-in operator of this form:</span></span>

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

<span data-ttu-id="47bf9-138">Beispiele:</span><span class="sxs-lookup"><span data-stu-id="47bf9-138">Examples:</span></span>

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

<span data-ttu-id="47bf9-139">Darüber hinaus sollten `System.Index` eine implizite Konvertierung von `System.Int32`haben, damit keine Überlastung erforderlich ist, um ganze Zahlen und Indizes über mehrdimensionale Signaturen zu mischen.</span><span class="sxs-lookup"><span data-stu-id="47bf9-139">Moreover, `System.Index` should have an implicit conversion from `System.Int32`, in order not to need to overload for mixing integers and indexes over multi-dimensional signatures.</span></span>

## <a name="adding-index-and-range-support-to-existing-library-types"></a><span data-ttu-id="47bf9-140">Hinzufügen von Index-und Bereichs Unterstützung zu vorhandenen Bibliothekstypen</span><span class="sxs-lookup"><span data-stu-id="47bf9-140">Adding Index and Range support to existing library types</span></span>

### <a name="implicit-index-support"></a><span data-ttu-id="47bf9-141">Implizite Index Unterstützung</span><span class="sxs-lookup"><span data-stu-id="47bf9-141">Implicit Index support</span></span>

<span data-ttu-id="47bf9-142">Die Sprache stellt einen instanzindexer-Member mit einem einzelnen Parameter vom Typ `Index` für Typen bereit, die die folgenden Kriterien erfüllen:</span><span class="sxs-lookup"><span data-stu-id="47bf9-142">The language will provide an instance indexer member with a single parameter of type `Index` for types which meet the following criteria:</span></span>

- <span data-ttu-id="47bf9-143">Der Typ ist "zählbar".</span><span class="sxs-lookup"><span data-stu-id="47bf9-143">The type is Countable.</span></span>
- <span data-ttu-id="47bf9-144">Der-Typ verfügt über einen barrierefreien instanzindexer, der einen einzelnen `int` als Argument annimmt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-144">The type has an accessible instance indexer which takes a single `int` as the argument.</span></span>
- <span data-ttu-id="47bf9-145">Der-Typ verfügt über keinen zugänglichen instanzindexer, der einen `Index` als ersten Parameter annimmt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-145">The type does not have an accessible instance indexer which takes an `Index` as the first parameter.</span></span> <span data-ttu-id="47bf9-146">Der `Index` muss der einzige Parameter sein, oder die restlichen Parameter müssen optional sein.</span><span class="sxs-lookup"><span data-stu-id="47bf9-146">The `Index` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="47bf9-147">Ein Typ ist " ***zählbar*** ", wenn er über eine Eigenschaft mit dem Namen `Length` oder `Count` mit einem zugänglichen Getter und einem Rückgabetyp von `int`verfügt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-147">A type is ***Countable*** if it  has a property named `Length` or `Count` with an accessible getter and a return type of `int`.</span></span> <span data-ttu-id="47bf9-148">Die Sprache kann diese Eigenschaft verwenden, um einen Ausdruck vom Typ `Index` in eine `int` an der Stelle des Ausdrucks zu konvertieren, ohne dass der Typ `Index` verwendet werden muss.</span><span class="sxs-lookup"><span data-stu-id="47bf9-148">The language can make use of this property to convert an expression of type `Index` into an `int` at the point of the expression without the need to use the type `Index` at all.</span></span> <span data-ttu-id="47bf9-149">Wenn sowohl `Length` als auch `Count` vorhanden sind, werden `Length` bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-149">In case both `Length` and `Count` are present, `Length` will be preferred.</span></span> <span data-ttu-id="47bf9-150">Der Einfachheit halber wird in diesem Vorschlag der Name `Length` verwendet, um `Count` oder `Length`darzustellen.</span><span class="sxs-lookup"><span data-stu-id="47bf9-150">For simplicity going forward, the proposal will use the name `Length` to represent `Count` or `Length`.</span></span>

<span data-ttu-id="47bf9-151">Bei solchen Typen verhält sich die Sprache so, als ob ein Indexer-Member der Form `T this[Index index]` ist, wobei `T` der Rückgabetyp des `int` basierten Indexers ist, einschließlich aller `ref`-Stil Anmerkungen.</span><span class="sxs-lookup"><span data-stu-id="47bf9-151">For such types, the language will act as if there is an indexer member of the form `T this[Index index]` where `T` is the return type of the `int` based indexer including any `ref` style annotations.</span></span> <span data-ttu-id="47bf9-152">Das neue Element hat denselben `get` und `set` Member mit überein stimmendem Zugriff als `int` Indexer.</span><span class="sxs-lookup"><span data-stu-id="47bf9-152">The new member will have the same `get` and `set` members with matching accessibility as the `int` indexer.</span></span> 

<span data-ttu-id="47bf9-153">Der neue Indexer wird implementiert, indem das Argument vom Typ `Index` in eine `int` umgewandelt und ein Aufruf an den `int` basierten Indexer ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="47bf9-153">The new indexer will be implemented by converting the argument of type `Index` into an `int` and emitting a call to the `int` based indexer.</span></span> <span data-ttu-id="47bf9-154">Zu Diskussions Zwecken verwenden wir das Beispiel `receiver[expr]`.</span><span class="sxs-lookup"><span data-stu-id="47bf9-154">For discussion purposes, let's use the example of `receiver[expr]`.</span></span> <span data-ttu-id="47bf9-155">Die Konvertierung von `expr` in `int` erfolgt wie folgt:</span><span class="sxs-lookup"><span data-stu-id="47bf9-155">The conversion of `expr` to `int` will occur as follows:</span></span>

- <span data-ttu-id="47bf9-156">Wenn das-Argument das Formular `^expr2` und der Typ der `expr2` `int`ist, wird es in `receiver.Length - expr2`übersetzt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-156">When the argument is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="47bf9-157">Andernfalls wird Sie als `expr.GetOffset(receiver.Length)`übersetzt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-157">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="47bf9-158">Dies ermöglicht es Entwicklern, die `Index` Funktion für vorhandene Typen zu verwenden, ohne dass Änderungen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="47bf9-158">This allows for developers to use the `Index` feature on existing types without the need for modification.</span></span> <span data-ttu-id="47bf9-159">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="47bf9-159">For example:</span></span>

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

<span data-ttu-id="47bf9-160">Die `receiver`-und `Length` Ausdrücke werden nach Bedarf überlaufen, um sicherzustellen, dass Nebeneffekte nur einmal ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="47bf9-160">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="47bf9-161">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="47bf9-161">For example:</span></span>

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int this[int index] => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get()[^1];
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="47bf9-162">Mit diesem Code wird "Get length 3" gedruckt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-162">This code will print "Get Length 3".</span></span>

### <a name="implicit-range-support"></a><span data-ttu-id="47bf9-163">Implizite Bereichs Unterstützung</span><span class="sxs-lookup"><span data-stu-id="47bf9-163">Implicit Range support</span></span>

<span data-ttu-id="47bf9-164">Die Sprache stellt einen instanzindexer-Member mit einem einzelnen Parameter vom Typ `Range` für Typen bereit, die die folgenden Kriterien erfüllen:</span><span class="sxs-lookup"><span data-stu-id="47bf9-164">The language will provide an instance indexer member with a single parameter of type `Range` for types which meet the following criteria:</span></span>

- <span data-ttu-id="47bf9-165">Der Typ ist "zählbar".</span><span class="sxs-lookup"><span data-stu-id="47bf9-165">The type is Countable.</span></span>
- <span data-ttu-id="47bf9-166">Der-Typ verfügt über einen zugänglichen Member mit dem Namen `Slice`, der zwei Parameter vom Typ `int`hat.</span><span class="sxs-lookup"><span data-stu-id="47bf9-166">The type has an accessible member named `Slice` which has two parameters of type `int`.</span></span>
- <span data-ttu-id="47bf9-167">Der-Typ verfügt über keinen instanzindexer, der einen einzelnen `Range` als ersten Parameter annimmt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-167">The type does not have an instance indexer which takes a single `Range` as the first parameter.</span></span> <span data-ttu-id="47bf9-168">Der `Range` muss der einzige Parameter sein, oder die restlichen Parameter müssen optional sein.</span><span class="sxs-lookup"><span data-stu-id="47bf9-168">The `Range` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="47bf9-169">Bei solchen Typen wird die Sprache so gebunden, als ob es einen Indexer-Member in der Form gibt `T this[Range range]` wobei `T` der Rückgabetyp der `Slice` Methode ist, einschließlich aller `ref`-Stil Anmerkungen.</span><span class="sxs-lookup"><span data-stu-id="47bf9-169">For such types, the language will bind as if there is an indexer member of the form `T this[Range range]` where `T` is the return type of the `Slice` method including any `ref` style annotations.</span></span> <span data-ttu-id="47bf9-170">Der neue Member verfügt auch über übereinstimmende Barrierefreiheit mit `Slice`.</span><span class="sxs-lookup"><span data-stu-id="47bf9-170">The new member will also have matching accessibility with `Slice`.</span></span> 

<span data-ttu-id="47bf9-171">Wenn der `Range` basierte Indexer an einen Ausdruck mit dem Namen `receiver`gebunden ist, wird er verringert, indem der `Range` Ausdruck in zwei Werte umgewandelt wird, die dann an die `Slice`-Methode weitergegeben werden.</span><span class="sxs-lookup"><span data-stu-id="47bf9-171">When the `Range` based indexer is bound on an expression named `receiver`, it will be lowered by converting the `Range` expression into two values that are then passed to the `Slice` method.</span></span> <span data-ttu-id="47bf9-172">Zu Diskussions Zwecken verwenden wir das Beispiel `receiver[expr]`.</span><span class="sxs-lookup"><span data-stu-id="47bf9-172">For discussion purposes, let's use the example of `receiver[expr]`.</span></span>

<span data-ttu-id="47bf9-173">Das erste Argument `Slice` wird durch das Umrechnen des typisierten Bereichs Ausdrucks in folgender Weise erreicht:</span><span class="sxs-lookup"><span data-stu-id="47bf9-173">The first argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="47bf9-174">Wenn `expr` die Form `expr1..expr2` (wo `expr2` ausgelassen werden kann) und `expr1` den Typ `int`hat, wird es als `expr1`ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="47bf9-174">When `expr` is of the form `expr1..expr2` (where `expr2` can be omitted) and `expr1` has type `int`, then it will be emitted as `expr1`.</span></span>
- <span data-ttu-id="47bf9-175">Wenn `expr` das Formular `^expr1..expr2` (wobei `expr2` ausgelassen werden kann), wird es als `receiver.Length - expr1`ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="47bf9-175">When `expr` is of the form `^expr1..expr2` (where `expr2` can be omitted), then it will be emitted as `receiver.Length - expr1`.</span></span>
- <span data-ttu-id="47bf9-176">Wenn `expr` das Formular `..expr2` (wobei `expr2` ausgelassen werden kann), wird es als `0`ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="47bf9-176">When `expr` is of the form `..expr2` (where `expr2` can be omitted), then it will be emitted as `0`.</span></span>
- <span data-ttu-id="47bf9-177">Andernfalls wird Sie als `expr.Start.GetOffset(receiver.Length)`ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="47bf9-177">Otherwise, it will be emitted as `expr.Start.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="47bf9-178">Dieser Wert wird bei der Berechnung des zweiten `Slice` Arguments wieder verwendet.</span><span class="sxs-lookup"><span data-stu-id="47bf9-178">This value will be re-used in the calculation of the second `Slice` argument.</span></span> <span data-ttu-id="47bf9-179">Wenn dies der Fall ist, wird es als `start`bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="47bf9-179">When doing so it will be referred to as `start`.</span></span> <span data-ttu-id="47bf9-180">Das zweite Argument von `Slice` wird durch die folgende typumrechnung des Bereichs typisierten Ausdrucks abgerufen:</span><span class="sxs-lookup"><span data-stu-id="47bf9-180">The second argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="47bf9-181">Wenn `expr` die Form `expr1..expr2` (wo `expr1` ausgelassen werden kann) und `expr2` den Typ `int`hat, wird es als `expr2 - start`ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="47bf9-181">When `expr` is of the form `expr1..expr2` (where `expr1` can be omitted) and `expr2` has type `int`, then it will be emitted as `expr2 - start`.</span></span>
- <span data-ttu-id="47bf9-182">Wenn `expr` das Formular `expr1..^expr2` (wobei `expr1` ausgelassen werden kann), wird es als `(receiver.Length - expr2) - start`ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="47bf9-182">When `expr` is of the form `expr1..^expr2` (where `expr1` can be omitted), then it will be emitted as `(receiver.Length - expr2) - start`.</span></span>
- <span data-ttu-id="47bf9-183">Wenn `expr` das Formular `expr1..` (wobei `expr1` ausgelassen werden kann), wird es als `receiver.Length - start`ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="47bf9-183">When `expr` is of the form `expr1..` (where `expr1` can be omitted), then it will be emitted as `receiver.Length - start`.</span></span>
- <span data-ttu-id="47bf9-184">Andernfalls wird Sie als `expr.End.GetOffset(receiver.Length) - start`ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="47bf9-184">Otherwise, it will be emitted as `expr.End.GetOffset(receiver.Length) - start`.</span></span>

<span data-ttu-id="47bf9-185">Die `receiver`-, `Length`-und `expr`-Ausdrücke werden nach Bedarf überlaufen, um sicherzustellen, dass Nebeneffekte nur einmal ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="47bf9-185">The `receiver`, `Length`, and `expr` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="47bf9-186">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="47bf9-186">For example:</span></span>

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int[] Slice(int start, int length) { 
        var slice = new int[length];
        Array.Copy(_array, start, slice, 0, length);
        return slice;
    }
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        var array = Get()[0..2];
        Console.WriteLine(array.length);
    }
}
```

<span data-ttu-id="47bf9-187">Mit diesem Code wird "Get length 2" gedruckt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-187">This code will print "Get Length 2".</span></span>

<span data-ttu-id="47bf9-188">In der Sprache werden die folgenden bekannten Typen als Sonderzeichen verwendet:</span><span class="sxs-lookup"><span data-stu-id="47bf9-188">The language will special case the following known types:</span></span> 

- <span data-ttu-id="47bf9-189">`string`: die Methode `Substring` wird anstelle von `Slice`verwendet.</span><span class="sxs-lookup"><span data-stu-id="47bf9-189">`string`: the method `Substring` will be used instead of `Slice`.</span></span>
- <span data-ttu-id="47bf9-190">`array`: die Methode `System.Reflection.CompilerServices.GetSubArray` wird anstelle von `Slice`verwendet.</span><span class="sxs-lookup"><span data-stu-id="47bf9-190">`array`: the method `System.Reflection.CompilerServices.GetSubArray` will be used instead of `Slice`.</span></span>

## <a name="alternatives"></a><span data-ttu-id="47bf9-191">Alternativen</span><span class="sxs-lookup"><span data-stu-id="47bf9-191">Alternatives</span></span>

<span data-ttu-id="47bf9-192">Die neuen Operatoren (`^` und `..`) sind syntaktische Sugar.</span><span class="sxs-lookup"><span data-stu-id="47bf9-192">The new operators (`^` and `..`) are syntactic sugar.</span></span> <span data-ttu-id="47bf9-193">Die Funktionalität kann durch explizite Aufrufe von `System.Index`-und `System.Range` Factorymethoden implementiert werden, führt jedoch zu viel mehr Code Bausteinen, und die Benutzer Funktionalität ist nicht intuitiv.</span><span class="sxs-lookup"><span data-stu-id="47bf9-193">The functionality can be implemented by explicit calls to `System.Index` and `System.Range` factory methods, but it will result in a lot more boilerplate code, and the experience will be unintuitive.</span></span>

## <a name="il-representation"></a><span data-ttu-id="47bf9-194">Il-Darstellung</span><span class="sxs-lookup"><span data-stu-id="47bf9-194">IL Representation</span></span>

<span data-ttu-id="47bf9-195">Diese beiden Operatoren werden auf reguläre Indexer-/Methodenaufrufe herabgesetzt, ohne Änderungen in nachfolgenden compilerschichten.</span><span class="sxs-lookup"><span data-stu-id="47bf9-195">These two operators will be lowered to regular indexer/method calls, with no change in subsequent compiler layers.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="47bf9-196">Laufzeitverhalten</span><span class="sxs-lookup"><span data-stu-id="47bf9-196">Runtime behavior</span></span>

- <span data-ttu-id="47bf9-197">Der Compiler kann Indexer für integrierte Typen wie Arrays und Zeichen folgen optimieren und die Indizierung auf die entsprechenden vorhandenen Methoden verringern.</span><span class="sxs-lookup"><span data-stu-id="47bf9-197">Compiler can optimize indexers for built-in types like arrays and strings, and lower the indexing to the appropriate existing methods.</span></span>
- <span data-ttu-id="47bf9-198">`System.Index` wird ausgelöst, wenn ein Wert mit einem negativen Wert erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="47bf9-198">`System.Index` will throw if constructed with a negative value.</span></span>
- <span data-ttu-id="47bf9-199">`^0` löst nicht aus, aber es wird in die Länge der Auflistung bzw. der Enumerable übersetzt, für die es bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="47bf9-199">`^0` does not throw, but it translates to the length of the collection/enumerable it is supplied to.</span></span>
- <span data-ttu-id="47bf9-200">`Range.All` ist semantisch äquivalent zu `0..^0`und kann für diese Indizes deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="47bf9-200">`Range.All` is semantically equivalent to `0..^0`, and can be deconstructed to these indices.</span></span>

## <a name="considerations"></a><span data-ttu-id="47bf9-201">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="47bf9-201">Considerations</span></span>

### <a name="detect-indexable-based-on-icollection"></a><span data-ttu-id="47bf9-202">Erkennen von indexbaren auf der Grundlage von ICollection</span><span class="sxs-lookup"><span data-stu-id="47bf9-202">Detect Indexable based on ICollection</span></span>

<span data-ttu-id="47bf9-203">Die Inspiration für dieses Verhalten waren Auflistungsinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="47bf9-203">The inspiration for this behavior was collection initializers.</span></span> <span data-ttu-id="47bf9-204">Verwenden der Struktur eines Typs, um zu vermitteln, dass Sie sich für eine Funktion entschieden hat.</span><span class="sxs-lookup"><span data-stu-id="47bf9-204">Using the structure of a type to convey that it had opted into a feature.</span></span> <span data-ttu-id="47bf9-205">Bei Auflistungsinitialisierern können Typen die Funktion aktivieren, indem Sie die-Schnittstelle implementieren `IEnumerable` (nicht generisch).</span><span class="sxs-lookup"><span data-stu-id="47bf9-205">In the case of collection initializers types can opt into the feature by implementing the interface `IEnumerable` (non generic).</span></span>

<span data-ttu-id="47bf9-206">Dieser Vorschlag erforderte zunächst, dass die Typen `ICollection` implementieren, um sich als indexbar zu qualifizieren.</span><span class="sxs-lookup"><span data-stu-id="47bf9-206">This proposal initially required that types implement `ICollection` in order to qualify as Indexable.</span></span> <span data-ttu-id="47bf9-207">Dies erforderte jedoch eine Reihe von besonderen Fällen:</span><span class="sxs-lookup"><span data-stu-id="47bf9-207">That required a number of special cases though:</span></span>

- <span data-ttu-id="47bf9-208">`ref struct`: Diese können keine Schnittstellen implementieren, aber Typen wie `Span<T>` eignen sich ideal für Index-und Bereichs Unterstützung.</span><span class="sxs-lookup"><span data-stu-id="47bf9-208">`ref struct`: these cannot implement interfaces yet types like `Span<T>` are ideal for index / range support.</span></span> 
- <span data-ttu-id="47bf9-209">`string`: implementiert keine `ICollection`, und das Hinzufügen dieser `interface` hat große Kosten.</span><span class="sxs-lookup"><span data-stu-id="47bf9-209">`string`: does not implement `ICollection` and adding that `interface` has a large cost.</span></span>

<span data-ttu-id="47bf9-210">Dies bedeutet, dass die Groß-/Kleinschreibung von Schlüsseltypen bereits erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="47bf9-210">This means to support key types special casing is already needed.</span></span> <span data-ttu-id="47bf9-211">Die Groß-/Kleinschreibung von `string` ist weniger interessant, da die Sprache dies in anderen Bereichen (`foreach` Herabsetzung, Konstanten usw.) tut. Die Groß-/Kleinschreibung von `ref struct` ist eher darauf zu achten, dass eine ganze Klasse von Typen eine besondere Schreibweise ist.</span><span class="sxs-lookup"><span data-stu-id="47bf9-211">The special casing of `string` is less interesting as the language does this in other areas (`foreach` lowering, constants, etc ...). The special casing of `ref struct` is more concerning as it's special casing an entire class of types.</span></span> <span data-ttu-id="47bf9-212">Sie werden als Indexer bezeichnet, wenn Sie einfach über eine Eigenschaft mit dem Namen "`Count`" mit dem Rückgabetyp "`int`" verfügen.</span><span class="sxs-lookup"><span data-stu-id="47bf9-212">They get labeled as Indexable if they simply have a property named `Count` with a return type of `int`.</span></span> 

<span data-ttu-id="47bf9-213">Nach der Überlegung wurde der Entwurf normalisiert, um zu sagen, dass jeder Typ, der über eine Eigenschaft `Count` / `Length` mit dem Rückgabetyp `int` ist, indexbar ist.</span><span class="sxs-lookup"><span data-stu-id="47bf9-213">After consideration the design was normalized to say that any type which has a property `Count` / `Length` with a return type of `int` is Indexable.</span></span> <span data-ttu-id="47bf9-214">Dadurch werden alle Sonderzeichen entfernt, auch für `string` und Arrays.</span><span class="sxs-lookup"><span data-stu-id="47bf9-214">That removes all special casing, even for `string` and arrays.</span></span>

### <a name="detect-just-count"></a><span data-ttu-id="47bf9-215">Nur Anzahl erkennen</span><span class="sxs-lookup"><span data-stu-id="47bf9-215">Detect just Count</span></span>

<span data-ttu-id="47bf9-216">Das Erkennen von Eigenschaften Namen `Count` oder `Length` erschwert das Design etwas.</span><span class="sxs-lookup"><span data-stu-id="47bf9-216">Detecting on the property names `Count` or `Length` does complicate the design a bit.</span></span> <span data-ttu-id="47bf9-217">Wenn Sie nur eine für die Standardisierung auswählen, reicht dies nicht aus, da Sie am Ende eine große Anzahl von Typen ausschließt:</span><span class="sxs-lookup"><span data-stu-id="47bf9-217">Picking just one to standardize though is not sufficient as it ends up excluding a large number of types:</span></span>

- <span data-ttu-id="47bf9-218">Verwenden Sie `Length`: schließt in System. Collections und untergeordneten Namespaces sehr viele Sammlungen aus.</span><span class="sxs-lookup"><span data-stu-id="47bf9-218">Use `Length`: excludes pretty much every collection in System.Collections and sub-namespaces.</span></span> <span data-ttu-id="47bf9-219">Diese sind tendenziell von `ICollection` abgeleitet und bevorzugen daher `Count` über Länge.</span><span class="sxs-lookup"><span data-stu-id="47bf9-219">Those tend to derive from `ICollection` and hence prefer `Count` over length.</span></span>
- <span data-ttu-id="47bf9-220">`Count`verwenden: schließt `string`, Arrays, `Span<T>` und die meisten `ref struct` basierten Typen aus.</span><span class="sxs-lookup"><span data-stu-id="47bf9-220">Use `Count`: excludes `string`, arrays, `Span<T>` and most `ref struct` based types</span></span>

<span data-ttu-id="47bf9-221">Die zusätzliche Komplikation bei der anfänglichen Erkennung von indexbaren Typen wird durch die Vereinfachung in anderen Aspekten überwogen.</span><span class="sxs-lookup"><span data-stu-id="47bf9-221">The extra complication on the initial detection of Indexable types is outweighed by its simplification in other aspects.</span></span>

### <a name="choice-of-slice-as-a-name"></a><span data-ttu-id="47bf9-222">Auswahl des Slice als Name</span><span class="sxs-lookup"><span data-stu-id="47bf9-222">Choice of Slice as a name</span></span>

<span data-ttu-id="47bf9-223">Der Name `Slice` wurde als de-facto-Standardname für Slice-Vorgänge in .net ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-223">The name `Slice` was chosen as it's the de-facto standard name for slice style operations in .NET.</span></span> <span data-ttu-id="47bf9-224">Ab netcoreapp 2.1 verwenden alle Spannen Stiltypen den Namen `Slice` für aufteilen-Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="47bf9-224">Starting with netcoreapp2.1 all span style types use the name `Slice` for slicing operations.</span></span> <span data-ttu-id="47bf9-225">Vor netcoreapp 2.1 gibt es keine Beispiele für das Aufteilen von aufteilen, um ein Beispiel zu finden.</span><span class="sxs-lookup"><span data-stu-id="47bf9-225">Prior to netcoreapp2.1 there really aren't any examples of slicing to look to for an example.</span></span> <span data-ttu-id="47bf9-226">Typen wie `List<T>`, `ArraySegment<T>``SortedList<T>` wären ideal für die slizierung, aber das Konzept war nicht vorhanden, als Typen hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="47bf9-226">Types like `List<T>`, `ArraySegment<T>`, `SortedList<T>` would've been ideal for slicing but the concept didn't exist when types were added.</span></span> 

<span data-ttu-id="47bf9-227">Daher ist `Slice` das einzige Beispiel, dass es als Name ausgewählt wurde.</span><span class="sxs-lookup"><span data-stu-id="47bf9-227">Thus, `Slice` being the sole example, it was chosen as the name.</span></span>

### <a name="index-target-type-conversion"></a><span data-ttu-id="47bf9-228">Konvertierung des Index Ziel Typs</span><span class="sxs-lookup"><span data-stu-id="47bf9-228">Index target type conversion</span></span>

<span data-ttu-id="47bf9-229">Eine andere Möglichkeit, die `Index` Transformation in einem Indexer-Ausdruck anzuzeigen, ist die Konvertierung des Zieltyps.</span><span class="sxs-lookup"><span data-stu-id="47bf9-229">Another way to view the `Index` transformation in an indexer expression is as a target type conversion.</span></span> <span data-ttu-id="47bf9-230">Anstatt so zu binden, als ob ein Member der Form `return_type this[Index]`ist, weist die Sprache stattdessen eine typisierte Typisierung zu `int`zu.</span><span class="sxs-lookup"><span data-stu-id="47bf9-230">Instead of binding as if there is a member of the form `return_type this[Index]`, the language instead assigns a target typed conversion to `int`.</span></span> 

<span data-ttu-id="47bf9-231">Dieses Konzept kann für alle Element Zugriffe auf zählbare Typen generalisiert werden.</span><span class="sxs-lookup"><span data-stu-id="47bf9-231">This concept could be generalized to all member access on Countable types.</span></span> <span data-ttu-id="47bf9-232">Jedes Mal, wenn ein Ausdruck mit dem Typ `Index` als Argument für einen instanzelementaufruf verwendet wird und der Empfänger zählbar ist, verfügt der Ausdruck über eine Zieltyp Konvertierung in `int`.</span><span class="sxs-lookup"><span data-stu-id="47bf9-232">Whenever an expression with type `Index` is used as an argument to an instance member invocation and the receiver is Countable then the expression will have a target type conversion to `int`.</span></span> <span data-ttu-id="47bf9-233">Die für diese Konvertierung anwendbaren Element Aufrufe umfassen Methoden, Indexer, Eigenschaften, Erweiterungs Methoden usw... Nur Konstruktoren werden ausgeschlossen, da Sie über keinen Empfänger verfügen.</span><span class="sxs-lookup"><span data-stu-id="47bf9-233">The member invocations applicable for this conversion include methods, indexers, properties, extension methods, etc ... Only constructors are excluded as they have no receiver.</span></span> 

<span data-ttu-id="47bf9-234">Die Zieltyp Konvertierung wird für jeden Ausdruck, der einen `Index`Typ aufweist, wie folgt implementiert.</span><span class="sxs-lookup"><span data-stu-id="47bf9-234">The target type conversion will be implemented as follows for any expression which has a type of `Index`.</span></span> <span data-ttu-id="47bf9-235">Für Diskussions Zwecke können Sie das Beispiel `receiver[expr]`verwenden:</span><span class="sxs-lookup"><span data-stu-id="47bf9-235">For discussion purposes lets use the example of `receiver[expr]`:</span></span>

- <span data-ttu-id="47bf9-236">Wenn `expr` das Formular `^expr2` und der Typ der `expr2` `int`ist, wird er in `receiver.Length - expr2`übersetzt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-236">When `expr` is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="47bf9-237">Andernfalls wird Sie als `expr.GetOffset(receiver.Length)`übersetzt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-237">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="47bf9-238">Die `receiver`-und `Length` Ausdrücke werden nach Bedarf überlaufen, um sicherzustellen, dass Nebeneffekte nur einmal ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="47bf9-238">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="47bf9-239">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="47bf9-239">For example:</span></span>

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int GetAt(int index) => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get().GetAt(^1);
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="47bf9-240">Mit diesem Code wird "Get length 3" gedruckt.</span><span class="sxs-lookup"><span data-stu-id="47bf9-240">This code will print "Get Length 3".</span></span> 

<span data-ttu-id="47bf9-241">Diese Funktion ist nützlich für alle Member, die über einen Parameter verfügen, der einen Index repräsentiert.</span><span class="sxs-lookup"><span data-stu-id="47bf9-241">This feature would be beneficial to any member which had a parameter that represented an index.</span></span> <span data-ttu-id="47bf9-242">Beispiel: `List<T>.InsertAt`.</span><span class="sxs-lookup"><span data-stu-id="47bf9-242">For example `List<T>.InsertAt`.</span></span> <span data-ttu-id="47bf9-243">Dies kann auch Verwirrung verursachen, da die Sprache keine Hinweise dazu gibt, ob ein Ausdruck für die Indizierung vorgesehen ist.</span><span class="sxs-lookup"><span data-stu-id="47bf9-243">This also has the potential for confusion as the language can't give any guidance as to whether or not an expression is meant for indexing.</span></span> <span data-ttu-id="47bf9-244">Dabei können alle `Index` Ausdrücke in `int` konvertiert werden, wenn ein Member für einen zählbaren Typ aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="47bf9-244">All it can do is convert any `Index` expression to `int` when invoking a member on a Countable type.</span></span> 

<span data-ttu-id="47bf9-245">Begrenzungen</span><span class="sxs-lookup"><span data-stu-id="47bf9-245">Restrictions:</span></span>

- <span data-ttu-id="47bf9-246">Diese Konvertierung ist nur anwendbar, wenn der Ausdruck mit dem Typ `Index` direkt ein Argument für den Member ist.</span><span class="sxs-lookup"><span data-stu-id="47bf9-246">This conversion is only applicable when the expression with type `Index` is directly an argument to the member.</span></span> <span data-ttu-id="47bf9-247">Dies gilt nicht für andere Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="47bf9-247">It would not apply to any nested expressions.</span></span>

## <a name="decisions-made-during-implementation"></a><span data-ttu-id="47bf9-248">Entscheidungen bei der Implementierung</span><span class="sxs-lookup"><span data-stu-id="47bf9-248">Decisions made during implementation</span></span>

- <span data-ttu-id="47bf9-249">Alle Elemente im Muster müssen Instanzmember sein.</span><span class="sxs-lookup"><span data-stu-id="47bf9-249">All members in the pattern must be instance members</span></span>
- <span data-ttu-id="47bf9-250">Wenn eine Length-Methode gefunden wird, jedoch den falschen Rückgabetyp aufweist, sollten Sie die Anzahl weiterhin suchen.</span><span class="sxs-lookup"><span data-stu-id="47bf9-250">If a Length method is found but it has the wrong return type, continue looking for Count</span></span>
- <span data-ttu-id="47bf9-251">Der Indexer, der für das Index Muster verwendet wird, muss genau einen int-Parameter aufweisen.</span><span class="sxs-lookup"><span data-stu-id="47bf9-251">The indexer used for the Index pattern must have exactly one int parameter</span></span>
- <span data-ttu-id="47bf9-252">Die für das Bereichs Muster verwendete Slice-Methode muss genau zwei int-Parameter aufweisen.</span><span class="sxs-lookup"><span data-stu-id="47bf9-252">The Slice method used for the Range pattern must have exactly two int parameters</span></span>
- <span data-ttu-id="47bf9-253">Bei der Suche nach den mustermembern suchen wir nach ursprünglichen Definitionen, nicht nach konstruierten Membern.</span><span class="sxs-lookup"><span data-stu-id="47bf9-253">When looking for the pattern members, we look for original definitions, not constructed members</span></span>

## <a name="design-meetings"></a><span data-ttu-id="47bf9-254">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="47bf9-254">Design Meetings</span></span>

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a><span data-ttu-id="47bf9-255">Fragen</span><span class="sxs-lookup"><span data-stu-id="47bf9-255">Questions</span></span>
