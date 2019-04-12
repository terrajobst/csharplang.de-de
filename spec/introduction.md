---
ms.openlocfilehash: db10046af5d635b430951679a448e23680b18b87
ms.sourcegitcommit: a19fac74c01a6c3da67d38b2f79527145d4edcbc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59426811"
---
# <a name="introduction"></a><span data-ttu-id="b9fcc-101">Einführung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-101">Introduction</span></span>

<span data-ttu-id="b9fcc-102">C# (Aussprache „C Sharp“) ist eine einfache, moderne, objektorientierte und typsichere Programmiersprache.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-102">C# (pronounced "See Sharp") is a simple, modern, object-oriented, and type-safe programming language.</span></span> <span data-ttu-id="b9fcc-103">C# hat seine Wurzeln in der Sprachen C und C, C++ und Java-Programmierer sofort vertraut sein.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-103">C# has its roots in the C family of languages and will be immediately familiar to C, C++, and Java programmers.</span></span> <span data-ttu-id="b9fcc-104">C# ist durch ECMA-International als standardisiert die ***ECMA-334*** standard und ISO/IEC als die ***ISO/IEC 23270*** standard.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-104">C# is standardized by ECMA International as the ***ECMA-334*** standard and by ISO/IEC as the ***ISO/IEC 23270*** standard.</span></span> <span data-ttu-id="b9fcc-105">Von Microsoft c#-Compiler für .NET Framework ist keine entsprechende Implementierung dieser beiden dieser Standards.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-105">Microsoft's C# compiler for the .NET Framework is a conforming implementation of both of these standards.</span></span>

<span data-ttu-id="b9fcc-106">C# ist eine objektorientierte Sprache, umfasst allerdings auch Unterstützung für eine ***komponentenorientierte*** Programmierung.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-106">C# is an object-oriented language, but C# further includes support for ***component-oriented*** programming.</span></span> <span data-ttu-id="b9fcc-107">Die Softwareentwicklung von heute beruht zunehmend auf Softwarekomponenten in Form von eigenständigen und selbstbeschreibenden Funktionspaketen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-107">Contemporary software design increasingly relies on software components in the form of self-contained and self-describing packages of functionality.</span></span> <span data-ttu-id="b9fcc-108">Wichtig bei solchen Komponenten ist, dass sie für ein Programmiermodell mit Eigenschaften, Methoden und Ereignissen stehen. Sie verfügen über Attribute, die deklarative Informationen zur Komponente bereitstellen, und lassen sich in ihre eigene Dokumentation integrieren.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-108">Key to such components is that they present a programming model with properties, methods, and events; they have attributes that provide declarative information about the component; and they incorporate their own documentation.</span></span> <span data-ttu-id="b9fcc-109">C# bietet Sprachkonstrukte zur direkten Unterstützung für diese Konzepte, wodurch c# zu einer sehr natürlichen Sprache, in denen zum Erstellen und Verwenden der Software-Komponenten.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-109">C# provides language constructs to directly support these concepts, making C# a very natural language in which to create and use software components.</span></span>

<span data-ttu-id="b9fcc-110">Mehrere C#-Funktionen helfen beim Entwickeln stabiler und dauerhafter Anwendungen: ***Die automatische speicherbereinigung*** von nicht verwendeten Objekte belegten Arbeitsspeicher automatisch freigibt ***Ausnahmebehandlung*** bietet einen strukturierten und erweiterbaren Ansatz für die fehlererkennung und Wiederherstellung und die ***typsichere*** Aufbau der Sprache macht es unmöglich, nicht initialisierte Variablen gelesen werden soll. Geben Sie zum Indizieren Umwandlungen Arrays über die Grenzen oder führen Sie deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-110">Several C# features aid in the construction of robust and durable applications: ***Garbage collection*** automatically reclaims memory occupied by unused objects; ***exception handling*** provides a structured and extensible approach to error detection and recovery; and the ***type-safe*** design of the language makes it impossible to read from uninitialized variables, to index arrays beyond their bounds, or to perform unchecked type casts.</span></span>

<span data-ttu-id="b9fcc-111">C# verfügt über ein ***einheitliches Typsystem***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-111">C# has a ***unified type system***.</span></span> <span data-ttu-id="b9fcc-112">Alle C#-Typen, einschließlich primitiver Typen wie `int` und `double`, erben von einem einzelnen `object`-Stammtyp.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-112">All C# types, including primitive types such as `int` and `double`, inherit from a single root `object` type.</span></span> <span data-ttu-id="b9fcc-113">Daher verwenden alle Typen einen Satz allgemeiner Vorgänge, und Werte eines beliebigen Typs können gespeichert, übertragen und konsistent ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-113">Thus, all types share a set of common operations, and values of any type can be stored, transported, and operated upon in a consistent manner.</span></span> <span data-ttu-id="b9fcc-114">Darüber hinaus unterstützt C# benutzerdefinierte Verweistypen und Werttypen und ermöglicht so die dynamische Zuordnung von Objekten sowie die Inlinespeicherung einfacher Strukturen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-114">Furthermore, C# supports both user-defined reference types and value types, allowing dynamic allocation of objects as well as in-line storage of lightweight structures.</span></span>

<span data-ttu-id="b9fcc-115">Um sicherzustellen, dass c#-Programmen und Bibliotheken im Laufe der Zeit kompatibel weiterentwickelt werden können, wurde viel Bedeutung für platziert ***versionsverwaltung*** # Entwurf.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-115">To ensure that C# programs and libraries can evolve over time in a compatible manner, much emphasis has been placed on ***versioning*** in C#'s design.</span></span> <span data-ttu-id="b9fcc-116">Viele Programmiersprachen schenken diesem Problem wenig Beachtung, und in dieser Sprache geschriebene Programme stürzen daher häufiger als notwendig ab, wenn neuere Versionen abhängiger Bibliotheken eingeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-116">Many programming languages pay little attention to this issue, and, as a result, programs written in those languages break more often than necessary when newer versions of dependent libraries are introduced.</span></span> <span data-ttu-id="b9fcc-117">Aspekte der C#des Entwurfs, der direkt von Überlegungen bei der Versionskontrolle beeinflusst wurden, gehören die separaten `virtual` und `override` Modifizierer, die Regeln für die überladungsauflösung und die Unterstützung für explizite Schnittstellenmember-Deklarationen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-117">Aspects of C#'s design that were directly influenced by versioning considerations include the separate `virtual` and `override` modifiers, the rules for method overload resolution, and support for explicit interface member declarations.</span></span>

<span data-ttu-id="b9fcc-118">Im verbleibenden Teil dieses Kapitels wird beschrieben, die wesentlichen Funktionen von c#-Sprache.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-118">The rest of this chapter describes the essential features of the C# language.</span></span> <span data-ttu-id="b9fcc-119">Obwohl es sich bei spätere Kapiteln detailorientiertheit und manchmal mathematischen Regeln und Ausnahmen zu beschreiben, wurde in diesem Kapitel für Klarheit und Übersichtlichkeit zu Lasten der Vollständigkeit.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-119">Although later chapters describe rules and exceptions in a detail-oriented and sometimes mathematical manner, this chapter strives for clarity and brevity at the expense of completeness.</span></span> <span data-ttu-id="b9fcc-120">Ziel ist für eine Einführung in die Sprache des Readers bereit, die das Schreiben von frühen Programme und das Lesen der späteren Kapiteln erleichtern wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-120">The intent is to provide the reader with an introduction to the language that will facilitate the writing of early programs and the reading of later chapters.</span></span>

## <a name="hello-world"></a><span data-ttu-id="b9fcc-121">Hello World</span><span class="sxs-lookup"><span data-stu-id="b9fcc-121">Hello world</span></span>

<span data-ttu-id="b9fcc-122">Das Programm „Hello, World“ wird für gewöhnlich zur Einführung einer Programmiersprache verwendet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-122">The "Hello, World" program is traditionally used to introduce a programming language.</span></span> <span data-ttu-id="b9fcc-123">Hier ist es in C#:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-123">Here it is in C#:</span></span>

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

<span data-ttu-id="b9fcc-124">C#-Quelldateien weisen in der Regel die Dateierweiterung `.cs` auf.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-124">C# source files typically have the file extension `.cs`.</span></span> <span data-ttu-id="b9fcc-125">Unter der Annahme, dass das Programm "Hello, World" in der Datei gespeichert wird `hello.cs`, das Programm kompiliert werden kann, mit dem Microsoft C#-Compiler, die über die Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="b9fcc-125">Assuming that the "Hello, World" program is stored in the file `hello.cs`, the program can be compiled with the Microsoft C# compiler using the command line</span></span>
```
csc hello.cs
```
<span data-ttu-id="b9fcc-126">erzeugt eine ausführbare Assembly mit dem Namen `hello.exe`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-126">which produces an executable assembly named `hello.exe`.</span></span> <span data-ttu-id="b9fcc-127">Die Ausgabe von dieser Anwendung erstellt wird, wenn er ausgeführt wird</span><span class="sxs-lookup"><span data-stu-id="b9fcc-127">The output produced by this application when it is run is</span></span>
```
Hello, World
```

<span data-ttu-id="b9fcc-128">Das Programm „Hello, World“ wird mit einer `using`-Richtlinie gestartet, die auf den `System`-Namespace verweist.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-128">The "Hello, World" program starts with a `using` directive that references the `System` namespace.</span></span> <span data-ttu-id="b9fcc-129">Namespaces bieten eine hierarchische Möglichkeit zum Organisieren von C#-Programmen und -Bibliotheken.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-129">Namespaces provide a hierarchical means of organizing C# programs and libraries.</span></span> <span data-ttu-id="b9fcc-130">Namespaces enthalten Typen und andere Namespaces. Beispiel: Der `System`-Namespace enthält eine Reihe von Typen, wie etwa die `Console`-Klasse, auf die im Programm verwiesen wird, und eine Reihe anderer Namespaces, wie etwa `IO` und `Collections`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-130">Namespaces contain types and other namespaces—for example, the `System` namespace contains a number of types, such as the `Console` class referenced in the program, and a number of other namespaces, such as `IO` and `Collections`.</span></span> <span data-ttu-id="b9fcc-131">Eine `using`-Richtlinie, die auf einen bestimmten Namespace verweist, ermöglicht die nicht qualifizierte Nutzung der Typen, die Member dieses Namespace sind.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-131">A `using` directive that references a given namespace enables unqualified use of the types that are members of that namespace.</span></span> <span data-ttu-id="b9fcc-132">Aufgrund der `using`-Direktive kann das Programm `Console.WriteLine` als Abkürzung für `System.Console.WriteLine` verwenden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-132">Because of the `using` directive, the program can use `Console.WriteLine` as shorthand for `System.Console.WriteLine`.</span></span>

<span data-ttu-id="b9fcc-133">Die `Hello`-Klasse, die vom Programm „Hello, World“ deklariert wird, verfügt über einen einzelnen Member: die `Main`-Methode.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-133">The `Hello` class declared by the "Hello, World" program has a single member, the method named `Main`.</span></span> <span data-ttu-id="b9fcc-134">Die `Main` Methode wird deklariert, mit der `static` Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-134">The `Main` method is declared with the `static` modifier.</span></span> <span data-ttu-id="b9fcc-135">Auch wenn Instanzmethoden mit dem Schlüsselwort `this` auf eine bestimmte einschließende Objektinstanz verweisen können, agieren statische Methoden ohne Verweis auf ein bestimmtes Objekt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-135">While instance methods can reference a particular enclosing object instance using the keyword `this`, static methods operate without reference to a particular object.</span></span> <span data-ttu-id="b9fcc-136">Gemäß der Konvention fungiert eine statische Methode mit der Bezeichnung `Main` als Einstiegspunkt eines Programms.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-136">By convention, a static method named `Main` serves as the entry point of a program.</span></span>

<span data-ttu-id="b9fcc-137">Die Ausgabe des Programms wird anhand der `WriteLine`-Methode der `Console`-Klasse im `System`-Namespace generiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-137">The output of the program is produced by the `WriteLine` method of the `Console` class in the `System` namespace.</span></span> <span data-ttu-id="b9fcc-138">Diese Klasse wird von der .NET Framework-Klassenbibliotheken bereitgestellt, die standardmäßig vom Microsoft C#-Compiler automatisch verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-138">This class is provided by the .NET Framework class libraries, which, by default, are automatically referenced by the Microsoft C# compiler.</span></span> <span data-ttu-id="b9fcc-139">Beachten Sie, dass c# selbst keine separate Laufzeit-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-139">Note that C# itself does not have a separate runtime library.</span></span> <span data-ttu-id="b9fcc-140">Stattdessen ist .NET Framework die Runtime-Bibliothek von c#.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-140">Instead, the .NET Framework is the runtime library of C#.</span></span>

## <a name="program-structure"></a><span data-ttu-id="b9fcc-141">Programmstruktur</span><span class="sxs-lookup"><span data-stu-id="b9fcc-141">Program structure</span></span>

<span data-ttu-id="b9fcc-142">Die organisatorischen Schlüsselkonzepte in C# sind: ***Programme***, ***Namespaces***, ***Typen***, ***Member*** und ***Assemblys***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-142">The key organizational concepts in C# are ***programs***, ***namespaces***, ***types***, ***members***, and ***assemblies***.</span></span> <span data-ttu-id="b9fcc-143">C#-Programme bestehen aus mindestens einer Quelldatei.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-143">C# programs consist of one or more source files.</span></span> <span data-ttu-id="b9fcc-144">Programme deklarieren Typen, die Member enthalten, und können in Namespaces organisiert werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-144">Programs declare types, which contain members and can be organized into namespaces.</span></span> <span data-ttu-id="b9fcc-145">Klassen und Schnittstellen sind Beispiele für Typen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-145">Classes and interfaces are examples of types.</span></span> <span data-ttu-id="b9fcc-146">Felder, Methoden, Eigenschaften und Ereignisse sind Beispiele für Member.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-146">Fields, methods, properties, and events are examples of members.</span></span> <span data-ttu-id="b9fcc-147">Wenn C#-Programme kompiliert werden, werden sie physisch in Assemblys verpackt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-147">When C# programs are compiled, they are physically packaged into assemblies.</span></span> <span data-ttu-id="b9fcc-148">Assemblys müssen in der Regel die Dateierweiterung `.exe` oder `.dll`, abhängig davon, ob sie implementieren ***Anwendungen*** oder ***Bibliotheken***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-148">Assemblies typically have the file extension `.exe` or `.dll`, depending on whether they implement ***applications*** or ***libraries***.</span></span>

<span data-ttu-id="b9fcc-149">Im Beispiel</span><span class="sxs-lookup"><span data-stu-id="b9fcc-149">The example</span></span>

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
<span data-ttu-id="b9fcc-150">deklariert eine Klasse namens `Stack` in einem Namespace namens `Acme.Collections`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-150">declares a class named `Stack` in a namespace called `Acme.Collections`.</span></span> <span data-ttu-id="b9fcc-151">Der vollqualifizierte Name dieser Klasse ist `Acme.Collections.Stack`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-151">The fully qualified name of this class is `Acme.Collections.Stack`.</span></span> <span data-ttu-id="b9fcc-152">Die Klasse enthält mehrere Member: ein Feld mit dem Namen `top`, zwei Methoden mit dem Namen `Push` und `Pop` sowie eine geschachtelte Klasse mit dem Namen `Entry`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-152">The class contains several members: a field named `top`, two methods named `Push` and `Pop`, and a nested class named `Entry`.</span></span> <span data-ttu-id="b9fcc-153">Die `Entry`-Klasse enthält weitere drei Member: ein Feld mit dem Namen `next`, ein Feld mit dem Namen `data` und einen Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-153">The `Entry` class further contains three members: a field named `next`, a field named `data`, and a constructor.</span></span> <span data-ttu-id="b9fcc-154">Vorausgesetzt, dass der Quellcode des Beispiels in der Datei `acme.cs` gespeichert wird, kompiliert die Befehlszeile</span><span class="sxs-lookup"><span data-stu-id="b9fcc-154">Assuming that the source code of the example is stored in the file `acme.cs`, the command line</span></span>

```
csc /t:library acme.cs
```
<span data-ttu-id="b9fcc-155">das Beispiel als Bibliothek (Code ohne `Main`-Einstiegspunkt) und erstellt eine Assembly mit dem Namen `acme.dll`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-155">compiles the example as a library (code without a `Main` entry point) and produces an assembly named `acme.dll`.</span></span>

<span data-ttu-id="b9fcc-156">Assemblys enthalten ausführbaren Code in Form von ***Intermediate Language*** (IL)-Anweisungen und symbolischen Informationen in Form von ***Metadaten***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-156">Assemblies contain executable code in the form of ***Intermediate Language*** (IL) instructions, and symbolic information in the form of ***metadata***.</span></span> <span data-ttu-id="b9fcc-157">Vor der Ausführung wird der IL-Code in einer Assembly automatisch durch den Just-in-Time-Compiler (JIT) der .NET Common Language Runtime in prozessorspezifischen Code konvertiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-157">Before it is executed, the IL code in an assembly is automatically converted to processor-specific code by the Just-In-Time (JIT) compiler of .NET Common Language Runtime.</span></span>

<span data-ttu-id="b9fcc-158">Da eine Assembly eine selbstbeschreibende Funktionseinheit mit Code und Metadaten ist, besteht in C# keine Notwendigkeit für `#include`-Direktiven und Headerdateien.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-158">Because an assembly is a self-describing unit of functionality containing both code and metadata, there is no need for `#include` directives and header files in C#.</span></span> <span data-ttu-id="b9fcc-159">Die öffentlichen Typen und Member, die in einer bestimmten Assembly enthalten sind, werden einfach durch Verweisen auf die Assembly beim Kompilieren des Programms in einem C#-Programm verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-159">The public types and members contained in a particular assembly are made available in a C# program simply by referencing that assembly when compiling the program.</span></span> <span data-ttu-id="b9fcc-160">Dieses Programm verwendet z.B. die `Acme.Collections.Stack`-Klasse aus der `acme.dll`-Assembly:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-160">For example, this program uses the `Acme.Collections.Stack` class from the `acme.dll` assembly:</span></span>

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
<span data-ttu-id="b9fcc-161">Wenn das Programm in der Datei gespeichert ist `test.cs`bei `test.cs` kompiliert wurde, die `acme.dll` Assembly kann verwiesen werden, mithilfe des Compilers `/r` Option:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-161">If the program is stored in the file `test.cs`, when `test.cs` is compiled, the `acme.dll` assembly can be referenced using the compiler's `/r` option:</span></span>

```
csc /r:acme.dll test.cs
```
<span data-ttu-id="b9fcc-162">So wird eine ausführbare Assembly mit dem Namen `test.exe` erstellt, die bei Ausführung folgende Ausgabe erzeugt:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-162">This creates an executable assembly named `test.exe`, which, when run, produces the output:</span></span>

```
100
10
1
```
<span data-ttu-id="b9fcc-163">In C# kann der Quelltext eines Programms in verschiedenen Quelldateien gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-163">C# permits the source text of a program to be stored in several source files.</span></span> <span data-ttu-id="b9fcc-164">Bei der Kompilierung eines C#-Programms mit mehreren Dateien werden alle Quelldateien zusammen verarbeitet, und die Quelldateien können frei aufeinander verweisen – vom Konzept her ist es so, als seien alle Quelldateien vor der Verarbeitung in einer einzigen großen Datei verkettet worden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-164">When a multi-file C# program is compiled, all of the source files are processed together, and the source files can freely reference each other—conceptually, it is as if all the source files were concatenated into one large file before being processed.</span></span> <span data-ttu-id="b9fcc-165">Vorwärtsdeklarationen sind in C# nie erforderlich, da die Reihenfolge der Deklaration mit wenigen Ausnahmen unbedeutend ist.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-165">Forward declarations are never needed in C# because, with very few exceptions, declaration order is insignificant.</span></span> <span data-ttu-id="b9fcc-166">C# beschränkt eine Quelldatei weder auf die Deklaration eines einzigen öffentlichen Typs, noch muss der Name der Quelldatei mit einem in der Quelldatei deklarierten Typ übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-166">C# does not limit a source file to declaring only one public type nor does it require the name of the source file to match a type declared in the source file.</span></span>

## <a name="types-and-variables"></a><span data-ttu-id="b9fcc-167">Typen und Variablen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-167">Types and variables</span></span>

<span data-ttu-id="b9fcc-168">Es gibt zwei Arten von Typen in C#: ***Werttypen*** und ***Verweistypen***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-168">There are two kinds of types in C#: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="b9fcc-169">Variablen von Werttypen enthalten ihre Daten direkt, Variablen von Verweistypen speichern hingegen Verweise auf ihre Daten – letztere werden als Objekte bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-169">Variables of value types directly contain their data whereas variables of reference types store references to their data, the latter being known as objects.</span></span> <span data-ttu-id="b9fcc-170">Mit Verweistypen können zwei Variablen auf das gleiche Objekt verweisen, und so können an einer Variablen durchgeführte Vorgänge das Objekt beeinflussen, auf das die andere Variable verweist.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-170">With reference types, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="b9fcc-171">Bei Werttypen besitzt jede Variable eine eigene Kopie der Daten, und auf eine Variable angewendete Vorgänge können die andere Variable nicht beeinflussen (außer im Fall von `ref`- und `out`-Parametervariablen).</span><span class="sxs-lookup"><span data-stu-id="b9fcc-171">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other (except in the case of `ref` and `out` parameter variables).</span></span>

<span data-ttu-id="b9fcc-172">C#die Werttypen sind weiter unterteilt in ***einfache Typen***, ***Enumerationstypen***, ***Strukturtypen***, und ***auf NULL festlegbare Typen***, und C#des Verweistypen sind weiter unterteilt in ***Klassentypen***, ***Schnittstellentypen***, ***Arraytypen***, und ***Delegattypen***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-172">C#'s value types are further divided into ***simple types***, ***enum types***, ***struct types***, and ***nullable types***, and C#'s reference types are further divided into ***class types***, ***interface types***, ***array types***, and ***delegate types***.</span></span>

<span data-ttu-id="b9fcc-173">Die folgende Tabelle enthält eine Übersicht über C#Typsystem.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-173">The following table provides an overview of C#'s type system.</span></span>

| __<span data-ttu-id="b9fcc-174">Kategorie</span><span class="sxs-lookup"><span data-stu-id="b9fcc-174">Category</span></span>__    |                 | __<span data-ttu-id="b9fcc-175">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-175">Description</span></span>__ |
|-----------------|-----------------|-----------------|
| <span data-ttu-id="b9fcc-176">Werttypen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-176">Value types</span></span>     | <span data-ttu-id="b9fcc-177">Einfache Typen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-177">Simple types</span></span>    | <span data-ttu-id="b9fcc-178">Ganzzahlig mit Vorzeichen: `sbyte`, `short`, `int`,</span><span class="sxs-lookup"><span data-stu-id="b9fcc-178">Signed integral: `sbyte`, `short`, `int`,</span></span> `long` |
|                 |                 | <span data-ttu-id="b9fcc-179">Ganzzahlig ohne Vorzeichen: `byte`, `ushort`, `uint`,</span><span class="sxs-lookup"><span data-stu-id="b9fcc-179">Unsigned integral: `byte`, `ushort`, `uint`,</span></span> `ulong` |
|                 |                 | <span data-ttu-id="b9fcc-180">Unicode-Zeichen:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-180">Unicode characters:</span></span> `char` |
|                 |                 | <span data-ttu-id="b9fcc-181">IEEE-Gleitkomma: `float`,</span><span class="sxs-lookup"><span data-stu-id="b9fcc-181">IEEE floating point: `float`,</span></span> `double` |
|                 |                 | <span data-ttu-id="b9fcc-182">Decimal mit hoher Präzision:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-182">High-precision decimal:</span></span> `decimal` |
|                 |                 | <span data-ttu-id="b9fcc-183">Boolescher Wert:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-183">Boolean:</span></span> `bool` |
|                 | <span data-ttu-id="b9fcc-184">Enumerationstypen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-184">Enum types</span></span>      | <span data-ttu-id="b9fcc-185">Benutzerdefinierte Typen des Formulars</span><span class="sxs-lookup"><span data-stu-id="b9fcc-185">User-defined types of the form</span></span> `enum E {...}` |
|                 | <span data-ttu-id="b9fcc-186">Strukturtypen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-186">Struct types</span></span>    | <span data-ttu-id="b9fcc-187">Benutzerdefinierte Typen des Formulars</span><span class="sxs-lookup"><span data-stu-id="b9fcc-187">User-defined types of the form</span></span> `struct S {...}` |
|                 | <span data-ttu-id="b9fcc-188">Auf NULL festlegbare Typen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-188">Nullable types</span></span>  | <span data-ttu-id="b9fcc-189">Erweiterungen aller anderen Werttypen mit einem `null`-Wert</span><span class="sxs-lookup"><span data-stu-id="b9fcc-189">Extensions of all other value types with a `null` value</span></span> |
| <span data-ttu-id="b9fcc-190">Verweistypen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-190">Reference types</span></span> | <span data-ttu-id="b9fcc-191">Klassentypen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-191">Class types</span></span>     | <span data-ttu-id="b9fcc-192">Ultimative Basisklasse aller anderen Typen:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-192">Ultimate base class of all other types:</span></span> `object` |
|                 |                 | <span data-ttu-id="b9fcc-193">Unicode-Zeichenfolgen:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-193">Unicode strings:</span></span> `string` |
|                 |                 | <span data-ttu-id="b9fcc-194">Benutzerdefinierte Typen des Formulars</span><span class="sxs-lookup"><span data-stu-id="b9fcc-194">User-defined types of the form</span></span> `class C {...}` |
|                 | <span data-ttu-id="b9fcc-195">Schnittstellentypen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-195">Interface types</span></span> | <span data-ttu-id="b9fcc-196">Benutzerdefinierte Typen des Formulars</span><span class="sxs-lookup"><span data-stu-id="b9fcc-196">User-defined types of the form</span></span> `interface I {...}` |
|                 | <span data-ttu-id="b9fcc-197">Arraytypen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-197">Array types</span></span>     | <span data-ttu-id="b9fcc-198">Ein- und mehrdimensional, z. B. `int[]` und</span><span class="sxs-lookup"><span data-stu-id="b9fcc-198">Single- and multi-dimensional, for example, `int[]` and</span></span> `int[,]` |
|                 | <span data-ttu-id="b9fcc-199">Delegattypen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-199">Delegate types</span></span>  | <span data-ttu-id="b9fcc-200">Benutzerdefinierte Typen im Format z. B.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-200">User-defined types of the form e.g.</span></span> `delegate int  D(...)` |

<span data-ttu-id="b9fcc-201">Die acht Ganzzahltypen unterstützen 8-Bit-, 16-Bit, 32-Bit- und 64-Bit-Werte mit oder ohne Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-201">The eight integral types provide support for 8-bit, 16-bit, 32-bit, and 64-bit values in signed or unsigned form.</span></span>

<span data-ttu-id="b9fcc-202">Zeigen Sie die beiden floating-Typen, `float` und `double`, werden mit der 32-Bit mit einfacher Genauigkeit und 64-Bit-Gleitkommazahl mit doppelter Genauigkeit IEEE 754-Formate dargestellt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-202">The two floating point types, `float` and `double`, are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats.</span></span>

<span data-ttu-id="b9fcc-203">Der `decimal`-Typ ist ein für Finanz-und Währungsberechnungen geeigneter 128-Bit-Datentyp.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-203">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span>

<span data-ttu-id="b9fcc-204">C#die `bool` Typ dient zur Darstellung boolescher Werte – Werte, die entweder `true` oder `false`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-204">C#'s `bool` type is used to represent boolean values—values that are either `true` or `false`.</span></span>

<span data-ttu-id="b9fcc-205">Zur Zeichen- und Zeichenfolgenverarbeitung in C# wird die Unicode-Codierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-205">Character and string processing in C# uses Unicode encoding.</span></span> <span data-ttu-id="b9fcc-206">Der `char`-Typ stellt eine UTF-16-Codeeinheit dar und der `string`-Typ eine Folge von UTF-16-Codeeinheiten.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-206">The `char` type represents a UTF-16 code unit, and the `string` type represents a sequence of UTF-16 code units.</span></span>

<span data-ttu-id="b9fcc-207">Die folgende Tabelle enthält C#der numerischen Typen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-207">The following table summarizes C#'s numeric types.</span></span>


| __<span data-ttu-id="b9fcc-208">Kategorie</span><span class="sxs-lookup"><span data-stu-id="b9fcc-208">Category</span></span>__      | __<span data-ttu-id="b9fcc-209">Bits</span><span class="sxs-lookup"><span data-stu-id="b9fcc-209">Bits</span></span>__ | __<span data-ttu-id="b9fcc-210">Typ</span><span class="sxs-lookup"><span data-stu-id="b9fcc-210">Type</span></span>__  | __<span data-ttu-id="b9fcc-211">Bereich/der gleichen Genauigkeit</span><span class="sxs-lookup"><span data-stu-id="b9fcc-211">Range/Precision</span></span>__ |
|-------------------|----------|-----------|---------------------|
| <span data-ttu-id="b9fcc-212">Ganzzahlig mit Vorzeichen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-212">Signed integral</span></span>   | <span data-ttu-id="b9fcc-213">8</span><span class="sxs-lookup"><span data-stu-id="b9fcc-213">8</span></span>        | `sbyte`   | <span data-ttu-id="b9fcc-214">-128...127</span><span class="sxs-lookup"><span data-stu-id="b9fcc-214">-128...127</span></span> |
|                   | <span data-ttu-id="b9fcc-215">16</span><span class="sxs-lookup"><span data-stu-id="b9fcc-215">16</span></span>       | `short`   | <span data-ttu-id="b9fcc-216">-32,768...32,767</span><span class="sxs-lookup"><span data-stu-id="b9fcc-216">-32,768...32,767</span></span> |
|                   | <span data-ttu-id="b9fcc-217">32</span><span class="sxs-lookup"><span data-stu-id="b9fcc-217">32</span></span>       | `int`     | <span data-ttu-id="b9fcc-218">-2,147,483,648...2,147,483,647</span><span class="sxs-lookup"><span data-stu-id="b9fcc-218">-2,147,483,648...2,147,483,647</span></span> |
|                   | <span data-ttu-id="b9fcc-219">64</span><span class="sxs-lookup"><span data-stu-id="b9fcc-219">64</span></span>       | `long`    | <span data-ttu-id="b9fcc-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span><span class="sxs-lookup"><span data-stu-id="b9fcc-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span></span> |
| <span data-ttu-id="b9fcc-221">Ganzzahlig ohne Vorzeichen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-221">Unsigned integral</span></span> | <span data-ttu-id="b9fcc-222">8</span><span class="sxs-lookup"><span data-stu-id="b9fcc-222">8</span></span>        | `byte`    | <span data-ttu-id="b9fcc-223">0...255</span><span class="sxs-lookup"><span data-stu-id="b9fcc-223">0...255</span></span> |
|                   | <span data-ttu-id="b9fcc-224">16</span><span class="sxs-lookup"><span data-stu-id="b9fcc-224">16</span></span>       | `ushort`  | <span data-ttu-id="b9fcc-225">0...65,535</span><span class="sxs-lookup"><span data-stu-id="b9fcc-225">0...65,535</span></span> |
|                   | <span data-ttu-id="b9fcc-226">32</span><span class="sxs-lookup"><span data-stu-id="b9fcc-226">32</span></span>       | `uint`    | <span data-ttu-id="b9fcc-227">0...4,294,967,295</span><span class="sxs-lookup"><span data-stu-id="b9fcc-227">0...4,294,967,295</span></span> |
|                   | <span data-ttu-id="b9fcc-228">64</span><span class="sxs-lookup"><span data-stu-id="b9fcc-228">64</span></span>       | `ulong`   | <span data-ttu-id="b9fcc-229">0...18,446,744,073,709,551,615</span><span class="sxs-lookup"><span data-stu-id="b9fcc-229">0...18,446,744,073,709,551,615</span></span> |
| <span data-ttu-id="b9fcc-230">Gleitkomma</span><span class="sxs-lookup"><span data-stu-id="b9fcc-230">Floating point</span></span>    | <span data-ttu-id="b9fcc-231">32</span><span class="sxs-lookup"><span data-stu-id="b9fcc-231">32</span></span>       | `float`   | <span data-ttu-id="b9fcc-232">1,5 × 10 ^-45 bis 3,4 × 10 ^ 38, Genauigkeit von 7 Stellen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-232">1.5 × 10^−45 to 3.4 × 10^38, 7-digit precision</span></span> |
|                   | <span data-ttu-id="b9fcc-233">64</span><span class="sxs-lookup"><span data-stu-id="b9fcc-233">64</span></span>       | `double`  | <span data-ttu-id="b9fcc-234">5,0 × 10 ^-324 bis 1,7 × 10 ^ 308, Genauigkeit von 15 Stellen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-234">5.0 × 10^−324 to 1.7 × 10^308, 15-digit precision</span></span> |
| <span data-ttu-id="b9fcc-235">Decimal</span><span class="sxs-lookup"><span data-stu-id="b9fcc-235">Decimal</span></span>           | <span data-ttu-id="b9fcc-236">128</span><span class="sxs-lookup"><span data-stu-id="b9fcc-236">128</span></span>      | `decimal` | <span data-ttu-id="b9fcc-237">1.0 × 10 ^-28 7,9 × 10 ^ 28, 28 Stellen Genauigkeit</span><span class="sxs-lookup"><span data-stu-id="b9fcc-237">1.0 × 10^−28 to 7.9 × 10^28, 28-digit precision</span></span> |

<span data-ttu-id="b9fcc-238">C#-Programme verwenden ***Typdeklarationen***, um neue Typen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-238">C# programs use ***type declarations*** to create new types.</span></span> <span data-ttu-id="b9fcc-239">Eine Typdeklaration gibt den Namen und die Member des neuen Typs an.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-239">A type declaration specifies the name and the members of the new type.</span></span> <span data-ttu-id="b9fcc-240">Fünf C#der Kategorien von Typen sind Benutzerdefinierbar: Klasse Typen, Strukturtypen, Schnittstellentypen, Enumerationstypen und Delegattypen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-240">Five of C#'s categories of types are user-definable: class types, struct types, interface types, enum types, and delegate types.</span></span>

<span data-ttu-id="b9fcc-241">Ein Klassentyp definiert eine Datenstruktur, die Datenmember (Felder) und Funktionsmember (Methoden, Eigenschaften usw.) enthält.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-241">A class type defines a data structure that contains data members (fields) and function members (methods, properties, and others).</span></span> <span data-ttu-id="b9fcc-242">Klassentypen unterstützen einzelne Vererbung und Polymorphie. Dies sind Mechanismen, durch die abgeleitete Klassen erweitert und Basisklassen spezialisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-242">Class types support single inheritance and polymorphism, mechanisms whereby derived classes can extend and specialize base classes.</span></span>

<span data-ttu-id="b9fcc-243">Ein Strukturtyp ähnelt einem Klassentyp, da es sich um eine Struktur mit Datenmembern und Funktionsmembern darstellt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-243">A struct type is similar to a class type in that it represents a structure with data members and function members.</span></span> <span data-ttu-id="b9fcc-244">Im Gegensatz zu Klassen, Strukturen sind allerdings Werttypen und erfordern keine Heapzuordnung.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-244">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="b9fcc-245">Strukturtypen unterstützen keine benutzerdefinierte Vererbung, und alle Strukturtypen erben implizit vom Typ `object`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-245">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="b9fcc-246">Ein Schnittstellentyp definiert einen Vertrag als benannter Satz öffentlicher Funktionsmember.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-246">An interface type defines a contract as a named set of public function members.</span></span> <span data-ttu-id="b9fcc-247">Eine Klasse oder Struktur, die eine Schnittstelle implementiert, muss Implementierungen der Funktionsmember der Schnittstelle bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-247">A class or struct that implements an interface must provide implementations of the interface's function members.</span></span> <span data-ttu-id="b9fcc-248">Eine Schnittstelle kann von mehreren Basisschnittstellen erben, und eine Klasse oder Struktur kann mehrere Schnittstellen implementieren.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-248">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="b9fcc-249">Ein Delegattyp stellt Verweise auf Methoden mit einer bestimmten Parameterliste und dem Rückgabetyp dar.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-249">A delegate type represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="b9fcc-250">Delegate ermöglichen die Behandlung von Methoden als Entitäten, die Variablen zugewiesen und als Parameter übergeben werden können.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-250">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="b9fcc-251">Delegate ähneln dem Konzept von Funktionszeigern, die Sie in einigen anderen Sprachen finden. Im Gegensatz zu Funktionszeigern sind Delegate allerdings objektorientiert und typsicher.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-251">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="b9fcc-252">Klasse, Struktur, Schnittstellen- und Delegattypen-Typen unterstützen generische Typen, wobei sie mit anderen Typen parametrisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-252">Class, struct, interface and delegate types all support generics, whereby they can be parameterized with other types.</span></span>

<span data-ttu-id="b9fcc-253">Ein Enumerationstyp ist ein eigenständiger Typ mit benannter Konstanten.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-253">An enum type is a distinct type with named constants.</span></span> <span data-ttu-id="b9fcc-254">Jeder Enumerationstyp hat einen zugrunde liegenden Typ, der eine der acht ganzzahligen Typen sein muss.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-254">Every enum type has an underlying type, which must be one of the eight integral types.</span></span> <span data-ttu-id="b9fcc-255">Der Satz von Werten eines Enumerationstyps ist der Satz von Werten des zugrunde liegenden Typs identisch.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-255">The set of values of an enum type is the same as the set of values of the underlying type.</span></span>

<span data-ttu-id="b9fcc-256">C# unterstützt ein- und mehrdimensionale Arrays beliebigen Typs.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-256">C# supports single- and multi-dimensional arrays of any type.</span></span> <span data-ttu-id="b9fcc-257">Im Gegensatz zu den oben aufgeführten Typen müssen Arraytypen nicht deklariert werden, bevor sie verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-257">Unlike the types listed above, array types do not have to be declared before they can be used.</span></span> <span data-ttu-id="b9fcc-258">Stattdessen werden Arraytypen erstellt, indem hinter einen Typnamen eckige Klammern gesetzt werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-258">Instead, array types are constructed by following a type name with square brackets.</span></span> <span data-ttu-id="b9fcc-259">Z. B. `int[]` ist ein eindimensionales Array von `int`, `int[,]` wird ein zweidimensionales Array von `int`, und `int[][]` ist ein eindimensionales Array des eindimensionalen Arrays von `int`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-259">For example, `int[]` is a single-dimensional array of `int`, `int[,]` is a two-dimensional array of `int`, and `int[][]` is a single-dimensional array of single-dimensional arrays of `int`.</span></span>

<span data-ttu-id="b9fcc-260">Auf NULL festlegbare Typen auch keine deklariert werden, bevor sie verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-260">Nullable types also do not have to be declared before they can be used.</span></span> <span data-ttu-id="b9fcc-261">Für jeden NULL-Werte `T` wird es ein entsprechenden nullable-Typ `T?`, die einen zusätzlichen Wert enthalten kann `null`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-261">For each non-nullable value type `T` there is a corresponding nullable type `T?`, which can hold an additional value `null`.</span></span> <span data-ttu-id="b9fcc-262">Z. B. `int?` ist ein Typ, der alle 32-Bit-Ganzzahlwert oder den Wert aufnehmen kann `null`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-262">For instance, `int?` is a type that can hold any 32 bit integer or the value `null`.</span></span>

<span data-ttu-id="b9fcc-263">C#Typsystem ist dahingehend vereinheitlicht, dass ein Wert eines beliebigen Typs als Objekt behandelt werden kann.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-263">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="b9fcc-264">Jeder Typ in C# ist direkt oder indirekt vom `object`-Klassentyp abgeleitet, und `object` ist die ultimative Basisklasse aller Typen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-264">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="b9fcc-265">Werte von Verweistypen werden als Objekte behandelt, indem die Werte einfach als Typ `object` angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-265">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="b9fcc-266">Werte von Werttypen werden als Objekte behandelt, indem Sie Ausführung ***Boxing*** und ***unboxing*** Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-266">Values of value types are treated as objects by performing ***boxing*** and ***unboxing*** operations.</span></span> <span data-ttu-id="b9fcc-267">Im folgenden Beispiel wird ein `int`-Wert in ein `object` und wieder in einen `int`-Wert konvertiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-267">In the following example, an `int` value is converted to `object` and back again to `int`.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // Boxing
        int j = (int)o;        // Unboxing
    }
}
```
<span data-ttu-id="b9fcc-268">Wenn der Wert eines Werttyps in den Typ konvertiert wird `object`, eine Objektinstanz, die auch "Box" genannte wird zum Speichern des Werts zugeordnet, und der Wert wird in diese Box kopiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-268">When a value of a value type is converted to type `object`, an object instance, also called a "box," is allocated to hold the value, and the value is copied into that box.</span></span> <span data-ttu-id="b9fcc-269">Im Gegensatz dazu, wann ein `object` Verweis auf einen Werttyp umgewandelt wird, wird eine Überprüfung durchgeführt, die das referenzierte Objekt ein Feld des korrekten Datentyps ist und, wenn die Überprüfung erfolgreich ist, wird der Wert in das Feld kopiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-269">Conversely, when an `object` reference is cast to a value type, a check is made that the referenced object is a box of the correct value type, and, if the check succeeds, the value in the box is copied out.</span></span>

<span data-ttu-id="b9fcc-270">C#der unified Type System bedeutet, dass Werttypen "bei Bedarf." Objekte werden können</span><span class="sxs-lookup"><span data-stu-id="b9fcc-270">C#'s unified type system effectively means that value types can become objects "on demand."</span></span> <span data-ttu-id="b9fcc-271">Aufgrund der Vereinheitlichung können Bibliotheken für allgemeine Zwecke, die den Typ `object` verwenden, sowohl mit Verweis- als auch Werttypen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-271">Because of the unification, general-purpose libraries that use type `object` can be used with both reference types and value types.</span></span>

<span data-ttu-id="b9fcc-272">Es gibt mehrere Arten von ***Variablen*** in C#, einschließlich Feldern, Arrayelementen, lokalen Variablen und Parametern.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-272">There are several kinds of ***variables*** in C#, including fields, array elements, local variables, and parameters.</span></span> <span data-ttu-id="b9fcc-273">Variablen stellen Speicherorte dar, und jede Variable hat einen Typ, der bestimmt, welche Werte können in der Variablen gespeichert wird, wie in der folgenden Tabelle gezeigt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-273">Variables represent storage locations, and every variable has a type that determines what values can be stored in the variable, as shown by the following table.</span></span>


| __<span data-ttu-id="b9fcc-274">Variablentyp</span><span class="sxs-lookup"><span data-stu-id="b9fcc-274">Type of Variable</span></span>__    | __<span data-ttu-id="b9fcc-275">Mögliche Inhalt</span><span class="sxs-lookup"><span data-stu-id="b9fcc-275">Possible Contents</span></span>__ |
|-------------------------|-----------------------|
| <span data-ttu-id="b9fcc-276">Nicht auf NULL festlegbarer Werttyp</span><span class="sxs-lookup"><span data-stu-id="b9fcc-276">Non-nullable value type</span></span> | <span data-ttu-id="b9fcc-277">Ein Wert genau dieses Typs</span><span class="sxs-lookup"><span data-stu-id="b9fcc-277">A value of that exact type</span></span> |
| <span data-ttu-id="b9fcc-278">Auf NULL festlegbarer Werttyp</span><span class="sxs-lookup"><span data-stu-id="b9fcc-278">Nullable value type</span></span>     | <span data-ttu-id="b9fcc-279">Ein null-Wert oder einen Wert genau dieses Typs</span><span class="sxs-lookup"><span data-stu-id="b9fcc-279">A null value or a value of that exact type</span></span> |
| `object`                | <span data-ttu-id="b9fcc-280">Ein null-Verweis, einen Verweis auf ein Objekt von einem beliebigen Verweistyp oder ein Verweis auf einen geschachtelten Wert eines beliebigen Werttyps</span><span class="sxs-lookup"><span data-stu-id="b9fcc-280">A null reference, a reference to an object of any reference type, or a reference to a boxed value of any value type</span></span> |
| <span data-ttu-id="b9fcc-281">Klassentyp</span><span class="sxs-lookup"><span data-stu-id="b9fcc-281">Class type</span></span>              | <span data-ttu-id="b9fcc-282">Ein null-Verweis, einen Verweis auf eine Instanz dieses Klassentyps oder ein Verweis auf eine Instanz einer Klasse abgeleitet werden, von diesem Klassentyp</span><span class="sxs-lookup"><span data-stu-id="b9fcc-282">A null reference, a reference to an instance of that class type, or a reference to an instance of a class derived from that class type</span></span> |
| <span data-ttu-id="b9fcc-283">Schnittstellentyp</span><span class="sxs-lookup"><span data-stu-id="b9fcc-283">Interface type</span></span>          | <span data-ttu-id="b9fcc-284">Ein null-Verweis, einen Verweis auf eine Instanz eines Klassentyps, der diesen Schnittstellentyp implementiert oder einen Verweis auf einen geschachtelten Wert eines Werttyps, der diesen Schnittstellentyp implementiert</span><span class="sxs-lookup"><span data-stu-id="b9fcc-284">A null reference, a reference to an instance of a class type that implements that interface type, or a reference to a boxed value of a value type that implements that interface type</span></span> |
| <span data-ttu-id="b9fcc-285">Arraytyp</span><span class="sxs-lookup"><span data-stu-id="b9fcc-285">Array type</span></span>              | <span data-ttu-id="b9fcc-286">Ein null-Verweis, einen Verweis auf eine Instanz dieses Arraytyps oder ein Verweis auf eine Instanz von eines kompatiblen Arraytyps</span><span class="sxs-lookup"><span data-stu-id="b9fcc-286">A null reference, a reference to an instance of that array type, or a reference to an instance of a compatible array type</span></span> |
| <span data-ttu-id="b9fcc-287">Delegattyp</span><span class="sxs-lookup"><span data-stu-id="b9fcc-287">Delegate type</span></span>           | <span data-ttu-id="b9fcc-288">Ein null-Verweis oder einen Verweis auf eine Instanz dieses Delegattyps</span><span class="sxs-lookup"><span data-stu-id="b9fcc-288">A null reference or a reference to an instance of that delegate type</span></span> |

## <a name="expressions"></a><span data-ttu-id="b9fcc-289">Ausdrücke</span><span class="sxs-lookup"><span data-stu-id="b9fcc-289">Expressions</span></span>

<span data-ttu-id="b9fcc-290">***Ausdrücke*** bestehen aus ***Operanden*** und ***Operatoren***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-290">***Expressions*** are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="b9fcc-291">Die Operatoren eines Ausdrucks geben an, welche Operationen auf die Operanden angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-291">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="b9fcc-292">Beispiele für Operatoren sind `+`, `-`, `*`, `/` und `new`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-292">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="b9fcc-293">Beispiele für Operanden sind Literale, Felder, lokale Variablen und Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-293">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="b9fcc-294">Wenn ein Ausdruck mehrere Operatoren enthält, bestimmt die ***Rangfolge*** der Operatoren die Reihenfolge, in der die einzelnen Operatoren ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-294">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="b9fcc-295">Der Ausdruck `x + y * z` wird z.B. als `x + (y * z)` ausgewertet, da der `*`-Operator Vorrang vor dem `+`-Operator hat.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-295">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span>

<span data-ttu-id="b9fcc-296">Die meisten Operatoren können ***überladen*** werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-296">Most operators can be ***overloaded***.</span></span> <span data-ttu-id="b9fcc-297">Das Überladen von Operatoren ermöglicht die Angabe benutzerdefinierter Operatorimplementierungen für Vorgänge, in denen einer der Operanden oder beide einer benutzerdefinierten Klasse oder einem benutzerdefinierten Strukturtyp angehören.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-297">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type.</span></span>

<span data-ttu-id="b9fcc-298">Die folgende Tabelle enthält C#Operatoren, die Operatorkategorien gemäß der Rangfolge von der höchsten zur niedrigsten Ebene auflisten.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-298">The following table summarizes C#'s operators, listing the operator categories in order of precedence from highest to lowest.</span></span> <span data-ttu-id="b9fcc-299">Operatoren der gleichen Kategorie haben den gleichen Rang.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-299">Operators in the same category have equal precedence.</span></span>


| __<span data-ttu-id="b9fcc-300">Kategorie</span><span class="sxs-lookup"><span data-stu-id="b9fcc-300">Category</span></span>__                     | __<span data-ttu-id="b9fcc-301">Ausdruck</span><span class="sxs-lookup"><span data-stu-id="b9fcc-301">Expression</span></span>__    | __<span data-ttu-id="b9fcc-302">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-302">Description</span></span>__ |
|----------------------------------|-------------------|-----------------|
| <span data-ttu-id="b9fcc-303">Primär</span><span class="sxs-lookup"><span data-stu-id="b9fcc-303">Primary</span></span>                          | `x.m`             | <span data-ttu-id="b9fcc-304">Memberzugriff</span><span class="sxs-lookup"><span data-stu-id="b9fcc-304">Member access</span></span> |
|                                  | `x(...)`          | <span data-ttu-id="b9fcc-305">Methoden- und Delegataufruf</span><span class="sxs-lookup"><span data-stu-id="b9fcc-305">Method and delegate invocation</span></span> |
|                                  | `x[...]`          | <span data-ttu-id="b9fcc-306">Array- und Indexerzugriff</span><span class="sxs-lookup"><span data-stu-id="b9fcc-306">Array and indexer access</span></span> |
|                                  | `x++`             | <span data-ttu-id="b9fcc-307">Postinkrement</span><span class="sxs-lookup"><span data-stu-id="b9fcc-307">Post-increment</span></span> |
|                                  | `x--`             | <span data-ttu-id="b9fcc-308">Postdekrement</span><span class="sxs-lookup"><span data-stu-id="b9fcc-308">Post-decrement</span></span> |
|                                  | `new T(...)`      | <span data-ttu-id="b9fcc-309">Objekt- und Delegaterstellung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-309">Object and delegate creation</span></span> |
|                                  | `new T(...){...}` | <span data-ttu-id="b9fcc-310">Objekterstellung mit Initialisierer</span><span class="sxs-lookup"><span data-stu-id="b9fcc-310">Object creation with initializer</span></span> |
|                                  | `new {...}`       | <span data-ttu-id="b9fcc-311">Anonymer Objektinitialisierer</span><span class="sxs-lookup"><span data-stu-id="b9fcc-311">Anonymous object initializer</span></span> |
|                                  | `new T[...]`      | <span data-ttu-id="b9fcc-312">Arrayerstellung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-312">Array creation</span></span> |
|                                  | `typeof(T)`       | <span data-ttu-id="b9fcc-313">Abrufen `System.Type` für Objekt</span><span class="sxs-lookup"><span data-stu-id="b9fcc-313">Obtain `System.Type` object for</span></span> `T` |
|                                  | `checked(x)`      | <span data-ttu-id="b9fcc-314">Auswerten von Ausdrücken in geprüftem Kontext</span><span class="sxs-lookup"><span data-stu-id="b9fcc-314">Evaluate expression in checked context</span></span> |
|                                  | `unchecked(x)`    | <span data-ttu-id="b9fcc-315">Auswerten von Ausdrücken in nicht geprüftem Kontext</span><span class="sxs-lookup"><span data-stu-id="b9fcc-315">Evaluate expression in unchecked context</span></span> |
|                                  | `default(T)`      | <span data-ttu-id="b9fcc-316">Abrufen des Standardwerts vom Typ</span><span class="sxs-lookup"><span data-stu-id="b9fcc-316">Obtain default value of type</span></span> `T` |
|                                  | `delegate {...}`  | <span data-ttu-id="b9fcc-317">Anonyme Funktion (anonyme Methode)</span><span class="sxs-lookup"><span data-stu-id="b9fcc-317">Anonymous function (anonymous method)</span></span> |
| <span data-ttu-id="b9fcc-318">Unär</span><span class="sxs-lookup"><span data-stu-id="b9fcc-318">Unary</span></span>                            | `+x`              | <span data-ttu-id="b9fcc-319">Identität</span><span class="sxs-lookup"><span data-stu-id="b9fcc-319">Identity</span></span> |
|                                  | `-x`              | <span data-ttu-id="b9fcc-320">Negation</span><span class="sxs-lookup"><span data-stu-id="b9fcc-320">Negation</span></span> |
|                                  | `!x`              | <span data-ttu-id="b9fcc-321">Logische Negation</span><span class="sxs-lookup"><span data-stu-id="b9fcc-321">Logical negation</span></span> |
|                                  | `~x`              | <span data-ttu-id="b9fcc-322">Bitweise Negation</span><span class="sxs-lookup"><span data-stu-id="b9fcc-322">Bitwise negation</span></span> |
|                                  | `++x`             | <span data-ttu-id="b9fcc-323">Präinkrement</span><span class="sxs-lookup"><span data-stu-id="b9fcc-323">Pre-increment</span></span> |
|                                  | `--x`             | <span data-ttu-id="b9fcc-324">Prädekrement</span><span class="sxs-lookup"><span data-stu-id="b9fcc-324">Pre-decrement</span></span> |
|                                  | `(T)x`            | <span data-ttu-id="b9fcc-325">Konvertieren Sie explizit `x` eingeben</span><span class="sxs-lookup"><span data-stu-id="b9fcc-325">Explicitly convert `x` to type</span></span> `T` |
|                                  | `await x`         | <span data-ttu-id="b9fcc-326">Asynchrones Warten auf den Abschluss von `x`</span><span class="sxs-lookup"><span data-stu-id="b9fcc-326">Asynchronously wait for `x` to complete</span></span> |
| <span data-ttu-id="b9fcc-327">Multiplikativ</span><span class="sxs-lookup"><span data-stu-id="b9fcc-327">Multiplicative</span></span>                   | `x * y`           | <span data-ttu-id="b9fcc-328">Multiplikation</span><span class="sxs-lookup"><span data-stu-id="b9fcc-328">Multiplication</span></span> |
|                                  | `x / y`           | <span data-ttu-id="b9fcc-329">Division</span><span class="sxs-lookup"><span data-stu-id="b9fcc-329">Division</span></span> |
|                                  | `x % y`           | <span data-ttu-id="b9fcc-330">Rest</span><span class="sxs-lookup"><span data-stu-id="b9fcc-330">Remainder</span></span> |
| <span data-ttu-id="b9fcc-331">Additiv</span><span class="sxs-lookup"><span data-stu-id="b9fcc-331">Additive</span></span>                         | `x + y`           | <span data-ttu-id="b9fcc-332">Addition, Zeichenfolgenverkettung, Delegatkombination</span><span class="sxs-lookup"><span data-stu-id="b9fcc-332">Addition, string concatenation, delegate combination</span></span> |
|                                  | `x - y`           | <span data-ttu-id="b9fcc-333">Subtraktion, Delegatentfernung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-333">Subtraction, delegate removal</span></span> |
| <span data-ttu-id="b9fcc-334">Shift</span><span class="sxs-lookup"><span data-stu-id="b9fcc-334">Shift</span></span>                            | `x << y`          | <span data-ttu-id="b9fcc-335">Linksverschiebung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-335">Shift left</span></span> |
|                                  | `x >> y`          | <span data-ttu-id="b9fcc-336">Rechtsverschiebung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-336">Shift right</span></span> |
| <span data-ttu-id="b9fcc-337">Relational und Typtest</span><span class="sxs-lookup"><span data-stu-id="b9fcc-337">Relational and type testing</span></span>      | `x < y`           | <span data-ttu-id="b9fcc-338">Kleiner als</span><span class="sxs-lookup"><span data-stu-id="b9fcc-338">Less than</span></span> |
|                                  | `x > y`           | <span data-ttu-id="b9fcc-339">Größer als</span><span class="sxs-lookup"><span data-stu-id="b9fcc-339">Greater than</span></span> |
|                                  | `x <= y`          | <span data-ttu-id="b9fcc-340">Kleiner oder gleich</span><span class="sxs-lookup"><span data-stu-id="b9fcc-340">Less than or equal</span></span> |
|                                  | `x >= y`          | <span data-ttu-id="b9fcc-341">Größer oder gleich</span><span class="sxs-lookup"><span data-stu-id="b9fcc-341">Greater than or equal</span></span> |
|                                  | `x is T`          | <span data-ttu-id="b9fcc-342">`true` zurückgeben, wenn `x` ein `T` ist, andernfalls `false`</span><span class="sxs-lookup"><span data-stu-id="b9fcc-342">Return `true` if `x` is a `T`, `false` otherwise</span></span> |
|                                  | `x as T`          | <span data-ttu-id="b9fcc-343">Zurückgeben `x` als `T`, oder `null` Wenn `x` kein</span><span class="sxs-lookup"><span data-stu-id="b9fcc-343">Return `x` typed as `T`, or `null` if `x` is not a</span></span> `T` |
| <span data-ttu-id="b9fcc-344">Gleichheit</span><span class="sxs-lookup"><span data-stu-id="b9fcc-344">Equality</span></span>                         | `x == y`          | <span data-ttu-id="b9fcc-345">Gleich</span><span class="sxs-lookup"><span data-stu-id="b9fcc-345">Equal</span></span>      |
|                                  | `x != y`          | <span data-ttu-id="b9fcc-346">Ungleich</span><span class="sxs-lookup"><span data-stu-id="b9fcc-346">Not equal</span></span> |
| <span data-ttu-id="b9fcc-347">Logisches AND</span><span class="sxs-lookup"><span data-stu-id="b9fcc-347">Logical AND</span></span>                      | `x & y`           | <span data-ttu-id="b9fcc-348">Ganzzahliges bitweises AND, boolesches logisches AND</span><span class="sxs-lookup"><span data-stu-id="b9fcc-348">Integer bitwise AND, boolean logical AND</span></span> |
| <span data-ttu-id="b9fcc-349">Logisches XOR</span><span class="sxs-lookup"><span data-stu-id="b9fcc-349">Logical XOR</span></span>                      | `x ^ y`           | <span data-ttu-id="b9fcc-350">Ganzzahliges bitweises XOR, boolesches logisches XOR</span><span class="sxs-lookup"><span data-stu-id="b9fcc-350">Integer bitwise XOR, boolean logical XOR</span></span> |
| <span data-ttu-id="b9fcc-351">Logisches OR</span><span class="sxs-lookup"><span data-stu-id="b9fcc-351">Logical OR</span></span>                       | <code>x &#124; y</code> | <span data-ttu-id="b9fcc-352">Ganzzahliges bitweises OR, boolesches logisches OR</span><span class="sxs-lookup"><span data-stu-id="b9fcc-352">Integer bitwise OR, boolean logical OR</span></span> |
| <span data-ttu-id="b9fcc-353">Bedingtes AND</span><span class="sxs-lookup"><span data-stu-id="b9fcc-353">Conditional AND</span></span>                  | `x && y`          | <span data-ttu-id="b9fcc-354">Wertet `y` nur, wenn `x` ist</span><span class="sxs-lookup"><span data-stu-id="b9fcc-354">Evaluates `y` only if `x` is</span></span> `true` |
| <span data-ttu-id="b9fcc-355">Bedingtes OR</span><span class="sxs-lookup"><span data-stu-id="b9fcc-355">Conditional OR</span></span>                   | <code>x &#124;&#124; y</code> | <span data-ttu-id="b9fcc-356">Wertet `y` nur, wenn `x` ist</span><span class="sxs-lookup"><span data-stu-id="b9fcc-356">Evaluates `y` only if `x` is</span></span> `false` |
| <span data-ttu-id="b9fcc-357">NULL-Sammeloperator</span><span class="sxs-lookup"><span data-stu-id="b9fcc-357">Null coalescing</span></span>                  | `x ?? y`          | <span data-ttu-id="b9fcc-358">Ergibt `y` Wenn `x` ist `null`zu `x` andernfalls</span><span class="sxs-lookup"><span data-stu-id="b9fcc-358">Evaluates to `y` if `x` is `null`, to `x` otherwise</span></span> |
| <span data-ttu-id="b9fcc-359">Bedingt</span><span class="sxs-lookup"><span data-stu-id="b9fcc-359">Conditional</span></span>                      | `x ? y : z`       | <span data-ttu-id="b9fcc-360">Wertet `y` Wenn `x` ist `true`, `z` Wenn `x` ist</span><span class="sxs-lookup"><span data-stu-id="b9fcc-360">Evaluates `y` if `x` is `true`, `z` if `x` is</span></span> `false` |
| <span data-ttu-id="b9fcc-361">Zuweisung oder anonyme Funktion</span><span class="sxs-lookup"><span data-stu-id="b9fcc-361">Assignment or anonymous function</span></span> | `x = y`           | <span data-ttu-id="b9fcc-362">Zuweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-362">Assignment</span></span> |
|                                  | `x op= y`         | <span data-ttu-id="b9fcc-363">Zusammengesetzte Zuweisung; unterstützte Operatoren sind</span><span class="sxs-lookup"><span data-stu-id="b9fcc-363">Compound assignment; supported operators are</span></span> `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code> |
|                                  | `(T x) => y`      | <span data-ttu-id="b9fcc-364">Anonyme Funktion (Lambda-Ausdruck)</span><span class="sxs-lookup"><span data-stu-id="b9fcc-364">Anonymous function (lambda expression)</span></span> |

## <a name="statements"></a><span data-ttu-id="b9fcc-365">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-365">Statements</span></span>

<span data-ttu-id="b9fcc-366">Die Aktionen eines Programms werden mit ***Anweisungen*** ausgedrückt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-366">The actions of a program are expressed using ***statements***.</span></span> <span data-ttu-id="b9fcc-367">C# unterstützt verschiedene Arten von Anweisungen, von denen ein Teil als eingebettete Anweisungen definiert ist.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-367">C# supports several different kinds of statements, a number of which are defined in terms of embedded statements.</span></span>

<span data-ttu-id="b9fcc-368">Ein ***Block*** ermöglicht, mehrere Anweisungen in Kontexten zu schreiben, in denen eine einzelne Anweisung zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-368">A ***block*** permits multiple statements to be written in contexts where a single statement is allowed.</span></span> <span data-ttu-id="b9fcc-369">Ein Block besteht aus einer Liste von Anweisungen, die zwischen den Trennzeichen `{` und `}` geschrieben sind.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-369">A block consists of a list of statements written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="b9fcc-370">***Deklarationsanweisungen*** werden verwendet, um lokale Variablen und Konstanten deklarieren.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-370">***Declaration statements*** are used to declare local variables and constants.</span></span>

<span data-ttu-id="b9fcc-371">***Ausdrucksanweisungen*** werden zum Auswerten von Ausdrücken verwendet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-371">***Expression statements*** are used to evaluate expressions.</span></span> <span data-ttu-id="b9fcc-372">Ausdrücke, die als Anweisungen verwendet werden können, enthalten Methodenaufrufe, objektzuordnungen mit dem `new` Operator, Zuweisungen mit `=` und die Verbundzuweisungsoperatoren, inkrementier- und dekrementiervorgänge-Vorgänge, die unter Verwendung der `++`und `--` Operatoren und await-Ausdrücken.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-372">Expressions that can be used as statements include method invocations, object allocations using the `new` operator, assignments using `=` and the compound assignment operators, increment and decrement operations using the `++` and `--` operators and await expressions.</span></span>

<span data-ttu-id="b9fcc-373">***Auswahlanweisungen*** werden verwendet, um eine Anzahl von möglichen Anweisungen für die Ausführung anhand des Werts eines Ausdrucks auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-373">***Selection statements*** are used to select one of a number of possible statements for execution based on the value of some expression.</span></span> <span data-ttu-id="b9fcc-374">Zu dieser Gruppe gehören die `if`- und `switch`-Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-374">In this group are the `if` and `switch` statements.</span></span>

<span data-ttu-id="b9fcc-375">***Iterationsanweisungen*** werden verwendet, um eine eingebettete Anweisung wiederholt auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-375">***Iteration statements*** are used to repeatedly execute an embedded statement.</span></span> <span data-ttu-id="b9fcc-376">Zu dieser Gruppe gehören die `while`-, `do`-, `for`- und `foreach`-Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-376">In this group are the `while`, `do`, `for`, and `foreach` statements.</span></span>

<span data-ttu-id="b9fcc-377">***Sprunganweisungen*** werden verwendet, um die Steuerung zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-377">***Jump statements*** are used to transfer control.</span></span> <span data-ttu-id="b9fcc-378">Zu dieser Gruppe gehören die `break`-, `continue`-, `goto`-, `throw`-, `return`- und `yield`-Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-378">In this group are the `break`, `continue`, `goto`, `throw`, `return`, and `yield` statements.</span></span>

<span data-ttu-id="b9fcc-379">Mit der `try`... `catch`-Anweisung werden Ausnahmen abgefangen, die während der Ausführung eines Blocks auftreten, und mit der `try`... `finally`-Anweisung wird Finalisierungscode angegeben, der immer ausgeführt wird, unabhängig davon, ob eine Ausnahme aufgetreten ist oder nicht.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-379">The `try`...`catch` statement is used to catch exceptions that occur during execution of a block, and the `try`...`finally` statement is used to specify finalization code that is always executed, whether an exception occurred or not.</span></span>

<span data-ttu-id="b9fcc-380">Die `checked` und `unchecked` Anweisungen werden verwendet, um den Kontext für arithmetische Operationen für ganzzahlige Typen und Konvertierungen für die überlaufüberprüfung zu steuern.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-380">The `checked` and `unchecked` statements are used to control the overflow checking context for integral-type arithmetic operations and conversions.</span></span>

<span data-ttu-id="b9fcc-381">Die `lock`-Anweisung wird verwendet, um die Sperre für gegenseitigen Ausschluss für ein bestimmtes Objekt abzurufen, eine Anweisung auszuführen und die Sperre aufzuheben.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-381">The `lock` statement is used to obtain the mutual-exclusion lock for a given object, execute a statement, and then release the lock.</span></span>

<span data-ttu-id="b9fcc-382">Die `using`-Anweisung wird verwendet, um eine Ressource abzurufen, eine Anweisung auszuführen und dann diese Ressource zu verwerfen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-382">The `using` statement is used to obtain a resource, execute a statement, and then dispose of that resource.</span></span>

<span data-ttu-id="b9fcc-383">Im folgenden finden Sie Beispiele für jede Art von Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-383">Below are examples of each kind of statement</span></span>

__<span data-ttu-id="b9fcc-384">Deklarationen von lokalen Variablen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-384">Local variable declarations</span></span>__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__<span data-ttu-id="b9fcc-385">Deklaration von lokalen Konstanten</span><span class="sxs-lookup"><span data-stu-id="b9fcc-385">Local constant declaration</span></span>__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__<span data-ttu-id="b9fcc-386">Ausdrucksanweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-386">Expression statement</span></span>__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__`if` <span data-ttu-id="b9fcc-387">Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-387">statement</span></span>__

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


__`switch` <span data-ttu-id="b9fcc-388">Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-388">statement</span></span>__

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

__`while` <span data-ttu-id="b9fcc-389">Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-389">statement</span></span>__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do` <span data-ttu-id="b9fcc-390">Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-390">statement</span></span>__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for` <span data-ttu-id="b9fcc-391">Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-391">statement</span></span>__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach` <span data-ttu-id="b9fcc-392">Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-392">statement</span></span>__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break` <span data-ttu-id="b9fcc-393">Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-393">statement</span></span>__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue` <span data-ttu-id="b9fcc-394">Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-394">statement</span></span>__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto` <span data-ttu-id="b9fcc-395">Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-395">statement</span></span>__

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

__`return` <span data-ttu-id="b9fcc-396">Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-396">statement</span></span>__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield` <span data-ttu-id="b9fcc-397">Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-397">statement</span></span>__

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

__`throw` <span data-ttu-id="b9fcc-398">und `try` Anweisungen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-398">and `try` statements</span></span>__

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

__`checked` <span data-ttu-id="b9fcc-399">und `unchecked` Anweisungen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-399">and `unchecked` statements</span></span>__

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // Exception
    }
    unchecked {
        Console.WriteLine(i + 1);        // Overflow
    }
}
```

__`lock` <span data-ttu-id="b9fcc-400">Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-400">statement</span></span>__

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

__`using` <span data-ttu-id="b9fcc-401">Anweisung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-401">statement</span></span>__

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a><span data-ttu-id="b9fcc-402">Klassen und Objekte</span><span class="sxs-lookup"><span data-stu-id="b9fcc-402">Classes and objects</span></span>

<span data-ttu-id="b9fcc-403">***Klassen*** sind die grundlegendsten der C#-Typen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-403">***Classes*** are the most fundamental of C#'s types.</span></span> <span data-ttu-id="b9fcc-404">Eine Klasse ist eine Datenstruktur, die einen Zustand (Felder) und Aktionen (Methoden und andere Funktionsmember) in einer einzigen Einheit kombiniert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-404">A class is a data structure that combines state (fields) and actions (methods and other function members) in a single unit.</span></span> <span data-ttu-id="b9fcc-405">Eine Klasse stellt eine Definition für dynamisch erstellte ***Instanzen*** der Klasse, auch bekannt als ***Objekte*** bereit.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-405">A class provides a definition for dynamically created ***instances*** of the class, also known as ***objects***.</span></span> <span data-ttu-id="b9fcc-406">Klassen unterstützen ***Vererbung*** und ***Polymorphie***. Dies sind Mechanismen, durch die ***abgeleitete Klassen*** erweitert und ***Basisklassen*** spezialisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-406">Classes support ***inheritance*** and ***polymorphism***, mechanisms whereby ***derived classes*** can extend and specialize ***base classes***.</span></span>

<span data-ttu-id="b9fcc-407">Neue Klassen werden mithilfe von Klassendeklarationen erstellt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-407">New classes are created using class declarations.</span></span> <span data-ttu-id="b9fcc-408">Eine Klassendeklaration beginnt mit einem Header, der die Attribute und Modifizierer der Klasse, den Namen der Klasse, die Basisklasse (sofern vorhanden) und die von der Klasse implementierten Schnittstellen angibt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-408">A class declaration starts with a header that specifies the attributes and modifiers of the class, the name of the class, the base class (if given), and the interfaces implemented by the class.</span></span> <span data-ttu-id="b9fcc-409">Auf den Header folgt der Klassenkörper. Dieser besteht aus einer Liste der Memberdeklarationen, die zwischen den Trennzeichen `{` und `}` eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-409">The header is followed by the class body, which consists of a list of member declarations written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="b9fcc-410">Nachfolgend sehen Sie eine Deklaration einer einfachen Klasse namens `Point`:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-410">The following is a declaration of a simple class named `Point`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="b9fcc-411">Instanzen von Klassen werden mit dem `new`-Operator erstellt. Dieser reserviert Speicher für eine neue Instanz, ruft einen Konstruktor zum Initialisieren der Instanz auf und gibt einen Verweis auf die Instanz zurück.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-411">Instances of classes are created using the `new` operator, which allocates memory for a new instance, invokes a constructor to initialize the instance, and returns a reference to the instance.</span></span> <span data-ttu-id="b9fcc-412">Die folgenden Anweisungen erstellen Sie zwei `Point` Objekte und Verweise auf diese Objekte in zwei Variablen zu speichern:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-412">The following statements create two `Point` objects and store references to those objects in two variables:</span></span>

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
<span data-ttu-id="b9fcc-413">Der von einem Objekt belegte Arbeitsspeicher wird automatisch freigegeben, wenn das Objekt nicht mehr verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-413">The memory occupied by an object is automatically reclaimed when the object is no longer in use.</span></span> <span data-ttu-id="b9fcc-414">Es ist weder erforderlich noch möglich, die Zuweisung von Objekten in C# explizit aufzuheben.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-414">It is neither necessary nor possible to explicitly deallocate objects in C#.</span></span>

### <a name="members"></a><span data-ttu-id="b9fcc-415">Member</span><span class="sxs-lookup"><span data-stu-id="b9fcc-415">Members</span></span>

<span data-ttu-id="b9fcc-416">Die Member einer Klasse sind entweder ***statische Member*** oder ***Instanzmember***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-416">The members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="b9fcc-417">Statische Member gehören zu Klassen, Instanzmember gehören zu Objekten (Instanzen von Klassen).</span><span class="sxs-lookup"><span data-stu-id="b9fcc-417">Static members belong to classes, and instance members belong to objects (instances of classes).</span></span>

<span data-ttu-id="b9fcc-418">Die folgende Tabelle enthält eine Übersicht über die Arten von Membern, die eine Klasse enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-418">The following table provides an overview of the kinds of members a class can contain.</span></span>


| __<span data-ttu-id="b9fcc-419">Member</span><span class="sxs-lookup"><span data-stu-id="b9fcc-419">Member</span></span>__   | __<span data-ttu-id="b9fcc-420">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-420">Description</span></span>__ |
|------------  |-----------------|
| <span data-ttu-id="b9fcc-421">Konstanten</span><span class="sxs-lookup"><span data-stu-id="b9fcc-421">Constants</span></span>    | <span data-ttu-id="b9fcc-422">Konstante Werte, die der Klasse zugeordnet sind</span><span class="sxs-lookup"><span data-stu-id="b9fcc-422">Constant values associated with the class</span></span> |
| <span data-ttu-id="b9fcc-423">Felder</span><span class="sxs-lookup"><span data-stu-id="b9fcc-423">Fields</span></span>       | <span data-ttu-id="b9fcc-424">Variablen der Klasse</span><span class="sxs-lookup"><span data-stu-id="b9fcc-424">Variables of the class</span></span> |
| <span data-ttu-id="b9fcc-425">Methoden</span><span class="sxs-lookup"><span data-stu-id="b9fcc-425">Methods</span></span>      | <span data-ttu-id="b9fcc-426">Berechnungen und Aktionen, die von der Klasse ausgeführt werden</span><span class="sxs-lookup"><span data-stu-id="b9fcc-426">Computations and actions that can be performed by the class</span></span> |
| <span data-ttu-id="b9fcc-427">Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="b9fcc-427">Properties</span></span>   | <span data-ttu-id="b9fcc-428">Aktionen im Zusammenhang mit dem Lesen und Schreiben von benannten Eigenschaften der Klasse</span><span class="sxs-lookup"><span data-stu-id="b9fcc-428">Actions associated with reading and writing named properties of the class</span></span> |
| <span data-ttu-id="b9fcc-429">Indexer</span><span class="sxs-lookup"><span data-stu-id="b9fcc-429">Indexers</span></span>     | <span data-ttu-id="b9fcc-430">Aktionen im Zusammenhang mit dem Indizieren von Instanzen der Klasse, z.B. einem Array</span><span class="sxs-lookup"><span data-stu-id="b9fcc-430">Actions associated with indexing instances of the class like an array</span></span> |
| <span data-ttu-id="b9fcc-431">Ereignisse</span><span class="sxs-lookup"><span data-stu-id="b9fcc-431">Events</span></span>       | <span data-ttu-id="b9fcc-432">Benachrichtigungen, die von der Klasse generiert werden können</span><span class="sxs-lookup"><span data-stu-id="b9fcc-432">Notifications that can be generated by the class</span></span> |
| <span data-ttu-id="b9fcc-433">Operatoren</span><span class="sxs-lookup"><span data-stu-id="b9fcc-433">Operators</span></span>    | <span data-ttu-id="b9fcc-434">Operatoren für Konvertierungen und Ausdrücke, die von der Klasse unterstützt werden</span><span class="sxs-lookup"><span data-stu-id="b9fcc-434">Conversions and expression operators supported by the class</span></span> |
| <span data-ttu-id="b9fcc-435">Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="b9fcc-435">Constructors</span></span> | <span data-ttu-id="b9fcc-436">Aktionen, die zum Initialisieren von Instanzen der Klasse oder der Klasse selbst benötigt werden</span><span class="sxs-lookup"><span data-stu-id="b9fcc-436">Actions required to initialize instances of the class or the class itself</span></span> |
| <span data-ttu-id="b9fcc-437">Destruktoren</span><span class="sxs-lookup"><span data-stu-id="b9fcc-437">Destructors</span></span>  | <span data-ttu-id="b9fcc-438">Aktionen, die ausgeführt werden, bevor Instanzen der Klasse dauerhaft verworfen werden</span><span class="sxs-lookup"><span data-stu-id="b9fcc-438">Actions to perform before instances of the class are permanently discarded</span></span> |
| <span data-ttu-id="b9fcc-439">Typen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-439">Types</span></span>        | <span data-ttu-id="b9fcc-440">Geschachtelte Typen, die von der Klasse deklariert werden</span><span class="sxs-lookup"><span data-stu-id="b9fcc-440">Nested types declared by the class</span></span> |

### <a name="accessibility"></a><span data-ttu-id="b9fcc-441">Zugriff</span><span class="sxs-lookup"><span data-stu-id="b9fcc-441">Accessibility</span></span>

<span data-ttu-id="b9fcc-442">Jeder Member einer Klasse ist mit einem Zugriff verknüpft, der die Regionen des Programmtexts steuert, die auf den Member zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-442">Each member of a class has an associated accessibility, which controls the regions of program text that are able to access the member.</span></span> <span data-ttu-id="b9fcc-443">Es gibt fünf mögliche Formen des Zugriffs.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-443">There are five possible forms of accessibility.</span></span> <span data-ttu-id="b9fcc-444">Diese werden in der folgenden Tabelle zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-444">These are summarized in the following table.</span></span>


| __<span data-ttu-id="b9fcc-445">Zugriff</span><span class="sxs-lookup"><span data-stu-id="b9fcc-445">Accessibility</span></span>__    | __<span data-ttu-id="b9fcc-446">Bedeutung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-446">Meaning</span></span>__ |
|----------------------|-----------------|
| `public`             | <span data-ttu-id="b9fcc-447">Der Zugriff ist nicht eingeschränkt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-447">Access not limited</span></span> |
| `protected`          | <span data-ttu-id="b9fcc-448">Der Zugriff ist auf diese Klasse oder auf von dieser Klasse abgeleitete Klassen beschränkt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-448">Access limited to this class or classes derived from this class</span></span> |
| `internal`           | <span data-ttu-id="b9fcc-449">Der Zugriff ist auf dieses Programm beschränkt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-449">Access limited to this program</span></span> |
| `protected internal` | <span data-ttu-id="b9fcc-450">Der Zugriff ist auf dieses Programm oder auf von dieser Klasse abgeleitete Klassen beschränkt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-450">Access limited to this program or classes derived from this class</span></span> |
| `private`            | <span data-ttu-id="b9fcc-451">Der Zugriff ist auf diese Klasse beschränkt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-451">Access limited to this class</span></span> |

### <a name="type-parameters"></a><span data-ttu-id="b9fcc-452">Typparameter</span><span class="sxs-lookup"><span data-stu-id="b9fcc-452">Type parameters</span></span>

<span data-ttu-id="b9fcc-453">Eine Klassendefinition kann einen Satz an Typparametern angeben, indem eine Liste der Typparameternamen in spitzen Klammern an den Klassennamen angehängt wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-453">A class definition may specify a set of type parameters by following the class name with angle brackets enclosing a list of type parameter names.</span></span> <span data-ttu-id="b9fcc-454">Der Typparameter können die im Text der Klassendeklarationen verwendet werden, um der Member der Klasse zu definieren.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-454">The type parameters can the be used in the body of the class declarations to define the members of the class.</span></span> <span data-ttu-id="b9fcc-455">Im folgenden Beispiel lauten die Typparameter von `Pair` `TFirst` und `TSecond`:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-455">In the following example, the type parameters of `Pair` are `TFirst` and `TSecond`:</span></span>

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
<span data-ttu-id="b9fcc-456">Ein Klassentyp, der zum Akzeptieren von Typparametern deklariert ist, wird einen Typ generische Klasse aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-456">A class type that is declared to take type parameters is called a generic class type.</span></span> <span data-ttu-id="b9fcc-457">Struktur-, Schnittstellen- und Delegattypen können auch generisch sein.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-457">Struct, interface and delegate types can also be generic.</span></span>

<span data-ttu-id="b9fcc-458">Wenn die generische Klasse verwendet wird, müssen für jeden der Typparameter Typargumente angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-458">When the generic class is used, type arguments must be provided for each of the type parameters:</span></span>

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
<span data-ttu-id="b9fcc-459">Ein generischer Typ mit den Typargumente angegeben wurden, wie z. B. `Pair<int,string>
    ` oben einen konstruierten Typ aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-459">A generic type with type arguments provided, like `Pair<int,string>
    ` above, is called a constructed type.</span></span>

### <a name="base-classes"></a><span data-ttu-id="b9fcc-460">Basisklassen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-460">Base classes</span></span>

<span data-ttu-id="b9fcc-461">Eine Klassendeklaration kann eine Basisklasse angeben, indem ein Doppelpunkt und der Name der Basisklasse an den Klassennamen und die Typparameter angehängt wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-461">A class declaration may specify a base class by following the class name and type parameters with a colon and the name of the base class.</span></span> <span data-ttu-id="b9fcc-462">Das Auslassen einer Basisklassenspezifikation ist dasselbe wie eine Ableitung vom Typ `object`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-462">Omitting a base class specification is the same as deriving from type `object`.</span></span> <span data-ttu-id="b9fcc-463">Im folgenden Beispiel ist `Point` die Basisklasse von `Point3D`, und die Basisklasse von `Point` ist `object`:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-463">In the following example, the base class of `Point3D` is `Point`, and the base class of `Point` is `object`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
<span data-ttu-id="b9fcc-464">Eine Klasse erbt die Member der zugehörigen Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-464">A class inherits the members of its base class.</span></span> <span data-ttu-id="b9fcc-465">Vererbung bedeutet, dass eine Klasse implizit alle Member der Basisklasse, mit Ausnahme von der Instanz und statische Konstruktoren und Destruktoren der Basisklasse enthält.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-465">Inheritance means that a class implicitly contains all members of its base class, except for the instance and static constructors, and the destructors of the base class.</span></span> <span data-ttu-id="b9fcc-466">Eine abgeleitete Klasse kann den geerbten Membern neue Member hinzufügen, aber die Definition eines geerbten Members kann nicht entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-466">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span> <span data-ttu-id="b9fcc-467">Im vorherigen Beispiel erbt `Point3D` die Felder `x` und `y` von `Point`, und jede `Point3D`-Instanz enthält drei Felder: `x`, `y` und `z`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-467">In the previous example, `Point3D` inherits the `x` and `y` fields from `Point`, and every `Point3D` instance contains three fields, `x`, `y`, and `z`.</span></span>

<span data-ttu-id="b9fcc-468">Ein Klassentyp kann implizit in einen beliebigen zugehörigen Basisklassentyp konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-468">An implicit conversion exists from a class type to any of its base class types.</span></span> <span data-ttu-id="b9fcc-469">Deshalb kann eine Variable eines Klassentyps auf eine Instanz dieser Klasse oder auf eine Instanz einer beliebigen abgeleiteten Klasse verweisen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-469">Therefore, a variable of a class type can reference an instance of that class or an instance of any derived class.</span></span> <span data-ttu-id="b9fcc-470">Beispielsweise kann in den vorherigen Klassendeklarationen eine Variable vom Typ `Point` entweder auf `Point` oder auf `Point3D` verweisen:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-470">For example, given the previous class declarations, a variable of type `Point` can reference either a `Point` or a `Point3D`:</span></span>

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a><span data-ttu-id="b9fcc-471">Felder</span><span class="sxs-lookup"><span data-stu-id="b9fcc-471">Fields</span></span>

<span data-ttu-id="b9fcc-472">Ein Feld ist eine Variable, die mit einer Klasse oder eine Instanz einer Klasse zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-472">A field is a variable that is associated with a class or with an instance of a class.</span></span>

<span data-ttu-id="b9fcc-473">Ein Feld deklariert, mit der `static` -Modifizierer definiert eine ***statisches Feld***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-473">A field declared with the `static` modifier defines a ***static field***.</span></span> <span data-ttu-id="b9fcc-474">Ein statisches Feld identifiziert genau einen Speicherort.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-474">A static field identifies exactly one storage location.</span></span> <span data-ttu-id="b9fcc-475">Unabhängig davon, wie viele Instanzen einer Klasse erstellt werden, gibt es nur eine Kopie eines statischen Felds.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-475">No matter how many instances of a class are created, there is only ever one copy of a static field.</span></span>

<span data-ttu-id="b9fcc-476">Ein Feld deklariert, ohne die `static` -Modifizierer definiert eine ***Instanzenfeld***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-476">A field declared without the `static` modifier defines an ***instance field***.</span></span> <span data-ttu-id="b9fcc-477">Jede Instanz einer Klasse enthält eine separate Kopie aller Instanzfelder dieser Klasse.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-477">Every instance of a class contains a separate copy of all the instance fields of that class.</span></span>

<span data-ttu-id="b9fcc-478">Im folgenden Beispiel weist jede Instanz der `Color`-Klasse eine separate Kopie der Instanzfelder `r`, `g` und `b` auf, aber es gibt nur eine Kopie der statischen Felder `Black`, `White`, `Red`, `Green` und `Blue`:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-478">In the following example, each instance of the `Color` class has a separate copy of the `r`, `g`, and `b` instance fields, but there is only one copy of the `Black`, `White`, `Red`, `Green`, and `Blue` static fields:</span></span>

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
<span data-ttu-id="b9fcc-479">Wie im vorherigen Beispiel gezeigt, können ***schreibgeschützte Felder*** mit einem `readonly`-Modifizierer deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-479">As shown in the previous example, ***read-only fields*** may be declared with a `readonly` modifier.</span></span> <span data-ttu-id="b9fcc-480">Zuweisung zu einem `readonly` Feld kann nur als Teil einer Deklaration oder in einem Konstruktor derselben Klasse auftreten.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-480">Assignment to a `readonly` field can only occur as part of the field's declaration or in a constructor in the same class.</span></span>

### <a name="methods"></a><span data-ttu-id="b9fcc-481">Methoden</span><span class="sxs-lookup"><span data-stu-id="b9fcc-481">Methods</span></span>

<span data-ttu-id="b9fcc-482">Eine ***Methode*** ist ein Member, das eine Berechnung oder eine Aktion implementiert, die durch ein Objekt oder eine Klasse durchgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-482">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="b9fcc-483">Auf ***statische Methoden*** wird über die Klasse zugegriffen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-483">***Static methods*** are accessed through the class.</span></span> <span data-ttu-id="b9fcc-484">Auf ***Instanzmethoden*** wird über Instanzen der Klasse zugegriffen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-484">***Instance methods*** are accessed through instances of the class.</span></span>

<span data-ttu-id="b9fcc-485">Methoden verfügen über eine (möglicherweise leere) Liste von ***Parameter***, die darstellen, Werte oder Variablenverweise an die Methode übergeben, und ein ***Rückgabetyp***, der angibt, dass der Typ des Werts berechnet und zurückgegeben, indem die Methode.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-485">Methods have a (possibly empty) list of ***parameters***, which represent values or variable references passed to the method, and a ***return type***, which specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="b9fcc-486">Der Rückgabetyp einer Methode ist `void` , wenn sie keinen Wert zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-486">A method's return type is `void` if it does not return a value.</span></span>

<span data-ttu-id="b9fcc-487">Ebenso wie Typen können Methoden einen Satz an Typparametern aufweisen, für den beim Aufruf der Methode Typargumente angegeben werden müssen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-487">Like types, methods may also have a set of type parameters, for which type arguments must be specified when the method is called.</span></span> <span data-ttu-id="b9fcc-488">Im Gegensatz zu Typen können die Typargumente häufig aus den Argumenten eines Methodenaufrufs abgeleitet werden und müssen nicht explizit angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-488">Unlike types, the type arguments can often be inferred from the arguments of a method call and need not be explicitly given.</span></span>

<span data-ttu-id="b9fcc-489">Die ***Signatur*** einer Methode muss innerhalb der Klasse eindeutig sein, in der die Methode deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-489">The ***signature*** of a method must be unique in the class in which the method is declared.</span></span> <span data-ttu-id="b9fcc-490">Die Signatur einer Methode besteht aus dem Namen der Methode, der Anzahl von Typparametern und der Anzahl, den Modifizierern und den Typen der zugehörigen Parameter.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-490">The signature of a method consists of the name of the method, the number of type parameters and the number, modifiers, and types of its parameters.</span></span> <span data-ttu-id="b9fcc-491">Die Signatur einer Methode umfasst nicht den Rückgabetyp.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-491">The signature of a method does not include the return type.</span></span>

#### <a name="parameters"></a><span data-ttu-id="b9fcc-492">Parameter</span><span class="sxs-lookup"><span data-stu-id="b9fcc-492">Parameters</span></span>

<span data-ttu-id="b9fcc-493">Parameter werden dazu verwendet, Werte oder Variablenverweise an Methoden zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-493">Parameters are used to pass values or variable references to methods.</span></span> <span data-ttu-id="b9fcc-494">Die Parameter einer Methode erhalten ihre tatsächlichen Werte über ***Argumente***, die angegeben werden, wenn die Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-494">The parameters of a method get their actual values from the ***arguments*** that are specified when the method is invoked.</span></span> <span data-ttu-id="b9fcc-495">Es gibt vier Arten von Parametern: Wertparameter, Verweisparameter, Ausgabeparameter und Parameterarrays.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-495">There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.</span></span>

<span data-ttu-id="b9fcc-496">Ein ***Wertparameter*** wird zum Übergeben von Eingabeparametern verwendet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-496">A ***value parameter*** is used for input parameter passing.</span></span> <span data-ttu-id="b9fcc-497">Ein Wertparameter entspricht einer lokalen Variablen, die ihren Anfangswert von dem Argument erhält, das für den Parameter übergeben wurde.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-497">A value parameter corresponds to a local variable that gets its initial value from the argument that was passed for the parameter.</span></span> <span data-ttu-id="b9fcc-498">Änderungen an einem Wertparameter wirken sich nicht auf das Argument aus, das für den Parameter übergeben wurde.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-498">Modifications to a value parameter do not affect the argument that was passed for the parameter.</span></span>

<span data-ttu-id="b9fcc-499">Wertparameter können optional sein, indem ein Standardwert festgelegt wird, damit die zugehörigen Argumente weggelassen werden können.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-499">Value parameters can be optional, by specifying a default value so that corresponding arguments can be omitted.</span></span>

<span data-ttu-id="b9fcc-500">Ein ***Verweisparameter*** wird sowohl für die Übergabe von Eingabe- als auch Ausgabeparametern verwendet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-500">A ***reference parameter*** is used for both input and output parameter passing.</span></span> <span data-ttu-id="b9fcc-501">Das für einen Verweisparameter übergebene Argument muss eine Variable sein, und während der Ausführung der Methode repräsentiert der Verweisparameter denselben Speicherort wie die Argumentvariable.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-501">The argument passed for a reference parameter must be a variable, and during execution of the method, the reference parameter represents the same storage location as the argument variable.</span></span> <span data-ttu-id="b9fcc-502">Ein Verweisparameter wird mit dem `ref`-Modifizierer deklariert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-502">A reference parameter is declared with the `ref` modifier.</span></span> <span data-ttu-id="b9fcc-503">Das folgende Beispiel veranschaulicht die Verwendung des `ref`-Parameters.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-503">The following example shows the use of `ref` parameters.</span></span>

```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
<span data-ttu-id="b9fcc-504">Ein ***Ausgabeparameter*** wird zum Übergeben von Ausgabeparametern verwendet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-504">An ***output parameter*** is used for output parameter passing.</span></span> <span data-ttu-id="b9fcc-505">Ein Ausgabeparameter ähnelt einem Verweisparameter, mit dem Unterschied, dass der Anfangswert des vom Aufrufer bereitgestellten Arguments nicht von Bedeutung ist.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-505">An output parameter is similar to a reference parameter except that the initial value of the caller-provided argument is unimportant.</span></span> <span data-ttu-id="b9fcc-506">Ein Ausgabeparameter wird mit dem `out`-Modifizierer deklariert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-506">An output parameter is declared with the `out` modifier.</span></span> <span data-ttu-id="b9fcc-507">Das folgende Beispiel veranschaulicht die Verwendung des `out`-Parameters.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-507">The following example shows the use of `out` parameters.</span></span>

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
<span data-ttu-id="b9fcc-508">Ein ***Parameterarray*** ermöglicht es, eine variable Anzahl von Argumenten an eine Methode zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-508">A ***parameter array*** permits a variable number of arguments to be passed to a method.</span></span> <span data-ttu-id="b9fcc-509">Ein Parameterarray wird mit dem `params`-Modifizierer deklariert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-509">A parameter array is declared with the `params` modifier.</span></span> <span data-ttu-id="b9fcc-510">Nur der letzte Parameter einer Methode kann ein Parameterarray sein, und es muss sich um ein eindimensionales Parameterarray handeln.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-510">Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type.</span></span> <span data-ttu-id="b9fcc-511">Die `Write` und `WriteLine` Methoden der `System.Console` -Klasse sind gute Beispiele für die Verwendung eines Parameterarrays.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-511">The `Write` and `WriteLine` methods of the `System.Console` class are good examples of parameter array usage.</span></span> <span data-ttu-id="b9fcc-512">Sie werden folgendermaßen deklariert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-512">They are declared as follows.</span></span>

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
<span data-ttu-id="b9fcc-513">Innerhalb einer Methode mit einem Parameterarray verhält sich das Parameterarray wie ein regulärer Parameter des Arraytyps.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-513">Within a method that uses a parameter array, the parameter array behaves exactly like a regular parameter of an array type.</span></span> <span data-ttu-id="b9fcc-514">Beim Aufruf einer Methode mit einem Parameterarray ist es jedoch möglich, entweder ein einzelnes Argument des Parameterarraytyps oder eine beliebige Anzahl von Argumenten des Elementtyps des Parameterarrays zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-514">However, in an invocation of a method with a parameter array, it is possible to pass either a single argument of the parameter array type or any number of arguments of the element type of the parameter array.</span></span> <span data-ttu-id="b9fcc-515">Im letzteren Fall wird automatisch eine Arrayinstanz erstellt und mit den vorgegebenen Argumenten initialisiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-515">In the latter case, an array instance is automatically created and initialized with the given arguments.</span></span> <span data-ttu-id="b9fcc-516">Dieses Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-516">This example</span></span>

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
<span data-ttu-id="b9fcc-517">...entspricht dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-517">is equivalent to writing the following.</span></span>

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a><span data-ttu-id="b9fcc-518">Methodenkörper und lokale Variablen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-518">Method body and local variables</span></span>

<span data-ttu-id="b9fcc-519">Der Methodenkörper gibt an, die Anweisungen ausgeführt werden, wenn die Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-519">A method's body specifies the statements to execute when the method is invoked.</span></span>

<span data-ttu-id="b9fcc-520">Ein Methodenkörper kann Variablen deklarieren, die für den Aufruf der Methode spezifisch sind.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-520">A method body can declare variables that are specific to the invocation of the method.</span></span> <span data-ttu-id="b9fcc-521">Diese Variable werden ***lokale Variablen*** genannt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-521">Such variables are called ***local variables***.</span></span> <span data-ttu-id="b9fcc-522">Die Deklaration einer lokalen Variable gibt einen Typnamen, einen Variablennamen und eventuell einen Anfangswert an.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-522">A local variable declaration specifies a type name, a variable name, and possibly an initial value.</span></span> <span data-ttu-id="b9fcc-523">Im folgenden Beispiel wird eine lokale Variable `i` mit einem Anfangswert von 0 und einer lokalen Variablen `j` ohne Anfangswert deklariert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-523">The following example declares a local variable `i` with an initial value of zero and a local variable `j` with no initial value.</span></span>

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
<span data-ttu-id="b9fcc-524">In C# muss eine lokale Variable ***definitiv zugewiesen*** sein, bevor ihr Wert abgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-524">C# requires a local variable to be ***definitely assigned*** before its value can be obtained.</span></span> <span data-ttu-id="b9fcc-525">Wenn beispielsweise die vorherige Deklaration von `i` keinen Anfangswert enthielte, würde der Compiler bei der nachfolgenden Verwendung von `i` einen Fehler melden, weil `i` zu diesen Zeitpunkten im Programm nicht definitiv zugewiesen wäre.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-525">For example, if the declaration of the previous `i` did not include an initial value, the compiler would report an error for the subsequent usages of `i` because `i` would not be definitely assigned at those points in the program.</span></span>

<span data-ttu-id="b9fcc-526">Eine Methode kann `return`-Anweisungen verwenden, um die Steuerung an den zugehörigen Aufrufer zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-526">A method can use `return` statements to return control to its caller.</span></span> <span data-ttu-id="b9fcc-527">In einer Methode, die `void` zurückgibt, können `return`-Anweisungen keinen Ausdruck angeben.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-527">In a method returning `void`, `return` statements cannot specify an expression.</span></span> <span data-ttu-id="b9fcc-528">In einer Methode zurückgeben von nicht -`void`, `return` -Anweisungen müssen einen Ausdruck, der den Rückgabewert berechnet enthalten.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-528">In a method returning non-`void`, `return` statements must include an expression that computes the return value.</span></span>

#### <a name="static-and-instance-methods"></a><span data-ttu-id="b9fcc-529">Statische Methoden und Instanzmethoden</span><span class="sxs-lookup"><span data-stu-id="b9fcc-529">Static and instance methods</span></span>

<span data-ttu-id="b9fcc-530">Eine Methode deklariert, mit einem `static` Modifizierer ist ein ***statische Methode***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-530">A method declared with a `static` modifier is a ***static method***.</span></span> <span data-ttu-id="b9fcc-531">Eine statische Methode führt keine Vorgänge für eine spezifische Instanz aus und kann nur direkt auf statische Member zugreifen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-531">A static method does not operate on a specific instance and can only directly access static members.</span></span>

<span data-ttu-id="b9fcc-532">Eine Methode deklariert werden, ohne eine `static` Modifizierer ist ein ***Instanzmethode***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-532">A method declared without a `static` modifier is an ***instance method***.</span></span> <span data-ttu-id="b9fcc-533">Eine Instanzmethode führt Vorgänge für eine spezifische Instanz aus und kann sowohl auf statische Member als auch auf Instanzmember zugreifen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-533">An instance method operates on a specific instance and can access both static and instance members.</span></span> <span data-ttu-id="b9fcc-534">Auf die Instanz, für die eine Instanzmethode aufgerufen wurde, kann explizit als `this` zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-534">The instance on which an instance method was invoked can be explicitly accessed as `this`.</span></span> <span data-ttu-id="b9fcc-535">Es ist ein Fehler, in einer statischen Methode auf `this` zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-535">It is an error to refer to `this` in a static method.</span></span>

<span data-ttu-id="b9fcc-536">Die folgende `Entity`-Klasse umfasst sowohl statische Member als auch Instanzmember.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-536">The following `Entity` class has both static and instance members.</span></span>

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
<span data-ttu-id="b9fcc-537">Jede `Entity`-Instanz enthält eine Seriennummer (und vermutlich weitere Informationen, die hier nicht angezeigt werden).</span><span class="sxs-lookup"><span data-stu-id="b9fcc-537">Each `Entity` instance contains a serial number (and presumably some other information that is not shown here).</span></span> <span data-ttu-id="b9fcc-538">Der `Entity`-Konstruktor (der einer Instanzmethode ähnelt) initialisiert die neue Instanz mit der nächsten verfügbaren Seriennummer.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-538">The `Entity` constructor (which is like an instance method) initializes the new instance with the next available serial number.</span></span> <span data-ttu-id="b9fcc-539">Da der Konstruktor ein Instanzmember ist, kann er sowohl auf das `serialNo`-Instanzfeld als auch auf das statische `nextSerialNo`-Feld zugreifen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-539">Because the constructor is an instance member, it is permitted to access both the `serialNo` instance field and the `nextSerialNo` static field.</span></span>

<span data-ttu-id="b9fcc-540">Die statischen Methoden `GetNextSerialNo` und `SetNextSerialNo` können auf das statische Feld `nextSerialNo` zugreifen, aber es wäre ein Fehler, über diese Methoden direkt auf das Instanzfeld `serialNo` zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-540">The `GetNextSerialNo` and `SetNextSerialNo` static methods can access the `nextSerialNo` static field, but it would be an error for them to directly access the `serialNo` instance field.</span></span>

<span data-ttu-id="b9fcc-541">Das folgende Beispiel zeigt die Verwendung der `Entity` Klasse.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-541">The following example shows the use of the `Entity` class.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
<span data-ttu-id="b9fcc-542">Beachten Sie, dass die statischen Methoden `SetNextSerialNo` und `GetNextSerialNo` für die Klasse aufgerufen werden, während die `GetSerialNo`-Instanzmethode für Instanzen der Klasse aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-542">Note that the `SetNextSerialNo` and `GetNextSerialNo` static methods are invoked on the class whereas the `GetSerialNo` instance method is invoked on instances of the class.</span></span>

#### <a name="virtual-override-and-abstract-methods"></a><span data-ttu-id="b9fcc-543">Virtuelle, überschriebene und abstrakte Methoden</span><span class="sxs-lookup"><span data-stu-id="b9fcc-543">Virtual, override, and abstract methods</span></span>

<span data-ttu-id="b9fcc-544">Wenn die Deklaration einer Instanzmethode einen `virtual`-Modifizierer enthält, wird die Methode als ***virtuelle Methode*** bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-544">When an instance method declaration includes a `virtual` modifier, the method is said to be a ***virtual method***.</span></span> <span data-ttu-id="b9fcc-545">Wenn kein `virtual` Modifizierer vorhanden ist, wird die Methode gilt eine ***nicht virtuellen Methode***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-545">When no `virtual` modifier is present, the method is said to be a ***non-virtual method***.</span></span>

<span data-ttu-id="b9fcc-546">Beim Aufruf einer virtuellen Methode bestimmt der ***Laufzeittyp*** der Instanz, für die der Aufruf erfolgt, die tatsächlich aufzurufende Methodenimplementierung.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-546">When a virtual method is invoked, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="b9fcc-547">Beim Aufruf einer nicht virtuellen Methode ist der ***Kompilierzeittyp*** der bestimmende Faktor.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-547">In a nonvirtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span>

<span data-ttu-id="b9fcc-548">Eine virtuelle Methode kann in einer abgeleiteten Klasse ***überschrieben*** werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-548">A virtual method can be ***overridden*** in a derived class.</span></span> <span data-ttu-id="b9fcc-549">Wenn eine Instanzmethodendeklaration enthält ein `override` Modifizierer, die Methode überschreibt eine geerbte virtuelle Methode mit der gleichen Signatur.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-549">When an instance method declaration includes an `override` modifier, the method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="b9fcc-550">Während eine Deklaration einer virtuellen Methode eine neue Methode einführt, spezialisiert eine Deklaration einer überschriebenen Methode eine vorhandene geerbte virtuelle Methode, indem eine neue Implementierung dieser Methode bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-550">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="b9fcc-551">Ein ***abstrakte*** Methode ist eine virtuelle Methode ohne Implementierung.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-551">An ***abstract*** method is a virtual method with no implementation.</span></span> <span data-ttu-id="b9fcc-552">Eine abstrakte Methode ist deklariert, mit der `abstract` Modifizierer und darf nur in einer Klasse, die auch deklariert wird `abstract`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-552">An abstract method is declared with the `abstract` modifier and is permitted only in a class that is also declared `abstract`.</span></span> <span data-ttu-id="b9fcc-553">Eine abstrakte Methode muss in jeder nicht abstrakten abgeleiteten Klasse überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-553">An abstract method must be overridden in every non-abstract derived class.</span></span>

<span data-ttu-id="b9fcc-554">Im folgenden Beispiel wird die abstrakte Klasse `Expression` deklariert, die einen Ausdrucksbaumstrukturknoten sowie drei abgeleitete Klassen repräsentiert: `Constant`, `VariableReference` und `Operation`. Diese implementieren Ausdrucksbaumstrukturknoten für Konstanten, variable Verweise und arithmetische Operationen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-554">The following example declares an abstract class, `Expression`, which represents an expression tree node, and three derived classes, `Constant`, `VariableReference`, and `Operation`, which implement expression tree nodes for constants, variable references, and arithmetic operations.</span></span> <span data-ttu-id="b9fcc-555">(Dieser Vorgang ähnelt, aber nicht zu verwechseln mit die ausdrucksbaumstrukturtypen, das in eingeführte [ausdrucksbaumstrukturtypen](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="b9fcc-555">(This is similar to, but not to be confused with the expression tree types introduced in [Expression tree types](types.md#expression-tree-types)).</span></span>

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
<span data-ttu-id="b9fcc-556">Die vorherigen vier Klassen können zum Modellieren arithmetischer Ausdrücke verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-556">The previous four classes can be used to model arithmetic expressions.</span></span> <span data-ttu-id="b9fcc-557">Beispielsweise kann mithilfe von Instanzen dieser Klassen der Ausdruck `x + 3` folgendermaßen dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-557">For example, using instances of these classes, the expression `x + 3` can be represented as follows.</span></span>

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
<span data-ttu-id="b9fcc-558">Die `Evaluate`-Methode einer `Expression`-Instanz wird aufgerufen, um den vorgegebenen Ausdruck auszuwerten und einen `double`-Wert zu generieren.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-558">The `Evaluate` method of an `Expression` instance is invoked to evaluate the given expression and produce a `double` value.</span></span> <span data-ttu-id="b9fcc-559">Die Methode, die als Argument akzeptiert eine `Hashtable` , Variablennamen (als Schlüssel der Einträge) und Werte (als Werte der Einträge) enthält.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-559">The method takes as an argument a `Hashtable` that contains variable names (as keys of the entries) and values (as values of the entries).</span></span> <span data-ttu-id="b9fcc-560">Die `Evaluate` Methode ist eine virtuelle abstrakte Methode, was bedeutet, dass nicht abstrakte abgeleitete Klassen überschreiben müssen, um eine tatsächliche Implementierung bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-560">The `Evaluate` method is a virtual abstract method, meaning that non-abstract derived classes must override it to provide an actual implementation.</span></span>

<span data-ttu-id="b9fcc-561">Eine Implementierung von `Constant` für `Evaluate` gibt lediglich die gespeicherte Konstante zurück.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-561">A `Constant`'s implementation of `Evaluate` simply returns the stored constant.</span></span> <span data-ttu-id="b9fcc-562">Ein `VariableReference`die Implementierung sieht nach dem Variablennamen in der Hashtabelle und gibt den Ergebniswert zurück.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-562">A `VariableReference`'s implementation looks up the variable name in the hashtable and returns the resulting value.</span></span> <span data-ttu-id="b9fcc-563">Eine Implementierung von `Operation` wertet zunächst (durch einen rekursiven Aufruf der zugehörigen `Evaluate`-Methoden) den linken und rechten Operanden aus und führt dann die vorgegebene arithmetische Operation aus.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-563">An `Operation`'s implementation first evaluates the left and right operands (by recursively invoking their `Evaluate` methods) and then performs the given arithmetic operation.</span></span>

<span data-ttu-id="b9fcc-564">Das folgende Programm verwendet die `Expression`-Klassen zum Auswerten des Ausdrucks `x * (y + 2)` für verschiedene Werte von `x` und `y`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-564">The following program uses the `Expression` classes to evaluate the expression `x * (y + 2)` for different values of `x` and `y`.</span></span>

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### <a name="method-overloading"></a><span data-ttu-id="b9fcc-565">Methodenüberladung</span><span class="sxs-lookup"><span data-stu-id="b9fcc-565">Method overloading</span></span>

<span data-ttu-id="b9fcc-566">Das ***Überladen*** von Methoden macht es möglich, dass mehrere Methoden in derselben Klasse denselben Namen verwenden, solange sie eindeutige Signaturen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-566">Method ***overloading*** permits multiple methods in the same class to have the same name as long as they have unique signatures.</span></span> <span data-ttu-id="b9fcc-567">Beim Kompilieren des Aufrufs einer überladenen Methode verwendet der Compiler die ***Überladungsauflösung***, um die spezifische Methode zu ermitteln, die aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-567">When compiling an invocation of an overloaded method, the compiler uses ***overload resolution*** to determine the specific method to invoke.</span></span> <span data-ttu-id="b9fcc-568">Mithilfe der Überladungsauflösung wird die Methode ermittelt, die den Argumenten am besten entspricht, bzw. es wird ein Fehler ausgegeben, wenn keine passende Methode gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-568">Overload resolution finds the one method that best matches the arguments or reports an error if no single best match can be found.</span></span> <span data-ttu-id="b9fcc-569">Das folgende Beispiel zeigt die Verwendung der Überladungsauflösung.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-569">The following example shows overload resolution in effect.</span></span> <span data-ttu-id="b9fcc-570">Der Kommentar für jeden Aufruf in der `Main`-Methode zeigt, welche Methode tatsächlich aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-570">The comment for each invocation in the `Main` method shows which method is actually invoked.</span></span>

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
<span data-ttu-id="b9fcc-571">Wie im Beispiel gezeigt, kann eine bestimmte Methode immer ausgewählt werden, indem die Argumente explizit in die passenden Parametertypen konvertiert und/oder explizit Typargumente angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-571">As shown by the example, a particular method can always be selected by explicitly casting the arguments to the exact parameter types and/or explicitly supplying type arguments.</span></span>

### <a name="other-function-members"></a><span data-ttu-id="b9fcc-572">Andere Funktionsmember</span><span class="sxs-lookup"><span data-stu-id="b9fcc-572">Other function members</span></span>

<span data-ttu-id="b9fcc-573">Member, die ausführbaren Code enthalten, werden als ***Funktionsmember*** einer Klasse bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-573">Members that contain executable code are collectively known as the ***function members*** of a class.</span></span> <span data-ttu-id="b9fcc-574">In den vorangegangenen Abschnitten wurden die Methoden beschrieben, die wichtigste Form der Funktionsmember.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-574">The preceding section describes methods, which are the primary kind of function members.</span></span> <span data-ttu-id="b9fcc-575">In diesem Abschnitt wird beschrieben, die andere Arten von Funktionsmembern von c# unterstützt: Konstruktoren, Eigenschaften, Indexer, Ereignisse, Operatoren und Destruktoren.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-575">This section describes the other kinds of function members supported by C#: constructors, properties, indexers, events, operators, and destructors.</span></span>

<span data-ttu-id="b9fcc-576">Der folgende Code zeigt eine generische Klasse namens `List<T>`, die eine wachsende Liste von Objekten implementiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-576">The following code shows a generic class called `List<T>`, which implements a growable list of objects.</span></span> <span data-ttu-id="b9fcc-577">Die Klasse enthält verschiedene Beispiele der gängigsten Arten von Funktionsmembern.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-577">The class contains several examples of the most common kinds of function members.</span></span>


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### <a name="constructors"></a><span data-ttu-id="b9fcc-578">Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="b9fcc-578">Constructors</span></span>

<span data-ttu-id="b9fcc-579">C# unterstützt sowohl Instanzkonstruktoren als auch statische Konstruktoren.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-579">C# supports both instance and static constructors.</span></span> <span data-ttu-id="b9fcc-580">Ein ***Instanzkonstruktor*** ist ein Member, der die erforderlichen Aktionen zum Initialisieren einer Instanz einer Klasse implementiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-580">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="b9fcc-581">Ein ***statischer Konstruktor*** ist ein Member, der die zum Initialisieren einer Klasse erforderlichen Aktionen implementiert, um die Klasse beim ersten Laden selbst zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-581">A ***static constructor*** is a member that implements the actions required to initialize a class itself when it is first loaded.</span></span>

<span data-ttu-id="b9fcc-582">Ein Konstruktor wird wie eine Methode ohne Rückgabetyp und mit demselben Namen wie die enthaltende Klasse deklariert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-582">A constructor is declared like a method with no return type and the same name as the containing class.</span></span> <span data-ttu-id="b9fcc-583">Wenn eine Konstruktordeklaration verwendet enthält eine `static` Modifizierer verwenden, wird einen statischen Konstruktor deklariert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-583">If a constructor declaration includes a `static` modifier, it declares a static constructor.</span></span> <span data-ttu-id="b9fcc-584">Andernfalls wird ein Instanzkonstruktor deklariert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-584">Otherwise, it declares an instance constructor.</span></span>

<span data-ttu-id="b9fcc-585">Instanzkonstruktoren können überladen werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-585">Instance constructors can be overloaded.</span></span> <span data-ttu-id="b9fcc-586">Beispielsweise deklariert die `List<T>
`-Klasse zwei Instanzkonstruktoren, einen ohne Parameter und einen weiteren mit einem `int`-Parameter.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-586">For example, the `List<T>
` class declares two instance constructors, one with no parameters and one that takes an `int` parameter.</span></span> <span data-ttu-id="b9fcc-587">Instanzkonstruktoren werden über den `new`-Operator aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-587">Instance constructors are invoked using the `new` operator.</span></span> <span data-ttu-id="b9fcc-588">Die folgenden Anweisungen weisen zwei `List<string>
` Instanzen, indem Sie die Konstruktoren der `List` Klasse.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-588">The following statements allocate two `List<string>
` instances using each of the constructors of the `List` class.</span></span>

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
<span data-ttu-id="b9fcc-589">Im Gegensatz zu anderen Members werden Instanzkonstruktoren nicht geerbt, und eine Klasse weist keine anderen Instanzkonstruktoren auf als diejenigen, die tatsächlich in der Klasse deklariert wurden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-589">Unlike other members, instance constructors are not inherited, and a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="b9fcc-590">Wenn kein Instanzkonstruktor für eine Klasse angegeben ist, wird automatisch ein leerer Instanzkonstruktor ohne Parameter bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-590">If no instance constructor is supplied for a class, then an empty one with no parameters is automatically provided.</span></span>

#### <a name="properties"></a><span data-ttu-id="b9fcc-591">Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="b9fcc-591">Properties</span></span>

<span data-ttu-id="b9fcc-592">***Eigenschaften*** sind eine natürliche Erweiterung der Felder.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-592">***Properties*** are a natural extension of fields.</span></span> <span data-ttu-id="b9fcc-593">Beide sind benannte Member mit zugeordneten Typen, und für den Zugriff auf Felder und Eigenschaften wird dieselbe Syntax verwendet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-593">Both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="b9fcc-594">Im Gegensatz zu Feldern bezeichnen Eigenschaften jedoch keine Speicherorte.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-594">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="b9fcc-595">Stattdessen verfügen Eigenschaften über ***Accessors*** zum Angeben der Anweisungen, die beim Lesen oder Schreiben ihrer Werte ausgeführt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-595">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span>

<span data-ttu-id="b9fcc-596">Eine Eigenschaft wird wie ein Feld deklariert, außer dass die Deklaration mit endet eine `get` Accessor und/oder einen `set` Accessor, die zwischen den Trennzeichen `{` und `}` statt einem Semikolon endet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-596">A property is declared like a field, except that the declaration ends with a `get` accessor and/or a `set` accessor written between the delimiters `{` and `}` instead of ending in a semicolon.</span></span> <span data-ttu-id="b9fcc-597">Eine Eigenschaft, die beides aufweist eine `get` Accessor und einen `set` -Accessor ist eine ***Lese-/ Schreibzugriff-Eigenschaft***, eine Eigenschaft, die nur eine `get` -Accessor ist eine ***schreibgeschützte Eigenschaft***, und ein Eigenschaft, die nur eine `set` -Accessor ist eine ***lesegeschützte Eigenschaft***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-597">A property that has both a `get` accessor and a `set` accessor is a ***read-write property***, a property that has only a `get` accessor is a ***read-only property***, and a property that has only a `set` accessor is a ***write-only property***.</span></span>

<span data-ttu-id="b9fcc-598">Ein `get` -Accessor entspricht einer Methode ohne Parameter mit einem Rückgabewert des Eigenschaftstyps.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-598">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="b9fcc-599">Eine Eigenschaft in einem Ausdruck verwiesen wird, wird als Ziel einer Zuweisung, außer die `get` -Accessor der Eigenschaft wird aufgerufen, um den Wert der Eigenschaft zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-599">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property.</span></span>

<span data-ttu-id="b9fcc-600">Ein `set` Accessor entspricht einer Methode mit einem einzigen Parameter, die mit dem Namen `value` und keinen Rückgabewert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-600">A `set` accessor corresponds to a method with a single parameter named `value` and no return type.</span></span> <span data-ttu-id="b9fcc-601">Wenn eine Eigenschaft verwiesen wird, als das Ziel einer Zuweisung oder als Operand `++` oder `--`, `set` -Accessor wird aufgerufen, mit der ein Argument, das den neuen Wert bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-601">When a property is referenced as the target of an assignment or as the operand of `++` or `--`, the `set` accessor is invoked with an argument that provides the new value.</span></span>

<span data-ttu-id="b9fcc-602">Die `List<T>
`-Klasse deklariert die beiden Eigenschaften „`Count`“ und „`Capacity`“, von denen die eine schreibgeschützt ist und die andere Lese- und Schreibzugriff besitzt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-602">The `List<T>
` class declares two properties, `Count` and `Capacity`, which are read-only and read-write, respectively.</span></span> <span data-ttu-id="b9fcc-603">Es folgt ein Beispiel zur Verwendung dieser Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-603">The following is an example of use of these properties.</span></span>

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
<span data-ttu-id="b9fcc-604">Ähnlich wie bei Feldern und Methoden unterstützt C# sowohl Instanzeigenschaften als auch statische Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-604">Similar to fields and methods, C# supports both instance properties and static properties.</span></span> <span data-ttu-id="b9fcc-605">Statische Eigenschaften werden deklariert, mit der `static` Modifizierer und Instanzeigenschaften werden ohne ihn deklariert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-605">Static properties are declared with the `static` modifier, and instance properties are declared without it.</span></span>

<span data-ttu-id="b9fcc-606">Die Accessors einer Eigenschaft können virtuell sein.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-606">The accessor(s) of a property can be virtual.</span></span> <span data-ttu-id="b9fcc-607">Wenn eine Eigenschaftendeklaration einen `virtual`-, `abstract`- oder `override`-Modifizierer enthält, wird dieser auf den Accessor der Eigenschaft angewendet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-607">When a property declaration includes a `virtual`, `abstract`, or `override` modifier, it applies to the accessor(s) of the property.</span></span>

#### <a name="indexers"></a><span data-ttu-id="b9fcc-608">Indexer</span><span class="sxs-lookup"><span data-stu-id="b9fcc-608">Indexers</span></span>

<span data-ttu-id="b9fcc-609">Ein ***Indexer*** ist ein Member, mit dem Objekte wie ein Array indiziert werden können.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-609">An ***indexer*** is a member that enables objects to be indexed in the same way as an array.</span></span> <span data-ttu-id="b9fcc-610">Ein Indexer wird wie eine Eigenschaft deklariert, mit dem Unterschied, dass der Name des Members `this` gefolgt von einer Parameterliste, die zwischen den Trennzeichen `[` und `]`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-610">An indexer is declared like a property except that the name of the member is `this` followed by a parameter list written between the delimiters `[` and `]`.</span></span> <span data-ttu-id="b9fcc-611">Die Parameter stehen im Accessor des Indexers zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-611">The parameters are available in the accessor(s) of the indexer.</span></span> <span data-ttu-id="b9fcc-612">Ähnlich wie Eigenschaften können Indexer Lese-/Schreibzugriff besitzen, schreibgeschützt und lesegeschützt sein und virtuelle Accessors verwenden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-612">Similar to properties, indexers can be read-write, read-only, and write-only, and the accessor(s) of an indexer can be virtual.</span></span>

<span data-ttu-id="b9fcc-613">Die `List`-Klasse deklariert einen einzigen Indexer mit Lese-/Schreibzugriff, der einen `int`-Parameter akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-613">The `List` class declares a single read-write indexer that takes an `int` parameter.</span></span> <span data-ttu-id="b9fcc-614">Der Indexer ermöglicht es, Instanzen von `List` mit `int`-Werten zu indizieren.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-614">The indexer makes it possible to index `List` instances with `int` values.</span></span> <span data-ttu-id="b9fcc-615">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-615">For example</span></span>

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
<span data-ttu-id="b9fcc-616">Indexer können überladen werden, d.h. eine Klasse kann mehrere Indexer deklarieren, solange sich die Anzahl oder Typen ihrer Parameter unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-616">Indexers can be overloaded, meaning that a class can declare multiple indexers as long as the number or types of their parameters differ.</span></span>

#### <a name="events"></a><span data-ttu-id="b9fcc-617">Ereignisse</span><span class="sxs-lookup"><span data-stu-id="b9fcc-617">Events</span></span>

<span data-ttu-id="b9fcc-618">Ein ***Ereignis*** ist ein Member, der es einer Klasse oder einem Objekt ermöglicht, Benachrichtigungen bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-618">An ***event*** is a member that enables a class or object to provide notifications.</span></span> <span data-ttu-id="b9fcc-619">Ein Ereignis wird wie ein Feld deklariert, mit dem Unterschied, dass die Deklaration enthält eine `event` -Schlüsselwort und der Typ müssen ein Delegattyp sein.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-619">An event is declared like a field except that the declaration includes an `event` keyword and the type must be a delegate type.</span></span>

<span data-ttu-id="b9fcc-620">Innerhalb einer Klasse, die einen Ereignismember deklariert, verhält sich das Ereignis wie ein Feld des Delegattyps (vorausgesetzt, das Ereignis ist nicht abstrakt und deklariert keine Accessors).</span><span class="sxs-lookup"><span data-stu-id="b9fcc-620">Within a class that declares an event member, the event behaves just like a field of a delegate type (provided the event is not abstract and does not declare accessors).</span></span> <span data-ttu-id="b9fcc-621">Das Feld speichert einen Verweis auf einen Delegaten, der die Ereignishandler repräsentiert, die dem Ereignis hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-621">The field stores a reference to a delegate that represents the event handlers that have been added to the event.</span></span> <span data-ttu-id="b9fcc-622">Wenn keine Handles für ein Ereignis vorhanden sind, wird das Feld `null`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-622">If no event handles are present, the field is `null`.</span></span>

<span data-ttu-id="b9fcc-623">Die `List<T>
`-Klasse deklariert einen einzigen Ereignismember namens `Changed`, der angibt, dass der Liste ein neues Element hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-623">The `List<T>
` class declares a single event member called `Changed`, which indicates that a new item has been added to the list.</span></span> <span data-ttu-id="b9fcc-624">Die `Changed` Ereignis wird ausgelöst, durch die `OnChanged` virtuelle Methode, die zunächst prüft, ob das Ereignis `null` (d. h., dass keine Handler vorhanden sind).</span><span class="sxs-lookup"><span data-stu-id="b9fcc-624">The `Changed` event is raised by the `OnChanged` virtual method, which first checks whether the event is `null` (meaning that no handlers are present).</span></span> <span data-ttu-id="b9fcc-625">Das Auslösen eines Ereignisses entspricht exakt dem Aufrufen des Delegaten, der durch das Ereignis repräsentiert wird, es gibt deshalb keine besonderen Sprachkonstrukte zum Auslösen von Ereignissen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-625">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span>

<span data-ttu-id="b9fcc-626">Clients reagieren über ***Ereignishandler*** auf Ereignisse.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-626">Clients react to events through ***event handlers***.</span></span> <span data-ttu-id="b9fcc-627">Ereignishandler werden unter Verwendung des `+=`-Operators angefügt und mit dem `-=`-Operator entfernt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-627">Event handlers are attached using the `+=` operator and removed using the `-=` operator.</span></span> <span data-ttu-id="b9fcc-628">Im folgenden Beispiel wird dem `Changed`-Ereignis von `List<string>
` ein Ereignishandler hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-628">The following example attaches an event handler to the `Changed` event of a `List<string>
`.</span></span>

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
<span data-ttu-id="b9fcc-629">In komplexeren Szenarien, in denen die zugrunde liegende Speicherung eines Ereignisses gesteuert werden soll, können in einer Ereignisdeklaration explizit die `add`- und `remove`-Accessors bereitgestellt werden. Diese ähneln in gewisser Weise dem `set`-Accessor einer Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-629">For advanced scenarios where control of the underlying storage of an event is desired, an event declaration can explicitly provide `add` and `remove` accessors, which are somewhat similar to the `set` accessor of a property.</span></span>

#### <a name="operators"></a><span data-ttu-id="b9fcc-630">Operatoren</span><span class="sxs-lookup"><span data-stu-id="b9fcc-630">Operators</span></span>

<span data-ttu-id="b9fcc-631">Ein ***Operator*** ist ein Member, der die Bedeutung der Anwendung eines bestimmten Ausdrucksoperators auf Instanzen einer Klasse definiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-631">An ***operator*** is a member that defines the meaning of applying a particular expression operator to instances of a class.</span></span> <span data-ttu-id="b9fcc-632">Es können drei Arten von Operatoren definiert werden: unäre Operatoren, binäre Operatoren und Konvertierungsoperatoren.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-632">Three kinds of operators can be defined: unary operators, binary operators, and conversion operators.</span></span> <span data-ttu-id="b9fcc-633">Alle Operatoren müssen als `public` und `static` deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-633">All operators must be declared as `public` and `static`.</span></span>

<span data-ttu-id="b9fcc-634">Die `List<T>
`-Klasse deklariert zwei Operatoren, `operator==` und `operator!=`, und verleiht so Ausdrücken, die diese Operatoren auf `List`-Instanzen anwenden, eine neue Bedeutung.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-634">The `List<T>
` class declares two operators, `operator==` and `operator!=`, and thus gives new meaning to expressions that apply those operators to `List` instances.</span></span> <span data-ttu-id="b9fcc-635">Insbesondere die Operatoren definieren die Gleichheit von zwei `List<T>
` als Vergleich aller enthaltenen Objekte mithilfe von Instanzen ihrer `Equals` Methoden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-635">Specifically, the operators define equality of two `List<T>
` instances as comparing each of the contained objects using their `Equals` methods.</span></span> <span data-ttu-id="b9fcc-636">Im folgenden Beispiel wird der `==`-Operator verwendet, um zwei Instanzen von `List<int>
` zu vergleichen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-636">The following example uses the `==` operator to compare two `List<int>
` instances.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

<span data-ttu-id="b9fcc-637">Die erste Methode `Console.WriteLine` gibt `True` aus, weil die zwei Listen dieselbe Anzahl von Objekten mit denselben Werten in derselben Reihenfolge enthalten.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-637">The first `Console.WriteLine` outputs `True` because the two lists contain the same number of objects with the same values in the same order.</span></span> <span data-ttu-id="b9fcc-638">Wenn `List<T>
` nicht `operator==` definieren würde, würde die Ausgabe der ersten `Console.WriteLine`-Methode `False` lauten, weil `a` und `b` auf unterschiedliche `List<int>
`-Instanzen verweisen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-638">Had `List<T>
` not defined `operator==`, the first `Console.WriteLine` would have output `False` because `a` and `b` reference different `List<int>
` instances.</span></span>

#### <a name="destructors"></a><span data-ttu-id="b9fcc-639">Destruktoren</span><span class="sxs-lookup"><span data-stu-id="b9fcc-639">Destructors</span></span>

<span data-ttu-id="b9fcc-640">Ein ***Destruktor*** ist ein Element, das die erforderlichen Aktionen zum zerstört einer Instanz einer Klasse implementiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-640">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="b9fcc-641">Destruktoren können keine Parameter haben, sie können keine Zugriffsmodifizierer aufweisen und sie können nicht explizit aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-641">Destructors cannot have parameters, they cannot have accessibility modifiers, and they cannot be invoked explicitly.</span></span> <span data-ttu-id="b9fcc-642">Der Destruktor für eine Instanz wird automatisch während der automatischen speicherbereinigung aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-642">The destructor for an instance is invoked automatically during garbage collection.</span></span>

<span data-ttu-id="b9fcc-643">Der Garbage Collector kann entscheiden, wann Objekte sammeln und Destruktoren ausführen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-643">The garbage collector is allowed wide latitude in deciding when to collect objects and run destructors.</span></span> <span data-ttu-id="b9fcc-644">Insbesondere der Zeitpunkt der Destruktor aufrufen, ist nicht deterministisch und Destruktoren können in jedem Thread ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-644">Specifically, the timing of destructor invocations is not deterministic, and destructors may be executed on any thread.</span></span> <span data-ttu-id="b9fcc-645">Für diesen und anderen Gründen sollten Klassen mit Destruktoren implementieren, nur, wenn keine andere Lösung möglich ist.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-645">For these and other reasons, classes should implement destructors only when no other solutions are feasible.</span></span>

<span data-ttu-id="b9fcc-646">Die `using`-Anweisung bietet einen besseren Ansatz für die Objektzerstörung.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-646">The `using` statement provides a better approach to object destruction.</span></span>

## <a name="structs"></a><span data-ttu-id="b9fcc-647">Strukturen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-647">Structs</span></span>

<span data-ttu-id="b9fcc-648">Wie Klassen sind ***Strukturen*** Datenstrukturen, die Datenmember und Funktionsmember enthalten können, aber im Gegensatz zu Klassen sind Strukturen Werttypen und erfordern keine Heapzuordnung.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-648">Like classes, ***structs*** are data structures that can contain data members and function members, but unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="b9fcc-649">Eine Variable eines Strukturtyps speichert die Daten der Struktur direkt, während eine Variable eines Klassentyps einen Verweis auf ein dynamisch zugeordnetes Objekt speichert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-649">A variable of a struct type directly stores the data of the struct, whereas a variable of a class type stores a reference to a dynamically allocated object.</span></span> <span data-ttu-id="b9fcc-650">Strukturtypen unterstützen keine benutzerdefinierte Vererbung, und alle Strukturtypen erben implizit vom Typ `object`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-650">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="b9fcc-651">Strukturen sind besonders nützlich für kleine Datenstrukturen, die über Wertsemantik verfügen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-651">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="b9fcc-652">Komplexe Zahlen, Punkte in einem Koordinatensystem oder Schlüssel-Wert-Paare im Wörterbuch sind gute Beispiele für Strukturen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-652">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="b9fcc-653">Die Verwendung von Strukturen statt Klassen für kleine Datenstrukturen kann bei der Anzahl der Speicherbelegungen, die eine Anwendung durchführt, einen großen Unterschied ausmachen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-653">The use of structs rather than classes for small data structures can make a large difference in the number of memory allocations an application performs.</span></span> <span data-ttu-id="b9fcc-654">Das folgende Programm erstellt und initialisiert z.B. ein Array aus 100 Punkten.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-654">For example, the following program creates and initializes an array of 100 points.</span></span> <span data-ttu-id="b9fcc-655">Mit `Point` als implementierter Klasse werden 101 separate Objekte instanziiert – eines für das Array und jeweils eines für jedes der 100 Elemente.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-655">With `Point` implemented as a class, 101 separate objects are instantiated—one for the array and one each for the 100 elements.</span></span>

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
<span data-ttu-id="b9fcc-656">Eine Alternative ist, `Point` eine Struktur.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-656">An alternative is to make `Point` a struct.</span></span>

```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="b9fcc-657">Jetzt wird nur ein Objekt instanziiert – für das Array – und die `Point`-Instanzen werden inline im Array gespeichert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-657">Now, only one object is instantiated—the one for the array—and the `Point` instances are stored in-line in the array.</span></span>

<span data-ttu-id="b9fcc-658">Strukturkonstruktoren werden mit dem neuen Operator `new` aufgerufen, doch das bedeutet nicht, dass der Arbeitsspeicher belegt wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-658">Struct constructors are invoked with the `new` operator, but that does not imply that memory is being allocated.</span></span> <span data-ttu-id="b9fcc-659">Statt ein Objekt dynamisch zuzuordnen und einen Verweis darauf zurückzugeben, gibt ein Strukturkonstruktor einfach den Strukturwert selbst zurück (in der Regel in einen temporären Speicherort auf dem Stapel), und dieser Wert wird dann nach Bedarf kopiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-659">Instead of dynamically allocating an object and returning a reference to it, a struct constructor simply returns the struct value itself (typically in a temporary location on the stack), and this value is then copied as necessary.</span></span>

<span data-ttu-id="b9fcc-660">Mit Klassen können zwei Variablen auf das gleiche Objekt verweisen, und so können an einer Variablen durchgeführte Vorgänge das Objekt beeinflussen, auf das die andere Variable verweist.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-660">With classes, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="b9fcc-661">Mit Strukturen besitzt jede Variable eine eigene Kopie der Daten, und es ist nicht möglich, dass an einer Variablen durchgeführte Vorgänge die andere beeinflussen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-661">With structs, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="b9fcc-662">Zum Beispiel die Ausgabe durch das folgende Codefragment davon abhängig, ob `Point` ist eine Klasse oder Struktur.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-662">For example, the output produced by the following code fragment depends on whether `Point` is a class or a struct.</span></span>

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
<span data-ttu-id="b9fcc-663">Wenn `Point` ist eine Klasse, die Ausgabe ist `20` da `a` und `b` auf das gleiche Objekt verweisen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-663">If `Point` is a class, the output is `20` because `a` and `b` reference the same object.</span></span> <span data-ttu-id="b9fcc-664">Wenn `Point` ist eine Struktur, die Ausgabe ist `10` da die Zuweisung von `a` zu `b` erstellt eine Kopie des Werts, und diese Kopie ist nicht betroffen von der nachfolgenden Zuweisung zu `a.x`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-664">If `Point` is a struct, the output is `10` because the assignment of `a` to `b` creates a copy of the value, and this copy is unaffected by the subsequent assignment to `a.x`.</span></span>

<span data-ttu-id="b9fcc-665">Im vorherigen Beispiel werden zwei der Einschränkungen von Strukturen hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-665">The previous example highlights two of the limitations of structs.</span></span> <span data-ttu-id="b9fcc-666">Erstens ist das Kopieren einer gesamten Struktur in der Regel weniger effizient als das Kopieren eines Objektverweises, sodass Zuweisung und Wertparameterübergabe mit Strukturen aufwändiger sein kann als mit Verweistypen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-666">First, copying an entire struct is typically less efficient than copying an object reference, so assignment and value parameter passing can be more expensive with structs than with reference types.</span></span> <span data-ttu-id="b9fcc-667">Zweitens ist es mit Ausnahme der `ref`- und `out`-Parameter nicht möglich, Verweise auf Strukturen zu erstellen, was ihre Verwendung in einer Reihe von Situationen ausschließt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-667">Second, except for `ref` and `out` parameters, it is not possible to create references to structs, which rules out their usage in a number of situations.</span></span>

## <a name="arrays"></a><span data-ttu-id="b9fcc-668">Arrays</span><span class="sxs-lookup"><span data-stu-id="b9fcc-668">Arrays</span></span>

<span data-ttu-id="b9fcc-669">Ein ***Array*** ist eine Datenstruktur, die eine Anzahl von Variablen enthält, auf die über berechnete Indizes zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-669">An ***array*** is a data structure that contains a number of variables that are accessed through computed indices.</span></span> <span data-ttu-id="b9fcc-670">Die im Array enthaltenen Variablen, auch ***Elemente*** des Arrays genannt, weisen alle denselben Typ auf. Dieser Typ wird als ***Elementtyp*** des Arrays bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-670">The variables contained in an array, also called the ***elements*** of the array, are all of the same type, and this type is called the ***element type*** of the array.</span></span>

<span data-ttu-id="b9fcc-671">Arraytypen sind Verweistypen, und die Deklaration einer Arrayvariablen reserviert Speicher für einen Verweis auf eine Arrayinstanz.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-671">Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance.</span></span> <span data-ttu-id="b9fcc-672">Die tatsächlichen Arrayinstanzen werden dynamisch erstellt, zur Laufzeit mithilfe der `new` Operator.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-672">Actual array instances are created dynamically at run-time using the `new` operator.</span></span> <span data-ttu-id="b9fcc-673">Die `new` Vorgang gibt die ***Länge*** der neuen Arrayinstanz, die dann für die Lebensdauer der Instanz behoben wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-673">The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance.</span></span> <span data-ttu-id="b9fcc-674">Die Indizes der Arrayelemente reichen von `0` bis `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-674">The indices of the elements of an array range from `0` to `Length - 1`.</span></span> <span data-ttu-id="b9fcc-675">Der `new`-Operator initialisiert die Elemente eines Arrays automatisch mit ihren Standardwerten. Dieser lautet z.B. für alle numerischen Typen 0 und für alle Verweistypen `null`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-675">The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.</span></span>

<span data-ttu-id="b9fcc-676">Im folgenden Beispiel wird ein Array aus `int`-Elementen erstellt. Anschließend wird das Array initialisiert und die Inhalte des Arrays werden gedruckt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-676">The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
<span data-ttu-id="b9fcc-677">Mit diesem Beispiel wird ein ***eindimensionales Array*** erstellt und verwendet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-677">This example creates and operates on a ***single-dimensional array***.</span></span> <span data-ttu-id="b9fcc-678">C# unterstützt auch ***mehrdimensionale Arrays***.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-678">C# also supports ***multi-dimensional arrays***.</span></span> <span data-ttu-id="b9fcc-679">Die Anzahl von Dimensionen eines Arraytyps, auch als ***Rang*** des Arraytyps bezeichnet, ist 1 plus die Anzahl von Kommas, die innerhalb der eckigen Klammern des Arraytyps angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-679">The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type.</span></span> <span data-ttu-id="b9fcc-680">Im folgende Beispiel weist ein eindimensionales, ein zweidimensionales und ein dreidimensionales Array.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-680">The following example allocates a one-dimensional, a two-dimensional, and a three-dimensional array.</span></span>

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
<span data-ttu-id="b9fcc-681">Das `a1`-Array enthält 10 Elemente, das `a2`-Array umfasst 50 (10 × 5) Elemente, und das `a3`-Array enthält 100 (10 × 5 × 2) Elemente.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-681">The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.</span></span>

<span data-ttu-id="b9fcc-682">Ein Array kann einen beliebigen Elementtyp verwenden, einschließlich eines Arraytyps. </span><span class="sxs-lookup"><span data-stu-id="b9fcc-682">The element type of an array can be any type, including an array type.</span></span> <span data-ttu-id="b9fcc-683">Ein Array mit Elementen eines Arraytyps wird auch als ***verzweigtes Array*** bezeichnet, weil die Länge der Elementarrays nicht identisch sein muss.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-683">An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays do not all have to be the same.</span></span> <span data-ttu-id="b9fcc-684">Im folgenden Beispiel wird ein Array aus `int`-Arrays zugewiesen:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-684">The following example allocates an array of arrays of `int`:</span></span>

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
<span data-ttu-id="b9fcc-685">In der ersten Zeile wird ein Array mit drei Elementen erstellt, das jeweils den Typ `int[]` und einen Anfangswert von `null` aufweist.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-685">The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`.</span></span> <span data-ttu-id="b9fcc-686">In den folgenden Zeilen werden die drei Elemente mit Verweisen auf einzelne Arrayinstanzen unterschiedlicher Länge initialisiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-686">The subsequent lines then initialize the three elements with references to individual array instances of varying lengths.</span></span>

<span data-ttu-id="b9fcc-687">Die `new` Operator erlaubt es, die Anfangswerte der Arrayelemente unter Verwendung einer ***Arrayinitialisierer***, dies ist eine Liste von Ausdrücken, die zwischen den Trennzeichen `{` und `}`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-687">The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`.</span></span> <span data-ttu-id="b9fcc-688">Mit dem folgenden Beispiel wird ein `int[]` mit drei Elementen zugewiesen und initialisiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-688">The following example allocates and initializes an `int[]` with three elements.</span></span>

```csharp
int[] a = new int[] {1, 2, 3};
```
<span data-ttu-id="b9fcc-689">Beachten Sie, dass die Länge des Arrays, von der Anzahl von Ausdrücken zwischen abgeleitet wird `{` und `}`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-689">Note that the length of the array is inferred from the number of expressions between `{` and `}`.</span></span> <span data-ttu-id="b9fcc-690">Deklarationen lokaler Variablen und Felder können weiter verkürzt werden, sodass der Arraytyp nicht erneut aufgeführt werden muss.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-690">Local variable and field declarations can be shortened further such that the array type does not have to be restated.</span></span>

```csharp
int[] a = {1, 2, 3};
```
<span data-ttu-id="b9fcc-691">Die zwei vorherigen Beispiele entsprechen dem folgenden:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-691">Both of the previous examples are equivalent to the following:</span></span>

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a><span data-ttu-id="b9fcc-692">Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-692">Interfaces</span></span>

<span data-ttu-id="b9fcc-693">Eine ***Schnittstelle*** definiert einen Vertrag, der von Klassen und Strukturen implementiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-693">An ***interface*** defines a contract that can be implemented by classes and structs.</span></span> <span data-ttu-id="b9fcc-694">Eine Schnittstelle kann Methoden, Eigenschaften, Ereignisse und Indexer enthalten.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-694">An interface can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="b9fcc-695">Eine Schnittstelle stellt keine Implementierungen der von ihr definierten Member bereit. Sie gibt lediglich die Member an, die von Klassen oder Strukturen bereitgestellt werden müssen, die die Schnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-695">An interface does not provide implementations of the members it defines—it merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

<span data-ttu-id="b9fcc-696">Schnittstellen können ***Mehrfachvererbung*** einsetzen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-696">Interfaces may employ ***multiple inheritance***.</span></span> <span data-ttu-id="b9fcc-697">Im folgenden Beispiel erbt die Schnittstelle `IComboBox` sowohl von `ITextBox` als auch `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-697">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`.</span></span>

```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="b9fcc-698">Klassen und Strukturen können mehrere Schnittstellen implementieren.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-698">Classes and structs can implement multiple interfaces.</span></span> <span data-ttu-id="b9fcc-699">Im folgenden Beispiel implementiert die Klasse `EditBox` sowohl `IControl` als auch `IDataBound`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-699">In the following example, the class `EditBox` implements both `IControl` and `IDataBound`.</span></span>

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
<span data-ttu-id="b9fcc-700">Wenn eine Klasse oder Struktur eine bestimmte Schnittstelle implementiert, können Instanzen dieser Klasse oder Struktur implizit in diesen Schnittstellentyp konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-700">When a class or struct implements a particular interface, instances of that class or struct can be implicitly converted to that interface type.</span></span> <span data-ttu-id="b9fcc-701">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-701">For example</span></span>

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
<span data-ttu-id="b9fcc-702">In Fällen, in denen nicht bekannt ist, dass eine Instanz eine bestimmte Schnittstelle implementiert, können dynamische Typumwandlungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-702">In cases where an instance is not statically known to implement a particular interface, dynamic type casts can be used.</span></span> <span data-ttu-id="b9fcc-703">Beispielsweise verwenden die folgenden Anweisungen dynamische Typumwandlungen zum Abrufen eines Objekts `IControl` und `IDataBound` schnittstellenimplementierungen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-703">For example, the following statements use dynamic type casts to obtain an object's `IControl` and `IDataBound` interface implementations.</span></span> <span data-ttu-id="b9fcc-704">Da der tatsächliche Typ des Objekts ist `EditBox`, die Umwandlungen erfolgreich.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-704">Because the actual type of the object is `EditBox`, the casts succeed.</span></span>

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
<span data-ttu-id="b9fcc-705">In der vorherigen `EditBox` -Klasse, die `Paint` Methode aus der `IControl` Schnittstelle und die `Bind` Methode aus der `IDataBound` Schnittstelle implementiert werden, mithilfe von `public` Member.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-705">In the previous `EditBox` class, the `Paint` method from the `IControl` interface and the `Bind` method from the `IDataBound` interface are implemented using `public` members.</span></span> <span data-ttu-id="b9fcc-706">C# unterstützt außerdem ***explizite Implementierungen eines Schnittstellenmembers***, verwenden die Klasse oder Struktur kann vermeiden, dass die Mitglieder `public`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-706">C# also supports ***explicit interface member implementations***, using which the class or struct can avoid making the members `public`.</span></span> <span data-ttu-id="b9fcc-707">Eine explizite Implementierung eines Schnittstellenmembers wird mit dem vollqualifizierten Namen des Schnittstellenmembers geschrieben.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-707">An explicit interface member implementation is written using the fully qualified interface member name.</span></span> <span data-ttu-id="b9fcc-708">Die `EditBox`-Klasse könnte z.B. die `IControl.Paint`- und `IDataBound.Bind`-Methode wie folgt über explizite Implementierungen eines Schnittstellenmembers implementieren.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-708">For example, the `EditBox` class could implement the `IControl.Paint` and `IDataBound.Bind` methods using explicit interface member implementations as follows.</span></span>

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
<span data-ttu-id="b9fcc-709">Der Zugriff auf explizite Schnittstellenmember kann nur über den Schnittstellentyp erfolgen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-709">Explicit interface members can only be accessed via the interface type.</span></span> <span data-ttu-id="b9fcc-710">Z. B. die Implementierung der `IControl.Paint` bereitgestellt, die von der vorherigen `EditBox` Klasse kann nur aufgerufen werden, indem zunächst die `EditBox` Verweis auf die `IControl` Schnittstellentyp.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-710">For example, the implementation of `IControl.Paint` provided by the previous `EditBox` class can only be invoked by first converting the `EditBox` reference to the `IControl` interface type.</span></span>

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a><span data-ttu-id="b9fcc-711">Enumerationen</span><span class="sxs-lookup"><span data-stu-id="b9fcc-711">Enums</span></span>

<span data-ttu-id="b9fcc-712">Ein ***Enumerationstyp*** ist ein eindeutiger Werttyp mit einem Satz benannter Konstanten.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-712">An ***enum type*** is a distinct value type with a set of named constants.</span></span> <span data-ttu-id="b9fcc-713">Das folgende Beispiel deklariert und verwendet einen Enumerationstyp, der mit dem Namen `Color` mit drei Konstanten Werten `Red`, `Green`, und `Blue`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-713">The following example declares and uses an enum type named `Color` with three constant values, `Red`, `Green`, and `Blue`.</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
<span data-ttu-id="b9fcc-714">Jeder Enumerationstyp hat einen entsprechenden ganzzahligen Typ, der Namen der ***zugrunde liegender Typ*** des Enum-Typs.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-714">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="b9fcc-715">Ein Enumerationstyp, der nicht explizit einen zugrunde liegenden Typ deklariert, hat einen zugrunde liegenden Typ `int`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-715">An enum type that does not explicitly declare an underlying type has an underlying type of `int`.</span></span> <span data-ttu-id="b9fcc-716">Speicherformat und den Bereich der möglichen Werte eines Enumerationstyps werden durch die zugrunde liegenden Typ bestimmt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-716">An enum type's storage format and range of possible values are determined by its underlying type.</span></span> <span data-ttu-id="b9fcc-717">Der Satz von Werten, die ein Enum-Typ übernehmen kann, ist nicht durch die Enumerationsmember beschränkt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-717">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="b9fcc-718">Insbesondere ein Wert für den zugrunde liegenden Typ einer Enumeration in Enum-Typs umgewandelt werden kann und ein eindeutiger gültiger Wert dieses Enum-Typs ist.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-718">In particular, any value of the underlying type of an enum can be cast to the enum type and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="b9fcc-719">Das folgende Beispiel deklariert einen Enum-Typ, der mit dem Namen `Alignment` mit einem zugrunde liegenden Typs `sbyte`.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-719">The following example declares an enum type named `Alignment` with an underlying type of `sbyte`.</span></span>

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
<span data-ttu-id="b9fcc-720">Wie im vorherigen Beispiel gezeigt wird, zählen eine Enum-Element-Deklaration einen konstanten Ausdruck, der den Wert des Members angibt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-720">As shown by the previous example, an enum member declaration can include a constant expression that specifies the value of the member.</span></span> <span data-ttu-id="b9fcc-721">Der Konstante Wert für jede Enum-Element muss im Bereich von der zugrunde liegende Typ der Enumeration.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-721">The constant value for each enum member must be in the range of the underlying type of the enum.</span></span> <span data-ttu-id="b9fcc-722">Wenn eine Enum-Memberdeklaration nicht explizit einen Wert angeben, erhält das Element den Wert 0 (null), (wenn es das erste Element in der Enumerationstyp ist) oder den Wert des textlich vorausgehenden Enum-Element plus 1.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-722">When an enum member declaration does not explicitly specify a value, the member is given the value zero (if it is the first member in the enum type) or the value of the textually preceding enum member plus one.</span></span>

<span data-ttu-id="b9fcc-723">Enum-Werte können in ganzzahlige Werte und umgekehrt mit Typumwandlungen werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-723">Enum values can be converted to integral values and vice versa using type casts.</span></span> <span data-ttu-id="b9fcc-724">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-724">For example</span></span>

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
<span data-ttu-id="b9fcc-725">Der Standardwert eines beliebigen Enumerationstyps ist der ganzzahlige Wert 0 (null), die in den Enumerationstyp konvertiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-725">The default value of any enum type is the integral value zero converted to the enum type.</span></span> <span data-ttu-id="b9fcc-726">In Fällen, in denen Variablen automatisch auf einen Standardwert initialisiert werden, ist dies der Wert, der Variablen von Enum-Typen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-726">In cases where variables are automatically initialized to a default value, this is the value given to variables of enum types.</span></span> <span data-ttu-id="b9fcc-727">In der Reihenfolge für den Standardwert eines Enumerationstyps leicht verfügbar ist, ist das Literal `0` implizit in einen beliebigen Enumerationstyp konvertiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-727">In order for the default value of an enum type to be easily available, the literal `0` implicitly converts to any enum type.</span></span> <span data-ttu-id="b9fcc-728">Daher ist Folgendes zugelassen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-728">Thus, the following is permitted.</span></span>

```csharp
Color c = 0;
```

## <a name="delegates"></a><span data-ttu-id="b9fcc-729">Delegaten</span><span class="sxs-lookup"><span data-stu-id="b9fcc-729">Delegates</span></span>

<span data-ttu-id="b9fcc-730">Ein ***Delegattyp*** stellt Verweise auf Methoden mit einer bestimmten Parameterliste und dem Rückgabetyp dar.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-730">A ***delegate type*** represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="b9fcc-731">Delegate ermöglichen die Behandlung von Methoden als Entitäten, die Variablen zugewiesen und als Parameter übergeben werden können.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-731">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="b9fcc-732">Delegate ähneln dem Konzept von Funktionszeigern, die Sie in einigen anderen Sprachen finden. Im Gegensatz zu Funktionszeigern sind Delegate allerdings objektorientiert und typsicher.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-732">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="b9fcc-733">Im folgenden Beispiel wird ein Delegattyp namens `Function` deklariert und verwendet.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-733">The following example declares and uses a delegate type named `Function`.</span></span>

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
<span data-ttu-id="b9fcc-734">Eine Instanz des Delegattyps `Function` kann auf jede Methode verweisen, die ein `double`-Argument und einen `double`-Wert akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-734">An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value.</span></span> <span data-ttu-id="b9fcc-735">Die `Apply` Methode gilt eine angegebenen `Function` für die Elemente der ein `double[]`, zurückgeben eine `double[]` mit den Ergebnissen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-735">The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results.</span></span> <span data-ttu-id="b9fcc-736">In der `Main`-Methode wird `Apply` verwendet, um drei verschiedene Funktionen auf ein `double[]` anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-736">In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.</span></span>

<span data-ttu-id="b9fcc-737">Ein Delegat kann entweder auf eine statische Methode verweisen (z.B. `Square` oder `Math.Sin` im vorherigen Beispiel) oder eine Instanzmethode (z.B. `m.Multiply` im vorherigen Beispiel).</span><span class="sxs-lookup"><span data-stu-id="b9fcc-737">A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example).</span></span> <span data-ttu-id="b9fcc-738">Ein Delegat, der auf eine Instanzmethode verweist, verweist auch auf ein bestimmtes Objekt, und wenn die Instanzmethode durch den Delegaten aufgerufen wird, wird das Objekt `this` im Aufruf.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-738">A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.</span></span>

<span data-ttu-id="b9fcc-739">Delegaten können auch mit anonymen Funktionen erstellt werden, die dynamisch erstellte „Inlinemethoden“ sind.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-739">Delegates can also be created using anonymous functions, which are "inline methods" that are created on the fly.</span></span> <span data-ttu-id="b9fcc-740">Anonyme Funktionen können die lokalen Variablen der umgebenden Methoden sehen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-740">Anonymous functions can see the local variables of the surrounding methods.</span></span> <span data-ttu-id="b9fcc-741">Daher das obige multiplikatorbeispiel geschrieben werden kann leichter ohne Verwendung einer `Multiplier` Klasse:</span><span class="sxs-lookup"><span data-stu-id="b9fcc-741">Thus, the multiplier example above can be written more easily without using a `Multiplier` class:</span></span>

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
<span data-ttu-id="b9fcc-742">Eine interessante und nützliche Eigenschaft eines Delegaten ist, dass er die Klasse der Methode, auf die er verweist, nicht kennt oder sie ignoriert; wichtig ist nur, dass die referenzierte Methode die gleichen Parameter und den gleichen Rückgabetyp hat wie der Delegat.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-742">An interesting and useful property of a delegate is that it does not know or care about the class of the method it references; all that matters is that the referenced method has the same parameters and return type as the delegate.</span></span>

## <a name="attributes"></a><span data-ttu-id="b9fcc-743">Attribute</span><span class="sxs-lookup"><span data-stu-id="b9fcc-743">Attributes</span></span>

<span data-ttu-id="b9fcc-744">Typen, Member und andere Entitäten in einem C#-Programm unterstützen Modifizierer, die bestimmte Aspekte ihres Verhaltens steuern.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-744">Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior.</span></span> <span data-ttu-id="b9fcc-745">Der Zugriff auf eine Methode wird beispielsweise mithilfe der Modifizierer `public`, `protected`, `internal` und `private` kontrolliert.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-745">For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers.</span></span> <span data-ttu-id="b9fcc-746">C# generalisiert diese Funktionalität, indem benutzerdefinierte Typen deklarativer Informationen an eine Programmentität angefügt und zur Laufzeit abgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-746">C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time.</span></span> <span data-ttu-id="b9fcc-747">Programm geben diese zusätzlichen deklarativen Informationen durch das Definieren und Verwenden von ***Attributen*** an.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-747">Programs specify this additional declarative information by defining and using ***attributes***.</span></span>

<span data-ttu-id="b9fcc-748">Im folgenden Beispiel wird ein `HelpAttribute`-Attribut deklariert, dass in Programmentitäten platziert werden kann, um Links zur zugehörigen Dokumentation bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-748">The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.</span></span>

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
<span data-ttu-id="b9fcc-749">Alle Attributklassen leiten Sie von der `System.Attribute` Basisklasse, die von .NET Framework bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-749">All attribute classes derive from the `System.Attribute` base class provided by the .NET Framework.</span></span> <span data-ttu-id="b9fcc-750">Attribute können durch Angabe ihres Namens angewendet werden, zusammen mit beliebigen Argumenten. Diese müssen in eckigen Klammern genau vor der zugehörigen Deklaration eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-750">Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration.</span></span> <span data-ttu-id="b9fcc-751">Wenn Sie den Namen eines Attributs endet `Attribute`, dieser Teil des Namens kann ausgelassen werden, wenn das Attribut verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-751">If an attribute's name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced.</span></span> <span data-ttu-id="b9fcc-752">Beispielsweise kann das `HelpAttribute`-Attribut wie folgt verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-752">For example, the `HelpAttribute` attribute can be used as follows.</span></span>

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
<span data-ttu-id="b9fcc-753">In diesem Beispiel fügt eine `HelpAttribute` auf die `Widget` -Klasse und eine weitere `HelpAttribute` auf die `Display` Methode in der Klasse.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-753">This example attaches a `HelpAttribute` to the `Widget` class and another `HelpAttribute` to the `Display` method in the class.</span></span> <span data-ttu-id="b9fcc-754">Die öffentlichen Konstruktoren einer Attributklasse steuern die Informationen, die beim Anfügen des Attributs an eine Programmentität angegeben werden müssen.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-754">The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity.</span></span> <span data-ttu-id="b9fcc-755">Zusätzliche Informationen können angegeben werden, indem auf öffentliche Eigenschaften mit Lese-/Schreibzugriff der Attributklasse verwiesen wird (z.B. wie der obige Verweis auf die `Topic`-Eigenschaft).</span><span class="sxs-lookup"><span data-stu-id="b9fcc-755">Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).</span></span>

<span data-ttu-id="b9fcc-756">Das folgende Beispiel zeigt, wie Informationen über Bildattribute für ein bestimmtes Programmentität zur Laufzeit abgerufen werden kann mithilfe von Reflektion.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-756">The following example shows how attribute information for a given program entity can be retrieved at run-time using reflection.</span></span>

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
<span data-ttu-id="b9fcc-757">Wenn per Reflektion ein bestimmtes Attribut angefordert wird, wird der Konstruktor für die Attributklasse mit den in der Programmquelle angegebenen Informationen aufgerufen, und die resultierende Attributinstanz wird zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-757">When a particular attribute is requested through reflection, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned.</span></span> <span data-ttu-id="b9fcc-758">Wenn zusätzliche Informationen über Eigenschaften bereitgestellt wurden, werden diese Eigenschaften auf die vorgegebenen Werte festgelegt, bevor die Attributinstanz zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b9fcc-758">If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.</span></span>
