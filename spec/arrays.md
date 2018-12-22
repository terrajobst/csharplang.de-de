# <a name="arrays"></a><span data-ttu-id="3665b-101">Arrays</span><span class="sxs-lookup"><span data-stu-id="3665b-101">Arrays</span></span>

<span data-ttu-id="3665b-102">Ein Array ist eine Datenstruktur, die eine Reihe von Variablen enthält, die über berechnete Indizes zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="3665b-102">An array is a data structure that contains a number of variables which are accessed through computed indices.</span></span> <span data-ttu-id="3665b-103">In einem Array, das die Elemente des Arrays, so genannte enthaltenen Variablen sind alle vom selben Typ, und diese Art wird den Elementtyp des Arrays bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="3665b-103">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="3665b-104">Ein Array verfügt über einen Rang, der die Anzahl der Indizes, die jedes Arrayelement zugeordnet bestimmt.</span><span class="sxs-lookup"><span data-stu-id="3665b-104">An array has a rank which determines the number of indices associated with each array element.</span></span> <span data-ttu-id="3665b-105">Der Rang eines Arrays ist auch als Dimensionen des Arrays bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="3665b-105">The rank of an array is also referred to as the dimensions of the array.</span></span> <span data-ttu-id="3665b-106">Ein Array mit dem Rang eins wird aufgerufen, eine ***eindimensionales Array***.</span><span class="sxs-lookup"><span data-stu-id="3665b-106">An array with a rank of one is called a ***single-dimensional array***.</span></span> <span data-ttu-id="3665b-107">Ein Array mit einem Rang größer als eine aufgerufen wird eine ***mehrdimensionales Array***.</span><span class="sxs-lookup"><span data-stu-id="3665b-107">An array with a rank greater than one is called a ***multi-dimensional array***.</span></span> <span data-ttu-id="3665b-108">Bestimmte Größe mehrdimensionale Arrays werden häufig als zweidimensionale Arrays, dreidimensionale Arrays usw. bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="3665b-108">Specific sized multi-dimensional arrays are often referred to as two-dimensional arrays, three-dimensional arrays, and so on.</span></span>

<span data-ttu-id="3665b-109">Jeder Dimension eines Arrays verfügt über eine zugeordnete Länge eine ganzzahlige Anzahl größer als oder gleich 0 (null) handelt.</span><span class="sxs-lookup"><span data-stu-id="3665b-109">Each dimension of an array has an associated length which is an integral number greater than or equal to zero.</span></span> <span data-ttu-id="3665b-110">Die Längen der Dimension sind nicht Teil des Typs des Arrays, aber eingerichtet, wenn eine Instanz des Arraytyps zur Laufzeit erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="3665b-110">The dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run-time.</span></span> <span data-ttu-id="3665b-111">Die Länge einer Dimension bestimmt des gültigen Bereichs von Indizes für diese Dimension: Für eine Dimension der Länge `N`, Indizes Prioritätswerte liegen zwischen `0` zu `N - 1` inklusive.</span><span class="sxs-lookup"><span data-stu-id="3665b-111">The length of a dimension determines the valid range of indices for that dimension: For a dimension of length `N`, indices can range from `0` to `N - 1` inclusive.</span></span> <span data-ttu-id="3665b-112">Die Gesamtanzahl der Elemente in einem Array ist das Produkt der Längen der in jeder Dimension im Array.</span><span class="sxs-lookup"><span data-stu-id="3665b-112">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="3665b-113">Wenn eine oder mehrere der Dimensionen eines Arrays eine Länge von 0 (null) haben, hört das Array leer sein.</span><span class="sxs-lookup"><span data-stu-id="3665b-113">If one or more of the dimensions of an array have a length of zero, the array is said to be empty.</span></span>

<span data-ttu-id="3665b-114">Ein Array kann einen beliebigen Elementtyp verwenden, einschließlich eines Arraytyps. </span><span class="sxs-lookup"><span data-stu-id="3665b-114">The element type of an array can be any type, including an array type.</span></span>

## <a name="array-types"></a><span data-ttu-id="3665b-115">Arraytypen</span><span class="sxs-lookup"><span data-stu-id="3665b-115">Array types</span></span>

<span data-ttu-id="3665b-116">Array-Typ wird geschrieben, als eine *Non_array_type* gefolgt von einem oder mehreren *Rank_specifier*s:</span><span class="sxs-lookup"><span data-stu-id="3665b-116">An array type is written as a *non_array_type* followed by one or more *rank_specifier*s:</span></span>

```antlr
array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;
```

<span data-ttu-id="3665b-117">Ein *Non_array_type* ist "any" *Typ* , nicht selbst eine *Array_type*.</span><span class="sxs-lookup"><span data-stu-id="3665b-117">A *non_array_type* is any *type* that is not itself an *array_type*.</span></span>

<span data-ttu-id="3665b-118">Der Rang eines Arraytyps wird angegeben, durch die am weitesten links stehende *Rank_specifier* in die *Array_type*: Ein *Rank_specifier* gibt an, dass das Array ein Array mit Rang eins plus die Anzahl der "`,`" Token in der *Rank_specifier*.</span><span class="sxs-lookup"><span data-stu-id="3665b-118">The rank of an array type is given by the leftmost *rank_specifier* in the *array_type*: A *rank_specifier* indicates that the array is an array with a rank of one plus the number of "`,`" tokens in the *rank_specifier*.</span></span>

<span data-ttu-id="3665b-119">Der Elementtyp des Arraytyps ist der Typ, der ergibt, der am weitesten links stehende löschen *Rank_specifier*:</span><span class="sxs-lookup"><span data-stu-id="3665b-119">The element type of an array type is the type that results from deleting the leftmost *rank_specifier*:</span></span>

*  <span data-ttu-id="3665b-120">Array-Typ des Formulars `T[R]` ist ein Array mit Rang `R` und einen nicht-Array-Elementtyp `T`.</span><span class="sxs-lookup"><span data-stu-id="3665b-120">An array type of the form `T[R]` is an array with rank `R` and a non-array element type `T`.</span></span>
*  <span data-ttu-id="3665b-121">Array-Typ des Formulars `T[R][R1]...[Rn]` ist ein Array mit Rang `R` und eines Elementtyps `T[R1]...[Rn]`.</span><span class="sxs-lookup"><span data-stu-id="3665b-121">An array type of the form `T[R][R1]...[Rn]` is an array with rank `R` and an element type `T[R1]...[Rn]`.</span></span>

<span data-ttu-id="3665b-122">Faktisch sind die *Rank_specifier*s werden von links nach rechts vor dem letzten Element der nicht-Array-Typ gelesen.</span><span class="sxs-lookup"><span data-stu-id="3665b-122">In effect, the *rank_specifier*s are read from left to right before the final non-array element type.</span></span> <span data-ttu-id="3665b-123">Der Typ `int[][,,][,]` ist ein eindimensionales Array von dreidimensionalen Arrays eines zweidimensionalen Arrays von `int`.</span><span class="sxs-lookup"><span data-stu-id="3665b-123">The type `int[][,,][,]` is a single-dimensional array of three-dimensional arrays of two-dimensional arrays of `int`.</span></span>

<span data-ttu-id="3665b-124">Zur Laufzeit, kann ein Wert, der ein Arraytyp sein `null` oder ein Verweis auf eine Instanz dieses Arraytyps.</span><span class="sxs-lookup"><span data-stu-id="3665b-124">At run-time, a value of an array type can be `null` or a reference to an instance of that array type.</span></span>

### <a name="the-systemarray-type"></a><span data-ttu-id="3665b-125">Des Typs System.Array</span><span class="sxs-lookup"><span data-stu-id="3665b-125">The System.Array type</span></span>

<span data-ttu-id="3665b-126">Der Typ `System.Array` ist der abstrakte Basistyp aller Typen von Arrays.</span><span class="sxs-lookup"><span data-stu-id="3665b-126">The type `System.Array` is the abstract base type of all array types.</span></span> <span data-ttu-id="3665b-127">Implizite verweiskonvertierung ([implizite Verweis-](conversions.md#implicit-reference-conversions)) vorhanden ist, über einen anderen Arraytyp aufweisen, `System.Array`, und eine explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-reference-conversions)) vorhanden ist von `System.Array` auf einen anderen Arraytyp aufweisen.</span><span class="sxs-lookup"><span data-stu-id="3665b-127">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from any array type to `System.Array`, and an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `System.Array` to any array type.</span></span> <span data-ttu-id="3665b-128">Beachten Sie, dass `System.Array` ist nicht selbst eine *Array_type*.</span><span class="sxs-lookup"><span data-stu-id="3665b-128">Note that `System.Array` is not itself an *array_type*.</span></span> <span data-ttu-id="3665b-129">Es handelt sich vielmehr eine *Class_type* von der alle *Array_type*s abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="3665b-129">Rather, it is a *class_type* from which all *array_type*s are derived.</span></span>

<span data-ttu-id="3665b-130">Zur Laufzeit, ein Wert vom Typ `System.Array` kann `null` oder ein Verweis auf eine Instanz eines beliebigen Arraytyps.</span><span class="sxs-lookup"><span data-stu-id="3665b-130">At run-time, a value of type `System.Array` can be `null` or a reference to an instance of any array type.</span></span>

### <a name="arrays-and-the-generic-ilist-interface"></a><span data-ttu-id="3665b-131">Arrays und die generische IList-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="3665b-131">Arrays and the generic IList interface</span></span>

<span data-ttu-id="3665b-132">Ein eindimensionales Array `T[]` implementiert die Schnittstelle `System.Collections.Generic.IList<T>` (`IList<T>` kurz) und die Basis-Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="3665b-132">A one-dimensional array `T[]` implements the interface `System.Collections.Generic.IList<T>` (`IList<T>` for short) and its base interfaces.</span></span> <span data-ttu-id="3665b-133">Daher besteht eine implizite Konvertierung von `T[]` zu `IList<T>` und die Basis-Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="3665b-133">Accordingly, there is an implicit conversion from `T[]` to `IList<T>` and its base interfaces.</span></span> <span data-ttu-id="3665b-134">Darüber hinaus liegt eine implizite verweiskonvertierung von `S` zu `T` dann `S[]` implementiert `IList<T>` und es gibt eine implizite verweiskonvertierung von `S[]` zu `IList<T>` und Basis-Schnittstellen ( [Implizite Verweis-](conversions.md#implicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="3665b-134">In addition, if there is an implicit reference conversion from `S` to `T` then `S[]` implements `IList<T>` and there is an implicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="3665b-135">Es ist eine explizite Konvertierung von `S` zu `T` dann gibt es eine explizite Konvertierung von `S[]` zu `IList<T>` und die Basisschnittstellen ([explizite Konvertierungen](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="3665b-135">If there is an explicit reference conversion from `S` to `T` then there is an explicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="3665b-136">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="3665b-136">For example:</span></span>
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

<span data-ttu-id="3665b-137">Die Zuweisung `lst2 = oa1` generiert einen Fehler während der Kompilierung, da die Konvertierung von `object[]` zu `IList<string>` ist eine explizite Konvertierung, die nicht implizit.</span><span class="sxs-lookup"><span data-stu-id="3665b-137">The assignment `lst2 = oa1` generates a compile-time error since the conversion from `object[]` to `IList<string>` is an explicit conversion, not implicit.</span></span> <span data-ttu-id="3665b-138">Die Umwandlung `(IList<string>)oa1` bewirkt, dass eine Ausnahme zur Laufzeit seit werden `oa1` Verweise ein `object[]` und keine `string[]`.</span><span class="sxs-lookup"><span data-stu-id="3665b-138">The cast `(IList<string>)oa1` will cause an exception to be thrown at run-time since `oa1` references an `object[]` and not a `string[]`.</span></span> <span data-ttu-id="3665b-139">Jedoch die Umwandlung `(IList<string>)oa2` führt nicht dazu, dass eine Ausnahme ausgelöst werden, da `oa2` Verweise ein `string[]`.</span><span class="sxs-lookup"><span data-stu-id="3665b-139">However the cast `(IList<string>)oa2` will not cause an exception to be thrown since `oa2` references a `string[]`.</span></span>

<span data-ttu-id="3665b-140">Jedes Mal, wenn es gibt eine implizite oder explizite verweiskonvertierung von `S[]` zu `IList<T>`, es gibt auch eine explizite Konvertierung von `IList<T>` und Basis-Schnittstellen für `S[]` ([expliziten Verweis Konvertierungen](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="3665b-140">Whenever there is an implicit or explicit reference conversion from `S[]` to `IList<T>`, there is also an explicit reference conversion from `IList<T>` and its base interfaces to `S[]` ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span>

<span data-ttu-id="3665b-141">Wenn ein Arraytyp `S[]` implementiert `IList<T>`, einige der Member der implementierten Schnittstelle kann Ausnahmen auslösen.</span><span class="sxs-lookup"><span data-stu-id="3665b-141">When an array type `S[]` implements `IList<T>`, some of the members of the implemented interface may throw exceptions.</span></span> <span data-ttu-id="3665b-142">Das genaue Verhalten der Implementierung der Schnittstelle ist nicht Gegenstand dieser Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="3665b-142">The precise behavior of the implementation of the interface is beyond the scope of this specification.</span></span>

## <a name="array-creation"></a><span data-ttu-id="3665b-143">Arrayerstellung</span><span class="sxs-lookup"><span data-stu-id="3665b-143">Array creation</span></span>

<span data-ttu-id="3665b-144">Arrayinstanzen werden erstellt, indem *Array_creation_expression*s ([Array-Ausdrücke für die Arrayerstellung](expressions.md#array-creation-expressions)) oder durch das Feld oder eine lokale Variablendeklarationen, die enthalten eine *Array_initializer*([Array Initializers](arrays.md#array-initializers)).</span><span class="sxs-lookup"><span data-stu-id="3665b-144">Array instances are created by *array_creation_expression*s ([Array creation expressions](expressions.md#array-creation-expressions)) or by field or local variable declarations that include an *array_initializer* ([Array initializers](arrays.md#array-initializers)).</span></span>

<span data-ttu-id="3665b-145">Wenn eine Arrayinstanz erstellt wird, ändern Sie den Rang und die-Länge der einzelnen Dimensionen werden festgelegt, und Sie dann konstant bleiben, während der gesamten Lebensdauer der Instanz.</span><span class="sxs-lookup"><span data-stu-id="3665b-145">When an array instance is created, the rank and length of each dimension are established and then remain constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="3665b-146">Das heißt, es ist nicht möglich, den Rang einer vorhandenen Instanz des Arrays zu ändern, noch ist es möglich, die Größe seiner Dimensionen ändern.</span><span class="sxs-lookup"><span data-stu-id="3665b-146">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

<span data-ttu-id="3665b-147">Es ist immer eine Arrayinstanz eines Arraytyps.</span><span class="sxs-lookup"><span data-stu-id="3665b-147">An array instance is always of an array type.</span></span> <span data-ttu-id="3665b-148">Die `System.Array` Typ ist ein abstrakter Typ, der nicht instanziiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="3665b-148">The `System.Array` type is an abstract type that cannot be instantiated.</span></span>

<span data-ttu-id="3665b-149">Elemente des Arrays, die von erstellten *Array_creation_expression*s werden immer auf ihren Standardwert initialisiert ([Standardwerte](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="3665b-149">Elements of arrays created by *array_creation_expression*s are always initialized to their default value ([Default values](variables.md#default-values)).</span></span>

## <a name="array-element-access"></a><span data-ttu-id="3665b-150">Array Element access</span><span class="sxs-lookup"><span data-stu-id="3665b-150">Array element access</span></span>

<span data-ttu-id="3665b-151">Elemente des Arrays erfolgt mit *Element_access* Ausdrücke ([Array Zugriff](expressions.md#array-access)) des Formulars `A[I1, I2, ..., In]`, wobei `A` ist ein Ausdruck vom Array-Typ und den einzelnen `Ix` ist ein Ein Ausdruck vom Typ `int`, `uint`, `long`, `ulong`, oder an eine oder mehrere der folgenden Typen implizit konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="3665b-151">Array elements are accessed using *element_access* expressions ([Array access](expressions.md#array-access)) of the form `A[I1, I2, ..., In]`, where `A` is an expression of an array type and each `Ix` is an expression of type `int`, `uint`, `long`, `ulong`, or can be implicitly converted to one or more of these types.</span></span> <span data-ttu-id="3665b-152">Das Ergebnis ein Array Element Access ist eine Variable, nämlich dem Array-Element, das von den Indizes ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="3665b-152">The result of an array element access is a variable, namely the array element selected by the indices.</span></span>

<span data-ttu-id="3665b-153">Die Elemente eines Arrays aufgelistet werden können, mithilfe einer `foreach` Anweisung ([der Foreach-Anweisung](statements.md#the-foreach-statement)).</span><span class="sxs-lookup"><span data-stu-id="3665b-153">The elements of an array can be enumerated using a `foreach` statement ([The foreach statement](statements.md#the-foreach-statement)).</span></span>

## <a name="array-members"></a><span data-ttu-id="3665b-154">Array-Elemente</span><span class="sxs-lookup"><span data-stu-id="3665b-154">Array members</span></span>

<span data-ttu-id="3665b-155">Jedes Array-Typ erbt die Member deklariert, indem die `System.Array` Typ.</span><span class="sxs-lookup"><span data-stu-id="3665b-155">Every array type inherits the members declared by the `System.Array` type.</span></span>

## <a name="array-covariance"></a><span data-ttu-id="3665b-156">Array-Kovarianz</span><span class="sxs-lookup"><span data-stu-id="3665b-156">Array covariance</span></span>

<span data-ttu-id="3665b-157">Für zwei *Reference_type*s `A` und `B`, wenn implizite verweiskonvertierung ([implizite Verweis-](conversions.md#implicit-reference-conversions)) oder explizite Konvertierung ([ Explizite Konvertierungen](conversions.md#explicit-reference-conversions)) vorhanden ist, von `A` zu `B`, und klicken Sie dann auch die gleichen verweiskonvertierung aus dem Arraytyp vorhanden ist `A[R]` in den Arraytyp `B[R]`, wobei `R` ist "any" bestimmte *Rank_specifier* (aber sowohl für Arraytypen).</span><span class="sxs-lookup"><span data-stu-id="3665b-157">For any two *reference_type*s `A` and `B`, if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `A` to `B`, then the same reference conversion also exists from the array type `A[R]` to the array type `B[R]`, where `R` is any given *rank_specifier* (but the same for both array types).</span></span> <span data-ttu-id="3665b-158">Diese Beziehung wird als bezeichnet ***Array-Kovarianz***.</span><span class="sxs-lookup"><span data-stu-id="3665b-158">This relationship is known as ***array covariance***.</span></span> <span data-ttu-id="3665b-159">Array-Kovarianz vor allem bedeutet, dass einen Wert eines Arraytyps `A[R]` möglicherweise tatsächlich ein Verweis auf eine Instanz des Arraytyps `B[R]`, sofern eine implizite verweiskonvertierung von vorhanden `B` zu `A`.</span><span class="sxs-lookup"><span data-stu-id="3665b-159">Array covariance in particular means that a value of an array type `A[R]` may actually be a reference to an instance of an array type `B[R]`, provided an implicit reference conversion exists from `B` to `A`.</span></span>

<span data-ttu-id="3665b-160">Aufgrund der Array-Kovarianz enthalten Zuweisungen auf Elemente des Verweistyparrays eine laufzeitüberprüfung wird sichergestellt, dass der Wert zugewiesen wird, auf das Arrayelement tatsächlich einen zulässigen Typ aufweist ([einfache Zuweisung](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="3665b-160">Because of array covariance, assignments to elements of reference type arrays include a run-time check which ensures that the value being assigned to the array element is actually of a permitted type ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="3665b-161">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="3665b-161">For example:</span></span>
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

<span data-ttu-id="3665b-162">Die Zuweisung zu `array[i]` in die `Fill` Methode enthält implizit eine laufzeitüberprüfung das Objekt, das auf stellt sicher, dass `value` ist entweder `null` oder eine Instanz, die kompatibel mit dem tatsächlichen Element `array`.</span><span class="sxs-lookup"><span data-stu-id="3665b-162">The assignment to `array[i]` in the `Fill` method implicitly includes a run-time check which ensures that the object referenced by `value` is either `null` or an instance that is compatible with the actual element type of `array`.</span></span> <span data-ttu-id="3665b-163">In `Main`, die ersten beiden Aufrufe der `Fill` erfolgreich, aber der dritte Aufruf bewirkt, dass eine `System.ArrayTypeMismatchException` ausgelöst wird, bei der Ausführung der ersten Zuweisung zu `array[i]`.</span><span class="sxs-lookup"><span data-stu-id="3665b-163">In `Main`, the first two invocations of `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` to be thrown upon executing the first assignment to `array[i]`.</span></span> <span data-ttu-id="3665b-164">Die Ausnahme tritt auf, weil eine geschachtelte `int` kann nicht gespeichert werden, einem `string` Array.</span><span class="sxs-lookup"><span data-stu-id="3665b-164">The exception occurs because a boxed `int` cannot be stored in a `string` array.</span></span>

<span data-ttu-id="3665b-165">Array-Kovarianz speziell erstreckt sich nicht um Arrays von *Value_type*s.</span><span class="sxs-lookup"><span data-stu-id="3665b-165">Array covariance specifically does not extend to arrays of *value_type*s.</span></span> <span data-ttu-id="3665b-166">Beispielsweise keine Konvertierung möglich ist, ermöglicht eine `int[]` behandelt werden soll, als ein `object[]`.</span><span class="sxs-lookup"><span data-stu-id="3665b-166">For example, no conversion exists that permits an `int[]` to be treated as an `object[]`.</span></span>

## <a name="array-initializers"></a><span data-ttu-id="3665b-167">Arrayinitialisierer</span><span class="sxs-lookup"><span data-stu-id="3665b-167">Array initializers</span></span>

<span data-ttu-id="3665b-168">Arrayinitialisierer können angegeben werden, in die Steuerelementfeld-Deklarationen ([Felder](classes.md#fields)), lokale Variablendeklarationen ([lokale Variablendeklarationen](statements.md#local-variable-declarations)), und Erstellen von Ausdrücken ([Array erstellen Ausdrücke](expressions.md#array-creation-expressions)):</span><span class="sxs-lookup"><span data-stu-id="3665b-168">Array initializers may be specified in field declarations ([Fields](classes.md#fields)), local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), and array creation expressions ([Array creation expressions](expressions.md#array-creation-expressions)):</span></span>

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="3665b-169">Ein Arrayinitialisierer besteht aus einer Sequenz von Variableninitialisierern, eingeschlossen durch "`{`"und"`}`"Token und getrennt durch"`,`" Token.</span><span class="sxs-lookup"><span data-stu-id="3665b-169">An array initializer consists of a sequence of variable initializers, enclosed by "`{`" and "`}`" tokens and separated by "`,`" tokens.</span></span> <span data-ttu-id="3665b-170">Jede Variableninitialisierer ist ein Ausdruck oder, im Fall eines mehrdimensionalen Arrays, ein geschachtelter Arrayinitialisierer.</span><span class="sxs-lookup"><span data-stu-id="3665b-170">Each variable initializer is an expression or, in the case of a multi-dimensional array, a nested array initializer.</span></span>

<span data-ttu-id="3665b-171">Der Kontext, in dem ein Arrayinitialisierer verwendet wird, bestimmt den Typ des Arrays initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="3665b-171">The context in which an array initializer is used determines the type of the array being initialized.</span></span> <span data-ttu-id="3665b-172">In einem Arrayerstellungsausdruck das Array vom Typ sofort steht die Initialisierung oder von die Ausdrücke im Arrayinitialisierer für abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="3665b-172">In an array creation expression, the array type immediately precedes the initializer, or is inferred from the expressions in the array initializer.</span></span> <span data-ttu-id="3665b-173">In einem Feld oder die Deklaration von Variablen ist der Arraytyp den Typ des Felds oder der Variable deklariert wird.</span><span class="sxs-lookup"><span data-stu-id="3665b-173">In a field or variable declaration, the array type is the type of the field or variable being declared.</span></span> <span data-ttu-id="3665b-174">Wenn ein Arrayinitialisierer in einem Feld oder die Deklaration von Variablen, wie z. B. verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="3665b-174">When an array initializer is used in a field or variable declaration, such as:</span></span>
```csharp
int[] a = {0, 2, 4, 6, 8};
```
<span data-ttu-id="3665b-175">Es ist einfach die Kurzschreibweise für einen entsprechenden Ausdruck für die Arrayerstellung:</span><span class="sxs-lookup"><span data-stu-id="3665b-175">it is simply shorthand for an equivalent array creation expression:</span></span>
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

<span data-ttu-id="3665b-176">Für ein eindimensionales Array sind muss des Arrayinitialisierers einer Sequenz von Ausdrücken bestehen, die Zuweisung, die mit dem Elementtyp des Arrays kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="3665b-176">For a single-dimensional array, the array initializer must consist of a sequence of expressions that are assignment compatible with the element type of the array.</span></span> <span data-ttu-id="3665b-177">Die Ausdrücke in aufsteigender Reihenfolge, beginnend mit dem Element am Index 0 (null) Elemente des Arrays initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="3665b-177">The expressions initialize array elements in increasing order, starting with the element at index zero.</span></span> <span data-ttu-id="3665b-178">Die Anzahl der Ausdrücke im Arrayinitialisierer für bestimmt die Länge der Arrayinstanz erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="3665b-178">The number of expressions in the array initializer determines the length of the array instance being created.</span></span> <span data-ttu-id="3665b-179">Die oben genannten Arrayinitialisierer erstellt z. B. eine `int[]` Instanz der Länge 5, und klicken Sie dann initialisiert die Instanz mit den folgenden Werten:</span><span class="sxs-lookup"><span data-stu-id="3665b-179">For example, the array initializer above creates an `int[]` instance of length 5 and then initializes the instance with the following values:</span></span>
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

<span data-ttu-id="3665b-180">Für ein mehrdimensionales Array ist müssen die Arrayinitialisierer beliebig viele Ebenen von geschachtelt werden, wenn die Dimensionen im Array vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="3665b-180">For a multi-dimensional array, the array initializer must have as many levels of nesting as there are dimensions in the array.</span></span> <span data-ttu-id="3665b-181">Die äußerste Schachtelungsebene der am weitesten links stehende Dimension entspricht, und die innerste Schachtelungsebene entspricht die Dimension ganz rechts.</span><span class="sxs-lookup"><span data-stu-id="3665b-181">The outermost nesting level corresponds to the leftmost dimension and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="3665b-182">Die Länge der einzelnen Dimensionen des Arrays wird durch die Anzahl der Elemente auf der entsprechenden Schachtelungsebene im Arrayinitialisierer bestimmt.</span><span class="sxs-lookup"><span data-stu-id="3665b-182">The length of each dimension of the array is determined by the number of elements at the corresponding nesting level in the array initializer.</span></span> <span data-ttu-id="3665b-183">Für jede geschachtelter Arrayinitialisierer muss die Anzahl von Elementen identisch mit den anderen Arrayinitialisierer auf derselben Ebene.</span><span class="sxs-lookup"><span data-stu-id="3665b-183">For each nested array initializer, the number of elements must be the same as the other array initializers at the same level.</span></span> <span data-ttu-id="3665b-184">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="3665b-184">The example:</span></span>
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
<span data-ttu-id="3665b-185">erstellt ein zweidimensionales Array mit einer Länge von fünf der am weitesten links stehende Dimension und einer Länge von zwei für die Dimension ganz rechts:</span><span class="sxs-lookup"><span data-stu-id="3665b-185">creates a two-dimensional array with a length of five for the leftmost dimension and a length of two for the rightmost dimension:</span></span>
```csharp
int[,] b = new int[5, 2];
```
<span data-ttu-id="3665b-186">und initialisiert dann das Array Instanz mit den folgenden Werten:</span><span class="sxs-lookup"><span data-stu-id="3665b-186">and then initializes the array instance with the following values:</span></span>
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

<span data-ttu-id="3665b-187">Wenn eine andere Dimension als am weitesten rechts mit der Länge 0 (null) angegeben ist, werden die nachfolgenden Dimensionen als auch die Länge 0 (null) haben.</span><span class="sxs-lookup"><span data-stu-id="3665b-187">If a dimension other than the rightmost is given with length zero, the subsequent dimensions are assumed to also have length zero.</span></span> <span data-ttu-id="3665b-188">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="3665b-188">The example:</span></span>
```csharp
int[,] c = {};
```
<span data-ttu-id="3665b-189">erstellt ein zweidimensionales Array mit einer Länge von 0 (null), die am weitesten links stehende sowohl für die Dimension ganz rechts:</span><span class="sxs-lookup"><span data-stu-id="3665b-189">creates a two-dimensional array with a length of zero for both the leftmost and the rightmost dimension:</span></span>
```csharp
int[,] c = new int[0, 0];
```

<span data-ttu-id="3665b-190">Wenn einem Arrayerstellungsausdruck sowohl die Längen explizite Dimension als auch ein Arrayinitialisierer enthält, die Längen müssen Konstante Ausdrücke sein, und die Anzahl der Elemente auf jeder Schachtelungsebene muss die entsprechende Dimensionslänge entsprechen.</span><span class="sxs-lookup"><span data-stu-id="3665b-190">When an array creation expression includes both explicit dimension lengths and an array initializer, the lengths must be constant expressions and the number of elements at each nesting level must match the corresponding dimension length.</span></span> <span data-ttu-id="3665b-191">Hier einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="3665b-191">Here are some examples:</span></span>
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

<span data-ttu-id="3665b-192">Hier können die Initialisierung für `y` führt zu einem Kompilierzeitfehler, da es sich bei der Dimension Länge-Ausdruck ist eine Konstante und den Initialisierer für `z` führt zu einem Kompilierzeitfehler, da die Länge und die Anzahl der Elemente in der Initialisierer stimmen nicht überein.</span><span class="sxs-lookup"><span data-stu-id="3665b-192">Here, the initializer for `y` results in a compile-time error because the dimension length expression is not a constant, and the initializer for `z` results in a compile-time error because the length and the number of elements in the initializer do not agree.</span></span>
