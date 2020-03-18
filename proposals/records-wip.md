---
ms.openlocfilehash: 25756c1811d5e6dc97512ce70f99ab7fefa91c4a
ms.sourcegitcommit: 2a6dffb60718065ece95df75e1cc7eb509e48a8d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2020
ms.locfileid: "79484129"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="12b60-101">Datensätze werden in Bearbeitung aufgezeichnet</span><span class="sxs-lookup"><span data-stu-id="12b60-101">Records Work-in-Progress</span></span>

<span data-ttu-id="12b60-102">Anders als bei den anderen Datensätzen, ist dies kein Vorschlag in sich selbst, sondern eine Arbeit in Bearbeitung, die für das Aufzeichnen von Entscheidungen zum Entwurf von Konsens für das Feature "Datensätze" konzipiert ist.</span><span class="sxs-lookup"><span data-stu-id="12b60-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="12b60-103">Die Spezifikations Details werden bei Bedarf hinzugefügt, um Fragen zu lösen.</span><span class="sxs-lookup"><span data-stu-id="12b60-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="12b60-104">Die Syntax für einen Datensatz muss wie folgt hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="12b60-104">The syntax for a record is proposed to be added as follows:</span></span>

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

<span data-ttu-id="12b60-105">Das `attributes` nicht Terminal ermöglicht außerdem das neue kontextabhängige Attribut `data`.</span><span class="sxs-lookup"><span data-stu-id="12b60-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="12b60-106">Eine Klasse (Struktur), die mit einer Parameterliste oder einem `data` Modifizierer deklariert wurde, wird als Daten Satz Klasse (Daten Satzstruktur) bezeichnet, von denen jeder ein Daten Satz Typ ist.</span><span class="sxs-lookup"><span data-stu-id="12b60-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="12b60-107">Es ist ein Fehler, einen Daten Satz Typen ohne eine Parameterliste und den `data` Modifizierer zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="12b60-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="12b60-108">Mitglieder eines Daten Satz Typs</span><span class="sxs-lookup"><span data-stu-id="12b60-108">Members of a record type</span></span>

<span data-ttu-id="12b60-109">Zusätzlich zu den Membern, die im Class-Body deklariert sind, verfügt ein Daten Satz Typen über die folgenden zusätzlichen Member:</span><span class="sxs-lookup"><span data-stu-id="12b60-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="12b60-110">Primärer Konstruktor</span><span class="sxs-lookup"><span data-stu-id="12b60-110">Primary Constructor</span></span>

<span data-ttu-id="12b60-111">Ein Daten Recordtyp verfügt über einen öffentlichen Konstruktor, dessen Signatur den Wert Parametern der Typdeklaration entspricht.</span><span class="sxs-lookup"><span data-stu-id="12b60-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="12b60-112">Dies wird als primärer Konstruktor für den Typ bezeichnet und bewirkt, dass der implizit deklarierte Standardkonstruktor unterdrückt wird.</span><span class="sxs-lookup"><span data-stu-id="12b60-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="12b60-113">Es ist ein Fehler, dass ein primärer Konstruktor und ein Konstruktor mit der gleichen Signatur bereits in der Klasse vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="12b60-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="12b60-114">Zur Laufzeit der primäre Konstruktor</span><span class="sxs-lookup"><span data-stu-id="12b60-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="12b60-115">führt die instanzfeldinitialisierer aus, die in der Klasse Body angezeigt werden. und ruft dann den Basisklassenkonstruktor ohne Argumente auf.</span><span class="sxs-lookup"><span data-stu-id="12b60-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="12b60-116">Initialisiert vom Compiler generierte Unterstützungs Felder für die Eigenschaften, die den Wert Parametern entsprechen (wenn diese Eigenschaften vom Compiler bereitgestellt werden; siehe [synthetische Eigenschaften](#Synthesized Properties)).</span><span class="sxs-lookup"><span data-stu-id="12b60-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="12b60-117">[] TODO: Hinzufügen von grundlegenden aufrufen Syntax und Spezifikation zum Auswählen des basiskonstruktors durch Überladungs Auflösung</span><span class="sxs-lookup"><span data-stu-id="12b60-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="12b60-118">Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="12b60-118">Properties</span></span>

<span data-ttu-id="12b60-119">Für jeden Datensatz-Parameter einer Datensatz-Typdeklaration gibt es einen entsprechenden Member der öffentlichen Eigenschaft, dessen Name und Typ aus der Deklaration des Wert Parameters entnommen werden.</span><span class="sxs-lookup"><span data-stu-id="12b60-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="12b60-120">Wenn keine konkrete (d.h. nicht abstrakte) Eigenschaft mit einem get-Accessor und mit diesem Namen und Typ explizit deklariert oder geerbt wird, wird der Compiler wie folgt erstellt:</span><span class="sxs-lookup"><span data-stu-id="12b60-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="12b60-121">Für eine Daten Satzstruktur oder eine Daten Satz Klasse:</span><span class="sxs-lookup"><span data-stu-id="12b60-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="12b60-122">Eine öffentliche automatische Get-only-Eigenschaft wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="12b60-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="12b60-123">Der Wert wird während der Erstellung mit dem Wert des entsprechenden primären Konstruktorparameters initialisiert.</span><span class="sxs-lookup"><span data-stu-id="12b60-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="12b60-124">Jeder Get-Accessor der geerbten abstrakten Eigenschaft wird überschrieben.</span><span class="sxs-lookup"><span data-stu-id="12b60-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="12b60-125">Gleichheits Elemente</span><span class="sxs-lookup"><span data-stu-id="12b60-125">Equality members</span></span>

<span data-ttu-id="12b60-126">Daten Satz Typen führen zu synthetisierten Implementierungen für die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="12b60-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="12b60-127">`object.GetHashCode()` außer Kraft setzung, sofern nicht versiegelt oder vom Benutzer bereitgestellt</span><span class="sxs-lookup"><span data-stu-id="12b60-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="12b60-128">`object.Equals(object)` außer Kraft setzung, sofern nicht versiegelt oder vom Benutzer bereitgestellt</span><span class="sxs-lookup"><span data-stu-id="12b60-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="12b60-129">`T Equals(T)` Methode, bei der `T` der aktuelle Typ ist.</span><span class="sxs-lookup"><span data-stu-id="12b60-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="12b60-130">`T Equals(T)` wird angegeben, um Wert Gleichheit auszuführen. dabei wird die-Eigenschaft mit demselben Namen wie jeder primäre Konstruktorparameter mit der entsprechenden Eigenschaft des anderen Typs verglichen.</span><span class="sxs-lookup"><span data-stu-id="12b60-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="12b60-131">`object.Equals` führt die Entsprechung von</span><span class="sxs-lookup"><span data-stu-id="12b60-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```
