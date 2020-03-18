---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484081"
---
# <a name="default-interface-methods"></a><span data-ttu-id="cdf65-101">Standardschnittstellen Methoden</span><span class="sxs-lookup"><span data-stu-id="cdf65-101">default interface methods</span></span>

* <span data-ttu-id="cdf65-102">[x] vorgeschlagen</span><span class="sxs-lookup"><span data-stu-id="cdf65-102">[x] Proposed</span></span>
* <span data-ttu-id="cdf65-103">[] Prototyp: [in](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md) Bearbeitung</span><span class="sxs-lookup"><span data-stu-id="cdf65-103">[ ] Prototype: [In progress](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span></span>
* <span data-ttu-id="cdf65-104">[] Implementierung: keine</span><span class="sxs-lookup"><span data-stu-id="cdf65-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="cdf65-105">[] Spezifikation: in Bearbeitung</span><span class="sxs-lookup"><span data-stu-id="cdf65-105">[ ] Specification: In progress, below</span></span>

## <a name="summary"></a><span data-ttu-id="cdf65-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="cdf65-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="cdf65-107">Hinzufügen von Unterstützung für _virtuelle Erweiterungs Methoden_ -Methoden in Schnittstellen mit konkreten Implementierungen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-107">Add support for _virtual extension methods_ - methods in interfaces with concrete implementations.</span></span> <span data-ttu-id="cdf65-108">Eine Klasse oder Struktur, die eine solche Schnittstelle implementiert, muss über eine einzige _spezifischere_ Implementierung für die Schnittstellen Methode verfügen, die entweder von der Klasse oder Struktur implementiert wird oder von ihren Basisklassen oder Schnittstellen geerbt wird.</span><span class="sxs-lookup"><span data-stu-id="cdf65-108">A class or struct that implements such an interface is required to have a single _most specific_ implementation for the interface method, either implemented by the class or struct, or inherited from its base classes or interfaces.</span></span> <span data-ttu-id="cdf65-109">Mithilfe von virtuellen Erweiterungs Methoden kann ein API-Autor Methoden zu einer Schnittstelle in zukünftigen Versionen hinzufügen, ohne dass die Quell-oder Binärkompatibilität mit vorhandenen Implementierungen dieser Schnittstelle unterbrochen wird.</span><span class="sxs-lookup"><span data-stu-id="cdf65-109">Virtual extension methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>

<span data-ttu-id="cdf65-110">Diese ähneln den ["Standardmethoden"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)von Java.</span><span class="sxs-lookup"><span data-stu-id="cdf65-110">These are similar to Java's ["Default Methods"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html).</span></span>

<span data-ttu-id="cdf65-111">(Basierend auf dem wahrscheinlichen Implementierungs Verfahren) erfordert dieses Feature entsprechende Unterstützung in der CLI/CLR.</span><span class="sxs-lookup"><span data-stu-id="cdf65-111">(Based on the likely implementation technique) this feature requires corresponding support in the CLI/CLR.</span></span> <span data-ttu-id="cdf65-112">Programme, die dieses Feature nutzen, können in früheren Versionen der Plattform nicht ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-112">Programs that take advantage of this feature cannot run on earlier versions of the platform.</span></span>

## <a name="motivation"></a><span data-ttu-id="cdf65-113">Motivation</span><span class="sxs-lookup"><span data-stu-id="cdf65-113">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="cdf65-114">Die Hauptgründe für dieses Feature sind</span><span class="sxs-lookup"><span data-stu-id="cdf65-114">The principal motivations for this feature are</span></span>

- <span data-ttu-id="cdf65-115">Mithilfe von Standardschnittstellen Methoden kann ein API-Autor in zukünftigen Versionen Methoden zu einer Schnittstelle hinzufügen, ohne dass die Quell-oder Binärkompatibilität mit vorhandenen Implementierungen dieser Schnittstelle unterbrochen wird.</span><span class="sxs-lookup"><span data-stu-id="cdf65-115">Default interface methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>
- <span data-ttu-id="cdf65-116">Mit der- C# Funktion können Sie mit APIs für [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) und [IOS (SWIFT)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267)interagieren, die ähnliche Funktionen unterstützen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-116">The feature enables C# to interoperate with APIs targeting [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) and [iOS (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), which support similar features.</span></span>
- <span data-ttu-id="cdf65-117">Es stellt sich heraus, dass das Hinzufügen von Standardschnittstellen Implementierungen die Elemente der Sprachfunktion "Merkmale" (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>) bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-117">As it turns out, adding default interface implementations provides the elements of the "traits" language feature (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>).</span></span> <span data-ttu-id="cdf65-118">Merkmale haben sich als leistungsstarkes Programmierverfahren (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>) bewährt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-118">Traits have proven to be a powerful programming technique (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).</span></span>

## <a name="detailed-design"></a><span data-ttu-id="cdf65-119">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="cdf65-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="cdf65-120">Die Syntax für eine Schnittstelle wird auf zulassen erweitert.</span><span class="sxs-lookup"><span data-stu-id="cdf65-120">The syntax for an interface is extended to permit</span></span>

- <span data-ttu-id="cdf65-121">Member-Deklarationen, die Konstanten, Operatoren, statische Konstruktoren und die Typen von Typen deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cdf65-121">member declarations that declare constants, operators, static constructors, and nested types;</span></span>
- <span data-ttu-id="cdf65-122">ein *Text* für eine Methode oder einen Indexer, eine Eigenschaft oder einen Ereignis Accessor (d. h. eine "Default"-Implementierung);</span><span class="sxs-lookup"><span data-stu-id="cdf65-122">a *body* for a method or indexer, property, or event accessor (that is, a "default" implementation);</span></span>
- <span data-ttu-id="cdf65-123">Member-Deklarationen, die statische Felder, Methoden, Eigenschaften, Indexer und Ereignisse deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cdf65-123">member declarations that declare static fields, methods, properties, indexers, and events;</span></span>
- <span data-ttu-id="cdf65-124">Member-Deklarationen mit der expliziten Schnittstellen Implementierungs Syntax; immer</span><span class="sxs-lookup"><span data-stu-id="cdf65-124">member declarations using the explicit interface implementation syntax; and</span></span>
- <span data-ttu-id="cdf65-125">Explizite Zugriffsmodifizierer (der Standard Zugriff ist `public`).</span><span class="sxs-lookup"><span data-stu-id="cdf65-125">Explicit access modifiers (the default access is `public`).</span></span>

<span data-ttu-id="cdf65-126">Member mit Text ermöglichen der-Schnittstelle, eine "Default"-Implementierung für die Methode in Klassen und Strukturen bereitzustellen, die keine über schreibende Implementierung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-126">Members with bodies permit the interface to provide a "default" implementation for the method in classes and structs that do not provide an overriding implementation.</span></span>

<span data-ttu-id="cdf65-127">Schnittstellen dürfen keinen Instanzstatus enthalten.</span><span class="sxs-lookup"><span data-stu-id="cdf65-127">Interfaces may not contain instance state.</span></span> <span data-ttu-id="cdf65-128">Obwohl statische Felder jetzt zulässig sind, sind Instanzfelder in Schnittstellen nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="cdf65-128">While static fields are now permitted, instance fields are not permitted in interfaces.</span></span> <span data-ttu-id="cdf65-129">Automatische Instanzeigenschaften werden in Schnittstellen nicht unterstützt, da Sie implizit ein ausgeblendetes Feld deklarieren würden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-129">Instance auto-properties are not supported in interfaces, as they would implicitly declare a hidden field.</span></span>

<span data-ttu-id="cdf65-130">Statische und private Methoden ermöglichen ein nützliches Refactoring und eine Organisation des Codes, der zum Implementieren der öffentlichen API der Schnittstelle verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="cdf65-130">Static and private methods permit useful refactoring and organization of code used to implement the interface's public API.</span></span>

<span data-ttu-id="cdf65-131">Eine Methoden Überschreibung in einer Schnittstelle muss die explizite Schnittstellen Implementierungs Syntax verwenden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-131">A method override in an interface must use the explicit interface implementation syntax.</span></span>

<span data-ttu-id="cdf65-132">Es ist ein Fehler, einen Klassentyp, Strukturtyp oder Enumerationstyp innerhalb des Gültigkeits Bereichs eines Typparameters zu deklarieren, der mit einem *variance_annotation*deklariert wurde.</span><span class="sxs-lookup"><span data-stu-id="cdf65-132">It is an error to declare a class type, struct type, or enum type within the scope of a type parameter that was declared with a *variance_annotation*.</span></span>  <span data-ttu-id="cdf65-133">Beispielsweise ist die Deklaration von `C` unten ein Fehler.</span><span class="sxs-lookup"><span data-stu-id="cdf65-133">For example, the declaration of `C` below is an error.</span></span>

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a><span data-ttu-id="cdf65-134">Konkrete Methoden in Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="cdf65-134">Concrete methods in interfaces</span></span>

<span data-ttu-id="cdf65-135">Die einfachste Form dieses Features ist die Möglichkeit, eine *konkrete Methode* in einer Schnittstelle zu deklarieren, bei der es sich um eine Methode mit einem Text handelt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-135">The simplest form of this feature is the ability to declare a *concrete method* in an interface, which is a method with a body.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

<span data-ttu-id="cdf65-136">Eine Klasse, die diese Schnittstelle implementiert, muss Ihre konkrete Methode nicht implementieren.</span><span class="sxs-lookup"><span data-stu-id="cdf65-136">A class that implements this interface need not implement its concrete method.</span></span>

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

<span data-ttu-id="cdf65-137">Die abschließende außer Kraft setzung für `IA.M` in Class `C` ist die konkrete Methode `M` die in `IA`deklariert wurde.</span><span class="sxs-lookup"><span data-stu-id="cdf65-137">The final override for `IA.M` in class `C` is the concrete method `M` declared in `IA`.</span></span> <span data-ttu-id="cdf65-138">Beachten Sie, dass eine Klasse keine Member von ihren Schnittstellen erbt. Dies wird von dieser Funktion nicht geändert:</span><span class="sxs-lookup"><span data-stu-id="cdf65-138">Note that a class does not inherit members from its interfaces; that is not changed by this feature:</span></span>

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

<span data-ttu-id="cdf65-139">In einem Instanzmember einer Schnittstelle hat `this` den Typ der einschließenden Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="cdf65-139">Within an instance member of an interface, `this` has the type of the enclosing interface.</span></span>

### <a name="modifiers-in-interfaces"></a><span data-ttu-id="cdf65-140">Modifiziererer in Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="cdf65-140">Modifiers in interfaces</span></span>

<span data-ttu-id="cdf65-141">Die Syntax für eine Schnittstelle wird gelockert, um modifiziererer auf ihren Membern zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-141">The syntax for an interface is relaxed to permit modifiers on its members.</span></span> <span data-ttu-id="cdf65-142">Folgendes ist zulässig: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`und `partial`.</span><span class="sxs-lookup"><span data-stu-id="cdf65-142">The following are permitted: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`, and `partial`.</span></span>

> <span data-ttu-id="cdf65-143">***TODO***: Überprüfen Sie, welche anderen modifiziererer vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="cdf65-143">***TODO***: check what other modifiers exist.</span></span>

<span data-ttu-id="cdf65-144">Ein Schnittstellenmember, dessen Deklaration einen Text enthält, ist ein `virtual` Member, es sei denn, der `sealed` oder `private` Modifizierer wird verwendet</span><span class="sxs-lookup"><span data-stu-id="cdf65-144">An interface member whose declaration includes a body is a `virtual` member unless the `sealed` or `private` modifier is used.</span></span> <span data-ttu-id="cdf65-145">Der `virtual` Modifizierer kann auf einem Funktionsmember verwendet werden, der andernfalls implizit `virtual`wäre.</span><span class="sxs-lookup"><span data-stu-id="cdf65-145">The `virtual` modifier may be used on a function member that would otherwise be implicitly `virtual`.</span></span> <span data-ttu-id="cdf65-146">Auch wenn `abstract` der Standardwert für Schnittstellenmember ohne Text ist, kann dieser Modifizierer explizit angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-146">Similarly, although `abstract` is the default on interface members without bodies, that modifier may be given explicitly.</span></span> <span data-ttu-id="cdf65-147">Ein nicht virtuelles Member kann mit dem `sealed`-Schlüsselwort deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-147">A non-virtual member may be declared using the `sealed` keyword.</span></span>

<span data-ttu-id="cdf65-148">Es ist ein Fehler bei einem `private` oder `sealed` Funktionsmember einer Schnittstelle, dass kein Text vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-148">It is an error for a `private` or `sealed` function member of an interface to have no body.</span></span> <span data-ttu-id="cdf65-149">Ein `private` Funktionsmember verfügt möglicherweise nicht über den-Modifizierer `sealed`.</span><span class="sxs-lookup"><span data-stu-id="cdf65-149">A `private` function member may not have the modifier `sealed`.</span></span>

<span data-ttu-id="cdf65-150">Zugriffsmodifizierer können für Schnittstellenmember aller zulässigen Member verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-150">Access modifiers may be used on interface members of all kinds of members that are permitted.</span></span> <span data-ttu-id="cdf65-151">Die Zugriffsebene `public` ist die Standardeinstellung, kann aber explizit angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-151">The access level `public` is the default but it may be given explicitly.</span></span>

> <span data-ttu-id="cdf65-152">***Problem öffnen:*** Wir müssen die genaue Bedeutung der Zugriffsmodifizierer angeben, z. b. `protected` und `internal`. Außerdem müssen Sie angeben, welche Deklarationen Sie (in einer abgeleiteten Schnittstelle) überschreiben oder implementieren (in einer Klasse, die die-Schnittstelle implementiert).</span><span class="sxs-lookup"><span data-stu-id="cdf65-152">***Open Issue:*** We need to specify the precise meaning of the access modifiers such as `protected` and `internal`, and which declarations do and do not override them (in a derived interface) or implement them (in a class that implements the interface).</span></span>

<span data-ttu-id="cdf65-153">Schnittstellen können `static` Member deklarieren, einschließlich der Typen, Methoden, Indexer, Eigenschaften, Ereignisse und statischen Konstruktoren.</span><span class="sxs-lookup"><span data-stu-id="cdf65-153">Interfaces may declare `static` members, including nested types, methods, indexers, properties, events, and static constructors.</span></span> <span data-ttu-id="cdf65-154">Die Standard Zugriffsebene für alle Schnittstellenmember ist `public`.</span><span class="sxs-lookup"><span data-stu-id="cdf65-154">The default access level for all interface members is `public`.</span></span>

<span data-ttu-id="cdf65-155">Schnittstellen können keine Instanzkonstruktoren, Dekonstruktoren oder Felder deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cdf65-155">Interfaces may not declare instance constructors, destructors, or fields.</span></span>

> <span data-ttu-id="cdf65-156">***Geschlossene Probleme:*** Sollten Operator Deklarationen in einer Schnittstelle zulässig sein?</span><span class="sxs-lookup"><span data-stu-id="cdf65-156">***Closed Issue:*** Should operator declarations be permitted in an interface?</span></span> <span data-ttu-id="cdf65-157">Wahrscheinlich keine Konvertierungs Operatoren, aber was ist mit anderen?</span><span class="sxs-lookup"><span data-stu-id="cdf65-157">Probably not conversion operators, but what about others?</span></span> <span data-ttu-id="cdf65-158">***Entscheidung***: Operatoren sind *mit Ausnahme* von Konvertierungs-, Gleichheits-und Ungleichheits Operatoren zulässig.</span><span class="sxs-lookup"><span data-stu-id="cdf65-158">***Decision***: Operators are permitted *except* for conversion, equality, and inequality operators.</span></span>

> <span data-ttu-id="cdf65-159">***Geschlossene Probleme:*** Sollten `new` für Schnittstellenmember-Deklarationen zulässig sein, die Member von Basis Schnittstellen ausblenden?</span><span class="sxs-lookup"><span data-stu-id="cdf65-159">***Closed Issue:*** Should `new` be permitted on interface member declarations that hide members from base interfaces?</span></span> <span data-ttu-id="cdf65-160">***Entscheidung***: Ja.</span><span class="sxs-lookup"><span data-stu-id="cdf65-160">***Decision***: Yes.</span></span>

> <span data-ttu-id="cdf65-161">***Geschlossene Probleme:*** Wir gestatten derzeit keine `partial` auf einer Schnittstelle oder ihren Membern.</span><span class="sxs-lookup"><span data-stu-id="cdf65-161">***Closed Issue:*** We do not currently permit `partial` on an interface or its members.</span></span> <span data-ttu-id="cdf65-162">Dies erfordert einen separaten Vorschlag.</span><span class="sxs-lookup"><span data-stu-id="cdf65-162">That would require a separate proposal.</span></span> <span data-ttu-id="cdf65-163">***Entscheidung***: Ja.</span><span class="sxs-lookup"><span data-stu-id="cdf65-163">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a><span data-ttu-id="cdf65-164">Über Schreibungen in Schnittstellen</span><span class="sxs-lookup"><span data-stu-id="cdf65-164">Overrides in interfaces</span></span>

<span data-ttu-id="cdf65-165">Überschreibungs Deklarationen (d. h. solche, die den `override` Modifizierer enthalten) ermöglichen es dem Programmierer, eine spezifischere Implementierung eines virtuellen Members in einer Schnittstelle bereitzustellen, auf der der Compiler oder die Laufzeit andernfalls keinen finden würde.</span><span class="sxs-lookup"><span data-stu-id="cdf65-165">Override declarations (i.e. those containing the `override` modifier) allow the programmer to provide a most specific implementation of a virtual member in an interface where the compiler or runtime would not otherwise find one.</span></span> <span data-ttu-id="cdf65-166">Außerdem ermöglicht es die Umwandlung eines abstrakten Members aus einer Super Schnittstelle in ein Standardelement in einer abgeleiteten Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="cdf65-166">It also allows turning an abstract member from a super-interface into a default member in a derived interface.</span></span> <span data-ttu-id="cdf65-167">Eine Überschreibungs Deklaration ist berechtigt, eine bestimmte Basis Schnittstellen Methode *explizit* zu überschreiben, indem Sie die Deklaration mit dem Schnittstellennamen qualifiziert (in diesem Fall ist kein Zugriffsmodifizierer zulässig).</span><span class="sxs-lookup"><span data-stu-id="cdf65-167">An override declaration is permitted to *explicitly* override a particular base interface method by qualifying the declaration with the interface name (no access modifier is permitted in this case).</span></span> <span data-ttu-id="cdf65-168">Implizite über schreibungen sind nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="cdf65-168">Implicit overrides are not permitted.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

<span data-ttu-id="cdf65-169">Überschreibungs Deklarationen in Schnittstellen können nicht als `sealed`deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-169">Override declarations in interfaces may not be declared `sealed`.</span></span>

<span data-ttu-id="cdf65-170">Öffentliche `virtual` Funktionsmember in einer Schnittstelle können in einer abgeleiteten Schnittstelle explizit überschrieben werden (indem Sie den Namen in der Überschreibungs Deklaration mit dem Schnittstellentyp qualifizieren, der die Methode ursprünglich deklariert hat, und einen Zugriffsmodifizierer weggelassen).</span><span class="sxs-lookup"><span data-stu-id="cdf65-170">Public `virtual` function members in an interface may be overridden in a derived interface explicitly (by qualifying the name in the override declaration with the interface type that originally declared the method, and omitting an access modifier).</span></span>

<span data-ttu-id="cdf65-171">`virtual` Funktionsmember in einer Schnittstelle können nur explizit (nicht implizit) in abgeleiteten Schnittstellen überschrieben werden, und Member, die nicht `public` sind, können nur in einer Klasse oder Struktur explizit (nicht implizit) implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-171">`virtual` function members in an interface may only be overridden explicitly (not implicitly) in derived interfaces, and members that are not `public` may only be implemented in a class or struct explicitly (not implicitly).</span></span> <span data-ttu-id="cdf65-172">In beiden Fällen muss auf den überschriebenen oder implementierten Member zugegriffen werden können *, wo er* überschrieben wurde.</span><span class="sxs-lookup"><span data-stu-id="cdf65-172">In either case, the overridden or implemented member must be *accessible* where it is overridden.</span></span>

### <a name="reabstraction"></a><span data-ttu-id="cdf65-173">Neuabstraktion</span><span class="sxs-lookup"><span data-stu-id="cdf65-173">Reabstraction</span></span>

<span data-ttu-id="cdf65-174">Eine in einer Schnittstelle deklarierte virtuelle (konkrete) Methode kann in einer abgeleiteten Schnittstelle als abstrakt überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-174">A virtual (concrete) method declared in an interface may be overridden to be abstract in a derived interface</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

<span data-ttu-id="cdf65-175">Der `abstract` Modifizierer ist in der Deklaration von `IB.M` nicht erforderlich (Dies ist die Standardeinstellung in-Schnittstellen), aber es ist wahrscheinlich empfehlenswert, explizit in einer Überschreibungs Deklaration zu sein.</span><span class="sxs-lookup"><span data-stu-id="cdf65-175">The `abstract` modifier is not required in the declaration of `IB.M` (that is the default in interfaces), but it is probably good practice to be explicit in an override declaration.</span></span>

<span data-ttu-id="cdf65-176">Dies ist in abgeleiteten Schnittstellen nützlich, bei denen die Standard Implementierung einer Methode ungeeignet ist und eine geeignetere Implementierung durch Implementieren von Klassen bereitgestellt werden sollte.</span><span class="sxs-lookup"><span data-stu-id="cdf65-176">This is useful in derived interfaces where the default implementation of a method is inappropriate and a more appropriate implementation should be provided by implementing classes.</span></span>

> <span data-ttu-id="cdf65-177">***Problem öffnen:*** Sollte eine erneute Abstraktion zulässig sein?</span><span class="sxs-lookup"><span data-stu-id="cdf65-177">***Open Issue:*** Should reabstraction be permitted?</span></span>

### <a name="the-most-specific-override-rule"></a><span data-ttu-id="cdf65-178">Die spezifischsten Überschreibungs Regel</span><span class="sxs-lookup"><span data-stu-id="cdf65-178">The most specific override rule</span></span>

<span data-ttu-id="cdf65-179">Wir verlangen, dass jede Schnittstelle und Klasse über eine *spezifischere außer Kraft* setzung für jedes virtuelle Element unter den außer Kraft setzungen verfügen, die im Typ oder seinen direkten und indirekten Schnittstellen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-179">We require that every interface and class have a *most specific override* for every virtual member among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="cdf65-180">Die *spezifischere außer Kraft* Setzung ist eine eindeutige außer Kraft setzung, die spezifischer ist als jede andere außer Kraft Setzung.</span><span class="sxs-lookup"><span data-stu-id="cdf65-180">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="cdf65-181">Wenn keine außer Kraft Setzung vorhanden ist, wird der Member selbst als die spezifischsten außer Kraft Setzung betrachtet.</span><span class="sxs-lookup"><span data-stu-id="cdf65-181">If there is no override, the member itself is considered the most specific override.</span></span>

<span data-ttu-id="cdf65-182">Eine außer Kraft Setzung `M1` gilt als *spezifischere* als eine andere außer Kraft Setzung `M2` wenn `M1` für den Typ `T1`deklariert ist, `M2` für den Typ `T2`deklariert ist, und entweder</span><span class="sxs-lookup"><span data-stu-id="cdf65-182">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>

1. <span data-ttu-id="cdf65-183">`T1` enthält `T2` zu den direkten oder indirekten Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-183">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
2. <span data-ttu-id="cdf65-184">`T2` ist ein Schnittstellentyp, aber `T1` ist kein Schnittstellentyp.</span><span class="sxs-lookup"><span data-stu-id="cdf65-184">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="cdf65-185">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cdf65-185">For example:</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

<span data-ttu-id="cdf65-186">Die spezifischere Überschreibungs Regel stellt sicher, dass ein Konflikt (d. h. eine Mehrdeutigkeit durch die rautenvererbung) explizit vom Programmierer an dem Punkt aufgelöst wird, an dem der Konflikt auftritt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-186">The most specific override rule ensures that a conflict (i.e. an ambiguity arising from diamond inheritance) is resolved explicitly by the programmer at the point where the conflict arises.</span></span>

<span data-ttu-id="cdf65-187">Da wir explizite abstrakte über Schreibungen in Schnittstellen unterstützen, können wir dies auch in Klassen tun.</span><span class="sxs-lookup"><span data-stu-id="cdf65-187">Because we support explicit abstract overrides in interfaces, we could do so in classes as well</span></span>

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> <span data-ttu-id="cdf65-188">***Open Issue***: sollten explizite Schnittstellen abstrakte über Schreibungen in Klassen unterstützt werden?</span><span class="sxs-lookup"><span data-stu-id="cdf65-188">***Open issue***: should we support explicit interface abstract overrides in classes?</span></span>

<span data-ttu-id="cdf65-189">Außerdem ist es ein Fehler, wenn in einer Klassen Deklaration die spezifischere außer Kraft Setzung einer Schnittstellen Methode eine abstrakte außer Kraft Setzung ist, die in einer Schnittstelle deklariert wurde.</span><span class="sxs-lookup"><span data-stu-id="cdf65-189">In addition, it is an error if in a class declaration the most specific override of some interface method is an abstract override that was declared in an interface.</span></span> <span data-ttu-id="cdf65-190">Dies ist eine vorhandene Regel, die mithilfe der neuen Terminologie neu angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="cdf65-190">This is an existing rule restated using the new terminology.</span></span>

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

<span data-ttu-id="cdf65-191">Eine in einer Schnittstelle deklarierte virtuelle Eigenschaft kann eine spezifischere außer Kraft setzung für den `get` Accessor in einer Schnittstelle und eine spezifischere außer Kraft setzung für den `set` Accessor in einer anderen Schnittstelle aufweisen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-191">It is possible for a virtual property declared in an interface to have a most specific override for its `get` accessor in one interface and a most specific override for its `set` accessor in a different interface.</span></span> <span data-ttu-id="cdf65-192">Dies wird als Verstoß gegen die *spezifischsten Überschreibungs* Regel angesehen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-192">This is considered a violation of the *most specific override* rule.</span></span>

### <a name="static-and-private-methods"></a><span data-ttu-id="cdf65-193">Die Methoden `static` und `private`</span><span class="sxs-lookup"><span data-stu-id="cdf65-193">`static` and `private` methods</span></span>

<span data-ttu-id="cdf65-194">Da Schnittstellen nun ausführbaren Code enthalten können, ist es hilfreich, allgemeinen Code in private und statische Methoden zu abstrahieren.</span><span class="sxs-lookup"><span data-stu-id="cdf65-194">Because interfaces may now contain executable code, it is useful to abstract common code into private and static methods.</span></span> <span data-ttu-id="cdf65-195">Diese werden jetzt in Schnittstellen zugelassen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-195">We now permit these in interfaces.</span></span>

> <span data-ttu-id="cdf65-196">***Geschlossenes Problem***: sollen private Methoden unterstützt werden?</span><span class="sxs-lookup"><span data-stu-id="cdf65-196">***Closed issue***: Should we support private methods?</span></span> <span data-ttu-id="cdf65-197">Sollten statische Methoden unterstützt werden?</span><span class="sxs-lookup"><span data-stu-id="cdf65-197">Should we support static methods?</span></span> <span data-ttu-id="cdf65-198">**Entscheidung: Ja**</span><span class="sxs-lookup"><span data-stu-id="cdf65-198">**Decision: YES**</span></span>

> <span data-ttu-id="cdf65-199">***Open Issue***: sollen Schnittstellen Methoden `protected` oder `internal` oder andere Zugriffsberechtigungen zugelassen werden?</span><span class="sxs-lookup"><span data-stu-id="cdf65-199">***Open issue***: should we permit interface methods to be `protected` or `internal` or other access?</span></span> <span data-ttu-id="cdf65-200">Wenn dies der Fall ist, wie lautet die Semantik?</span><span class="sxs-lookup"><span data-stu-id="cdf65-200">If so, what are the semantics?</span></span> <span data-ttu-id="cdf65-201">Sind Sie standardmäßig `virtual`?</span><span class="sxs-lookup"><span data-stu-id="cdf65-201">Are they `virtual` by default?</span></span> <span data-ttu-id="cdf65-202">Wenn dies der Fall ist, gibt es eine Möglichkeit, Sie als nicht virtuell zu gestalten?</span><span class="sxs-lookup"><span data-stu-id="cdf65-202">If so, is there a way to make them non-virtual?</span></span>

> <span data-ttu-id="cdf65-203">***Open Issue***: Wenn statische Methoden unterstützt werden, sollten (statische) Operatoren unterstützt werden?</span><span class="sxs-lookup"><span data-stu-id="cdf65-203">***Open issue***: If we support static methods, should we support (static) operators?</span></span>

### <a name="base-interface-invocations"></a><span data-ttu-id="cdf65-204">Basis Schnittstellen Aufrufe</span><span class="sxs-lookup"><span data-stu-id="cdf65-204">Base interface invocations</span></span>

<span data-ttu-id="cdf65-205">Code in einem Typ, der von einer Schnittstelle mit einer Standardmethode abgeleitet ist, kann die "Base"-Implementierung dieser Schnittstelle explizit aufrufen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-205">Code in a type that derives from an interface with a default method can explicitly invoke that interface's "base" implementation.</span></span>

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

<span data-ttu-id="cdf65-206">Eine Instanzmethode (nicht statisch) kann die Implementierung einer zugänglichen Instanzmethode in einer direkten Basisschnittstelle nicht virtuell aufrufen, indem Sie Sie mit der Syntax `base(Type).M`benennt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-206">An instance (nonstatic) method is permitted to invoke the implementation of an accessible instance method in a direct base interface nonvirtually by naming it using the syntax `base(Type).M`.</span></span> <span data-ttu-id="cdf65-207">Dies ist hilfreich, wenn eine außer Kraft setzung, die aufgrund der rautenvererbung bereitgestellt werden muss, durch Delegieren an eine bestimmte Basis Implementierung aufgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="cdf65-207">This is useful when an override that is required to be provided due to diamond inheritance is resolved by delegating to one particular base implementation.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

<span data-ttu-id="cdf65-208">Wenn auf eine `virtual` oder `abstract` Member mithilfe der Syntax `base(Type).M`zugegriffen wird, ist es erforderlich, dass `Type` eine eindeutige *außer Kraft* setzung für `M`enthält.</span><span class="sxs-lookup"><span data-stu-id="cdf65-208">When a `virtual` or `abstract` member is accessed using the syntax `base(Type).M`, it is required that `Type` contains a unique *most specific override* for `M`.</span></span>

### <a name="binding-base-clauses"></a><span data-ttu-id="cdf65-209">Bindungs Basis Klauseln</span><span class="sxs-lookup"><span data-stu-id="cdf65-209">Binding base clauses</span></span>

<span data-ttu-id="cdf65-210">Schnittstellen enthalten nun Typen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-210">Interfaces now contain types.</span></span>  <span data-ttu-id="cdf65-211">Diese Typen können in der Base-Klausel als Basis Schnittstellen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-211">These types may be used in the base clause as base interfaces.</span></span>  <span data-ttu-id="cdf65-212">Beim Binden einer Basis Klausel müssen wir möglicherweise den Satz von Basis Schnittstellen kennen, um diese Typen zu binden (z. b. um Nachrichten zu suchen und geschützten Zugriff aufzulösen).</span><span class="sxs-lookup"><span data-stu-id="cdf65-212">When binding a base clause, we may need to know the set of base interfaces to bind those types (e.g. to lookup in them and to resolve protected access).</span></span>  <span data-ttu-id="cdf65-213">Die Bedeutung der Basis Klausel einer Schnittstelle wird daher zirkulär definiert.</span><span class="sxs-lookup"><span data-stu-id="cdf65-213">The meaning of an interface's base clause is thus circularly defined.</span></span>  <span data-ttu-id="cdf65-214">Um den Kreis zu unterbrechen, fügen wir eine neue Sprachregel hinzu, die einer ähnlichen Regel entspricht, die für Klassen bereits vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-214">To break the cycle, we add a new language rules corresponding to a similar rule already in place for classes.</span></span>

<span data-ttu-id="cdf65-215">Beim Ermitteln der Bedeutung des *interface_base* einer Schnittstelle wird davon ausgegangen, dass die Basis Schnittstellen temporär leer sind.</span><span class="sxs-lookup"><span data-stu-id="cdf65-215">While determining the meaning of the *interface_base* of an interface, the base interfaces are temporarily assumed to be empty.</span></span> <span data-ttu-id="cdf65-216">Intuitiv wird dadurch sichergestellt, dass die Bedeutung einer Basis Klausel nicht rekursiv von sich selbst abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-216">Intuitively this ensures that the meaning of a base clause cannot recursively depend on itself.</span></span> 

<span data-ttu-id="cdf65-217">**Wir haben die folgenden Regeln verwendet:**</span><span class="sxs-lookup"><span data-stu-id="cdf65-217">**We used to have the following rules:**</span></span>

<span data-ttu-id="cdf65-218">"Wenn eine Klasse B von einer Klasse a abgeleitet ist, ist dies ein Kompilierzeitfehler für eine, die von B abhängig ist. Eine Klasse **hängt direkt** von ihrer direkten Basisklasse (sofern vorhanden) ab und **hängt direkt** von der ~~**Klasse**~~ ab, in der Sie sofort geschachtelt ist (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="cdf65-218">"When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the ~~**class**~~ within which it is immediately nested (if any).</span></span> <span data-ttu-id="cdf65-219">Bei dieser Definition ist der gesamte Satz von ~~**Klassen**~~ , von dem eine Klasse abhängt, die reflexive und transitiv Schließung der **direkt** von der Beziehung abhängig. "</span><span class="sxs-lookup"><span data-stu-id="cdf65-219">Given this definition, the complete set of ~~**classes**~~ upon which a class depends is the reflexive and transitive closure of the **directly depends on** relationship."</span></span>

<span data-ttu-id="cdf65-220">Es ist ein Kompilierzeitfehler für eine Schnittstelle, die direkt oder indirekt von sich selbst erbt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-220">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>
<span data-ttu-id="cdf65-221">Die **Basis Schnittstellen** einer Schnittstelle sind die expliziten Basis Schnittstellen und deren Basis Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-221">The **base interfaces** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="cdf65-222">Mit anderen Worten: der Satz von Basis Schnittstellen ist die komplette transitiv Schließung der expliziten Basis Schnittstellen, ihrer expliziten Basis Schnittstellen usw.</span><span class="sxs-lookup"><span data-stu-id="cdf65-222">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span>

<span data-ttu-id="cdf65-223">**Wir passen Sie wie folgt an:**</span><span class="sxs-lookup"><span data-stu-id="cdf65-223">**We are adjusting them as follows:**</span></span>

<span data-ttu-id="cdf65-224">Wenn eine Klasse B von einer Klasse a abgeleitet ist, ist dies ein Kompilierzeitfehler für eine, die von B abhängig ist. Eine Klasse **hängt direkt** von ihrer direkten Basisklasse (sofern vorhanden) ab und **hängt direkt** von dem _**Typ**_ ab, in dem Sie sofort geschachtelt ist (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="cdf65-224">When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the _**type**_ within which it is immediately nested (if any).</span></span>

<span data-ttu-id="cdf65-225">Wenn ein Interface-IB eine Interface IA erweitert, handelt es sich um einen Kompilierzeitfehler, damit IA von IB abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-225">When an interface IB extends an interface IA, it is a compile-time error for IA to depend on IB.</span></span> <span data-ttu-id="cdf65-226">Eine Schnittstelle **hängt direkt** von ihren direkten Basis Schnittstellen (sofern vorhanden) ab und **hängt direkt** von dem Typ ab, in dem Sie sofort geschachtelt ist (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="cdf65-226">An interface **directly depends on** its direct base interfaces (if any) and **directly depends on** the type within which it is immediately nested (if any).</span></span>

<span data-ttu-id="cdf65-227">Bei diesen Definitionen ist der gesamte Satz von **Typen** , von dem ein Typ abhängt, der reflexive und transitiv Abschluss der **direkt** von Beziehung abhängig.</span><span class="sxs-lookup"><span data-stu-id="cdf65-227">Given these definitions, the complete set of **types** upon which a type depends is the reflexive and transitive closure of the **directly depends on** relationship.</span></span>

### <a name="effect-on-existing-programs"></a><span data-ttu-id="cdf65-228">Auswirkung auf vorhandene Programme</span><span class="sxs-lookup"><span data-stu-id="cdf65-228">Effect on existing programs</span></span>

<span data-ttu-id="cdf65-229">Die hier vorgestellten Regeln sollen keine Auswirkung auf die Bedeutung vorhandener Programme haben.</span><span class="sxs-lookup"><span data-stu-id="cdf65-229">The rules presented here are intended to have no effect on the meaning of existing programs.</span></span>

<span data-ttu-id="cdf65-230">Beispiel 1:</span><span class="sxs-lookup"><span data-stu-id="cdf65-230">Example 1:</span></span>

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

<span data-ttu-id="cdf65-231">Beispiel 2:</span><span class="sxs-lookup"><span data-stu-id="cdf65-231">Example 2:</span></span>

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

<span data-ttu-id="cdf65-232">Die gleichen Regeln führen zu ähnlichen Ergebnissen wie bei den Standardschnittstellen Methoden:</span><span class="sxs-lookup"><span data-stu-id="cdf65-232">The same rules give similar results to the analogous situation involving default interface methods:</span></span>

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> <span data-ttu-id="cdf65-233">***Geschlossenes Problem***: Vergewissern Sie sich, dass dies eine beabsichtigte Folge der Spezifikation ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-233">***Closed issue***: confirm that this is an intended consequence of the specification.</span></span> <span data-ttu-id="cdf65-234">**Entscheidung: Ja**</span><span class="sxs-lookup"><span data-stu-id="cdf65-234">**Decision: YES**</span></span>

### <a name="runtime-method-resolution"></a><span data-ttu-id="cdf65-235">Auflösung der Lauf Zeit Methode</span><span class="sxs-lookup"><span data-stu-id="cdf65-235">Runtime method resolution</span></span>

> <span data-ttu-id="cdf65-236">***Geschlossene Probleme:*** Die Spezifikation sollte den Algorithmus zur Auflösung der Lauf Zeit Methode im Gesicht der Standardmethoden der Schnittstelle beschreiben.</span><span class="sxs-lookup"><span data-stu-id="cdf65-236">***Closed Issue:*** The spec should describe the runtime method resolution algorithm in the face of interface default methods.</span></span> <span data-ttu-id="cdf65-237">Wir müssen sicherstellen, dass die Semantik mit der sprach Semantik konsistent ist, z. b. welche deklarierten Methoden dies tun und keine `internal` Methode überschreiben oder implementieren.</span><span class="sxs-lookup"><span data-stu-id="cdf65-237">We need to ensure that the semantics are consistent with the language semantics, e.g. which declared methods do and do not override or implement an `internal` method.</span></span>

### <a name="clr-support-api"></a><span data-ttu-id="cdf65-238">CLR-Support-API</span><span class="sxs-lookup"><span data-stu-id="cdf65-238">CLR support API</span></span>

<span data-ttu-id="cdf65-239">Damit Compiler erkennen können, dass Sie für eine Laufzeit kompiliert werden, die diese Funktion unterstützt, werden Bibliotheken für diese Laufzeiten so geändert, dass diese Fakten über die in <https://github.com/dotnet/corefx/issues/17116>erörterte API angekündigt werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-239">In order for compilers to detect when they are compiling for a runtime that supports this feature, libraries for such runtimes are modified to advertise that fact through the API discussed in <https://github.com/dotnet/corefx/issues/17116>.</span></span> <span data-ttu-id="cdf65-240">Wir fügen</span><span class="sxs-lookup"><span data-stu-id="cdf65-240">We add</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> <span data-ttu-id="cdf65-241">***Open Issue***: ist der beste Name für die *CLR* -Funktion?</span><span class="sxs-lookup"><span data-stu-id="cdf65-241">***Open issue***: Is that the best name for the *CLR* feature?</span></span> <span data-ttu-id="cdf65-242">Die CLR-Funktion ist viel mehr als nur das (z. b. lockern von Schutz Einschränkungen, unterstützt über Schreibungen in Schnittstellen usw.).</span><span class="sxs-lookup"><span data-stu-id="cdf65-242">The CLR feature does much more than just that (e.g. relaxes protection constraints, supports overrides in interfaces, etc).</span></span> <span data-ttu-id="cdf65-243">Vielleicht sollte Sie z. b. "konkrete Methoden in Schnittstellen" oder "Merkmale" genannt werden?</span><span class="sxs-lookup"><span data-stu-id="cdf65-243">Perhaps it should be called something like "concrete methods in interfaces", or "traits"?</span></span>

### <a name="further-areas-to-be-specified"></a><span data-ttu-id="cdf65-244">Weitere Bereiche, die angegeben werden sollen</span><span class="sxs-lookup"><span data-stu-id="cdf65-244">Further areas to be specified</span></span>

- <span data-ttu-id="cdf65-245">[] Es ist hilfreich, die Arten von Quell-und Binär Kompatibilitäts Effekten zu katalogisieren, die durch das Hinzufügen von Standardschnittstellen Methoden und über schreibungen zu vorhandenen Schnittstellen verursacht werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-245">[ ] It would be useful to catalog the kinds of source and binary compatibility effects caused by adding default interface methods and overrides to existing interfaces.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="cdf65-246">Nachteile</span><span class="sxs-lookup"><span data-stu-id="cdf65-246">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="cdf65-247">Dieser Vorschlag erfordert ein koordiniertes Update der CLR-Spezifikation (zur Unterstützung konkreter Methoden in Schnittstellen und Methoden Auflösung).</span><span class="sxs-lookup"><span data-stu-id="cdf65-247">This proposal requires a coordinated update to the CLR specification (to support concrete methods in interfaces and method resolution).</span></span> <span data-ttu-id="cdf65-248">Es ist daher ziemlich "teuer", und es kann sinnvoll sein, in Kombination mit anderen Features, die wir erwarten, CLR-Änderungen zu erfordern.</span><span class="sxs-lookup"><span data-stu-id="cdf65-248">It is therefore fairly "expensive" and it may be worth doing in combination with other features that we also anticipate would require CLR changes.</span></span>

## <a name="alternatives"></a><span data-ttu-id="cdf65-249">Alternativen</span><span class="sxs-lookup"><span data-stu-id="cdf65-249">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="cdf65-250">None.</span><span class="sxs-lookup"><span data-stu-id="cdf65-250">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="cdf65-251">Nicht aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="cdf65-251">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="cdf65-252">Offene Fragen werden im obigen Vorschlag genannt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-252">Open questions are called out throughout the proposal, above.</span></span>
- <span data-ttu-id="cdf65-253">Eine Liste der offenen Fragen finden Sie unter <https://github.com/dotnet/csharplang/issues/406>.</span><span class="sxs-lookup"><span data-stu-id="cdf65-253">See also <https://github.com/dotnet/csharplang/issues/406> for a list of open questions.</span></span>
- <span data-ttu-id="cdf65-254">Die ausführliche Spezifikation muss den Auflösungs Mechanismus beschreiben, der zur Laufzeit verwendet wird, um die genaue Methode auszuwählen, die aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="cdf65-254">The detailed specification must describe the resolution mechanism used at runtime to select the precise method to be invoked.</span></span>
- <span data-ttu-id="cdf65-255">Die Interaktion von Metadaten, die von neuen Compilern erzeugt und von älteren Compilern genutzt werden, muss ausführlich verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-255">The interaction of metadata produced by new compilers and consumed by older compilers needs to be worked out in detail.</span></span> <span data-ttu-id="cdf65-256">Beispielsweise müssen wir sicherstellen, dass die von uns verwendete Metadatendarstellung nicht bewirkt, dass eine Standard Implementierung in einer Schnittstelle hinzugefügt wird, um eine vorhandene Klasse zu unterbrechen, die diese Schnittstelle implementiert, wenn Sie von einem älteren Compiler kompiliert wird.</span><span class="sxs-lookup"><span data-stu-id="cdf65-256">For example, we need to ensure that the metadata representation that we use does not cause the addition of a default implementation in an interface to break an existing class that implements that interface when compiled by an older compiler.</span></span> <span data-ttu-id="cdf65-257">Dies kann sich auf die Metadatendarstellung auswirken, die wir verwenden können.</span><span class="sxs-lookup"><span data-stu-id="cdf65-257">This may affect the metadata representation that we can use.</span></span>
- <span data-ttu-id="cdf65-258">Der Entwurf muss die Interoperation mit anderen Sprachen und vorhandenen Compilern für andere Sprachen in Erwägung gezogen werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-258">The design must consider interoperation with other languages and existing compilers for other languages.</span></span>

## <a name="resolved-questions"></a><span data-ttu-id="cdf65-259">Aufgelöste Fragen</span><span class="sxs-lookup"><span data-stu-id="cdf65-259">Resolved Questions</span></span>

### <a name="abstract-override"></a><span data-ttu-id="cdf65-260">Abstrakte außer Kraft Setzung</span><span class="sxs-lookup"><span data-stu-id="cdf65-260">Abstract Override</span></span>

<span data-ttu-id="cdf65-261">Die vorherige Entwurfs Spezifikation enthielt die Möglichkeit, eine geerbte Methode "reabstract" zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="cdf65-261">The earlier draft spec contained the ability to "reabstract" an inherited method:</span></span>

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

<span data-ttu-id="cdf65-262">Meine Notizen für 2017-03-20 haben ergeben, dass wir dies nicht zulassen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-262">My notes for 2017-03-20 showed that we decided not to allow this.</span></span> <span data-ttu-id="cdf65-263">Hierfür gibt es jedoch mindestens zwei Anwendungsfälle:</span><span class="sxs-lookup"><span data-stu-id="cdf65-263">However, there are at least two use cases for it:</span></span>

1. <span data-ttu-id="cdf65-264">Die Java-APIs, mit denen einige Benutzer dieses Features zusammenarbeiten, sind von dieser Funktion abhängig.</span><span class="sxs-lookup"><span data-stu-id="cdf65-264">The Java APIs, with which some users of this feature hope to interoperate, depend on this facility.</span></span>
2. <span data-ttu-id="cdf65-265">Die Programmierung mit *Merkmalen* profitiert davon.</span><span class="sxs-lookup"><span data-stu-id="cdf65-265">Programming with *traits* benefits from this.</span></span> <span data-ttu-id="cdf65-266">Reabstraktion ist eines der Elemente der Sprachfunktion "Merkmale" (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span><span class="sxs-lookup"><span data-stu-id="cdf65-266">Reabstraction is one of the elements of the "traits" language feature (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span></span> <span data-ttu-id="cdf65-267">Die folgenden Klassen sind zulässig:</span><span class="sxs-lookup"><span data-stu-id="cdf65-267">The following is permitted with classes:</span></span>

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

<span data-ttu-id="cdf65-268">Leider kann dieser Code nicht als Satz von Schnittstellen (Merkmalen) umgestaltet werden, es sei denn, dies ist zulässig.</span><span class="sxs-lookup"><span data-stu-id="cdf65-268">Unfortunately this code cannot be refactored as a set of interfaces (traits) unless this is permitted.</span></span> <span data-ttu-id="cdf65-269">Das *Jared-Prinzip der Gier*sollte zugelassen werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-269">By the *Jared principle of greed*, it should be permitted.</span></span>

> <span data-ttu-id="cdf65-270">***Geschlossene Probleme:*** Sollte eine erneute Abstraktion zulässig sein?</span><span class="sxs-lookup"><span data-stu-id="cdf65-270">***Closed issue:*** Should reabstraction be permitted?</span></span> <span data-ttu-id="cdf65-271">Zwar Meine Notizen waren falsch.</span><span class="sxs-lookup"><span data-stu-id="cdf65-271">[YES] My notes were wrong.</span></span> <span data-ttu-id="cdf65-272">Die [LDM bemerkt](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) , dass eine erneute Abstraktion in einer Schnittstelle zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-272">The [LDM notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) say that reabstraction is permitted in an interface.</span></span> <span data-ttu-id="cdf65-273">Nicht in einer Klasse.</span><span class="sxs-lookup"><span data-stu-id="cdf65-273">Not in a class.</span></span>

### <a name="virtual-modifier-vs-sealed-modifier"></a><span data-ttu-id="cdf65-274">Virtueller Modifizierer vs sealed-Modifizierer</span><span class="sxs-lookup"><span data-stu-id="cdf65-274">Virtual Modifier vs Sealed Modifier</span></span>

<span data-ttu-id="cdf65-275">Aus [Aleksey tsingauz](https://github.com/AlekseyTs):</span><span class="sxs-lookup"><span data-stu-id="cdf65-275">From [Aleksey Tsingauz](https://github.com/AlekseyTs):</span></span>

> <span data-ttu-id="cdf65-276">Es wurde beschlossen, Modifizierer explizit für Schnittstellenmember anzugeben, es sei denn, es gibt einen Grund dafür, einige davon zu unterbinden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-276">We decided to allow modifiers explicitly stated on interface members, unless there is a reason to disallow some of them.</span></span> <span data-ttu-id="cdf65-277">Dies bringt eine interessante Frage in Bezug auf den virtuellen Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="cdf65-277">This brings an interesting question around virtual modifier.</span></span> <span data-ttu-id="cdf65-278">Sollte es für Member mit Standard Implementierung erforderlich sein?</span><span class="sxs-lookup"><span data-stu-id="cdf65-278">Should it be required on members with default implementation?</span></span>
>
> <span data-ttu-id="cdf65-279">Wir könnten Folgendes sagen:</span><span class="sxs-lookup"><span data-stu-id="cdf65-279">We could say that:</span></span>
>
> - <span data-ttu-id="cdf65-280">Wenn keine Implementierung vorhanden ist und weder Virtual noch Sealed angegeben sind, wird davon ausgegangen, dass der Member abstrakt ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-280">if there is no implementation and neither virtual, nor sealed are specified, we assume the member is abstract.</span></span>
> - <span data-ttu-id="cdf65-281">Wenn eine Implementierung vorhanden ist und weder abstract noch Sealed angegeben sind, wird davon ausgegangen, dass der Member virtuell ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-281">if there is an implementation and neither abstract, nor sealed are specified, we assume the member is virtual.</span></span>
> - <span data-ttu-id="cdf65-282">der sealed-Modifizierer ist erforderlich, um eine Methode weder virtuell noch abstrakt zu machen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-282">sealed modifier is required to make a method neither virtual, nor abstract.</span></span>
>
> <span data-ttu-id="cdf65-283">Wir könnten auch sagen, dass der virtuelle Modifizierer für ein virtuelles Element erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-283">Alternatively, we could say that virtual modifier is required for a virtual member.</span></span> <span data-ttu-id="cdf65-284">Wenn also ein Member vorhanden ist, dessen Implementierung nicht explizit mit dem virtuellen Modifizierer markiert ist, ist es weder virtuell noch abstrakt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-284">I.e, if there is a member with implementation not explicitly marked with virtual modifier, it is neither virtual, nor abstract.</span></span> <span data-ttu-id="cdf65-285">Diese Vorgehensweise bietet möglicherweise eine bessere Leistung, wenn eine Methode von einer Klasse zu einer Schnittstelle verschoben wird:</span><span class="sxs-lookup"><span data-stu-id="cdf65-285">This approach might provide better experience when a method is moved from a class to an interface:</span></span>
>
> - <span data-ttu-id="cdf65-286">eine abstrakte Methode bleibt abstrakt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-286">an abstract method stays abstract.</span></span>
> - <span data-ttu-id="cdf65-287">eine virtuelle Methode bleibt virtuell.</span><span class="sxs-lookup"><span data-stu-id="cdf65-287">a virtual method stays virtual.</span></span>
> - <span data-ttu-id="cdf65-288">eine Methode ohne einen Modifizierer bleibt weder virtuell noch abstrakt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-288">a method without any modifier stays neither virtual, nor abstract.</span></span>
> - <span data-ttu-id="cdf65-289">der sealed-Modifizierer kann nicht auf eine Methode angewendet werden, die keine außer Kraft Setzung ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-289">sealed modifier cannot be applied to a method that is not an override.</span></span>
>
> <span data-ttu-id="cdf65-290">Was denkst du?</span><span class="sxs-lookup"><span data-stu-id="cdf65-290">What do you think?</span></span>

> <span data-ttu-id="cdf65-291">***Geschlossene Probleme:*** Sollte eine konkrete Methode (mit Implementierung) implizit `virtual`werden?</span><span class="sxs-lookup"><span data-stu-id="cdf65-291">***Closed Issue:*** Should a concrete method (with implementation) be implicitly `virtual`?</span></span> <span data-ttu-id="cdf65-292">Zwar</span><span class="sxs-lookup"><span data-stu-id="cdf65-292">[YES]</span></span>

<span data-ttu-id="cdf65-293">***Entscheidungen:*** Erstellt in LDM 2017-04-05:</span><span class="sxs-lookup"><span data-stu-id="cdf65-293">***Decisions:*** Made in the LDM 2017-04-05:</span></span>

1. <span data-ttu-id="cdf65-294">nicht virtuell sollte explizit durch `sealed` oder `private`ausgedrückt werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-294">non-virtual should be explicitly expressed through `sealed` or `private`.</span></span>
2. <span data-ttu-id="cdf65-295">`sealed` ist das Schlüsselwort, um schnittstelleninstanzmember mit nicht virtuellen Text Schnittstellen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-295">`sealed` is the keyword to make interface instance members with bodies non-virtual</span></span>
3. <span data-ttu-id="cdf65-296">Wir möchten alle modifiziererer in Schnittstellen zulassen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-296">We want to allow all modifiers in interfaces</span></span>  
4. <span data-ttu-id="cdf65-297">Der Standard Zugriff für Schnittstellenmember ist öffentlich, einschließlich der Untertypen</span><span class="sxs-lookup"><span data-stu-id="cdf65-297">Default accessibility for interface members is public, including nested types</span></span>
5. <span data-ttu-id="cdf65-298">Private Funktionsmember in Schnittstellen sind implizit versiegelt, und `sealed` ist für Sie nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="cdf65-298">private function members in interfaces are implicitly sealed, and `sealed` is not permitted on them.</span></span>
6. <span data-ttu-id="cdf65-299">Private Klassen (in Schnittstellen) sind zulässig und können versiegelt werden, und das bedeutet versiegelt in der Klassen Sense Sealed.</span><span class="sxs-lookup"><span data-stu-id="cdf65-299">Private classes (in interfaces) are permitted and can be sealed, and that means sealed in the class sense of sealed.</span></span>
7. <span data-ttu-id="cdf65-300">Wenn kein gutes Angebot vorhanden ist, ist partiell bei Schnittstellen oder ihren Membern weiterhin nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="cdf65-300">Absent a good proposal, partial is still not allowed on interfaces or their members.</span></span>

### <a name="binary-compatibility-1"></a><span data-ttu-id="cdf65-301">Binäre Kompatibilität 1</span><span class="sxs-lookup"><span data-stu-id="cdf65-301">Binary Compatibility 1</span></span>

<span data-ttu-id="cdf65-302">Wenn eine Bibliothek eine Standard Implementierung bereitstellt</span><span class="sxs-lookup"><span data-stu-id="cdf65-302">When a library provides a default implementation</span></span>

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

<span data-ttu-id="cdf65-303">Wir wissen, dass die Implementierung von `I1.M` in `C` `I1.M`ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-303">We understand that the implementation of `I1.M` in `C` is `I1.M`.</span></span> <span data-ttu-id="cdf65-304">Was geschieht, wenn die Assembly, die `I2` enthält, wie folgt geändert und neu kompiliert wird.</span><span class="sxs-lookup"><span data-stu-id="cdf65-304">What if the assembly containing `I2` is changed as follows and recompiled</span></span>

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

<span data-ttu-id="cdf65-305">`C` wird jedoch nicht neu kompiliert.</span><span class="sxs-lookup"><span data-stu-id="cdf65-305">but `C` is not recompiled.</span></span> <span data-ttu-id="cdf65-306">Was geschieht, wenn das Programm ausgeführt wird?</span><span class="sxs-lookup"><span data-stu-id="cdf65-306">What happens when the program is run?</span></span> <span data-ttu-id="cdf65-307">Ein Aufruf von `(C as I1).M()`</span><span class="sxs-lookup"><span data-stu-id="cdf65-307">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="cdf65-308">Führt `I1.M`</span><span class="sxs-lookup"><span data-stu-id="cdf65-308">Runs `I1.M`</span></span>
2. <span data-ttu-id="cdf65-309">Führt `I2.M`</span><span class="sxs-lookup"><span data-stu-id="cdf65-309">Runs `I2.M`</span></span>
3. <span data-ttu-id="cdf65-310">Löst einen Laufzeitfehler aus.</span><span class="sxs-lookup"><span data-stu-id="cdf65-310">Throws some kind of runtime error</span></span>

<span data-ttu-id="cdf65-311">***Entscheidung:*** 2017-04-11: führt `I2.M`aus, bei dem es sich um die eindeutig spezifischere außer Kraft Setzung zur Laufzeit handelt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-311">***Decision:*** Made 2017-04-11: Runs `I2.M`, which is the unambiguously most specific override at runtime.</span></span>

### <a name="event-accessors-closed"></a><span data-ttu-id="cdf65-312">Ereignisaccessoren (geschlossen)</span><span class="sxs-lookup"><span data-stu-id="cdf65-312">Event accessors (closed)</span></span>

> <span data-ttu-id="cdf65-313">***Geschlossene Probleme:*** Kann ein Ereignis "schrittweise" überschrieben werden?</span><span class="sxs-lookup"><span data-stu-id="cdf65-313">***Closed Issue:*** Can an event be overridden "piecewise"?</span></span>

<span data-ttu-id="cdf65-314">Beachten Sie diesen Fall:</span><span class="sxs-lookup"><span data-stu-id="cdf65-314">Consider this case:</span></span>

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

<span data-ttu-id="cdf65-315">Diese "partielle" Implementierung des Ereignisses ist unzulässig, da die Syntax für eine Ereignis Deklaration, wie in einer Klasse, nicht nur einen Accessor zulässt. Beide (oder keine) müssen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-315">This "partial" implementation of the event is not permitted because, as in a class, the syntax for an event declaration does not permit only one accessor; both (or neither) must be provided.</span></span> <span data-ttu-id="cdf65-316">Sie können das gleiche erreichen, indem Sie zulassen, dass der abstrakte remove-Accessor in der Syntax implizit abstrakt ist, wenn kein Text vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="cdf65-316">You could accomplish the same thing by permitting the abstract remove accessor in the syntax to be implicitly abstract by the absence of a body:</span></span>

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

<span data-ttu-id="cdf65-317">Beachten Sie, dass *es sich hierbei um eine neue (vorgeschlagene) Syntax handelt*.</span><span class="sxs-lookup"><span data-stu-id="cdf65-317">Note that *this is a new (proposed) syntax*.</span></span> <span data-ttu-id="cdf65-318">In der aktuellen Grammatik haben Ereignisaccessoren einen obligatorischen Text.</span><span class="sxs-lookup"><span data-stu-id="cdf65-318">In the current grammar, event accessors have a mandatory body.</span></span>

> <span data-ttu-id="cdf65-319">***Geschlossene Probleme:*** Kann ein Ereignis Accessor (implizit) durch das Weglassen eines Texts abstrahiert werden, ähnlich der Art, wie Methoden in Schnittstellen und Eigenschaftenaccessoren (implizit) durch das Weglassen eines Texts abstrakt sind?</span><span class="sxs-lookup"><span data-stu-id="cdf65-319">***Closed Issue:*** Can an event accessor be (implicitly) abstract by the omission of a body, similarly to the way that methods in interfaces and property accessors are (implicitly) abstract by the omission of a body?</span></span>

<span data-ttu-id="cdf65-320">***Entscheidung:*** (2017-04-18) Nein, Ereignis Deklarationen erfordern sowohl konkrete Accessoren (oder keines von beiden).</span><span class="sxs-lookup"><span data-stu-id="cdf65-320">***Decision:*** (2017-04-18) No, event declarations require both concrete accessors (or neither).</span></span>

### <a name="reabstraction-in-a-class-closed"></a><span data-ttu-id="cdf65-321">Neuabstraktion in einer Klasse (geschlossen)</span><span class="sxs-lookup"><span data-stu-id="cdf65-321">Reabstraction in a Class (closed)</span></span>

<span data-ttu-id="cdf65-322">***Geschlossene Probleme:*** Wir sollten bestätigen, dass dies zulässig ist (Andernfalls wäre das Hinzufügen einer Standard Implementierung eine Breaking Change):</span><span class="sxs-lookup"><span data-stu-id="cdf65-322">***Closed Issue:*** We should confirm that this is permitted (otherwise adding a default implementation would be a breaking change):</span></span>

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

<span data-ttu-id="cdf65-323">***Entscheidung:*** (2017-04-18) ja, das Hinzufügen eines Texts zu einer Schnittstellenmember-Deklaration sollte C nicht unterbrechen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-323">***Decision:*** (2017-04-18) Yes, adding a body to an interface member declaration shouldn't break C.</span></span>

### <a name="sealed-override-closed"></a><span data-ttu-id="cdf65-324">Versiegelte außer Kraft Setzung (geschlossen)</span><span class="sxs-lookup"><span data-stu-id="cdf65-324">Sealed Override (closed)</span></span>

<span data-ttu-id="cdf65-325">Die vorherige Frage geht implizit davon aus, dass der `sealed` Modifizierer auf eine `override` in einer Schnittstelle angewendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="cdf65-325">The previous question implicitly assumes that the `sealed` modifier can be applied to an `override` in an interface.</span></span> <span data-ttu-id="cdf65-326">Dies widerspricht der Entwurfs Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="cdf65-326">This contradicts the draft specification.</span></span> <span data-ttu-id="cdf65-327">Möchten wir das Versiegeln einer außer Kraft Setzung zulassen?</span><span class="sxs-lookup"><span data-stu-id="cdf65-327">Do we want to permit sealing an override?</span></span> <span data-ttu-id="cdf65-328">Die Auswirkungen auf die Quelle und die binäre Kompatibilität der Versiegelung sollten berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-328">Source and binary compatibility effects of sealing should be considered.</span></span>

> <span data-ttu-id="cdf65-329">***Geschlossene Probleme:*** Sollten wir das Versiegeln einer außer Kraft Setzung zulassen?</span><span class="sxs-lookup"><span data-stu-id="cdf65-329">***Closed Issue:*** Should we permit sealing an override?</span></span>

<span data-ttu-id="cdf65-330">***Entscheidung:*** (2017-04-18) erlauben Sie keine `sealed` für außer Kraft setzungen in Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-330">***Decision:*** (2017-04-18) Let's not allowed `sealed` on overrides in interfaces.</span></span> <span data-ttu-id="cdf65-331">Die einzige Verwendung von `sealed` für Schnittstellenmember ist, dass Sie in ihrer ursprünglichen Deklaration nicht virtuell sind.</span><span class="sxs-lookup"><span data-stu-id="cdf65-331">The only use of `sealed` on interface members is to make them non-virtual in their initial declaration.</span></span>

### <a name="diamond-inheritance-and-classes-closed"></a><span data-ttu-id="cdf65-332">Rautenvererbung und-Klassen (geschlossen)</span><span class="sxs-lookup"><span data-stu-id="cdf65-332">Diamond inheritance and classes (closed)</span></span>

<span data-ttu-id="cdf65-333">Der Entwurf des Angebots bevorzugt die Außerkraftsetzungs Überschreibungen von Klassen in rautenvererbungs Szenarien:</span><span class="sxs-lookup"><span data-stu-id="cdf65-333">The draft of the proposal prefers class overrides to interface overrides in diamond inheritance scenarios:</span></span>

> <span data-ttu-id="cdf65-334">Wir verlangen, dass jede Schnittstelle und Klasse eine *spezifischere außer Kraft* setzung für jede Schnittstellen Methode unter den außer Kraft setzungen aufweisen, die im Typ oder den direkten und indirekten Schnittstellen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-334">We require that every interface and class have a *most specific override* for every interface method among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="cdf65-335">Die *spezifischere außer Kraft* Setzung ist eine eindeutige außer Kraft setzung, die spezifischer ist als jede andere außer Kraft Setzung.</span><span class="sxs-lookup"><span data-stu-id="cdf65-335">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="cdf65-336">Wenn keine außer Kraft Setzung vorhanden ist, wird die Methode selbst als die spezifischere außer Kraft Setzung betrachtet.</span><span class="sxs-lookup"><span data-stu-id="cdf65-336">If there is no override, the method itself is considered the most specific override.</span></span>
>
> <span data-ttu-id="cdf65-337">Eine außer Kraft Setzung `M1` gilt als *spezifischere* als eine andere außer Kraft Setzung `M2` wenn `M1` für den Typ `T1`deklariert ist, `M2` für den Typ `T2`deklariert ist, und entweder</span><span class="sxs-lookup"><span data-stu-id="cdf65-337">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>
>
> 1. <span data-ttu-id="cdf65-338">`T1` enthält `T2` zu den direkten oder indirekten Schnittstellen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-338">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
> 2. <span data-ttu-id="cdf65-339">`T2` ist ein Schnittstellentyp, aber `T1` ist kein Schnittstellentyp.</span><span class="sxs-lookup"><span data-stu-id="cdf65-339">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="cdf65-340">Das Szenario ist</span><span class="sxs-lookup"><span data-stu-id="cdf65-340">The scenario is this</span></span>

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

<span data-ttu-id="cdf65-341">Wir sollten dieses Verhalten bestätigen (oder andernfalls festlegen).</span><span class="sxs-lookup"><span data-stu-id="cdf65-341">We should confirm this behavior (or decide otherwise)</span></span>

> <span data-ttu-id="cdf65-342">***Geschlossene Probleme:*** Bestätigen Sie die oben beschriebene Entwurfs Spezifikation für *die spezifischsten außer Kraft* setzung, da Sie auf gemischte Klassen und Schnittstellen angewendet wird (eine Klasse hat Vorrang vor einer Schnittstelle).</span><span class="sxs-lookup"><span data-stu-id="cdf65-342">***Closed Issue:*** Confirm the draft spec, above, for *most specific override* as it applies to mixed classes and interfaces (a class takes priority over an interface).</span></span> <span data-ttu-id="cdf65-343">Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span><span class="sxs-lookup"><span data-stu-id="cdf65-343">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span></span>

### <a name="interface-methods-vs-structs-closed"></a><span data-ttu-id="cdf65-344">Schnittstellen Methoden im Vergleich zu Strukturen (geschlossen)</span><span class="sxs-lookup"><span data-stu-id="cdf65-344">Interface methods vs structs (closed)</span></span>

<span data-ttu-id="cdf65-345">Es gibt einige unglückliche Interaktionen zwischen Standardschnittstellen Methoden und Strukturen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-345">There are some unfortunate interactions between default interface methods and structs.</span></span>

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

<span data-ttu-id="cdf65-346">Beachten Sie, dass Schnittstellenmember nicht geerbt werden:</span><span class="sxs-lookup"><span data-stu-id="cdf65-346">Note that interface members are not inherited:</span></span>

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

<span data-ttu-id="cdf65-347">Folglich muss der Client die Struktur zum Aufrufen von Schnittstellen Methoden in Box Box</span><span class="sxs-lookup"><span data-stu-id="cdf65-347">Consequently, the client must box the struct to invoke interface methods</span></span>

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

<span data-ttu-id="cdf65-348">Wenn Sie auf diese Weise Boxing, werden die Hauptvorteile eines `struct` Typs zunichte.</span><span class="sxs-lookup"><span data-stu-id="cdf65-348">Boxing in this way defeats the principal benefits of a `struct` type.</span></span> <span data-ttu-id="cdf65-349">Außerdem haben alle mutations Methoden keinen offensichtlichen Effekt, da Sie auf einer geachtelten *Kopie* der Struktur ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="cdf65-349">Moreover, any mutation methods will have no apparent effect, because they are operating on a *boxed copy* of the struct:</span></span>

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> <span data-ttu-id="cdf65-350">***Geschlossene Probleme:*** Dazu können Sie Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="cdf65-350">***Closed Issue:*** What can we do about this:</span></span>
>
> 1. <span data-ttu-id="cdf65-351">Verbieten, dass eine `struct` eine Standard Implementierung erbt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-351">Forbid a `struct` from inheriting a default implementation.</span></span> <span data-ttu-id="cdf65-352">Alle Schnittstellen Methoden werden in einem `struct`als abstrakt behandelt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-352">All interface methods would be treated as abstract in a `struct`.</span></span> <span data-ttu-id="cdf65-353">Dann können wir später einige Zeit in Anspruch nehmen, um die Arbeit zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="cdf65-353">Then we may take time later to decide how to make it work better.</span></span>
> 2. <span data-ttu-id="cdf65-354">Entwickeln Sie eine Art Code Generierungs Strategie, die Boxing vermeidet.</span><span class="sxs-lookup"><span data-stu-id="cdf65-354">Come up with some kind of code generation strategy that avoids boxing.</span></span> <span data-ttu-id="cdf65-355">Innerhalb einer Methode wie `IB.Increment`wäre der Typ des `this` vielleicht vergleichbar mit einem Typparameter, der auf `IB`eingeschränkt ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-355">Inside a method like `IB.Increment`, the type of `this` would perhaps be akin to a type parameter constrained to `IB`.</span></span> <span data-ttu-id="cdf65-356">Um das Boxing im Aufrufer zu vermeiden, werden nicht abstrakte Methoden in Verbindung mit den Schnittstellen geerbt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-356">In conjunction with that, to avoid boxing in the caller, non-abstract methods would be inherited from interfaces.</span></span> <span data-ttu-id="cdf65-357">Dadurch kann der Compiler und die CLR-Implementierung erheblich zunehmen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-357">This may increase compiler and CLR implementation work substantially.</span></span>
> 3. <span data-ttu-id="cdf65-358">Machen Sie sich keine Gedanken um die IT, und lassen Sie Sie einfach als wart.</span><span class="sxs-lookup"><span data-stu-id="cdf65-358">Not worry about it and just leave it as a wart.</span></span>
> 4. <span data-ttu-id="cdf65-359">Weitere Ideen?</span><span class="sxs-lookup"><span data-stu-id="cdf65-359">Other ideas?</span></span>

<span data-ttu-id="cdf65-360">***Entscheidung:*** Machen Sie sich keine Gedanken um die IT, und lassen Sie Sie einfach als wart.</span><span class="sxs-lookup"><span data-stu-id="cdf65-360">***Decision:*** Not worry about it and just leave it as a wart.</span></span> <span data-ttu-id="cdf65-361">Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span><span class="sxs-lookup"><span data-stu-id="cdf65-361">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span></span>

### <a name="base-interface-invocations-closed"></a><span data-ttu-id="cdf65-362">Basis Schnittstellen Aufrufe (geschlossen)</span><span class="sxs-lookup"><span data-stu-id="cdf65-362">Base interface invocations (closed)</span></span>

<span data-ttu-id="cdf65-363">Die Entwurfs Spezifikation schlägt eine Syntax für Basis Schnittstellen Aufrufe vor, die von Java inspiriert sind: `Interface.base.M()`.</span><span class="sxs-lookup"><span data-stu-id="cdf65-363">The draft spec suggests a syntax for base interface invocations inspired by Java: `Interface.base.M()`.</span></span> <span data-ttu-id="cdf65-364">Wir müssen eine Syntax auswählen, zumindest für den anfänglichen Prototyp.</span><span class="sxs-lookup"><span data-stu-id="cdf65-364">We need to select a syntax, at least for the initial prototype.</span></span> <span data-ttu-id="cdf65-365">Mein Favorit ist `base<Interface>.M()`.</span><span class="sxs-lookup"><span data-stu-id="cdf65-365">My favorite is `base<Interface>.M()`.</span></span>

> <span data-ttu-id="cdf65-366">***Geschlossene Probleme:*** Was ist die Syntax für einen Basismember-Aufruf?</span><span class="sxs-lookup"><span data-stu-id="cdf65-366">***Closed Issue:*** What is the syntax for a base member invocation?</span></span>

<span data-ttu-id="cdf65-367">***Entscheidung:*** Die Syntax ist `base(Interface).M()`.</span><span class="sxs-lookup"><span data-stu-id="cdf65-367">***Decision:*** The syntax is `base(Interface).M()`.</span></span> <span data-ttu-id="cdf65-368">Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span><span class="sxs-lookup"><span data-stu-id="cdf65-368">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span></span> <span data-ttu-id="cdf65-369">Die Schnittstelle, die so benannt ist, muss eine Basisschnittstelle sein, es muss sich jedoch nicht um eine direkte Basisschnittstelle handeln.</span><span class="sxs-lookup"><span data-stu-id="cdf65-369">The interface so named must be a base interface, but does not need to be a direct base interface.</span></span>

> <span data-ttu-id="cdf65-370">***Problem öffnen:*** Sollten Basis Schnittstellen Aufrufe in Klassenmembern zulässig sein?</span><span class="sxs-lookup"><span data-stu-id="cdf65-370">***Open Issue:*** Should base interface invocations be permitted in class members?</span></span>

<span data-ttu-id="cdf65-371">***Entscheidung***: Ja.</span><span class="sxs-lookup"><span data-stu-id="cdf65-371">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a><span data-ttu-id="cdf65-372">Überschreiben von nicht öffentlichen Schnittstellenmembern (geschlossen)</span><span class="sxs-lookup"><span data-stu-id="cdf65-372">Overriding non-public interface members (closed)</span></span>

<span data-ttu-id="cdf65-373">In einer Schnittstelle werden nicht öffentliche Member von Basis Schnittstellen mithilfe des `override` Modifizierers überschrieben.</span><span class="sxs-lookup"><span data-stu-id="cdf65-373">In an interface, non-public members from base interfaces are overridden using the `override` modifier.</span></span> <span data-ttu-id="cdf65-374">Wenn es sich um eine explizite außer Kraft Setzung handelt, die die Schnittstelle mit dem Member benennt, wird der Zugriffsmodifizierer ausgelassen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-374">If it is an "explicit" override that names the interface containing the member, the access modifier is omitted.</span></span>

> <span data-ttu-id="cdf65-375">***Geschlossene Probleme:*** Muss der Zugriffsmodifizierer abgeglichen werden, wenn es sich um eine "implizite" außer Kraft Setzung handelt, die der Schnittstelle nicht entspricht?</span><span class="sxs-lookup"><span data-stu-id="cdf65-375">***Closed Issue:*** If it is an "implicit" override that does not name the interface, does the access modifier have to match?</span></span>

<span data-ttu-id="cdf65-376">***Entscheidung:*** Nur öffentliche Member können implizit überschrieben werden, und der Zugriff muss entsprechend erfolgen.</span><span class="sxs-lookup"><span data-stu-id="cdf65-376">***Decision:*** Only public members may be implicitly overridden, and the access must match.</span></span> <span data-ttu-id="cdf65-377">Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span><span class="sxs-lookup"><span data-stu-id="cdf65-377">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

> <span data-ttu-id="cdf65-378">***Problem öffnen:*** Ist der Zugriffsmodifizierer für eine explizite außer Kraft Setzung erforderlich, optional oder ausgelassen, z. b. `override void IB.M() {}`?</span><span class="sxs-lookup"><span data-stu-id="cdf65-378">***Open Issue:*** Is the access modifier required, optional, or omitted on an explicit override such as `override void IB.M() {}`?</span></span>

> <span data-ttu-id="cdf65-379">***Problem öffnen:*** Ist `override` erforderlich, optional oder bei einer expliziten außer Kraft setzung, z. b. `void IB.M() {}`, ausgelassen?</span><span class="sxs-lookup"><span data-stu-id="cdf65-379">***Open Issue:*** Is `override` required, optional, or omitted on an explicit override such as `void IB.M() {}`?</span></span>

<span data-ttu-id="cdf65-380">Wie implementiert ein nicht öffentliches Schnittstellenmember in einer Klasse?</span><span class="sxs-lookup"><span data-stu-id="cdf65-380">How does one implement a non-public interface member in a class?</span></span> <span data-ttu-id="cdf65-381">Sie müssen möglicherweise explizit ausgeführt werden?</span><span class="sxs-lookup"><span data-stu-id="cdf65-381">Perhaps it must be done explicitly?</span></span>

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> <span data-ttu-id="cdf65-382">***Geschlossene Probleme:*** Wie implementiert ein nicht öffentliches Schnittstellenmember in einer Klasse?</span><span class="sxs-lookup"><span data-stu-id="cdf65-382">***Closed Issue:*** How does one implement a non-public interface member in a class?</span></span>

<span data-ttu-id="cdf65-383">***Entscheidung:*** Nicht öffentliche Schnittstellenmember können nur explizit implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-383">***Decision:*** You can only implement non-public interface members explicitly.</span></span> <span data-ttu-id="cdf65-384">Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span><span class="sxs-lookup"><span data-stu-id="cdf65-384">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

<span data-ttu-id="cdf65-385">***Entscheidung***: für Schnittstellenmember ist kein `override` Schlüsselwort zulässig.</span><span class="sxs-lookup"><span data-stu-id="cdf65-385">***Decision***: No `override` keyword permitted on interface members.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a><span data-ttu-id="cdf65-386">Binäre Kompatibilität 2 (geschlossen)</span><span class="sxs-lookup"><span data-stu-id="cdf65-386">Binary Compatibility 2 (closed)</span></span>

<span data-ttu-id="cdf65-387">Beachten Sie den folgenden Code, in dem sich die einzelnen Typen in einer separaten Assembly befinden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-387">Consider the following code in which each type is in a separate assembly</span></span>

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

<span data-ttu-id="cdf65-388">Wir wissen, dass die Implementierung von `I1.M` in `C` `I2.M`ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-388">We understand that the implementation of `I1.M` in `C` is `I2.M`.</span></span> <span data-ttu-id="cdf65-389">Was geschieht, wenn die Assembly, die `I3` enthält, wie folgt geändert und neu kompiliert wird.</span><span class="sxs-lookup"><span data-stu-id="cdf65-389">What if the assembly containing `I3` is changed as follows and recompiled</span></span>

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

<span data-ttu-id="cdf65-390">`C` wird jedoch nicht neu kompiliert.</span><span class="sxs-lookup"><span data-stu-id="cdf65-390">but `C` is not recompiled.</span></span> <span data-ttu-id="cdf65-391">Was geschieht, wenn das Programm ausgeführt wird?</span><span class="sxs-lookup"><span data-stu-id="cdf65-391">What happens when the program is run?</span></span> <span data-ttu-id="cdf65-392">Ein Aufruf von `(C as I1).M()`</span><span class="sxs-lookup"><span data-stu-id="cdf65-392">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="cdf65-393">Führt `I1.M`</span><span class="sxs-lookup"><span data-stu-id="cdf65-393">Runs `I1.M`</span></span>
2. <span data-ttu-id="cdf65-394">Führt `I2.M`</span><span class="sxs-lookup"><span data-stu-id="cdf65-394">Runs `I2.M`</span></span>
3. <span data-ttu-id="cdf65-395">Führt `I3.M`</span><span class="sxs-lookup"><span data-stu-id="cdf65-395">Runs `I3.M`</span></span>
4. <span data-ttu-id="cdf65-396">Entweder 2 oder 3, deterministisch</span><span class="sxs-lookup"><span data-stu-id="cdf65-396">Either 2 or 3, deterministically</span></span>
5. <span data-ttu-id="cdf65-397">Löst eine Art von Lauf Zeit Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="cdf65-397">Throws some kind of runtime exception</span></span>

<span data-ttu-id="cdf65-398">***Entscheidung***: lösen Sie eine Ausnahme aus (5).</span><span class="sxs-lookup"><span data-stu-id="cdf65-398">***Decision***: Throw an exception (5).</span></span> <span data-ttu-id="cdf65-399">Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span><span class="sxs-lookup"><span data-stu-id="cdf65-399">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span></span>

### <a name="permit-partial-in-interface-closed"></a><span data-ttu-id="cdf65-400">`partial` in der Schnittstelle zulassen?</span><span class="sxs-lookup"><span data-stu-id="cdf65-400">Permit `partial` in interface?</span></span> <span data-ttu-id="cdf65-401">geschlossen</span><span class="sxs-lookup"><span data-stu-id="cdf65-401">(closed)</span></span>

<span data-ttu-id="cdf65-402">Da Schnittstellen in ähnlicher Weise wie abstrakte Klassen verwendet werden können, kann es sinnvoll sein, Sie `partial`zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cdf65-402">Given that interfaces may be used in ways analogous to the way abstract classes are used, it may be useful to declare them `partial`.</span></span> <span data-ttu-id="cdf65-403">Dies wäre besonders nützlich bei Generatoren.</span><span class="sxs-lookup"><span data-stu-id="cdf65-403">This would be particularly useful in the face of generators.</span></span>

> <span data-ttu-id="cdf65-404">***Angebot:*** Entfernen Sie die sprach Einschränkung, dass Schnittstellen und Member von Schnittstellen möglicherweise nicht als `partial`deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-404">***Proposal:*** Remove the language restriction that interfaces and members of interfaces may not be declared `partial`.</span></span>

<span data-ttu-id="cdf65-405">***Entscheidung***: Ja.</span><span class="sxs-lookup"><span data-stu-id="cdf65-405">***Decision***: Yes.</span></span> <span data-ttu-id="cdf65-406">Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span><span class="sxs-lookup"><span data-stu-id="cdf65-406">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span></span>

### <a name="main-in-an-interface-closed"></a><span data-ttu-id="cdf65-407">`Main` in einer Schnittstelle?</span><span class="sxs-lookup"><span data-stu-id="cdf65-407">`Main` in an interface?</span></span> <span data-ttu-id="cdf65-408">geschlossen</span><span class="sxs-lookup"><span data-stu-id="cdf65-408">(closed)</span></span>

> <span data-ttu-id="cdf65-409">***Problem öffnen:*** Ist eine `static Main` Methode in einer Schnittstelle ein Kandidat für den Einstiegspunkt des Programms?</span><span class="sxs-lookup"><span data-stu-id="cdf65-409">***Open Issue:*** Is a `static Main` method in an interface a candidate to be the program's entry point?</span></span>

<span data-ttu-id="cdf65-410">***Entscheidung***: Ja.</span><span class="sxs-lookup"><span data-stu-id="cdf65-410">***Decision***: Yes.</span></span> <span data-ttu-id="cdf65-411">Siehe <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span><span class="sxs-lookup"><span data-stu-id="cdf65-411">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span></span>

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a><span data-ttu-id="cdf65-412">Bestätigen, dass öffentliche, nicht virtuelle Methoden unterstützt werden sollen (geschlossen)</span><span class="sxs-lookup"><span data-stu-id="cdf65-412">Confirm intent to support public non-virtual methods (closed)</span></span>

<span data-ttu-id="cdf65-413">Können wir unsere Entscheidung bestätigen (oder umkehren), um nicht virtuelle öffentliche Methoden in einer Schnittstelle zuzulassen?</span><span class="sxs-lookup"><span data-stu-id="cdf65-413">Can we please confirm (or reverse) our decision to permit non-virtual public methods in an interface?</span></span>

```csharp
interface IA
{
    public sealed void M() { }
}
```

> <span data-ttu-id="cdf65-414">***Semiclosed-Problem:*** (2017-04-18) Wir sind davon überzeugt, dass es nützlich sein wird, es wird jedoch zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="cdf65-414">***Semi-Closed Issue:*** (2017-04-18) We think it is going to be useful, but will come back to it.</span></span> <span data-ttu-id="cdf65-415">Dabei handelt es sich um einen Block für das Muster des geistigen Modells.</span><span class="sxs-lookup"><span data-stu-id="cdf65-415">This is a mental model tripping block.</span></span>

<span data-ttu-id="cdf65-416">***Entscheidung***: Ja.</span><span class="sxs-lookup"><span data-stu-id="cdf65-416">***Decision***: Yes.</span></span> <span data-ttu-id="cdf65-417">[https://login.microsoftonline.com/consumers/](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>).</span><span class="sxs-lookup"><span data-stu-id="cdf65-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.</span></span>

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a><span data-ttu-id="cdf65-418">Führt ein `override` in einer Schnittstelle ein neues Mitglied ein?</span><span class="sxs-lookup"><span data-stu-id="cdf65-418">Does an `override` in an interface introduce a new member?</span></span> <span data-ttu-id="cdf65-419">geschlossen</span><span class="sxs-lookup"><span data-stu-id="cdf65-419">(closed)</span></span>

<span data-ttu-id="cdf65-420">Es gibt mehrere Möglichkeiten, um zu überprüfen, ob eine Überschreibungs Deklaration einen neuen Member einführt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-420">There are a few ways to observe whether an override declaration introduces a new member or not.</span></span>

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> <span data-ttu-id="cdf65-421">***Problem öffnen:*** Führt eine Überschreibungs Deklaration in einer Schnittstelle einen neuen Member ein?</span><span class="sxs-lookup"><span data-stu-id="cdf65-421">***Open Issue:*** Does an override declaration in an interface introduce a new member?</span></span> <span data-ttu-id="cdf65-422">geschlossen</span><span class="sxs-lookup"><span data-stu-id="cdf65-422">(closed)</span></span>

<span data-ttu-id="cdf65-423">In einer Klasse ist eine über schreibende Methode in gewisser Hinsicht "sichtbar".</span><span class="sxs-lookup"><span data-stu-id="cdf65-423">In a class, an overriding method is "visible" in some senses.</span></span> <span data-ttu-id="cdf65-424">Die Namen der Parameter haben beispielsweise Vorrang vor den Namen von Parametern in der überschriebenen Methode.</span><span class="sxs-lookup"><span data-stu-id="cdf65-424">For example, the names of its parameters take precedence over the names of parameters in the overridden method.</span></span> <span data-ttu-id="cdf65-425">Möglicherweise ist es möglich, dieses Verhalten in Schnittstellen zu duplizieren, da immer eine spezifischere außer Kraft Setzung vorliegt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-425">It may be possible to duplicate that behavior in interfaces, as there is always a most specific override.</span></span> <span data-ttu-id="cdf65-426">Möchten Sie dieses Verhalten jedoch duplizieren?</span><span class="sxs-lookup"><span data-stu-id="cdf65-426">But do we want to duplicate that behavior?</span></span>

<span data-ttu-id="cdf65-427">Außerdem ist es möglich, eine Überschreibungs Methode außer Kraft zu setzen?</span><span class="sxs-lookup"><span data-stu-id="cdf65-427">Also, it is possible to "override" an override method?</span></span> <span data-ttu-id="cdf65-428">Fraglich</span><span class="sxs-lookup"><span data-stu-id="cdf65-428">[Moot]</span></span>

<span data-ttu-id="cdf65-429">***Entscheidung***: für Schnittstellenmember ist kein `override` Schlüsselwort zulässig.</span><span class="sxs-lookup"><span data-stu-id="cdf65-429">***Decision***: No `override` keyword permitted on interface members.</span></span> <span data-ttu-id="cdf65-430">[https://login.microsoftonline.com/consumers/](<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>).</span><span class="sxs-lookup"><span data-stu-id="cdf65-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.</span></span>

### <a name="properties-with-a-private-accessor-closed"></a><span data-ttu-id="cdf65-431">Eigenschaften mit einem privaten Accessor (geschlossen)</span><span class="sxs-lookup"><span data-stu-id="cdf65-431">Properties with a private accessor (closed)</span></span>

<span data-ttu-id="cdf65-432">Wir sagen, dass private Member nicht virtuell sind und die Kombination aus Virtual und private nicht zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-432">We say that private members are not virtual, and the combination of virtual and private is disallowed.</span></span> <span data-ttu-id="cdf65-433">Aber wie sieht es mit einer Eigenschaft mit einem privaten Accessor aus?</span><span class="sxs-lookup"><span data-stu-id="cdf65-433">But what about a property with a private accessor?</span></span>

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

<span data-ttu-id="cdf65-434">Ist dies zulässig?</span><span class="sxs-lookup"><span data-stu-id="cdf65-434">Is this allowed?</span></span> <span data-ttu-id="cdf65-435">`virtual` der `set` Accessor oder nicht?</span><span class="sxs-lookup"><span data-stu-id="cdf65-435">Is the `set` accessor here `virtual` or not?</span></span> <span data-ttu-id="cdf65-436">Kann sie überschrieben werden, wo Sie zugänglich ist?</span><span class="sxs-lookup"><span data-stu-id="cdf65-436">Can it be overridden where it is accessible?</span></span> <span data-ttu-id="cdf65-437">Implementiert Folgendes implizit nur den `get` Accessor?</span><span class="sxs-lookup"><span data-stu-id="cdf65-437">Does the following implicitly implement only the `get` accessor?</span></span>

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

<span data-ttu-id="cdf65-438">Der folgende Fehler ist vermutlich ein Fehler, weil IA vorliegt. P. Set ist nicht virtuell und auch weil nicht darauf zugegriffen werden kann?</span><span class="sxs-lookup"><span data-stu-id="cdf65-438">Is the following presumably an error because IA.P.set isn't virtual and also because it isn't accessible?</span></span>

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

<span data-ttu-id="cdf65-439">***Entscheidung***: das erste Beispiel ist gültig, während das letzte nicht.</span><span class="sxs-lookup"><span data-stu-id="cdf65-439">***Decision***: The first example looks valid, while the last does not.</span></span> <span data-ttu-id="cdf65-440">Dies wird analog zur Funktionsweise in C#gelöst.</span><span class="sxs-lookup"><span data-stu-id="cdf65-440">This is resolved analogously to how it already works in C#.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a><span data-ttu-id="cdf65-441">Basis Schnittstellen Aufrufe, Round 2 (geschlossen)</span><span class="sxs-lookup"><span data-stu-id="cdf65-441">Base Interface Invocations, round 2 (closed)</span></span>

<span data-ttu-id="cdf65-442">Unsere vorherige "Lösung" zur Behandlung von grundlegenden aufrufen bietet keine ausreichende Ausdruckskraft.</span><span class="sxs-lookup"><span data-stu-id="cdf65-442">Our previous "resolution" to how to handle base invocations doesn't actually provide sufficient expressiveness.</span></span> <span data-ttu-id="cdf65-443">Es stellt sich heraus, C# dass Sie in und der CLR im Gegensatz zu Java sowohl die-Schnittstelle, die die Methoden Deklaration enthält, als auch den Speicherort der Implementierung angeben müssen, die Sie aufrufen möchten.</span><span class="sxs-lookup"><span data-stu-id="cdf65-443">It turns out that in C# and the CLR, unlike Java, you need to specify both the interface containing the method declaration and the location of the implementation you want to invoke.</span></span>

<span data-ttu-id="cdf65-444">Ich schlage die folgende Syntax für Basis Aufrufe in Schnittstellen vor.</span><span class="sxs-lookup"><span data-stu-id="cdf65-444">I propose the following syntax for base calls in interfaces.</span></span> <span data-ttu-id="cdf65-445">Ich bin nicht in der Liebe, aber es wird veranschaulicht, welche Syntax in der Lage sein muss, folgendes auszudrücken:</span><span class="sxs-lookup"><span data-stu-id="cdf65-445">I’m not in love with it, but it illustrates what any syntax must be able to express:</span></span>

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

<span data-ttu-id="cdf65-446">Wenn keine Mehrdeutigkeit vorliegt, können Sie Sie einfacher schreiben.</span><span class="sxs-lookup"><span data-stu-id="cdf65-446">If there is no ambiguity, you can write it more simply</span></span>

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

<span data-ttu-id="cdf65-447">Oder</span><span class="sxs-lookup"><span data-stu-id="cdf65-447">Or</span></span>

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

<span data-ttu-id="cdf65-448">Oder</span><span class="sxs-lookup"><span data-stu-id="cdf65-448">Or</span></span>

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

<span data-ttu-id="cdf65-449">***Entscheidung***: entscheiden Sie sich für `base(N.I1<T>).M(s)`, und wenn wir eine Aufruf Bindung haben, können Sie später möglicherweise Probleme finden.</span><span class="sxs-lookup"><span data-stu-id="cdf65-449">***Decision***: Decided on `base(N.I1<T>).M(s)`, conceding that if we have an invocation binding there may be problem here later on.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a><span data-ttu-id="cdf65-450">Warnung für die Struktur, die die Standardmethode nicht implementiert?</span><span class="sxs-lookup"><span data-stu-id="cdf65-450">Warning for struct not implementing default method?</span></span> <span data-ttu-id="cdf65-451">geschlossen</span><span class="sxs-lookup"><span data-stu-id="cdf65-451">(closed)</span></span>

<span data-ttu-id="cdf65-452">@vancem bestätigt, dass wir ernsthaft eine Warnung erstellen sollten, wenn eine Werttyp Deklaration eine Schnittstellen Methode nicht außer Kraft setzt, auch wenn Sie eine Implementierung dieser Methode von einer Schnittstelle erben würde.</span><span class="sxs-lookup"><span data-stu-id="cdf65-452">@vancem asserts that we should seriously consider producing a warning if a value type declaration fails to override some interface method, even if it would inherit an implementation of that method from an interface.</span></span> <span data-ttu-id="cdf65-453">Da dies zum Boxing und zum untergraben von eingeschränkten aufrufen bewirkt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-453">Because it causes boxing and undermines constrained calls.</span></span>

<span data-ttu-id="cdf65-454">***Entscheidung***: Dies scheint etwas, das für einen Analyzer besser geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="cdf65-454">***Decision***: This seems like something more suited for an analyzer.</span></span> <span data-ttu-id="cdf65-455">Es scheint auch, dass diese Warnung nicht mehr angezeigt wird, da Sie auch dann ausgelöst würde, wenn die Standardschnittstellen Methode niemals aufgerufen wird und nie Boxing auftritt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-455">It also seems like this warning could be noisy, since it would fire even if the default interface method is never called and no boxing will ever occur.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a><span data-ttu-id="cdf65-456">Statische Schnittstellen Konstruktoren (geschlossen)</span><span class="sxs-lookup"><span data-stu-id="cdf65-456">Interface static constructors (closed)</span></span>

<span data-ttu-id="cdf65-457">Wann werden statische Konstruktoren für die Schnittstelle ausgeführt?</span><span class="sxs-lookup"><span data-stu-id="cdf65-457">When are interface static constructors run?</span></span>  <span data-ttu-id="cdf65-458">Der aktuelle CLI-Entwurf schlägt vor, dass er auftritt, wenn auf die erste statische Methode oder das erste Feld zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="cdf65-458">The current CLI draft proposes that it occurs when the first static method or field is accessed.</span></span> <span data-ttu-id="cdf65-459">Wenn keines davon vorhanden ist, wird es möglicherweise nie ausgeführt?</span><span class="sxs-lookup"><span data-stu-id="cdf65-459">If there are neither of those then it might never be run??</span></span>

<span data-ttu-id="cdf65-460">[2018-10-09 das CLR-Team schlägt vor, was wir für Werttypen tun (cctor Check on Access to each instance method)]</span><span class="sxs-lookup"><span data-stu-id="cdf65-460">[2018-10-09 The CLR team proposes "Going to mirror what we do for valuetypes (cctor check on access to each instance method)"]</span></span>

<span data-ttu-id="cdf65-461">***Entscheidung***: statische Konstruktoren werden auch für den Eintrag in Instanzmethoden ausgeführt, wenn der statische Konstruktor nicht `beforefieldinit`wurde. in diesem Fall werden statische Konstruktoren vor dem Zugriff auf das erste statische Feld ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="cdf65-461">***Decision***: Static constructors are also run on entry to instance methods, if the static constructor was not `beforefieldinit`, in which case static constructors are run before access to the first static field.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a><span data-ttu-id="cdf65-462">Treffen von Besprechungen</span><span class="sxs-lookup"><span data-stu-id="cdf65-462">Design meetings</span></span>

<span data-ttu-id="cdf65-463">[2017-03-08 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 Meeting "CLR Behavior for Default Interface Methods"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md) Meeting [Notes
2017-04-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) LDM Meeting Notes
[2017-04-19 LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md) Meeting Notes
2017-05-17 [LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md) Meeting [Notes
2017-05-31](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) LDM Meeting Notes
2017-06-14 LDM [ Besprechungs Hinweise](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)</span><span class="sxs-lookup"><span data-stu-id="cdf65-463">[2017-03-08 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 meeting "CLR Behavior for Default Interface Methods"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)
[2017-04-18 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md)
[2017-04-19 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)
[2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
[2017-05-31 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md)
[2017-06-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)</span></span>
