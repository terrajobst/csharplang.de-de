---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281956"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="91ce4-101">Datensätze werden in Bearbeitung aufgezeichnet</span><span class="sxs-lookup"><span data-stu-id="91ce4-101">Records Work-in-Progress</span></span>

<span data-ttu-id="91ce4-102">Anders als bei den anderen Datensätzen, ist dies kein Vorschlag in sich selbst, sondern eine Arbeit in Bearbeitung, die für das Aufzeichnen von Entscheidungen zum Entwurf von Konsens für das Feature "Datensätze" konzipiert ist.</span><span class="sxs-lookup"><span data-stu-id="91ce4-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="91ce4-103">Die Spezifikations Details werden bei Bedarf hinzugefügt, um Fragen zu lösen.</span><span class="sxs-lookup"><span data-stu-id="91ce4-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="91ce4-104">Die Syntax für einen Datensatz muss wie folgt hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="91ce4-104">The syntax for a record is proposed to be added as follows:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
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

<span data-ttu-id="91ce4-105">Das `attributes` nicht Terminal ermöglicht außerdem das neue kontextabhängige Attribut `data`.</span><span class="sxs-lookup"><span data-stu-id="91ce4-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="91ce4-106">Eine Klasse (Struktur), die mit einer Parameterliste oder einem `data` Modifizierer deklariert wurde, wird als Daten Satz Klasse (Daten Satzstruktur) bezeichnet, von denen jeder ein Daten Satz Typ ist.</span><span class="sxs-lookup"><span data-stu-id="91ce4-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="91ce4-107">Es ist ein Fehler, einen Daten Satz Typen ohne eine Parameterliste und den `data` Modifizierer zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="91ce4-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="91ce4-108">Mitglieder eines Daten Satz Typs</span><span class="sxs-lookup"><span data-stu-id="91ce4-108">Members of a record type</span></span>

<span data-ttu-id="91ce4-109">Zusätzlich zu den Membern, die im Class-Body deklariert sind, verfügt ein Daten Satz Typen über die folgenden zusätzlichen Member:</span><span class="sxs-lookup"><span data-stu-id="91ce4-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="91ce4-110">Primärer Konstruktor</span><span class="sxs-lookup"><span data-stu-id="91ce4-110">Primary Constructor</span></span>

<span data-ttu-id="91ce4-111">Ein Daten Recordtyp verfügt über einen öffentlichen Konstruktor, dessen Signatur den Wert Parametern der Typdeklaration entspricht.</span><span class="sxs-lookup"><span data-stu-id="91ce4-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="91ce4-112">Dies wird als primärer Konstruktor für den Typ bezeichnet und bewirkt, dass der implizit deklarierte Standardkonstruktor unterdrückt wird.</span><span class="sxs-lookup"><span data-stu-id="91ce4-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="91ce4-113">Es ist ein Fehler, dass ein primärer Konstruktor und ein Konstruktor mit der gleichen Signatur bereits in der Klasse vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="91ce4-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="91ce4-114">Zur Laufzeit der primäre Konstruktor</span><span class="sxs-lookup"><span data-stu-id="91ce4-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="91ce4-115">führt die instanzfeldinitialisierer aus, die in der Klasse Body angezeigt werden. und ruft dann den Basisklassenkonstruktor ohne Argumente auf.</span><span class="sxs-lookup"><span data-stu-id="91ce4-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="91ce4-116">Initialisiert vom Compiler generierte Unterstützungs Felder für die Eigenschaften, die den Wert Parametern entsprechen (wenn diese Eigenschaften vom Compiler bereitgestellt werden; siehe [synthetische Eigenschaften](#Synthesized Properties)).</span><span class="sxs-lookup"><span data-stu-id="91ce4-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="91ce4-117">[] TODO: Hinzufügen von grundlegenden aufrufen Syntax und Spezifikation zum Auswählen des basiskonstruktors durch Überladungs Auflösung</span><span class="sxs-lookup"><span data-stu-id="91ce4-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="91ce4-118">Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="91ce4-118">Properties</span></span>

<span data-ttu-id="91ce4-119">Für jeden Datensatz-Parameter einer Datensatz-Typdeklaration gibt es einen entsprechenden Member der öffentlichen Eigenschaft, dessen Name und Typ aus der Deklaration des Wert Parameters entnommen werden.</span><span class="sxs-lookup"><span data-stu-id="91ce4-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="91ce4-120">Wenn keine konkrete (d.h. nicht abstrakte) Eigenschaft mit einem get-Accessor und mit diesem Namen und Typ explizit deklariert oder geerbt wird, wird der Compiler wie folgt erstellt:</span><span class="sxs-lookup"><span data-stu-id="91ce4-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="91ce4-121">Für eine Daten Satzstruktur oder eine Daten Satz Klasse:</span><span class="sxs-lookup"><span data-stu-id="91ce4-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="91ce4-122">Eine öffentliche automatische Get-only-Eigenschaft wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="91ce4-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="91ce4-123">Der Wert wird während der Erstellung mit dem Wert des entsprechenden primären Konstruktorparameters initialisiert.</span><span class="sxs-lookup"><span data-stu-id="91ce4-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="91ce4-124">Jeder Get-Accessor der geerbten abstrakten Eigenschaft wird überschrieben.</span><span class="sxs-lookup"><span data-stu-id="91ce4-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="91ce4-125">Gleichheits Elemente</span><span class="sxs-lookup"><span data-stu-id="91ce4-125">Equality members</span></span>

<span data-ttu-id="91ce4-126">Daten Satz Typen führen zu synthetisierten Implementierungen für die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="91ce4-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="91ce4-127">`object.GetHashCode()` außer Kraft setzung, sofern nicht versiegelt oder vom Benutzer bereitgestellt</span><span class="sxs-lookup"><span data-stu-id="91ce4-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="91ce4-128">`object.Equals(object)` außer Kraft setzung, sofern nicht versiegelt oder vom Benutzer bereitgestellt</span><span class="sxs-lookup"><span data-stu-id="91ce4-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="91ce4-129">`T Equals(T)` Methode, bei der `T` der aktuelle Typ ist.</span><span class="sxs-lookup"><span data-stu-id="91ce4-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="91ce4-130">`T Equals(T)` wird angegeben, um Wert Gleichheit auszuführen. dabei wird die-Eigenschaft mit demselben Namen wie jeder primäre Konstruktorparameter mit der entsprechenden Eigenschaft des anderen Typs verglichen.</span><span class="sxs-lookup"><span data-stu-id="91ce4-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="91ce4-131">`object.Equals` führt die Entsprechung von</span><span class="sxs-lookup"><span data-stu-id="91ce4-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a><span data-ttu-id="91ce4-132">`with`-Ausdruck</span><span class="sxs-lookup"><span data-stu-id="91ce4-132">`with` expression</span></span>

<span data-ttu-id="91ce4-133">Ein `with` Ausdruck ist ein neuer Ausdruck, der die folgende Syntax verwendet.</span><span class="sxs-lookup"><span data-stu-id="91ce4-133">A `with` expression is a new expression using the following syntax.</span></span>

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

<span data-ttu-id="91ce4-134">Ein `with` Ausdruck ermöglicht die "nicht zerstörerische Mutation", die eine Kopie des Empfänger Ausdrucks mit Änderungen an den Eigenschaften erzeugt, die im `anonymous_object_initializer`aufgelistet sind.</span><span class="sxs-lookup"><span data-stu-id="91ce4-134">A `with` expression allows for "non-destructive mutation", designed to produce a copy of the receiver expression with modifications to properties listed in the `anonymous_object_initializer`.</span></span>

<span data-ttu-id="91ce4-135">Ein gültiger `with` Ausdruck hat einen Empfänger mit einem nicht-void-Typ.</span><span class="sxs-lookup"><span data-stu-id="91ce4-135">A valid `with` expression has a receiver with a non-void type.</span></span> <span data-ttu-id="91ce4-136">Der Empfängertyp muss eine barrierefreie Instanzmethode mit dem Namen `With` mit den entsprechenden Parametern und dem Rückgabetyp enthalten.</span><span class="sxs-lookup"><span data-stu-id="91ce4-136">The receiver type must contain an accessible instance method called `With` with the appropriate parameters and return type.</span></span> <span data-ttu-id="91ce4-137">Wenn mehrere `With` Methoden ohne außer Kraft Setzung vorhanden sind, ist ein Fehler aufgetreten.</span><span class="sxs-lookup"><span data-stu-id="91ce4-137">It is an error if there are multiple non-override `With` methods.</span></span> <span data-ttu-id="91ce4-138">Wenn mehrere `With` außer Kraft setzungen vorhanden sind, muss eine nicht über schreibende `With` Methode vorhanden sein, die die Ziel Methode ist.</span><span class="sxs-lookup"><span data-stu-id="91ce4-138">If there are multiple `With` overrides, there must be a non-override `With` method, which is the target method.</span></span> <span data-ttu-id="91ce4-139">Andernfalls muss genau eine `With`-Methode vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="91ce4-139">Otherwise, there must be exactly one `With` method.</span></span>

<span data-ttu-id="91ce4-140">Auf der rechten Seite des `with` Ausdrucks handelt es sich um eine `anonymous_object_initializer` mit einer Sequenz von Zuweisungen mit einem Feld oder einer Eigenschaft des Empfängers auf der linken Seite der Zuweisung und einem willkürlichen Ausdruck auf der rechten Seite, der implizit in den Typ der linken Seite konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="91ce4-140">On the right hand side of the `with` expression is an `anonymous_object_initializer` with a sequence of assignments with a field or property of the receiver on the left-hand side of the assignment, and an arbitrary expression on the right-hand side which is implicitly convertible to the type of the left-hand side.</span></span>

<span data-ttu-id="91ce4-141">Bei Angabe einer Ziel `With` Methode muss der Rückgabetyp der Typ des Empfänger Ausdrucks Typs oder ein Basistyp sein.</span><span class="sxs-lookup"><span data-stu-id="91ce4-141">Given a target `With` method, the return type must be the type of the receiver expression type, or a base type thereof.</span></span> <span data-ttu-id="91ce4-142">Für jeden Parameter der `With`-Methode muss ein barrierefreies entsprechendes Instanzfeld oder eine lesbare Eigenschaft für den Empfängertyp mit demselben Namen und demselben Typ vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="91ce4-142">For each parameter of the `With` method, there must be an accessible corresponding instance field or readable property on the receiver type with the same name and the same type.</span></span> <span data-ttu-id="91ce4-143">Jede Eigenschaft oder jedes Feld auf der rechten Seite des with-Ausdrucks muss auch einem Parameter desselben Namens in der `With`-Methode entsprechen.</span><span class="sxs-lookup"><span data-stu-id="91ce4-143">Each property or field in the right-hand side of the With expression must also correspond to a parameter of the same name in the `With` method.</span></span>

<span data-ttu-id="91ce4-144">Wenn eine gültige `With`-Methode vorhanden ist, entspricht die Auswertung eines `with` Ausdrucks dem Aufrufen der `With`-Methode mit den Ausdrücken im `anonymous_object_initializer` die den Parameter mit dem gleichen Namen wie die Eigenschaft auf der linken Seite ersetzt.</span><span class="sxs-lookup"><span data-stu-id="91ce4-144">Given a valid `With` method, the evaluation of a `with` expression is equivalent to calling the `With` method with the expressions in the `anonymous_object_initializer` substituted for the parameter of the same name as the property on the left hand side.</span></span> <span data-ttu-id="91ce4-145">Wenn keine übereinstimmende Eigenschaft für einen angegebenen Parameter in der `anonymous_object_initializer`vorhanden ist, ist das Argument die Auswertung des Felds oder der Eigenschaft mit demselben Namen auf dem Empfänger.</span><span class="sxs-lookup"><span data-stu-id="91ce4-145">If there is no matching property for a given parameter in the `anonymous_object_initializer`, the argument is the evaluation of the field or property of the same name on the receiver.</span></span>

<span data-ttu-id="91ce4-146">Die Reihenfolge der Auswertung von Nebeneffekten sieht wie folgt aus, wobei jeder Ausdruck genau einmal ausgewertet wird:</span><span class="sxs-lookup"><span data-stu-id="91ce4-146">The order of evaluation of side effects is as follows, with each expression evaluated exactly once:</span></span>

1. <span data-ttu-id="91ce4-147">Empfänger Ausdruck</span><span class="sxs-lookup"><span data-stu-id="91ce4-147">Receiver expression</span></span>

2. <span data-ttu-id="91ce4-148">Ausdrücke im `anonymous_object_initializer`in lexikalischer Reihenfolge</span><span class="sxs-lookup"><span data-stu-id="91ce4-148">Expressions in the `anonymous_object_initializer`, in lexical order</span></span>

3. <span data-ttu-id="91ce4-149">Die Auswertung von Eigenschaften, die mit den Parametern der `With` Methode übereinstimmen, in der Reihenfolge der Definition der Parameter der `With` Methode.</span><span class="sxs-lookup"><span data-stu-id="91ce4-149">The evaluation of any properties matching the `With` method parameters, in order of definition of the `With` method parameters.</span></span>