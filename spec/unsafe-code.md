---
ms.openlocfilehash: 90001cf3d48f216787fc65e59166ec57c5d0ca34
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229620"
---
# <a name="unsafe-code"></a><span data-ttu-id="237cf-101">Unsicherer Code</span><span class="sxs-lookup"><span data-stu-id="237cf-101">Unsafe code</span></span>

<span data-ttu-id="237cf-102">Der Kern von C#-Sprache unterscheidet sich gemäß der in den vorangegangenen Kapiteln, insbesondere von C- und C++ in Auslassen von Zeigern als Datentyp.</span><span class="sxs-lookup"><span data-stu-id="237cf-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="237cf-103">Stattdessen bietet C#-Verweise und die Möglichkeit zum Erstellen von Objekten, die von einem Garbage Collector verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="237cf-104">Aufgrund dieses Designs, zusammen mit anderen Funktionen sind C# eine viel sicherere Sprache als C- oder C++.</span><span class="sxs-lookup"><span data-stu-id="237cf-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="237cf-105">In der Kernsprache C# ist es einfach nicht möglich, dass eine nicht initialisierte Variable, eine "verwaiste" Zeiger oder ein Ausdruck, der ein Array außerhalb ihrer Grenzen indiziert.</span><span class="sxs-lookup"><span data-stu-id="237cf-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="237cf-106">Gesamte Kategorien von Fehlern kämpfen, routinemäßig C und C++-Programme werden somit entfernt.</span><span class="sxs-lookup"><span data-stu-id="237cf-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="237cf-107">Praktisch jeder Zeiger-Typ-Konstrukt in C oder C++ eine Verweis-Typ-Entsprechung in C# geschrieben wurde, sind jedoch trotzdem, gibt es Situationen, in denen Zugriff auf Zeigertypen notwendig wird.</span><span class="sxs-lookup"><span data-stu-id="237cf-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="237cf-108">Z. B. eine Schnittstelle mit dem zugrunde liegenden Betriebssystem, den Zugriff auf ein Gerät mit zugewiesenem Speicher oder einen zeitkritischen Algorithmus implementieren möglich oder praktikabel ist, ohne Zugriff auf die Zeiger möglicherweise nicht.</span><span class="sxs-lookup"><span data-stu-id="237cf-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="237cf-109">Um diesem Erfordernis entgegenzukommen, C# bietet die Möglichkeit zum Schreiben ***unsicheren Code***.</span><span class="sxs-lookup"><span data-stu-id="237cf-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="237cf-110">In unsicherem Code ist es möglich, deklarieren und Arbeiten mit Zeigern, die zum Durchführen von Konvertierungen zwischen Zeigern und ganzzahligen Typen, für die Adresse der Variablen, und so weiter.</span><span class="sxs-lookup"><span data-stu-id="237cf-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="237cf-111">In gewisser Hinsicht ähnelt das Schreiben von unsicherem Code Schreiben von C#-Code in einem C#-Programm.</span><span class="sxs-lookup"><span data-stu-id="237cf-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="237cf-112">Unsicherer Code ist in der Tat ein "sicher" Feature aus Sicht der Entwickler und Benutzer.</span><span class="sxs-lookup"><span data-stu-id="237cf-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="237cf-113">Unsicherer Code muss eindeutig gekennzeichnet werden, mit dem Modifizierer `unsafe`, sodass Entwickler können keine möglicherweise unsichere Funktionen versehentlich, und die ausführungs-Engine funktioniert, um sicherzustellen, dass die unsicherer Code in einer nicht vertrauenswürdigen Umgebung ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="237cf-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="237cf-114">Nicht sicheren Kontexten</span><span class="sxs-lookup"><span data-stu-id="237cf-114">Unsafe contexts</span></span>

<span data-ttu-id="237cf-115">Die unsichere Funktionen von C# sind nur in einem unsicheren Kontext verfügbar.</span><span class="sxs-lookup"><span data-stu-id="237cf-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="237cf-116">Ein unsicherer Kontext wird eingeführt, durch Einschließen einer `unsafe` Modifizierer in der Deklaration eines Typs oder Members oder durch den Einsatz einer *Unsafe_statement*:</span><span class="sxs-lookup"><span data-stu-id="237cf-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="237cf-117">Eine Deklaration einer Klasse, Struktur, Schnittstelle oder Delegaten eventuell eine `unsafe` Modifizierer, die in der Groß-und Kleinschreibung der gesamte Text Wertebereich diese Typdeklaration (einschließlich des Texts der Klasse, Struktur oder Schnittstelle) einen unsicheren Kontext berücksichtigt wird.</span><span class="sxs-lookup"><span data-stu-id="237cf-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="237cf-118">Eine Deklaration eines Felds, Methode, Eigenschaft, Ereignis, Indexer, Operator, Instanzenkonstruktor, Destruktor oder statischen Konstruktor enthalten möglicherweise eine `unsafe` Modifizierer verwenden, in dem Fall der gesamte Text Wertebereich, Memberdeklaration eine unsichere gilt Kontext.</span><span class="sxs-lookup"><span data-stu-id="237cf-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="237cf-119">Ein *Unsafe_statement* ermöglicht die Verwendung von einem unsicheren Kontext innerhalb einer *Block*.</span><span class="sxs-lookup"><span data-stu-id="237cf-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="237cf-120">Das Ausmaß der gesamte Text des zugeordneten *Block* einen unsicheren Kontext gilt.</span><span class="sxs-lookup"><span data-stu-id="237cf-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="237cf-121">Die zugeordneten Grammatikproduktionen werden unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="237cf-121">The associated grammar productions are shown below.</span></span>

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

<span data-ttu-id="237cf-122">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="237cf-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="237cf-123">die `unsafe` Modifizierer, die in der Strukturdeklaration angegebenen bewirkt, dass den gesamte Text Wertebereich die Strukturdeklaration zu einem unsicheren Kontext.</span><span class="sxs-lookup"><span data-stu-id="237cf-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="237cf-124">Daher ist es möglich, deklarieren die `Left` und `Right` Felder eines Zeigertyps sein.</span><span class="sxs-lookup"><span data-stu-id="237cf-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="237cf-125">Das obige Beispiel könnte auch geschrieben werden</span><span class="sxs-lookup"><span data-stu-id="237cf-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="237cf-126">Hier ist die `unsafe` Modifizierer in den Felddeklarationen dazu führen, dass diese Deklarationen nicht sicheren Kontexten berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="237cf-127">Im Gegensatz zum Einrichten eines unsicheren Kontexts, also eine ermöglicht die Verwendung von Zeigertypen, die `unsafe` Modifizierer hat keine Auswirkungen auf einen Typ oder Member.</span><span class="sxs-lookup"><span data-stu-id="237cf-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="237cf-128">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="237cf-128">In the example</span></span>

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

<span data-ttu-id="237cf-129">die `unsafe` Modifizierer für den `F` -Methode in der `A` einfach führt dazu, dass der Text der `F` zu einem unsicheren Kontext, in dem die unsicheren Funktionen der Sprache verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="237cf-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="237cf-130">Überschreiben `F` in `B`, besteht keine Notwendigkeit, geben Sie erneut die `unsafe` Modifizierer – es sei denn, natürlich die `F` -Methode in der `B` selbst benötigt Zugriff auf unsichere Funktionen.</span><span class="sxs-lookup"><span data-stu-id="237cf-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="237cf-131">Die Situation ist etwas anders, wenn ein Zeigertyp Teil der Signatur der Methode ist.</span><span class="sxs-lookup"><span data-stu-id="237cf-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

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

<span data-ttu-id="237cf-132">Hier ist da `F`Signatur enthält einen Zeigertyp, es kann nur geschrieben werden in einem unsicheren Kontext.</span><span class="sxs-lookup"><span data-stu-id="237cf-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="237cf-133">Allerdings kann der unsichere Kontext eingeführt werden, indem entweder die gesamte Klasse unsicher, wie in der Fall ist `A`, oder ein `unsafe` -Modifizierer in der Deklaration der Methode, wie in der Fall ist `B`.</span><span class="sxs-lookup"><span data-stu-id="237cf-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="237cf-134">Zeigertypen</span><span class="sxs-lookup"><span data-stu-id="237cf-134">Pointer types</span></span>

<span data-ttu-id="237cf-135">In einem unsicheren Kontext einen *Typ* ([Typen](types.md)) möglicherweise ein *Pointer_type* als auch ein *Value_type* oder *Reference_type* .</span><span class="sxs-lookup"><span data-stu-id="237cf-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="237cf-136">Allerdings eine *Pointer_type* kann ebenfalls verwendet werden, eine `typeof` Ausdruck ([anonyme Erstellung Objektausdrücke](expressions.md#anonymous-object-creation-expressions)) außerhalb eines unsicheren Kontexts als solche Verwendung ist nicht unsicher.</span><span class="sxs-lookup"><span data-stu-id="237cf-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="237cf-137">Ein *Pointer_type* wird geschrieben, als ein *Unmanaged_type* oder das Schlüsselwort `void`, gefolgt von einem `*` token:</span><span class="sxs-lookup"><span data-stu-id="237cf-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="237cf-138">Vor dem angegebenen Typ der `*` in einen Zeiger Typ wird aufgerufen, die ***Referent Typ*** des Zeigertyps.</span><span class="sxs-lookup"><span data-stu-id="237cf-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="237cf-139">Es stellt den Typ der Variablen, die auf den Wert der Zeigertyp zeigt dar.</span><span class="sxs-lookup"><span data-stu-id="237cf-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="237cf-140">Im Gegensatz zu verweisen (Werte von Verweistypen) Zeigern werden nicht vom Garbage Collector verfolgt, denn der Garbage Collector hat keine Kenntnis von Zeigern und die Daten, die auf die sie verweisen.</span><span class="sxs-lookup"><span data-stu-id="237cf-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="237cf-141">Aus diesem Grund ein Zeiger ist nicht zulässig, um auf einen Verweis zu verweisen oder auf eine Struktur, die Verweise enthält, und der Referent Typ eines Zeigers muss ein *Unmanaged_type*.</span><span class="sxs-lookup"><span data-stu-id="237cf-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="237cf-142">Ein *Unmanaged_type* ist jeder Typ, die keine *Reference_type* oder konstruierter Typ und enthält keine *Reference_type* oder auf einer beliebigen Ebene-Feldern erstellt schachteln.</span><span class="sxs-lookup"><span data-stu-id="237cf-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="237cf-143">Das heißt, eine *Unmanaged_type* ist eine der folgenden:</span><span class="sxs-lookup"><span data-stu-id="237cf-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="237cf-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, oder `bool`.</span><span class="sxs-lookup"><span data-stu-id="237cf-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="237cf-145">Alle *Enum_type*.</span><span class="sxs-lookup"><span data-stu-id="237cf-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="237cf-146">Alle *Pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="237cf-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="237cf-147">Eine benutzerdefinierte *Struct_type* , keinen konstruierten Typ und enthält Felder *Unmanaged_type*nur.</span><span class="sxs-lookup"><span data-stu-id="237cf-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="237cf-148">Die Faustregel für das Mischen von Zeiger und Verweise ist, dass Referenten von verweisen (Objekte) sind zulässig, die Zeiger enthalten, aber Referenten von Zeigern nicht zulässig sind, um die Verweise enthalten.</span><span class="sxs-lookup"><span data-stu-id="237cf-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="237cf-149">Einige Beispiele für Zeigertypen werden in der folgenden Tabelle angegeben:</span><span class="sxs-lookup"><span data-stu-id="237cf-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="237cf-150">__Beispiel__</span><span class="sxs-lookup"><span data-stu-id="237cf-150">__Example__</span></span> | <span data-ttu-id="237cf-151">__Beschreibung__</span><span class="sxs-lookup"><span data-stu-id="237cf-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="237cf-152">Zeiger auf `byte`</span><span class="sxs-lookup"><span data-stu-id="237cf-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="237cf-153">Zeiger auf `char`</span><span class="sxs-lookup"><span data-stu-id="237cf-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="237cf-154">Zeiger auf Zeiger auf `int`</span><span class="sxs-lookup"><span data-stu-id="237cf-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="237cf-155">Eindimensionales Array von Zeigern auf `int`</span><span class="sxs-lookup"><span data-stu-id="237cf-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="237cf-156">Zeiger auf unbekannten Typ</span><span class="sxs-lookup"><span data-stu-id="237cf-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="237cf-157">Für eine bestimmte Implementierung müssen alle Zeigertypen gleicher Größe und Darstellung.</span><span class="sxs-lookup"><span data-stu-id="237cf-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="237cf-158">Im Gegensatz zu C und C++, wenn mehrere Zeiger, in der gleichen Deklaration in C# deklariert werden die `*` wird zusammen mit dem zugrunde liegenden Typ, nicht als ein Präfix Punctuator für jeden Zeigernamen geschrieben.</span><span class="sxs-lookup"><span data-stu-id="237cf-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="237cf-159">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="237cf-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="237cf-160">Der Wert eines Zeigers, der mit einem Typ `T*` stellt die Adresse einer Variablen vom Typ `T`.</span><span class="sxs-lookup"><span data-stu-id="237cf-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="237cf-161">Der Zeiger-Dereferenzierungsoperator `*` ([Zeigerdereferenzierung](unsafe-code.md#pointer-indirection)) kann verwendet werden, um diese Variable zugreifen.</span><span class="sxs-lookup"><span data-stu-id="237cf-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="237cf-162">Angenommen, eine Variable `P` des Typs `int*`, den Ausdruck `*P` kennzeichnet die `int` Variablen finden Sie unter der Adresse, die in enthaltenen `P`.</span><span class="sxs-lookup"><span data-stu-id="237cf-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="237cf-163">Wie einen Objektverweis, möglicherweise ein Zeiger `null`.</span><span class="sxs-lookup"><span data-stu-id="237cf-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="237cf-164">Anwendung des Dereferenzierungsoperators auf einen `null` Zeiger führt die Implementierung definiertes Verhalten.</span><span class="sxs-lookup"><span data-stu-id="237cf-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="237cf-165">Ein Zeiger mit dem Wert `null` wird durch alle Bits auf 0 dargestellt.</span><span class="sxs-lookup"><span data-stu-id="237cf-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="237cf-166">Die `void*` Typ stellt einen Zeiger auf einen unbekannten Typ.</span><span class="sxs-lookup"><span data-stu-id="237cf-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="237cf-167">Da der Referent-Typ unbekannt ist, kann der Dereferenzierungsoperator kann nicht in einen Zeiger vom Typ angewendet werden `void*`, noch kann für einen solchen Zeiger eine arithmetische Operation ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="237cf-168">Allerdings einen Zeiger vom Typ `void*` umgewandelt werden kann, um einen anderen Zeigertyp (und umgekehrt).</span><span class="sxs-lookup"><span data-stu-id="237cf-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="237cf-169">Zeigertypen sind eine eigene Kategorie von Typen.</span><span class="sxs-lookup"><span data-stu-id="237cf-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="237cf-170">Im Gegensatz zu Verweistypen und Werttypen, Zeigertypen erben nicht von `object` und keine Konvertierung zwischen Zeigertypen und `object`.</span><span class="sxs-lookup"><span data-stu-id="237cf-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="237cf-171">Insbesondere das boxing und unboxing ([Boxing und unboxing](types.md#boxing-and-unboxing)) werden für Zeiger nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="237cf-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="237cf-172">Allerdings sind Konvertierungen zwischen verschiedenen Zeigertypen sowie zwischen Zeigertypen und ganzzahligen Typen zulässig.</span><span class="sxs-lookup"><span data-stu-id="237cf-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="237cf-173">Finden Sie im [zeigerkonvertierungen](unsafe-code.md#pointer-conversions).</span><span class="sxs-lookup"><span data-stu-id="237cf-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="237cf-174">Ein *Pointer_type* kann nicht als Typargument verwendet werden ([Typen konstruiert](types.md#constructed-types)), und den Typrückschluss ([Typrückschluss](expressions.md#type-inference)) ein Fehler auftritt, auf generische Methodenaufrufen, die abgeleitet haben würde ein Geben Sie ein Argument für ein Zeigertyp sein.</span><span class="sxs-lookup"><span data-stu-id="237cf-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="237cf-175">Ein *Pointer_type* als den Typ eines flüchtigen Felds verwendet werden kann ([flüchtige Felder](classes.md#volatile-fields)).</span><span class="sxs-lookup"><span data-stu-id="237cf-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="237cf-176">Obwohl Zeiger können, als übergeben werden `ref` oder `out` Parameter nicht definiertem Verhalten führen kann, da der Zeiger gut kann auch festgelegt werden, zeigen Sie auf eine lokale Variable, die nicht mehr vorhanden ist, wenn die aufgerufene Methode zurückgibt, oder das feste Objekt auf das Sie verweisen, ist nicht mehr festgelegt.</span><span class="sxs-lookup"><span data-stu-id="237cf-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="237cf-177">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="237cf-177">For example:</span></span>

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

<span data-ttu-id="237cf-178">Eine Methode kann einen Wert eines bestimmten Typs zurückgeben, und dieser Typ kann ein Zeiger sein.</span><span class="sxs-lookup"><span data-stu-id="237cf-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="237cf-179">Beispielsweise, wenn ein Zeiger auf eine fortlaufende Folge von angegeben `int`s, diese Sequenz die Elementanzahl und einige andere `int` Wert, die folgende Methode gibt die Adresse des entsprechenden Wertes in dieser Reihenfolge, zurück, wenn eine Übereinstimmung vorliegt; andernfalls `null`:</span><span class="sxs-lookup"><span data-stu-id="237cf-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

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

<span data-ttu-id="237cf-180">Verschiedene Konstrukte stehen in einem unsicheren Kontext für den Betrieb für Zeiger ist:</span><span class="sxs-lookup"><span data-stu-id="237cf-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="237cf-181">Die `*` Operator kann verwendet werden, um Zeigerdereferenzierung ([Zeigerdereferenzierung](unsafe-code.md#pointer-indirection)).</span><span class="sxs-lookup"><span data-stu-id="237cf-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="237cf-182">Die `->` Operator über einen Zeiger auf einen Member einer Struktur verwendet werden kann ([Zeigermemberzugriff](unsafe-code.md#pointer-member-access)).</span><span class="sxs-lookup"><span data-stu-id="237cf-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="237cf-183">Die `[]` Operator kann verwendet werden, um einen Zeiger zu indizieren ([Zeigerelementzugriff auf](unsafe-code.md#pointer-element-access)).</span><span class="sxs-lookup"><span data-stu-id="237cf-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="237cf-184">Die `&` Operator kann verwendet werden, um die Adresse einer Variablen zu erhalten ([Address-of-Operators](unsafe-code.md#the-address-of-operator)).</span><span class="sxs-lookup"><span data-stu-id="237cf-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="237cf-185">Die `++` und `--` Operatoren können verwendet werden, um Inkrementieren und Dekrementieren von Zeigern ([Zeiger Inkrementieren und Dekrementieren](unsafe-code.md#pointer-increment-and-decrement)).</span><span class="sxs-lookup"><span data-stu-id="237cf-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="237cf-186">Die `+` und `-` Operatoren können verwendet werden, um das Ausführen von Zeigerarithmetik ([Zeigerarithmetik](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="237cf-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="237cf-187">Die `==`, `!=`, `<`, `>`, `<=`, und `=>` Operatoren verwendet werden können, Vergleichen von Zeigern ([zeigervergleich](unsafe-code.md#pointer-comparison)).</span><span class="sxs-lookup"><span data-stu-id="237cf-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="237cf-188">Die `stackalloc` Operator kann verwendet werden, um die speicherbelegung in der Aufrufliste ([Puffer fester Größe](unsafe-code.md#fixed-size-buffers)).</span><span class="sxs-lookup"><span data-stu-id="237cf-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="237cf-189">Die `fixed` Anweisung kann verwendet werden, um eine Variable vorübergehend zu beheben, damit seine Adresse abgerufen werden kann ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)).</span><span class="sxs-lookup"><span data-stu-id="237cf-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="237cf-190">Feste und verschiebbare Variablen</span><span class="sxs-lookup"><span data-stu-id="237cf-190">Fixed and moveable variables</span></span>

<span data-ttu-id="237cf-191">Der Address-of-Operator ([Address-of-Operators](unsafe-code.md#the-address-of-operator)) und die `fixed` Anweisung ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)) Variablen in zwei Kategorien unterteilen: ***Variablen fester*** und ***verschiebbare Variablen***.</span><span class="sxs-lookup"><span data-stu-id="237cf-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="237cf-192">Feste Variablen befinden sich in die Speicherorte, die von den Vorgang der Garbage Collection nicht betroffen sind.</span><span class="sxs-lookup"><span data-stu-id="237cf-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="237cf-193">(Beispiele für feste Variablen sind lokale Variablen, Parameter und Variablen, die durch die Dereferenzierung von Zeigern erstellt.) Befinden auf der anderen Seite verschiebbare Variablen an externen Speicherorten, die vom Garbage Collector verschoben oder verworfen werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="237cf-194">(Beispiele für bewegliche Variablen sind die Felder in Objekte und Elemente des Arrays an.)</span><span class="sxs-lookup"><span data-stu-id="237cf-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="237cf-195">Die `&` Operator ([Address-of-Operators](unsafe-code.md#the-address-of-operator)) ermöglicht die Adresse des eine feste Variable ohne Einschränkungen abgerufen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="237cf-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="237cf-196">Aber da verschoben oder verworfen, die vom Garbage Collector eine bewegliche Variable ist, die Adresse des eine bewegliche Variable nur abgerufen werden kann mithilfe einer `fixed` Anweisung ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)), und diese Adresse bleibt gültig, nur für die Dauer dieser `fixed` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="237cf-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="237cf-197">Eine feste Variable ist präzise ausgedrückt eine der folgenden:</span><span class="sxs-lookup"><span data-stu-id="237cf-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="237cf-198">Eine Variable aufgrund einer *Simple_name* ([einfache Namen](expressions.md#simple-names)), die auf eine lokale Variable oder ein Value-Parameter, es sei denn, die Variable eine anonyme Funktion erfasst wird.</span><span class="sxs-lookup"><span data-stu-id="237cf-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="237cf-199">Eine Variable aufgrund einer *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `V.I`, wobei `V` ist eine feste Variable von einem *Struct_type*.</span><span class="sxs-lookup"><span data-stu-id="237cf-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="237cf-200">Eine Variable aufgrund einer *Pointer_indirection_expression* ([Zeigerdereferenzierung](unsafe-code.md#pointer-indirection)) des Formulars `*P`, *Pointer_member_access* ([Zeigermemberzugriff](unsafe-code.md#pointer-member-access)) des Formulars `P->I`, oder ein *Pointer_element_access* ([Zeigerelementzugriff auf](unsafe-code.md#pointer-element-access)) des Formulars `P[E]`.</span><span class="sxs-lookup"><span data-stu-id="237cf-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="237cf-201">Alle anderen Variablen werden als bewegliche Variablen klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="237cf-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="237cf-202">Beachten Sie, dass ein statisches Feld als bewegliche Variable klassifiziert wurde.</span><span class="sxs-lookup"><span data-stu-id="237cf-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="237cf-203">Beachten Sie, dass eine `ref` oder `out` Parameter wird als eine bewegliche Variable klassifiziert, selbst wenn das Argument für den Parameter erhält eine feste Variable ist.</span><span class="sxs-lookup"><span data-stu-id="237cf-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="237cf-204">Beachten Sie außerdem, dass eine Variable, die durch einen Zeiger dereferenziert erzeugt immer als eine feste Variable klassifiziert wurde.</span><span class="sxs-lookup"><span data-stu-id="237cf-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="237cf-205">Zeigerkonvertierungen</span><span class="sxs-lookup"><span data-stu-id="237cf-205">Pointer conversions</span></span>

<span data-ttu-id="237cf-206">In einem unsicheren Kontext, den Satz von verfügbaren impliziten Konvertierungen ([implizite Konvertierungen](conversions.md#implicit-conversions)) wird erweitert, um die folgenden implizite zeigerkonvertierungen einzuschließen:</span><span class="sxs-lookup"><span data-stu-id="237cf-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="237cf-207">Von jedem *Pointer_type* in den Typ `void*`.</span><span class="sxs-lookup"><span data-stu-id="237cf-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="237cf-208">Von der `null` in einen literalen *Pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="237cf-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="237cf-209">Darüber hinaus in einem unsicheren Kontext, den Satz von verfügbaren explizite Konvertierungen ([explizite Konvertierungen](conversions.md#explicit-conversions)) wurde erweitert und die folgende explizite zeigerkonvertierungen enthalten:</span><span class="sxs-lookup"><span data-stu-id="237cf-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="237cf-210">Von jedem *Pointer_type* auch einer beliebigen anderen *Pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="237cf-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="237cf-211">Von `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, oder `ulong` auf *Pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="237cf-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="237cf-212">Von jedem *Pointer_type* zu `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, oder `ulong`.</span><span class="sxs-lookup"><span data-stu-id="237cf-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="237cf-213">Schließlich wird in einem unsicheren Kontext, den Satz von standardmäßigen implizite Konvertierungen ([Standard implizite Konvertierungen](conversions.md#standard-implicit-conversions)) enthält die folgende zeigerkonvertierung:</span><span class="sxs-lookup"><span data-stu-id="237cf-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="237cf-214">Von jedem *Pointer_type* in den Typ `void*`.</span><span class="sxs-lookup"><span data-stu-id="237cf-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="237cf-215">Konvertierungen zwischen zwei Typen von Funktionszeigern ändern sich nie den eigentlichen Zeigerwert.</span><span class="sxs-lookup"><span data-stu-id="237cf-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="237cf-216">Das heißt, hat eine Konvertierung von einem Zeigertyp in einen anderen keine Auswirkungen auf die zugrunde liegenden Adresse des Zeigers.</span><span class="sxs-lookup"><span data-stu-id="237cf-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="237cf-217">Wenn ein Zeigertyp in eine andere konvertiert wird, wenn der resultierende Zeiger für den Typ auf nicht korrekt ausgerichtet ist, ist das Verhalten undefiniert, wenn das Ergebnis dereferenziert wird.</span><span class="sxs-lookup"><span data-stu-id="237cf-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="237cf-218">Im Allgemeinen ist das Konzept "korrekt ausgerichtet" transitiv: Wenn ein Zeiger auf den Typ `A` wird für einen Zeiger auf den Typ richtig ausgerichtet `B`, das wiederum wird für einen Zeiger auf den Typ richtig ausgerichtet `C`, klicken Sie dann einen Zeiger auf Typ `A`wird für einen Zeiger auf den Typ richtig ausgerichtet `C`.</span><span class="sxs-lookup"><span data-stu-id="237cf-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="237cf-219">Betrachten Sie den folgenden Fall, in dem eine Variable eines bestimmten Typs, über einen Zeiger auf einen anderen Typ zugegriffen wird, aus:</span><span class="sxs-lookup"><span data-stu-id="237cf-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="237cf-220">Wenn ein Zeigertyp in einen Zeiger auf Byte, zeigt das Ergebnis, das niedrigste adressierte Byte der Variablen konvertiert.</span><span class="sxs-lookup"><span data-stu-id="237cf-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="237cf-221">Aufeinander folgenden Schritten des Ergebnisses wird bis zur Größe der Variablen, ergeben die Zeiger auf die verbleibenden Bytes der Variablen.</span><span class="sxs-lookup"><span data-stu-id="237cf-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="237cf-222">Beispielsweise zeigt die folgende Methode jedes Byte acht in einen Double-Wert als hexadezimaler Wert:</span><span class="sxs-lookup"><span data-stu-id="237cf-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

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

<span data-ttu-id="237cf-223">Natürlich hängt die Ausgabe von Bytereihenfolge.</span><span class="sxs-lookup"><span data-stu-id="237cf-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="237cf-224">Zuordnungen zwischen Zeigern und ganze Zahlen werden in der Implementierung definiert.</span><span class="sxs-lookup"><span data-stu-id="237cf-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="237cf-225">Jedoch auf 32 \* und 64-Bit-CPU-Architekturen mit einem linearen Adressraums, Konvertierungen von Zeigern zu oder von ganzzahligen Typen in der Regel Verhalten sich genau wie Konvertierungen von `uint` oder `ulong` -Werte an oder von diesen ganzzahligen Typen.</span><span class="sxs-lookup"><span data-stu-id="237cf-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="237cf-226">Zeiger-arrays</span><span class="sxs-lookup"><span data-stu-id="237cf-226">Pointer arrays</span></span>

<span data-ttu-id="237cf-227">In einem unsicheren Kontext können Arrays von Zeigern erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="237cf-228">Nur einige der Konvertierungen, die für andere Arraytypen gelten, sind für Zeiger Arrays zulässig:</span><span class="sxs-lookup"><span data-stu-id="237cf-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="237cf-229">Die implizite verweiskonvertierung ([implizite Verweis-](conversions.md#implicit-reference-conversions)) aus allen *Array_type* zu `System.Array` und die Schnittstellen, die es implementiert auch gilt, für Zeiger-Arrays.</span><span class="sxs-lookup"><span data-stu-id="237cf-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="237cf-230">Jedoch jeder Versuch, auf die Elemente des Arrays durch `System.Array` oder die Schnittstellen implementiert führt zur Laufzeit eine Ausnahme, da Zeigertypen nicht konvertierbar sind `object`.</span><span class="sxs-lookup"><span data-stu-id="237cf-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="237cf-231">Verweisen Sie die implizite und explizite Konvertierungen ([implizite Verweis-](conversions.md#implicit-reference-conversions), [explizite Konvertierungen](conversions.md#explicit-reference-conversions)) aus einem eindimensionalen Array-Typ `S[]` zu `System.Collections.Generic.IList<T>` und die generische Basisschnittstellen gelten nicht für Zeiger-Arrays, auf, da Zeigertypen nicht als Typargumente verwendet werden, und es keine Konvertierungen von Zeigertypen auf Nichtzeiger-Typen gibt.</span><span class="sxs-lookup"><span data-stu-id="237cf-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="237cf-232">Die explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-reference-conversions)) von `System.Array` und die Schnittstellen implementiert, alle *Array_type* gelten für Zeiger-Arrays.</span><span class="sxs-lookup"><span data-stu-id="237cf-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="237cf-233">Die explizite Konvertierungen ([explizite Konvertierungen](conversions.md#explicit-reference-conversions)) von `System.Collections.Generic.IList<S>` und Basis Schnittstellen verwenden, um einen eindimensionalen Arraytyp `T[]` wird nicht für Zeiger-Arrays, da Zeigertypen sein darf nicht verwendet als Typargumente ein, und es gibt keine Konvertierungen von Zeigertypen auf Nichtzeiger-Typen.</span><span class="sxs-lookup"><span data-stu-id="237cf-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="237cf-234">Diese Einschränkungen bedeuten, dass die Erweiterung für die `foreach` Anweisung über Arrays in beschriebenen [der Foreach-Anweisung](statements.md#the-foreach-statement) nicht auf Zeiger-Arrays angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="237cf-235">Stattdessen eine Foreach-Anweisung der Form</span><span class="sxs-lookup"><span data-stu-id="237cf-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="237cf-236">in denen der Typ des `x` ist ein Arraytyp des Formulars `T[,,...,]`, `N` ist die Anzahl der Dimensionen minus 1 und `T` oder `V` ist ein Typ, mit geschachtelten for-Schleifen wie folgt erweitert:</span><span class="sxs-lookup"><span data-stu-id="237cf-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

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

<span data-ttu-id="237cf-237">Die Variablen `a`, `i0`, `i1`,..., `iN` sind nicht sichtbar und zugänglich `x` oder *Embedded_statement* oder jeden anderen Quellcode des Programms.</span><span class="sxs-lookup"><span data-stu-id="237cf-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="237cf-238">Die Variable `v` in die eingebettete Anweisung schreibgeschützt ist.</span><span class="sxs-lookup"><span data-stu-id="237cf-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="237cf-239">Wenn eine explizite Konvertierung nicht vorhanden ist ([zeigerkonvertierungen](unsafe-code.md#pointer-conversions)) von `T` (der Typ des Elements), `V`, ein Fehler erzeugt wird und keine weiteren Schritte ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="237cf-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="237cf-240">Wenn `x` hat den Wert `null`, `System.NullReferenceException` wird zur Laufzeit ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="237cf-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="237cf-241">Zeiger in Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="237cf-241">Pointers in expressions</span></span>

<span data-ttu-id="237cf-242">Klicken Sie in einem unsicheren Kontext ein Ausdruck kann ein Ergebnis von einem Zeigertyp, aber außerhalb eines unsicheren Kontexts, die es ist ein Fehler während der Kompilierung für einen Ausdruck, der ein Zeigertyp sein.</span><span class="sxs-lookup"><span data-stu-id="237cf-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="237cf-243">Präzise ausgedrückt, außerhalb eines unsicheren Kontexts ein Kompilierungsfehler tritt auf, wenn alle *Simple_name* ([einfache Namen](expressions.md#simple-names)), *Member_access* ([Memberzugriff ](expressions.md#member-access)), *Invocation_expression* ([Aufrufausdrücke](expressions.md#invocation-expressions)), oder *Element_access* ([Elementzugriff](expressions.md#element-access)) ist ein Zeigertyp.</span><span class="sxs-lookup"><span data-stu-id="237cf-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="237cf-244">In einem unsicheren Kontext der *Primary_no_array_creation_expression* ([primärausdrücke](expressions.md#primary-expressions)) und *Unary_expression* ([unäre Operatoren](expressions.md#unary-operators)) Produktionen ermöglichen die folgenden zusätzliche Konstrukte:</span><span class="sxs-lookup"><span data-stu-id="237cf-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

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

<span data-ttu-id="237cf-245">Diese Konstrukte werden in den folgenden Abschnitten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="237cf-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="237cf-246">Die Rangfolge und Assoziativität der unsicheren Operatoren wird von der Grammatik impliziert.</span><span class="sxs-lookup"><span data-stu-id="237cf-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="237cf-247">Zeigerdereferenzierung</span><span class="sxs-lookup"><span data-stu-id="237cf-247">Pointer indirection</span></span>

<span data-ttu-id="237cf-248">Ein *Pointer_indirection_expression* besteht aus einem Sternchen (`*`) gefolgt von einem *Unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="237cf-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="237cf-249">Der unäre `*` Operator Zeigerdereferenzierung bezeichnet und wird verwendet, um die Variable zu ermitteln, zu dem ein Zeiger zeigt.</span><span class="sxs-lookup"><span data-stu-id="237cf-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="237cf-250">Das Ergebnis der Auswertung `*P`, wobei `P` ist ein Ausdruck eines Zeigertyps `T*`, eine Variable vom Typ `T`.</span><span class="sxs-lookup"><span data-stu-id="237cf-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="237cf-251">Es ist ein Fehler während der Kompilierung der unäre anzuwendende `*` Operator, um ein Ausdruck vom Typ `void*` oder auf einen Ausdruck, der einen Zeigertyp ist.</span><span class="sxs-lookup"><span data-stu-id="237cf-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="237cf-252">Der Effekt der Anwendung der unäre `*` Operator, um eine `null` Zeiger wird durch die Implementierung definiert.</span><span class="sxs-lookup"><span data-stu-id="237cf-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="237cf-253">Insbesondere besteht keine Garantie, die dieser Vorgang löst einen `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="237cf-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="237cf-254">Wenn ein ungültiger Wert auf den Zeiger, die das Verhalten der unären zugewiesen wurde `*` Operator nicht definiert ist.</span><span class="sxs-lookup"><span data-stu-id="237cf-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="237cf-255">Auf die ungültige Werte für einen Zeiger dereferenziert, indem Sie den unären `*` Operator werden eine Adresse, die nicht ordnungsgemäß ausgerichtet werden, für der Typ, zeigt (siehe Beispiel in [zeigerkonvertierungen](unsafe-code.md#pointer-conversions)), und die Adresse einer Variablen nach der Ende ihrer Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="237cf-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="237cf-256">Für Zwecke der definite Assignment Analyse, eine Variablen, die durch die Auswertung eines Ausdrucks des Formulars `*P` gilt als ursprünglich zugewiesen ([anfänglich zugewiesene Variablen](variables.md#initially-assigned-variables)).</span><span class="sxs-lookup"><span data-stu-id="237cf-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="237cf-257">Zeigermemberzugriff</span><span class="sxs-lookup"><span data-stu-id="237cf-257">Pointer member access</span></span>

<span data-ttu-id="237cf-258">Ein *Pointer_member_access* besteht aus einer *Primary_expression*, gefolgt von einer "`->`" token, gefolgt von einer *Bezeichner* und eine optionale *Type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="237cf-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="237cf-259">In einem Zeigermemberzugriff des Formulars `P->I`, `P` muss ein Ausdruck eines Zeigertyps außer `void*`, und `I` müssen einen verfügbaren Member des Typs, der dem kennzeichnen `P` Punkte.</span><span class="sxs-lookup"><span data-stu-id="237cf-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="237cf-260">Ein Zeigermemberzugriff des Formulars `P->I` wird ausgewertet, genau wie `(*P).I`.</span><span class="sxs-lookup"><span data-stu-id="237cf-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="237cf-261">Eine Beschreibung der Zeiger-Dereferenzierungsoperator (`*`), finden Sie unter [Zeigerdereferenzierung](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="237cf-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="237cf-262">Eine Beschreibung der den Memberzugriffsoperator (`.`), finden Sie unter [Memberzugriff](expressions.md#member-access).</span><span class="sxs-lookup"><span data-stu-id="237cf-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="237cf-263">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="237cf-263">In the example</span></span>

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

<span data-ttu-id="237cf-264">die `->` Operator wird verwendet, um den Zugriff auf Felder und Aufrufen einer Methode, einer Struktur über einen Zeiger.</span><span class="sxs-lookup"><span data-stu-id="237cf-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="237cf-265">Da der Vorgang `P->I` entspricht exakt dem `(*P).I`, `Main` Methode kann ebenso gut geschrieben wurden:</span><span class="sxs-lookup"><span data-stu-id="237cf-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

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

### <a name="pointer-element-access"></a><span data-ttu-id="237cf-266">Zeigerelementzugriff auf</span><span class="sxs-lookup"><span data-stu-id="237cf-266">Pointer element access</span></span>

<span data-ttu-id="237cf-267">Ein *Pointer_element_access* besteht aus einem *Primary_no_array_creation_expression* gefolgt von einem Ausdruck, der eingeschlossen in "`[`"und"`]`".</span><span class="sxs-lookup"><span data-stu-id="237cf-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="237cf-268">In einem Zeigerelementzugriff des Formulars `P[E]`, `P` muss ein Ausdruck eines Zeigertyps außer `void*`, und `E` muss ein Ausdruck, der implizit in konvertiert werden kann `int`, `uint`, `long`, oder `ulong`.</span><span class="sxs-lookup"><span data-stu-id="237cf-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="237cf-269">Ein Zeiger auf der Form `P[E]` wird ausgewertet, genau wie `*(P + E)`.</span><span class="sxs-lookup"><span data-stu-id="237cf-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="237cf-270">Eine Beschreibung der Zeiger-Dereferenzierungsoperator (`*`), finden Sie unter [Zeigerdereferenzierung](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="237cf-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="237cf-271">Eine Beschreibung des Zeigeradditionsoperators (`+`), finden Sie unter [Zeigerarithmetik](unsafe-code.md#pointer-arithmetic).</span><span class="sxs-lookup"><span data-stu-id="237cf-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="237cf-272">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="237cf-272">In the example</span></span>

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

<span data-ttu-id="237cf-273">Ein Zeiger auf wird verwendet, um den Zeichenpuffer in zu initialisieren. eine `for` Schleife.</span><span class="sxs-lookup"><span data-stu-id="237cf-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="237cf-274">Da der Vorgang `P[E]` entspricht exakt dem `*(P + E)`, im Beispiel kann ebenso gut geschrieben wurden:</span><span class="sxs-lookup"><span data-stu-id="237cf-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

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

<span data-ttu-id="237cf-275">Der Zeiger Element Access-Operator nicht außerhalb des gültigen Bereichs nach Fehlern und das Verhalten beim Zugriff auf ein Element außerhalb des gültigen Bereichs ist nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="237cf-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="237cf-276">Dies ist identisch mit C- und C++.</span><span class="sxs-lookup"><span data-stu-id="237cf-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="237cf-277">Der Address-of-operator</span><span class="sxs-lookup"><span data-stu-id="237cf-277">The address-of operator</span></span>

<span data-ttu-id="237cf-278">Ein *Addressof_expression* besteht aus einem kaufmännischen und-Zeichen (`&`) gefolgt von einem *Unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="237cf-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="237cf-279">Erhält einen Ausdruck `E` die ist ein Typ `T` und wird als eine feste Variable klassifiziert ([für feste und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)), das Konstrukt `&E` berechnet die Adresse der Variablen vom `E`.</span><span class="sxs-lookup"><span data-stu-id="237cf-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="237cf-280">Der Typ des Ergebnisses ist `T*` und wird als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="237cf-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="237cf-281">Ein Fehler während der Kompilierung tritt auf, wenn `E` wird nicht als Variable klassifiziert, wenn `E` wird als schreibgeschützte lokale Variable klassifiziert oder, wenn `E` eine bewegliche Variable bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="237cf-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="237cf-282">Im letzten Fall einer fixed-Anweisung ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)) kann verwendet werden, um vorübergehend "die Variable beheben", bevor Sie ihre Adresse abzurufen.</span><span class="sxs-lookup"><span data-stu-id="237cf-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="237cf-283">Wie in der angegeben [Memberzugriff](expressions.md#member-access), außerhalb eines Instanzkonstruktors oder einen statischen Konstruktor für eine Struktur oder Klasse, die definiert eine `readonly` Feld, in dieses Feld gilt einen Wert, nicht auf eine Variable.</span><span class="sxs-lookup"><span data-stu-id="237cf-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="237cf-284">Daher kann nicht die Adresse nicht ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="237cf-285">Die Adresse einer Konstanten können auf ähnliche Weise kann nicht ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="237cf-286">Die `&` Operator erfordert nicht das Argument in der definitiv zugewiesen werden, jedoch folgende eine `&` -Operation, die Variable, die auf die der Operator angewendet wird gilt als definitiv zugewiesen, in den Ausführungspfad, in dem der Vorgang auftritt.</span><span class="sxs-lookup"><span data-stu-id="237cf-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="237cf-287">Es handelt sich um die Verantwortung des Programmierers sicherzustellen, dass die richtige Initialisierung der Variable tatsächlich Stelle in diesem Fall übernimmt.</span><span class="sxs-lookup"><span data-stu-id="237cf-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="237cf-288">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="237cf-288">In the example</span></span>

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

<span data-ttu-id="237cf-289">`i` gilt als definitiv zugewiesen, nach der `&i` Vorgang, der zum Initialisieren verwendet `p`.</span><span class="sxs-lookup"><span data-stu-id="237cf-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="237cf-290">Die Zuweisung zu `*p` faktisch initialisiert `i`, aber der Einbindung dieser Initialisierung liegt in der Verantwortung des Programmierers und keine Kompilierungsfehler tritt auf, wenn die Zuweisung entfernt wurde.</span><span class="sxs-lookup"><span data-stu-id="237cf-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="237cf-291">Die Regeln für definitive Zuweisungen für die `&` Operator vorhanden sein, dass redundante Initialisierung von lokalen Variablen kann vermieden werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="237cf-292">Viele externe APIs werden z. B. einen Zeiger auf eine Struktur, die von der API angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="237cf-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="237cf-293">Aufrufe dieser APIs in der Regel übergeben Sie die Adresse einer Struktur von lokalen Variablen und ohne die Regel, redundante Initialisierung der Strukturvariable wären erforderlich.</span><span class="sxs-lookup"><span data-stu-id="237cf-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="237cf-294">Zeiger Inkrementieren und Dekrementieren</span><span class="sxs-lookup"><span data-stu-id="237cf-294">Pointer increment and decrement</span></span>

<span data-ttu-id="237cf-295">In einem unsicheren Kontext der `++` und `--` Operatoren ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators) und [Präfix-Inkrement und Dekrement-Operatoren](expressions.md#prefix-increment-and-decrement-operators)) können auf Zeiger angewendet werden Variablen aller Typen mit Ausnahme von `void*`.</span><span class="sxs-lookup"><span data-stu-id="237cf-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="237cf-296">Daher ist es bei jedem Zeigertyp `T*`, die folgenden Operatoren werden implizit definiert:</span><span class="sxs-lookup"><span data-stu-id="237cf-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="237cf-297">Die Operatoren erzeugen die gleichen Ergebnisse wie `x + 1` und `x - 1`bzw. ([Zeigerarithmetik](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="237cf-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="237cf-298">In anderen Worten: für eine Zeigervariable des Typs `T*`, `++` -Operator fügt hinzu, `sizeof(T)` an die Adresse, die in der Variablen enthalten und die `--` Operator subtrahiert `sizeof(T)` von der Adresse, die in der Variablen enthalten.</span><span class="sxs-lookup"><span data-stu-id="237cf-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="237cf-299">Wenn Sie eine Zeiger-Inkrement oder Dekrement Operation die Domänengrenzen des Zeigertyps, das Ergebnis ist die Implementierung definiert, aber keine Ausnahmen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="237cf-300">Zeigerarithmetik</span><span class="sxs-lookup"><span data-stu-id="237cf-300">Pointer arithmetic</span></span>

<span data-ttu-id="237cf-301">In einem unsicheren Kontext der `+` und `-` Operatoren ([Additionsoperator](expressions.md#addition-operator) und [Subtraktionsoperator](expressions.md#subtraction-operator)) auf Werte mit Ausnahme von alle Zeigertypen angewendet werden können `void*`.</span><span class="sxs-lookup"><span data-stu-id="237cf-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="237cf-302">Daher ist es bei jedem Zeigertyp `T*`, die folgenden Operatoren werden implizit definiert:</span><span class="sxs-lookup"><span data-stu-id="237cf-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

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

<span data-ttu-id="237cf-303">Erhält einen Ausdruck `P` eines Zeigertyps `T*` und einen Ausdruck `N` des Typs `int`, `uint`, `long`, oder `ulong`, die Ausdrücke `P + N` und `N + P` Berechnen der Zeigerwert des Typs `T*` , die sich aus der Addition ergibt `N * sizeof(T)` an die Adresse, die vom `P`.</span><span class="sxs-lookup"><span data-stu-id="237cf-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="237cf-304">Entsprechend dem Ausdruck `P - N` berechnet den Wert des Zeigers des Typs `T*` , die sich aus der Subtraktion ergibt `N * sizeof(T)` von der Adresse, die vom `P`.</span><span class="sxs-lookup"><span data-stu-id="237cf-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="237cf-305">Mit den Ausdrücken `P` und `Q`, eines Zeigertyps `T*`, den Ausdruck `P - Q` berechnet die Differenz zwischen den Adressen, die vom `P` und `Q` und klicken Sie dann diesen Unterschied von `sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="237cf-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="237cf-306">Der Typ des Ergebnisses ist immer `long`.</span><span class="sxs-lookup"><span data-stu-id="237cf-306">The type of the result is always `long`.</span></span> <span data-ttu-id="237cf-307">Tatsächlich `P - Q` wird berechnet als `((long)(P) - (long)(Q)) / sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="237cf-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="237cf-308">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="237cf-308">For example:</span></span>

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

<span data-ttu-id="237cf-309">die Ausgabe erzeugt:</span><span class="sxs-lookup"><span data-stu-id="237cf-309">which produces the output:</span></span>

```
p - q = -14
q - p = 14
```

<span data-ttu-id="237cf-310">Wenn eine arithmetische Operation der Zeiger auf die Domäne des Zeigertyps überläuft, das Ergebnis wird auf eine Weise implementierungsdefinierte abgeschnitten, aber keine Ausnahmen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="237cf-311">Zeigervergleich</span><span class="sxs-lookup"><span data-stu-id="237cf-311">Pointer comparison</span></span>

<span data-ttu-id="237cf-312">In einem unsicheren Kontext der `==`, `!=`, `<`, `>`, `<=`, und `=>` Operatoren ([Relational und Typtest Operatoren](expressions.md#relational-and-type-testing-operators)) kann auf alle Werte angewendet werden Zeigertypen.</span><span class="sxs-lookup"><span data-stu-id="237cf-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="237cf-313">Die Vergleichsoperatoren für Zeiger sind:</span><span class="sxs-lookup"><span data-stu-id="237cf-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="237cf-314">Da eine implizite Konvertierung in einen Zeigertyp, vorhanden ist. die `void*` Typ Operanden vom Zeigertyp mithilfe dieser Operatoren verglichen werden kann.</span><span class="sxs-lookup"><span data-stu-id="237cf-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="237cf-315">Die Vergleichsoperatoren vergleichen die Adressen, die durch die beiden Operanden angegeben wird, als wären sie ganze Zahlen ohne Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="237cf-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="237cf-316">Der Operator sizeof</span><span class="sxs-lookup"><span data-stu-id="237cf-316">The sizeof operator</span></span>

<span data-ttu-id="237cf-317">Die `sizeof` -Operator gibt die Anzahl der Bytes, die durch eine Variable eines bestimmten Typs belegt wird.</span><span class="sxs-lookup"><span data-stu-id="237cf-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="237cf-318">Der Typ als Operand für `sizeof` muss ein *Unmanaged_type* ([Zeigertypen](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="237cf-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="237cf-319">Das Ergebnis der `sizeof` Operator ist ein Wert vom Typ `int`.</span><span class="sxs-lookup"><span data-stu-id="237cf-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="237cf-320">Für bestimmte vordefinierte Typen, die `sizeof` -Operator liefert einen konstanten Wert an, wie in der folgenden Tabelle gezeigt.</span><span class="sxs-lookup"><span data-stu-id="237cf-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="237cf-321">__Expression (Ausdruck)__</span><span class="sxs-lookup"><span data-stu-id="237cf-321">__Expression__</span></span>   | <span data-ttu-id="237cf-322">__Ergebnis__</span><span class="sxs-lookup"><span data-stu-id="237cf-322">__Result__</span></span> |
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

<span data-ttu-id="237cf-323">Für alle anderen Dateitypen, das Ergebnis der `sizeof` Operator wird durch die Implementierung definiert und wird als Wert und kein Konstante klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="237cf-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="237cf-324">Die Reihenfolge, in der Elemente in einer Struktur gepackt werden, ist nicht angegeben.</span><span class="sxs-lookup"><span data-stu-id="237cf-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="237cf-325">Für die Ausrichtung möglicherweise benannte Füllzeichen am Anfang einer Struktur, in eine Struktur, und am Ende der Struktur.</span><span class="sxs-lookup"><span data-stu-id="237cf-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="237cf-326">Der Inhalt der Bits, die als Abstand verwendet ist unbestimmt.</span><span class="sxs-lookup"><span data-stu-id="237cf-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="237cf-327">Wenn auf einen Operanden angewendet, die Struktur verfügt, ist das Ergebnis die Gesamtzahl der Bytes in einer Variablen des Typs, einschließlich der Abstände an.</span><span class="sxs-lookup"><span data-stu-id="237cf-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="237cf-328">Die fixed-Anweisung</span><span class="sxs-lookup"><span data-stu-id="237cf-328">The fixed statement</span></span>

<span data-ttu-id="237cf-329">In einem unsicheren Kontext der *Embedded_statement* ([Anweisungen](statements.md)) Produktion ermöglicht ein zusätzliches Konstrukt, das `fixed` -Anweisung, die verwendet wird, um "eine bewegliche Variable korrigieren" so, dass die Adresse bleibt unverändert, für die Dauer der Anweisung.</span><span class="sxs-lookup"><span data-stu-id="237cf-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

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

<span data-ttu-id="237cf-330">Jede *Fixed_pointer_declarator* deklariert eine lokale Variable mit der angegebenen *Pointer_type* und initialisiert die lokale Variable mit der Adresse berechnet, indem Sie die entsprechende *Fixed_ Pointer_initializer*.</span><span class="sxs-lookup"><span data-stu-id="237cf-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="237cf-331">Eine lokale Variable deklariert, die eine `fixed` -Anweisung ist in jedem zugänglich *Fixed_pointer_initializer*s auf der rechten Seite der Deklaration dieser Variablen die, und in der *Embedded_statement* von der `fixed` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="237cf-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="237cf-332">Eine lokale Variable deklariert, indem eine `fixed` Anweisung ist schreibgeschützt.</span><span class="sxs-lookup"><span data-stu-id="237cf-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="237cf-333">Ein Fehler während der Kompilierung tritt auf, wenn die eingebettete Anweisung versucht, diese lokale Variable zu ändern (per Zuweisung oder `++` und `--` Operatoren) oder übergeben Sie sie als eine `ref` oder `out` Parameter.</span><span class="sxs-lookup"><span data-stu-id="237cf-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="237cf-334">Ein *Fixed_pointer_initializer* kann einen der folgenden sein:</span><span class="sxs-lookup"><span data-stu-id="237cf-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="237cf-335">Das Token "`&`" gefolgt von einem *Variable_reference* ([präzise Regeln für definitive Zuweisungen bestimmen](variables.md#precise-rules-for-determining-definite-assignment)) auf eine bewegliche Variable ([für feste und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)) eines nicht verwalteten Typs `T`, der Typ `T*` wird implizit in den Zeigertyp den `fixed` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="237cf-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="237cf-336">In diesem Fall berechnet die Initialisierung der Adresse der angegebenen Variablen und die Variable ist garantiert an fester Adresse für die Dauer der `fixed` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="237cf-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="237cf-337">Ein Ausdruck, der eine *Array_type* mit Elementen eines nicht verwalteten Typs `T`, der Typ `T*` wird implizit in den Zeigertyp die `fixed` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="237cf-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="237cf-338">In diesem Fall berechnet die Initialisierung der Adresse des ersten Elements im Array und das gesamte Array ist garantiert an fester Adresse für die Dauer der `fixed` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="237cf-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="237cf-339">Wenn der Ausdruck null ist oder wenn das Array 0 (null) Elemente verfügt, berechnet der Initialisierer einer Adresse gleich 0 (null).</span><span class="sxs-lookup"><span data-stu-id="237cf-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="237cf-340">Ein Ausdruck vom Typ `string`, der Typ `char*` wird implizit in den Zeigertyp den `fixed` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="237cf-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="237cf-341">In diesem Fall berechnet die Adresse des ersten Zeichens in der Zeichenfolge für die Initialisierung und die gesamte Zeichenfolge ist garantiert an fester Adresse für die Dauer der `fixed` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="237cf-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="237cf-342">Das Verhalten der `fixed` Anweisung ist Implementierung definiert, wenn der Ausdruck null ist.</span><span class="sxs-lookup"><span data-stu-id="237cf-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="237cf-343">Ein *Simple_name* oder *Member_access* , die verweist auf einen Member fester Größe Puffer eine bewegliche Variable, sofern der Typ des Members Puffer fester Größe implizit in den Zeigertyp ist. in der `fixed` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="237cf-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="237cf-344">In diesem Fall berechnet die Initialisierung für einen Zeiger auf das erste Element des Puffers fester Größe ([Puffer fester Größe in Ausdrücken](unsafe-code.md#fixed-size-buffers-in-expressions)), und der Puffer mit fester Größe ist garantiert an fester Adresse für die Dauer der `fixed`Anweisung.</span><span class="sxs-lookup"><span data-stu-id="237cf-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="237cf-345">Für jede Adresse berechnet, indem eine *Fixed_pointer_initializer* der `fixed` Anweisung wird sichergestellt, dass die Variable verwiesen wird, durch die Adresse nicht verschoben oder verworfen, die vom Garbage Collector für die Dauer der `fixed` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="237cf-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="237cf-346">Wenn die Adresse berechnet, indem Sie z. B. eine *Fixed_pointer_initializer* verweist auf ein Feld eines Objekts oder ein Element einer Instanz des Arrays, die `fixed` -Anweisung wird sichergestellt, dass die enthaltende Objektinstanz nicht verschoben wird oder während der Lebensdauer der Anweisung verworfen.</span><span class="sxs-lookup"><span data-stu-id="237cf-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="237cf-347">Es ist der Programmierer dafür verantwortlich, um sicherzustellen, dass der Zeiger von erstellt `fixed` Anweisungen bleiben nach Ausführung dieser Anweisungen nicht.</span><span class="sxs-lookup"><span data-stu-id="237cf-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="237cf-348">Z. B. Zeiger der Erstellung von `fixed` Anweisungen an externen APIs übergeben werden, es ist der Programmierer dafür verantwortlich, um sicherzustellen, dass die APIs keinen Speicher für diese Zeiger beibehalten.</span><span class="sxs-lookup"><span data-stu-id="237cf-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="237cf-349">Feste Objekte möglicherweise die Fragmentierung des Heaps (da sie nicht verschoben werden können).</span><span class="sxs-lookup"><span data-stu-id="237cf-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="237cf-350">Aus diesem Grund sollten Objekte nur wenn unbedingt nötig behoben werden und dann nur für kürzestmöglicher Zeit möglich.</span><span class="sxs-lookup"><span data-stu-id="237cf-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="237cf-351">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="237cf-351">The example</span></span>

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

<span data-ttu-id="237cf-352">veranschaulicht verschiedene Verwendungen von der `fixed` Anweisung.</span><span class="sxs-lookup"><span data-stu-id="237cf-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="237cf-353">Die erste Anweisung korrigiert und ruft die Adresse eines statischen Felds ab, die zweite Anweisung korrigiert und die Adresse eines Instanzenfelds und die dritte Anweisung korrigiert und ruft die Adresse eines Arrayelements ab.</span><span class="sxs-lookup"><span data-stu-id="237cf-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="237cf-354">In jedem Fall hätte ein Fehler mit der normalen `&` Operator, da die Variablen als bewegliche Variablen klassifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="237cf-355">Der vierte `fixed` -Anweisung im obigen Beispiel erzeugt ein ähnliches Ergebnis an den dritten.</span><span class="sxs-lookup"><span data-stu-id="237cf-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="237cf-356">Dieses Beispiel die `fixed` -Anweisung verwendet `string`:</span><span class="sxs-lookup"><span data-stu-id="237cf-356">This example of the `fixed` statement uses `string`:</span></span>

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

<span data-ttu-id="237cf-357">In einem unsicheren Kontext werden Elemente des Arrays von eindimensionalen Arrays in aufsteigender Indexreihenfolge, beginnend mit dem Index gespeichert `0` und endend mit Index `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="237cf-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="237cf-358">Für mehrdimensionale Arrays, Arrays, die Elemente gespeichert werden, dass die Indizes von die Dimension ganz rechts zunächst erhöht werden dann von der nächsten links Dimension, und so weiter auf der linken Seite.</span><span class="sxs-lookup"><span data-stu-id="237cf-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="237cf-359">Innerhalb einer `fixed` -Anweisung, die einen Zeiger erhält `p` auf eine Arrayinstanz `a`, die Zeigerwerte von `p` zu `p + a.Length - 1` Adressen der Elemente im Array darstellen.</span><span class="sxs-lookup"><span data-stu-id="237cf-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="237cf-360">Ebenso die Variablen, die im Bereich von `p[0]` zu `p[a.Length - 1]` die tatsächlichen Arrayelemente darstellen.</span><span class="sxs-lookup"><span data-stu-id="237cf-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="237cf-361">Wenn die Möglichkeit, die in der Arrays gespeichert sind, können wir ein Array von einer beliebigen Dimension, behandeln, als wäre es linear.</span><span class="sxs-lookup"><span data-stu-id="237cf-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="237cf-362">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="237cf-362">For example:</span></span>

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

<span data-ttu-id="237cf-363">die Ausgabe erzeugt:</span><span class="sxs-lookup"><span data-stu-id="237cf-363">which produces the output:</span></span>

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="237cf-364">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="237cf-364">In the example</span></span>

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

<span data-ttu-id="237cf-365">eine `fixed` -Anweisung verwendet, um ein Array zu beheben, damit seine Adresse an eine Methode übergeben werden kann, die einen Zeiger akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="237cf-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="237cf-366">Im folgenden Beispiel</span><span class="sxs-lookup"><span data-stu-id="237cf-366">In the example:</span></span>

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

<span data-ttu-id="237cf-367">eine fixed-Anweisung wird verwendet, um Puffers mit fester Größe einer Struktur zu beheben, damit seine Adresse als Zeiger verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="237cf-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="237cf-368">Ein `char*` Wert erzeugt, indem Sie die Fixierung einer Zeichenfolgeninstanz immer zeigt auf eine Null-terminierte Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="237cf-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="237cf-369">In einer fixed-Anweisung, die einen Zeiger erhält `p` zu einer Zeichenfolgeninstanz `s`, die Zeigerwerte von `p` zu `p + s.Length - 1` Adressen Zeichen in der Zeichenfolge und der Zeigerwert darstellen `p + s.Length` immer verweist auf ein Null-Zeichen (das Zeichen, mit dem Wert `'\0'`).</span><span class="sxs-lookup"><span data-stu-id="237cf-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="237cf-370">Ändern Objekte vom verwalteten Typ über feste Zeiger können die Ergebnisse in einem nicht definierten Verhalten.</span><span class="sxs-lookup"><span data-stu-id="237cf-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="237cf-371">Da Zeichenfolgen unveränderlich sind, ist es z. B. der Programmierer dafür verantwortlich, um sicherzustellen, dass die Zeichen, die auf die verwiesen wird durch einen Zeiger auf eine feste Zeichenfolge nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="237cf-372">Die automatischen Null-Terminierung von Zeichenfolgen ist besonders praktisch, wenn es sich bei externen APIs aufrufen, die voraussichtlich von Zeichenfolgen im "C-Stil".</span><span class="sxs-lookup"><span data-stu-id="237cf-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="237cf-373">Beachten Sie jedoch, dass eine Zeichenfolgeninstanz zulässig ist, Null-Zeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="237cf-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="237cf-374">Wenn diese Null-Zeichen vorhanden sind, die Zeichenfolge wird angezeigt, abgeschnittene als eine Null-terminierte behandelt `char*`.</span><span class="sxs-lookup"><span data-stu-id="237cf-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="237cf-375">Puffer fester Größe</span><span class="sxs-lookup"><span data-stu-id="237cf-375">Fixed size buffers</span></span>

<span data-ttu-id="237cf-376">Puffer fester Größe werden verwendet, um "C-Style" Inline-Arrays als Member von Strukturen deklariert werden, und eignen sich in erster Linie für die Interaktion mit nicht verwalteten APIs.</span><span class="sxs-lookup"><span data-stu-id="237cf-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="237cf-377">Puffer fester Größe-Deklarationen</span><span class="sxs-lookup"><span data-stu-id="237cf-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="237cf-378">Ein ***fester Größe Puffer*** angehört, der Speicher für einen Puffer fester Länge mit einer Variablen eines bestimmten Typs darstellt.</span><span class="sxs-lookup"><span data-stu-id="237cf-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="237cf-379">Puffer fester Größe Deklaration führt eine oder mehrere Puffer mit fester Größe eines bestimmten Elementtyps.</span><span class="sxs-lookup"><span data-stu-id="237cf-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="237cf-380">Puffer fester Größe dürfen nur in Strukturdeklarationen von und kann nur in nicht sicheren Kontexten auftreten ([nicht sicheren Kontexten](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="237cf-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

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

<span data-ttu-id="237cf-381">Deklaration Puffer fester Größe kann einen Satz von Attributen enthalten ([Attribute](attributes.md)), ein `new` Modifizierer ([Modifizierer](classes.md#modifiers)), eine gültige Kombination der vier Zugriffsmodifizierer ([Typ Parameter und Einschränkungen](classes.md#type-parameters-and-constraints)) und ein `unsafe` Modifizierer ([nicht sicheren Kontexten](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="237cf-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="237cf-382">Die Attribute und Modifizierer gelten für alle Member, die der Puffer fester Größe-Deklaration deklariert.</span><span class="sxs-lookup"><span data-stu-id="237cf-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="237cf-383">Es ist ein Fehler für den gleichen Modifizierer für mehrere Male in einer Puffer fester Größe angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="237cf-384">Deklaration Puffer fester Größe ist nicht zulässig, sollen die `static` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="237cf-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="237cf-385">Der Puffer-Elementtyp, der eine feste Größe Puffer Deklaration gibt an, den Typ des Elements, der die Puffer durch die Deklaration eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="237cf-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="237cf-386">Der Elementtyp der Puffer muss eine der vordefinierten Typen `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, oder `bool`.</span><span class="sxs-lookup"><span data-stu-id="237cf-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="237cf-387">Der Puffer-Elementtyp folgt eine Liste von Deklaratoren verwenden Puffer fester Größe, von denen jede einen neuen Member einführen.</span><span class="sxs-lookup"><span data-stu-id="237cf-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="237cf-388">Puffer fester Größe Deklaration besteht aus einem Bezeichner, der den Elementnamen gefolgt von einem konstanten Ausdruck, der eingeschlossen in `[` und `]` Token.</span><span class="sxs-lookup"><span data-stu-id="237cf-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="237cf-389">Der Konstante Ausdruck gibt an, die Anzahl der Elemente in das Element, das durch diese feste Größe Puffer Deklarator eingeführt wird.</span><span class="sxs-lookup"><span data-stu-id="237cf-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="237cf-390">Der Typ des konstanten Ausdrucks muss implizit in den Typ `int`, und der Wert muss eine ganze Zahl ungleich NULL sein.</span><span class="sxs-lookup"><span data-stu-id="237cf-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="237cf-391">Die Elemente des Puffers mit fester Größe werden garantiert sequenziell im Arbeitsspeicher angeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="237cf-392">Erklärung Puffer fester Größe, die mehrere Puffer mit fester Größe deklariert entspricht mehreren Deklarationen für eine Deklaration einer Puffer fester Größe mit denselben Attributen und Elementtypen.</span><span class="sxs-lookup"><span data-stu-id="237cf-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="237cf-393">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="237cf-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="237cf-394">für die folgende Syntax:</span><span class="sxs-lookup"><span data-stu-id="237cf-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="237cf-395">Puffer fester Größe in Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="237cf-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="237cf-396">Suche nach Membern ([Operatoren](expressions.md#operators)) eine feste Größe Puffer Member wird fortgesetzt, genau wie die Suche nach Membern eines Felds.</span><span class="sxs-lookup"><span data-stu-id="237cf-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="237cf-397">Puffers mit fester Größe kann verwiesen werden, in einem Ausdruck mit einer *Simple_name* ([Typrückschluss](expressions.md#type-inference)) oder ein *Member_access* ([Überprüfungen zur Kompilierzeit der Dynamische überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="237cf-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="237cf-398">Wenn Mitglied Puffer fester Größe als einen einfachen Namen verwiesen wird, der Effekt ist derselbe wie ein Memberzugriff des Formulars `this.I`, wobei `I` Members Puffer fester Größe ist.</span><span class="sxs-lookup"><span data-stu-id="237cf-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="237cf-399">In einem Memberzugriff des Formulars `E.I`, wenn `E` ist ein Strukturtyp und eine Suche nach Membern der `I` , Typ "Struktur" einen Member mit fester Größe, bezeichnet, die dann `E.I` ist eine klassifizierte wie folgt ausgewertet:</span><span class="sxs-lookup"><span data-stu-id="237cf-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="237cf-400">Wenn der Ausdruck `E.I` erfolgt nicht in einem unsicheren Kontext, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="237cf-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="237cf-401">Wenn `E` wird als Wert klassifiziert, ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="237cf-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="237cf-402">Andernfalls gilt: Wenn `E` ist eine bewegliche Variable ([für feste und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)) und der Ausdruck `E.I` keine *Fixed_pointer_initializer* ([festen Anweisung](unsafe-code.md#the-fixed-statement)), ein Fehler während der Kompilierung auftritt.</span><span class="sxs-lookup"><span data-stu-id="237cf-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="237cf-403">Andernfalls `E` verweist auf eine feste Variable und das Ergebnis des Ausdrucks ist ein Zeiger auf das erste Element des Elements Puffer fester Größe `I` in `E`.</span><span class="sxs-lookup"><span data-stu-id="237cf-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="237cf-404">Das Ergebnis ist vom Typ `S*`, wobei `S` ist der Elementtyp der `I`, und wird als Wert klassifiziert.</span><span class="sxs-lookup"><span data-stu-id="237cf-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="237cf-405">Die nachfolgenden Elemente des Puffers fester Größe können mithilfe von Zeigeroperationen vom ersten Element zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="237cf-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="237cf-406">Im Gegensatz zu den Zugriff auf Arrays Zugriff auf die Elemente eines Puffers mit fester Größe ist ein unsicherer Vorgang und ist kein Bereich überprüft.</span><span class="sxs-lookup"><span data-stu-id="237cf-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="237cf-407">Das folgende Beispiel deklariert und verwendet eine Struktur mit einem Member Puffer mit fester Größe.</span><span class="sxs-lookup"><span data-stu-id="237cf-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

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

### <a name="definite-assignment-checking"></a><span data-ttu-id="237cf-408">Definitive Zuweisung überprüfen</span><span class="sxs-lookup"><span data-stu-id="237cf-408">Definite assignment checking</span></span>

<span data-ttu-id="237cf-409">Puffer fester Größe werden nicht definitive Zuweisung überprüfen ([definitive Zuweisung](variables.md#definite-assignment)), und fester Größe Puffer Elemente werden ignoriert, zum Zweck der definitive Zuweisung, die Überprüfung der Struktur Typvariablen.</span><span class="sxs-lookup"><span data-stu-id="237cf-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="237cf-410">Wenn die äußersten enthaltenden Strukturvariable eines Elements Puffer fester Größe eine statische Variable, eine Instanzvariable für eine Instanz der Klasse oder eines Arrayelements ist, werden die Elemente des Puffers fester Größe automatisch auf ihre Standardwerte initialisiert ([Standardwerte](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="237cf-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="237cf-411">In allen anderen Fällen ist der ursprüngliche Inhalt des Puffers mit fester Größe nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="237cf-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="237cf-412">Stapelreservierung</span><span class="sxs-lookup"><span data-stu-id="237cf-412">Stack allocation</span></span>

<span data-ttu-id="237cf-413">In einem unsicheren Kontext, einer lokalen Variablendeklaration ([lokale Variablendeklarationen](statements.md#local-variable-declarations)) kann einen Stack Allocation-Initialisierer dem Speicher zugeordnet werden, in der Aufrufliste enthalten.</span><span class="sxs-lookup"><span data-stu-id="237cf-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="237cf-414">Die *Unmanaged_type* gibt den Typ der Elemente, die in den neu zugewiesenen Standort gespeichert werden und die *Ausdruck* gibt die Anzahl dieser Elemente.</span><span class="sxs-lookup"><span data-stu-id="237cf-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="237cf-415">Zusammen geben diese die erforderlichen Zuordnungsgröße.</span><span class="sxs-lookup"><span data-stu-id="237cf-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="237cf-416">Da die Größe des Stack-gesamtzuordnung darf nicht negativ sein, es ist ein Fehler während der Kompilierung an die Anzahl der Elemente als eine *Constant_expression* , die auf einen negativen Wert ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="237cf-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="237cf-417">Ein Stack Allocation-Initialisierer des Formulars `stackalloc T[E]` erfordert `T` einen nicht verwalteten Typ sein ([Zeigertypen](unsafe-code.md#pointer-types)) und `E` als Ausdruck vom Typ `int`.</span><span class="sxs-lookup"><span data-stu-id="237cf-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="237cf-418">Das Konstrukt weist `E * sizeof(T)` Bytes aus dem Aufruf Stapel und gibt einen Zeiger vom Typ `T*`, auf den neu belegten Block.</span><span class="sxs-lookup"><span data-stu-id="237cf-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="237cf-419">Wenn `E` ein negativer Wert ist, und klicken Sie dann das Verhalten nicht definiert ist.</span><span class="sxs-lookup"><span data-stu-id="237cf-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="237cf-420">Wenn `E` ist 0 (null), und klicken Sie dann keine Zuordnung erfolgt, und des zurückgegebene Zeigers Implementierung definiert.</span><span class="sxs-lookup"><span data-stu-id="237cf-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="237cf-421">Wenn nicht genügend Arbeitsspeicher verfügbar, um einen Block, der angegebenen Größe ein `System.StackOverflowException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="237cf-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="237cf-422">Der Inhalt des neu belegten Speichers ist nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="237cf-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="237cf-423">Stack Allocation-Initialisierer sind nicht zulässig `catch` oder `finally` Blöcke ([der Try-Anweisung](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="237cf-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="237cf-424">Es gibt keine Möglichkeit, explizit freizugeben, speicherbelegung, die mit `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="237cf-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="237cf-425">Alle stapelzugeordneten Speicherblöcke, die während der Ausführung ein Funktionsmember erstellt werden automatisch verworfen werden, wenn diese Funktionselement zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="237cf-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="237cf-426">Dies entspricht der `alloca` -Funktion, eine Erweiterung, die häufig in C und C++-Implementierungen gefunden.</span><span class="sxs-lookup"><span data-stu-id="237cf-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="237cf-427">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="237cf-427">In the example</span></span>

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

<span data-ttu-id="237cf-428">eine `stackalloc` Initialisierer werden in der `IntToString` Methode, um einen Puffer von 16 Zeichen auf dem Stapel zu reservieren.</span><span class="sxs-lookup"><span data-stu-id="237cf-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="237cf-429">Der Puffer wird automatisch verworfen, wenn die Methode zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="237cf-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="237cf-430">Dynamische speicherbelegung</span><span class="sxs-lookup"><span data-stu-id="237cf-430">Dynamic memory allocation</span></span>

<span data-ttu-id="237cf-431">Mit Ausnahme der `stackalloc` Operator C# bietet keine vordefinierten Konstrukte für die Verwaltung von erfassten nicht-Garbage-Speicher.</span><span class="sxs-lookup"><span data-stu-id="237cf-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="237cf-432">Diese Dienste werden in der Regel durch die Unterstützung von Klassenbibliotheken bereitgestellt oder direkt aus dem zugrunde liegenden Betriebssystem importiert.</span><span class="sxs-lookup"><span data-stu-id="237cf-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="237cf-433">Z. B. die `Memory` nachstehenden Klasse veranschaulicht, wie die Heapfunktionen eines zugrunde liegenden Betriebssystems aus C# zugegriffen werden können:</span><span class="sxs-lookup"><span data-stu-id="237cf-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

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

<span data-ttu-id="237cf-434">Ein Beispiel, verwendet der `Memory` Klasse sind unten angegeben:</span><span class="sxs-lookup"><span data-stu-id="237cf-434">An example that uses the `Memory` class is given below:</span></span>

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

<span data-ttu-id="237cf-435">Das Beispiel ordnet 256 Bytes an Arbeitsspeicher über `Memory.Alloc` und initialisiert den Speicherblock mit Werten von 0 bis 255.</span><span class="sxs-lookup"><span data-stu-id="237cf-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="237cf-436">Klicken Sie dann ein Element mit 256-Byte-Array zugeordnet und verwendet `Memory.Copy` um den Inhalt des Speicherblocks in der Byte-Array zu kopieren.</span><span class="sxs-lookup"><span data-stu-id="237cf-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="237cf-437">Schließlich wird der Speicherblock mit freigegeben `Memory.Free` und der Inhalt des Byte-Arrays auf der Konsole ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="237cf-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
