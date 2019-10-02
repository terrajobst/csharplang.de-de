---
ms.openlocfilehash: 4faef9a12bdff54fa59a55a0206fa72bda4ea585
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704062"
---
# <a name="unsafe-code"></a><span data-ttu-id="79d04-101">Unsicherer Code</span><span class="sxs-lookup"><span data-stu-id="79d04-101">Unsafe code</span></span>

<span data-ttu-id="79d04-102">Die in C# den vorstehenden Kapiteln definierte Kernsprache unterscheidet sich insbesondere von C und C++ durch das Weglassen von Zeigern als Datentyp.</span><span class="sxs-lookup"><span data-stu-id="79d04-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="79d04-103">Stattdessen C# bietet Verweise und die Möglichkeit, Objekte zu erstellen, die von einem Garbage Collector verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="79d04-104">Dieser Entwurf stellt C# in Verbindung mit anderen Features eine viel sicherere Sprache als C oder C++dar.</span><span class="sxs-lookup"><span data-stu-id="79d04-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="79d04-105">In der Kern C# Sprache ist es einfach nicht möglich, eine nicht initialisierte Variable, einen "Verb leibend"-Zeiger oder einen Ausdruck zu haben, der ein Array über seine Begrenzungen hinaus indiziert.</span><span class="sxs-lookup"><span data-stu-id="79d04-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="79d04-106">Alle Fehlerkategorien, die routinemäßig C und C++ Programme Plagen, werden daher vermieden.</span><span class="sxs-lookup"><span data-stu-id="79d04-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="79d04-107">Obwohl praktisch jedes Zeigertyp Konstrukt in C C++ oder ein Verweistyp in der C#Entsprechung in ist, gibt es jedoch Situationen, in denen der Zugriff auf Zeiger Typen zu einer Notwendigkeit wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="79d04-108">Beispielsweise ist die Schnittstellen mit dem zugrunde liegenden Betriebssystem, der Zugriff auf ein Speicher Abbild oder die Implementierung eines zeitkritischen Algorithmus möglicherweise ohne Zugriff auf Zeiger nicht möglich oder praktikabel.</span><span class="sxs-lookup"><span data-stu-id="79d04-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="79d04-109">Um diese Anforderung zu erfüllen C# , bietet die Möglichkeit, ***unsicheren Code***zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="79d04-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="79d04-110">Im unsicheren Code ist es möglich, Zeiger zu deklarieren und zu verarbeiten, Konvertierungen zwischen Zeigern und ganzzahligen Typen auszuführen, die Adresse von Variablen zu übernehmen usw.</span><span class="sxs-lookup"><span data-stu-id="79d04-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="79d04-111">In gewisser Weise ist das Schreiben von unsicherem Code ähnlich wie das Schreiben C# von C-Code in einem Programm.</span><span class="sxs-lookup"><span data-stu-id="79d04-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="79d04-112">Unsicherer Code ist tatsächlich eine "sichere" Funktion aus der Perspektive von Entwicklern und Benutzern.</span><span class="sxs-lookup"><span data-stu-id="79d04-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="79d04-113">Unsicherer Code muss eindeutig mit dem-Modifizierer `unsafe` gekennzeichnet werden, sodass Entwickler nicht möglicherweise unsichere Funktionen versehentlich verwenden können, und die Ausführungs-Engine kann sicherstellen, dass unsicherer Code nicht in einer nicht vertrauenswürdigen Umgebung ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="79d04-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="79d04-114">Unsichere Kontexte</span><span class="sxs-lookup"><span data-stu-id="79d04-114">Unsafe contexts</span></span>

<span data-ttu-id="79d04-115">Die unsicheren Funktionen von C# sind nur in unsicheren Kontexten verfügbar.</span><span class="sxs-lookup"><span data-stu-id="79d04-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="79d04-116">Ein unsicherer Kontext wird durch das Einschließen eines `unsafe`-Modifizierers in die Deklaration eines Typs oder Members oder durch die Verwendung eines *unsafe_statement*-Elements eingeführt:</span><span class="sxs-lookup"><span data-stu-id="79d04-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="79d04-117">Eine Deklaration einer Klasse, Struktur, Schnittstelle oder eines Delegaten kann einen `unsafe`-Modifizierer enthalten. in diesem Fall wird der gesamte Text Block dieser Typdeklaration (einschließlich des Texts der Klasse, Struktur oder Schnittstelle) als unsicherer Kontext betrachtet.</span><span class="sxs-lookup"><span data-stu-id="79d04-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="79d04-118">Eine Deklaration eines Felds, einer Methode, einer Eigenschaft, eines Ereignisses, eines Indexers, eines Operators, eines Instanzkonstruktors, eines Dekonstruktors oder eines statischen Konstruktors kann einen `unsafe`-Modifizierer enthalten. in diesem Fall wird der gesamte Text Block der Element Deklaration als unsicherer Kontext betrachtet.</span><span class="sxs-lookup"><span data-stu-id="79d04-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="79d04-119">Ein *unsafe_statement* ermöglicht die Verwendung eines unsicheren Kontexts innerhalb eines- *Blocks*.</span><span class="sxs-lookup"><span data-stu-id="79d04-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="79d04-120">Der gesamte Text Block des zugeordneten *Blocks* wird als unsicherer Kontext betrachtet.</span><span class="sxs-lookup"><span data-stu-id="79d04-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="79d04-121">Die zugehörigen Grammatik Produktionen sind unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="79d04-121">The associated grammar productions are shown below.</span></span>

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

<span data-ttu-id="79d04-122">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="79d04-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="79d04-123">der in der Struktur Deklaration angegebene `unsafe`-Modifizierer bewirkt, dass der gesamte Text Block der Struktur Deklaration zu einem unsicheren Kontext wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="79d04-124">Daher ist es möglich, die Felder "`Left`" und "`Right`" als Zeigertyp zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="79d04-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="79d04-125">Das obige Beispiel könnte auch geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="79d04-126">Hier bewirken die `unsafe`-Modifizierer in den Feld Deklarationen, dass diese Deklarationen als unsichere Kontexte angesehen werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="79d04-127">Abgesehen von der Einrichtung eines unsicheren Kontexts, der die Verwendung von Zeiger Typen ermöglicht, hat der `unsafe`-Modifizierer keine Auswirkung auf einen Typ oder einen Member.</span><span class="sxs-lookup"><span data-stu-id="79d04-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="79d04-128">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="79d04-128">In the example</span></span>

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

<span data-ttu-id="79d04-129">der `unsafe`-Modifizierer für die `F`-Methode in `A` bewirkt, dass der Text Block von `F` zu einem unsicheren Kontext wird, in dem die unsicheren Funktionen der Sprache verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="79d04-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="79d04-130">Bei der außer Kraft Setzung von `F` in `B` ist es nicht notwendig, den `unsafe`-Modifizierer erneut anzugeben, es sei denn, die `F`-Methode in `B` selbst benötigt Zugriff auf unsichere Features.</span><span class="sxs-lookup"><span data-stu-id="79d04-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="79d04-131">Die Situation ist etwas anders, wenn ein Zeigertyp Teil der Methoden Signatur ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

<span data-ttu-id="79d04-132">Da die Signatur von `F` einen Zeigertyp enthält, kann Sie nur in einem unsicheren Kontext geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="79d04-133">Allerdings kann der unsichere Kontext eingeführt werden, indem entweder die gesamte Klasse unsicher gemacht wird, wie es bei `A` der Fall ist, oder indem ein `unsafe`-Modifizierer in die Methoden Deklaration eingeschlossen wird, wie es in `B` der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="79d04-134">Zeigertypen</span><span class="sxs-lookup"><span data-stu-id="79d04-134">Pointer types</span></span>

<span data-ttu-id="79d04-135">In einem unsicheren Kontext kann ein *Typ* ([types](types.md)) ein *pointer_type* sowie ein *value_type* oder ein *reference_type*sein.</span><span class="sxs-lookup"><span data-stu-id="79d04-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="79d04-136">Ein *pointer_type* kann jedoch auch in einem `typeof`-Ausdruck ([Anonyme Objekt Erstellungs Ausdrücke](expressions.md#anonymous-object-creation-expressions)) außerhalb eines unsicheren Kontexts verwendet werden, da diese Verwendung nicht unsicher ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="79d04-137">Ein *pointer_type* -Wert wird als *unmanaged_type* oder das-Schlüsselwort `void`, gefolgt von einem `*`-Token geschrieben:</span><span class="sxs-lookup"><span data-stu-id="79d04-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="79d04-138">Der Typ, der vor dem `*` in einem Zeigertyp angegeben wird, wird als ***Verweistyp*** des Zeiger Typs bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="79d04-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="79d04-139">Sie stellt den Typ der Variablen dar, auf die ein Wert des Zeiger Typs zeigt.</span><span class="sxs-lookup"><span data-stu-id="79d04-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="79d04-140">Anders als Verweise (Werte von Verweis Typen) werden Zeiger nicht vom Garbage Collector nachverfolgt, und der Garbage Collector hat keine Informationen über Zeiger und die Daten, auf die Sie zeigen.</span><span class="sxs-lookup"><span data-stu-id="79d04-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="79d04-141">Aus diesem Grund kann ein Zeiger nicht auf einen Verweis oder eine Struktur verweisen, die Verweise enthält, und der Verweistyp eines Zeigers muss ein *unmanaged_type*sein.</span><span class="sxs-lookup"><span data-stu-id="79d04-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="79d04-142">Ein *unmanaged_type* ist ein beliebiger Typ, der kein *reference_type* oder konstruierter Typ ist und keine *reference_type* -oder konstruierten Typfelder auf einer beliebigen Schachtelungs Ebene enthält.</span><span class="sxs-lookup"><span data-stu-id="79d04-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="79d04-143">Mit anderen Worten: ein *unmanaged_type* -Wert ist einer der folgenden:</span><span class="sxs-lookup"><span data-stu-id="79d04-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="79d04-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0, 1 oder 2.</span><span class="sxs-lookup"><span data-stu-id="79d04-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="79d04-145">Alle *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="79d04-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="79d04-146">Alle *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="79d04-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="79d04-147">Alle benutzerdefinierten *struct_type* , bei denen es sich nicht um einen konstruierten Typ handelt und die nur die Felder *unmanaged_type*s enthalten.</span><span class="sxs-lookup"><span data-stu-id="79d04-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="79d04-148">Die intuitive Regel zum Mischen von Zeigern und verweisen ist, dass Verweise von verweisen (Objekten) Zeiger enthalten dürfen, aber Verweise von Zeigern dürfen keine Verweise enthalten.</span><span class="sxs-lookup"><span data-stu-id="79d04-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="79d04-149">In der folgenden Tabelle sind einige Beispiele für Zeiger Typen angegeben:</span><span class="sxs-lookup"><span data-stu-id="79d04-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="79d04-150">__Beispiel__</span><span class="sxs-lookup"><span data-stu-id="79d04-150">__Example__</span></span> | <span data-ttu-id="79d04-151">__Beschreibung__</span><span class="sxs-lookup"><span data-stu-id="79d04-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="79d04-152">Zeiger auf `byte`</span><span class="sxs-lookup"><span data-stu-id="79d04-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="79d04-153">Zeiger auf `char`</span><span class="sxs-lookup"><span data-stu-id="79d04-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="79d04-154">Zeiger auf den Zeiger auf `int`</span><span class="sxs-lookup"><span data-stu-id="79d04-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="79d04-155">Eindimensionales Array von Zeigern auf `int`</span><span class="sxs-lookup"><span data-stu-id="79d04-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="79d04-156">Zeiger auf unbekannten Typ</span><span class="sxs-lookup"><span data-stu-id="79d04-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="79d04-157">Für eine bestimmte Implementierung müssen alle Zeiger Typen die gleiche Größe und Darstellung aufweisen.</span><span class="sxs-lookup"><span data-stu-id="79d04-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="79d04-158">Im Gegensatz zu C++C und wird in C# der `*` zusammen mit dem zugrunde liegenden Typ und nicht als Präfix Satzzeichen für jeden Zeiger Namen geschrieben, wenn mehrere Zeiger in der gleichen Deklaration deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="79d04-159">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="79d04-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="79d04-160">Der Wert eines Zeigers mit dem Typ "`T*`" stellt die Adresse einer Variablen vom Typ "`T`" dar.</span><span class="sxs-lookup"><span data-stu-id="79d04-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="79d04-161">Der Zeiger Dereferenzierungsoperator `*` ([Zeigerdereferenzierung](unsafe-code.md#pointer-indirection)) kann verwendet werden, um auf diese Variable zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="79d04-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="79d04-162">Wenn z. b. eine Variable `P` vom Typ `int*` ist, kennzeichnet der Ausdruck `*P` die `int`-Variable, die sich an der in `P` enthaltenen Adresse befindet.</span><span class="sxs-lookup"><span data-stu-id="79d04-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="79d04-163">Ein Zeiger kann wie ein Objekt Verweis `null` sein.</span><span class="sxs-lookup"><span data-stu-id="79d04-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="79d04-164">Das Anwenden des Dereferenzierungsoperators auf einen `null`-Zeiger führt zu einem von der Implementierung definierten Verhalten.</span><span class="sxs-lookup"><span data-stu-id="79d04-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="79d04-165">Ein Zeiger mit dem Wert "`null`" wird durch "All-Bits-Zero" dargestellt.</span><span class="sxs-lookup"><span data-stu-id="79d04-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="79d04-166">Der `void*`-Typ stellt einen Zeiger auf einen unbekannten Typ dar.</span><span class="sxs-lookup"><span data-stu-id="79d04-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="79d04-167">Da der Verweistyp unbekannt ist, kann der Dereferenzierungsoperator nicht auf einen Zeiger vom Typ "`void*`" angewendet werden, und es können keine Arithmetik für einen solchen Zeiger ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="79d04-168">Ein Zeiger vom Typ "`void*`" kann jedoch in einen anderen Zeigertyp (und umgekehrt) umgewandelt werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="79d04-169">Zeiger Typen sind eine separate Kategorie von Typen.</span><span class="sxs-lookup"><span data-stu-id="79d04-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="79d04-170">Anders als bei Verweis Typen und Werttypen erben Zeiger Typen nicht von `object` und es sind keine Konvertierungen zwischen Zeiger Typen und `object` vorhanden.</span><span class="sxs-lookup"><span data-stu-id="79d04-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="79d04-171">Vor allem werden Boxing und Unboxing ([Boxing und Unboxing](types.md#boxing-and-unboxing)) für Zeiger nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="79d04-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="79d04-172">Allerdings sind Konvertierungen zwischen verschiedenen Zeiger Typen sowie zwischen Zeiger Typen und ganzzahligen Typen zulässig.</span><span class="sxs-lookup"><span data-stu-id="79d04-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="79d04-173">Dies wird in [Zeiger Konvertierungen](unsafe-code.md#pointer-conversions)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="79d04-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="79d04-174">Ein *pointer_type* kann nicht als Typargument ([konstruierte Typen](types.md#constructed-types)) verwendet werden, und der Typrückschluss ([Typrückschluss](expressions.md#type-inference)) schlägt bei generischen Methoden aufrufen fehl, die ein Typargument als Zeigertyp ableiten müssten.</span><span class="sxs-lookup"><span data-stu-id="79d04-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="79d04-175">Ein *pointer_type* kann als Typ eines flüchtigen Felds ([flüchtige Felder](classes.md#volatile-fields)) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="79d04-176">Obwohl Zeiger als `ref`-oder `out`-Parameter weitergegeben werden können, kann dies zu undefiniertem Verhalten führen, da der Zeiger möglicherweise so festgelegt wird, dass er auf eine lokale Variable verweist, die nicht mehr vorhanden ist, wenn die aufgerufene Methode zurückgibt, oder das fixierte Objekt, auf das verweist. , ist nicht mehr korrigiert.</span><span class="sxs-lookup"><span data-stu-id="79d04-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="79d04-177">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="79d04-177">For example:</span></span>

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

<span data-ttu-id="79d04-178">Eine Methode kann einen Wert eines Typs zurückgeben, und dieser Typ kann ein Zeiger sein.</span><span class="sxs-lookup"><span data-stu-id="79d04-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="79d04-179">Wenn beispielsweise ein Zeiger auf eine zusammenhängende Sequenz von `int`s, der Element Anzahl der Sequenz und einem anderen `int`-Wert angegeben wird, gibt die folgende Methode die Adresse dieses Werts in dieser Sequenz zurück, wenn eine Entsprechung auftritt. Andernfalls wird `null` zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="79d04-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

<span data-ttu-id="79d04-180">In einem unsicheren Kontext sind mehrere-Konstrukte zum Ausführen von Zeigern verfügbar:</span><span class="sxs-lookup"><span data-stu-id="79d04-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="79d04-181">Der `*`-Operator kann verwendet werden, um eine Zeigerdereferenzierung auszuführen ([Zeigerdereferenzierung](unsafe-code.md#pointer-indirection)).</span><span class="sxs-lookup"><span data-stu-id="79d04-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="79d04-182">Der `->`-Operator kann verwendet werden, um auf einen Member einer Struktur über einen Zeiger ([Zeiger Element Zugriff](unsafe-code.md#pointer-member-access)) zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="79d04-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="79d04-183">Der `[]`-Operator kann verwendet werden, um einen Zeiger zu indizieren ([Zeiger Element Zugriff](unsafe-code.md#pointer-element-access)).</span><span class="sxs-lookup"><span data-stu-id="79d04-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="79d04-184">Der `&`-Operator kann verwendet werden, um die Adresse einer Variablen ([dem address-of-Operator](unsafe-code.md#the-address-of-operator)) abzurufen.</span><span class="sxs-lookup"><span data-stu-id="79d04-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="79d04-185">Mit den Operatoren "`++`" und "`--`" können Zeiger erhöht und dekrementiert werden ([Zeiger Inkrement und Dekrement](unsafe-code.md#pointer-increment-and-decrement)).</span><span class="sxs-lookup"><span data-stu-id="79d04-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="79d04-186">Die Operatoren "`+`" und "`-`" können verwendet werden, um Zeigerarithmetik ([Zeigerarithmetik](unsafe-code.md#pointer-arithmetic)) auszuführen.</span><span class="sxs-lookup"><span data-stu-id="79d04-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="79d04-187">Der `==`-, `!=`-, `<`-, `>`-, `<=`-und `=>`-Operatoren können verwendet werden, um Zeiger zu vergleichen ([Zeiger Vergleiche](unsafe-code.md#pointer-comparison)).</span><span class="sxs-lookup"><span data-stu-id="79d04-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="79d04-188">Der `stackalloc`-Operator kann verwendet werden, um Speicher aus der aufrufsstapel zuzuordnen ([Puffer fester Größe](unsafe-code.md#fixed-size-buffers)).</span><span class="sxs-lookup"><span data-stu-id="79d04-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="79d04-189">Die `fixed`-Anweisung kann verwendet werden, um eine Variable temporär zu korrigieren, sodass Ihre Adresse abgerufen werden kann ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)).</span><span class="sxs-lookup"><span data-stu-id="79d04-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="79d04-190">Behobene und verschiebbare Variablen</span><span class="sxs-lookup"><span data-stu-id="79d04-190">Fixed and moveable variables</span></span>

<span data-ttu-id="79d04-191">Der Address-of-Operator ([der Address-of-Operator](unsafe-code.md#the-address-of-operator)) und die `fixed`-Anweisung ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)) dividieren Variablen in zwei Kategorien: ***Fixierte Variablen*** und ***verschiebbare Variablen***.</span><span class="sxs-lookup"><span data-stu-id="79d04-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="79d04-192">Fixierte Variablen befinden sich in Speicherorten, die von der Garbage Collector nicht betroffen sind.</span><span class="sxs-lookup"><span data-stu-id="79d04-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="79d04-193">(Beispiele für fixierte Variablen sind lokale Variablen, Wert Parameter und Variablen, die durch dereferenzierende Zeiger erstellt werden.) Auf der anderen Seite befinden sich verschiebbare Variablen in Speicherorten, die von der Garbage Collector verlagert oder verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="79d04-194">(Beispiele für verschiebbare Variablen sind Felder in Objekten und Elementen von Arrays.)</span><span class="sxs-lookup"><span data-stu-id="79d04-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="79d04-195">Der `&`-Operator ([der Address-of-Operator](unsafe-code.md#the-address-of-operator)) gestattet, dass die Adresse einer Variablen mit fester Größe ohne Einschränkungen abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="79d04-196">Da eine verschiebbare Variable jedoch vom Garbage Collector verschoben oder aufgehoben werden kann, kann die Adresse einer verschiebbaren Variablen nur mit einer `fixed`-Anweisung ([fixed-Anweisung](unsafe-code.md#the-fixed-statement)) abgerufen werden, und diese Adresse bleibt nur für die die Dauer dieser `fixed`-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="79d04-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="79d04-197">Eine Variable mit fester Genauigkeit ist eine der folgenden:</span><span class="sxs-lookup"><span data-stu-id="79d04-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="79d04-198">Eine Variable, die sich aus einem *Simple_name* ([simple names](expressions.md#simple-names)) ergibt, der auf eine lokale Variable oder einen value-Parameter verweist, es sei denn, die Variable wird von einer anonymen Funktion aufgezeichnet.</span><span class="sxs-lookup"><span data-stu-id="79d04-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="79d04-199">Eine Variable, die sich aus einem *member_access* ([Member Access](expressions.md#member-access)) der Form `V.I` ergibt, wobei "`V`" eine festgelegte Variable eines *struct_type*ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="79d04-200">Eine Variable, die sich aus einer *pointer_indirection_expression* ([Zeigerdereferenzierung](unsafe-code.md#pointer-indirection)) der Form `*P`, eines *pointer_member_access* ([Zeiger Element Zugriffs](unsafe-code.md#pointer-member-access)) der Form `P->I` oder einer *pointer_element_access* ( [Zeiger Element Zugriff](unsafe-code.md#pointer-element-access)) der Form `P[E]`.</span><span class="sxs-lookup"><span data-stu-id="79d04-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="79d04-201">Alle anderen Variablen werden als verschiebbare Variablen klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="79d04-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="79d04-202">Beachten Sie, dass ein statisches Feld als eine verschiebbare Variable klassifiziert ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="79d04-203">Beachten Sie außerdem, dass der Parameter "`ref`" oder "`out`" als eine verschiebbare Variable klassifiziert ist, auch wenn das für den-Parameter angegebene Argument eine Fixed-Variable ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="79d04-204">Beachten Sie schließlich, dass eine Variable, die durch dereferenzieren eines Zeigers erzeugt wird, immer als eine festgelegte Variable klassifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="79d04-205">Zeigerkonvertierungen</span><span class="sxs-lookup"><span data-stu-id="79d04-205">Pointer conversions</span></span>

<span data-ttu-id="79d04-206">In einem unsicheren Kontext wird der Satz der verfügbaren impliziten Konvertierungen ([implizite Konvertierungen](conversions.md#implicit-conversions)) um die folgenden impliziten Zeiger Konvertierungen erweitert:</span><span class="sxs-lookup"><span data-stu-id="79d04-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="79d04-207">Von allen *pointer_type* bis zum Typ `void*`.</span><span class="sxs-lookup"><span data-stu-id="79d04-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="79d04-208">Vom `null`-literalen zu beliebigen *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="79d04-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="79d04-209">Außerdem wird in einem unsicheren Kontext der Satz der verfügbaren expliziten Konvertierungen ([explizite Konvertierungen](conversions.md#explicit-conversions)) erweitert, um die folgenden expliziten Zeiger Konvertierungen einzubeziehen:</span><span class="sxs-lookup"><span data-stu-id="79d04-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="79d04-210">Von allen *pointer_type* bis hin zu anderen *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="79d04-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="79d04-211">Von `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` oder `ulong` in beliebige *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="79d04-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="79d04-212">Von jeder *pointer_type* zu `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` oder `ulong`.</span><span class="sxs-lookup"><span data-stu-id="79d04-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="79d04-213">Schließlich umfasst der Satz von impliziten Standard Konvertierungen ([standardmäßige implizite Konvertierungen](conversions.md#standard-implicit-conversions)) in einem unsicheren Kontext die folgende Zeiger Konvertierung:</span><span class="sxs-lookup"><span data-stu-id="79d04-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="79d04-214">Von allen *pointer_type* bis zum Typ `void*`.</span><span class="sxs-lookup"><span data-stu-id="79d04-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="79d04-215">Konvertierungen zwischen zwei Zeiger Typen ändern niemals den tatsächlichen Zeiger Wert.</span><span class="sxs-lookup"><span data-stu-id="79d04-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="79d04-216">Anders ausgedrückt: eine Konvertierung von einem Zeigertyp in einen anderen hat keine Auswirkungen auf die zugrunde liegende Adresse, die durch den Zeiger angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="79d04-217">Wenn ein Zeigertyp in einen anderen konvertiert wird, wenn der resultierende Zeiger nicht ordnungsgemäß für den Verweis auf den Typ ausgerichtet ist, ist das Verhalten nicht definiert, wenn das Ergebnis dereferenziert wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="79d04-218">Im Allgemeinen ist das Konzept "ordnungsgemäß ausgerichtet" transitiv: Wenn ein Zeiger auf den Typ "`A`" ordnungsgemäß für einen Zeiger auf den Typ "`B`" ausgerichtet ist, der wiederum ordnungsgemäß für einen Zeiger auf den Typ "`C`" ausgerichtet ist, wird ein Zeiger auf den Typ "`A`" ordnungsgemäß für einen Zeiger auf den Typ `C`.</span><span class="sxs-lookup"><span data-stu-id="79d04-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="79d04-219">Beachten Sie den folgenden Fall, in dem auf eine Variable mit einem Typ über einen Zeiger auf einen anderen Typ zugegriffen wird:</span><span class="sxs-lookup"><span data-stu-id="79d04-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="79d04-220">Wenn ein Zeigertyp in einen Zeiger auf ein Byte konvertiert wird, zeigt das Ergebnis auf das niedrigste adressierte Byte der Variablen.</span><span class="sxs-lookup"><span data-stu-id="79d04-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="79d04-221">Aufeinanderfolgende Inkremente des Ergebnisses bis zur Größe der Variablen ergeben Zeiger auf die restlichen Bytes dieser Variablen.</span><span class="sxs-lookup"><span data-stu-id="79d04-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="79d04-222">Die folgende Methode zeigt beispielsweise alle acht Bytes in einem Double-Wert als Hexadezimalwert an:</span><span class="sxs-lookup"><span data-stu-id="79d04-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="79d04-223">Die erstellte Ausgabe hängt natürlich von der-Unterscheidung ab.</span><span class="sxs-lookup"><span data-stu-id="79d04-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="79d04-224">Zuordnungen zwischen Zeigern und ganzen Zahlen sind Implementierungs definiert.</span><span class="sxs-lookup"><span data-stu-id="79d04-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="79d04-225">Allerdings Verhalten sich Konvertierungen von Zeigern auf oder von ganzzahligen Typen auf 32 \*-und 64-Bit-CPU-Architekturen mit einem linearen Adressraum in der Regel genauso wie Konvertierungen von `uint`-oder `ulong`-Werten in bzw. aus diesen ganzzahligen Typen.</span><span class="sxs-lookup"><span data-stu-id="79d04-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="79d04-226">Zeiger Arrays</span><span class="sxs-lookup"><span data-stu-id="79d04-226">Pointer arrays</span></span>

<span data-ttu-id="79d04-227">In einem unsicheren Kontext können Arrays von Zeigern erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="79d04-228">Nur einige der Konvertierungen, die für andere Array Typen gelten, sind in Zeiger Arrays zulässig:</span><span class="sxs-lookup"><span data-stu-id="79d04-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="79d04-229">Die implizite Verweis Konvertierung ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions)) von *array_type* in `System.Array` und die implementierten Schnittstellen gelten auch für Zeiger Arrays.</span><span class="sxs-lookup"><span data-stu-id="79d04-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="79d04-230">Allerdings wird bei jedem Versuch, auf die Array Elemente über `System.Array` oder die von ihm implementierten Schnittstelle zuzugreifen, zur Laufzeit eine Ausnahme ausgelöst, da Zeiger Typen nicht in `object` konvertierbar sind.</span><span class="sxs-lookup"><span data-stu-id="79d04-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="79d04-231">Die impliziten und expliziten Verweis Konvertierungen ([implizite Verweis](conversions.md#implicit-reference-conversions)Konvertierungen, [explizite Verweis Konvertierungen](conversions.md#explicit-reference-conversions)) aus einem eindimensionalen Arraytyp `S[]` in `System.Collections.Generic.IList<T>` und seine generischen Basis Schnittstellen werden nie auf Zeiger Arrays angewendet. Zeiger Typen können nicht als Typargumente verwendet werden, und es sind keine Konvertierungen von Zeiger Typen zu nicht-Zeiger Typen vorhanden.</span><span class="sxs-lookup"><span data-stu-id="79d04-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="79d04-232">Die explizite Verweis Konvertierung ([explizite Verweis Konvertierungen](conversions.md#explicit-reference-conversions)) von `System.Array` und die Schnittstellen, die Sie für beliebige *array_type* implementiert, gelten für Zeiger Arrays.</span><span class="sxs-lookup"><span data-stu-id="79d04-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="79d04-233">Die expliziten Verweis Konvertierungen ([explizite Verweis Konvertierungen](conversions.md#explicit-reference-conversions)) von `System.Collections.Generic.IList<S>` und deren Basis Schnittstellen in einen eindimensionalen Arraytyp `T[]` gelten nie für Zeiger Arrays, da Zeiger Typen nicht als Typargumente verwendet werden können und es keine Konvertierungen von Zeiger Typen in nicht-Zeiger Typen.</span><span class="sxs-lookup"><span data-stu-id="79d04-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="79d04-234">Diese Einschränkungen bedeuten, dass die Erweiterung für die `foreach`-Anweisung über Arrays, die in [der foreach-Anweisung](statements.md#the-foreach-statement) beschrieben werden, nicht auf Zeiger Arrays angewendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="79d04-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="79d04-235">Stattdessen wird eine foreach-Anweisung der Form</span><span class="sxs-lookup"><span data-stu-id="79d04-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="79d04-236">Wenn der `x`-Typ ein Arraytyp der Form ist `T[,,...,]`, `N` die Anzahl der Dimensionen minus 1 und `T` oder `V` ein Zeigertyp ist, wird wie folgt mithilfe von schsted for-Schleifen erweitert:</span><span class="sxs-lookup"><span data-stu-id="79d04-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

<span data-ttu-id="79d04-237">Die Variablen "`a`", "`i0`", "`i1`", "...", "`iN`" sind für `x` oder den *embedded_statement* oder einen beliebigen anderen Quellcode des Programms nicht sichtbar oder können darauf zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="79d04-238">Die Variable `v` ist in der eingebetteten Anweisung schreibgeschützt.</span><span class="sxs-lookup"><span data-stu-id="79d04-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="79d04-239">Wenn keine explizite Konvertierung ([Zeiger Konvertierungen](unsafe-code.md#pointer-conversions)) von `T` (dem Elementtyp) zu `V` vorhanden ist, wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="79d04-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="79d04-240">Wenn `x` den Wert `null`hat, wird `System.NullReferenceException` zur Laufzeit eine ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="79d04-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="79d04-241">Zeiger in Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="79d04-241">Pointers in expressions</span></span>

<span data-ttu-id="79d04-242">In einem unsicheren Kontext kann ein Ausdruck das Ergebnis eines Zeiger Typs ergeben, aber außerhalb eines unsicheren Kontexts ist es ein Kompilierzeitfehler für einen Ausdruck, der einen Zeigertyp aufweisen soll.</span><span class="sxs-lookup"><span data-stu-id="79d04-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="79d04-243">Genau gesagt: außerhalb eines unsicheren Kontexts tritt ein Kompilierzeitfehler auf, *Wenn Simple_name* ([simple names](expressions.md#simple-names)), *member_access* ([Member Access](expressions.md#member-access)), *invocation_expression* ([Aufruf Ausdrücke](expressions.md#invocation-expressions)) oder  *element_access* ([Element Zugriff](expressions.md#element-access)) ist ein Zeigertyp.</span><span class="sxs-lookup"><span data-stu-id="79d04-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="79d04-244">In einem unsicheren Kontext ermöglichen die *primary_no_array_creation_expression* ([Primary Expressions](expressions.md#primary-expressions)) und *unary_expression* ([unäre Operatoren](expressions.md#unary-operators)) die folgenden zusätzlichen Konstrukte:</span><span class="sxs-lookup"><span data-stu-id="79d04-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

<span data-ttu-id="79d04-245">Diese Konstrukte werden in den folgenden Abschnitten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="79d04-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="79d04-246">Die-Grammatik ist die Rangfolge und Assoziativität der unsicheren Operatoren.</span><span class="sxs-lookup"><span data-stu-id="79d04-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="79d04-247">Zeiger Dereferenzierung</span><span class="sxs-lookup"><span data-stu-id="79d04-247">Pointer indirection</span></span>

<span data-ttu-id="79d04-248">Ein *pointer_indirection_expression* besteht aus einem Sternchen (`*`), gefolgt von einem *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="79d04-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="79d04-249">Der unäre `*`-Operator bezeichnet die Zeiger Dereferenzierung und wird zum Abrufen der Variablen verwendet, auf die ein Zeiger zeigt.</span><span class="sxs-lookup"><span data-stu-id="79d04-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="79d04-250">Das Ergebnis der Auswertung von "`*P`", wobei "`P`" ein Ausdruck eines Zeiger Typs `T*` ist, ist eine Variable vom Typ "`T`".</span><span class="sxs-lookup"><span data-stu-id="79d04-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="79d04-251">Es handelt sich um einen Kompilierzeitfehler, mit dem der unäre `*`-Operator auf einen Ausdruck vom Typ `void*` oder auf einen Ausdruck angewendet wird, der kein Zeigertyp ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="79d04-252">Die Auswirkung der Anwendung des unären `*`-Operators auf einen `null`-Zeiger ist Implementierungs definiert.</span><span class="sxs-lookup"><span data-stu-id="79d04-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="79d04-253">Insbesondere gibt es keine Garantie, dass dieser Vorgang eine `System.NullReferenceException` auslöst.</span><span class="sxs-lookup"><span data-stu-id="79d04-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="79d04-254">Wenn dem Zeiger ein ungültiger Wert zugewiesen wurde, ist das Verhalten des unären `*`-Operators nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="79d04-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="79d04-255">Unter den ungültigen Werten für die Dereferenzierung eines Zeigers durch den unären `*`-Operator handelt es sich um eine Adresse, die für den Typ, auf den verwiesen wird (siehe Beispiel in [Zeiger Konvertierungen](unsafe-code.md#pointer-conversions)), und die Adresse einer Variablen nach dem Ende ihrer Lebensdauer nicht ordnungsgemäß ausgerichtet ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="79d04-256">Zum Zweck der eindeutigen Zuweisungs Analyse wird eine Variable, die durch Auswerten eines Ausdrucks der Form `*P` erstellt wird, als anfänglich zugewiesen ([ursprünglich zugewiesene Variablen](variables.md#initially-assigned-variables)).</span><span class="sxs-lookup"><span data-stu-id="79d04-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="79d04-257">Zugriff auf Zeiger Elemente</span><span class="sxs-lookup"><span data-stu-id="79d04-257">Pointer member access</span></span>

<span data-ttu-id="79d04-258">Ein *pointer_member_access* besteht aus einem *primary_expression*, gefolgt von einem "`->`"-Token, gefolgt von einem *Bezeichner* und einem optionalen *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="79d04-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="79d04-259">In einem Zeiger Element Zugriff auf das Formular `P->I` muss `P` ein Ausdruck eines anderen Zeiger Typs als `void*` sein, und `I` muss einen zugänglichen Member des Typs angeben, zu dem `P`-Punkte gehören.</span><span class="sxs-lookup"><span data-stu-id="79d04-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="79d04-260">Ein Zeiger Element Zugriff auf das Formular `P->I` wird genau wie `(*P).I` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="79d04-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="79d04-261">Eine Beschreibung des Zeigerdereferenzierungsoperators (`*`) finden Sie unter [Zeiger Dereferenzierung](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="79d04-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="79d04-262">Eine Beschreibung des Mitglieds Zugriffs Operators (`.`) finden Sie unter [Member Access](expressions.md#member-access).</span><span class="sxs-lookup"><span data-stu-id="79d04-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="79d04-263">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="79d04-263">In the example</span></span>

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

<span data-ttu-id="79d04-264">der `->`-Operator wird verwendet, um auf Felder zuzugreifen und eine Methode einer Struktur mithilfe eines Zeigers aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="79d04-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="79d04-265">Da der Vorgang `P->I` exakt dem `(*P).I` entspricht, könnte die `Main`-Methode gleichermaßen gut geschrieben worden sein:</span><span class="sxs-lookup"><span data-stu-id="79d04-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a><span data-ttu-id="79d04-266">Zeiger Element Zugriff</span><span class="sxs-lookup"><span data-stu-id="79d04-266">Pointer element access</span></span>

<span data-ttu-id="79d04-267">Ein *pointer_element_access* besteht aus einem *primary_no_array_creation_expression* , gefolgt von einem Ausdruck, der in "`[`" und "`]`" eingeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="79d04-268">Bei einem Zeiger Element Zugriff auf das Formular `P[E]` muss `P` ein Ausdruck eines anderen Zeiger Typs als `void*` sein, und `E` muss ein Ausdruck sein, der implizit in `int`, `uint`, `long` oder `ulong` konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="79d04-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="79d04-269">Ein Zeiger Element Zugriff auf das Formular `P[E]` wird genau wie `*(P + E)` ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="79d04-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="79d04-270">Eine Beschreibung des Zeigerdereferenzierungsoperators (`*`) finden Sie unter [Zeiger Dereferenzierung](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="79d04-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="79d04-271">Eine Beschreibung des Zeiger Additions Operators (`+`) finden Sie unter [Zeigerarithmetik](unsafe-code.md#pointer-arithmetic).</span><span class="sxs-lookup"><span data-stu-id="79d04-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="79d04-272">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="79d04-272">In the example</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

<span data-ttu-id="79d04-273">Ein Zeiger Element Zugriff wird verwendet, um den Zeichen Puffer in einer `for`-Schleife zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="79d04-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="79d04-274">Da der Vorgang `P[E]` genau mit `*(P + E)` übereinstimmt, könnte das Beispiel gleich gut geschrieben worden sein:</span><span class="sxs-lookup"><span data-stu-id="79d04-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

<span data-ttu-id="79d04-275">Der Zeiger Element-Zugriffs Operator prüft nicht auf Fehler aufgrund von Fehlern, und das Verhalten beim Zugriff auf ein Out-of-Bounds-Element ist nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="79d04-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="79d04-276">Dies entspricht dem C und C++.</span><span class="sxs-lookup"><span data-stu-id="79d04-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="79d04-277">Der Address-of-Operator</span><span class="sxs-lookup"><span data-stu-id="79d04-277">The address-of operator</span></span>

<span data-ttu-id="79d04-278">Ein *addressof_expression* besteht aus einem kaufmännischen und-(`&`), gefolgt von einem *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="79d04-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="79d04-279">Wenn ein Ausdruck `E` ist, der vom Typ "`T`" ist und als eine Variable mit fester Größe ([Fixed und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)) klassifiziert ist, berechnet das Konstrukt "`&E`" die Adresse der Variablen, die von `E` angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="79d04-280">Der Ergebnistyp ist `T*` und wird als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="79d04-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="79d04-281">Ein Kompilierzeitfehler tritt auf, wenn `E` nicht als Variable klassifiziert ist, wenn `E` als schreibgeschützte lokale Variable klassifiziert ist, oder wenn `E` eine verschiebbare Variable bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="79d04-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="79d04-282">Im letzten Fall kann eine fixed-Anweisung ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)) verwendet werden, um die Variable temporär zu korrigieren, bevor Sie Ihre Adresse erhält.</span><span class="sxs-lookup"><span data-stu-id="79d04-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="79d04-283">Wie in [Member Access](expressions.md#member-access)angegeben, wird das Feld außerhalb eines Instanzkonstruktors oder statischen Konstruktors für eine Struktur oder Klasse, die ein `readonly`-Feld definiert, als Wert, nicht als Variable angesehen.</span><span class="sxs-lookup"><span data-stu-id="79d04-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="79d04-284">Daher kann die Adresse nicht übernommen werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="79d04-285">Ebenso kann die Adresse einer Konstante nicht übernommen werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="79d04-286">Der `&`-Operator erfordert nicht, dass sein Argument definitiv zugewiesen wird, aber nach einem `&`-Vorgang wird die Variable, auf die der Operator angewendet wird, definitiv in dem Ausführungs Pfad zugewiesen, in dem der Vorgang stattfindet.</span><span class="sxs-lookup"><span data-stu-id="79d04-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="79d04-287">Es liegt in der Verantwortung des Programmierers, sicherzustellen, dass die richtige Initialisierung der Variablen in dieser Situation tatsächlich stattfindet.</span><span class="sxs-lookup"><span data-stu-id="79d04-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="79d04-288">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="79d04-288">In the example</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="79d04-289">`i` wird nach dem `&i`-Vorgang, der zum Initialisieren von `p` verwendet wird, definitiv zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="79d04-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="79d04-290">Die Zuweisung zu "`*p`" initialisiert `i`, aber die Einbindung dieser Initialisierung liegt in der Verantwortung des Programmierers, und es tritt kein Kompilierzeitfehler auf, wenn die Zuweisung entfernt wurde.</span><span class="sxs-lookup"><span data-stu-id="79d04-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="79d04-291">Die Regeln der eindeutigen Zuweisung für den `&`-Operator sind so vorhanden, dass die redundante Initialisierung lokaler Variablen vermieden werden kann.</span><span class="sxs-lookup"><span data-stu-id="79d04-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="79d04-292">Viele externe APIs nehmen z. b. einen Zeiger auf eine Struktur, die von der API ausgefüllt wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="79d04-293">Aufrufe dieser APIs übergeben in der Regel die Adresse einer lokalen Struktur Variablen, und ohne die Regel wäre eine redundante Initialisierung der Struktur Variablen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="79d04-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="79d04-294">Inkrementieren und Dekrementieren von Zeigern</span><span class="sxs-lookup"><span data-stu-id="79d04-294">Pointer increment and decrement</span></span>

<span data-ttu-id="79d04-295">In einem unsicheren Kontext können die Operatoren "`++`" und "`--`" ([postfix-Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators) und [Präfix Inkrement-und Dekrementoperatoren](expressions.md#prefix-increment-and-decrement-operators)) auf Zeiger Variablen aller Typen außer `void*` angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="79d04-296">Daher sind für jeden Zeigertyp `T*` die folgenden Operatoren implizit definiert:</span><span class="sxs-lookup"><span data-stu-id="79d04-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="79d04-297">Die Operatoren erzielen dieselben Ergebnisse wie `x + 1` und `x - 1` bzw. ([Zeigerarithmetik](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="79d04-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="79d04-298">Anders ausgedrückt: für eine Zeiger Variable vom Typ `T*` fügt der `++`-Operator der in der Variablen enthaltenen Adresse `sizeof(T)` hinzu, und der `--`-Operator subtrahiert `sizeof(T)` von der Adresse, die in der Variablen enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="79d04-299">Wenn ein Zeiger Inkrement-oder Dekrement-Vorgang die Domäne des Zeiger Typs überschreitet, wird das Ergebnis durch die Implementierung definiert, es werden jedoch keine Ausnahmen erzeugt.</span><span class="sxs-lookup"><span data-stu-id="79d04-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="79d04-300">Zeigerarithmetik</span><span class="sxs-lookup"><span data-stu-id="79d04-300">Pointer arithmetic</span></span>

<span data-ttu-id="79d04-301">In einem unsicheren Kontext können die Operatoren "`+`" und "`-`" (Additions[Operator](expressions.md#addition-operator) und [Subtraktions Operator](expressions.md#subtraction-operator)) auf Werte aller Zeiger Typen außer `void*` angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="79d04-302">Daher sind für jeden Zeigertyp `T*` die folgenden Operatoren implizit definiert:</span><span class="sxs-lookup"><span data-stu-id="79d04-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

<span data-ttu-id="79d04-303">Wenn ein Ausdruck `P` eines Zeiger Typs ist `T*` und ein Ausdruck `N` vom Typ `int`, `uint`, `long` oder `ulong`, berechnen die Ausdrücke `P + N` und `N + P` den Zeiger Wert des Typs `T*`, der sich aus dem Hinzufügen von 0 zur Adresse ergibt. wird von 1 angegeben.</span><span class="sxs-lookup"><span data-stu-id="79d04-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="79d04-304">Entsprechend berechnet der Ausdruck `P - N` den Zeiger Wert des Typs `T*`, der sich aus der Subtraktion von `N * sizeof(T)` von der Adresse ergibt, die von `P` angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="79d04-305">Wenn zwei Ausdrücke, `P` und `Q`, eines Zeiger Typs `T*`, berechnet der Ausdruck `P - Q` den Unterschied zwischen den Adressen, die von `P` und `Q` angegeben werden, und dividiert diese Differenz durch `sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="79d04-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="79d04-306">Der Ergebnistyp ist immer `long`.</span><span class="sxs-lookup"><span data-stu-id="79d04-306">The type of the result is always `long`.</span></span> <span data-ttu-id="79d04-307">In der Tat wird `P - Q` als `((long)(P) - (long)(Q)) / sizeof(T)` berechnet.</span><span class="sxs-lookup"><span data-stu-id="79d04-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="79d04-308">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="79d04-308">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

<span data-ttu-id="79d04-309">der die Ausgabe erzeugt:</span><span class="sxs-lookup"><span data-stu-id="79d04-309">which produces the output:</span></span>

```console
p - q = -14
q - p = 14
```

<span data-ttu-id="79d04-310">Wenn eine Zeiger arithmetische Operation die Domäne des Zeiger Typs überschreitet, wird das Ergebnis in einer durch die Implementierung definierten Weise abgeschnitten, es werden jedoch keine Ausnahmen erzeugt.</span><span class="sxs-lookup"><span data-stu-id="79d04-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="79d04-311">Zeiger Vergleich</span><span class="sxs-lookup"><span data-stu-id="79d04-311">Pointer comparison</span></span>

<span data-ttu-id="79d04-312">In einem unsicheren Kontext können die Operatoren "`==`", "`!=`", "`<`", "`>`", "`<=`" und "`=>`" ([relationale und Typtest Operatoren](expressions.md#relational-and-type-testing-operators)) auf Werte aller Zeiger Typen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="79d04-313">Die Zeiger Vergleichs Operatoren lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="79d04-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="79d04-314">Da eine implizite Konvertierung von einem Zeigertyp in den `void*`-Typ vorhanden ist, können die Operanden beliebiger Zeigertyp mithilfe dieser Operatoren verglichen werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="79d04-315">Die Vergleichs Operatoren vergleichen die Adressen, die von den beiden Operanden angegeben werden, so, als wären Sie ganze Zahlen ohne Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="79d04-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="79d04-316">Der sizeof-Operator</span><span class="sxs-lookup"><span data-stu-id="79d04-316">The sizeof operator</span></span>

<span data-ttu-id="79d04-317">Der `sizeof`-Operator gibt die Anzahl der Bytes zurück, die von einer Variablen eines bestimmten Typs belegt werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="79d04-318">Der als Operand angegebene Typ für `sizeof` muss ein *unmanaged_type* ([Zeiger Typen](unsafe-code.md#pointer-types)) sein.</span><span class="sxs-lookup"><span data-stu-id="79d04-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="79d04-319">Das Ergebnis des `sizeof`-Operators ist ein Wert vom Typ `int`.</span><span class="sxs-lookup"><span data-stu-id="79d04-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="79d04-320">Für bestimmte vordefinierte Typen ergibt der `sizeof`-Operator einen konstanten Wert, wie in der folgenden Tabelle dargestellt.</span><span class="sxs-lookup"><span data-stu-id="79d04-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="79d04-321">__expression__</span><span class="sxs-lookup"><span data-stu-id="79d04-321">__Expression__</span></span>   | <span data-ttu-id="79d04-322">__Ergebnis__</span><span class="sxs-lookup"><span data-stu-id="79d04-322">__Result__</span></span> |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

<span data-ttu-id="79d04-323">Für alle anderen Typen ist das Ergebnis des `sizeof`-Operators Implementierungs definiert und wird als Wert, nicht als Konstante klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="79d04-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="79d04-324">Die Reihenfolge, in der Elemente in einer Struktur verpackt werden, ist nicht angegeben.</span><span class="sxs-lookup"><span data-stu-id="79d04-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="79d04-325">Zu Ausrichtungs Zwecken gibt es möglicherweise unbenannte Auffüll Zeichen am Anfang einer Struktur, innerhalb einer Struktur und am Ende der Struktur.</span><span class="sxs-lookup"><span data-stu-id="79d04-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="79d04-326">Der Inhalt der als Auffüllung verwendeten Bits ist unbestimmt.</span><span class="sxs-lookup"><span data-stu-id="79d04-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="79d04-327">Wenn ein Operand mit einem Strukturtyp angewendet wird, ist das Ergebnis die Gesamtzahl der Bytes in einer Variablen dieses Typs, einschließlich aller Auffüll Zeichen.</span><span class="sxs-lookup"><span data-stu-id="79d04-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="79d04-328">Fixed-Anweisung</span><span class="sxs-lookup"><span data-stu-id="79d04-328">The fixed statement</span></span>

<span data-ttu-id="79d04-329">In einem unsicheren Kontext gestattet die *embedded_statement* ([Statements](statements.md)) Production ein zusätzliches Konstrukt, die `fixed`-Anweisung, die verwendet wird, um eine verschiebbare Variable zu "korrigieren", sodass die Adresse für die Dauer der Anweisung konstant bleibt. .</span><span class="sxs-lookup"><span data-stu-id="79d04-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

<span data-ttu-id="79d04-330">Jede *fixed_pointer_declarator* deklariert eine lokale Variable des angegebenen *pointer_type* und initialisiert diese lokale Variable mit der Adresse, die von der entsprechenden *fixed_pointer_initializer*berechnet wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="79d04-331">Auf eine lokale Variable, die in einer `fixed`-Anweisung deklariert ist, kann in allen *fixed_pointer_initializer*s, die rechts neben der Deklaration der Variablen auftreten, und in der *embedded_statement* -Anweisung der `fixed`-Anweisung zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="79d04-332">Eine lokale Variable, die von einer `fixed`-Anweisung deklariert wird, wird als schreibgeschützt betrachtet.</span><span class="sxs-lookup"><span data-stu-id="79d04-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="79d04-333">Ein Kompilierzeitfehler tritt auf, wenn die eingebettete Anweisung versucht, diese lokale Variable (überzuweisung oder den `++`-und `--`-Operatoren) zu ändern oder Sie als `ref`-oder `out`-Parameter zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="79d04-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="79d04-334">Ein *fixed_pointer_initializer* kann eines der folgenden sein:</span><span class="sxs-lookup"><span data-stu-id="79d04-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="79d04-335">Das Token "`&`" gefolgt von einem *variable_reference* ([genaue Regeln zum Bestimmen der eindeutigen Zuweisung](variables.md#precise-rules-for-determining-definite-assignment)) für eine verschiebbare Variable ([Feste und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)) eines nicht verwalteten Typs `T`, sofern der Typ `T*` ist implizit in den Zeigertyp konvertierbar, der in der `fixed`-Anweisung angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="79d04-336">In diesem Fall berechnet der Initialisierer die Adresse der angegebenen Variablen, und die Variable bleibt für die Dauer der `fixed`-Anweisung garantiert an einer bestimmten Adresse.</span><span class="sxs-lookup"><span data-stu-id="79d04-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="79d04-337">Ein Ausdruck eines *array_type* mit Elementen eines nicht verwalteten Typs `T`, sofern der Typ `T*` implizit in den Zeigertyp konvertiert werden kann, der in der `fixed`-Anweisung angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="79d04-338">In diesem Fall berechnet der Initialisierer die Adresse des ersten Elements im Array, und das gesamte Array bleibt für die Dauer der `fixed`-Anweisung garantiert an einer Fixed-Adresse.</span><span class="sxs-lookup"><span data-stu-id="79d04-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="79d04-339">Wenn der Array Ausdruck NULL ist oder das Array über keine Elemente verfügt, berechnet der Initialisierer eine Adresse, die gleich 0 (null) ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="79d04-340">Ein Ausdruck vom Typ "`string`", wenn der Typ "`char*`" implizit in den Zeigertyp konvertiert werden kann, der in der `fixed`-Anweisung angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="79d04-341">In diesem Fall berechnet der Initialisierer die Adresse des ersten Zeichens in der Zeichenfolge, und die gesamte Zeichenfolge bleibt für die Dauer der `fixed`-Anweisung garantiert an einer Fixed-Adresse.</span><span class="sxs-lookup"><span data-stu-id="79d04-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="79d04-342">Das Verhalten der `fixed`-Anweisung ist Implementierungs definiert, wenn der Zeichen folgen Ausdruck NULL ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="79d04-343">Ein *Simple_name* -oder *member_access* -Element, das auf einen Puffer mit fester Größe einer verschiebbaren Variablen verweist, sofern der Typ des Puffer Elements mit fester Größe implizit in den Zeigertyp konvertiert werden kann, der in der `fixed`-Anweisung angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="79d04-344">In diesem Fall berechnet der Initialisierer einen Zeiger auf das erste Element des Puffers mit fester Größe ([Puffer fester Größe in Ausdrücken](unsafe-code.md#fixed-size-buffers-in-expressions)), und der Puffer mit fester Größe bleibt für die Dauer der `fixed`-Anweisung garantiert an einer Fixed-Adresse.</span><span class="sxs-lookup"><span data-stu-id="79d04-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="79d04-345">Für jede Adresse, die von einem *fixed_pointer_initializer* berechnet wird, stellt die `fixed`-Anweisung sicher, dass die Variable, auf die die Adresse verweist, nicht für die Dauer der `fixed`-Anweisung von der Garbage Collector verlagert oder nicht zur Verfügung gestellt wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="79d04-346">Wenn die von einem *fixed_pointer_initializer* berechnete Adresse beispielsweise auf ein Feld eines Objekts oder ein Element einer Array Instanz verweist, stellt die `fixed`-Anweisung sicher, dass die enthaltende Objektinstanz während des die Lebensdauer der Anweisung.</span><span class="sxs-lookup"><span data-stu-id="79d04-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="79d04-347">Es ist Aufgabe des Programmierers, sicherzustellen, dass die von `fixed`-Anweisungen erstellten Zeiger nicht über die Ausführung dieser Anweisungen hinausgehen.</span><span class="sxs-lookup"><span data-stu-id="79d04-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="79d04-348">Wenn beispielsweise Zeiger, die von `fixed`-Anweisungen erstellt werden, an externe APIs übermittelt werden, liegt es in der Verantwortung des Programmierers sicherzustellen, dass die APIs keinen Arbeitsspeicher für diese Zeiger erhalten.</span><span class="sxs-lookup"><span data-stu-id="79d04-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="79d04-349">Fixed-Objekte können die Fragmentierung des Heaps verursachen (da Sie nicht verschoben werden können).</span><span class="sxs-lookup"><span data-stu-id="79d04-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="79d04-350">Aus diesem Grund sollten Objekte nur dann korrigiert werden, wenn Sie unbedingt erforderlich sind, und dann nur für die kürzeste benötigte Zeit.</span><span class="sxs-lookup"><span data-stu-id="79d04-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="79d04-351">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="79d04-351">The example</span></span>

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

<span data-ttu-id="79d04-352">veranschaulicht verschiedene Verwendungen der `fixed`-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="79d04-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="79d04-353">Mit der ersten Anweisung wird die Adresse eines statischen Felds korrigiert und abgerufen, mit der zweiten Anweisung wird die Adresse eines Instanzfelds korrigiert und abgerufen, und die dritte Anweisung korrigiert und ruft die Adresse eines Array Elements ab.</span><span class="sxs-lookup"><span data-stu-id="79d04-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="79d04-354">In jedem Fall wäre es ein Fehler, den regulären `&`-Operator zu verwenden, da die Variablen alle als verschiebbare Variablen klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="79d04-355">Die vierte `fixed`-Anweisung im obigen Beispiel führt zu einem ähnlichen Ergebnis wie das dritte.</span><span class="sxs-lookup"><span data-stu-id="79d04-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="79d04-356">In diesem Beispiel für die `fixed`-Anweisung wird `string` verwendet:</span><span class="sxs-lookup"><span data-stu-id="79d04-356">This example of the `fixed` statement uses `string`:</span></span>

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

<span data-ttu-id="79d04-357">In einem unsicheren Kontext Array werden Elemente von eindimensionalen Arrays in zunehmender Index Reihenfolge gespeichert, beginnend mit Index `0` und endende mit Index `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="79d04-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="79d04-358">Bei mehrdimensionalen Arrays werden Array Elemente so gespeichert, dass die Indizes der äußersten rechten Dimension zuerst, dann die nächste linke Dimension usw. nach links angehoben werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="79d04-359">Innerhalb einer `fixed`-Anweisung, die einen Zeiger `p` auf eine Array Instanz `a` abruft, stellen die Zeiger Werte zwischen `p` und `p + a.Length - 1` Adressen der Elemente im Array dar.</span><span class="sxs-lookup"><span data-stu-id="79d04-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="79d04-360">Ebenso stellen die Variablen, von `p[0]` bis `p[a.Length - 1]`, die eigentlichen Array Elemente dar.</span><span class="sxs-lookup"><span data-stu-id="79d04-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="79d04-361">Angesichts der Art und Weise, in der Arrays gespeichert werden, können wir ein Array einer beliebigen Dimension so behandeln, als wäre es linear.</span><span class="sxs-lookup"><span data-stu-id="79d04-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="79d04-362">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="79d04-362">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

<span data-ttu-id="79d04-363">der die Ausgabe erzeugt:</span><span class="sxs-lookup"><span data-stu-id="79d04-363">which produces the output:</span></span>

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="79d04-364">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="79d04-364">In the example</span></span>

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

<span data-ttu-id="79d04-365">eine `fixed`-Anweisung wird verwendet, um ein Array zu korrigieren, sodass die Adresse an eine Methode, die einen Zeiger annimmt, übermittelt werden kann.</span><span class="sxs-lookup"><span data-stu-id="79d04-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="79d04-366">Im folgenden Beispiel</span><span class="sxs-lookup"><span data-stu-id="79d04-366">In the example:</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

<span data-ttu-id="79d04-367">eine fixed-Anweisung wird verwendet, um einen Puffer fester Größe einer Struktur zu korrigieren, sodass die Adresse als Zeiger verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="79d04-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="79d04-368">Ein `char*`-Wert, der durch das Reparieren einer Zeichen folgen Instanz erzeugt wird, verweist immer auf eine mit NULL endenden Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="79d04-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="79d04-369">Innerhalb einer fixed-Anweisung, die einen Zeiger `p` auf eine Zeichen folgen Instanz `s` abruft, stellen die Zeiger Werte von `p` bis `p + s.Length - 1` Adressen der Zeichen in der Zeichenfolge dar, und der Zeiger Wert `p + s.Length` zeigt immer auf ein NULL-Zeichen (das Zeichen mit dem Wert `'\0'`).</span><span class="sxs-lookup"><span data-stu-id="79d04-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="79d04-370">Das Ändern von Objekten des verwalteten Typs durch Fixed-Zeiger kann zu undefiniertem Verhalten führen.</span><span class="sxs-lookup"><span data-stu-id="79d04-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="79d04-371">Da z. b. Zeichen folgen unveränderlich sind, ist es die Aufgabe des Programmierers sicherzustellen, dass die Zeichen, auf die von einem Zeiger auf eine festgelegte Zeichenfolge verwiesen wird, nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="79d04-372">Die automatische NULL-Beendigung von Zeichen folgen ist besonders praktisch, wenn externe APIs aufgerufen werden, die "C-Style"-Zeichen folgen erwarten.</span><span class="sxs-lookup"><span data-stu-id="79d04-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="79d04-373">Beachten Sie jedoch, dass eine Zeichen folgen Instanz NULL-Zeichen enthalten darf.</span><span class="sxs-lookup"><span data-stu-id="79d04-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="79d04-374">Wenn solche NULL-Zeichen vorhanden sind, wird die Zeichenfolge abgeschnitten, wenn Sie als NULL-terminierte `char*` behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="79d04-375">Puffer fester Größe</span><span class="sxs-lookup"><span data-stu-id="79d04-375">Fixed size buffers</span></span>

<span data-ttu-id="79d04-376">Puffer fester Größe werden verwendet, um in-Line-Arrays im C-Format als Member von Strukturen zu deklarieren und sind hauptsächlich für die Schnittstellen mit nicht verwalteten APIs nützlich.</span><span class="sxs-lookup"><span data-stu-id="79d04-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="79d04-377">Puffer Deklarationen fester Größe</span><span class="sxs-lookup"><span data-stu-id="79d04-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="79d04-378">Ein ***Puffer fester Größe*** ist ein Member, der den Speicher für einen Puffer fester Länge von Variablen eines bestimmten Typs darstellt.</span><span class="sxs-lookup"><span data-stu-id="79d04-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="79d04-379">Eine Puffer Deklaration mit fester Größe führt einen oder mehrere Puffer fester Größe eines bestimmten Elementtyps ein.</span><span class="sxs-lookup"><span data-stu-id="79d04-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="79d04-380">Puffer fester Größe sind nur in Struktur Deklarationen zulässig und können nur in unsicheren Kontexten ([unsichere Kontexte](unsafe-code.md#unsafe-contexts)) vorkommen.</span><span class="sxs-lookup"><span data-stu-id="79d04-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

<span data-ttu-id="79d04-381">Eine Puffer Deklaration fester Größe kann einen Satz von Attributen ([Attribute](attributes.md)), einen `new`-Modifizierer ([Modifizierer](classes.md#modifiers)), eine gültige Kombination der vier Zugriffsmodifizierer ([Typparameter und Einschränkungen](classes.md#type-parameters-and-constraints)) und einen `unsafe`-Modifizierer (unsicher) enthalten.[ Kontexte](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="79d04-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="79d04-382">Die Attribute und Modifizierer gelten für alle Member, die von der Puffer Deklaration mit fester Größe deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="79d04-383">Es ist ein Fehler, dass derselbe Modifizierer mehrmals in einer Puffer Deklaration fester Größe angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="79d04-384">Eine Puffer Deklaration mit fester Größe darf den `static`-Modifizierer nicht enthalten.</span><span class="sxs-lookup"><span data-stu-id="79d04-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="79d04-385">Der Puffer Elementtyp einer Puffer Deklaration mit fester Größe gibt den Elementtyp der Puffer an, die von der Deklaration eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="79d04-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="79d04-386">Beim Puffer Elementtyp muss es sich um einen der vordefinierten Typen `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0 oder 1 handeln.</span><span class="sxs-lookup"><span data-stu-id="79d04-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="79d04-387">Auf den Puffer Elementtyp folgt eine Liste von Puffer Deklaratoren fester Größe, von denen jeder einen neuen Member einführt.</span><span class="sxs-lookup"><span data-stu-id="79d04-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="79d04-388">Ein Puffer mit fester Größe besteht aus einem Bezeichner, der den Member benennt, gefolgt von einem konstanten Ausdruck, der in `[`-und `]`-Token eingeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="79d04-389">Der Konstante Ausdruck gibt die Anzahl der Elemente in dem Element an, das von diesem Puffer Deklarator mit fester Größe eingeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="79d04-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="79d04-390">Der Typ des konstanten Ausdrucks muss implizit in den Typ "`int`" konvertiert werden, und der Wert muss eine positive ganze Zahl ungleich NULL sein.</span><span class="sxs-lookup"><span data-stu-id="79d04-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="79d04-391">Es ist sichergestellt, dass die Elemente eines Puffers mit fester Größe sequenziell im Arbeitsspeicher angeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="79d04-392">Eine Puffer Deklaration fester Größe, die mehrere Puffer fester Größe deklariert, entspricht mehreren Deklarationen einer einzelnen Puffer Deklaration mit fester Größe mit denselben Attributen und Elementtypen.</span><span class="sxs-lookup"><span data-stu-id="79d04-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="79d04-393">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="79d04-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="79d04-394">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="79d04-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="79d04-395">Puffer fester Größe in Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="79d04-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="79d04-396">Die Member-Suche ([Operatoren](expressions.md#operators)) eines Puffer Elements fester Größe verläuft genau wie die Element Suche eines Felds.</span><span class="sxs-lookup"><span data-stu-id="79d04-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="79d04-397">Ein Puffer fester Größe kann in einem Ausdruck mithilfe eines *Simple_name* ([Typrückschlusses](expressions.md#type-inference)) oder eines *member_access* ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) referenziert werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="79d04-398">Wenn ein Puffer Element mit fester Größe als einfacher Name referenziert wird, entspricht der Effekt dem Element Zugriff im Formular `this.I`, wobei `I` das Puffer Element mit fester Größe ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="79d04-399">Wenn ein Element Zugriff auf das Formular `E.I`, wenn `E` einen Strukturtyp und eine Element Suche von `I` in diesem Strukturtyp einen Member mit fester Größe identifiziert, wird `E.I` wie folgt als klassifiziert ausgewertet:</span><span class="sxs-lookup"><span data-stu-id="79d04-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="79d04-400">Wenn der Ausdruck `E.I` nicht in einem unsicheren Kontext auftritt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="79d04-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="79d04-401">Wenn `E` als Wert klassifiziert wird, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="79d04-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="79d04-402">Andernfalls tritt ein Kompilierzeitfehler auf, wenn `E` eine verschiebbare Variable ([Fixed und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)) und der Ausdruck `E.I` kein *fixed_pointer_initializer* ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)) ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="79d04-403">Andernfalls verweist `E` auf eine Variable mit fester Größe, und das Ergebnis des Ausdrucks ist ein Zeiger auf das erste Element des Puffer Elements mit fester Größe `I` in `E`.</span><span class="sxs-lookup"><span data-stu-id="79d04-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="79d04-404">Das Ergebnis ist vom Typ "`S*`", wobei "`S`" der Elementtyp "`I`" ist und als Wert klassifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="79d04-405">Auf die nachfolgenden Elemente des Puffers mit fester Größe kann mithilfe von Zeiger Vorgängen aus dem ersten Element zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="79d04-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="79d04-406">Im Gegensatz zum Zugriff auf Arrays ist der Zugriff auf die Elemente eines Puffers fester Größe ein unsicherer Vorgang und ist nicht Bereichs geprüft.</span><span class="sxs-lookup"><span data-stu-id="79d04-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="79d04-407">Im folgenden Beispiel wird eine Struktur mit einem Puffer Element fester Größe deklariert und verwendet.</span><span class="sxs-lookup"><span data-stu-id="79d04-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a><span data-ttu-id="79d04-408">Definitive Zuweisungs Überprüfung</span><span class="sxs-lookup"><span data-stu-id="79d04-408">Definite assignment checking</span></span>

<span data-ttu-id="79d04-409">Puffer fester Größe unterliegen nicht der eindeutigen Zuweisungs Überprüfung ([definitive Zuweisung](variables.md#definite-assignment)), und Puffer Elemente fester Größe werden ignoriert, um eine definitive Zuweisungs Überprüfung von Strukturtyp Variablen zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="79d04-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="79d04-410">Wenn die äußerste enthaltende Struktur Variable eines Puffer Elements mit fester Größe eine statische Variable, eine Instanzvariable einer Klasseninstanz oder ein Array Element ist, werden die Elemente des Puffers fester Größe automatisch mit ihren Standardwerten initialisiert ([Standardeinstellung: Werte](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="79d04-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="79d04-411">In allen anderen Fällen ist der anfängliche Inhalt eines Puffers mit fester Größe nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="79d04-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="79d04-412">Stapel Zuordnung</span><span class="sxs-lookup"><span data-stu-id="79d04-412">Stack allocation</span></span>

<span data-ttu-id="79d04-413">In einem unsicheren Kontext kann eine lokale Variablen Deklaration ([Deklarationen von lokalen Variablen](statements.md#local-variable-declarations)) einen Stapel Zuordnungs Initialisierer enthalten, der Arbeitsspeicher aus der-aufrufsliste zuweist.</span><span class="sxs-lookup"><span data-stu-id="79d04-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="79d04-414">Der *unmanaged_type* gibt den Typ der Elemente an, die im neu zugewiesenen Speicherort gespeichert werden, und der *Ausdruck* gibt die Anzahl dieser Elemente an.</span><span class="sxs-lookup"><span data-stu-id="79d04-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="79d04-415">Diese geben die erforderliche Zuordnungs Größe an.</span><span class="sxs-lookup"><span data-stu-id="79d04-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="79d04-416">Da die Größe einer Stapel Zuordnung nicht negativ sein kann, handelt es sich um einen Kompilierzeitfehler, der die Anzahl der Elemente als *constant_expression* angibt, die einen negativen Wert ergibt.</span><span class="sxs-lookup"><span data-stu-id="79d04-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="79d04-417">Ein Stapel Zuordnungs Initialisierer der Form `stackalloc T[E]` erfordert, dass `T` ein nicht verwalteter Typ ([Zeiger Typen](unsafe-code.md#pointer-types)) und `E` ein Ausdruck des Typs `int` ist.</span><span class="sxs-lookup"><span data-stu-id="79d04-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="79d04-418">Das-Konstrukt ordnet `E * sizeof(T)` Bytes aus der-aufrufsstapel zu und gibt einen Zeiger vom Typ `T*` an den neu belegten Block zurück.</span><span class="sxs-lookup"><span data-stu-id="79d04-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="79d04-419">Wenn `E` ein negativer Wert ist, ist das Verhalten nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="79d04-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="79d04-420">Wenn `E` 0 (null) ist, wird keine Zuweisung durchgeführt, und der zurückgegebene Zeiger ist Implementierungs definiert.</span><span class="sxs-lookup"><span data-stu-id="79d04-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="79d04-421">Wenn nicht genügend Arbeitsspeicher verfügbar ist, um einen Block mit der angegebenen Größe zuzuordnen, wird eine `System.StackOverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="79d04-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="79d04-422">Der Inhalt des neu zugeordneten Speichers ist undefiniert.</span><span class="sxs-lookup"><span data-stu-id="79d04-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="79d04-423">Stapel Zuordnungs Initialisierer sind in `catch`-oder `finally`-Blöcken ([try-Anweisung](statements.md#the-try-statement)) nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="79d04-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="79d04-424">Es gibt keine Möglichkeit, Arbeitsspeicher, der mit `stackalloc` belegt wird, explizit freizugeben.</span><span class="sxs-lookup"><span data-stu-id="79d04-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="79d04-425">Alle Stapel zugeordneten Speicherblöcke, die während der Ausführung eines Funktionsmembers erstellt werden, werden automatisch verworfen, wenn der Funktions Member zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="79d04-426">Dies entspricht der Funktion "`alloca`", eine Erweiterung, die häufig in C++ C und Implementierungen gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="79d04-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="79d04-427">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="79d04-427">In the example</span></span>

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

<span data-ttu-id="79d04-428">ein `stackalloc`-Initialisierer wird in der `IntToString`-Methode verwendet, um einen Puffer mit 16 Zeichen im Stapel zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="79d04-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="79d04-429">Der Puffer wird automatisch verworfen, wenn die Methode zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="79d04-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="79d04-430">Dynamische Speicher Belegung</span><span class="sxs-lookup"><span data-stu-id="79d04-430">Dynamic memory allocation</span></span>

<span data-ttu-id="79d04-431">Mit Ausnahme des `stackalloc`-Operators C# werden von keine vordefinierten Konstrukte zum Verwalten von nicht Garbage Collection-Speicher bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="79d04-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="79d04-432">Solche Dienste werden in der Regel durch Unterstützung von Klassenbibliotheken oder direkt aus dem zugrunde liegenden Betriebssystem bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="79d04-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="79d04-433">Beispielsweise wird in der `Memory`-Klasse unten veranschaulicht, wie auf die Heap Funktionen eines zugrunde liegenden Betriebssystems C#zugegriffen werden kann:</span><span class="sxs-lookup"><span data-stu-id="79d04-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

<span data-ttu-id="79d04-434">Im folgenden finden Sie ein Beispiel, in dem die Klasse `Memory` verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="79d04-434">An example that uses the `Memory` class is given below:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

<span data-ttu-id="79d04-435">Im Beispiel werden 256 Bytes Arbeitsspeicher über `Memory.Alloc` zugewiesen und der Speicherblock mit Werten, die von 0 bis 255 steigen, initialisiert.</span><span class="sxs-lookup"><span data-stu-id="79d04-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="79d04-436">Anschließend ordnet es ein 256-Element-Bytearray zu und verwendet `Memory.Copy`, um den Inhalt des Speicherblocks in das Bytearray zu kopieren.</span><span class="sxs-lookup"><span data-stu-id="79d04-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="79d04-437">Schließlich wird der Speicherblock mithilfe von `Memory.Free` freigegeben, und der Inhalt des Bytearrays wird in der Konsole ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="79d04-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
