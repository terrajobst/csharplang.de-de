---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281956"
---
# <a name="records-work-in-progress"></a>Datensätze werden in Bearbeitung aufgezeichnet

Anders als bei den anderen Datensätzen, ist dies kein Vorschlag in sich selbst, sondern eine Arbeit in Bearbeitung, die für das Aufzeichnen von Entscheidungen zum Entwurf von Konsens für das Feature "Datensätze" konzipiert ist. Die Spezifikations Details werden bei Bedarf hinzugefügt, um Fragen zu lösen.

Die Syntax für einen Datensatz muss wie folgt hinzugefügt werden:

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

Das `attributes` nicht Terminal ermöglicht außerdem das neue kontextabhängige Attribut `data`.

Eine Klasse (Struktur), die mit einer Parameterliste oder einem `data` Modifizierer deklariert wurde, wird als Daten Satz Klasse (Daten Satzstruktur) bezeichnet, von denen jeder ein Daten Satz Typ ist.

Es ist ein Fehler, einen Daten Satz Typen ohne eine Parameterliste und den `data` Modifizierer zu deklarieren.

## <a name="members-of-a-record-type"></a>Mitglieder eines Daten Satz Typs

Zusätzlich zu den Membern, die im Class-Body deklariert sind, verfügt ein Daten Satz Typen über die folgenden zusätzlichen Member:

### <a name="primary-constructor"></a>Primärer Konstruktor

Ein Daten Recordtyp verfügt über einen öffentlichen Konstruktor, dessen Signatur den Wert Parametern der Typdeklaration entspricht. Dies wird als primärer Konstruktor für den Typ bezeichnet und bewirkt, dass der implizit deklarierte Standardkonstruktor unterdrückt wird. Es ist ein Fehler, dass ein primärer Konstruktor und ein Konstruktor mit der gleichen Signatur bereits in der Klasse vorhanden sind.
Zur Laufzeit der primäre Konstruktor 

1. führt die instanzfeldinitialisierer aus, die in der Klasse Body angezeigt werden. und ruft dann den Basisklassenkonstruktor ohne Argumente auf.

1. Initialisiert vom Compiler generierte Unterstützungs Felder für die Eigenschaften, die den Wert Parametern entsprechen (wenn diese Eigenschaften vom Compiler bereitgestellt werden; siehe [synthetische Eigenschaften](#Synthesized Properties)).


[] TODO: Hinzufügen von grundlegenden aufrufen Syntax und Spezifikation zum Auswählen des basiskonstruktors durch Überladungs Auflösung

### <a name="properties"></a>Eigenschaften

Für jeden Datensatz-Parameter einer Datensatz-Typdeklaration gibt es einen entsprechenden Member der öffentlichen Eigenschaft, dessen Name und Typ aus der Deklaration des Wert Parameters entnommen werden. Wenn keine konkrete (d.h. nicht abstrakte) Eigenschaft mit einem get-Accessor und mit diesem Namen und Typ explizit deklariert oder geerbt wird, wird der Compiler wie folgt erstellt:

Für eine Daten Satzstruktur oder eine Daten Satz Klasse:

* Eine öffentliche automatische Get-only-Eigenschaft wird erstellt. Der Wert wird während der Erstellung mit dem Wert des entsprechenden primären Konstruktorparameters initialisiert. Jeder Get-Accessor der geerbten abstrakten Eigenschaft wird überschrieben.

### <a name="equality-members"></a>Gleichheits Elemente

Daten Satz Typen führen zu synthetisierten Implementierungen für die folgenden Methoden:

* `object.GetHashCode()` außer Kraft setzung, sofern nicht versiegelt oder vom Benutzer bereitgestellt
* `object.Equals(object)` außer Kraft setzung, sofern nicht versiegelt oder vom Benutzer bereitgestellt
* `T Equals(T)` Methode, bei der `T` der aktuelle Typ ist.

`T Equals(T)` wird angegeben, um Wert Gleichheit auszuführen. dabei wird die-Eigenschaft mit demselben Namen wie jeder primäre Konstruktorparameter mit der entsprechenden Eigenschaft des anderen Typs verglichen.
`object.Equals` führt die Entsprechung von

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a>`with`-Ausdruck

Ein `with` Ausdruck ist ein neuer Ausdruck, der die folgende Syntax verwendet.

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

Ein `with` Ausdruck ermöglicht die "nicht zerstörerische Mutation", die eine Kopie des Empfänger Ausdrucks mit Änderungen an den Eigenschaften erzeugt, die im `anonymous_object_initializer`aufgelistet sind.

Ein gültiger `with` Ausdruck hat einen Empfänger mit einem nicht-void-Typ. Der Empfängertyp muss eine barrierefreie Instanzmethode mit dem Namen `With` mit den entsprechenden Parametern und dem Rückgabetyp enthalten. Wenn mehrere `With` Methoden ohne außer Kraft Setzung vorhanden sind, ist ein Fehler aufgetreten. Wenn mehrere `With` außer Kraft setzungen vorhanden sind, muss eine nicht über schreibende `With` Methode vorhanden sein, die die Ziel Methode ist. Andernfalls muss genau eine `With`-Methode vorhanden sein.

Auf der rechten Seite des `with` Ausdrucks handelt es sich um eine `anonymous_object_initializer` mit einer Sequenz von Zuweisungen mit einem Feld oder einer Eigenschaft des Empfängers auf der linken Seite der Zuweisung und einem willkürlichen Ausdruck auf der rechten Seite, der implizit in den Typ der linken Seite konvertiert werden kann.

Bei Angabe einer Ziel `With` Methode muss der Rückgabetyp der Typ des Empfänger Ausdrucks Typs oder ein Basistyp sein. Für jeden Parameter der `With`-Methode muss ein barrierefreies entsprechendes Instanzfeld oder eine lesbare Eigenschaft für den Empfängertyp mit demselben Namen und demselben Typ vorhanden sein. Jede Eigenschaft oder jedes Feld auf der rechten Seite des with-Ausdrucks muss auch einem Parameter desselben Namens in der `With`-Methode entsprechen.

Wenn eine gültige `With`-Methode vorhanden ist, entspricht die Auswertung eines `with` Ausdrucks dem Aufrufen der `With`-Methode mit den Ausdrücken im `anonymous_object_initializer` die den Parameter mit dem gleichen Namen wie die Eigenschaft auf der linken Seite ersetzt. Wenn keine übereinstimmende Eigenschaft für einen angegebenen Parameter in der `anonymous_object_initializer`vorhanden ist, ist das Argument die Auswertung des Felds oder der Eigenschaft mit demselben Namen auf dem Empfänger.

Die Reihenfolge der Auswertung von Nebeneffekten sieht wie folgt aus, wobei jeder Ausdruck genau einmal ausgewertet wird:

1. Empfänger Ausdruck

2. Ausdrücke im `anonymous_object_initializer`in lexikalischer Reihenfolge

3. Die Auswertung von Eigenschaften, die mit den Parametern der `With` Methode übereinstimmen, in der Reihenfolge der Definition der Parameter der `With` Methode.