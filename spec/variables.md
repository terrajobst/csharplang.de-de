---
ms.openlocfilehash: b7bb7dd575d9e2e6d5dd85bdd3e535411e29fcf4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488866"
---
# <a name="variables"></a>Variablen

Variablen stellen Speicherorte dar. Jede Variable hat einen Typ, der bestimmt, welche Werte können in der Variablen gespeichert. C# ist eine typsichere-Sprache, und der C#-Compiler garantiert, dass in Variablen gespeicherten Werte immer des entsprechenden Typs. Der Wert einer Variablen kann geändert werden, über die Zuweisung oder durch die `++` und `--` Operatoren.

Eine Variable muss ***definitiv zugewiesen*** ([definitive Zuweisung](variables.md#definite-assignment)), bevor ihr Wert abgerufen werden kann.

Wie in den folgenden Abschnitten beschrieben werden, sind Variablen ***ursprünglich zugewiesenen*** oder ***anfänglich nicht zugewiesene***. Eine anfänglich zugewiesene Variable verfügt über eine klar definierte Anfangswert und wird immer als definitiv zugewiesen. Eine anfänglich nicht zugewiesene Variable verfügt über keinen Anfangswert. Für eine anfänglich nicht zugewiesene Variable definitiv zugewiesen an einem bestimmten Standort berücksichtigt werden muss eine Zuweisung der Variablen in jedem möglichen Ausführungspfad für diesen Speicherort führende auftreten.

## <a name="variable-categories"></a>Variable Kategorien

C# definiert sieben Kategorien von Variablen: statische Variablen, Instanzvariablen, Elemente des Arrays, Wertparameter, Verweisparameter, Ausgabeparameter und lokale Variablen. Die folgenden Abschnitten wird jede dieser Kategorien beschrieben.

Im Beispiel
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
`x` ist eine statische Variable `y` ist eine Instanzvariable `v[0]` ist ein Arrayelement `a` Werteparameter ist ein, `b` ist ein Verweisparameter `c` ist ein Output-Parameter und `i` ist eine lokale Variable.

### <a name="static-variables"></a>Statische Variablen

Ein Feld deklariert, mit der `static` Modifizierer wird aufgerufen, eine ***statische Variable***. Wird erstellt, die vor der Ausführung der statische Konstruktor, eine statische Variable ([statische Konstruktoren](classes.md#static-constructors)) für seine enthaltenden Typ und mehr vorhanden ist, wenn die Domäne für die zugeordnete Anwendung ist nicht mehr vorhanden.

Der Anfangswert einer statischen Variablen ist der Standardwert ([Standardwerte](variables.md#default-values)) der den Typ der Variablen.

Für Zwecke der definitive Zuweisungen zu überprüfen gilt eine statische Variable als ursprünglich zugewiesen.

### <a name="instance-variables"></a>Instanzvariablen

Ein Feld deklariert, ohne die `static` Modifizierer wird aufgerufen, eine ***Instanzvariable***.

#### <a name="instance-variables-in-classes"></a>Instanzvariablen in Klassen

Eine Instanzvariable für eine Klasse wird erstellt, wenn eine neue Instanz dieser Klasse erstellt wird, und ist nicht vorhanden sein mehr, wenn keine Verweise auf diese Instanz vorhanden sind und der Destruktor der Instanz (falls vorhanden) ausgeführt.

Der Anfangswert einer Variablen Instanz einer Klasse ist der Standardwert ([Standardwerte](variables.md#default-values)) der den Typ der Variablen.

Für definitive Zuweisungen zu überprüfen, gilt eine Instanzvariable für eine Klasse als ursprünglich zugewiesen.

#### <a name="instance-variables-in-structs"></a>Instanzvariablen in Strukturen

Eine Instanzvariable für eine Struktur verfügt über genau dieselbe Lebensdauer wie die Strukturvariable, die zu der er gehört. Anders gesagt, wenn eine Variable eines Strukturtyps wird erstellt, oder ist nicht mehr vorhanden, daher führen Sie die Nachrichteninstanzvariablen der Struktur zu.

Die anfängliche Zuweisung-Status, der eine Instanzvariable für eine Struktur ist identisch, die von der enthaltenden Struktur-Variablen. Das heißt, wenn eine Strukturvariable als ist zugewiesen, also auch die zugehörigen Instanzvariablen, und wenn eine Strukturvariable als anfänglich kein Wert zugewiesen wird, sind die Instanzvariablen ebenso nicht zugewiesen.

### <a name="array-elements"></a>Array-Elemente

Die Elemente eines Arrays sind vorhanden, wenn eine Arrayinstanz erstellt wird, und nicht mehr vorhanden ist, wenn keine Verweise auf diese Arrayinstanz vorhanden sind.

Der Anfangswert aller Elemente eines Arrays ist der Standardwert ([Standardwerte](variables.md#default-values)) des Typs der Elemente des Arrays.

Für definitive Zuweisungen zu überprüfen, gilt ein Arrayelement anfänglich zugewiesen.

### <a name="value-parameters"></a>Wert-Parametern

Ein Parameter deklariert, ohne eine `ref` oder `out` Modifizierer ist ein ***"Value"-Parameter***.

Wird erstellt, die beim Aufruf der Funktionsmember der Member (Methode, Instanzenkonstruktor, Accessor oder -Operator) oder der anonymen Funktion, ein Value-Parameter in der der Parameter gehört, und initialisiert wird, mit dem Wert des Arguments im Aufruf angegeben. Ein Value-Parameter wird normalerweise bei der Rückgabe der Funktionsmember der Member oder der anonymen Funktion gelöscht. Allerdings wird der Value-Parameter von einer anonymen Funktion erfasst ([anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions)), dessen Lebensdauer mindestens erweitert, bis der Delegat oder Ausdrucksbaumstruktur, die erstellt, die von dieser anonymen Funktion ist für berechtigte eine Garbagecollection.

Für definitive Zuweisungen zu überprüfen, gilt ein Value-Parameter als ursprünglich zugewiesen.

### <a name="reference-parameters"></a>Verweisparameter

Deklarierter Parameter mit einem `ref` Modifizierer ist ein ***Verweisparameter***.

Ein Verweisparameter wird nicht mit einen neuen Speicherort erstellt. Stattdessen stellt ein Verweisparameter am gleichen Speicherort wie die Variable als Argument in der Funktionsmember der Member oder eine anonyme Funktionsaufruf angegeben. Der Wert der Verweisparameter ist daher immer identisch mit der zugrunde liegenden Variablen.

Die folgenden Regeln für definitive Zuweisungen gelten Parameter zu verweisen. Beachten Sie die Regeln für Output-Parameter, die in beschriebenen [Ausgabeparameter](variables.md#output-parameters).

*  Eine Variable definitiv zugewiesen werden muss ([definitive Zuweisung](variables.md#definite-assignment)), bevor es als in einem Funktionsaufruf Member oder der Delegat Verweisparameter übergeben werden kann.
*  Ein Verweisparameter in einer Funktionsmember der Member oder eine anonyme Funktion wird als ursprünglich zugewiesenen betrachtet.

Innerhalb einer Instanzenmethode oder Instanzaccessor eines Strukturtyps der `this` -Schlüsselwort verhält sich genau wie ein Verweisparameter des Strukturtyps ([diesen Zugriff](expressions.md#this-access)).

### <a name="output-parameters"></a>Output-Parameter

Ein Parameter deklariert, mit einer `out` Modifizierer ist ein ***output-Parameter***.

Ein Output-Parameter wird nicht mit einen neuen Speicherort erstellt. Stattdessen stellt ein Output-Parameter am gleichen Speicherort wie die Variable als Argument im Funktionsaufruf Member oder Delegaten angegeben. Daher ist der Wert des Output-Parameter immer identisch mit der zugrunde liegenden Variablen.

Die folgenden Regeln für definitive Zuweisungen gelten für Ausgabeparameter. Beachten Sie die Regeln für Verweisparameter, die in beschriebenen [Verweisparameter](variables.md#reference-parameters).

*  Eine Variable muss nicht definitiv zugewiesen werden, bevor es Delegataufruf oder können als Output-Parameter in einem Funktionsmember übergeben werden.
*  In diesem Ausführungspfad zugewiesen jede Variable, die übergeben wurde, als Ausgabeparameter betrachtet wird, gemäß dem normalen Abschluss einen Funktionsaufruf Member oder der Delegat wird.
*  In einer Funktionsmember der Member oder eine anonyme Funktion gilt ein Output-Parameter anfänglich kein Wert zugewiesen.
*  Alle Ausgabeparameter einer Funktionsmember der Member oder eine anonyme Funktion muss definitiv zugewiesen ([definitive Zuweisung](variables.md#definite-assignment)) vor der Funktion Elements oder einer anonymen Funktion normalerweise zurückgegeben.

Innerhalb eines Instanzkonstruktors eines Strukturtyps der `this` -Schlüsselwort verhält sich genau wie ein Output-Parameter des Strukturtyps ([diesen Zugriff](expressions.md#this-access)).

### <a name="local-variables"></a>Lokale Variablen

Ein ***lokale Variable*** wird deklariert, indem eine *Local_variable_declaration*, die möglicherweise auftreten, eine *Block*, *For_statement*, eine *Switch_statement* oder *Using_statement*; oder durch eine *Foreach_statement* oder *Specific_catch_clause* für eine *Try_statement*.

Die Lebensdauer einer lokalen Variablen ist der Teil der programmausführung, die während der Speicherung garantiert ist, dafür reserviert werden sollen. Diese Lebensdauer erstreckt sich über mindestens von Eintrag in der *Block*, *For_statement*, *Switch_statement*, *Using_statement*, *Foreach_statement*, oder *Specific_catch_clause* mit dem es zugeordnet ist, bis zur Ausführung dieser ist *Block*, *For_statement*, *Switch_statement*, *Using_statement*, *Foreach_statement*, oder *Specific_catch_clause* endet in keiner Weise. (Eingeben einer eingeschlossenen *Block* oder Aufrufen einer Methode wird angehalten, aber nicht beendet, Ausführung der aktuellen *Block*, *For_statement*, *Switch_statement* , *Using_statement*, *Foreach_statement*, oder *Specific_catch_clause*.) Wenn die lokale Variable, die von einer anonymen Funktion erfasst werden ([äußere Variablen erfasst](expressions.md#captured-outer-variables)), seine Lebensdauer mindestens erweitert, bis die Delegat- oder -Struktur, die von der anonymen Funktion, sowie alle anderen Objekte, die enthalten sind, erstellt die erfassten Variablen verweisen, sind für die Garbagecollection.

Wenn das übergeordnete Element *Block*, *For_statement*, *Switch_statement*, *Using_statement*, *Foreach_statement*, oder *Specific_catch_clause* eingegeben wird rekursiv, eine neue Instanz der lokalen Variablen wird jedes Mal erstellt und die zugehörige *Local_variable_initializer*ausgewertet, falls vorhanden, ist jedes Mal.

Eine lokale Variable, die eingeführt werden, indem eine *Local_variable_declaration* wird nicht automatisch initialisiert und somit hat keinen Standardwert. Für definitive Zuweisungen zu überprüfen, eine lokale Variable eingeführt, durch eine *Local_variable_declaration* gilt anfänglich kein Wert zugewiesen. Ein *Local_variable_declaration* eventuell eine *Local_variable_initializer*, in diesem Fall die Variable definitiv zugewiesen erst nach der Initialisierungsausdruck gilt ([ Deklarationsanweisungen](variables.md#declaration-statements)).

Innerhalb des Bereichs einer lokalen Variablen, die eingeführt werden, indem eine *Local_variable_declaration*, es ist ein Fehler während der Kompilierung zu verweisen auf die lokale Variable in der Lage, Text vor dessen *Local_variable_declarator*. Wenn es sich bei der Deklaration lokale Variable implizit ist ([lokale Variablendeklarationen](statements.md#local-variable-declarations)), es ist auch ein Fehler zum Verweisen auf die Variable in der *Local_variable_declarator*.

Eine lokale Variable, die eingeführt werden, indem eine *Foreach_statement* oder *Specific_catch_clause* gilt im gesamten Bereich definitiv zugewiesen.

Die tatsächliche Lebensdauer einer lokalen Variablen ist implementierungsabhängig. Beispielsweise kann ein Compiler statisch bestimmen, dass eine lokale Variable in einem Block nur für einen kleinen Teil des Blocks verwendet wird. Verwenden diese Analyse, kann der Compiler Code generieren, die den Wert der Variablen Speicher müssen eine kürzere Lebensdauer hat als des enthaltenden Blocks ergeben.

Verweist auf eine lokale Verweisvariable Speicher freigegeben wird, unabhängig von der Lebensdauer der Verweis auf lokale Variablen ([automatische Speicherverwaltung](basic-concepts.md#automatic-memory-management)).

## <a name="default-values"></a>Standardwerte

Die folgenden Kategorien von Variablen werden automatisch auf ihre Standardwerte initialisiert:

*  Statische Variablen.
*  Instanzvariablen von Klasseninstanzen.
*  Elemente des Arrays.

Der Standardwert einer Variablen, hängt vom Typ der Variable und wird wie folgt bestimmt:

*  Für eine Variable vom eine *Value_type*, der Standardwert ist identisch mit dem Wert berechnet, indem die *Value_type*des Standard-Konstruktor ([Standardkonstruktoren](types.md#default-constructors)).
*  Für eine Variable mit einem *Reference_type*, der Standardwert ist `null`.

Initialisierung mit Standardwerten erfolgt in der Regel, dass des Speicher-Managers oder Garbage Collection Arbeitsspeicher, um alle Bits auf 0 initialisiert werden, bevor es für die Verwendung zugeordnet ist. Aus diesem Grund ist es sinnvoll, alle Bits auf 0 zu verwenden, um die null-Verweis darstellen.

## <a name="definite-assignment"></a>Definitive Zuweisung

An einer bestimmten Position in den ausführbaren Code, der ein Funktionsmember werden soll, eine Variable gilt als ***definitiv zugewiesen*** Wenn der Compiler, durch eine bestimmte statische Flussanalyse nachweisen kann ([präzise Regeln für definitive ermitteln Zuweisung](variables.md#precise-rules-for-determining-definite-assignment)), die die Variable automatisch initialisiert wurde oder das Ziel mindestens eine Zuweisung wurde. Informell erwähnt, werden die Regeln für definitive Zuweisungen:

*  Ein anfänglich zugewiesene Variable ([anfänglich zugewiesene Variablen](variables.md#initially-assigned-variables)) immer als definitiv zugewiesen.
*  Ein anfänglich nicht zugewiesene Variable ([anfänglich nicht zugewiesene Variablen](variables.md#initially-unassigned-variables)) gilt, definitiv zugewiesen an einer bestimmten Position wenn sämtliche möglichen ausführungswege zu diesem Speicherort mindestens eine der folgenden enthalten:
    * Eine einfache Zuweisung ([einfache Zuweisung](expressions.md#simple-assignment)) in dem die Variable der linke Operand ist.
    * Ein Aufrufausdruck ([Aufrufausdrücke](expressions.md#invocation-expressions)) oder Objekterstellungsausdruck ([Erstellung Objektausdrücke](expressions.md#object-creation-expressions)), die die Variable als Output-Parameter übergibt.
    * Für eine lokale Variable einer lokalen Variablendeklaration ([lokale Variablendeklarationen](statements.md#local-variable-declarations)), die einem Variableninitialisierer enthält.

Die formale Spezifikation, die zugrunde liegende die oben genannten informellen Regeln finden Sie im [anfänglich zugewiesene Variablen](variables.md#initially-assigned-variables), [anfänglich nicht zugewiesene Variablen](variables.md#initially-unassigned-variables), und [präzise Regeln für die Bestimmung definitive Zuweisung](variables.md#precise-rules-for-determining-definite-assignment).

Die definitive Zuweisung Instanzvariablen der Status einer *Struct_type* Variable einzeln sowie als zusammen nachverfolgt werden. In den oben genannten Regeln zusätzliche die folgenden Regeln gelten für *Struct_type* Variablen und ihre Instanzvariablen:

*  Eine Instanzvariable gilt als definitiv zugewiesen, wenn die enthaltende *Struct_type* gilt als Variable definitiv zugewiesen.
*  Ein *Struct_type* Variablen wird definitiv zugewiesenen betrachtet, wenn jede der zugehörigen Instanzvariablen definitiv zugewiesenen betrachtet wird.

Definitive Zuweisung ist eine Voraussetzung in den folgenden Kontexten:

*  Eine Variable muss definitiv an jedem Standort zugewiesen werden, in denen der Wert abgerufen wird. Dadurch wird sichergestellt, dass nicht definierte Werte nie auftreten. Das Vorhandensein einer Variablen in einem Ausdruck wird betrachtet, um den Wert der Variablen an, außer wenn abzurufen
    * die Variable wird der linke Operand des eine einfache Zuweisung,
    * die Variable als ein Output-Parameter übergeben wird oder
    * die Variable ist ein *Struct_type* Variable und tritt ein, wenn der linke Operand des Memberzugriff.
*  Eine Variable muss definitiv an jedem Standort zugewiesen werden, in denen sie als Verweisparameter übergeben wird. Dadurch wird sichergestellt, dass die aufgerufene Funktionsmember den Verweisparameter, der ursprünglich zugewiesenen berücksichtigt werden kann.
*  Ein Funktionsmember alle Ausgabeparameter müssen an jedem Standort, der das Funktionselement, bei dem zurück definitiv zugewiesen werden (über eine `return` Anweisung oder durch die Ausführung, die das Ende der Hauptteil der Funktion Mitglied erreicht). Dadurch wird sichergestellt, dass Funktionsmember keine nicht definierte Werte in der Output-Parameter zurückgeben, sodass des Compilers einen Member-Funktionsaufruf berücksichtigt, der eine Variable als Output-Parameter akzeptiert wird, der eine Zuweisung zur Variablen entspricht.
*  Die `this` der Variable ein *Struct_type* Instanzkonstruktor muss definitiv zugewiesen werden, an jedem Speicherort, in denen dieser Instanzkonstruktor zurückgibt.

### <a name="initially-assigned-variables"></a>Ursprünglich zugewiesenen Variablen

Die folgenden Kategorien von Variablen sind klassifiziert als ursprünglich zugewiesen:

*  Statische Variablen.
*  Instanzvariablen von Klasseninstanzen.
*  Instanzvariablen des ursprünglich zugewiesenen Strukturvariablen.
*  Elemente des Arrays.
*  -Parameter.
*  Reference-Parameter.
*  Variablen, die in einem `catch` Klausel oder eine `foreach` Anweisung.

### <a name="initially-unassigned-variables"></a>Ursprünglich zugewiesenen Variablen

Die folgenden Kategorien von Variablen sind, wie es ursprünglich nicht zugewiesene klassifiziert:

*  Instanzvariablen von Strukturvariablen anfänglich kein Wert zugewiesen.
*  Output-Parameter, einschließlich der `this` Struktur Instanzkonstruktoren Variable.
*  Deklariert lokale Variablen, mit Ausnahme einer `catch` Klausel oder eine `foreach` Anweisung.

### <a name="precise-rules-for-determining-definite-assignment"></a>Präzise Regeln für definitive Zuweisungen ermitteln

Um zu bestimmen, dass jede verwendete Variable definitiv zugewiesen ist, muss der Compiler einen Prozess verwenden, der mit dem in diesem Abschnitt beschriebenen entspricht.

Der Compiler verarbeitet den Text der jeder Funktionsmember, die eine oder mehrere anfänglich nicht zugewiesene Variablen. Für jede anfänglich nicht zugewiesene Variable *v*, der Compiler bestimmt eine ***definitive zuweisungszustand*** für *v* an jedem der folgenden Punkte in der Funktionsmember der Member:

*  Am Anfang jeder Anweisung
*  Am Endpunkt ([Endpunkte und Erreichbarkeit](statements.md#end-points-and-reachability)) der einzelnen Anweisungen
*  Für jeden Bogen übergibt die Steuerung an eine andere Anweisung oder den Endpunkt einer Anweisung
*  Am Anfang jeder Ausdruck
*  Am Ende jeder Ausdruck

Die definitive Zuweisung Zustand des *v* kann es sich um:

*  Definitiv zugewiesen. Dies bedeutet, dass alle möglichen Flows an diesem Punkt ist *v* einen Wert zugewiesen wurde.
*  Nicht definitiv zugewiesen. Für den Status einer Variablen am Ende ein Ausdruck vom Typ `bool`, der Status einer Variablen, die Mai wird nicht definitiv zugewiesen (jedoch nicht notwendigerweise) fallen in eine der folgenden untergeordneten Status:
    * Nach der als TRUE ausgewertete Ausdruck definitiv zugewiesen. Dieser Status Gibt an, dass *v* ist definitiv zugewiesen werden, wenn der boolesche Ausdruck als True ausgewertet, aber es nicht notwendigerweise zugewiesen ist, wenn der boolesche Ausdruck als "false" ausgewertet.
    * Nach der false-Ausdruck definitiv zugewiesen. Dieser Status Gibt an, dass *v* ist definitiv zugewiesen werden, wenn der boolesche Ausdruck als "false" ausgewertet, aber es nicht notwendigerweise zugewiesen ist, wenn der boolesche Ausdruck als "true" ausgewertet.

Die folgenden Regeln bestimmen, wie der Status einer Variablen *v* richtet sich an jedem Standort.

#### <a name="general-rules-for-statements"></a>Allgemeine Regeln für Anweisungen

*  *V* wird am Anfang eines Funktionsrumpfs der Member nicht definitiv zugewiesen.
*  *V* wird zu Beginn nicht erreichbare Anweisungen definitiv zugewiesen.
*  Die definitive Zuweisung Zustand des *v* am Anfang eine weitere Anweisung richtet sich nach Überprüfen des Zustands definitive Zuweisung *v* auf alle Steuerelement-Flow-Übertragungen, die den Anfang, das als Ziel -Anweisung. Wenn (und nur wenn) *v* definitiv zugewiesen, auf alle diese Control Flow-Übertragungen, klicken Sie dann *v* definitiv zugewiesen, die zu Beginn der Anweisung. Der Satz von möglichen Flow Übertragungen wird bestimmt, auf die gleiche Weise wie bei der Anweisung Erreichbarkeit überprüfen ([Endpunkte und Erreichbarkeit](statements.md#end-points-and-reachability)).
*  Die definitive Zuweisung Zustand des *v* am Endpunkt eines Blocks `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, oder `switch` Anweisung richtet sich nach der Überprüfung des definitive Zuweisung *v* auf alle Steuerelement-Flow-Übertragungen, die auf den Endpunkt, der diese Anweisung abzielen. Wenn *v* definitiv zugewiesen, auf alle diese Control Flow-Übertragungen, klicken Sie dann *v* definitiv auf den Endpunkt der Anweisung zugewiesen wird. Andernfalls *v* am Endpunkt der Anweisung nicht definitiv zugewiesen. Der Satz von möglichen Flow Übertragungen wird bestimmt, auf die gleiche Weise wie bei der Anweisung Erreichbarkeit überprüfen ([Endpunkte und Erreichbarkeit](statements.md#end-points-and-reachability)).

#### <a name="block-statements-checked-and-unchecked-statements"></a>Block-Anweisungen, die dieses Kontrollkästchen aktiviert, und deaktiviert-Anweisungen

Die definitive Zuweisung Zustand des *v* Übertragung an die erste Anweisung der Anweisungsliste im-Block (oder am Ende des Blocks, wenn die Anweisungsliste leer ist) für das Steuerelement ist identisch mit der Anweisung definitive Zuweisung *v* vor dem Block, `checked`, oder `unchecked` Anweisung.

#### <a name="expression-statements"></a>Ausdrucksanweisungen

Für eine Ausdrucksanweisung *Stmt* , der den Ausdruck aus *Expr*:

*  *V* hat den gleichen definitive zuweisungszustand am Anfang des *Expr* wie am Anfang des *Stmt*.
*  Wenn *v* Wenn am Ende der definitiv zugewiesen *Expr*, es ist definitiv zugewiesen, auf den Endpunkt des *Stmt*; andernfalls ist es nicht auf jeden Fall an den Endpunkt des zugewiesen*Stmt*.

#### <a name="declaration-statements"></a>Deklarationsanweisungen

*  Wenn *Stmt* wird von eine deklarationsanweisung ohne Initialisierer, dann *v* hat den gleichen definitive zuweisungszustand an den Endpunkt des *Stmt* wie am Anfang des *Stmt*.
*  Wenn *Stmt* befindet sich auf eine deklarationsanweisung mit Initialisierern, klicken Sie dann die definitive Zuweisung im Status *v* wird bestimmt, als ob *Stmt* wurden eine Anweisungsliste, mit der eine Zuordnung Anweisung für jede Deklaration mit einem Initialisierer (in der Reihenfolge der Deklaration).

#### <a name="if-statements"></a>Wenn Anweisungen

Für eine `if` Anweisung *Stmt* des Formulars:
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *V* hat den gleichen definitive zuweisungszustand am Anfang des *Expr* wie am Anfang des *Stmt*.
*  Wenn *v* definitiv zugewiesen, am Ende der *Expr*, und klicken Sie dann sie definitiv zugewiesen ist, auf die Steuerelement-Flow-Übertragung an *Then_stmt* und entweder *Else_stmt*  oder der Endpunkt des *Stmt* , falls keine else-Klausel vorhanden ist.
*  Wenn *v* hat den Status "definitiv zugewiesen nach" true "Ausdruck" am Ende der *Expr*, und klicken Sie dann sie definitiv zugewiesen ist, auf die Steuerelement-Flow-Übertragung an *Then_stmt*, und nicht definitiv zugewiesen, auf die Steuerelement-Flow-Übertragung entweder *Else_stmt* oder der Endpunkt des *Stmt* , falls keine else-Klausel vorhanden ist.
*  Wenn *v* hat den Status "definitiv zugewiesen nach" false "Ausdruck" am Ende der *Expr*, und klicken Sie dann sie definitiv zugewiesen ist, auf die Steuerelement-Flow-Übertragung an *Else_stmt*, und nicht definitiv zugewiesen, auf die Steuerelement-Flow-Übertragung an *Then_stmt*. Es ist definitiv zugewiesen, auf den Endpunkt des *Stmt* nur, wenn es an der Endpunkt des definitiv zugewiesen ist *Then_stmt*.
*  Andernfalls *v* als nicht definitiv zugewiesen, auf die Steuerelement-Flow-Übertragung entweder die *Then_stmt* oder *Else_stmt*, oder der Endpunkt des  *Stmt* , falls keine else-Klausel vorhanden ist.

#### <a name="switch-statements"></a>Switch-Anweisungen

In einem `switch` Anweisung *Stmt* mit einem steuernden Ausdruck *Expr*:

*  Die definitive Zuweisung Zustand des *v* am Anfang des *Expr* ist identisch mit den Status der *v* am Anfang des *Stmt*.
*  Die definitive Zuweisung Zustand des *v* auf dem Steuerungsfluss Übertragung einer erreichbar Switch-Anweisung Blockierungsliste ist identisch mit den definitive Zuweisung Zustand des *v* am Ende der *Expr*.

#### <a name="while-statements"></a>While-Anweisungen

Für eine `while` Anweisung *Stmt* des Formulars:
```csharp
while ( expr ) while_body
```

*  *V* hat den gleichen definitive zuweisungszustand am Anfang des *Expr* wie am Anfang des *Stmt*.
*  Wenn *v* definitiv zugewiesen, am Ende der *Expr*, und klicken Sie dann sie definitiv zugewiesen ist, auf die Steuerelement-Flow-Übertragung an *While_body* und den Endpunkt des  *Stmt*.
*  Wenn *v* hat den Status "definitiv zugewiesen nach" true "Ausdruck" am Ende der *Expr*, und klicken Sie dann sie definitiv zugewiesen ist, auf die Steuerelement-Flow-Übertragung an *While_body*, aber nicht der Endpunkt des definitiv zugewiesen *Stmt*.
*  Wenn *v* hat den Status "definitiv zugewiesen nach" false "Ausdruck" am Ende der *Expr*, und sie definitiv, auf die Steuerelement-Flow-Übertragung an den Endpunkt des zugewiesen ist *Stmt* , aber nicht definitiv zugewiesen ist, auf die Steuerelement-Flow-Übertragung an *While_body*.

#### <a name="do-statements"></a>Führen Sie die Anweisungen

Für eine `do` Anweisung *Stmt* des Formulars:
```csharp
do do_body while ( expr ) ;
```

*  *V* hat den gleichen definitive zuweisungszustand auf die Steuerelement-Flow-Übertragung vom Anfang des *Stmt* zu *Do_body* wie am Anfang des *Stmt*.
*  *V* hat den gleichen definitive zuweisungszustand am Anfang des *Expr* wie in den Endpunkt des *Do_body*.
*  Wenn *v* definitiv zugewiesen, am Ende der *Expr*, und sie definitiv, auf die Steuerelement-Flow-Übertragung an den Endpunkt des zugewiesen ist *Stmt*.
*  Wenn *v* hat den Status "definitiv zugewiesen nach" false "Ausdruck" am Ende der *Expr*, und sie definitiv, auf die Steuerelement-Flow-Übertragung an den Endpunkt des zugewiesen ist *Stmt* .

#### <a name="for-statements"></a>Für Anweisungen

Definitive Zuweisungen für die Überprüfung einer `for` -Anweisung der Form:
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
erfolgt, als ob die Anweisung geschrieben wurden:
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

Wenn die *For_condition* in weggelassen wird die `for` -Anweisung, und klicken Sie dann auf Auswertung definitive Zuweisung fortgesetzt wie *For_condition* wurden durch ersetzt `true` in der oben genannten Erweiterung .

#### <a name="break-continue-and-goto-statements"></a>Unterbrechen, den Vorgang fortzusetzen und die Goto-Anweisungen

Die definitive Zuweisung Zustand des *v* auf die Steuerelement-Flow-Übertragung aufgrund einer `break`, `continue`, oder `goto` -Anweisung ist identisch mit den definitive Zuweisung Zustand des *v* an die Beginn der Anweisung.

#### <a name="throw-statements"></a>Throw-Anweisungen

Für eine Anweisung *Stmt* des Formulars
```csharp
throw expr ;
```

Die definitive Zuweisung Zustand des *v* am Anfang des *Expr* ist identisch mit den definitive Zuweisung Zustand des *v* am Anfang des *Stmt*.

#### <a name="return-statements"></a>Return-Anweisungen

Für eine Anweisung *Stmt* des Formulars
```csharp
return expr ;
```

*  Die definitive Zuweisung Zustand des *v* am Anfang des *Expr* ist identisch mit den definitive Zuweisung Zustand des *v* am Anfang des *Stmt*.
*  Wenn *v* ist ein Output-Parameter, und es definitiv zugewiesen werden muss:
    * nach dem *Expr*
    * oder am Ende der `finally` -Block eine `try` - `finally` oder `try` - `catch` - `finally` einschließt, die die `return` Anweisung.

Für eine Anweisung Stmt des Formulars:
```csharp
return ;
```

*  Wenn *v* ist ein Output-Parameter, und es definitiv zugewiesen werden muss:
    * vor dem *Stmt*
    * oder am Ende der `finally` -Block eine `try` - `finally` oder `try` - `catch` - `finally` einschließt, die die `return` Anweisung.

#### <a name="try-catch-statements"></a>Try-Catch-Anweisungen

Für eine Anweisung *Stmt* des Formulars:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  Die definitive Zuweisung Zustand des *v* am Anfang des *Try_block* ist identisch mit den definitive Zuweisung Zustand des *v* am Anfang des *Stmt*.
*  Die definitive Zuweisung Zustand des *v* am Anfang des *Catch_block_i* (für alle *ich*) ist identisch mit den definitive Zuweisung Zustand des *v*am Anfang des *Stmt*.
*  Die definitive Zuweisung Zustand des *v* an den Endpunkt des *Stmt* ist definitiv zugewiesenen If (und nur wenn) *v* definitiv zugewiesen, auf den Endpunkt des  *Try_block* und jede *Catch_block_i* (für jede *ich* von 1 bis *n*).

#### <a name="try-finally-statements"></a>Try-finally-Anweisungen

Für eine `try` Anweisung *Stmt* des Formulars:
```csharp
try try_block finally finally_block
```

*  Die definitive Zuweisung Zustand des *v* am Anfang des *Try_block* ist identisch mit den definitive Zuweisung Zustand des *v* am Anfang des *Stmt*.
*  Die definitive Zuweisung Zustand des *v* am Anfang des *Finally_block* ist identisch mit den definitive Zuweisung Zustand des *v* am Anfang des *Stmt* .
*  Die definitive Zuweisung Zustand des *v* an den Endpunkt des *Stmt* ist definitiv zugewiesenen If (und nur wenn) mindestens eine der folgenden gilt:
    * *V* definitiv zugewiesen, auf den Endpunkt des *Try_block*
    * *V* definitiv zugewiesen, auf den Endpunkt des *Finally_block*

Wenn eine Übertragung der Steuerelement-Flow (z. B. eine `goto` Anweisung) erfolgt, beginnt innerhalb *Try_block*, und endet, die außerhalb von *Try_block*, klicken Sie dann *v* ist auch als definitiv zugewiesen, auf diese Control Flow-Übertragung, wenn *v* definitiv zugewiesen, auf den Endpunkt des *Finally_block*. (Dies ist nicht nur, wenn ein, wenn *v* ist aus einem anderen Grund für diese Control Flow-Übertragung, definitiv zugewiesen, und er weiterhin definitiv zugewiesen gilt.)

#### <a name="try-catch-finally-statements"></a>Try-Catch-finally-Anweisungen

Definite Assignment-Analyse für eine `try` - `catch` - `finally` -Anweisung der Form:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
wird ausgeführt, als wäre die Anweisung eine `try` - `finally` einschließenden Anweisung eine `try` - `catch` Anweisung:
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

Im folgende Beispiel wird veranschaulicht, wie die verschiedenen Blöcke einer `try` Anweisung ([der Try-Anweisung](statements.md#the-try-statement)) Auswirkungen auf die definitive Zuweisung.
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a>Foreach-Anweisungen

Für eine `foreach` Anweisung *Stmt* des Formulars:
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  Die definitive Zuweisung Zustand des *v* am Anfang des *Expr* ist identisch mit den Status der *v* am Anfang des *Stmt*.
*  Die definitive Zuweisung Zustand des *v* auf die Steuerelement-Flow-Übertragung an *Embedded_statement* oder den Endpunkt des *Stmt* ist identisch mit den Status der *v* am Ende der *Expr*.

#### <a name="using-statements"></a>Using-Anweisungen

Für eine `using` Anweisung *Stmt* des Formulars:
```csharp
using ( resource_acquisition ) embedded_statement
```

*  Die definitive Zuweisung Zustand des *v* am Anfang des *Resource_acquisition* ist identisch mit den Status der *v* am Anfang des *Stmt*.
*  Die definitive Zuweisung Zustand des *v* auf die Steuerelement-Flow-Übertragung an *Embedded_statement* ist identisch mit den Status der *v* am Ende der *Resource_ Übernahme*.

#### <a name="lock-statements"></a>Lock-Anweisungen

Für eine `lock` Anweisung *Stmt* des Formulars:
```csharp
lock ( expr ) embedded_statement
```

*  Die definitive Zuweisung Zustand des *v* am Anfang des *Expr* ist identisch mit den Status der *v* am Anfang des *Stmt*.
*  Die definitive Zuweisung Zustand des *v* auf die Steuerelement-Flow-Übertragung an *Embedded_statement* ist identisch mit den Status der *v* am Ende der *Expr*.

#### <a name="yield-statements"></a>Yield-Anweisungen

Für eine `yield return` Anweisung *Stmt* des Formulars:
```csharp
yield return expr ;
```

*  Die definitive Zuweisung Zustand des *v* am Anfang des *Expr* ist identisch mit den Status der *v* am Anfang des *Stmt*.
*  Die definitive Zuweisung Zustand des *v* am Ende der *Stmt* ist identisch mit den Status der *v* am Ende der *Expr*.
*  Ein `yield break` -Anweisung hat keine Auswirkung auf die definitive zuweisungszustand.

#### <a name="general-rules-for-simple-expressions"></a>Allgemeine Regeln für einfache Ausdrücke

Die folgende Regel gilt für diese Arten von Ausdrücken: Literale ([Literale](expressions.md#literals)), einfache Namen ([einfache Namen](expressions.md#simple-names)), Ausdrücke für den Memberzugriff ([Memberzugriff](expressions.md#member-access)), Basiszugriff nicht indizierte Ausdrücke ([Base Access](expressions.md#base-access)), `typeof` Ausdrücke ([der Typeof-Operator](expressions.md#the-typeof-operator)), default Wertausdrücken ([Standardwertausdrücke ](expressions.md#default-value-expressions)) und `nameof` Ausdrücke ([Nameof-Ausdrücken](expressions.md#nameof-expressions)).

*  Die definitive Zuweisung Zustand des *v* am Ende ein solcher Ausdruck entspricht den definitive Zuweisung Zustand des *v* am Anfang des Ausdrucks.

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>Allgemeine Regeln für Ausdrücke mit eingebetteten Ausdrücken

Die folgenden Regeln gelten für diese Arten von Ausdrücken: Ausdrücke in Klammern ([Ausdrücke in Klammern](expressions.md#parenthesized-expressions)), Element Access Expressions ([Elementzugriff](expressions.md#element-access)), Basis Zugriff auf die Ausdrücke mit Indizierung ([Base Access](expressions.md#base-access)), Inkrement- und Dekrement-Ausdrücke ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators), [Präfix-Inkrement und Dekrement-Operatoren](expressions.md#prefix-increment-and-decrement-operators)), Umwandlungsausdrücke ([Umwandlungsausdrücke](expressions.md#cast-expressions)), unäre `+`, `-`, `~`, `*` binäre Ausdrücke `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` Ausdrücke ([arithmetische Operatoren](expressions.md#arithmetic-operators), [Schiebeoperatoren](expressions.md#shift-operators), [Relational und Typtest Operatoren](expressions.md#relational-and-type-testing-operators) [Logische Operatoren](expressions.md#logical-operators)), zusammengesetzte Zuweisungsausdrücke ([Verbundzuweisung](expressions.md#compound-assignment)), `checked` und `unchecked` Ausdrücke ([checked und unchecked Operatoren](expressions.md#the-checked-and-unchecked-operators)), sowie Arrays und Delegaten erstellen Ausdrücke ([des neuen Operators](expressions.md#the-new-operator)).

Jeder dieser Ausdrücke hat einen oder mehrere untergeordnete Ausdrücke, die ohne Bedingung in einer festen Reihenfolge ausgewertet werden. Z. B. die Binärdatei `%` -Operator wertet der linken Seite des Operators, und klicken Sie dann auf der rechten Seite. Ein Indizierungsvorgang wertet den Ausdruck, der indizierten, und jeder Index Ausdrücke aus, in der Reihenfolge von links nach rechts ausgewertet. Für einen Ausdruck *Expr*, Unterausdrücken IValidator.h *e1, e2,..., eN*, die in dieser Reihenfolge ausgewertet:

*  Die definitive Zuweisung Zustand des *v* am Anfang des *e1* ist identisch mit den definitive zuweisungszustand am Anfang des *Expr*.
*  Die definitive Zuweisung Zustand des *v* am Anfang des *Ei* (*ich* größer als 1) ist identisch mit den definitive zuweisungszustand am Ende der vorherigen Unterausdruck.
*  Die definitive Zuweisung Zustand des *v* am Ende der *Expr* ist identisch mit den definitive zuweisungszustand am Ende der *eN*

#### <a name="invocation-expressions-and-object-creation-expressions"></a>Das Aufrufen von Ausdrücken und Erstellung Objektausdrücke

Für ein Aufrufausdruck *Expr* des Formulars:
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
oder ein Objekterstellungsausdruck des Formulars:
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  Für ein Aufrufausdruck, den Status für definitive Zuweisungen der *v* vor *Primary_expression* ist identisch mit den Status der *v* vor *Expr*.
*  Für ein Aufrufausdruck, den Status für definitive Zuweisungen der *v* vor *arg1* ist identisch mit den Status der *v* nach *Primary_expression*.
*  Für eine Objekterstellungsausdruck, den Status für definitive Zuweisungen der *v* vor *arg1* ist identisch mit den Status der *v* vor *Expr*.
*  Für jedes Argument *Argi*, den Status für definitive Zuweisungen der *v* nach *Argi* richtet sich nach den regulären Ausdruck Regeln ignorieren `ref` oder `out`Modifizierer.
*  Für jedes Argument *Argi* für alle *ich* größer als eins, den Status für definitive Zuweisungen der *v* vor *Argi* ist identisch mit den Status der *v* nach der letzten *Arg*.
*  Wenn die Variable *v* übergeben wird, als ein `out` Argument (d. h., ein Argument des Formulars `out v`) in einem der Argumente, und klicken Sie dann auf den Zustand des *v* nach *Expr* ist definitiv zugewiesen werden. Andernfalls der Status der *v* nach *Expr* ist identisch mit den Status der *v* nach *ArgN*.
*  Für Arrayinitialisierer ([Array-Ausdrücke für die Arrayerstellung](expressions.md#array-creation-expressions)), Objektinitialisierer ([Objektinitialisierer](expressions.md#object-initializers)), Auflistungsinitialisierer ([Auflistungsinitialisierer](expressions.md#collection-initializers)) und anonyme Objektinitialisierer ([anonyme Erstellung Objektausdrücke](expressions.md#anonymous-object-creation-expressions)), der Status für definitive Zuweisungen richtet sich nach der Erweiterung, die diese Konstrukte in Form von definiert sind.

#### <a name="simple-assignment-expressions"></a>Ausdrücke für einfache Zuweisung

Für einen Ausdruck *Expr* des Formulars `w = expr_rhs`:

*  Die definitive Zuweisung Zustand des *v* vor *Expr_rhs* ist identisch mit den definitive Zuweisung Zustand des *v* vor *Expr*.
*  Die definitive Zuweisung Zustand des *v* nach *Expr* richtet sich nach:
   * Wenn *w* ist die gleiche Variable als *v*, klicken Sie dann die definitive Zuweisung Zustand des *v* nach *Expr* definitiv zugewiesen.
   * Andernfalls, wenn die Zuweisung im Instanzkonstruktor eines Strukturtyps auftritt, wenn *w* ist ein Eigenschaftenzugriff festlegen eine automatisch implementierte Eigenschaft *P* für die Instanz, die erstellt wird und *v* ist das ausgeblendete dahinter liegende Feld *P*, klicken Sie dann die definitive Zuweisung Zustand des *v* nach *Expr* ist definitiv zugewiesen.
   * Andernfalls die definitive Zuweisung Zustand des *v* nach *Expr* ist identisch mit den definitive Zuweisung Zustand des *v* nach *Expr_rhs*.

#### <a name="-conditional-and-expressions"></a>& & (bedingte AND) Ausdrücke

Für einen Ausdruck *Expr* des Formulars `expr_first && expr_second`:

*  Die definitive Zuweisung Zustand des *v* vor *Expr_first* ist identisch mit den definitive Zuweisung Zustand des *v* vor *Expr*.
*  Die definitive Zuweisung Zustand des *v* vor *Expr_second* wird definitiv zugewiesen werden, wenn der Status der *v* nach *Expr_first* ist entweder definitiv zugewiesen oder "nach der als TRUE ausgewertete Ausdruck definitiv zugewiesenen". Andernfalls ist es nicht definitiv zugewiesen.
*  Die definitive Zuweisung Zustand des *v* nach *Expr* richtet sich nach:
    * Wenn *Expr_first* ist ein konstanter Ausdruck mit dem Wert `false`, klicken Sie dann die definitive Zuweisung Zustand des *v* nach *Expr* ist identisch mit die definitive Zuweisung Status des *v* nach *Expr_first*.
    * Andernfalls gilt: Wenn der Status der *v* nach *Expr_first* definitiv zugewiesen wird, klicken Sie dann den Status der *v* nach *Expr* definitiv zugewiesen.
    * Andernfalls, wenn der Status der *v* nach *Expr_second* definitiv zugewiesen wird, und der Status der *v* nach *Expr_first* ist "auf jeden Fall zugewiesene nach "false" Ausdruck", und klicken Sie dann den Status der *v* nach *Expr* definitiv zugewiesen.
    * Andernfalls gilt: Wenn der Status der *v* nach *Expr_second* definitiv zugewiesen oder "nach der als TRUE ausgewertete Ausdruck definitiv zugewiesenen", und klicken Sie dann den Status der *v* nach  *Expr* "definitiv zugewiesen, nach" true "Ausdruck".
    * Andernfalls gilt: Wenn der Status der *v* nach *Expr_first* ist "nach" false "Ausdruck definitiv zugewiesenen" und den Status der *v* nach *Expr_second* ist "definitiv zugewiesenen nach" false "Ausdruck", und klicken Sie dann den Status der *v* nach *Expr* "definitiv zugewiesen, nach" false "Ausdruck".
    * Andernfalls den Zustand des *v* nach *Expr* nicht definitiv zugewiesen.

Im Beispiel
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
die Variable `i` gilt in einem von der eingebetteten Anweisungen des definitiv zugewiesenen ein `if` Anweisung aber nicht in die andere. In der `if` -Anweisung in der Methode `F`, die Variable `i` wird in der ersten eingebettete Anweisung definitiv zugewiesen, da Ausführung des Ausdrucks `(i = y)` steht immer vor Ausführung dieser eingebetteten Anweisung. Im Gegensatz dazu sind die Variable `i` nicht zugewiesen, in der zweiten eingebettete Anweisung, da `x >= 0` möglicherweise wurden getestet "false", wodurch die Variable `i` aufgehoben.

#### <a name="-conditional-or-expressions"></a>|| (bedingte OR) Ausdrücke

Für einen Ausdruck *Expr* des Formulars `expr_first || expr_second`:

*  Die definitive Zuweisung Zustand des *v* vor *Expr_first* ist identisch mit den definitive Zuweisung Zustand des *v* vor *Expr*.
*  Die definitive Zuweisung Zustand des *v* vor *Expr_second* wird definitiv zugewiesen werden, wenn der Status der *v* nach *Expr_first* ist entweder definitiv zugewiesen oder "nach" false "Ausdruck definitiv zugewiesenen". Andernfalls ist es nicht definitiv zugewiesen.
*  Die definitive Zuweisung-Anweisung von *v* nach *Expr* richtet sich nach:
    * Wenn *Expr_first* ist ein konstanter Ausdruck mit dem Wert `true`, klicken Sie dann die definitive Zuweisung Zustand des *v* nach *Expr* ist identisch mit die definitive Zuweisung Status des *v* nach *Expr_first*.
    * Andernfalls gilt: Wenn der Status der *v* nach *Expr_first* definitiv zugewiesen wird, klicken Sie dann den Status der *v* nach *Expr* definitiv zugewiesen.
    * Andernfalls, wenn der Status der *v* nach *Expr_second* definitiv zugewiesen wird, und der Status der *v* nach *Expr_first* ist "auf jeden Fall zugewiesene nach "true" Ausdruck", und klicken Sie dann den Status der *v* nach *Expr* definitiv zugewiesen.
    * Andernfalls gilt: Wenn der Status der *v* nach *Expr_second* definitiv zugewiesen oder "nach" false "Ausdruck definitiv zugewiesenen", und klicken Sie dann den Status der *v* nach *Expr* "definitiv zugewiesen, nach" false "Ausdruck".
    * Andernfalls gilt: Wenn der Status der *v* nach *Expr_first* ist "nach der als TRUE ausgewertete Ausdruck definitiv zugewiesenen" und den Status der *v* nach *Expr_second*ist "nach der als TRUE ausgewertete Ausdruck definitiv zugewiesenen", und klicken Sie dann den Status der *v* nach *Expr* "definitiv zugewiesen, nach" true "Ausdruck".
    * Andernfalls den Zustand des *v* nach *Expr* nicht definitiv zugewiesen.

Im Beispiel
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
die Variable `i` gilt in einem von der eingebetteten Anweisungen des definitiv zugewiesenen ein `if` Anweisung aber nicht in die andere. In der `if` -Anweisung in der Methode `G`, die Variable `i` wird in der zweiten eingebettete Anweisung definitiv zugewiesen, da Ausführung des Ausdrucks `(i = y)` steht immer vor Ausführung dieser eingebetteten Anweisung. Im Gegensatz dazu sind die Variable `i` nicht zugewiesen, in der ersten eingebettete Anweisung, da `x >= 0` möglicherweise wurden getestet "true", wodurch die Variable `i` aufgehoben.

#### <a name="-logical-negation-expressions"></a>! (logische Negation)-Ausdrücke

Für einen Ausdruck *Expr* des Formulars `! expr_operand`:

*  Die definitive Zuweisung Zustand des *v* vor *Expr_operand* ist identisch mit den definitive Zuweisung Zustand des *v* vor *Expr*.
*  Die definitive Zuweisung Zustand des *v* nach *Expr* richtet sich nach:
    * Wenn der Status der *v* nach * Expr_operand * definitiv zugewiesen wird, klicken Sie dann den Status der *v* nach *Expr* definitiv zugewiesen.
    * Wenn der Status der *v* nach * Expr_operand * nicht definitiv zugewiesen wird, klicken Sie dann den Status der *v* nach *Expr* nicht definitiv zugewiesen.
    * Wenn der Status der *v* nach * Expr_operand * ist "definitiv zugewiesenen nach" false "Ausdruck", und klicken Sie dann den Status der *v* nach *Expr* "definitiv zugewiesen, nach dem" true " Ausdruck".
    * Wenn der Status der *v* nach * Expr_operand * ist "nach der als TRUE ausgewertete Ausdruck definitiv zugewiesenen", und klicken Sie dann den Status der *v* nach *Expr* "definitiv zugewiesen, nach" false " Ausdruck".

#### <a name="-null-coalescing-expressions"></a>?? Ausdrücke (null-Sammeloperator)

Für einen Ausdruck *Expr* des Formulars `expr_first ?? expr_second`:

*  Die definitive Zuweisung Zustand des *v* vor *Expr_first* ist identisch mit den definitive Zuweisung Zustand des *v* vor *Expr*.
*  Die definitive Zuweisung Zustand des *v* vor *Expr_second* ist identisch mit den definitive Zuweisung Zustand des *v* nach *Expr_first*.
*  Die definitive Zuweisung-Anweisung von *v* nach *Expr* richtet sich nach:
    * Wenn *Expr_first* ist ein konstanter Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)) mit dem Wert null ist, die den Status der *v* nach *Expr* entspricht Wenn sich der Zustand *v* nach *Expr_second*.
*  Andernfalls den Zustand des *v* nach *Expr* ist identisch mit den definitive Zuweisung Zustand des *v* nach *Expr_first*.

#### <a name="-conditional-expressions"></a>?: (Bedingungsausdrücken)

Für einen Ausdruck *Expr* des Formulars `expr_cond ? expr_true : expr_false`:

*  Die definitive Zuweisung Zustand des *v* vor *Expr_cond* ist identisch mit den Status der *v* vor *Expr*.
*  Die definitive Zuweisung Zustand des *v* vor *Expr_true* ist definitiv zugewiesen werden, wenn eine der folgenden enthält:
    * *Expr_cond* ist ein konstanter Ausdruck mit dem Wert `false`
    * der Status der *v* nach *Expr_cond* definitiv zugewiesen ist, oder "definitiv zugewiesen nach" true "Ausdruck".
*  Die definitive Zuweisung Zustand des *v* vor *Expr_false* ist definitiv zugewiesen werden, wenn eine der folgenden enthält:
    * *Expr_cond* ist ein konstanter Ausdruck mit dem Wert `true`
*  der Status der *v* nach *Expr_cond* definitiv zugewiesen ist, oder "definitiv zugewiesen nach" false "Ausdruck".
*  Die definitive Zuweisung Zustand des *v* nach *Expr* richtet sich nach:
    * Wenn *Expr_cond* ist ein konstanter Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)) mit dem Wert `true` klicken Sie dann den Status der *v* nach *Expr* ist identisch mit den Status der *v* nach *Expr_true*.
    * Andernfalls gilt: Wenn *Expr_cond* ist ein konstanter Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)) mit dem Wert `false` klicken Sie dann den Status der *v* nach *Expr* ist identisch mit den Status der *v* nach *Expr_false*.
    * Andernfalls gilt: Wenn der Status der *v* nach *Expr_true* definitiv zugewiesen und der Status der *v* nach *Expr_false* ist definitiv zugewiesen, klicken Sie dann den Status der *v* nach *Expr* definitiv zugewiesen.
    * Andernfalls den Zustand des *v* nach *Expr* nicht definitiv zugewiesen.

#### <a name="anonymous-functions"></a>Anonyme Funktionen

Für eine *Lambda_expression* oder *Anonymous_method_expression* *Expr* mit einem Text (entweder *Block* oder *Ausdruck* ) *Text*:

*  Die definitive Zuweisung-Status, der eine äußere Variable *v* vor *Text* ist identisch mit den Status der *v* vor *Expr*. D. h. wird definitive zuweisungszustand von äußeren Variablen aus dem Kontext der anonymen Funktion geerbt.
*  Die definitive Zuweisung-Status, der eine äußere Variable *v* nach *Expr* ist identisch mit den Status der *v* vor *Expr*.

Im Beispiel
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
generiert einen Fehler während der Kompilierung, da `max` wird nicht definitiv zugewiesen, in die anonyme Funktion deklariert wird. Im Beispiel
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
generiert außerdem einen Fehler während der Kompilierung, da die Zuweisung zu `n` in der anonymen Funktion hat keine Auswirkungen auf die definitive Zuweisung Zustand des `n` außerhalb der anonymen Funktion.

## <a name="variable-references"></a>Variablenverweise

Ein *Variable_reference* ist ein *Ausdruck* , als Variable klassifiziert wird. Ein *Variable_reference* gibt einen Speicherort, der den aktuellen Wert abzurufen und speichern einen neuen Wert zugegriffen werden kann.

```antlr
variable_reference
    : expression
    ;
```

In C und C++, *Variable_reference* heißt ein *Lvalue*.

## <a name="atomicity-of-variable-references"></a>Unteilbarkeit variabler Verweise

Lese- und Schreibvorgänge für die folgenden Datentypen sind atomisch: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`, und verweisen auf Typen. Darüber hinaus sind Lese- und Schreibvorgänge für Enum-Typen mit einem zugrunde liegenden Typ in der vorherigen Liste ebenfalls atomar. Lese- und Schreibvorgänge von anderen Typen, einschließlich `long`, `ulong`, `double`, und `decimal`, sowie benutzerdefinierte Typen werden nicht unbedingt atomar sein. Abgesehen von der die Bibliotheksfunktionen für diesen Zweck entwickelt gibt es keine Garantie der atomarer Lese-Änderungs-Schreib, z. B. im Fall von Inkrement oder Dekrement.

