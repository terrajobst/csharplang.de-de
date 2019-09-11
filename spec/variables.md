---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876799"
---
# <a name="variables"></a>Variablen

Variablen stellen Speicherorte dar. Jede Variable verfügt über einen Typ, der bestimmt, welche Werte in der Variablen gespeichert werden können. C#ist eine typsichere Sprache, und der C# Compiler stellt sicher, dass Werte, die in Variablen gespeichert sind, immer den entsprechenden Typ haben. Der Wert einer Variablen kann durch Zuweisung oder durch Verwendung `++` der Operatoren und `--` geändert werden.

Eine Variable muss ***definitiv zugewiesen*** werden ([definitive Zuweisung](variables.md#definite-assignment)), bevor ihr Wert abgerufen werden kann.

Wie in den folgenden Abschnitten beschrieben, werden Variablen entweder ***anfänglich zugewiesen*** oder ***ursprünglich nicht zugewiesen***. Eine anfänglich zugewiesene Variable verfügt über einen klar definierten Anfangswert und wird immer als definitiv zugewiesen betrachtet. Eine anfänglich nicht zugewiesene Variable hat keinen Anfangswert. Damit eine anfänglich nicht zugewiesene Variable an einem bestimmten Speicherort als definitiv zugewiesen wird, muss eine Zuweisung zur Variablen in jedem möglichen Ausführungs Pfad erfolgen, der zu diesem Speicherort führt.

## <a name="variable-categories"></a>Variablen Kategorien

C#definiert sieben Kategorien von Variablen: statische Variablen, Instanzvariablen, Array Elemente, Wert Parameter, Verweis Parameter, Ausgabeparameter und lokale Variablen. In den folgenden Abschnitten wird jede dieser Kategorien beschrieben.

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
`x`ist eine statische Variable, `y` ist eine Instanzvariable `v[0]` , ist ein Array Element `a` , ist ein Wert Parameter `b` , ist ein Verweis Parameter `c` , ist ein Output-Parameter `i` und ist eine lokale Variable. .

### <a name="static-variables"></a>Statische Variablen

Ein Feld, das mit `static` dem-Modifizierer deklariert wird, wird als ***statische Variable***bezeichnet. Eine statische Variable kommt vor der Ausführung des statischen Konstruktors ([statischer Konstruktoren](classes.md#static-constructors)) für den enthaltenden Typ vor und ist nicht mehr vorhanden, wenn die zugehörige Anwendungsdomäne nicht mehr vorhanden ist.

Der Anfangswert einer statischen Variablen ist der Standardwert ([Standardwerte](variables.md#default-values)) des Variablen Typs.

Zum Zweck der eindeutigen Zuweisungs Überprüfung wird eine statische Variable als anfänglich zugewiesen betrachtet.

### <a name="instance-variables"></a>Instanzvariablen

Ein Feld, das ohne `static` den Modifizierer deklariert wird, wird als ***Instanzvariable***bezeichnet.

#### <a name="instance-variables-in-classes"></a>Instanzvariablen in Klassen

Eine Instanzvariable einer Klasse tritt auf, wenn eine neue Instanz dieser Klasse erstellt wird, und ist nicht mehr vorhanden, wenn keine Verweise auf diese Instanz vorhanden sind und der Dekonstruktor der Instanz (sofern vorhanden) ausgeführt wurde.

Der Anfangswert einer Instanzvariablen einer Klasse ist der Standardwert ([Standardwerte](variables.md#default-values)) des Variablen Typs.

Zum Zweck der eindeutigen Zuweisungs Überprüfung wird eine Instanzvariable einer Klasse als anfänglich zugewiesen betrachtet.

#### <a name="instance-variables-in-structs"></a>Instanzvariablen in Strukturen

Eine Instanzvariable einer Struktur hat genau die gleiche Lebensdauer wie die Struktur Variable, zu der Sie gehört. Anders ausgedrückt: Wenn eine Variable eines Struktur Typs vorhanden ist oder nicht mehr vorhanden ist, müssen Sie auch die Instanzvariablen der Struktur verwenden.

Der anfängliche Zuweisungs Status einer Instanzvariablen einer Struktur ist mit der der enthaltenden Struktur Variablen identisch. Anders ausgedrückt: Wenn eine Struktur Variable als anfänglich zugewiesen betrachtet wird, sind auch die Instanzvariablen, und wenn eine Struktur Variable anfänglich als nicht zugewiesen betrachtet wird, sind deren Instanzvariablen ebenfalls nicht zugewiesen.

### <a name="array-elements"></a>Array Elemente

Die Elemente eines Arrays entstehen, wenn eine Array Instanz erstellt wird, und sind nicht mehr vorhanden, wenn keine Verweise auf diese Array Instanz vorhanden sind.

Der Anfangswert jedes Elements eines Arrays ist der Standardwert ([Standardwerte](variables.md#default-values)) vom Typ der Array Elemente.

Zum Zweck der eindeutigen Zuweisungs Überprüfung wird ein Array Element als anfänglich zugewiesen betrachtet.

### <a name="value-parameters"></a>Wert Parameter

Ein Parameter, der ohne `ref` - `out` oder-Modifizierer deklariert wird, ist ein ***Wert Parameter***.

Ein value-Parameter ist bei dem Aufruf des Funktionsmembers (Methode, Instanzkonstruktor, Accessor oder Operator) oder der anonymen Funktion, zu der der Parameter gehört, vorhanden und wird mit dem Wert des im Aufruf angegebenen Arguments initialisiert. Ein value-Parameter ist bei der Rückgabe des Funktionsmembers oder der anonymen Funktion normalerweise nicht mehr vorhanden. Wenn der value-Parameter jedoch von einer anonymen Funktion ([Anonyme Funktions Ausdrücke](expressions.md#anonymous-function-expressions)) aufgezeichnet wird, erweitert seine Lebensdauer mindestens so lange, bis der Delegat oder die Ausdrucks Struktur, die von dieser anonymen Funktion erstellt wurde, für Garbage Collection qualifiziert ist.

Zum Zweck der eindeutigen Zuweisungs Überprüfung wird ein value-Parameter als anfänglich zugewiesen betrachtet.

### <a name="reference-parameters"></a>Verweisparameter

Ein Parameter, der mit `ref` einem-Modifizierer deklariert wird, ist ein ***Verweis Parameter***.

Ein Verweis Parameter erstellt keinen neuen Speicherort. Stattdessen stellt ein Verweis Parameter denselben Speicherort wie die Variable dar, die als Argument im Funktionsmember oder anonymen Funktionsaufruf angegeben wird. Folglich ist der Wert eines Verweis Parameters immer mit der zugrunde liegenden Variablen identisch.

Die folgenden konkreten Zuweisungs Regeln gelten für Verweis Parameter. Beachten Sie die verschiedenen Regeln für Ausgabeparameter, die in [Ausgabeparametern](variables.md#output-parameters)beschrieben werden.

*  Eine Variable muss definitiv zugewiesen werden ([definitive Zuweisung](variables.md#definite-assignment)), bevor Sie als Verweis Parameter in einem Funktionsmember oder Delegataufruf übergeben werden kann.
*  Innerhalb eines Funktionsmembers oder einer anonymen Funktion wird ein Verweis Parameter als anfänglich zugewiesen betrachtet.

In einer Instanzmethode oder einem Instanzaccessor eines Strukturtyps `this` verhält sich das Schlüsselwort genau als Verweis Parameter des Struktur Typs ([dieser Zugriff](expressions.md#this-access)).

### <a name="output-parameters"></a>Ausgabeparameter

Ein mit einem `out` -Modifizierer deklarierter Parameter ist ein ***Output-Parameter***.

Mit einem Output-Parameter wird kein neuer Speicherort erstellt. Stattdessen stellt ein Ausgabeparameter denselben Speicherort wie die Variable dar, die im Funktionsmember oder Delegataufruf als Argument angegeben wurde. Folglich ist der Wert eines Output-Parameters immer mit der zugrunde liegenden Variablen identisch.

Die folgenden konkreten Zuweisungs Regeln gelten für Ausgabeparameter. Beachten Sie die verschiedenen Regeln für Verweis Parameter, die in [Verweis Parametern](variables.md#reference-parameters)beschrieben werden.

*  Eine Variable muss nicht definitiv zugewiesen werden, bevor Sie als Ausgabeparameter in einem Funktionsmember oder Delegataufruf übergeben werden kann.
*  Nach dem normalen Abschluss eines Funktionsmembers oder delegataufruens wird jede Variable, die als Output-Parameter übergeben wurde, in diesem Ausführungs Pfad als zugewiesen betrachtet.
*  In einem Funktionsmember oder einer anonymen Funktion wird ein Ausgabeparameter als anfänglich nicht zugewiesen betrachtet.
*  Jeder Ausgabeparameter eines Funktionsmembers oder einer anonymen Funktion muss definitiv zugewiesen werden ([definitive Zuweisung](variables.md#definite-assignment)), bevor der Funktionsmember oder die anonyme Funktion normal zurückgegeben wird.

Innerhalb eines Instanzkonstruktors eines Struktur Typs verhält sich `this` das Schlüsselwort genau wie ein Ausgabeparameter des Struktur Typs ([dieser Zugriff](expressions.md#this-access)).

### <a name="local-variables"></a>Lokale Variablen

Eine ***lokale Variable*** wird von einem *local_variable_declaration*deklariert, der in einem- *Block*, einem *for_Statement*-, *switch_statement* -oder *using_statement*-Vorgang vorkommen kann. oder durch eine *foreach_statement* oder eine *specific_catch_clause* für eine *try_statement*.

Die Lebensdauer einer lokalen Variablen ist der Teil der Programmausführung, in dem sichergestellt wird, dass Speicher für Sie reserviert ist. Diese Lebensdauer erweitert mindestens den Eintrag in *Block*, *for_Statement*, *switch_statement*, *using_statement*, *foreach_statement*oder *specific_catch_clause* , mit dem Sie verknüpft ist, bis die Ausführung dieses *Blocks*, *for_Statement*, *switch_statement*, *using_statement*, *foreach_statement*oder *specific_catch_clause* endet in beliebiger Weise. (Wenn ein eingeschlossener *Block* eingegeben oder eine Methode aufgerufen wird, wird die Ausführung des aktuellen *Blocks*, *for_Statement*, *switch_statement*, *using_statement*, *foreach_statement*oder specific_ angehalten, jedoch nicht beendet.  *catch_clause*.) Wenn die lokale Variable von einer anonymen Funktion ([erfasste äußere Variablen](expressions.md#captured-outer-variables)) aufgezeichnet wird, erweitert ihre Lebensdauer mindestens so lange, bis die Delegat-oder Ausdrucks Baumstruktur, die aus der anonymen Funktion erstellt wurde, zusammen mit allen anderen Objekten, die auf das erfasste Variable, sind für Garbage Collection berechtigt.

Wenn der übergeordnete *Block*, *for_Statement*, *switch_statement*, *using_statement*, *foreach_statement*oder *specific_catch_clause* rekursiv eingegeben wird, wird jeweils eine neue Instanz der lokalen Variablen erstellt. die Zeit und Ihre *local_variable_initializer*werden ggf. jedes Mal ausgewertet.

Eine von einem *local_variable_declaration* eingeführte lokale Variable wird nicht automatisch initialisiert und hat daher keinen Standardwert. Zum Zweck der eindeutigen Zuweisungs Überprüfung wird eine lokale Variable, die von einem *local_variable_declaration* eingeführt wurde, als anfänglich nicht zugewiesen betrachtet. Ein *local_variable_declaration* kann ein *local_variable_initializer*enthalten. in diesem Fall wird die Variable nach dem Initialisierungs Ausdruck ([Deklarations Anweisungen](variables.md#declaration-statements)) definitiv als definitiv zugewiesen betrachtet.

Innerhalb des Gültigkeits Bereichs einer lokalen Variablen, die von einem *local_variable_declaration*eingeführt wurde, handelt es sich um einen Kompilierzeitfehler, der in einer Textposition, die dem *local_variable_declarator*vorangestellt ist, auf diese lokale Variable verweist. Wenn die Deklaration der lokalen Variablen implizit ist ([Deklarationen von lokalen Variablen](statements.md#local-variable-declarations)), ist es auch ein Fehler, auf die Variable innerhalb Ihrer *local_variable_declarator*zu verweisen.

Eine lokale Variable, die von einem *foreach_statement* oder einem *specific_catch_clause* eingeführt wurde, wird als definitiv in Ihrem gesamten Bereich zugewiesen.

Die tatsächliche Lebensdauer einer lokalen Variablen ist implementierungsabhängig. Beispielsweise kann ein Compiler statisch ermitteln, dass eine lokale Variable in einem-Block nur für einen kleinen Teil dieses Blocks verwendet wird. Mithilfe dieser Analyse könnte der Compiler Code generieren, der dazu führt, dass der Speicher der Variablen eine kürzere Lebensdauer aufweist als der enthaltende Block.

Der Speicher, auf den von einer lokalen Verweis Variablen verwiesen wird, wird unabhängig von der Lebensdauer dieser lokalen Verweis Variablen ([Automatische Speicherverwaltung](basic-concepts.md#automatic-memory-management)) freigegeben.

## <a name="default-values"></a>Standardwerte

Die folgenden Kategorien von Variablen werden automatisch mit ihren Standardwerten initialisiert:

*  Statische Variablen.
*  Instanzvariablen von Klassen Instanzen.
*  Array Elemente.

Der Standardwert einer Variablen hängt vom Typ der Variablen ab und wird wie folgt bestimmt:

*  Bei einer Variablen eines *value_type*ist der Standardwert identisch mit dem Wert, der vom Standardkonstruktor des *value_type*([Standardkonstruktoren](types.md#default-constructors)) berechnet wird.
*  Der Standardwert für eine Variable eines *reference_type*ist `null`.

Die Initialisierung zu Standardwerten erfolgt in der Regel, indem der Speicher-Manager oder Garbage Collector den Arbeitsspeicher für alle Bits-NULL initialisieren, bevor er zur Verwendung zugeordnet wird. Aus diesem Grund ist es praktisch, all-Bits-Zero zum Darstellen des NULL-Verweises zu verwenden.

## <a name="definite-assignment"></a>Definitive Zuweisung

An einer bestimmten Stelle im ausführbaren Code eines Funktionsmembers wird eine Variable als ***definitiv zugewiesen*** , wenn der Compiler von einer bestimmten statischen Fluss Analyse ([genaue Regeln zum Bestimmen der eindeutigen Zuweisung](variables.md#precise-rules-for-determining-definite-assignment)) nachweisen kann, dass die Variable wurde automatisch initialisiert oder war das Ziel von mindestens einer Zuweisung. Die Regeln der eindeutigen Zuweisung sind:

*  Eine anfänglich zugewiesene Variable ([anfänglich zugewiesene Variablen](variables.md#initially-assigned-variables)) wird immer als definitiv zugewiesen betrachtet.
*  Eine anfänglich nicht zugewiesene Variable ([anfänglich nicht zugewiesene Variablen](variables.md#initially-unassigned-variables)) wird als definitiv an einem bestimmten Speicherort zugewiesen betrachtet, wenn alle möglichen Ausführungs Pfade, die zu diesem Speicherort führen, mindestens eine der folgenden Zeichen enthalten:
    * Eine einfache Zuweisung ([einfache Zuweisung](expressions.md#simple-assignment)), bei der die Variable der linke Operand ist.
    * Ein Aufruf Ausdruck ([Aufruf Ausdrücke](expressions.md#invocation-expressions)) oder ein Objekt Erstellungs Ausdruck ([Objekt Erstellungs Ausdrücke](expressions.md#object-creation-expressions)), der die Variable als Ausgabeparameter übergibt.
    * Für eine lokale Variable eine lokale Variablen Deklaration ([Deklarationen von lokalen Variablen](statements.md#local-variable-declarations)), die einen Variableninitialisierer einschließt.

Die formale Spezifikation, die den oben genannten informellen Regeln zugrunde liegt, wird in [anfänglich zugewiesenen Variablen](variables.md#initially-assigned-variables), [anfänglich nicht zugewiesenen Variablen](variables.md#initially-unassigned-variables)und [präzisen Regeln zum Bestimmen der eindeutigen Zuweisung](variables.md#precise-rules-for-determining-definite-assignment)beschrieben.

Die eindeutigen Zuweisungs Zustände von Instanzvariablen einer *struct_type* -Variablen werden einzeln und Kollektiv nachverfolgt. Zusätzlich zu den oben aufgeführten Regeln gelten für *struct_type* -Variablen und deren Instanzvariablen die folgenden Regeln:

*  Eine Instanzvariable wird als definitiv zugewiesen betrachtet, wenn die enthaltende *struct_type* -Variable als definitiv zugewiesen wird.
*  Eine *struct_type* -Variable wird als definitiv zugewiesen betrachtet, wenn die einzelnen Instanzvariablen als definitiv zugewiesen betrachtet werden.

Eine definitive Zuweisung ist eine Anforderung in den folgenden Kontexten:

*  Eine Variable muss an jedem Speicherort, an dem der Wert abgerufen wird, definitiv zugewiesen werden. Dadurch wird sichergestellt, dass nie definierte Werte auftreten. Das Vorkommen einer Variablen in einem Ausdruck wird in Erwägung gezogen, den Wert der Variablen zu erhalten, außer wenn
    * die Variable ist der linke Operand einer einfachen Zuweisung.
    * die Variable wird als Output-Parameter übergeben.
    * die Variable ist eine *struct_type* -Variable und tritt als Linker Operand eines Element Zugriffs auf.
*  Eine Variable muss an jedem Speicherort, an dem Sie als Verweis Parameter übergeben wird, definitiv zugewiesen werden. Dadurch wird sichergestellt, dass das aufgerufene Funktionsmember den Verweis Parameter, der anfänglich zugewiesen ist, als
*  Alle Ausgabeparameter eines Funktionsmembers müssen definitiv an jedem Speicherort zugewiesen werden, an dem der Funktionsmember `return` zurückgibt (über eine-Anweisung oder durch die Ausführung, die das Ende des Funktionselement Texts erreicht). Dadurch wird sichergestellt, dass Funktionsmember nicht definierte Werte in Ausgabeparametern zurückgeben. Dadurch kann der Compiler einen Funktionselement Aufruf in Erwägung ziehen, der eine Variable als Output-Parameter annimmt, der einer Zuweisung zur Variablen entspricht.
*  Die `this` Variable eines *struct_type* -Instanzkonstruktors muss an jedem Speicherort, an dem der Instanzkonstruktor zurückgegeben wird, definitiv zugewiesen werden.

### <a name="initially-assigned-variables"></a>Anfänglich zugewiesene Variablen

Die folgenden Kategorien von Variablen werden als anfänglich zugewiesen klassifiziert:

*  Statische Variablen.
*  Instanzvariablen von Klassen Instanzen.
*  Instanzvariablen der anfänglich zugewiesenen Struktur Variablen.
*  Array Elemente.
*  Wert Parameter.
*  Verweis Parameter.
*  In einer `catch` -Klausel oder einer `foreach` -Anweisung deklarierte Variablen.

### <a name="initially-unassigned-variables"></a>Anfänglich nicht zugewiesene Variablen

Die folgenden Kategorien von Variablen werden als anfänglich nicht zugewiesen klassifiziert:

*  Instanzvariablen ursprünglich nicht zugewiesener Struktur Variablen.
*  Ausgabeparameter, einschließlich der `this` Variablen von Strukturinstanzkonstruktoren.
*  Lokale Variablen, mit Ausnahme derjenigen, die `catch` in einer- `foreach` Klausel oder einer-Anweisung deklariert werden.

### <a name="precise-rules-for-determining-definite-assignment"></a>Genaue Regeln zum Bestimmen der eindeutigen Zuweisung

Um zu ermitteln, ob jede verwendete Variable definitiv zugewiesen ist, muss der Compiler einen Prozess verwenden, der dem in diesem Abschnitt beschriebenen Prozess entspricht.

Der Compiler verarbeitet den Text der einzelnen Funktionsmember, der über eine oder mehrere anfänglich nicht zugewiesene Variablen verfügt. Für jede anfänglich nicht zugewiesene Variable *v*bestimmt der Compiler einen ***eindeutigen Zuweisungs Zustand*** für *v* an jedem der folgenden Punkte im Funktionsmember:

*  Am Anfang jeder Anweisung
*  Am Endpunkt ([Endpunkte und Erreichbarkeit](statements.md#end-points-and-reachability)) der einzelnen Anweisungen
*  Auf jedem Bogen, der die Steuerung an eine andere Anweisung oder an den Endpunkt einer Anweisung überträgt
*  Am Anfang jedes Ausdrucks
*  Am Ende jedes Ausdrucks

Der eindeutige Zuweisungs Status von *v* kann wie folgt lauten:

*  Definitiv zugewiesen. Dies deutet darauf hin, dass *v* bei allen möglichen Ablauf Steuerungen ein Wert zugewiesen wurde.
*  Nicht definitiv zugewiesen. Für den Zustand einer Variablen am Ende eines Ausdrucks vom Typ `bool`wird der Status einer Variablen, die nicht definitiv zugewiesen ist, möglicherweise in einen der folgenden Unterzustände versetzt:
    * Definitiv nach dem true-Ausdruck zugewiesen. Dieser Status gibt an, dass *v* definitiv zugewiesen wird, wenn der boolesche Ausdruck als true ausgewertet wird, aber nicht notwendigerweise zugewiesen wird, wenn der boolesche Ausdruck als false ausgewertet wird.
    * Definitiv zugewiesen nach false-Ausdruck. Dieser Status gibt an, dass *v* definitiv zugewiesen wird, wenn der boolesche Ausdruck als false ausgewertet wird, aber nicht notwendigerweise zugewiesen wird, wenn der boolesche Ausdruck als true ausgewertet wird.

Die folgenden Regeln bestimmen, wie der Zustand einer Variablen *v* an jedem Speicherort bestimmt wird.

#### <a name="general-rules-for-statements"></a>Allgemeine Regeln für Anweisungen

*  *v* wird nicht definitiv am Anfang eines Funktionsmember-Texts zugewiesen.
*  *v* wird definitiv am Anfang einer beliebigen nicht erreichbaren Anweisung zugewiesen.
*  Der definitive Zuweisungs Zustand von *v* zu Beginn einer beliebigen anderen Anweisung wird durch Überprüfen des eindeutigen Zuweisungs Status von *v* für alle Ablauf Steuerungs Übertragungen bestimmt, die auf den Anfang dieser Anweisung abzielen. Wenn (und nur wenn) *v* definitiv allen Ablauf Steuerungs Übertragungen zugewiesen ist, wird *v* definitiv am Anfang der Anweisung zugewiesen. Der Satz möglicher Ablauf Steuerungs Übertragungen wird auf die gleiche Weise wie für das Überprüfen der Anweisungs Erreichbarkeit ([Endpunkte und Erreichbarkeit](statements.md#end-points-and-reachability)) bestimmt.
*  Der definitive Zuweisungs Zustand von *v* am Endpunkt eines Blocks, `checked` `unchecked`,, `foreach` `if` `while`,, `do`, `for`,, `lock`, `using`oder `switch`die-Anweisung wird ermittelt, indem der definitive Zuweisungs Status von *v* für alle Ablauf Steuerungs Übertragungen überprüft wird, die auf den Endpunkt dieser Anweisung abzielen. Wenn *v* definitiv allen Ablauf Steuerungs Übertragungen zugewiesen ist, wird *v* definitiv am Endpunkt der Anweisung zugewiesen. Sonst *v* ist nicht definitiv am Endpunkt der Anweisung zugewiesen. Der Satz möglicher Ablauf Steuerungs Übertragungen wird auf die gleiche Weise wie für das Überprüfen der Anweisungs Erreichbarkeit ([Endpunkte und Erreichbarkeit](statements.md#end-points-and-reachability)) bestimmt.

#### <a name="block-statements-checked-and-unchecked-statements"></a>Block Anweisungen, aktivierte und überprüfte Anweisungen

Der definitive Zuweisungs Zustand *v* auf dem Steuerelement wird an die erste Anweisung der Anweisungs Liste im-Block (oder bis zum Endpunkt des-Blocks, wenn die Anweisungs Liste leer ist) mit der eindeutigen Zuweisungsanweisung *v* vor dem-Block übereinstimmen. , `checked` oder`unchecked` -Anweisung.

#### <a name="expression-statements"></a>Ausdrucksanweisungen

Für eine Expression-Anweisung *stmt* , die aus dem Expression- *expr*besteht:

*  *v* hat am Anfang von *expr* denselben eindeutigen Zuweisungs Zustand wie am Anfang von " *stmt*".
*  Wenn *v* , wenn es definitiv am Ende von *expr*zugewiesen wurde, definitiv am Endpunkt von *stmt*zugewiesen wird. sonst Es ist nicht definitiv am Endpunkt von *stmt*zugewiesen.

#### <a name="declaration-statements"></a>Deklarationsanweisungen

*  Wenn *stmt* eine Deklarations Anweisung ohne Initialisierer ist, hat *v* denselben eindeutigen Zuweisungs Zustand am Endpunkt von *stmt* wie am Anfang von *stmt*.
*  Wenn *stmt* eine Deklarations Anweisung mit Initialisierern ist, wird der definitive Zuweisungs Zustand für *v* so bestimmt, als ob *stmt* eine Anweisungs Liste wäre, mit einer Zuweisungsanweisung für jede Deklaration mit einem Initialisierer (in der Reihenfolge von Deklaration).

#### <a name="if-statements"></a>If-Anweisungen

Für eine `if` -Anweisung *stmt* in der Form:
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v* hat am Anfang von *expr* denselben eindeutigen Zuweisungs Zustand wie am Anfang von " *stmt*".
*  Wenn *v* am Ende von *expr*definitiv zugewiesen ist, wird es definitiv auf der Ablauf Steuerungs Übertragung an *then_stmt* und entweder an *else_stmt* oder an den Endpunkt von *stmt* zugewiesen, wenn keine ELSE-Klausel vorhanden ist.
*  Wenn *v* den Zustand "definitiv nach dem wahren Ausdruck zugewiesen" am Ende von *expr*hat, wird es definitiv auf der Ablauf Steuerungs Übertragung an *then_stmt*zugewiesen und nicht definitiv auf der Ablauf Steuerungs Übertragung an *else_ stmt* oder zum Endpunkt von *stmt* , wenn keine ELSE-Klausel vorhanden ist.
*  Weist *v* den Status "definitiv zugewiesen nach dem falschen Ausdruck" am Ende von *expr*auf, wird es definitiv auf der Ablauf Steuerungs Übertragung an *else_stmt*zugewiesen und nicht definitiv auf der Ablauf Steuerungs Übertragung an then_stmt zugewiesen.. Sie ist definitiv nur dann am Endpunkt von *stmt* zugewiesen, wenn Sie definitiv am Endpunkt *then_stmt*zugewiesen ist.
*  Andernfalls wird *v* als nicht definitiv auf der Ablauf Steuerungs Übertragung an *then_stmt* oder *else_stmt*oder an den Endpunkt von *stmt* zugewiesen, wenn keine ELSE-Klausel vorhanden ist.

#### <a name="switch-statements"></a>Switch-Anweisungen

In einer `switch` Anweisung *stmt* mit einem steuernden Ausdrucks- *expr*:

*  Der definitive Zuweisungs Status von *v* am Anfang von *expr* ist mit dem Zustand *v* am Anfang von " *stmt*" identisch.
*  Der definitive Zuweisungs Status von *v* auf der Ablauf Steuerungs Liste der Ablauf Steuerung ist mit dem eindeutigen Zuweisungs Zustand *v* am Ende von *expr*identisch.

#### <a name="while-statements"></a>While-Anweisungen

Für eine `while` Anweisung *stmt* in der Form:
```csharp
while ( expr ) while_body
```

*  *v* hat am Anfang von *expr* denselben eindeutigen Zuweisungs Zustand wie am Anfang von " *stmt*".
*  Wenn *v* am Ende von *expr*definitiv zugewiesen ist, wird es definitiv auf der Ablauf Steuerungs Übertragung an *while_body* und an den Endpunkt von *stmt*zugewiesen.
*  Wenn *v* den Zustand "definitiv nach dem wahren Ausdruck zugewiesen" am Ende von *expr*hat, wird es definitiv auf der Ablauf Steuerungs Übertragung an *while_body*zugewiesen, jedoch nicht definitiv am Endpunkt von *stmt*zugewiesen.
*  Wenn *v* den Zustand "definitiv zugewiesen nach dem falschen Ausdruck" am Ende von *expr*hat, wird es definitiv auf der Ablauf Steuerungs Übertragung an den Endpunkt von *stmt*zugewiesen, aber nicht definitiv auf der Ablauf Steuerungs Übertragung an, *während _body*.

#### <a name="do-statements"></a>Do-Anweisungen

Für eine `do` Anweisung *stmt* in der Form:
```csharp
do do_body while ( expr ) ;
```

*  *v* hat denselben eindeutigen Zuweisungs Status für die Ablauf Steuerungs Übertragung von Anfang an *do_body* wie am *Anfang von* *stmt*.
*  *v* hat am Anfang von *expr* denselben eindeutigen Zuweisungs Zustand wie am Endpunkt *do_body*.
*  Wenn *v* am Ende von *expr*definitiv zugewiesen ist, wird es definitiv der Ablauf Steuerungs Übertragung an den Endpunkt von *stmt*zugewiesen.
*  Wenn *v* den Zustand "definitiv zugewiesen nach dem falschen Ausdruck" am Ende von *expr*hat, wird es definitiv der Ablauf Steuerungs Übertragung an den Endpunkt von *stmt*zugewiesen.

#### <a name="for-statements"></a>For-Anweisungen

Eindeutige Zuweisungs Überprüfung für `for` eine-Anweisung in der Form:
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
wird ausgeführt, als ob die-Anweisung geschrieben wurde:
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

Wenn das *for_condition* -Argument `true` in der `for` Anweisung weggelassen wird, wird die Auswertung der eindeutigen Zuweisung so fortgesetzt, als wäre *for_condition* in der obigen Erweiterung durch ersetzt worden.

#### <a name="break-continue-and-goto-statements"></a>Break-, Continue-und GOTO-Anweisungen

Der definitive Zuweisungs Status von *v* auf der Ablauf Steuerungs Übertragung, die `break`durch eine- `goto` ,-oder-Anweisung verursacht wurde, `continue`entspricht dem eindeutigen Zuweisungs Zustand von *v* am Anfang der Anweisung.

#### <a name="throw-statements"></a>Throw-Anweisungen

Für eine Anweisung *stmt* des Formulars
```csharp
throw expr ;
```

Der definitive Zuweisungs Status von *v* am Anfang von *expr* entspricht dem eindeutigen Zuweisungs Zustand *v* am Anfang von *stmt*.

#### <a name="return-statements"></a>Return-Anweisungen

Für eine Anweisung *stmt* des Formulars
```csharp
return expr ;
```

*  Der definitive Zuweisungs Status von *v* am Anfang von *expr* entspricht dem eindeutigen Zuweisungs Zustand *v* am Anfang von *stmt*.
*  Wenn *v* ein Ausgabeparameter ist, muss ihm definitiv entweder Folgendes zugewiesen werden:
    * nach *expr*
    * `finally` oder am Ende des-Blocks `try` von`return` oder - `try` `finally` ,der`finally` die-Anweisung einschließt. - `catch` -

Für eine Anweisung stmt in der Form:
```csharp
return ;
```

*  Wenn *v* ein Ausgabeparameter ist, muss ihm definitiv entweder Folgendes zugewiesen werden:
    * vor *stmt*
    * `finally` oder am Ende des-Blocks `try` von`return` oder - `try` `finally` ,der`finally` die-Anweisung einschließt. - `catch` -

#### <a name="try-catch-statements"></a>Try-catch-Anweisungen

Für eine Anweisung *stmt* in der Form:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  Der definitive Zuweisungs Zustand von *v* am Anfang von *try_block* entspricht dem eindeutigen Zuweisungs Zustand *v* am Anfang von *stmt*.
*  Der definitive Zuweisungs Zustand von *v* am Anfang von *catch_block_i* (für beliebige *i*) entspricht dem eindeutigen Zuweisungs Zustand *v* am Anfang von *stmt*.
*  Der definitive Zuweisungs Zustand *v* am Endpunkt von *stmt* ist definitiv zugewiesen, wenn (und nur wenn) *v* definitiv am Endpunkt von *try_block* und jeder *catch_block_i* zugewiesen ist (für alle *i* von 1 bis n).).

#### <a name="try-finally-statements"></a>Try-endlich-Anweisungen

Für eine `try` Anweisung *stmt* in der Form:
```csharp
try try_block finally finally_block
```

*  Der definitive Zuweisungs Zustand von *v* am Anfang von *try_block* entspricht dem eindeutigen Zuweisungs Zustand *v* am Anfang von *stmt*.
*  Der definitive Zuweisungs Zustand von *v* am Anfang von *finally_block* entspricht dem eindeutigen Zuweisungs Zustand *v* am Anfang von *stmt*.
*  Der definitive Zuweisungs Zustand *v* am Endpunkt von *stmt* ist definitiv zugewiesen, wenn mindestens einer der folgenden Punkte zutrifft:
    * *v* ist definitiv am Endpunkt *try_block* zugewiesen
    * *v* ist definitiv am Endpunkt *finally_block* zugewiesen

Wenn eine Ablauf Steuerungs Übertragung (z. b. `goto` eine-Anweisung) erfolgt, die innerhalb von *try_block*beginnt und außerhalb von *try_block*endet, wird *v* auch als definitiv auf der Ablauf Steuerungs Übertragung zugewiesen, wenn *v* definitiv zugewiesen am Endpunkt *finally_block*. (Dies ist nicht nur der Fall, wenn –, wenn *v* definitiv aus einem anderen Grund für diese Ablauf Steuerungs Übertragung zugewiesen ist, auch als definitiv zugewiesen angesehen wird.)

#### <a name="try-catch-finally-statements"></a>Try-catch-endlich-Anweisungen

Definitive Zuweisungs Analyse für `try` eine - `catch` - - AnweisunginderForm:`finally`
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
wird ausgeführt, als ob die-Anweisung `try` eine - - `finally` -Anweisung ist `try` , die eine `catch` -Anweisung einschließt:
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

Im folgenden Beispiel wird veranschaulicht, wie sich die verschiedenen `try` Blöcke einer-Anweisung ([try-Anweisung](statements.md#the-try-statement)) auf eine definitive Zuweisung auswirken.
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

Für eine `foreach` Anweisung *stmt* in der Form:
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  Der definitive Zuweisungs Status von *v* am Anfang von *expr* ist mit dem Zustand *v* am Anfang von " *stmt*" identisch.
*  Der definitive Zuweisungs Status *v* auf der Ablauf Steuerungs Übertragung an *embedded_statement* oder an den Endpunkt von *stmt* entspricht dem Zustand *v* am Ende von *expr*.

#### <a name="using-statements"></a>Using-Anweisungen

Für eine `using` Anweisung *stmt* in der Form:
```csharp
using ( resource_acquisition ) embedded_statement
```

*  Der definitive Zuweisungs Status von *v* am Anfang von *resource_acquisition* entspricht dem Zustand *v* am Anfang von *stmt*.
*  Der definitive Zuweisungs Zustand *v* auf der Ablauf Steuerungs Übertragung an *embedded_statement* entspricht dem Zustand *v* am Ende von *resource_acquisition*.

#### <a name="lock-statements"></a>Lock-Anweisungen

Für eine `lock` Anweisung *stmt* in der Form:
```csharp
lock ( expr ) embedded_statement
```

*  Der definitive Zuweisungs Status von *v* am Anfang von *expr* ist mit dem Zustand *v* am Anfang von " *stmt*" identisch.
*  Der definitive Zuweisungs Status von *v* auf der Ablauf Steuerungs Übertragung an *embedded_statement* entspricht dem Zustand *v* am Ende von *expr*.

#### <a name="yield-statements"></a>Yield-Anweisungen

Für eine `yield return` Anweisung *stmt* in der Form:
```csharp
yield return expr ;
```

*  Der definitive Zuweisungs Status von *v* am Anfang von *expr* ist mit dem Zustand *v* am Anfang von " *stmt*" identisch.
*  Der definitive Zuweisungs Status von *v* am Ende von *stmt* ist mit dem Zustand *v* am Ende von *expr*identisch.
*  Eine `yield break` -Anweisung hat keine Auswirkung auf den eindeutigen Zuweisungs Zustand.

#### <a name="general-rules-for-simple-expressions"></a>Allgemeine Regeln für einfache Ausdrücke

Die folgende Regel gilt für diese Arten von Ausdrücken: Literale ([Literale](expressions.md#literals)), einfache Namen ([einfache Namen](expressions.md#simple-names)), Element Zugriffs Ausdrücke ([Member Access](expressions.md#member-access)), nicht indizierte Basis Zugriffs Ausdrücke ([Basis Zugriff](expressions.md#base-access)), `typeof`Ausdrücke ([typeof-Operator](expressions.md#the-typeof-operator)), Standardwert Ausdrücke ([Standardwert Ausdrücke](expressions.md#default-value-expressions)) und `nameof` Ausdrücke ([nameof-Ausdrücke](expressions.md#nameof-expressions)).

*  Der definitive Zuweisungs Zustand von *v* am Ende eines solchen Ausdrucks entspricht dem eindeutigen Zuweisungs Zustand von *v* am Anfang des Ausdrucks.

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>Allgemeine Regeln für Ausdrücke mit eingebetteten Ausdrücken

Die folgenden Regeln gelten für diese Arten von Ausdrücken: Klammern in Klammern ([Ausdrücke in Klammern](expressions.md#parenthesized-expressions)), Element Zugriffs Ausdrücke ([Element Zugriff](expressions.md#element-access)), Basis Zugriffs Ausdrücke mit Indizierung ([Basis Zugriff](expressions.md#base-access)), Inkrement und Dekrementausdrücke ([postfix-Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators), [Präfix Inkrement-und Dekrementoperatoren](expressions.md#prefix-increment-and-decrement-operators)), Umwandlungs `-`Ausdrücke `~`(Umwandlungs[Ausdrücke](expressions.md#cast-expressions)), unärer `+`,,, `*`Ausdrücke, Binär `+`, `-`, `*`, ,`/` ,`<<`, ,`<`,, ,`>`, `%` `>>` `<=` `>=` `==`, ,,`is` ,`^` ,, Ausdrücke ([arithmetische Operatoren](expressions.md#arithmetic-operators), [Shift-Operatoren](expressions.md#shift-operators), relational `|` `&` `as` `!=` [ Operatoren](expressions.md#relational-and-type-testing-operators), [logische Operatoren](expressions.md#logical-operators)), zusammengesetzte Zuweisungs Ausdrücke ( `checked` [Verbund Zuweisung](expressions.md#compound-assignment)) und `unchecked` Ausdrücke (die aktivierten und deaktivierten[Operatoren](expressions.md#the-checked-and-unchecked-operators)), plus Array und Delegat Erstellungs Ausdrücke ([der New-Operator](expressions.md#the-new-operator)).

Jeder dieser Ausdrücke verfügt über einen oder mehrere unter Ausdrücke, die in einer festgelegten Reihenfolge bedingungslos ausgewertet werden. Der binäre `%` Operator wertet z. b. die linke Seite des Operators und die Rechte Seite aus. Ein Index Vorgang wertet den indizierten Ausdruck aus und wertet dann jeden Index Ausdruck in der Reihenfolge von links nach rechts aus. Für einen Expression- *expr*, der unter Ausdrücke *E1, E2,..., en*aufweist, die in dieser Reihenfolge ausgewertet werden:

*  Der definitive Zuweisungs Zustand von *v* am Anfang von *E1* entspricht dem eindeutigen Zuweisungs Zustand am Anfang von *expr*.
*  Der definitive Zuweisungs Zustand von *v* am Anfang von *Ei* (*i* größer als 1) entspricht dem eindeutigen Zuweisungs Zustand am Ende des vorherigen unter Ausdrucks.
*  Der definitive Zuweisungs Status von *v* am Ende von *expr* entspricht dem eindeutigen Zuweisungs Zustand am Ende von *en* .

#### <a name="invocation-expressions-and-object-creation-expressions"></a>Ausdrücke zum Aufrufen und zum Erstellen von Objekten

Für einen Aufruf Ausdruck " *expr* " in der Form:
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
oder ein Objekt Erstellungs Ausdruck in der Form:
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  Bei einem Aufruf Ausdruck ist der definitive Zuweisungs Zustand von *v* vor *primary_expression* mit dem Zustand *v* vor *expr*identisch.
*  Bei einem Aufruf Ausdruck ist der definitive Zuweisungs Zustand von *v* vor *arg1* mit dem Zustand *v* nach *primary_expression*identisch.
*  Bei einem Objekt Erstellungs Ausdruck ist der definitive Zuweisungs Zustand von *v* vor *arg1* mit dem Zustand *v* vor *expr*identisch.
*  Für jedes Argument *Argi*wird der definitive Zuweisungs Zustand *v* nach *Argi* durch die normalen ausdrucksregeln bestimmt, wobei alle `ref` -oder `out` -Modifizierer ignoriert werden.
*  Für jedes Argument *Argi* für jeden *, der größer als* 1 ist, ist der definitive Zuweisungs Zustand *v* vor *Argi* mit dem Zustand von *v* nach dem vorherigen *arg*identisch.
*  Wenn die Variable *v* als `out` Argument (d. h. ein Argument des Formulars `out v`) in einem der Argumente weitergegeben wird, dann wird der Zustand von *v* nach *expr* definitiv zugewiesen. Sonst der Zustand von *v* nach *expr* ist mit dem Zustand *v* nach dem *argN*identisch.
*  Für Arrayinitialisierer ([Array Erstellungs Ausdrücke](expressions.md#array-creation-expressions)), Objektinitialisierer ([Objektinitialisierer](expressions.md#object-initializers)), sammlungsinitialisierer (Auflistungsinitialisierer[) und](expressions.md#collection-initializers)anonyme Objektinitialisierer ([Anonyme Objekt Erstellung) Ausdrücke](expressions.md#anonymous-object-creation-expressions)), wird der definitive Zuweisungs Zustand durch die Erweiterung bestimmt, auf die diese Konstrukte festgelegt sind.

#### <a name="simple-assignment-expressions"></a>Einfache Zuweisungs Ausdrücke

Für einen Ausdrucks- *expr* in der `w = expr_rhs`Form:

*  Der definitive Zuweisungs Zustand von *v* vor *expr_rhs* entspricht dem eindeutigen Zuweisungs Zustand von *v* vor *expr*.
*  Der eindeutige Zuweisungs Zustand von *v* nach *expr* wird durch Folgendes bestimmt:
   * Wenn " *w* " dieselbe Variable wie " *v*" ist, wird der definitive Zuweisungs Zustand " *v* " nach der *expr* definitiv zugewiesen.
   * Andernfalls, wenn die Zuweisung innerhalb des Instanzkonstruktors eines Struktur Typs erfolgt, wenn *w* ein Eigenschaften Zugriff ist, der eine automatisch implementierte Eigenschaft *P* auf der zu erstellenden Instanz festlegt und *v* das verborgene dahinter liegende Feld von *P*, dann ist der definitive Zuweisungs Status von *v* nach *expr* definitiv zugewiesen.
   * Andernfalls entspricht der eindeutige Zuweisungs Zustand von *v* nach *expr* dem eindeutigen Zuweisungs Zustand *v* nach *expr_rhs*.

#### <a name="-conditional-and-expressions"></a>& & (bedingte und) Ausdrücke

Für einen Ausdrucks- *expr* in der `expr_first && expr_second`Form:

*  Der definitive Zuweisungs Zustand von *v* vor *expr_first* entspricht dem eindeutigen Zuweisungs Zustand von *v* vor *expr*.
*  Der definitive Zuweisungs Zustand von *v* vor *expr_second* ist definitiv zugewiesen, wenn der Zustand von *v* nach *expr_first* entweder definitiv zugewiesen oder "nach dem wahren Ausdruck definitiv zugewiesen" ist. Andernfalls ist Sie nicht definitiv zugewiesen.
*  Der eindeutige Zuweisungs Zustand von *v* nach *expr* wird durch Folgendes bestimmt:
    * Wenn *expr_first* ein konstanter `false`Ausdruck mit dem Wert ist, entspricht der eindeutige Zuweisungs Zustand von *v* nach *expr* dem eindeutigen Zuweisungs Zustand *v* nach *expr_first*.
    * Andernfalls, wenn der Zustand von *v* nach *expr_first* definitiv zugewiesen ist, wird der Status von *v* nach dem *exponl* definitiv zugewiesen.
    * Andernfalls, wenn der Zustand von *v* nach *expr_second* definitiv zugewiesen ist und der Zustand von *v* nach *expr_first* "definitiv zugewiesen nach false-Ausdruck" lautet, ist der Status von " *v* " nach *expr* definitiv vorgesehen.
    * Andernfalls, wenn der Zustand von *v* nach *expr_second* definitiv zugewiesen oder "definitiv nach dem wahren Ausdruck zugewiesen" ist, *dann wird der* Zustand von *v* nach dem Ausdruck "true" nach "true" zugewiesen.
    * Andernfalls, wenn der Zustand von *v* nach *expr_first* "definitiv zugewiesen nach false-Ausdruck" und der Zustand von *v* nach *expr_second* "definitiv zugewiesen nach false-Ausdruck", dann der Zustand *v* nach  *expr* ist "definitiv nach false-Ausdruck zugewiesen".
    * Andernfalls ist der Zustand von *v* nach *expr* nicht definitiv zugewiesen.

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
die Variable `i` wird als definitiv in einer der eingebetteten Anweisungen `if` einer-Anweisung zugewiesen, jedoch nicht in der anderen. In der `if` -Anweisung in `F`der-Methode `i` ist die-Variable definitiv in der ersten eingebetteten-Anweisung zugewiesen, `(i = y)` da die Ausführung des Ausdrucks immer der Ausführung dieser eingebetteten Anweisung vorangestellt wird. Im Gegensatz dazu ist die `i` Variable in der zweiten eingebetteten Anweisung nicht definitiv zugewiesen, da `x >= 0` möglicherweise false getestet hat, was dazu führt `i` , dass die Variable nicht zugewiesen wird.

#### <a name="-conditional-or-expressions"></a>|| (bedingte oder)-Ausdrücke

Für einen Ausdrucks- *expr* in der `expr_first || expr_second`Form:

*  Der definitive Zuweisungs Zustand von *v* vor *expr_first* entspricht dem eindeutigen Zuweisungs Zustand von *v* vor *expr*.
*  Der definitive Zuweisungs Zustand *v* vor *expr_second* ist definitiv zugewiesen, wenn der Zustand von *v* nach *expr_first* entweder definitiv zugewiesen oder "definitiv nach false-Ausdruck zugewiesen" ist. Andernfalls ist Sie nicht definitiv zugewiesen.
*  Die definitive Zuweisungsanweisung von *v* nach *expr* wird durch Folgendes bestimmt:
    * Wenn *expr_first* ein konstanter `true`Ausdruck mit dem Wert ist, entspricht der eindeutige Zuweisungs Zustand von *v* nach *expr* dem eindeutigen Zuweisungs Zustand *v* nach *expr_first*.
    * Andernfalls, wenn der Zustand von *v* nach *expr_first* definitiv zugewiesen ist, wird der Status von *v* nach dem *exponl* definitiv zugewiesen.
    * Andernfalls, wenn der Zustand von *v* nach *expr_second* definitiv zugewiesen ist und der Status von *v* after *expr_first* "definitiv nach dem wahren Ausdruck zugewiesen" lautet, ist der Status von *v* nach der *expr* definitiv vorgesehen.
    * Andernfalls, wenn der Zustand von *v* nach *expr_second* definitiv zugewiesen ist oder "definitiv nach false-Ausdruck zugewiesen" ist, wird der Status von *v* nach *expr* "definitiv nach dem falschen Ausdruck zugewiesen" angezeigt.
    * Andernfalls, wenn der Zustand von *v* nach *expr_first* "definitiv nach dem wahren Ausdruck zugewiesen" und der Zustand von *v* nach *expr_second* "definitiv zugewiesen nach dem true-Ausdruck", dann der Status von *v* nach *expr* ist "definitiv nach dem wahren Ausdruck zugewiesen".
    * Andernfalls ist der Zustand von *v* nach *expr* nicht definitiv zugewiesen.

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
die Variable `i` wird als definitiv in einer der eingebetteten Anweisungen `if` einer-Anweisung zugewiesen, jedoch nicht in der anderen. In der `if` -Anweisung in `G`der-Methode `i` wird die-Variable definitiv in der zweiten eingebetteten-Anweisung zugewiesen, `(i = y)` da die Ausführung des Ausdrucks immer der Ausführung dieser eingebetteten Anweisung vorangestellt wird. Im Gegensatz dazu ist die `i` Variable in der ersten eingebetteten Anweisung nicht definitiv zugewiesen, da `x >= 0` möglicherweise true getestet hat, was dazu führt `i` , dass die Variable nicht zugewiesen wird.

#### <a name="-logical-negation-expressions"></a>! (logische Negations Ausdrücke)

Für einen Ausdrucks- *expr* in der `! expr_operand`Form:

*  Der definitive Zuweisungs Zustand von *v* vor *expr_operand* entspricht dem eindeutigen Zuweisungs Zustand von *v* vor *expr*.
*  Der eindeutige Zuweisungs Zustand von *v* nach *expr* wird durch Folgendes bestimmt:
    * Wenn der Zustand *v* nach * expr_operand * definitiv zugewiesen ist, wird der Status von *v* nach dem *exponl* definitiv zugewiesen.
    * Wenn der Zustand *v* nach * expr_operand * nicht definitiv zugewiesen ist, wird der Status von *v* nach *expr* nicht definitiv zugewiesen.
    * Wenn der Zustand *v* after * expr_operand * "definitiv nach false-Ausdruck zugewiesen *" lautet, wird der* Status von *v* nach dem Ausdruck "true" nach dem Ausdruck "true" zugewiesen.
    * Wenn der Zustand *v* after * expr_operand * "definitiv nach dem true-Ausdruck zugewiesen" lautet, wird der Status von *v* nach der *expr* "definitiv nach dem falschen Ausdruck zugewiesen" angezeigt.

#### <a name="-null-coalescing-expressions"></a>?? (null Coalescing) Ausdrücke

Für einen Ausdrucks- *expr* in der `expr_first ?? expr_second`Form:

*  Der definitive Zuweisungs Zustand von *v* vor *expr_first* entspricht dem eindeutigen Zuweisungs Zustand von *v* vor *expr*.
*  Der definitive Zuweisungs Zustand von *v* vor *expr_second* entspricht dem eindeutigen Zuweisungs Zustand von *v* nach *expr_first*.
*  Die definitive Zuweisungsanweisung von *v* nach *expr* wird durch Folgendes bestimmt:
    * Wenn *expr_first* ein konstanter Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)) mit dem Wert NULL ist, ist der Zustand *von v* nach *expr* mit dem Zustand von *v* nach *expr_second*identisch.
*  Andernfalls ist der Zustand von *v* nach *expr* mit dem eindeutigen Zuweisungs Zustand *v* nach *expr_first*identisch.

#### <a name="-conditional-expressions"></a>?: (bedingte) Ausdrücke

Für einen Ausdrucks- *expr* in der `expr_cond ? expr_true : expr_false`Form:

*  Der definitive Zuweisungs Zustand von *v* vor *expr_cond* entspricht dem Zustand von *v* vor *expr*.
*  Der definitive Zuweisungs Zustand *v* vor *expr_true* ist definitiv nur dann zugewiesen, wenn eine der folgenden Punkte Folgendes enthält:
    * *expr_cond* ist ein konstanter Ausdruck mit dem Wert`false`
    * der Status von *v* nach dem *expr_cond* -Wert ist definitiv zugewiesen oder "nach true-Ausdruck definitiv zugewiesen".
*  Der definitive Zuweisungs Zustand *v* vor *expr_false* ist definitiv nur dann zugewiesen, wenn eine der folgenden Punkte Folgendes enthält:
    * *expr_cond* ist ein konstanter Ausdruck mit dem Wert`true`
*  der Status von *v* nach dem *expr_cond* -Wert ist definitiv zugewiesen oder "definitiv zugewiesen nach false-Ausdruck".
*  Der eindeutige Zuweisungs Zustand von *v* nach *expr* wird durch Folgendes bestimmt:
    * Wenn *expr_cond* ein konstanter Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)) mit `true` Wert ist, ist der Zustand von *v* nach *expr* mit dem Zustand von *v* nach *expr_true*identisch.
    * Wenn *expr_cond* ein konstanter Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions) `false` ) mit Wert ist, dann ist der Zustand von *v* nach *expr* mit dem Zustand von *v nach* *expr_false*identisch.
    * Andernfalls, wenn der Zustand von *v* nach *expr_true* definitiv zugewiesen ist und der Status von *v* nach dem *expr_false* definitiv zugewiesen ist, dann wird der Status von *v* nach der *expr* definitiv zugewiesen.
    * Andernfalls ist der Zustand von *v* nach *expr* nicht definitiv zugewiesen.

#### <a name="anonymous-functions"></a>Anonyme Funktionen

Für *lambda_expression* -oder *anonymous_method_expression* - *expr* mit einem Textkörper ( *Block* -oder *Ausdrucks* *Text*):

*  Der definitive Zuweisungs Zustand einer äußeren Variablen *v* vor dem *Text* ist mit dem Zustand *v* vor *expr*identisch. Das heißt, dass der definitive Zuweisungs Status äußerer Variablen vom Kontext der anonymen Funktion geerbt wird.
*  Der eindeutige Zuweisungs Zustand einer äußeren Variable *v* nach *expr* ist mit dem Zustand *v* vor *expr*identisch.

Das Beispiel
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
generiert einen Kompilierzeitfehler, `max` da nicht definitiv zugewiesen ist, wo die anonyme Funktion deklariert wird. Das Beispiel
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
generiert außerdem einen Kompilierzeitfehler, da die Zuweisung `n` zu in der anonymen Funktion keine Auswirkung auf den eindeutigen Zuweisungs Zustand `n` von außerhalb der anonymen Funktion hat.

## <a name="variable-references"></a>Variablen Verweise

Ein *variable_reference* ist ein *Ausdruck* , der als Variable klassifiziert wird. Ein *variable_reference* bezeichnet einen Speicherort, auf den sowohl zum Abrufen des aktuellen Werts als auch zum Speichern eines neuen Werts zugegriffen werden kann.

```antlr
variable_reference
    : expression
    ;
```

In C und C++wird ein *variable_reference* als *Lvalue*bezeichnet.

## <a name="atomicity-of-variable-references"></a>Atomizität von Variablen verweisen

Lese-und Schreibvorgänge der folgenden Datentypen sind atomarisch `char`: `byte` `bool`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`,, und Verweis Typen. Außerdem sind Lese-und Schreibvorgänge von Enumerationstypen mit einem zugrunde liegenden Typ in der vorherigen Liste ebenfalls atomarisch. Lese-und Schreibvorgänge anderer Typen, `long`einschließlich `ulong`, `double`, und `decimal`sowie benutzerdefinierte Typen, sind nicht unbedingt atomarisch. Abgesehen von den Bibliotheksfunktionen, die für diesen Zweck entworfen wurden, gibt es keine Garantie für atomarische Lese-und Schreibvorgänge, z. b. im Fall von Inkrement oder Dekrement.

